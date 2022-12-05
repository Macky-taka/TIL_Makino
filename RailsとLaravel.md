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






参考
https://techblog.raccoon.ne.jp/archives/1550033605.html
