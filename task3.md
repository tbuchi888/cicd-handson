# Handson TASK3
## TASK3: Git および、手動での GitOps を体感
CI　用 Github project を自チームの Github アカウントへインポートし、 Github Actions の Secret 設定を行い
Github Codespaces での、Git commit / push トリガーに、CIによるコンテナビルド及び、GitOps プロジェクトの　Kubernetes　マニフェストを自動更新による、GitOps　を体感します。

## TASK3.1: CI（Github Actions）　からのコードの変更を許可するために　API TOKEN　を発行
Github　へログイン後、右上の自チームのアイコン > `Settings` > 左メニュー下の`<　>　Developer settings` > 左メニューの`Personal access tokens` > 右上の`Generate new token`をクリックしてアクセストークン作成画面へ移動します。

以下を設定し、画面下部の　緑色の`Generate token`をクリックします。
なお、作成したアクセストークンは、後ほど、Github Actions のSeceret 設定で使用するので控えておきます。

+ Note： for-github-actions-handson（任意）
+ Expiration： 30days(有効期限のデフォルト値ですが、Handson後も利用する場合、適宜変更してください）
+ Select scopes: `repo` 　へチェックを入れる（必須）

## TASK3.2: CI用 Github project を自分のGithubアカウントへインポート
Github　画面右上の`+`より`Import Repogitory`をクリックします。

以下を入力して`Begin import`をクリックします。
+ Your old repository’s clone URL: https://github.com/tbuchi888/demo-js-nginx-docker-k8s.git
+ Repository Name: handson-ci-github-actions
+ Privacy: Public

`Importing complete! Your new repository [YOUR-ACCOUNT-NAME]/handson-ci-github-actions is ready.`と表示されるまでまって
表示されている作成されたリポジトリへのリンクをクリックします。

## TASK3.3: CI用 Github project　のGithub Actions Secretの設定
CI　用のGithub Actions　のワークフローは既に作成済みです。
あとは、ビルドするコンテナイメージの情報や、更新する　GitOps　の　Git　レポジトリの情報などを
Github　の秘匿情報を格納する　Actions　Secrets　を設定することで、CI　の設定が完了します。

Secret　を設定するめに、TASK2.2　でインポートした Github　へ移動後、画面上右側の`Setting`をクリックします。

## TASK3.4:　コンテンツの内容を変更後、Git Commit/Push により、CIをトリガーする 
