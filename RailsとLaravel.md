# Rials and Laravel

RailsとLaravelの違いについて学習する。

### ルーティング
Laravel<br>
routes/web.php
```
<?php
Route::get('/', 'HogeController@fuga');
```
Rails<br>
config/routes.rb
```
Rails.application.routes.draw do
  get '/', to: 'hoge#fuga'
end
```
'/'にgetでアクセスが来た時に、hogeコントローラーのfugaメソッドを実行する処理を表す。
getの他のpost, put, patch, deleteも同様でRESTルーティングが一気に定義できる「resource」も両方とも使用できる。
<br>
今更だがメタ構文変数について:<br>
慣習として、説明したいことの趣旨とは一切関係ない、なんでもいいことを示す名前として用いられる。
(https://media.wemotion.co.jp/class-diary/%E3%80%90q%EF%BC%86a%E3%80%91%E8%A7%A3%E8%AA%AC%E8%A8%98%E4%BA%8B%E3%81%AB%E5%87%BA%E3%81%A6%E3%81%8F%E3%82%8B%E3%80%8Choge%E3%80%8D%E3%81%A8%E3%81%8B%E3%80%8Cfuga%E3%80%8D%E3%81%A3%E3%81%A6%E3%81%AA/)

### ORM

ORMのフレームワークとして、Laravelは「eloquent」、Railsは「ActiveRecord」という名称がついている。
どちらもActive Recordパターンを実装したORMだ。基本的にはModelクラスを生成い、そのModelクラスを使ってメソッドを実行することでCRUD操作ができる。<br>
＊CRUD＝データの作成（Create）、読み出し（Read）、更新（Update）、削除（Delete）の頭文字を繋げた語<br>

そもそもORMとは、Oject-Relational Mappingでオブジェクトと関係(RDB、関係データベース)とのマッピングを行う。createメソッドで新規作成を行ったり、destroyメソッドで削除することに該当。

##### Modelの生成

初期状態の比較
Laravel
```
<?php
 
namespace App;
 
use Illuminate\Database\Eloquent\Model;
 
class User extends Model
{
    //
}
 
```
Rails
```
class User < ApplicationRecord
end
```
書き方に違いはあるが（言語仕様）、これでCRUD操作が可能。
以下CRUDの例。
|	|Laravel|	Rails|
|---|---|-------|
|create|	$user = new User;<br>$user->name = 'saito;<br>$user->save();|user = User.new<br>user.name = 'saito'<br>user.save|
|read|	$users = User::all();<br>$user = User::find(1);	|users = User.all<br>user = User.find(1)|
|update|$user = User::find(1);<br>$user->name = 'tanaka;<br>$user->save();|user = User.find(1)<br>user.name = 'tanaka'<br>user.save|
|delete|	$user = User::find(1);<br>$user->delete();|user = User.find(1)<br>user.destroy|

書き方は非常に似ている。

機能の違いとしてLaravekには論理削除機能が標準搭載されている。Railsはgemを入れれば実現可能。<br>
*論理削除とは、RBDを削除する際、現在有効か否かを示すフィードの値を変更することで削除扱いにする方式。実際のデータは削除しない。（https://e-words.jp/w/%E8%AB%96%E7%90%86%E5%89%8A%E9%99%A4.html）

 ### マイグレーション
 
 どちらも実装されている。コマンドは後ほど見る。
 
 ##### マイグレーションファイル
Useerモデルと紐付いている、userテーブルで考える。
userテーブルの定義は以下。
|カラム名	|データ型	|NULL|	Key	Default|	属性|
|-------|------|-----|-------------|-----|
|id	|int|	NO|	PRI|	NULL|	auto_increment|
|name	|varchar	|NO|	|	NULL|	|
|email|	varchar|	NO|	|	NULL	|unique|
|password|	varchar	|NO	||	NULL	||
|comment|	text	|YES	||	NULL|	|
|create_at	|timestamp	|NO|	|	NULL|	|
*varchar：可変文字列
*プライマリーキ-は最も適したカラムに設定されるもの。<br>


マイグレーションファイルの比較
Laravel
```
<?php
 
use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;
 
class CreateUsersTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->increments('id');
            $table->string('name');
            $table->string('email')->unique();
            $table->string('password');
            $table->text('comment')->nullable();
            $table->timestamps();
        });
    }
 
    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('users');
    }
}
```
Rails
```
class CreateUsers < ActiveRecord::Migration[5.2]
  def change
    create_table :users do |t|
      t.string :name, null: false
      t.string :email, unique: true, null: false
      t.string :password, null: false
      t.text :comment
      t.timestamps null: false
    end
  end
end
```

テーブル作成のマイグレーションファイルをコマンで生成するとRAilsではchangeメソッドがデフォルトで記述されたものが生成され、Laravelではupメソッドとdownメソッドがデフォルトで記述されたものが生成される。<br>
Laravelではプライマリーキーになるidカラムの定義を記述しているが、 Railsでは、なのも記述しなくてもidカラムは生成される。<br>
これらをマイグレートするとLaravelにidは「int(10) unsigned」となり、Railsのidは「bigint(20)」となり、同じテーブルにはならない。
参考：https://confrage.jp/mysql%E3%81%AEintbigintdecimal%E3%81%AE%E4%BD%BF%E3%81%84%E5%88%86%E3%81%91%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6/ <br>
https://blog.s-giken.net/367.html
<br>
また、LaravelではデフォルトでNOT NULLの制約がつくため、NULLを許可するカラムに"nullable()"とつけるが、Railsでは”NULL:false”でNOT NULL制約が定義される。

### DI
DI（Dependency Injection）：オブジェクトを成立させるために必要なコードをを実行時に注入する概念。詳しくはhttps://e-words.jp/w/%E4%BE%9D%E5%AD%98%E6%80%A7%E6%B3%A8%E5%85%A5.html <br>

Laravelには機能があり、Railsにはない。(だから理解しにくかったのか...）
LaravelのDI機能は簡潔には下記になる。<br>

・Laravelアプリケーションの起動処理（サービスプロパイダと呼ぶ）の中で、DIするオブジェクトを格納するコンテナ（サービスコンテナと呼ぶ）へオブジェクトを登録する。<br>
・サービスコンテナからオブジェクトを呼び出す

### バリデーション

記述場所とタイミング

|Laravel	|Rails|
|-------|-------|
|記述場所	|Controller(FormRequest)	|Model|
|推奨実行タイミング	|Controllerのメソッド内任意	|Modelのcreateやsave,updateの実行時|

推奨実行タイミングではあるため、どこでやってもいいが、フレームワークとしては上記の場所で実行する方が見た目は良さそう。<br>
Laravelの記述場所である"FormRequest"だが、これを用いるとControllerメソッドに入ったタイミングでバリデーションが完了している。

##### コード
場所が違うため、見た目も変わる
バリデーションの定義
|項目名	|バリデーション|
|------|----------|
|name|	必須,100文字以下|
|email|	必須,100文字以下,メール形式,usersテーブルのemailカラムに同じ値が存在しない|
|password	|必須,6文字以上, 12文字以下|
Laravel
UserRequest.php
```
<?php
 
namespace App\Http\Requests;
 
use Illuminate\Foundation\Http\FormRequest;
 
class UserRequest extends FormRequest
{
    public function authorize()
    {
        return true;
    }
 
    public function rules()
    {
        return [
            'name' => ['required', 'max:100'],
            'email' => ['required', 'max:100', 'email', unique:users],
            'password' => ['required', 'min:6', 'max:12'],
        ];
    }
```
Rails 
User.rb
```
class User < ApplicationRecord
    VALID_EMAIL_REGEX = /\A[\w+\-.]+@[a-z\d\-.]+\.[a-z]+\z/i
 
    validates :name, presence: true, length: { maximum: 100 }
    validates :email, presence: true, length: { maximum: 100 }, format: { with: VALID_EMAIL_REGEX }, uniqueness: true
    validates :password, presence: true, length: { in: 6..12 }
end
```
基本的には似ている。Laravelはメールアドレスの形式チェックが最初から用意されているように、バリデーションの種類が豊富。

### コマンド

まとめ一覧
|操作	|Laravel	|Rails|
|---|------|--------|
|ルート確認	|php artisan route:list	|rails routes|
|Controller作成	|php artisan make:controller コントローラー名	|rails generate controller コントローラー名|
|Model作成	|php artisan make:model モデル名	|rails generate model モデル名|
|マイグレーションファイル作成	|php artisan make:migration マイグレーションファイル名	|rails generate migration マイグレーションファイル名|
|マイグレーション実行	|php artisan migrate	|rails db:migrate|
|シーディング実行	|php artisan db:seed	|rails db:seed|
|サーバ起動	|php artisan serve	|rails server|
|REPL	|php artisan tinker	|rails console|

基本的に似ている。
サービスプロバイダやフォームリクエストもコマンドで生成する。

|操作	|Laravel|
|サービスプロバイダ作成	|php artisan make:provider サービスプロバイダ名|
|FormRequest作成	|php artisan make:request フォームリクエスト名|



使用例として
Laravel:時事通信ニュース、グルナビ<br>
Rails:クックパッド、価格.com<br>
Laravelの方が自由度が高いが、コードが複雑になりがち

　###### パッケージ管理
 Ruby: bundler<br>
 RHP: Composer
 
 Railsはgemで様々なライブラリを使用できる。
 
 










参考
https://techblog.raccoon.ne.jp/archives/1550033605.html
https://keyua.org/blog/rails-vs-laravel-in-depth-comparison/
