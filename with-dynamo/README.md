# SAM(serverless-allication-model)のサンプル

## 環境構築

### 事前準備
- S3バケット作成
  - aws s3 mb s3://sam-sample
- role作成
  - roleはサービスが「lambda」でポリシーに「AmazonDynamoDBFullAccess」と「AWSLambdaDBExecutionRole」を選択する
- profileの設定(.aws/credentials)
  - 私はdefaultが開発用のアカウントなのでprofileなしで進めてます

### cli

1. CFnテンプレートのパッケージング

```
aws cloudformation package \
    --template-file template.yaml \
    --s3-bucket sam-sample \
    --output-template-file packaged-template.yaml \
```

2. 作成されたCFnをAWSにdeploy

```
aws cloudformation deploy \
    --template-file packaged-template.yaml \
    --stack-name sam-sample-stack \
    --capabilities CAPABILITY_IAM \
```

### 修正したら

zipして、packageし直して、またdeploy。ちょっと面倒

### Dynamoのテーブル名について

```
テーブル名は構築時(deploy時)に決定するので、プログラム作成時点では確定していない。
CloudFormation Stack 構築時に決まるリソースIDを扱うには、Lambda の環境変数（Environment > Variables）機能と CloudFormation の Ref 関数を用います。

環境変数に埋め込む値(ここではDynamoDBのテーブル名)は、Stack 構築時に決まります。
このような値を参照するには、CloudFormation の Ref 関数を用います。

```
