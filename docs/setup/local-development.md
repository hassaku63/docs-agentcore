# ローカル開発環境のセットアップ

> **更新日**: 2026-03-09

---

## 必要なソフトウェア

| ツール | バージョン | 用途 |
|--------|-----------|------|
| Python | 3.10 以上 | エージェント開発言語 |
| AWS CLI | v2.x | AWS リソース操作・認証情報管理 |
| Docker | 最新安定版 | コンテナイメージのビルド（ローカルテスト時） |
| bedrock-agentcore-starter-toolkit | 最新版 | agentcore CLI・SDK |
| boto3 | 最新版 | AWS SDK for Python |

---

## 1. Python のセットアップ

### バージョン確認

```bash
python3 --version
# Python 3.10.x 以上であることを確認
```

### 仮想環境の作成（推奨）

プロジェクトごとに仮想環境を作成することを推奨します。

```bash
# プロジェクトディレクトリで仮想環境を作成
python3 -m venv .venv

# 仮想環境を有効化
source .venv/bin/activate        # macOS / Linux
.venv\Scripts\activate           # Windows

# pip を最新化
pip install --upgrade pip
```

---

## 2. AWS CLI v2 のセットアップ

### インストール

公式インストーラーを使用します。

```bash
# macOS (Homebrew)
brew install awscli

# Linux (公式インストーラー)
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# バージョン確認
aws --version
# aws-cli/2.x.x 以上であることを確認
```

### 認証情報の設定

#### 方法 1: 通常の IAM ユーザー（アクセスキー）

```bash
aws configure
# AWS Access Key ID: <アクセスキー ID>
# AWS Secret Access Key: <シークレットアクセスキー>
# Default region name: us-east-1
# Default output format: json
```

#### 方法 2: IAM Identity Center（SSO）推奨

組織で IAM Identity Center（旧 AWS SSO）を使用している場合はこちらを推奨します。

```bash
# SSO プロファイルを設定
aws configure sso --profile agentcore-dev
# SSO start URL: <SSO ポータル URL>
# SSO Region: us-east-1
# SSO account ID: <AWS アカウント ID>
# SSO role name: <IAM Identity Center ロール名>

# ログインする
aws sso login --profile agentcore-dev

# 動作確認
aws sts get-caller-identity --profile agentcore-dev
```

#### 方法 3: IAM ロール（EC2 / Cloud9 / AWS 環境上での開発）

AWS 環境上で開発する場合は、インスタンスプロファイルまたはロールが自動的に使用されます。追加の設定は不要です。

### 動作確認

```bash
# 現在の認証情報を確認する
aws sts get-caller-identity

# 期待される出力例:
# {
#     "UserId": "AIDAXXXXXXXXXXXXXXXXX",
#     "Account": "123456789012",
#     "Arn": "arn:aws:iam::123456789012:user/developer"
# }
```

---

## 3. Python パッケージのインストール

仮想環境を有効化した状態でインストールします。

```bash
# コア SDK と Starter Toolkit をインストール
pip install bedrock-agentcore bedrock-agentcore-starter-toolkit

# エージェントフレームワーク（任意）
# Strands Agents を使う場合
pip install strands-agents

# LangGraph を使う場合
pip install langgraph

# boto3（Starter Toolkit に含まれているが、明示的にインストール推奨）
pip install boto3
```

### インストール確認

```bash
# agentcore CLI が使えることを確認
agentcore --help

# 期待される出力（抜粋）:
# Usage: agentcore [OPTIONS] COMMAND [ARGS]...
#
#   Amazon Bedrock AgentCore CLI
#
# Commands:
#   create    Create a new AgentCore agent project
#   deploy    Deploy agent to AgentCore Runtime
#   dev       Start local development server
#   invoke    Invoke an agent
```

---

## 4. Docker のセットアップ（オプション）

ローカルでコンテナイメージをビルドする場合に必要です。`agentcore deploy` コマンドは AWS CodeBuild を使ってクラウド上でビルドするため、ローカル Docker は必須ではありません。

```bash
# Docker バージョン確認
docker --version

# Docker が起動していることを確認
docker ps
```

---

## 5. プロジェクトの作成と動作確認

### 新規プロジェクトの作成

```bash
# 新しいエージェントプロジェクトを作成する
agentcore create

# 対話形式でプロジェクト設定を行う
# - フレームワークの選択（Strands Agents / LangGraph / カスタム）
# - プロジェクト名の入力
# - 使用するモデルの選択
```

### 作成されるファイル構成

```
my-agent/
├── agent.py                    # エージェントのメインコード
├── requirements.txt            # 依存ライブラリ
├── .bedrock_agentcore.yaml     # AgentCore 設定ファイル
└── Dockerfile                  # コンテナイメージ定義
```

### ローカルでのテスト実行

```bash
cd my-agent

# ローカル開発サーバーを起動する
agentcore dev

# 別のターミナルでエージェントを呼び出す
agentcore invoke --dev "Hello, AgentCore!"
```

### クラウドへのデプロイ

```bash
# AgentCore Runtime にデプロイする
agentcore deploy

# デプロイ完了後、エンドポイントを呼び出す
agentcore invoke "Hello, AgentCore!"
```

---

## 6. 環境変数の設定（任意）

`.env` ファイルまたはシェルの設定ファイルに以下の環境変数を設定しておくと便利です。

```bash
# AWS 設定
export AWS_DEFAULT_REGION=us-east-1
export AWS_PROFILE=agentcore-dev  # SSO プロファイルを使う場合

# AgentCore 設定（任意）
export BEDROCK_AGENTCORE_REGION=us-east-1
```

---

## トラブルシューティング

### `agentcore` コマンドが見つからない

```bash
# 仮想環境が有効化されているか確認
which python
# /path/to/.venv/bin/python と表示されるはず

# 再インストール
pip install --force-reinstall bedrock-agentcore-starter-toolkit
```

### AWS 認証エラー

```bash
# 認証情報が正しく設定されているか確認
aws sts get-caller-identity

# SSO を使用している場合、ログインの有効期限切れの可能性
aws sso login --profile agentcore-dev
```

### Bedrock モデルアクセスエラー

```bash
# モデルアクセスが有効化されているか確認
aws bedrock list-foundation-models \
  --region us-east-1 \
  --query 'modelSummaries[?contains(modelId, `claude`)].[modelId, modelLifecycle.status]' \
  --output table
```

---

## 参照リンク

- [AWS CLI インストールガイド](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [bedrock-agentcore-starter-toolkit クイックスタート](https://aws.github.io/bedrock-agentcore-starter-toolkit/user-guide/runtime/quickstart.html)
- [boto3 ドキュメント](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)
- [Strands Agents ドキュメント](https://strandsagents.com/docs/)
- [セットアップガイド目次](index.md)
