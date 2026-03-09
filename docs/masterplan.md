# プロジェクト全体計画

> **タスク管理ルール**
> - タスクを追加したときは、このテーブルに行を追加しステータスを「未着手」にする
> - 着手したときはステータスを「進行中」に更新する
> - 完了したときはステータスを「完了」に更新し、成果物列にリンクを記載する
> - ステータス値は「未着手」「進行中」「完了」の3種類のみ使用する

---

## タスク一覧

| # | タスク名 | ステータス | 成果物 | 備考 |
|---|----------|------------|--------|------|
| 1 | AIエージェント向けガイドライン文書群の初期構築 | 完了 | [ai-guidelines-master.md](ai-guidelines-master.md), [CLAUDE.md](../CLAUDE.md), [AGENTS.md](../AGENTS.md), [.github/copilot-instructions.md](../.github/copilot-instructions.md), [.kiro/steering/project.md](../.kiro/steering/project.md) | 常時参照ファイルと詳細ドキュメントの構築 |
| 2 | タスクサイクル詳細手順の整備 | 完了 | [workflow/task-cycle.md](workflow/task-cycle.md), [workflow/templates/task.md](workflow/templates/task.md) | タスクサイクルの手順書とテンプレートの作成 |
| 3 | AgentCore Runtime 調査 | 未着手 | — | 実行基盤としての AgentCore Runtime の機能調査 |

---

## プロジェクト概要

このリポジトリは、AI エージェントが常時稼働できるプラットフォームを調査するためのドキュメントリポジトリです。
とくに **Amazon Bedrock AgentCore** ファミリーを対象とし、実行基盤としての **AgentCore Runtime** に着目します。

## 参照リンク

- [AIガイドラインマスター（SSOT）](ai-guidelines-master.md)
- [タスクサイクル詳細手順](workflow/task-cycle.md)
- [汎用タスクテンプレート](workflow/templates/task.md)
