# Handson TASK2
## TASK2: Git および、手動での GitOps を体感
TASK1で利用した既存の GitOps ( ArgoCD )用 Github project を自チームの Github アカウントへインポートし、Git クライアント環境として`Github Codespaces`または、`cloud shell`を利用して変更を行い Git を体感します。

また、TASK1で作成したArgoCD Application の Git の設定をインポートした物へ変更し、手動でのGitOpsを体感します。

<img width="952" alt="ScreenShot 2022-02-09 9 16 18" src="https://user-images.githubusercontent.com/17949085/153098244-3e5e14b1-1437-4e04-8fb6-51a0ce4fdafa.png">


## TASK2.1: GitOps(ArgoCD)用 Github project を自分のGithubアカウントへインポート
各チームで事前に準備した Github アカウントを使って、[Github へログイン](https://github.com/login)します。

<img width="369" alt="ScreenShot 2022-02-03 16 12 56" src="https://user-images.githubusercontent.com/17949085/152297962-25000e43-623b-4fc6-8602-5c03b06e894d.png">

ログイン後、画面右上の`+`より`Import Repogitory`をクリックします。

以下を入力して`Begin import`をクリックします。
+ Your old repository’s clone URL: https://github.com/tbuchi888/argocd-sample-repo.git
+ Repository Name: handson-gitops
+ Privacy: Public

<img width="734" alt="ScreenShot 2022-02-03 16 30 24" src="https://user-images.githubusercontent.com/17949085/152299932-7b1d17eb-ace5-4d23-8856-be0e0da30615.png">

`Importing complete! Your new repository [YOUR-ACCOUNT-NAME]/handson-gitops is ready.`と表示されるまでまって
表示されている作成されたリポジトリへのリンクをクリックします。

<img width="677" alt="ScreenShot 2022-02-03 16 34 39" src="https://user-images.githubusercontent.com/17949085/152300228-1eec8a02-3e29-4ef5-a01e-7cec9cb5703f.png">

作成した Githubへ移動後、`< >Code`緑色のアイコンをクリックし、GitリポジトリのURLをコピーします。

<img width="784" alt="ScreenShot 2022-02-03 16 40 22" src="https://user-images.githubusercontent.com/17949085/152300919-7ec339a5-9397-4b5d-b52b-f1a681be5ece.png">

## TASK2.2: ArgoCD Application のGitの設定変更
TASK1 で作成した ArgoCD Application の Git URL へ、コピーした Git リポジトリの URL を設定し、ArgoCD が参照している GitOps 用のリポジトリを切り替えます。

ArgoCD 画面右上の `APP DETAILS` > `EDIT` で編集モードに入り、`REPO URL`を作成（インポート）した、各自の Git リポジトリのURLへ変更し、`Save`し、画面右上の`X`で`APP DETAILS`画面を閉じます。
+ REPO URL: https://github.com/[YOUR-ACCOUNT-NAME]/handson-gitops.git

<img width="387" alt="ScreenShot 2022-02-03 16 44 13" src="https://user-images.githubusercontent.com/17949085/152301341-ef711789-4cad-478b-8387-53e7085f2ce7.png">
<img width="1100" alt="ScreenShot 2022-02-03 16 53 07" src="https://user-images.githubusercontent.com/17949085/152302563-41e1633d-0b9f-4d53-8f2b-8f69307b8766.png">
<img width="1097" alt="ScreenShot 2022-02-03 16 54 39" src="https://user-images.githubusercontent.com/17949085/152302648-4384d086-7200-4584-b5e0-6664d69fbc9b.png">

次に、画面上部の`SYNC` > ` SYNCRONIZE` より、変更したGitリポジトリと強制的に同期させて、`LAST SYNC RESULT Sync OK`となることを確認します。

<img width="929" alt="ScreenShot 2022-02-03 17 03 12" src="https://user-images.githubusercontent.com/17949085/152303748-d92eeb25-790b-47ae-9b0d-6b3fd84a1a03.png">
<img width="294" alt="ScreenShot 2022-02-03 17 04 55" src="https://user-images.githubusercontent.com/17949085/152303963-dae81937-c4f6-40ad-af64-0f443747c83f.png">

## TASK2.3: Git リポジトリを編集し、Git および手動での GitOps を体感 その1
GitOps 用 の Git プロジェクトについて、変更を行います。
+ `Github Codespaces`を利用可能な場合は、[TASK2.4.1](https://github.com/tbuchi888/cicd-handson/blob/main/task2.md#task241-github-codespaces-%E3%82%92%E5%88%A9%E7%94%A8%E3%81%99%E3%82%8B%E5%A0%B4%E5%90%88)を実施してください。
+ `Github Codespaces`を利用出来ない場合は、[TASK2.4.2](https://github.com/tbuchi888/cicd-handson/blob/main/task2.md#task242-cloud-shell-%E3%82%92%E5%88%A9%E7%94%A8%E3%81%99%E3%82%8B%E5%A0%B4%E5%90%88-github-codespaces-%E3%82%92%E5%88%A9%E7%94%A8%E3%81%A7%E3%81%8D%E3%81%AA%E3%81%84%E5%A0%B4%E5%90%88)を実施してください。

## TASK2.4.1: Github Codespaces を利用する場合
**`Github Codespaces`を利用出来ない場合は、こちらはスキップして、[TASK2.4.2](https://github.com/tbuchi888/cicd-handson/blob/main/task2.md#task242-cloud-shell-%E3%82%92%E5%88%A9%E7%94%A8%E3%81%99%E3%82%8B%E5%A0%B4%E5%90%88-github-codespaces-%E3%82%92%E5%88%A9%E7%94%A8%E3%81%A7%E3%81%8D%E3%81%AA%E3%81%84%E5%A0%B4%E5%90%88)へ進んでください。**

再度、作成した Githubへ移動後、`< >Code`緑色のアイコンをクリックし、`CodeSpaces`タブ > `New Codespace`をクリックします。
https://github.com/[YOUR-ACCOUNT-NAME]/handson-gitops.git

<img width="388" alt="ScreenShot 2022-02-03 17 13 29" src="https://user-images.githubusercontent.com/17949085/152305036-e7eaa6fa-150f-4228-b5c0-da384801c816.png">
<img width="772" alt="ScreenShot 2022-02-03 17 14 45" src="https://user-images.githubusercontent.com/17949085/152305225-734be4cc-c525-4eb3-ae01-c6c8eca4ca2a.png">

CodeSpaces が起動したら、好きなテーマを選択して、画面左より `k8s` > `bl.yml` をクリックして、編集対象の kubernetes のマニフェストファイルを表示します。
<img width="1134" alt="ScreenShot 2022-02-03 17 18 42" src="https://user-images.githubusercontent.com/17949085/152305816-c6f029c4-7592-4ae8-98fb-8802e32a33a3.png">

12行目の 
```
  replicas: 4
```
を
```
  replicas: 10
```
へ変更して

画面下の`TERAMINAL`へ以下をコピー＆ペーストして、変更をリモートのGitへ反映させます。

```
# Localで変更内容をCommit
git commit -a -m "1st Change"
# 変更したCommit をリモートリポジトリへ push
git push origin master
```
<img width="1026" alt="ScreenShot 2022-02-03 17 46 08" src="https://user-images.githubusercontent.com/17949085/152309716-9f6668f8-c566-4b22-aaac-290cdf5fbdb7.png">

## TASK2.4.2: Cloud shell を利用する場合( Github Codespaces を利用できない場合)
**`Github Codespaces`を利用出来ない場合は、こちらの手順で実施してください。TASK2.4.1を実施済みの場合は、こちらはスキップして、[TASK2.5](https://github.com/tbuchi888/cicd-handson/blob/main/task2.md#task25-%E6%89%8B%E5%8B%95%E3%81%A7%E3%81%AE-gitops-%E3%82%92%E4%BD%93%E6%84%9F-%E3%81%9D%E3%81%AE1%E5%85%B1%E9%80%9A)へ進んでください。**

はじめに、`Cloud shell`より、`git push`コマンドを実行するために、Githubの`Personal access token`を発行します。
Github へログイン後、右上の自チームのアイコン > `Settings` > 左メニュー下の`< > Developer settings` > 左メニューの`Personal access tokens` > 右上の`Generate new token`をクリックしてアクセストークン作成画面へ移動します。

<img width="261" alt="ScreenShot 2022-02-04 12 10 13" src="https://user-images.githubusercontent.com/17949085/152472816-ff0b4224-4334-4053-bb0f-ed5ea657cd9d.png">

<img width="332" alt="ScreenShot 2022-02-04 12 17 37" src="https://user-images.githubusercontent.com/17949085/152472827-2f8acc3a-d727-4ed0-a583-e8d4a2e19da1.png">

<img width="431" alt="ScreenShot 2022-02-04 12 19 42" src="https://user-images.githubusercontent.com/17949085/152472863-72a166ec-8c3e-44eb-8cc2-376b82be932f.png">

<img width="1097" alt="ScreenShot 2022-02-04 12 20 16" src="https://user-images.githubusercontent.com/17949085/152472894-ed4e5fc6-4fb4-4a39-892a-688469fcda17.png">

以下を設定し、画面下部の 緑色の`Generate token`をクリックします。
なお、作成したアクセストークンは、後ほど、Github Actions のSeceret 設定でも使用するのでコピーして、控えておきます。（作成時しか確認出来ませんので、控え忘れた場合は、再作成となります。）

+ Note： for-github-actions-handson（任意）
+ Expiration： 30days(有効期限のデフォルト値ですが、Handson後も利用する場合、適宜変更してください）
+ Select scopes: `repo`  へチェックを入れる（必須）

<img width="1130" alt="ScreenShot 2022-02-04 12 22 35" src="https://user-images.githubusercontent.com/17949085/152473078-288a249b-f091-4f0e-9291-0c19514c23c5.png">

<img width="813" alt="ScreenShot 2022-02-07 12 13 21" src="https://user-images.githubusercontent.com/17949085/152718824-6b70373a-25a1-48b7-9474-0dac2a2681c2.png">


次に、[Azure ポータル](https://ms.portal.azure.com/)へログインし、 **cloud shell** を起動し、`git clone`コマンドで ソースコードをローカルにクローンします。

以下[YOUR-ACCOUNT-NAME]は各自で置き換えてくささい

```
# git global 設定 （設定していない場合）
git config --global user.name "[YOUR-ACCOUNT-NAME]"
git config --global user.email "[YOUR-ACCOUNT-NAME]@users.noreply.github.com"
# ソースコードをクローン
git clone https://github.com/[YOUR-ACCOUNT-NAME]/handson-gitops.git
# ソースコードのディレクトリへ移動
cd handson-gitops
# ファイルやディレクトリを表示
ls -al
```

`code` または`vi`等のテキストエディターを利用して編集対象の kubernetes のマニフェストファイル`k8s/bl.yml`を開きます。

例）
```
# code 
code k8s/bl.yml
```

12行目の 
```
  replicas: 4
```
を
```
  replicas: 10
```
へ変更してファイルを保存します。

以下`code`エディターでの編集の例

<img width="645" alt="ScreenShot 2022-02-08 17 09 16" src="https://user-images.githubusercontent.com/17949085/152947041-14369966-40e9-41f8-a4b8-81c6ae0d5e75.png">

<img width="1165" alt="ScreenShot 2022-02-08 17 42 35" src="https://user-images.githubusercontent.com/17949085/152950060-5a6e428f-e771-4e86-be35-19273485734d.png">


次にTERMINALへ以下をコピー＆ペーストして、変更をリモートのGitへ反映させます。
なお、`git push`コマンド実行時に対話型で Github の`Username`（アカウント名）と`Password`（先ほど発行した`Personal access token`**Githubのパスワードではないので注意**）を聞かれますのでそれぞれ入力します。

```
# Localで変更内容をCommit
git commit -a -m "1st Change"
# 変更したCommit をリモートリポジトリへ push
git push origin master
```

<img width="755" alt="ScreenShot 2022-02-08 17 20 36" src="https://user-images.githubusercontent.com/17949085/152947596-523b1e69-d9fb-4905-bde7-7fe066506ac4.png">



## TASK2.5: 手動での GitOps を体感 その1（共通）
Github へ戻って、変更が反映されていることを確認
<img width="800" alt="ScreenShot 2022-02-03 17 38 39" src="https://user-images.githubusercontent.com/17949085/152309292-35f294ac-278f-45e0-b112-8bdc266366b0.png">

<img width="545" alt="ScreenShot 2022-02-03 17 38 27" src="https://user-images.githubusercontent.com/17949085/152308903-a2c28e66-c552-45b2-97ce-742db3f2f8fd.png">

ArgoCD 側で 参照している Git 上の変更を検知して、自動的に Kuberentes（AKS）へ変更を反映することを確認
<img width="1140" alt="ScreenShot 2022-02-03 17 31 48" src="https://user-images.githubusercontent.com/17949085/152308859-2bc8ae50-dc96-41f9-85c3-0776552a6315.png">

<img width="1187" alt="ScreenShot 2022-02-03 17 39 28" src="https://user-images.githubusercontent.com/17949085/152309348-26ea5010-e52c-47f4-ad76-3b3834e2b695.png">



## TASK2.6: 手動での GitOps を体感 その2（共通）
再度、`Codospaces` または、`Cloud shell`へ戻り、`bl.yml`を以下の通り、変更し、リモートリポジトリへ変更反映

+ TASK2.2を参考にチャンレジしてください 
+ YAML形式なので、インデントに注意して作業してください
+ 12行目の`replicas`を 4 に戻す
+ 27行目のコンテナイメージ`image`を以下へ変更

```
      - image: docker.io/takuyak/nginx-sample-gr:v1
```

Git でリポジトリの内容をアップデートします。

```
# Localで変更内容をCommit
git commit -a -m "2nd Change"
# 変更したCommit をリモートリポジトリへ push
git push origin master
```

ArgoCD  に戻り、上記変更が反映されることを確認し、ブラウザ より アプリケーション（コンテナイメージ）が変更されたことを確認してください。
ブラウザのキャッシュが残っていて、変更が反映されない場合は、シークレットウィンドウで再度開いてください。

![image](https://user-images.githubusercontent.com/15963767/152686678-925933aa-a404-4467-a0c5-6c07e4d09d4f.png)

確認ができたら先に進みます。


つづいて、再度、`Codospaces` または、`cloud shell`へ戻り、`bl.yml`を以下の通り、変更し、リモートリポジトリへ変更反映します。

+ TASK2.4を参考にチャンレジしてください 
+ YAML形式なので、インデントに注意して作業してください
+ 27行目のコンテナイメージ`image`のタグ部分を`v1`から`v2`へ変更

```
      - image: docker.io/takuyak/nginx-sample-gr:v2
```

Git でリポジトリの内容をアップデートします。

```
# Localで変更内容をCommit
git commit -a -m "3rd Change"
# 変更したCommit をリモートリポジトリへ push
git push origin master
```

再度、ArgoCD  に戻り、上記変更が反映されることを確認し、ブラウザ より アプリケーション（コンテナイメージ）が変更されたことを確認してください。
ブラウザのキャッシュが残っていて、変更が反映されない場合は、シークレットウィンドウで再度開いてください。

以上でこのタスクは終了となります。
[TASK3へ](https://github.com/tbuchi888/cicd-handson/blob/main/task3.md)

---
