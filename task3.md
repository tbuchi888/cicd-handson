# Handson TASK3
## TASK3: CI（Github Actions）および、CIと連動した CD（GitOps）を体感
CI 用 空のコンテナイメージを Dockerhub へPublicとして新規作成します。 
CI 用 Github project を自チームの Github アカウントへインポートし、 Github Actions の Secret 設定を行い CI の設定を完了させます。
また、`Github Codespaces`や`cloud shell`にてコンテンツを変更の上、Git commit / push をトリガーとして、
CI( Github Actions )によるコンテナビルド及び、GitOps 用プロジェクトの Kubernetes マニフェストを自動更新による、GitOps を体感します。

<img width="952" alt="ScreenShot 2022-02-09 9 16 27" src="https://user-images.githubusercontent.com/17949085/153097731-f20e683e-792a-472d-b498-fb7f824b7c69.png">


## TASK3.1: CI 用 空のコンテナイメージを Dockerhub へ新規作成 および Access Token を発行
Dockerhub へログイン後、画面上の`Create Repository` をクリックして作成画面へ移動します。
<img width="1142" alt="ScreenShot 2022-02-07 13 00 26" src="https://user-images.githubusercontent.com/17949085/152723036-ba1d37db-6d0e-451b-a15e-5cb1f5f45358.png">

<img width="1155" alt="ScreenShot 2022-02-07 13 00 42" src="https://user-images.githubusercontent.com/17949085/152723100-9338fb84-a56f-46cf-a1c9-e74d248eba1d.png">

以下を入力し`Create`をクリックし、空のコンテナイメージを新規に作成します。
+ Repository名: handson-ci-image（コンテナイメージ名）
+ Visibility: Public

<img width="1160" alt="ScreenShot 2022-02-07 13 01 08" src="https://user-images.githubusercontent.com/17949085/152723138-cfdd7093-1faa-4ea6-8f80-af14735366ce.png">

<img width="1165" alt="ScreenShot 2022-02-07 13 01 22" src="https://user-images.githubusercontent.com/17949085/152723398-a8f240fa-610a-4f91-a6c2-d04a8d10bad3.png">


次に画面右上のアカウント名をクリックし、`Account Settings` > `Security` > `Access Tokens`で`New Access Token`をクリックしてｔｏｋｅｎ発行します。

![ScreenShot 2022-02-07 17 53 10](https://user-images.githubusercontent.com/17949085/152755873-be5a212f-990b-4754-b953-5bdb1a89f11f.png)

![ScreenShot 2022-02-07 17 55 27](https://user-images.githubusercontent.com/17949085/152756468-bf939436-e03a-4b1b-99fb-d86755f61aba.png)


以下で`Generate`をクリックし、発行された`Access Token`は後ほど、使うので控えておきます。
（控えられなかった場合は、削除して再作成となるので注意）
+ Access Token Description * : For github actions（任意の説明）
+ Access permissions:Read, Write, Delete（初期値のまま）

![ScreenShot 2022-02-07 17 57 36](https://user-images.githubusercontent.com/17949085/152756503-b2836f8e-071e-463d-9d1c-b79e38ecf122.png)

<img width="734" alt="ScreenShot 2022-02-07 18 14 37" src="https://user-images.githubusercontent.com/17949085/152759222-b66d226e-a09c-41a1-a2ff-976ec837313a.png">

## TASK3.2: CI（Github Actions）からのコードの変更を許可するために Personal access token を発行
**TASK2.4.2で`Personal access token`を既に発行済みの場合は、この手順はスキップして、TASK3.3へ進んでください。**
Github へログイン後、右上の自チームのアイコン > `Settings` > 左メニュー下の`< > Developer settings` > 左メニューの`Personal access tokens` > 右上の`Generate new token`をクリックしてアクセストークン作成画面へ移動します。

<img width="261" alt="ScreenShot 2022-02-04 12 10 13" src="https://user-images.githubusercontent.com/17949085/152472816-ff0b4224-4334-4053-bb0f-ed5ea657cd9d.png">

<img width="332" alt="ScreenShot 2022-02-04 12 17 37" src="https://user-images.githubusercontent.com/17949085/152472827-2f8acc3a-d727-4ed0-a583-e8d4a2e19da1.png">

<img width="431" alt="ScreenShot 2022-02-04 12 19 42" src="https://user-images.githubusercontent.com/17949085/152472863-72a166ec-8c3e-44eb-8cc2-376b82be932f.png">

<img width="1097" alt="ScreenShot 2022-02-04 12 20 16" src="https://user-images.githubusercontent.com/17949085/152472894-ed4e5fc6-4fb4-4a39-892a-688469fcda17.png">

以下を設定し、画面下部の 緑色の`Generate token`をクリックします。
なお、作成したアクセストークンは、後ほど、Github Actions のSeceret 設定で使用するのでコピーして、控えておきます。（作成時しか確認出来ませんので、控え忘れた場合は、再作成となります。）

+ Note： for-github-actions-handson（任意）
+ Expiration： 30days(有効期限のデフォルト値ですが、Handson後も利用する場合、適宜変更してください）
+ Select scopes: `repo`  へチェックを入れる（必須）

<img width="1130" alt="ScreenShot 2022-02-04 12 22 35" src="https://user-images.githubusercontent.com/17949085/152473078-288a249b-f091-4f0e-9291-0c19514c23c5.png">

<img width="813" alt="ScreenShot 2022-02-07 12 13 21" src="https://user-images.githubusercontent.com/17949085/152718824-6b70373a-25a1-48b7-9474-0dac2a2681c2.png">

## TASK3.3: CI用 Github project を自分のGithubアカウントへインポート
Github 画面右上の`+`より`Import Repogitory`をクリックします。

以下を入力して`Begin import`をクリックします。
+ Your old repository’s clone URL: https://github.com/tbuchi888/demo-js-nginx-docker-k8s.git
+ Repository Name: handson-ci-github-actions
+ Privacy: Public（任意 Privateでも可）

`Importing complete! Your new repository [YOUR-ACCOUNT-NAME]/handson-ci-github-actions is ready.`と表示されるまでまって
表示されている作成されたリポジトリへのリンクをクリックします。

## TASK3.4: CI用 Github project のGithub Actions Secretの設定
CI 用のGithub Actions のワークフローは既に作成済みです。
あとは、ビルドするコンテナイメージの情報や、更新する GitOps の Git レポジトリの情報などを
Github の秘匿情報を格納する Actions Secrets を設定することで、CI の設定が完了します。

Secret を設定するめに、TASK3.2 でインポートした Github へ移動後、画面上右側の`Setting`をクリックします。
左メニュー下の`Security` `Secrets`> `Actions` をクリックして `Actions secrets`画面を表示します。

<img width="893" alt="ScreenShot 2022-02-07 11 25 45" src="https://user-images.githubusercontent.com/17949085/152715362-cfa16061-ef99-44e6-924d-503f217bff82.png">
<img width="1136" alt="ScreenShot 2022-02-07 11 28 32" src="https://user-images.githubusercontent.com/17949085/152715382-88fc0701-c59c-428e-8658-28ce9238be1a.png">


`New repository secret`をクリックし以下6つの Secrets を追加します。
+ DOCKER_USERNAME: 各チームで事前に用意した Dockerhub アカウント
+ DOCKER_PASSWORD: 前のタスクで生成した`Access token`の値（パスワードではないので注意）
+ DOCKER_REPONAME: handson-ci-image（コンテナイメージ名）
+ GIT_USERNAME_DEPLOY: 各チームで事前に用意した Github アカウント（前タスクで作成した GitOps 用の Github アカウント）
+ GIT_PASSWORD_DEPLOY: 前のタスクで生成した`Personal access token`の値（パスワードではないので注意）
+ GIT_REPONAME_DEPLOY: handson-gitops（前タスクで作成した GitOps 用の Github プロジェクト名）

<img width="1132" alt="ScreenShot 2022-02-07 11 49 54" src="https://user-images.githubusercontent.com/17949085/152717952-1f10f618-0c68-44f2-9066-ab9071f2a545.png">

<img width="1154" alt="ScreenShot 2022-02-07 12 03 48" src="https://user-images.githubusercontent.com/17949085/152718071-f21dd659-ed69-499f-a94a-9ee990bf7fc6.png">

## TASK3.5: Github Actions を有効にする
続いて、Github Actions を有効にするために、再度、画面上右側の`Setting`をクリックします。
左メニュー下`Security`の`Secrets`> `Actions` をクリックして Actions permissions を変更します。
`Allow all actions`を選択して`Save`をクリックします。

<img width="1169" alt="ScreenShot 2022-02-07 11 59 29" src="https://user-images.githubusercontent.com/17949085/152717717-adb510a8-275b-43a1-ad8b-bdcc7833b743.png">

以上で CI ( Github Actions )の準備が整いました。

## TASK3.6: コンテンツの内容を変更後、Git Commit/Push により、CI をトリガーし、GitOps との連携も体感
いよいよ、Web コンテンツの内容を変更し、Git コマンドで変更を反映させることで、 CI ( Github Actions )を起動します。
CIの主な内容は以下の2つです。
+ Web コンテンツを含む、コンテナイメージをビルドして、Dockerhub へ push
+ GitOps（ ArgoCD ）用の Github プロジェクト（ Githubアカウント名/handson-gitops ）にある、Kubernetes マニフェストファイル（ k8s/bl.yml ）を新規にビルドしたコンテナイメージのタグ名に書き換える
  
実際の CI ( Github Actions )の内容については、handson-ci-github-actions Github プロジェクトの`.github/workflows/main.yml`に記載されています。
こちらの中で、先ほど設定した秘匿情報である Secrets の値を` ${{ secrets.DOCKER_USERNAME }}`のように記述して呼び出しを行い
秘匿情報をコードに直接記述することなく、ログについてもマスクする等、セキュアに処理を行なうことが可能です。


それでは、はじめる前に、あらかじめ以下をブラウザで開いておきます。
+ Githubアカウント名/handson-ci-github-actions Github プロジェクト
+ Githubアカウント名/handson-gitops プロジェクト
+ Dockerhub( Login 済み handson-ci-image を表示)
+ ArgoCD( Login 済み Application のNW表示)

`handson-ci-github-actions`Github プロジェクトを
+ `Github Codespaces` で開く、
または、
+ Azure Portalの`cloud shell`で`git clone`し、

以下の部分を変更して、Gitコマンドで変更をリモートリポジトリ（`handson-ci-github-actions` Github プロジェクト）へ反映させます。

+ 変更点1: `html/index.html` 14〜20行目の HTML の Body 部分を変更します。（例： v1 から v2 ） 

```
        <p>The HOSTNAME is $HOSTNAME </p>
        <p>The version is demo </p>
        <p>The version is v1 </p>
        <p>The version is v1 </p>
        <p>The version is v1 </p>
        <p>The version is v1 </p>
        <p>The version is v1 </p>
```

+ 変更点2: `html/css/style.css` 88行目の CSS の背景色を変更します。（例： `#ffe3f1` ）

```
  background-color: #c9f2ff;
```

+ 参考：Googleでカラーコードを検索できます。

<img width="300" alt="ScreenShot 2022-02-07 14 26 10" src="https://user-images.githubusercontent.com/17949085/152935561-4742576a-60fe-4241-b24a-ebe95bd084bc.png">



以下 Git コマンドで変更をリモートリポジトリへ反映させます。

```
git commit -a -m "Changed the contents of index.html and css.js."
git push origin master
```

 CI ( Github Actions )が起動されますので、以下の状況を確認します。
+ Githubアカウント名/handson-ci-github-actions Github プロジェクト
  + `Actions`タブより、 CI ( Github Actions )の進捗状況を確認
    + 正常に完了（グリーン）することを確認
+ Githubアカウント名/handson-gitops プロジェクト
  + `k8s/bl.yml`のコンテナイメージのタグが、自動的に更新されていることを確認
+ Dockerhub( Login 済み handson-ci-image を表示)
  + 自動的にイメージがpushされ、タグが更新されていることを確認
+ ArgoCD( Login 済み Application のNW表示)
  + Githubアカウント名/handson-gitops プロジェクトの更新を検知して、AKS上のPod（コンテナ）が、ローリングアップデートされることを確認
    + ポーリング間隔（デフォルト3分）により、反映タイミングが多少前後します。

最後に、ブラウザより再度アプリケーションを開き、変更（ body 部及び背景色）が反映されたことを確認してください。
なお、ブラウザのキャッシュが残っていて、変更が反映されない場合は、シークレットウィンドウで再度開いてください。

以上でこのタスクは終了となります。 [TASK4](https://github.com/tbuchi888/cicd-handson/blob/main/task4.md)へ

---
