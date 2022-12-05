# Using Docker Compose

Docker Composeは、マルチコンテナプリの定義と共有しやすくするために開発されたものだ。
Composeを用いると、YAMLファイルを作成することでサービスを定義して、１つのコマンドで起動したり、停止したりすることができる。
Composeを使用する大きな利点は、ファイルにアプリケーションスタックを定義し、（バージョン管理されている）プロジェクトリポジトリのルートに、誰でも簡単にプロジェクトに
コントリビュートできるよにする。実際、GithubやGitLabにはそのようなプロジェクトが多数ある。

https://learn.microsoft.com/ja-jp/dynamics365/fin-ops-core/dev-itpro/dev-tools/application-stack-server-architecture


### Docker Composeのインストール
Docker Desktopのインストールで完了している。

### Compose Fileの作成
```
docker run -dp 3000:3000 `
  -w /app -v "$(pwd):/app" `
  --network todo-app `
  -e MYSQL_HOST=mysql `
  -e MYSQL_USER=root `
  -e MYSQL_PASSWORD=secret `
  -e MYSQL_DB=todos `
  node:12-alpine `
  sh -c "yarn install && yarn run dev"
```
最初に、コンテナのためのサービスエントリーとイメージを定義する。サービス名は任意のものを選べる。名前は自動的にネットワークエイリアスとして使用されるので、MySQL
サービスをを定義するのが楽になる。
あとはそれぞれキーを用いながら、移していく。

### MySQLサービスを定義する。

```
docker run -d \
  --network todo-app --network-alias mysql \
  -v todo-mysql-data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=todos \
  mysql:5.7
```
を移す。

### アプリケーションsタックを起動

```docker-compose.yml``が用意できたので起動する

###### ボリュームの削除について
デフォルトでは```docker-compose down```で停止してもnamedvolumeは削除されない。もし削除する際は、```--volumes```フラグを追加する必要がある。


### まとめ
docker Composeとそれによる複数サービスのアプリケーションの定義と共有がどれだけ簡単になるかを学んだ。また、適切なコンポーズの形式に沿ってコマンドを記載してコンポーズファイルを咲く英した。
Dockerfileには問題がある。イメージ構築に関するベストプラクティスを取り上げる。

