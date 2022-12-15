# node.jp とnpmについて

### node.jp

page作成に使われるjavascriptをサーバー側で動作させるプラットフォーム。本来webブラウザ上で動作するのみでサーバー上では動作しない。Node.jpはjavascript実行環境である。
本来は大量の同時接続をさばけるネットワークアプリケーションの構築が目的のようだ.


### npm

nomはパッケージ管理のツール。
MacにおけるHomebrew的なやつ。Ruby　on Railsではyarnを行っていたためにjavascriptも動作していたと考えられる。<br>
```
直接パッケージをダウンロードして利用すればいいのではと考えるかもしれませんが、以下のような問題が発生するため、npmでの管理が必要となってきます。

パッケージの依存関係
パッケージxを使用するためにパッケージyが必要な場合、パッケージxはパッケージyに依存していると言えます。したがって、あるパッケージをインストールする際、その依存関係にあるパッケージをインストールし、さらにその依存関係にあるパッケージを……と複雑になります。
パッケージの競合関係
パッケージxによって使用しているパッケージyが、これからインストールするパッケージzでも必要な場合、パッケージyを更にインストールしていまい衝突してしまいます。これをパッケージの競合関係といい、あるパッケージをインストールする際、競合するパッケージやモジュールがないかを調べなくてはいけません。<br>
```

引用：https://zenn.dev/antez/articles/a9d9d12178b7b2

今後webpackあたりも見ておきたい

### インストール方法
今回はvoltaを用いた。nodebrewでも普通にできる。
volta: https://zenn.dev/taichifukumoto/articles/how-to-use-volta<br>
nodebrew: https://qiita.com/yukibe/items/bae442fa6314bd8f9d7a<br>
詳細は上記。今回詰まったところとして、パスをきちんと設定してnpmを探させる。また、インストールしたファイルの置き場所を作る必要も生じた(nodebrewのみ)



参考：https://www.kagoya.jp/howto/it-glossary/develop/nodejs/ <br>
https://eng-entrance.com/what-is-nodejs<br>
https://qiita.com/non_cal/items/a8fee0b7ad96e67713eb <br>
https://zenn.dev/antez/articles/a9d9d12178b7b2
