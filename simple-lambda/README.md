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
