# Microppost モデル

Micropostモデルを作成するところから始める。今回のマイクロポストモデルは完全にテストされ、デフォルトの順序を持ち、また親であるユーザーが破棄された場合には自動的に破棄されるようにする。
### 基本的なモデル
![Image of Micropost model](https://railstutorial.jp/chapters/6.0/images/figures/micropost_model.png)<br>

String型ではなくText型を使う。これは、ある程度の量のテキストを格納するときに使われる型だ。
投稿フォームにString用のテキストフィールドではなくてText用のテキストエリアを使うため、より自然な投稿フォームが実現できる。
また、例えばいつか国際化をするときに、言語に応じて投稿の長さを調節をすることも可能である。
そして、Text型を使っていても本番環境でパフォーマンスの差は出ない。<br>
Userモデルとの最大の違いはreferences型を利用している点で、自動的にインデックスと外部キー参照付きのuser_idカラムが追加され 、
UserとMicropostを関連付けする下準備をする。Userモデルのときと同じで、Micropostモデルのマイグレーションファイルでも```t.timestamps```
という行（マジックカラム）が自動的に生成される。

### Micropostのバリデーション
慣習的に正しくActive Recordの関連付けを実装する。まずはMicropostモデル単体を動くようにする。<br>
Micropostの初期テストはUserモデルの初期テストと似ており、まずはsetupのステップで、fixtureのサンプルユーザーと紐付けた新しいマイクロポストを作成する。
次に、作成したマイクロポストが有効かどうかをチェックする。
最後に、あらゆるマイクロポストはユーザーのidを持っているべきなので、user_idの存在性のバリデーションに対するテストも追加する。
次にuser_id属性と同様に、content属性も存在する必要があるため、contentに対するバリデーションも追加する。さらにマイクロポストが140文字より長くならないよう制限を加える。

 # User/Micropostの関連付け
 MicropostとそのUserは belongs_to（1対1）の関係性
 ![Image of reralation belong to](https://railstutorial.jp/chapters/6.0/images/figures/micropost_belongs_to_user.png)<br>
 UserとそのMicropostは has_many（1対多）の関係性
 ![Image of reralation has many](https://railstutorial.jp/chapters/6.0/images/figures/user_has_many_microposts.png)<br>
 user/micropost関連メソッドのまとめ
| メソッド|	用途|
|------|-----|
|micropost.user	|Micropostに紐付いたUserオブジェクトを返す|
|user.microposts	|Userのマイクロポストの集合をかえす|
|user.microposts.create(arg)	|userに紐付いたマイクロポストを作成する|
|user.microposts.create!(arg)	|userに紐付いたマイクロポストを作成する（失敗時に例外を発生）|
|user.microposts.build(arg)	|userに紐付いた新しいMicropostオブジェクトを返す|
|user.microposts.find_by(id: 1)	|userに紐付いていて、idが1であるマイクロポストを検索する|

@user.microposts.buildのようなコードを使うため、 UserモデルとMicropostモデルをそれぞれ更新して、関連付ける必要がある。



