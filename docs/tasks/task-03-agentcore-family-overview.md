# タスク: AgentCore ファミリー全体概要調査

> **使い方**: このファイルは `docs/tasks/` 配下のタスク管理ファイルです。
> 進捗メモ・振り返りを随時更新してください。

---

## 概要

Amazon Bedrock AgentCore ファミリーを構成する各コンポーネント（Runtime / Memory / Identity / Tools / Gateway）の役割・機能・主要概念を調査し、エージェント開発に必要な知識基盤を整える。

## 完了条件

- [ ] AgentCore ファミリーの全コンポーネントの役割と概要を把握している
- [ ] 各コンポーネントの主要概念・用語が整理されている
- [ ] AWS 公式ドキュメント・サンプルコード（aws-samples 等）の参照先が一覧化されている
- [ ] コンポーネント間の関係・連携パターンが図または文章で説明されている
- [ ] 調査結果が `docs/research/` 配下にまとめられている

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

（調査着手時に記入）

### 実行

（調査中に記入）

### 検証

（検証時に記入）

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
