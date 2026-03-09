# タスク: 開発環境・前提条件のセットアップ手順整備

> **使い方**: このファイルは `docs/tasks/` 配下のタスク管理ファイルです。
> 進捗メモ・振り返りを随時更新してください。

---

## 概要

AgentCore ファミリーを使ったエージェント開発に必要な AWS 環境・ローカル開発環境のセットアップ手順をまとめ、誰でも再現できる状態にする。

## 完了条件

- [x] 必要な AWS サービス・権限（IAM ポリシー）が特定されている
- [x] AWS CLI / AWS SDK（Python boto3 等）のセットアップ手順が文書化されている
- [x] AgentCore 関連リソース作成に必要な IAM ポリシーのサンプルが用意されている
- [x] ローカル開発環境（Python バージョン、依存ライブラリ等）の要件が明記されている
- [x] セットアップ手順が `docs/setup/` 配下にまとめられている

## 仕様・制約

- AWS マネジメントコンソールではなく、CLI / SDK / IaC（CDK/CloudFormation）での操作を優先する
- 最小権限の原則に従い、必要最小限の IAM 権限のみ付与する
- セットアップ手順は Task 3 の調査結果に基づいて作成する（依存タスク: Task 3）

## 前提タスク

- Task 3: AgentCore ファミリー全体概要調査（完了後に着手推奨）

## セットアップ対象（候補）

### AWS 環境
- AWS アカウントの準備（Bedrock へのアクセス有効化）
- Amazon Bedrock モデルアクセスの有効化
- AgentCore 関連リソース作成に必要な IAM ロール・ポリシー

### ローカル開発環境
- Python 3.10 以上（boto3, bedrock-agentcore-sdk 等）
- AWS CLI v2
- (任意) AWS CDK v2（インフラをコードで管理する場合）

### 参照リソース（候補）
- [AWS CLI インストールガイド](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
- [boto3 ドキュメント](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)
- [Amazon Bedrock へのアクセス管理](https://docs.aws.amazon.com/bedrock/latest/userguide/security-iam.html)

## 進捗メモ

### 計画の言語化

Task 3 の調査後に必要な権限・依存関係が明らかになるため、本タスクは Task 3 完了後に詳細化する。
まずは一般的な Bedrock + AgentCore の前提条件を洗い出す。

### 詳細化

Task 3 の調査結果（agentcore-runtime.md）をもとに、必要なツール・権限を特定:

- **Python 3.10+** が必須（SDK・Starter Toolkit の要件）
- **AWS CLI v2** が必須（リソース操作・認証情報管理）
- **Docker** はオプション（CodeBuild でのクラウドビルドがあるため）
- **IAM ポリシーは 2 種類**が必要
  - 開発者用ポリシー: CLI によるリソース作成・デプロイ操作
  - エージェント実行ロール: bedrock-agentcore.amazonaws.com が AssumeRole する
- 信頼ポリシーに Condition を付けて "Confused Deputy" 攻撃を防ぐことが重要

### 実行

以下のドキュメントを `docs/setup/` 配下に作成した:

- `docs/setup/index.md` — ガイド全体の概要・前提条件チェックリスト・対応リージョン一覧
- `docs/setup/aws-environment.md` — AWS アカウント準備・Bedrock モデルアクセス有効化手順
- `docs/setup/iam-policies.md` — 開発者用ポリシーと実行ロールの JSON サンプル・CLI 手順
- `docs/setup/local-development.md` — Python・AWS CLI・Starter Toolkit のセットアップ手順・トラブルシューティング

### 検証

- 完了条件 5 項目をすべて満たす内容を作成した
- 最小権限の原則に従い、IAM ポリシーはリソース ARN を適切にスコープした
- Starter Toolkit による自動ロール作成のオプションも記載し、開発者が選択できるようにした
- Mermaid ダイアグラムを使って IAM ロールの関係を視覚化した

## 振り返り

### うまくいったこと

- Task 3 の調査結果（agentcore-runtime.md）を活用することで、必要な権限・ツールを効率的に特定できた
- IAM ポリシーを開発者用と実行ロール用に分けて整理したことで、役割が明確になった
- Starter Toolkit を使った自動ロール作成と手動作成の両方を記載し、柔軟性を確保した

### 改善できること

- 実際の AWS 環境での動作確認（ハンズオン）は未実施のため、手順の細部に誤りがある可能性がある
- Windows 環境向けの手順が一部不足している

### 学んだこと

- AgentCore Runtime の実行ロールには信頼ポリシーに Condition（SourceAccount, SourceArn）を付けることで "Confused Deputy" 問題を防げる
- `agentcore deploy` コマンドが AWS CodeBuild を使ってクラウド上でコンテナをビルドするため、ローカル Docker は必須ではない

## 参照リンク

- [タスクサイクル詳細手順](../workflow/task-cycle.md)
- [masterplan.md](../masterplan.md)
- [Task 3: AgentCore ファミリー全体概要調査](task-03-agentcore-family-overview.md)
- [Task 5: 基本エージェントの実装](task-05-basic-agent-implementation.md)
