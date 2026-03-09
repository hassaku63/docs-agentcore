# タスク: AgentCore Runtime を使った基本エージェントの実装

> **使い方**: このファイルは `docs/tasks/` 配下のタスク管理ファイルです。
> 進捗メモ・振り返りを随時更新してください。

---

## 概要

AgentCore Runtime を使って、最小構成のエージェントを実装・デプロイし、実際に稼働させることで、エージェント開発の基本的な流れを習得する。

## 完了条件

- [ ] AgentCore Runtime にデプロイ可能なシンプルなエージェントを実装している
- [ ] エージェントが AgentCore Runtime 上で正常に起動・動作することを確認している
- [ ] エンドポイント経由でエージェントへのリクエスト送信と応答確認ができている
- [ ] 実装手順・コードが `docs/examples/` または `src/` 配下に保存されている
- [ ] 実装・動作確認のポイントがこのタスクファイルに記録されている

## 仕様・制約

- 最初は Hello World 相当のシンプルな実装から始め、段階的に機能を追加する
- AWS 公式サンプル（aws-samples）を参考にしながら実装する
- Task 4 の環境セットアップが完了していることを前提とする（依存タスク: Task 4）

## 前提タスク

- Task 3: AgentCore ファミリー全体概要調査（完了後に着手推奨）
- Task 4: 開発環境・前提条件のセットアップ手順整備（完了必須）

## 実装方針（候補）

### エージェントフレームワーク
AgentCore Runtime は特定のエージェントフレームワークに依存しない。以下のいずれかを使用する:
- **Strands Agents**（AWS 公式 OSS エージェントフレームワーク）
- **LangGraph**（LangChain エコシステム）
- **boto3 の Bedrock API を直接使用**（フレームワーク非依存）

### 実装ステップ（目安）
1. エージェントのロジックを実装（ローカル動作確認）
2. AgentCore Runtime 用のエンドポイントハンドラーを追加
3. コンテナイメージのビルドと ECR へのプッシュ
4. AgentCore Runtime へのデプロイ
5. エンドポイント経由での動作確認

### 参照リソース（候補）
- [aws-samples/amazon-bedrock-agentcore-samples](https://github.com/aws-samples/amazon-bedrock-agentcore-samples)
- [AgentCore Runtime ドキュメント](https://docs.aws.amazon.com/bedrock/latest/userguide/agentcore-runtime.html)
- [Strands Agents GitHub](https://github.com/strands-agents/sdk-python)

## 進捗メモ

### 計画の言語化

AgentCore Runtime の基本的なデプロイフローを理解することが主目的。
まず aws-samples にあるサンプルを動かすことから始め、徐々に独自実装に切り替える。

### 詳細化

（Task 3, 4 完了後に具体的な実装方針を確定する）

### 実行

（実装中に記入）

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
- [Task 4: 開発環境セットアップ](task-04-development-environment-setup.md)
- [Task 6: Memory・Identity・Tools の統合](task-06-advanced-integration.md)
