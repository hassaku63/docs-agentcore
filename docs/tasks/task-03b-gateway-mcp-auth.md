# タスク: Gateway ネイティブ MCP サーバーの認証認可 詳細調査

> **使い方**: このファイルは `docs/tasks/` 配下のタスク管理ファイルです。
> 進捗メモ・振り返りを随時更新してください。

---

## 概要

AgentCore Gateway がネイティブ MCP サーバーをルーティングする際の認証認可フロー（Gateway ↔ MCP サーバー間の認証方式・認証コンテキストの伝播・対応スキーム一覧）を調査し、Task 6 での実装に向けた知識基盤を整える。

## 完了条件

- [x] インバウンド認証（エージェント → Gateway）のサポートスキームを把握している
- [x] アウトバウンド認証（Gateway → MCP サーバー）のサポートスキームを把握している
- [x] MCP サーバーターゲット固有の制約（Authorization Code 非対応など）を把握している
- [x] 認証コンテキスト（エンドユーザー ID）の伝播可否を明確化している
- [x] 設定手順・コード例が `docs/research/` 配下にまとめられている

## 仕様・制約

- 参照情報は AWS 公式発信を優先する（公式ドキュメント、AWS Blog、aws-samples）
- 実際のサンプルコード（Jupyter Notebook）から API 仕様の詳細を確認する

## 参照リソース（調査前の候補）

- [AWS 公式: Gateway 認証認可](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/gateway-using-auth.html)
- [GitHub サンプル: MCP サーバーをターゲットに](https://github.com/awslabs/amazon-bedrock-agentcore-samples/tree/main/01-tutorials/02-AgentCore-gateway/05-mcp-server-as-a-target)
- [AWS Blog: Unite MCP servers through AgentCore Gateway](https://aws.amazon.com/blogs/machine-learning/transform-your-mcp-architecture-unite-mcp-servers-through-agentcore-gateway/)

## 進捗メモ

### 計画の言語化

Task 3 の調査（[agentcore-gateway.md](../research/agentcore-gateway.md)）で、ネイティブ MCP サーバーへの認証方式の詳細が TODO として残っていた。本タスクではその詳細を調査し、Gateway 設定コード例とともに整理する。

調査の焦点：
1. **インバウンド認証**: エージェントが Gateway を呼び出す際の認証スキーム
2. **アウトバウンド認証**: Gateway が MCP サーバーを呼び出す際の認証スキーム
3. **制約・非サポート項目**: MCP サーバーターゲット固有の制限事項
4. **認証コンテキスト伝播**: エンドユーザー ID が MCP サーバーに渡るかどうか

### 詳細化

GitHub の公式サンプル Notebook（`01-tutorials/02-AgentCore-gateway/05-mcp-server-as-a-target/01-mcp-server-target.ipynb`）を参照し、実際の API 呼び出しパターンを確認した。

主な発見：
- インバウンド認証: `CUSTOM_JWT` と `NONE` がサポート。OIDC Discovery URL と許可クライアント ID を設定
- アウトバウンド認証: `OAUTH`（client_credentials）のみサポート。AgentCore Identity の Credential Provider を経由
- インバウンドとアウトバウンドで別々の IdP を使用可能（例: 別々の Cognito User Pool）
- tools/call 実行時は毎回新鮮なトークンを取得（キャッシュは使用しない）
- Authorization Code グラント（3LO）は非サポート（Gateway-MCP 間はサービス間通信のため）

### 実行

以下のドキュメントを `docs/research/` 配下に作成した：

- [agentcore-gateway-mcp-auth.md](../research/agentcore-gateway-mcp-auth.md) — 認証認可フロー詳細・設定例・スキーム対応一覧

また、[agentcore-gateway.md](../research/agentcore-gateway.md) の TODO を解消し、新規ドキュメントへのリンクを追加した。

### 検証

完了条件をすべて確認済み：
- インバウンド認証スキーム（CUSTOM_JWT / NONE）を把握
- アウトバウンド認証スキーム（OAuth2 client_credentials のみ）を把握
- MCP ターゲット固有の制約（3LO 非サポート、IAM 非サポート）を明確化
- 認証コンテキストは伝播しない（M2M のみ）を確認
- 設定手順・コード例を `docs/research/agentcore-gateway-mcp-auth.md` にまとめた

## 振り返り

### うまくいったこと

- 公式 GitHub サンプル Notebook から API の実際の使い方を確認できた
- インバウンド/アウトバウンドの独立した IdP 設定パターンを図で整理できた
- tools/call 時のクレデンシャル取得フローを詳細に整理できた

### 改善できること

- Authorization Code グラント（3LO）のユーザー ID 伝播が必要なケースの代替アーキテクチャを調査できていない（Task 6 で実装時に検討）
- 実際に AWS 環境で動作確認を行うことで情報の正確性を高められる（Task 6 で実施予定）

### 学んだこと

- MCP サーバーターゲットへのアウトバウンド認証は client_credentials（M2M）に限定されている
- インバウンドとアウトバウンドの IdP を分離することで、侵害時の影響範囲を限定できる
- tools/call 実行時に毎回新鮮なトークンを取得する設計により、期限切れトークン問題を自動的に回避している
- エンドユーザー ID の伝播が不要な M2M シナリオであれば、Gateway による MCP 統合は直接的に実現できる

## 参照リンク

- [タスクサイクル詳細手順](../workflow/task-cycle.md)
- [masterplan.md](../masterplan.md)
- [調査結果: Gateway MCP 認証認可](../research/agentcore-gateway-mcp-auth.md)
- [Task 3: AgentCore ファミリー全体概要調査](task-03-agentcore-family-overview.md)
- [Task 6: AgentCore Memory・Identity・Tools の統合](task-06-advanced-integration.md)
