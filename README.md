# cicd-handson
こちらは、GitOps と、Github Actions のサンプルを利用した CI/CD Handson の手順を説明したものとなります。
+ 記載の内容については、2022年2月4日時点の環境（ OSS や、その他 Azure 等プラットフォームのバージョン）をベースとしています。
+ **なお、こちらに記載の内容はあくまでサンプルとなりますので、ご注意ください。**

<img width="952" alt="ScreenShot 2022-02-09 9 15 53" src="https://user-images.githubusercontent.com/17949085/153098617-694bd597-408d-4934-bb86-a8ccbc7b8751.png">



## アジェンダ
+ [TASK0: 事前準備](https://github.com/tbuchi888/cicd-handson/blob/main/README.md#task0-%E4%BA%8B%E5%89%8D%E6%BA%96%E5%82%99)
+ [TASK1: Kubernetes および GitOps の初歩を体感](https://github.com/tbuchi888/cicd-handson/blob/main/README.md#task1-kubernetes-%E3%81%8A%E3%82%88%E3%81%B3--gitops-%E3%81%AE%E5%88%9D%E6%AD%A9%E3%82%92%E4%BD%93%E6%84%9F)
+ [TASK2: Git および、手動での GitOps を体感](https://github.com/tbuchi888/cicd-handson/blob/main/task2.md)
+ [TASK3: CI（Github Actions）および、CIと連動した CD（GitOps）を体感](https://github.com/tbuchi888/cicd-handson/blob/main/task3.md)
+ [TASK4: 後片付け、Azureリソースの削除](https://github.com/tbuchi888/cicd-handson/blob/main/task4.md)

## TASK0: 事前準備
本 Handson では、開始前に以下の準備ができていることを前提としています。
参加チーム毎に事前準備をお願いします

+ Hands on 用 Azure Subscription 準備
  + 用途： アプリデプロイ用のAKS(Azure Kubernetes Service)及び、AKS上でGitOpsのOSSツール（ArgoCD）を動作させます
  + 参考： [Azure の無料アカウント](https://azure.microsoft.com/ja-jp/free/)より新規作成
+ Github アカウントの準備
  + アカウントが存在しない場合： [Github Top ページの Sign up for GitHub](https://github.com/) より新規作成
  + 用途：GitOps用のプロジェクト並びに、CI用のプロジェクトを作成します
+ dockerhub Account 準備
  + アカウントが存在しない場合： [Dockerhub Top ページの Get Started Today for Free](https://hub.docker.com/) より新規作成
  + 用途：CIで作成するコンテナイメージ（Publicを想定）を格納します

<img width="952" alt="ScreenShot 2022-02-09 9 16 03" src="https://user-images.githubusercontent.com/17949085/153098658-6412a270-e2b2-4617-bf88-3e8504f50bfe.png">


### 本ハンズオンを実施する上での注意
本ハンズオンでは、同一のサブスクリプションで同時に複数の作業者が作業を実施することになります。
途中、以下のような表記が入力項目の一部に入ることがあり、読み替えていただく必要がある箇所がございますので、ご注意ください。
XXX はご自身のチームで決めた任意のID、その他 [] で囲まれた部分は該当するアカウントやシークレットの値を入力してください。
例)
myResourceGroup**XXX**
**[YOUR-ACCOUNT-NAME]**/handson-gitops

---

## TASK1: Kubernetes および  GitOps の初歩を体感
アプリケーションのデプロイ先となる AKS Kubernetes クラスタの作成 と GitOps ツールの1つ（*1）である ArgoCD を作成した AKS へインストールし、
既存のGithubプロジェクトのKubernetes用マニフェストファイルを利用して、アプリケーションをデプロイします。

*1: GitOps ツールについては、他に [Flux](https://fluxcd.io/) 等もありますが、今回は ArgoCD を利用します。

<img width="952" alt="ScreenShot 2022-02-09 9 16 09" src="https://user-images.githubusercontent.com/17949085/153380083-dab68f31-bfce-49fe-a645-7ac51e497e37.png">


## TASK1.1: AKS(Azure Kubernetes Service)のクラスターを作成
[Azure ポータル](https://ms.portal.azure.com/)へログインし、 **cloud shell** より  az コマンドで Azure 上に、リソースグループと [AKS クラスタを作成](https://docs.microsoft.com/ja-jp/azure/aks/kubernetes-service-principal?tabs=azure-cli)します。

<img width="1115" alt="ScreenShot 2022-02-03 11 55 21" src="https://user-images.githubusercontent.com/17949085/152273919-6f441588-a674-41e7-a5b7-eb9643c0246b.png">

**以下各チーム毎に準備されたものへ置き換えて実行をしてください**
+ myResourceGroupXXX: Subscription 内でユニークなものを設定
+ [my-subscription-name]: 今回作成する Subscription 名

**cloud shellのbashへ** 以下をコピー＆ペーストし、上記について各チーム毎に準備されたものへ置き換えてEnterを押下、AKS のクラスタ作成完了（ JSON 形式の結果が返ってくる）まで数分待ちます
(cloud shell を初めて使う場合には、ストレージの設定が必要です。)

デフォルトのサブスクリプションの確認し、異なる場合は利用するサブスクリプションを設定をします。

```
# デフォルトのサブスクリプションを確認します。
az account list --output table
# デフォルトのサブスクリプションが、作成したいサブスクリプションと異なっている場合
az account set --subscription '[my-subscription-name]'
```

続いて、

```
# 最初にリソースグループを作成します。
az group create --name myResourceGroupXXX --location japaneast

# AKSクラスタを作成し、クラスタオートスケーラを有効にします。
az aks create \
  --resource-group myResourceGroupXXX \
  --name myAKSCluster \
  --node-count 1 \
  --vm-set-type VirtualMachineScaleSets \
  --load-balancer-sku standard \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3 \
  --generate-ssh-keys \
```
<img width="909" alt="ScreenShot 2022-02-03 12 03 42" src="https://user-images.githubusercontent.com/17949085/152274651-f587da0d-6e44-4598-b796-febd306d8595.png">

## TASK1.2: AKSへ接続
さきほど作成した AKS へ argocd をインストールするために、AKSへの接続する準備をします。

Azureポータル ＞ リソースグループ ＞ `myResourceGroupXXX`
リソースグループより、`myAKSCluster`を選択して、AKS のメニューへ行き、画面上部の **接続** をクリックして、AKS への接続情報取得します。

<img width="651" alt="ScreenShot 2022-02-03 12 13 14" src="https://user-images.githubusercontent.com/17949085/152275741-b91fa40a-a49b-4133-b5a0-bdd480c6a4cc.png">
<img width="452<img width="573" alt="ScreenShot 2022-02-03 12 19 19" src="https://user-images.githubusercontent.com/17949085/152275911-720210d9-700a-4785-9c96-fb6e795f37e5.png">

**cloud shellのbashへ** 以下をコピー＆ペーストし、az コマンドにて AKS へ アクセスできるようにします。
```
# AKS接続情報を取得
az aks get-credentials --resource-group myResourceGroupXXX --name myAKSCluster

# kubectl コマンドで AKS クラスタ の worker node を表示
kubectl get node -o wide 
```

<img width="1227" alt="ScreenShot 2022-02-03 12 27 17" src="https://user-images.githubusercontent.com/17949085/152276596-b2c90888-f753-4a85-84a9-82eacc742d64.png">

### TASK1.3: ArgoCD インストール
[ArgoCD をインストールします。](https://argo-cd.readthedocs.io/en/stable/#quick-start)
**cloud shellのbashへ** 以下をコピー＆ペースト

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

ArgoCDのPodの状態が全て`Running`であることを確認します。
```
kubectl -n argocd get po
```

<img width="552" alt="ScreenShot 2022-02-03 13 07 03" src="https://user-images.githubusercontent.com/17949085/152279621-e0f90ff5-4d99-4829-a4d1-aa25630cd781.png">

次に、`admin`のパスワードを確認します。
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

ArgoCD の GUI へアクセスするために、LBから接続できるように変更し、
```
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

以下コマンドを`EXTERNAL-IP`にPIPが割り振られるまで何度入力し、ブラウザより`EXTERNAL-IP`へアクセスします。
```
kubectl -n argocd get svc
``` 

<img width="951" alt="ScreenShot 2022-02-03 13 10 09" src="https://user-images.githubusercontent.com/17949085/152279808-6d87c226-42a9-43ff-a2e8-5392eeb0779e.png">

ユーザー名は`admin`で、さきほど確認したパスワードでログインします。

<img width="980" alt="ScreenShot 2022-02-03 13 13 43" src="https://user-images.githubusercontent.com/17949085/152280256-ef276317-2555-464f-898f-400ad953ef1f.png">

## TASK1.4: ArgoCD applicationを設定
既存の Github プロジェクトの Kubernetes 用マニフェストファイルを利用して、新規にアプリケーションを登録します。

画面左上の`+ NEW APP`をクリックし以下を入力して、右上の`CREATE`をクリックします。
**書いていないものはデフォルト値のまま**

GENERAL
+ Application Name: argotest（任意）
+ Project: default（プルダウンより選択）
+ SYNC POLICY: Automatic（プルダウンより選択）
+ SYNC OPTIONS: AUTO-CREATE NAMESPACE(チェックを入れる)

SOURCE
+ Repository URL: https://github.com/tbuchi888/argocd-sample-repo.git
+ Path: k8s

DESTINATION
+ Cluster URL: https://kubernetes.default.svc（プルダウンより選択）
+ Namespace: argotest(kubectlコマンドで確認するため、こちらを設定するが、基本的には任意)
<img width="570" alt="ScreenShot 2022-02-03 13 43 09" src="https://user-images.githubusercontent.com/17949085/152282462-5fb13d5a-b613-4e6a-94da-d2f0ebdfbf36.png">

作成したアプリケーションをクリックして、AKS上へアプリケーションがデプロイされていることを確認します。
<img width="663" alt="ScreenShot 2022-02-03 13 44 46" src="https://user-images.githubusercontent.com/17949085/152282594-2c159ee9-4848-4b4d-9423-a2023962abb6.png">
<img width="1228" alt="ScreenShot 2022-02-03 13 46 10" src="https://user-images.githubusercontent.com/17949085/152282832-96d9f497-360c-405a-bdeb-d78fa6d3748a.png">

右上の`Network`のアイコンをクリックして表示を変えます
<img width="1215" alt="ScreenShot 2022-02-03 14 05 24" src="https://user-images.githubusercontent.com/17949085/152284486-45e3e318-4be1-4f50-aba9-c2b35bd50ae8.png">

インターネット（雲のアイコン）の右横に表示されているアプリケーションのPIPへブラウザよりアクセスし、デモアプリの画面が表示されることを確認します。

このTASKの最後に、Azure PortalのCloud Shell より  AKS 上にアプリケーションの Kubernetes オブジェクト( Pod(コンテナ), Deployment（コンテナ管理）, Service（L4LB）)がデプロイされていることを確認します。
**cloud shellのbashへ** 以下をコピー＆ペースト

```
kubectl -n argotest get all
```
<img width="639" alt="ScreenShot 2022-02-03 14 01 54" src="https://user-images.githubusercontent.com/17949085/152284177-42dd6671-4375-451c-9918-91a0d842c9b4.png">

以上でこのタスクは終了となります。
[TASK2へ](https://github.com/tbuchi888/cicd-handson/blob/main/task2.md)

---
