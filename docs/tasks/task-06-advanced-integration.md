# タスク: AgentCore Memory・Identity・Tools の統合

> **使い方**: このファイルは `docs/tasks/` 配下のタスク管理ファイルです。
> 進捗メモ・振り返りを随時更新してください。

---

## 概要

Task 5 で実装した基本エージェントに、AgentCore の Memory・Identity・Tools の各コンポーネントを統合し、本格的なエージェントとして機能を拡張する。

## 完了条件

- [ ] **Memory**: 会話履歴が AgentCore Memory に永続化され、セッション間で継続できることを確認している
- [ ] **Identity**: エージェントが AgentCore Identity を使って外部サービスへの認証ができることを確認している
- [ ] **Tools**: AgentCore Tools（または AgentCore Gateway 経由のツール）を呼び出せることを確認している
- [ ] 統合実装のコードが `docs/examples/` または `src/` 配下に保存されている
- [ ] 各コンポーネントの統合方法がこのタスクファイルまたは `docs/research/` に記録されている

## 仕様・制約

- Task 5 の基本エージェント実装が完了していることを前提とする（依存タスク: Task 5）
- 各コンポーネントの統合は独立して実施し、1 つずつ動作確認を行う
- AWS 公式の推奨パターンに従って実装する

## 前提タスク

- Task 3: AgentCore ファミリー全体概要調査（完了後に着手推奨）
- Task 5: AgentCore Runtime を使った基本エージェントの実装（完了必須）

## 統合対象コンポーネント

### AgentCore Memory
- **目的**: 会話履歴や長期記憶をエージェントに持たせる
- **主要概念**: Memory Store, Session, Namespace
- **参照**: [AgentCore Memory ドキュメント](https://docs.aws.amazon.com/bedrock/latest/userguide/agentcore-memory.html)

### AgentCore Identity
- **目的**: エージェントが外部サービス・API へ安全に認証するための仕組み
- **主要概念**: OAuth2 Token Exchange, Workload Identity
- **参照**: [AgentCore Identity ドキュメント](https://docs.aws.amazon.com/bedrock/latest/userguide/agentcore-identity.html)

### AgentCore Tools / Gateway
- **目的**: エージェントが外部ツール・API を呼び出せるようにする
- **主要概念**: Tool Definition, MCP (Model Context Protocol), Lambda ツール
- **参照**:
  - [AgentCore Tools ドキュメント](https://docs.aws.amazon.com/bedrock/latest/userguide/agentcore-tools.html)
  - [AgentCore Gateway ドキュメント](https://docs.aws.amazon.com/bedrock/latest/userguide/agentcore-gateway.html)

### 参照リソース（候補）
- [aws-samples/amazon-bedrock-agentcore-samples](https://github.com/aws-samples/amazon-bedrock-agentcore-samples)

## 進捗メモ

### 計画の言語化

Task 5 完了後、以下の順番で統合を進める:
1. Memory の統合（最も基本的で効果が分かりやすい）
2. Tools の統合（Lambda ツールまたは MCP サーバー）
3. Identity の統合（外部サービス連携が必要な場合）

### 詳細化

（Task 5 完了後に詳細化する）

### 実行

（統合実装中に記入）

### 検証

（動作確認時に記入）

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
- [Task 5: 基本エージェントの実装](task-05-basic-agent-implementation.md)
- [Task 7: 主要概念まとめ](task-07-knowledge-summary.md)
