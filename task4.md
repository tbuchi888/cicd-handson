# Handson TASK4
## TASK4: Handson 終了後の後片付け、Azure リソース等の削除
Handson で利用した Azure リソース等を削除します。

## TASK4.1: 各チームで TASK1 で作成した AKS 用のリソースグループを削除
Azure ポータルより、TASK1 で作成したリソースグループ`myResourceGroupXXX`を表示します。

<img width="538" alt="ScreenShot 2022-02-07 11 04 25" src="https://user-images.githubusercontent.com/17949085/152713678-d279b3c2-f7ec-4f65-b25f-3e153519d07f.png">

画面上段の`リソース　グループの削除`をクリックし、`リソース グループ名を入力してください:`へリソース グループ名`myResourceGroupXXX`を入力し、`削除`ボタンをクリックします。

<img width="775" alt="ScreenShot 2022-02-07 11 06 05" src="https://user-images.githubusercontent.com/17949085/152713929-b6030c87-0ec7-4682-90c9-bde2d63445ad.png">

<img width="592" alt="ScreenShot 2022-02-07 11 08 34" src="https://user-images.githubusercontent.com/17949085/152713936-c560c394-1ef2-4d13-be9c-4c15737db351.png">



## TASK4.2: Github の Personal access token  を削除
Github Actions　の Secret の設定で利用した Github の`Personal access token`を削除します。

Github へログイン後、右上の自チームのアイコン > `Settings` > 左メニュー下の`< > Developer settings` > `Personal access tokens` 画面へ移動し、作成した`Personal access token`の`Delete`をクリックし削除します。

<img width="261" alt="ScreenShot 2022-02-04 12 10 13" src="https://user-images.githubusercontent.com/17949085/152472816-ff0b4224-4334-4053-bb0f-ed5ea657cd9d.png">

<img width="332" alt="ScreenShot 2022-02-04 12 17 37" src="https://user-images.githubusercontent.com/17949085/152472827-2f8acc3a-d727-4ed0-a583-e8d4a2e19da1.png">

<img width="431" alt="ScreenShot 2022-02-04 12 19 42" src="https://user-images.githubusercontent.com/17949085/152472863-72a166ec-8c3e-44eb-8cc2-376b82be932f.png">

<img width="1122" alt="ScreenShot 2022-02-07 18 01 35" src="https://user-images.githubusercontent.com/17949085/152762920-dd5b3f98-d561-4257-a0c9-adcdcd9b7fd8.png">

![ScreenShot 2022-02-07 18 40 14](https://user-images.githubusercontent.com/17949085/152763271-b5c23534-7f45-49c3-a11f-79e711fd4ed1.png)



## TASK4.3: Dockerhub Access Token を削除
Github Actions　の Secret の設定で利用した Dockerhub の Access Token を削除します。
Dockerhub へログイン後、画面右上のアカウント名をクリックし、`Account Settings` > `Security` > `Access Tokens`へ移動します。

<img width="1142" alt="ScreenShot 2022-02-07 13 00 26" src="https://user-images.githubusercontent.com/17949085/152723036-ba1d37db-6d0e-451b-a15e-5cb1f5f45358.png">

![ScreenShot 2022-02-07 17 53 10](https://user-images.githubusercontent.com/17949085/152755873-be5a212f-990b-4754-b953-5bdb1a89f11f.png)

作成した`Access Token`を選択（チェック）した状態で、`Delete`をクリックし、`Delete　Forvever`をクリックして削除します。

![ScreenShot 2022-02-07 18 09 04](https://user-images.githubusercontent.com/17949085/152760898-1ae6922a-c48f-4963-8c0d-7e887c9637e2.png)

![ScreenShot 2022-02-07 18 27 13](https://user-images.githubusercontent.com/17949085/152761260-8d515bdc-4a23-4315-bf4f-041adc1af901.png)



## TASK4.4: 各チーム用に事前に作成した、サービスプリンシパルを Azure より削除
事前作業でサービスプリンシパルを作成したメンバーへ削除を依頼してください。
