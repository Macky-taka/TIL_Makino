# Localstack

開発環境において無料でAWSのアカウント登録なしでAWSのサービスを擬似的に使用できるモックフレームワークでpipやdockerを用いて簡単に環境構築が可能。
今回のモチベーションは、開発者が少ない場合は、ローカル環境から直接AWSのサービスを使うことも可能ですが、数十名以上の規模になってくるとAWS利用料が重くなることが挙げられるのではと推測。あとは自分の立場。

https://developer.mamezou-tech.com/containers/k8s/tutorial/app/localstack/ (今回はdocker)



サービスは以下
|無料	|有料|
|---|----|
|ACM<br>API Gateway<br>CloudFormation<br>CloudWatch<br>CloudWatchLogs<br>DynamoDB<br>DynamoDB Streams<br>EC2<br>Elasticsearch Service<br>EventBridge<br>Firehose<br>IAM<br>Kinesis<br>KMS<br>Lambda<br>Redshift<br>Route53<br>S3<br>SecretsManager<br>SES<br>StepFunctions<br><br>STS|Amplify<br>AppConfig<br>Application AutoScaling<br>AppSync<br>Athena<br>Backup<br>Batch<br>CloudFront<br>CloudTrail<br>CodeCommit<br>Cognito Identity<br>ECR<br>ECS<br>EKS<br>ElastiCache<br>EMR<br>Glue<br>IoT (including IoT Analytics, IoT Data)<br>Managed Streaming for Kafka (MSK)<br>Neptune DB<br>QLDB<br>RDS / Aurora Serverless<br>SageMaker<br>Timestream<br>Transfer<br>XRay<br>|

### docker-compose.yml

```ports```は4566。<br>
```SERVICES```でLocalStackで起動するサービスは指定。<br>
```DATA_DIR```はデータを保存するディレクトリの指定。


### コンテナ起動
今回はコンテナが起動できず下記のログを出力した。
```
2022-12-16 15:13:12   It seems you are mounting the LocalStack volume into /tmp/localstack.
2022-12-16 15:13:12   This will break the LocalStack container! Please update your volume mount
2022-12-16 15:13:12   destination to /var/lib/localstack.
2022-12-16 15:13:12   You can suppress this error by setting LEGACY_DIRECTORIES=1.
2022-12-16 15:13:12 
2022-12-16 15:13:12   See: https://github.com/localstack/localstack/issues/6398
```
ボリュームをマウントする場所を変更したら起動した。

##### ボリュームについて
Docker の管理下でストレージ領域を確保する。Linux なら ```/var/lib/docker/volumes/```以下。
名前付きボリュームと匿名ボリュームがあり、名前付きの場合は Docker ホスト内で名前解決できるのでアクセスしやすい。匿名ボリュームは適当にハッシュ値が振られる。<br>
dockerのストレージには他に2つある。一つはバインドマウントでホスト側のディレクトリをコンテナ内のディレクトリと共有する。もう一つはtmpfsでメモリ上にストレージ領域を確保する。名前の通り一時的な領域となる。用途としては、機密性の高い情報を一時的にマウントする場合などに使う。

https://qiita.com/aki_55p/items/63c47214cab7bcb027e0

https://qiita.com/engineer_minarai/items/200d3c4e23b800e433bb

https://it-blue-collar-dairy.com/localstack_on_docker-compose/

### aws cli

AWSをコマンド入力できるようにするインターフェースだ。
メリットとしてコマンドを組み合わせてスクリプトを作成し、一連の操作を自動化できる点にある。具体的には。<br>
管理操作手順をスクリプトにまとめると、担当の引継ぎが簡単になる。<br>
自動化によって業務効率化ができ、人的ミスを削減することができる。<br>
GUIに比べて作業スピードが上がる。<br>
AWSリソース間の自動連携ツールも作成することができる。<br>

### バケット作成

awscliを用いてs3のバケットを作成する。
ついでに```awslocal```を使えるようにした。エンドポイントの指定ががめんどくさかったため。


https://qiita.com/rkunihiro/items/646d9c39923cf8b23533

https://kenzoblog.vercel.app/posts/localstack

### AWS s3

Railsでも一度使用した。S3とはAWSのサービスのひとつで、「Amazon Simple Storage Service」の略称。
オブジェクトのファイル単位での出し入れが可能なので、その場に応じて自由な使い道が想定され、より柔軟なデータ保存が実行できるのが特徴となっている。
以下のようなS3のメリットが挙げられる。

柔軟に対応可能なストレージ機能<br>
耐久性と可用性<br>
AWSのS3ではすべてのオブジェクトに対して99.999999999% (9 x 11)の耐久性を実現<br>
低コストによる運用が可能<br>



公開鍵
https://it-trend.jp/encryption/article/64-0089

