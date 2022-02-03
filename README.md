# cicd-handson
CI/CD Handson の説明資料です
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

## TASK1: Kubernetes および　 GitOps の初歩を体感
Handsonの環境を構築します。
### AKS(Azure Kubernetes Service)のクラスターを作成
[Azure ポータル](https://ms.portal.azure.com/)へログインし、 **Cloudshell**　より　以下　az コマンドでAzure上に、リソースグループとAKSクラスタを作成します。
<img width="1115" alt="ScreenShot 2022-02-03 11 55 21" src="https://user-images.githubusercontent.com/17949085/152273919-6f441588-a674-41e7-a5b7-eb9643c0246b.png">


```
# First create a resource group
az group create --name myResourceGroup --location eastus

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
