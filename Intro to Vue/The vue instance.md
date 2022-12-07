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

インスタンスのデータ
