# scafoldを用いたユーザーリソースの確認
###### ユーザーページの探索
/users	index	すべてのユーザーを一覧するページ <br>
/users/1	show	id=1のユーザーを表示するページ <br>
/users/new	new	新規ユーザーを作成するページ  <br>
/users/1/edit	edit	id=1のユーザーを編集するページ  <br>

###### MVCでの挙動
![Imige of MVC](https://railstutorial.jp/chapters/6.0/images/figures/mvc_detailed.png)<br>
1.ブラウザから「/users」というURLのリクエストをRailsサーバーに送信する。<br>
2.「/users」リクエストは、Railsのルーティング機構（ルーター）によってUsersコントローラ内のindexアクションに割り当てられる。<br>
3.indexアクションが実行され、そこからUserモデルに、「すべてのユーザーを取り出せ」（User.all）と問い合わせる。<br>
4.Userモデルは問い合わせを受け、すべてのユーザーをデータベースから取り出す。<br>
5.データベースから取り出したユーザーの一覧をUserモデルからコントローラに返す。<br>
6.Usersコントローラは、ユーザーの一覧を@users変数（@はRubyのインスタンス変数を表す）に保存し、indexビューに渡す。<br>
7.indexビューが起動し、ERB（Embedded RuBy: ビューのHTMLに埋め込まれているRubyコード）を実行して HTMLを生成（レンダリング）する。<br>
8.コントローラは、ビューで生成されたHTMLを受け取り、ブラウザに返す 。<br>
