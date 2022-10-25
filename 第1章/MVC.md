# MVC
###### ![Image of MVC](https://railstutorial.jp/chapters/6.0/images/figures/mvc_schematic.png)
ブラウザがRailsアプリと通信する際、一般的にWebサーバーにリクエストを送信し、これはリクエストを処理する役割を担っているRailsのcontrollerに渡されます。<br>
controllerは、viewを生成してHTMLをブラウザに送り返します。<br>
動的なサイトでは、一般にcontrollerは（ユーザーなどの）サイトの要素を表しており、データベースとの通信を担当しているRubyのオブジェクトであるmodelと対話します。<br>
moelを呼び出した後、controllerは、viewを描画し、完成したWebページをHTMLとしてブラウザに返します。
