# プロジェクト全体計画

> **タスク管理ルール**
> - タスクを追加したときは、このテーブルに行を追加しステータスを「未着手」にする
> - 着手したときはステータスを「進行中」に更新する
> - 完了したときはステータスを「完了」に更新し、成果物列にリンクを記載する
> - ステータス値は「未着手」「進行中」「完了」の3種類のみ使用する

---

## エージェント開発ロードマップ

AgentCore ファミリーを使って独自エージェントを構築・稼働させるまでの道筋を以下のフェーズで整理します。

```
フェーズ 1: 概念習得  →  フェーズ 2: 環境準備  →  フェーズ 3: 実装・稼働  →  フェーズ 4: 知識整理
```

| フェーズ | 内容 | 対応タスク |
|----------|------|------------|
| 1. 概念習得 | AgentCore ファミリー全体の概要と主要概念を把握する | Task 3 |
| 2. 環境準備 | 開発・実行に必要な AWS 環境・ツールを整える | Task 4 |
| 3. 実装・稼働 | エージェントを実装し、AgentCore Runtime で動かす | Task 5, 6 |
| 4. 知識整理 | 主要概念・ベストプラクティスを整理して自走できるようにする | Task 7 |

---

## タスク一覧

| # | タスク名 | ステータス | 成果物 | 備考 |
|---|----------|------------|--------|------|
| 1 | AIエージェント向けガイドライン文書群の初期構築 | 完了 | [ai-guidelines-master.md](ai-guidelines-master.md), [CLAUDE.md](../CLAUDE.md), [AGENTS.md](../AGENTS.md), [.github/copilot-instructions.md](../.github/copilot-instructions.md), [.kiro/steering/project.md](../.kiro/steering/project.md) | 常時参照ファイルと詳細ドキュメントの構築 |
| 2 | タスクサイクル詳細手順の整備 | 完了 | [workflow/task-cycle.md](workflow/task-cycle.md), [workflow/templates/task.md](workflow/templates/task.md) | タスクサイクルの手順書とテンプレートの作成 |
| 3 | AgentCore ファミリー全体概要調査 | 完了 | [agentcore-family-overview.md](research/agentcore-family-overview.md), [agentcore-runtime.md](research/agentcore-runtime.md), [agentcore-memory.md](research/agentcore-memory.md), [agentcore-identity.md](research/agentcore-identity.md), [agentcore-tools.md](research/agentcore-tools.md), [agentcore-gateway.md](research/agentcore-gateway.md) | Runtime / Memory / Identity / Tools / Gateway の各コンポーネントの役割・機能・主要概念を把握する（[タスクファイル](tasks/task-03-agentcore-family-overview.md)）|
| 4 | 開発環境・前提条件のセットアップ手順整備 | 未着手 | — | AWS CLI / SDK / IAM 権限など、エージェント開発に必要な環境構築手順をまとめる（[タスクファイル](tasks/task-04-development-environment-setup.md)）|
| 5 | AgentCore Runtime を使った基本エージェントの実装 | 未着手 | — | シンプルなエージェントを実装し、AgentCore Runtime にデプロイして動作確認する（[タスクファイル](tasks/task-05-basic-agent-implementation.md)）|
| 6 | AgentCore Memory・Identity・Tools の統合 | 未着手 | — | 会話履歴の永続化・認証・ツール呼び出しをエージェントに組み込む（[タスクファイル](tasks/task-06-advanced-integration.md)）|
| 7 | 主要概念まとめ・自走のための参照資料整備 | 未着手 | — | 学習・調査で得た知見を整理し、ユーザーが自走できる参照ドキュメントを作成する（[タスクファイル](tasks/task-07-knowledge-summary.md)）|

---

## プロジェクト概要

このリポジトリは、AI エージェントが常時稼働できるプラットフォームを調査するためのドキュメントリポジトリです。
とくに **Amazon Bedrock AgentCore** ファミリーを対象とし、実行基盤としての **AgentCore Runtime** に着目します。

## 参照リンク

- [AIガイドラインマスター（SSOT）](ai-guidelines-master.md)
- [タスクサイクル詳細手順](workflow/task-cycle.md)
- [汎用タスクテンプレート](workflow/templates/task.md)
