# PasswoordResetsリソース

今回はパスワードを再設定するフォームが必要なので、ビューを描画するためのnewアクションとeditアクションが必要になる。

### PasswordResetsコントローラ

パスワード再設定用のコントローラを作る。newアクションとeditアクションも一緒に生成する。

route.rbは以下のルーティング
|HTTPリクエスト|URL|Action|名前付きルート|
|-----|-----|----|---|
|GET|/password_resets/new|new	|new_password_reset_path|
|POST	|/password_resets	|create	|password_resets_path|
|GET	|/password_resets/トークン/edit	edit|	edit_password_reset_url(token)|
|PATCH|	/password_resets/トークン|	update	|password_reset_url(token)|
トークンのURLは絶対パス(https://〜)を使用する必要があるためした２つはurl

### 新しいパスワードの設定
アカウントの有効化と似ておりパスワードの再設定でも、トークン用の仮想的な属性とそれに対応するダイジェストを用意する。セキュリティ上の注意点はダイジェストにする他に、
再設定用のリンクはなるべく短時間（数時間以内）で期限切れになるようにする必要がある。
そのために再設定メールの送信時刻も記録する。
```reset_digest```属性と```reset_sent_at```属性をUserモデルに追加した結果が以下
![image of usermodel](https://railstutorial.jp/chapters/6.0/images/figures/user_model_password_reset.png)



### 
