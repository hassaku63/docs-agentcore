# タスク: 開発環境・前提条件のセットアップ手順整備

> **使い方**: このファイルは `docs/tasks/` 配下のタスク管理ファイルです。
> 進捗メモ・振り返りを随時更新してください。

---

## 概要

AgentCore ファミリーを使ったエージェント開発に必要な AWS 環境・ローカル開発環境のセットアップ手順をまとめ、誰でも再現できる状態にする。

## 完了条件

- [ ] 必要な AWS サービス・権限（IAM ポリシー）が特定されている
- [ ] AWS CLI / AWS SDK（Python boto3 等）のセットアップ手順が文書化されている
- [ ] AgentCore 関連リソース作成に必要な IAM ポリシーのサンプルが用意されている
- [ ] ローカル開発環境（Python バージョン、依存ライブラリ等）の要件が明記されている
- [ ] セットアップ手順が `docs/setup/` 配下にまとめられている

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

（Task 3 完了後に記入）

### 実行

（セットアップ手順作成中に記入）

### 検証

（手順を実際に実行して確認時に記入）

## 振り返り

### うまくいったこと

-

### 改善できること

-

### 学んだこと

-

## 参照リンク

- [タスクサイクル詳細手順](../workflow/task-cycle.md)
- [masterplan.md](../masterplan.md)
- [Task 3: AgentCore ファミリー全体概要調査](task-03-agentcore-family-overview.md)
- [Task 5: 基本エージェントの実装](task-05-basic-agent-implementation.md)
