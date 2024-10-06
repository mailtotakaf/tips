# AWSにDockerイメージを作成し、ECR（Elastic Container Registry）にプッシュするための基本的な手順とDockerfileの例

## 1. Dockerfileの作成
プロジェクトディレクトリ内にDockerfileを作成します。以下は基本的なPythonアプリケーションのDockerfileの例です。

```dockerfile
# ベースイメージの指定
FROM public.ecr.aws/lambda/python:3.12

# Set the working directory inside the container
WORKDIR /var/task

# 必要なファイルをコピー
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# Copy the app.py file into the working directory
COPY app.py .

# エントリーポイントを指定
CMD ["app.lambda_handler"]  # 'app' はPythonファイル名、'lambda_handler' は関数名

```

## 2. Dockerイメージのビルド
ターミナルで次のコマンドを実行して、Dockerイメージをビルドします。
```bash
docker build -t my-aws-lambda-image .
```

## 3. AWS CLIの設定
ECRにプッシュする前に、AWS CLIを使用してECRにログインする必要があります。次のコマンドでログインします。
```bash
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
```
ここで、<region>はAWSリージョン（例: us-west-2）、<account-id>はAWSアカウントIDです。

## 4. ECRリポジトリの作成
ECRリポジトリを作成していない場合は、次のコマンドで作成できます。
```bash
aws ecr create-repository --repository-name my-aws-lambda-repo --region <region>
```

## 5. DockerイメージをECRにタグ付け
```bash
docker tag my-aws-lambda-image:latest <account-id>.dkr.ecr.<region>.amazonaws.com/my-aws-lambda-repo:latest
```

## 6. DockerイメージをECRにプッシュ
```bash
docker push <account-id>.dkr.ecr.<region>.amazonaws.com/my-aws-lambda-repo:latest
```

## 7. Lambdaをコンテナイメージから作成
AWSコンソール > Lambda > Create function > 

## 8. Lambda動作確認
AWSコンソールのテストでもOK。

```bash
aws lambda invoke --function-name <my-aws-lambda> outfile
```