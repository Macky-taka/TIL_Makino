# Relationship model
ユーザーをフォローする機能を実装する第一歩は、データモデルを構成することである。


### データモデルの問題および解決策

ユーザーをフォローするデータモデル構成のための第一歩として典型的な場合を検討する。あるユーザーが、別のユーザーをフォローしているところを考える。
片方からはフォーロしていて、逆はフォローされている。Railsにおけるデフォルトの複数形の慣習に従えば、あるユーザーをフォローしているすべてのユーザーの集合はfollowersとなり、```user.followers```はそれらのユーザーの配列を表すことになる。しかし、この名前付けは逆向きでは上手くいかない.
ここで、Twitterの慣習にならい、本チュートリアルではfollowingという呼称を用いる。
###### フォローしているユーザーの素朴な実装例
![Image of following](https://railstutorial.jp/chapters/6.0/images/figures/naive_user_has_many_following.png)

このデータモデルの問題点は、非常に無駄が多い。この問題の根本は、必要な抽象化を行なっていないことだ。
あるユーザーが別のユーザーをフォローするとき、何が作成されるか。 あるユーザーが別のユーザーをフォロー解除するとき、何が削除されるか。を考えるとろ2人のユーザーの「関係（リレーションシップ）である。よって、1人のユーザーは1対多の関係を持つことができ、さらにユーザーはリレーションシップを経由して多くのfollowing（またはfollowers）と関係を持つことができる。<br>
また、フォロー関係では左右非対称の性質がある。これを見分けるために、能動的関係（Active Relationship）と受動的関係（Passive Relationship）と呼称する。
まずは、フォローしているユーザーを生成するために、能動的関係に焦点を当てる。
###### 能動的関係をとおしてフォローしているユーザーを取得する模式図
![Image of active relationship](https://railstutorial.jp/chapters/6.0/images/figures/user_has_many_following_3rd_edition.png)


複合キーインデックスという行もあることに注意。```follower_id```と```followed_id```の組み合わせが必ずユニークであることを保証する仕組みだ。
同じユーザーが2回以上フォローするのを防ぐ。<br>

```microposts```テーブルには```user_id```属性があるので、これを辿って対応する所有者（ユーザー）を特定する
データベースの2つのテーブルを繋ぐとき、このようなidは外部キー（foreign key）と呼ぶ。すなわち、Userモデルに繋げる外部キーが、Micropostモデルのuser_id属性ということだ。この外部キーの名前を使って、Railsは関連付けの推測をしている。
ユーザーと能動的関係の関連付けによって使えるようになったメソッドのまとめは以下
|メソッド|	用途|
|-----|------|
|```active_relationship.follower```|	フォロワーを返します|
|```active_relationship.followed```|	フォローしているユーザーを返します|
|```user.active_relationships.create(followed_id: other_user.id)```| userと紐付けて能動的関係を作成/登録する|
|```user.active_relationships.create!(followed_id: other_user.id)```|	userを紐付けて能動的関係を作成/登録する（失敗時にエラーを出力）|
|```user.active_relationships.build(followed_id: other_user.id)```|	userと紐付けた新しいRelationshipオブジェクトを返す|

### Relationshipのバリデーション
Relationshipモデルの検証を追加して完全なものにする。

### フォローしているユーザー
