# Image Building Best Practices

### Security Scanning

セキュリティのいい練習は、イメージを構築した時に、```docker scan```コマンドを使ってセキュリティの脆弱性をスキャンすることだ。
Dockerは、Snykと提携して脆弱性スキャンサービスを提供している。
```docker scan```での出力は、脆弱性の種類、詳細を調べるためのURL、そしてライブラリのどのバージョンのどのバージョンで
脆弱性が修正されるかという情報が含まれている。
<br>
新しく作成したイメージをコマンドライン上でスキャンするだけでなく、新しくプッシュされたイメージが自動的にスキャンされるようにDocker Hubを設定することができる。
その結果はDocker Hub、Docker Desktopの両方で確認できる。

### Image Layering

```dicker image history```コマンドを用いてイメージに含まれる各レイヤーに作成に使用されたコマンドを確認できる。
```
IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
5a96d632dcd2   4 days ago    CMD ["node" "src/index.js"]                     0B        buildkit.dockerfile.v0
<missing>      4 days ago    RUN /bin/sh -c yarn install --production # b…   83.1MB    buildkit.dockerfile.v0
<missing>      4 days ago    COPY . . # buildkit                             17.4MB    buildkit.dockerfile.v0
<missing>      4 days ago    WORKDIR /app                                    0B        buildkit.dockerfile.v0
<missing>      3 weeks ago   /bin/sh -c #(nop)  CMD ["node"]                 0B        
<missing>      3 weeks ago   /bin/sh -c #(nop)  ENTRYPOINT ["docker-entry…   0B        
<missing>      3 weeks ago   /bin/sh -c #(nop) COPY file:4d192565a7220e13…   388B      
<missing>      3 weeks ago   /bin/sh -c apk add --no-cache --virtual .bui…   7.78MB    
<missing>      3 weeks ago   /bin/sh -c #(nop)  ENV YARN_VERSION=1.22.19     0B        
<missing>      3 weeks ago   /bin/sh -c addgroup -g 1000 node     && addu…   154MB     
<missing>      3 weeks ago   /bin/sh -c #(nop)  ENV NODE_VERSION=18.12.1     0B        
<missing>      3 weeks ago   /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B        
<missing>      3 weeks ago   /bin/sh -c #(nop) ADD file:ceeb6e8632fafc657…   5.54MB    

```
この画面ではイメージのベースが下部、最新のレーヤーが上部に表示される。これを使うことで、素早くそれぞれのレイヤーを確認し、サイズの大きいイメージを突き止めることができる。
```--no-trunc```フラグを用いるとフル出力できる。

### Layer Cashing

今、レイヤーを見たが、これはコンテナイメージののビルド時間を減らすことに関してとても重要な話だ。<br>

レイヤーを変更したら、下部のすべてのレイヤーを「再生する必要がある。

Dockerfileのそれぞれのコマンドがイメージの新しいレイヤーになっていることがわかる。イメージに変更を加えた時、yarnの依存関係が再インストールされされている。
これを修正する方法はあるのか。<br>

これを趨勢するには、Dockerfileを再構築して依存関係をキャッシュする必要がある。Nodeベースのアプリケーションでは、```package.json```ファイルで依存関係が定義されている。だから、このファイルだけを最初にコピーしておけば、依存関係がインストールされてから他のファイルがコピーされる。なので```package.json```を変更したときのみyarnの依存関係が再生成されるようになる。<br>

キャッシュとは（https://e-words.jp/w/%E3%82%AD%E3%83%A3%E3%83%83%E3%82%B7%E3%83%A5.html）<br>

```.dockerignore```ファイルを用いると、イメージに必要なファイルだけを選択してコピーすることができる。
この場合、```node_modules```フォルダは2回目の```COPY```で除外される。
さもなければ```RUN```のコマンドで生成されたファイルで上書きされる。

### Mulit-Stage Build

複数のステージを使用してイメージを作成するのに役立つパワフルのツールだ。以下のような利点がある。
・ランタイムの



