 # Userモデル
 ユーザー登録でまず初めにやることは、新しいユーザーの情報を保存するためのデータ構造を作成することである。<br>
 Railsでは
 データを永続化するデフォルトの解決策として、データベースを使ってデータを長期保存する。データベースとやり取りするデフォルトのRailsライブラリはActive Recordと呼ばれる。
 Active Recordは、データオブジェクトの作成、保存、検索のためのメソッドを持っている。これらのメソッドを使うのに、リレーショナルデータベースで使うSQL（Structured Query Language）2 を意識する必要はない。<br>
 また、RailsにはMigrationという機能がある。データの定義をRubyで記述することができ、SQLのDDL（Data Definition Language）を新たに学ぶ必要がない。<br>
 Railsは、データベースの細部をほぼ完全に隠蔽し、切り離してくれる。
 
 ### データベースの移行
RailsコンソールでUserクラスのオブジェクトを作っても、コンソールからexitするとそのオブジェクトはすぐに消えてしまう。この節での目的は、簡単に消えることのないユーザーのモデルを構築することだ。
```
class User
  attr_accessor :name, :email
  .
  .
  .
end
```
4.17では```attr_accessor```を用いた。<br>
それとは対照的に、Railsでユーザーをモデリングするときは、属性を明示的に識別する必要がない。
上で簡潔に述べたように、Railsはデータを保存する際にデフォルトでリレーショナルデータベースを使う。
リレーショナルデータベースは、データ行で構成されるテーブルからなり、各行はデータ属性のカラム（列）を持つ。
例えばnameとemailを持つユーザーを保存するのであれば、nameとemailのカラムを持つusersテーブルを作成する<br>
![Image of user sample](https://railstutorial.jp/chapters/6.0/images/figures/users_table.png)
マイグレーション自体は、データベースに与える変更を定義したchangeメソッドの集まりだ。
