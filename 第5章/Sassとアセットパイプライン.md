# Sassとアセットパイプライン
## アセットパイプライン
CSS、JavaScript、画像などの静的コンテンツの生産性と管理を大幅に強化する。アセットパイプラインは　webpackやyarnのどちらでもうまく動く。
#### アセットディテクトり
```app/assets```: 現在のアプリケーション固有のアセット<br>
```lib/assets```: あなたの開発チームによって作成されたライブラリ用のアセット<br>
```vendor/assets```: サードパーティのアセット（デフォルトでは存在しない）<br>
#### マニフェストファイル
的ファイル（アセット）を上記の場所へそれぞれ配置すれば、マニフェストファイルを使って、それらをどのように1つのファイルにまとめるのかをRailsに指示することができる。実際にアセットをまとめる処理を行うのはSprocketsというgemである。マニフェストファイルはCSSとJavaScriptには適用されますが、画像ファイルには適用されない。
``` *= require_tree .```: ```app/assets/stylesheets```ディレクトリ（サブディレクトリを含む）中のすべてのCSSファイルが、アプリケーションCSSに含まれるようにする。<br>
```*= require_self``` : CSSの読み込みシーケンスの中で、```application.css```自身もその対象に含める。
#### プロフェッサエンジン
必要なアセットをディレクトリに配置してまとめた後、Railsはさまざまなプリプロセッサエンジンを介して実行し、ブラウザに配信できるよう結合する。拡張子をつなげることも可能。右から実行される。

## Sass
スタイルシートを記述するための言語であり、CSSに比べて多くの点が強化されている。
#### ネスト
スタイルシート内に共通のパターンがある場合は、要素をネストさせることができる
```.center {
  text-align: center;
}

.center h1 {
  margin-bottom: 10px;
}
```
```
.center {
  text-align: center;
  h1 {
    margin-bottom: 10px;
  }
}
```
```h1```が```.center```を継承

```
#logo {
  float: left;
  margin-right: 10px;
  font-size: 1.7em;
  color: #fff;
  text-transform: uppercase;
  letter-spacing: -1px;
  padding-top: 9px;
  font-weight: bold;
}

#logo:hover {
  color: #fff;
  text-decoration: none;
}
```
```#logo {
  float: left;
  margin-right: 10px;
  font-size: 1.7em;
  color: #fff;
  text-transform: uppercase;
  letter-spacing: -1px;
  padding-top: 9px;
  font-weight: bold;
  &:hover {
    color: #fff;
    text-decoration: none;
  }
}
```
```&```を用いて実現可能
#### 変数
冗長なコードを削除し、より自由な表現を可能にするために、変数が定義できる。
```h2 {
  .
  .
  .
  color: #777;
}
.
.
.
footer {
  .
  .
  .
  color: #777;
}
```
について```$light-gray: #777;```を定義し、
```$light-gray: #777;
.
.
.
h2 {
  .
  .
  .
  color: $light-gray;
}
.
.
.
footer {
  .
  .
  .
  color: $light-gray;
}
```
とすることができる
