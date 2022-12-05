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






参考
https://techblog.raccoon.ne.jp/archives/1550033605.html
