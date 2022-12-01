# Sharing our Application

 Docker imageを共有するのにdocker レポジトリを利用する必要がある。Docker Hubを用いる。
 
 
 ### Pushing our image
 ```
  docker push docker/getting-started
 ```
 では失敗する。それはdocker/getting-startedという名前のイメージを探すが、REPOSITORYを確認してもない。
 そのためにtagでイメージに別名を付与する。
 
 ### Running our Image on a new Insatance
 
 プッシュしたイメージを新規の環境で起動させる。
 
 Play with docker を用いる。
 CIパイプラインと同じで、パイプラインがイメージを作成してレジストリにプッシュすると、本番環境では最新版のイメージを使用できる。
 次は再起動後にデータを保持するようにする。
