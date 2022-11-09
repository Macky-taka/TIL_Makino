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
![Image of user sample](https://railstutorial.jp/chapters/6.0/images/figures/users_table.png) <br>
マイグレーション自体は、データベースに与える変更を定義した```change```メソッドの集まりだ。
```
class CreateUsers < ActiveRecord::Migration[6.0]
  def change
    create_table :users do |t|
      t.string :name
      t.string :email

      t.timestamps
    end
  end
end
```
上の場合、```change```メソッドは```create_table```というRailsのメソッドを呼び、ユーザーを保存するためのテーブルをデータベースに作成する。
```create_table```メソッドはブロック変数を1つ持つブロックを受け取る。
ブロックの最後の行```t.timestamps```は特別なコマンドで、```created_at```と```updated_at```という２つの「マジックカラム（Magic Columns）」を作成する、
これらは、あるユーザーが作成または更新されたときに、その時刻を自動的に記録するタイムスタンプだ。<br>
![Image of user deta table](https://user-images.githubusercontent.com/116047233/200261029-85ab04c0-2720-4dde-aa12-eb427be887ff.png)
#### モデルファイル
Userのclassを確認。
```rails console --sandbox```:　そのセッションで行ったデータベースへの変更をコンソールの終了時にすべて “ロールバック”（取り消し）する。
###### オブジェクトの検索
```find```を用いる
###### オブジェクトの更新
```update````, ```update_attribute```を用いる。
