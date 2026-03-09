# Amazon Kiro 向けガイドライン

> **このファイルはマスターガイドライン（SSOT）の要約です。**
> 詳細・更新はすべて [`docs/ai-guidelines-master.md`](../../docs/ai-guidelines-master.md) を参照してください。
> このファイルを変更する場合は、マスターおよび他の全サービス向けファイルを同時に更新してください。

---

## プロジェクト概要

このリポジトリは、**Amazon Bedrock AgentCore** ファミリーを対象とした調査・ドキュメントリポジトリです。
とくに実行基盤としての **AgentCore Runtime** に着目しています。

## タスクの進め方

タスクは次のサイクルで反復します。詳細は [`docs/workflow/task-cycle.md`](../../docs/workflow/task-cycle.md) を参照してください。

1. 計画の言語化
2. 詳細化（仕様検討）
3. 実行（実装）
4. 検証
5. 振り返りを記録

## 重要なルール

- **言語**: 見出し・説明文は日本語を優先する。技術的な固有名詞・ライブラリ名は英語のままでよい。
- **図・ダイアグラム**: プレーンテキスト（ASCII アート）ではなく **Mermaid 記法**を使用する。詳細は [`docs/ai-guidelines-master.md`](../../docs/ai-guidelines-master.md) の「表現フォーマット規約」を参照。
- **タスク管理**: タスクのステータスは [`docs/masterplan.md`](../../docs/masterplan.md) で管理する。
- **ドキュメント更新**: ガイドラインを変更したときは全サービス向けファイルを同時に更新する。

## 参照リンク

| ファイル | 内容 |
|----------|------|
| [`docs/ai-guidelines-master.md`](../../docs/ai-guidelines-master.md) | 全サービス向けガイドライン（SSOT） |
| [`docs/masterplan.md`](../../docs/masterplan.md) | プロジェクト全体計画・タスクステータス一覧 |
| [`docs/workflow/task-cycle.md`](../../docs/workflow/task-cycle.md) | タスクサイクルの詳細手順 |
| [`docs/workflow/templates/task.md`](../../docs/workflow/templates/task.md) | 汎用タスクテンプレート |
