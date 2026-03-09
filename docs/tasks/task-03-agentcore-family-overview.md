# タスク: AgentCore ファミリー全体概要調査

> **使い方**: このファイルは `docs/tasks/` 配下のタスク管理ファイルです。
> 進捗メモ・振り返りを随時更新してください。

---

## 概要

Amazon Bedrock AgentCore ファミリーを構成する各コンポーネント（Runtime / Memory / Identity / Tools / Gateway）の役割・機能・主要概念を調査し、エージェント開発に必要な知識基盤を整える。

## 完了条件

- [x] AgentCore ファミリーの全コンポーネントの役割と概要を把握している
- [x] 各コンポーネントの主要概念・用語が整理されている
- [x] AWS 公式ドキュメント・サンプルコード（aws-samples 等）の参照先が一覧化されている
- [x] コンポーネント間の関係・連携パターンが図または文章で説明されている
- [x] 調査結果が `docs/research/` 配下にまとめられている

## 仕様・制約

- 参照情報は AWS 公式発信を優先する（公式ドキュメント、AWS Blog、aws-samples）
- 情報の鮮度を重視し、最新の公式情報を使用する
- 非公式情報も実用性・鮮度が高い場合は積極的に収集する

## 参照リソース（調査前の候補）

### AWS 公式ドキュメント
- [Amazon Bedrock AgentCore（公式）](https://docs.aws.amazon.com/bedrock/latest/userguide/agentcore.html)
- [Amazon Bedrock AgentCore Runtime](https://docs.aws.amazon.com/bedrock/latest/userguide/agentcore-runtime.html)
- [Amazon Bedrock AgentCore Memory](https://docs.aws.amazon.com/bedrock/latest/userguide/agentcore-memory.html)
- [Amazon Bedrock AgentCore Identity](https://docs.aws.amazon.com/bedrock/latest/userguide/agentcore-identity.html)
- [Amazon Bedrock AgentCore Tools](https://docs.aws.amazon.com/bedrock/latest/userguide/agentcore-tools.html)
- [Amazon Bedrock AgentCore Gateway](https://docs.aws.amazon.com/bedrock/latest/userguide/agentcore-gateway.html)

### GitHub サンプルコード
- [aws-samples/amazon-bedrock-agentcore-samples](https://github.com/aws-samples/amazon-bedrock-agentcore-samples)

### AWS ブログ・発表資料
- [AWS Blog: Amazon Bedrock AgentCore カテゴリ](https://aws.amazon.com/blogs/machine-learning/)

## 進捗メモ

### 計画の言語化

AgentCore は 2025 年に発表された比較的新しいサービスファミリーであるため、公式ドキュメントと aws-samples を中心に調査する。各コンポーネントを独立して調査し、最終的にコンポーネント間の連携パターンをまとめる。

調査対象コンポーネント:
1. **AgentCore Runtime** — エージェントのデプロイ・実行基盤
2. **AgentCore Memory** — 会話履歴・長期記憶の永続化
3. **AgentCore Identity** — エージェントの認証・認可
4. **AgentCore Tools** — エージェントが呼び出せるツール
5. **AgentCore Gateway** — エージェントへの外部アクセス管理

### 詳細化

調査対象コンポーネントを確定し、AWS 公式ドキュメント・aws-samples・コミュニティ記事を情報源として調査を実施した。

情報源の優先順位:
1. AWS 公式ドキュメント（docs.aws.amazon.com/bedrock-agentcore/）
2. aws-samples / awslabs GitHub リポジトリ（サンプルコード・チュートリアル）
3. AWS 公式ブログ（aws.amazon.com/blogs/machine-learning/）
4. コミュニティ記事（InfoQ、dev.to、DeepWiki 等）

### 実行

以下のドキュメントを `docs/research/` 配下に作成した:

- [agentcore-family-overview.md](../research/agentcore-family-overview.md) — 全体概要・コンポーネント間関係
- [agentcore-runtime.md](../research/agentcore-runtime.md) — Runtime の詳細
- [agentcore-memory.md](../research/agentcore-memory.md) — Memory の詳細
- [agentcore-identity.md](../research/agentcore-identity.md) — Identity の詳細
- [agentcore-tools.md](../research/agentcore-tools.md) — Tools（Browser / Code Interpreter）の詳細
- [agentcore-gateway.md](../research/agentcore-gateway.md) — Gateway の詳細

### 検証

完了条件をすべて確認済み:
- 全コンポーネント（Runtime / Memory / Identity / Tools / Gateway + Observability）の役割・概要を把握
- 主要概念・用語（microVM、STM/LTM、ワークロード ID、MCP、M×N 統合問題など）を整理
- AWS 公式ドキュメント・aws-samples・AWS ブログの参照先を一覧化
- コンポーネント間の連携パターンをアーキテクチャ図で説明
- 全成果物を `docs/research/` 配下に保存

## 振り返り

### うまくいったこと

- AWS 公式ドキュメント・aws-samples・AWS ブログを組み合わせることで、各コンポーネントの詳細な情報を収集できた
- 各コンポーネントを独立した文書として整理したことで、参照しやすい構造になった
- コンポーネント間の連携パターンをアーキテクチャ図で表現できた

### 改善できること

- 各コンポーネントの API リファレンスをより詳細に記載できる
- 実際にサンプルコードを動かして動作確認を行うことで、情報の正確性を高められる（Task 5 で実施予定）

### 学んだこと

- AgentCore は 2025 年に発表された比較的新しいサービスであり、情報が急速に更新されている
- M×N 統合問題の解決が Gateway の中核的な価値である
- microVM（Firecracker）による完全セッション分離がエンタープライズ向けセキュリティの根幹
- MCP（Model Context Protocol）がエコシステム全体の統合プロトコルとして採用されている
- Runtime / Memory / Identity / Gateway / Tools はそれぞれ独立して利用可能だが、組み合わせることで真価を発揮する

## 参照リンク

- [タスクサイクル詳細手順](../workflow/task-cycle.md)
- [masterplan.md](../masterplan.md)
- [Task 4: 開発環境セットアップ](task-04-development-environment-setup.md)
