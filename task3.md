# Handson TASK3
## TASK3: CI（Github Actions）および、CIと連動した CD（GitOps）を体感
CI 用 Github project を自チームの Github アカウントへインポートし、 Github Actions の Secret 設定を行い CI の設定を完了させます。
また、Github Codespaces にてコンテンツを変更の上、Git commit / push をトリガーとして、
CI( Github Actions )によるコンテナビルド及び、GitOps 用プロジェクトの Kubernetes マニフェストを自動更新による、GitOps を体感します。

## TASK3.1: CI（Github Actions）からのコードの変更を許可するために Personal access token を発行
Github へログイン後、右上の自チームのアイコン > `Settings` > 左メニュー下の`< > Developer settings` > 左メニューの`Personal access tokens` > 右上の`Generate new token`をクリックしてアクセストークン作成画面へ移動します。

<img width="261" alt="ScreenShot 2022-02-04 12 10 13" src="https://user-images.githubusercontent.com/17949085/152472816-ff0b4224-4334-4053-bb0f-ed5ea657cd9d.png">

<img width="332" alt="ScreenShot 2022-02-04 12 17 37" src="https://user-images.githubusercontent.com/17949085/152472827-2f8acc3a-d727-4ed0-a583-e8d4a2e19da1.png">

<img width="431" alt="ScreenShot 2022-02-04 12 19 42" src="https://user-images.githubusercontent.com/17949085/152472863-72a166ec-8c3e-44eb-8cc2-376b82be932f.png">

<img width="1097" alt="ScreenShot 2022-02-04 12 20 16" src="https://user-images.githubusercontent.com/17949085/152472894-ed4e5fc6-4fb4-4a39-892a-688469fcda17.png">

<img width="1130" alt="ScreenShot 2022-02-04 12 22 35" src="https://user-images.githubusercontent.com/17949085/152473078-288a249b-f091-4f0e-9291-0c19514c23c5.png">

以下を設定し、画面下部の 緑色の`Generate token`をクリックします。
なお、作成したアクセストークンは、後ほど、Github Actions のSeceret 設定で使用するので控えておきます。

+ Note： for-github-actions-handson（任意）
+ Expiration： 30days(有効期限のデフォルト値ですが、Handson後も利用する場合、適宜変更してください）
+ Select scopes: `repo`  へチェックを入れる（必須）

## TASK3.2: CI用 Github project を自分のGithubアカウントへインポート
Github 画面右上の`+`より`Import Repogitory`をクリックします。

以下を入力して`Begin import`をクリックします。
+ Your old repository’s clone URL: https://github.com/tbuchi888/demo-js-nginx-docker-k8s.git
+ Repository Name: handson-ci-github-actions
+ Privacy: Public（任意　Privateでも可）

`Importing complete! Your new repository [YOUR-ACCOUNT-NAME]/handson-ci-github-actions is ready.`と表示されるまでまって
表示されている作成されたリポジトリへのリンクをクリックします。

## TASK3.3: CI用 Github project のGithub Actions Secretの設定
CI 用のGithub Actions のワークフローは既に作成済みです。
あとは、ビルドするコンテナイメージの情報や、更新する GitOps の Git レポジトリの情報などを
Github の秘匿情報を格納する Actions Secrets を設定することで、CI の設定が完了します。

Secret を設定するめに、TASK3.2 でインポートした Github へ移動後、画面上右側の`Setting`をクリックします。

## TASK3.4: Github Actions　を有効にする
続いて、Github Actions　を有効にするために、再度、画面上右側の`Setting`をクリックします。
左メニュー下の`< > Developer settings` > 左メニューの`Actions`> `General` をクリックして Actions permissions　を変更します。

## TASK3.5: Github Codespaces　上でコンテンツの内容を変更後、Git Commit/Push により、CIをトリガーし、GitOpsとの連携も体感
手順を追加する

以上でこのタスクは終了となります。 [TASK4](https://github.com/tbuchi888/cicd-handson/blob/main/task4.md)へ
---
