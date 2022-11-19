# AccountActivationsリソース
セッション機能を用いて、アカウントの有効化という作業をリソースとしてモデル化する。

### Account Activations コントローラー
有効化に次のURLを含む
edit_account_activation_url(activation_token, ...)
よって、```edit```アクションへの名前付きルートが必要になる。_pathだと相対パスになるので_urlを用いる。

### AccountActivationのデータモデル
有効化のメールには一意の有効化トークンが必要である。セキュリティも考えてパスワードの実装（第6章）や記憶トークンの実装（第9章）と同じように仮想的な属性を使ってハッシュ化した文字列をデータベースに保存するようにする。
具体的には、次のように仮想属性の有効化トークンにアクセスし、
```
user.activation_token
```
このようなコードでユーザーを認証できるようになる。
```
user.authenticated?(:activation, token)
```
次に```activated```属性を追加し論理値を取るようにする。これでユーザーが有効かどうかテストできるようになる。
Userモデルにユーザー有効化用の属性を追加する
![image of User model](https://railstutorial.jp/chapters/6.0/images/figures/user_model_account_activation.png)

###### Activationトークンのコールバック
ユーザーが新しい登録を完了するためには必ずアカウントの有効化が必要になるため、有効化トークンや有効化ダイジェストはユーザーオブジェクトが作成される前に作成しておく必要がある。
今回はオブジェクトが作成されたときのみコールバックを呼び出したい。そこで```before_create```コールバックが必要となる。
```before_create :create_activation_digest```はメソッド参照で```create_activation_digest```というメソッドを探し、ユーザーを作成する前に実行する
```create_activation_digest```メソッド自体はUserモデル内でしか使わないので、外部に公開する必要はなく、```private```を指定する。

###### サンプルユーザーの生成とテスト
サンプルデータとfixtureも更新し、テスト時のサンプルとユーザーを事前に有効化する。
```Time.zone.now```はRailsの組み込みヘルパーであり、サーバーのタイムゾーンに応じたタイムスタンプを返
