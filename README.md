# cicd-handson
![ScreenShot 2022-02-03 12 38 43](https://user-images.githubusercontent.com/17949085/152277472-e69f6882-c0a2-4b7f-9407-563a4e8b7b45.png)

## アジェンダ
+ TASK0: 事前準備
+ TASK1: Kubernetes および　 GitOps の初歩を体感
+ TASK2: Git および、手動での GitOps を体感

## TASK0: 事前準備
本　Handson では、開始前に以下の準備ができていることを前提としています。
参加チーム毎に以下の事前準備をお願いします

+ Hands on 用 Azure Subscription 準備
  + 用途： アプリデプロイ用のAKS(Azure Kubernetes Service)及び、AKS上でGitOpsのOSSツール（ArgoCD）を動作させます
  + 参考： [Azure の無料アカウント](https://azure.microsoft.com/ja-jp/free/)より新規作成
+ Github アカウントの準備
  + アカウントが存在しない場合： [Github Top ページの Sign up for GitHub](https://github.com/) より新規作成
  + 用途：GitOps用のプロジェクト並びに、CI用のプロジェクトを作成します
+ dockerhub Account 準備
  + アカウントが存在しない場合： [Dockerhub Top ページの　Get Started Today for Free](https://hub.docker.com/) より新規作成
  + 用途：CIで作成するコンテナイメージ（Publicを想定）を格納します

---

## TASK1: Kubernetes および　 GitOps の初歩を体感
アプリケーションのデプロイ先となる　AKS Kubernetes クラスタの作成　と　GitOps　ツールである　ArgoCD　を作成したAKSへインストールし、
既存のGithubプロジェクトのKubernetes用マニフェストファイルを利用して、アプリケーションをデプロイします。
<img width="952" alt="ScreenShot 2022-02-03 12 47 50" src="https://user-images.githubusercontent.com/17949085/152278272-379adebe-6cca-4b5e-b356-c8263f3afa88.png">




### TASK1.1:　AKS(Azure Kubernetes Service)のクラスターを作成
[Azure ポータル](https://ms.portal.azure.com/)へログインし、 **Cloudshell**　より　　az コマンドで　Azure　上に、リソースグループと　AKS　クラスタを作成します。
<img width="1115" alt="ScreenShot 2022-02-03 11 55 21" src="https://user-images.githubusercontent.com/17949085/152273919-6f441588-a674-41e7-a5b7-eb9643c0246b.png">

**Cloudshellのbashへ**　以下をコピー＆ペーストしてEnterを押下、AKSのクラスタ作成完了（JSON形式の結果が返ってくる）まで数分待ちます

```
# First create a resource group
az group create --name myResourceGroup --location japaneast

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 1 \
  --vm-set-type VirtualMachineScaleSets \
  --load-balancer-sku standard \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```
<img width="909" alt="ScreenShot 2022-02-03 12 03 42" src="https://user-images.githubusercontent.com/17949085/152274651-f587da0d-6e44-4598-b796-febd306d8595.png">

### TASK1.2:　AKSへ接続
さきほど作成したAKS へ　argocd　をインストールするために、AKSへの接続する準備をします。

Azureポータル ＞ リソースグループ ＞ `myResourceGroup`
リソースグループより、`myAKSCluster`を選択して、AKSのメニューへ行き、画面上部の　**接続**　をクリックして、AKSへの接続情報取得し、
以下　az　コマンドにて　AKS　へ　アクセスできるようにします。

```
# AKS接続情報を取得
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster

# kubectl　コマンドで　AKS　クラスタ　の　worker node　を表示
kubectl get node -o wide 
```

<img width="651" alt="ScreenShot 2022-02-03 12 13 14" src="https://user-images.githubusercontent.com/17949085/152275741-b91fa40a-a49b-4133-b5a0-bdd480c6a4cc.png">
<img width="452<img width="573" alt="ScreenShot 2022-02-03 12 19 19" src="https://user-images.githubusercontent.com/17949085/152275911-720210d9-700a-4785-9c96-fb6e795f37e5.png">
<img width="1227" alt="ScreenShot 2022-02-03 12 27 17" src="https://user-images.githubusercontent.com/17949085/152276596-b2c90888-f753-4a85-84a9-82eacc742d64.png">

### TASK1.2:　ArgoCD インストール
[ArgoCD をインストールします。](https://argo-cd.readthedocs.io/en/stable/#quick-start)
**Cloudshellのbashへ**　以下をコピー＆ペースト

```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

ArgoCDのPodの状態が全て`Running`であることを確認します。
```
kubectl -n argocd get po
```

ArgoCDのGUIへアクセスするために、LBから接続できるように変更し、EXTERNAL-IPにPIPが割り振られたら`Ctrl+C`でwatchコマンドを抜けます
```
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
watch kubectl -n argocd get svc
```
ブラウザ
### TASK1.3:　ArgoCD applicationを設定
