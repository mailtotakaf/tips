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