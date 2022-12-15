# vue.jpについて

今回はタスク遂行の中で生じた疑問点を中心にまとめる。基本的なvueとはは書かない。

### 基本文法

##### ```v-bind```

略して```:```。要素の属性を動的に設定できる。
例
```
<template>
  <input v-bind:type="myInputType">
</template>
```
HTMLではv-bind:【属性名】="【JavaScript式】"で記述する。

参考：https://qiita.com/ota-meshi/items/ffa875daa9ebc9fc57c0


##### ```v-for```

配列、オブジェクト、数値に使う。今回はオビジェクトだと思う。選択のドロップダウンに用いた。上の学年がそれだったためパクった。
valueとkey属性をindexにしているが、vueの特性でdeleteしたときにずれないようにだと思うが、valueがつけているのがまだ理解できていない。

参考：https://qiita.com/JetNel0/items/1f618683e4acce5f9aa6

##### ```v-model```

双方向のデータバインディングを実現できる。つまり、ユーザーはinputを使用してデータを変更し、必要に応じて結果を同時に確認できる。

```
<input v-model="message" placeholder="メッセージを入力">
<p>メッセージ: {{ message }}</p>

let app = new Vue({
	el: '#app',
	data: {
		message: ''
	}
});
```

参考：https://qiita.com/_masa_u/items/7a940f1aea8be4eef4fe

##### ```v-slot```

slotとは親となるコンポーネント側から、子のコンポーネントのテンプレートの一部を差し込む機能である。
今回ならerrorとかを差し込んでいた。。。はず。

参考：https://future-architect.github.io/articles/20200428/

基本構文全体はhttps://qiita.com/_masa_u/items/7a940f1aea8be4eef4fe

### ファイルの読み込み

export defaultと　import from


https://prograshi.com/language/vue-js/how-to-use-export-default-in-vue-and-js/

その他参考：
https://qiita.com/mimoe/items/b2ebfc1b38e5a905d19b

