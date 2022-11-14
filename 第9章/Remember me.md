# Remember me 機能
ユーザーが明示的にログアウトを実行しない限り、ログイン状態を維持することができるようにする。

### 記憶トークンと暗号化
cookiesを永続化するとセッションハイジャックという攻撃を受ける可能性がある。この攻撃は、記憶トークンを奪って、特定のユーザーになりすましてログインするというものである。
cookiesを盗み出す有名な方法は4通りある。<br>
（1）管理の甘いネットワークを通過するネットワークパケットからパケットスニッファという特殊なソフトウェアで直接cookieを取り出す1。<br>
（2）データベースから記憶トークンを取り出す。<br>
（3）クロスサイトスクリプティング（XSS）を使う。<br>
（4）ユーザーがログインしているパソコンやスマホを直接操作してアクセスを奪い取る。<br>
対(1)としてSSLによりネットワークデータを暗号化で保護し、パケットスニッファから読み取られないようにしている。
対(2)として記憶トークンをそのままデータベースに保存するのではなく、記憶トークンのハッシュ値を保存している。
対(3)はRailsが自動的に行なっている。具体的には、ビューのテンプレートで入力した内容をすべて自動的にエスケープしている。
対(4)について、ログイン中のコンピュータへの物理アクセスによる攻撃については、さすがにシステム側での根本的な防衛手段を講じることは不可能だが、二次被害を最小限にすることは可能だ。
具体的には、ユーザーが（別端末などで）ログアウトしたときにトークンを必ず変更するようにし、セキュリティ上重要になる可能性のある情報を表示するときはデジタル署名という暗号技術を使う。
次の方針で永続的セッションを作成する<br>
記憶トークンにはランダムな文字列を生成して用いる。<br>
ブラウザのcookiesにトークンを保存するときには、有効期限を設定する。<br>
トークンはハッシュ値に変換してからデータベースに保存する。<br>
ブラウザのcookiesに保存するユーザーIDは暗号化しておく。<br>
永続ユーザーIDを含むcookiesを受け取ったら、そのIDでデータベースを検索し、記憶トークンのcookiesがデータベース内のハッシュ値と一致することを確認する。<br>
remember_digest属性をUserモデルに追加する。<br>
![Image of remember added](https://railstutorial.jp/chapters/6.0/images/figures/user_model_remember_digest.png)
記憶トークンとして何を使うか決定する。ここではRuby標準ライブラリの```SecureRandom```モジュールにある```urlsafe_base64```メソッドを使う。
```
rails console
>> SecureRandom.urlsafe_base64
=> "brl_446-8bqHv87AQzUj_Q"
```
ユーザーを記憶するには、記憶トークンを作成して、そのトークンをダイジェストに変換したものをデータベースに保存する。
```
class User < ApplicationRecord
  attr_accessor :remember_token
  .
  .
  .
  def remember
    self.remember_token = ...
    update_attribute(:remember_digest, ...)
  end
end
```
selfというキーワードを使わないと、Rubyによってremember_tokenという名前のローカル変数が作成される。
selfキーワードを与えると、この代入によってユーザーのremember_token属性が期待どおりに設定される。
```update_attribute```メソッドを使って記憶ダイジェストを更新する。

### ログイン状態の保持
永続永続セッションの作成をする。cookiesメソッドを用いる。このメソッドは、sessionのときと同様にハッシュとして扱える。
```
cookies[:remember_token] = { value:   remember_token,
                             expires: 20.years.from_now.utc }
```
```cookies[:user_id] = user.id```ならば生のテキストで穂斬されるので暗号化cookieを用いる。
```cookies.encrypted[:user_id] = user.id```
そしてcookieを永続化するために
```
cookies.permanent.encrypted[:user_id] = user.id
```
で```encrypted```と```oermanent```をメソッドチェーンで繋ぐ<br>
```cookies.encrypted[:user_id]```では自動的にユーザーIDのcookiesの暗号が解除され元に戻る。
続いて、bcryptを使って```cookies[:remember_token]```がremember_digestと一致することを確認しする。
```BCrypt::Password.new(remember_digest) == remember_token```
は```BCrypt::Password.new(remember_digest).is_password?(remember_token)```となっている。
```
if session[:user_id]
  @current_user ||= User.find_by(id: session[:user_id])
elsif cookies.encrypted[:user_id]
  user = User.find_by(id: cookies.encrypted[:user_id])
  if user && user.authenticated?(cookies[:remember_token])
    log_in user
    @current_user = user
  end
  ```
  で一時セッションがあればそこから、それ以外は```cookies[:user_id]```から取り出す。
end
