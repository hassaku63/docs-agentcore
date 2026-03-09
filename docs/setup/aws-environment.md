# AWS 環境のセットアップ

> **更新日**: 2026-03-09

---

## 1. AWS アカウントの準備

### 必要なもの
- AWS アカウント（請求情報が設定済みであること）
- Amazon Bedrock が利用可能なリージョンへのアクセス

### リージョンの選択

AgentCore のすべてのコンポーネントを利用するには、対応リージョンを選択する必要があります。本ガイドでは **`us-east-1`（米国東部: バージニア北部）** を推奨リージョンとします。

```bash
# AWS CLI でデフォルトリージョンを確認する
aws configure get region

# または環境変数で指定する
export AWS_DEFAULT_REGION=us-east-1
```

---

## 2. Amazon Bedrock モデルアクセスの有効化

AgentCore Runtime でエージェントを動作させるには、使用するモデルのアクセスを事前に有効化する必要があります。

### コンソールでの手順

1. [AWS マネジメントコンソール](https://console.aws.amazon.com/) にサインイン
2. **Amazon Bedrock** サービスに移動（対象リージョンを選択）
3. 左ナビゲーションペインから **「モデルアクセス」** を選択
4. **「モデルアクセスを管理」** をクリック
5. 使用したいモデル（推奨: **Anthropic Claude Sonnet 4.0** または **Amazon Nova Pro**）を有効化
6. 利用規約に同意し、**「変更を保存」** をクリック

> **注意**: Anthropic などのサードパーティモデルは、初回利用時にユースケースフォームの入力が必要な場合があります。アクセス承認には最大数分かかることがあります。

### CLI での確認

```bash
# 利用可能なモデル一覧を確認する
aws bedrock list-foundation-models \
  --region us-east-1 \
  --query 'modelSummaries[?modelLifecycle.status==`ACTIVE`].[modelId,modelName]' \
  --output table

# 特定モデルのアクセス状態を確認する
aws bedrock get-foundation-model \
  --model-identifier anthropic.claude-sonnet-4-0 \
  --region us-east-1
```

### 推奨モデル

| モデル | モデル ID | 用途 |
|--------|-----------|------|
| Anthropic Claude Sonnet 4.5 | `anthropic.claude-sonnet-4-5:0` | 汎用エージェント |
| Amazon Nova Pro | `amazon.nova-pro-v1:0` | コスト効率重視 |
| Amazon Nova Lite | `amazon.nova-lite-v1:0` | 軽量タスク・テスト用 |

---

## 3. AWS アカウントの AgentCore 利用準備

### サービスクォータの確認

AgentCore を使用する前に、アカウントのサービスクォータを確認しておきます。

```bash
# AgentCore Runtime のサービスクォータを確認する
aws service-quotas list-service-quotas \
  --service-code bedrock-agentcore \
  --region us-east-1 \
  --output table
```

主なクォータ:
- 同時 Runtime セッション数: デフォルト最大 3,000（アカウント全体）

### ECR（Amazon Elastic Container Registry）の準備

AgentCore Runtime はコンテナイメージを ECR から取得します。Starter Toolkit を使うと自動的に ECR リポジトリが作成されますが、手動で確認・作成することもできます。

```bash
# ECR リポジトリの一覧を確認する
aws ecr describe-repositories --region us-east-1 --output table
```

---

## 参照リンク

- [Amazon Bedrock へのアクセス管理](https://docs.aws.amazon.com/bedrock/latest/userguide/security-iam.html)
- [Bedrock モデルアクセスの有効化](https://docs.aws.amazon.com/bedrock/latest/userguide/model-access.html)
- [AgentCore IAM 権限](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-permissions.html)
- [セットアップガイド目次](index.md)
