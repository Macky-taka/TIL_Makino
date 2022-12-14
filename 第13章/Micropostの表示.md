# Micropostを表示する

ユーザーのshowページで直接マイクロポストを表示させる。ユーザープロフィールにマイクロポストを表示させるため、最初に極めてシンプルなERbテンプレートを作成する。
サンプルデータ生成タスクにマイクロポストのサンプルを追加して、画面にサンプルデータが表示されるようにする。

### Micropostの描画
```
<ol class="microposts">
  <%= render @microposts %>
</ol>
```
で表示をする。順序付きリストのolタグを使用する。
各マイクロポストに対してCSSのidを割り振っているが、一般的に良いとされる慣習で、例えば将来、JavaScriptを使って各マイクロポストを操作したくなったときなどに役立つ。
次は、一度にすべてのマイクロポストが表示されてしまう潜在的問題にページネーションで対応する。
```
<%= will_paginate @microposts %>
```
```
<%= will_paginate %>
```
前回は下で動作していたが、これは```will_paginate```が、Usersコントローラのコンテキストにおいて、@usersインスタンス変数が存在していることを前提としているからだ。
このインスタンス変数は、10.3.3でも述べたようにActiveRecord::Relationクラスのインスタンスである。
今回の場合はUsersコントローラのコンテキストからマイクロポストをページネーションしたいため（つまりコンテキストが異なるため）、明示的に@microposts変数をwill_paginateに渡す必要がある。
最後の課題はマイクロポストの投稿数を表示することだが、これはcountメソッドを使うことで解決する。countメソッドではデータベース上のマイクロポストを全部読みだしてから結果の配列に対してlengthを呼ぶ、といった無駄な処理はしていないという点
が大切だ。

### Micropostのサンプル
takeで呼び出しorderを経由することで最初の方から呼び出す。
Faker gemにLorem.sentenceという便利なメソッドを用いる。

### プロフィール画面のmicropostをテスト
プロフィール画面で表示されるマイクロポストに対して、統合テストを書く
ロフィール画面にアクセスした後に、ページタイトルとユーザー名、Gravatar、マイクロポストの投稿数、そしてページ分割されたマイクロポスト、といった順でテストする。
```response.body```にはそのページの完全なHTMLが含まれて
