# Using Bind Mounts

バインドマウントを用いるとホスト上の正確な　Mountpointをコントロールできる。
データの永続化にも使用できるが、追加のデータをコンテナに提供するのによく使われる。
アプリを開発するとき、バインドマウントでソースコードをコンテナに接続して、コードを変更したり、応答したり、変更したりできる、<br>

Nodeで作られたアプリの場合、ファイルの変更を監視してアプリケーションを再起動するのにはnodemonが最適だ。
同じようなツールは、ほとんどの言語とフレームワークに存在する。

### ボリュームタイプの比較

|	   |Named Volumes|	Bind Mounts|
|---|----|-------|
|Host Location	|Docker chooses	|You control|
|Mount Example (using -v)	|my-volume:/usr/local/data	|/path/to/data:/usr/local/data|
|Populates new volume with container contents	|Yes	|No|
|Supports Volume Drivers	|Yes	|No|

###  DevMode　Containerを起動

開発段階で使えるコンテナを起動する。以下を行う。
・ソースコードをコンテナにマウントする
・"dev" dependenciesを含む全ての依存関係をインストールする
 ・nodemonを起動してファイルの変更を監視する。
 
 ```
 docker run -dp 3000:3000 \
    -w /app -v "$(pwd):/app" \
    node:12-alpine \
    sh -c "yarn install && yarn run dev"
 ```
 ```-dp  3000:3000  ```：デタッチモードで起動し、ポートマッピングを作成。
 ```-w /app``` :　「ワーキングディレクトリ」か、コマンドが実行されるカレントディレクトリを指定。
 ```-v "$(pwd):/app"```:　コンテナのホストから```/app```ディレクトリにカレントディレクトリをバインドマウントする。
 ```node:12-alpine```: 使用するイメージ
 ```sh -c "yarn install && yarn run dev" ```: ```sh```でシェルを起動し、後ろ２つを実行。
 
バインドマウントを使用することはローカル開発において一般的だ。その利点は、開発マシンにビルドツールや環境がインストールされている必要がないところだ。
```docker run```で開発環境はプルされ、準備が完了する。

### 要約

製品化に備えて、データベースをSQLiteより拡張性の高いものに移行する必要がある。
単純に考えると、リレーショナルデータベースはそのままにMySQLを使用すべきだ。そこでMySQLについて学ぶ。


 
