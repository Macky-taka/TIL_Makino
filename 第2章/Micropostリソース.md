# Micropostリソース
#### Micropostsリソースが提供するリスト 2.12のRESTfulルート
| HTTPリクエスト	| URL	| アクション |	用途 |
| ----------- | ---- | ------- | ---- |
| GET	| /microposts	| index	| すべてのマイクロポストを表示するページ |
| GET	| /microposts/1 |	show | id=1のマイクロポストを表示するページ |
| GET	| /microposts/new	| new	| マイクロポストを新規作成するページ |
| POST	| /microposts	| create	| マイクロポストを新規作成するアクション |
| GET	| /microposts/1/edit | edit	| id=1のマイクロポストを編集するページ |
| PATCH	| /microposts/1 |	update | id=1のマイクロポストを更新するアクション |
| DELETE	| /microposts/1	| destroy	| id1のマイクロポストを削除する |
 #### MIcropostsをマイクロにする
 ```lenght```を用いる
#### 継承の階層
UserモデルとMicropostモデルの継承階層
##### Record
![Imige of keishoukaiso](https://railstutorial.jp/chapters/6.0/images/figures/demo_model_inheritance_4th_ed.png)<br>
##### Controllar
![Imige of keishoukaiso2](https://railstutorial.jp/chapters/6.0/images/figures/demo_controller_inheritance.png)
 
