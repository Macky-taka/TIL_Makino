 # The vue instance
 
 ### 目標
 Vueを用いて、Webページへのデータの表示方法を見る。
 
 ```
 var app = new Vue({
    el: '#app',
    data: {
        product:"Socks"
    }
})
```
##### Vue　Instance
Vueのインスタンスはアプリケーションのルートだ。optionオブジェクトを渡すことで作成される。このオブジェクトには様々なオプションのプロパティがあり、データの蓄積や、アクションの実行といった機能を与える。

##### Plugging in to an Element

Vueインスタンスは選択した要素にプラグインされ、インスタンスとDOMのその部分との関係を形成する。言い換えれば、'#app'をインスタンスにプラグインされるel要素と設定することでapp, idを持つdivにVueを起動する・

##### Putting our data in its place

インスタンスのデータはそのインスタンスがプラグインされている要素の内側からアクセスできる。
##### Using an Expression

https://unpkg.com/vue@next

https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js

チュートリアルの推奨がvue3のくせに全てvue2で動くコードがサンプルで気づくのに時間がかかった。

https://blog.capilano-fw.com/?p=6393#Vue

{{}}が受け口でjavascriptが使える。

##### Introducing Reactivity

Vueがreactiveであり、インスタンスのデータは、データが、参照される全ての場所にリンクしている。

