# Amazon Bedrock AgentCore ファミリー全体概要

> **調査日**: 2026-03-09  
> **情報の鮮度**: 2025年下半期時点の公式情報に基づく

---

## AgentCore とは

Amazon Bedrock AgentCore は、2025年に AWS が発表した、エンタープライズ向け AI エージェントの開発・デプロイ・運用を支えるフルマネージドサービスファミリーです。

プロトタイプ段階のエージェントを安全でスケーラブルな本番環境へ移行するための「インフラストラクチャレイヤー」として位置づけられています。エージェントフレームワーク（Strands Agents、LangGraph、CrewAI 等）やモデル（Amazon Bedrock、OpenAI、Anthropic 等）に依存しない、フレームワーク非依存の設計となっています。

---

## コンポーネント一覧

AgentCore ファミリーは以下の 6 つのコンポーネントで構成されます。

| コンポーネント | 役割 |
|----------------|------|
| **Runtime** | エージェントのサーバーレス実行基盤（microVM による強固なセッション分離） |
| **Memory** | 短期・長期の会話記憶の永続化と取得 |
| **Identity** | エージェントおよびユーザーの認証・認可・資格情報管理 |
| **Tools** | ブラウザ自動化・コードインタープリター等のビルトインツール |
| **Gateway** | REST API・Lambda・MCP サーバーをエージェント向けツールとして統合する管理ゲートウェイ |
| **Observability** | OpenTelemetry ベースのモニタリング・ロギング・可視化 |

---

## 各コンポーネントの概要

### 1. AgentCore Runtime

エージェントを実行するためのサーバーレスコンテナプラットフォームです。

**主な特徴:**
- **microVM による完全セッション分離**: AWS Firecracker 技術を活用し、セッションごとに専用の microVM を起動。CPU・メモリ・ファイルシステムを完全に分離し、セッション間のデータ漏洩を防止
- **長時間実行セッション対応**: 最大 8 時間のセッション継続が可能。複雑なマルチターン推論や長時間ワークフローに対応
- **エフェメラル実行環境**: セッション終了時に microVM は完全に破棄され、すべてのリソースがサニタイズされる
- **フレームワーク非依存**: LangGraph、Strands Agents、CrewAI など任意のフレームワークで実装したエージェントをコンテナ（ARM64）としてデプロイ可能
- **可観測性**: CloudWatch との統合による詳細なモニタリング・監査ログ・リソース使用状況の可視化
- **スケーリング**: アカウントあたり最大 3,000 の同時 Runtime/Browser セッションをサポート

**詳細**: [agentcore-runtime.md](agentcore-runtime.md)

---

### 2. AgentCore Memory

エージェントが会話履歴・ユーザー設定・長期的知識を跨セッションで保持するためのフルマネージドメモリプラットフォームです。

**主な特徴:**
- **短期記憶 (STM)**: セッション内の会話イベントをリアルタイムに記録し、マルチターンの文脈を維持
- **長期記憶 (LTM)**: 非同期のメモリ戦略（セマンティック抽出、サマリー、ユーザー設定抽出など）により、重要な情報・ファクト・嗜好をセッションを跨いで保持
- **セマンティック検索**: ベクターデータベース（OpenSearch Serverless）を活用し、関連する記憶を自然言語クエリで取得
- **ネームスペース管理**: ユーザーやメモリ戦略単位で記憶を論理的に整理
- **サーバーレスで自動スケーリング**: 独自のデータベース管理が不要

**詳細**: [agentcore-memory.md](agentcore-memory.md)

---

### 3. AgentCore Identity

エージェントとユーザーの認証・認可・資格情報ライフサイクルを管理するサービスです。

**主な特徴:**
- **ワークロード ID (Workload Identity)**: エージェントアプリケーション自体を主体とした認証。Runtime 上では自動的にプロビジョニングされ、ローカル開発時は `.agentcore.json` に保存
- **インバウンド認証**: IAM SigV4 および JWT ベアラートークン（OIDC）による、エージェントへのアクセス制御
- **アウトバウンド認証**: OAuth2 (3-Legged OAuth 含む)・API キーによる外部 API へのアクセス資格情報を安全に管理するトークンボルト
- **主要 IdP との連携**: Amazon Cognito、Okta、Microsoft Entra ID 等の SSO プロバイダーと統合
- **IaC 対応**: Terraform (`aws_bedrockagentcore_workload_identity`) でのリソース管理が可能

**詳細**: [agentcore-identity.md](agentcore-identity.md)

---

### 4. AgentCore Tools

エージェントが利用できるビルトインツール（ブラウザ自動化・コードインタープリター）を提供します。

**主な特徴:**

**ブラウザツール:**
- Playwright / Puppeteer ベースのフルマネージドブラウザセッションを提供
- セッションごとに独立した VM で動作し、クロスセッションデータ漏洩を防止
- ライブビューストリーミングによる人間の監視・介入が可能
- カスタムプロファイル、プロキシ設定、ブラウザ拡張機能をサポート

**コードインタープリター:**
- 完全管理された隔離 VM 上で Python 等のコードを実行
- データ分析・可視化・レポート生成・ワークフロー検証に活用可能
- セッション分離によるサンドボックス実行でインフラへの直接露出を防止
- OpenTelemetry/CloudWatch による監査・コンプライアンス対応

**共通:**
- VPC 接続・AWS PrivateLink 対応（高度なネットワーク分離環境向け）

**詳細**: [agentcore-tools.md](agentcore-tools.md)

---

### 5. AgentCore Gateway

REST API・AWS Lambda 関数・MCP（Model Context Protocol）サーバーを、エージェントが利用できるツールとして統合する管理ゲートウェイです。M×N 統合問題（多数のエージェント × 多数のツール）を解決する中核コンポーネントです。

**主な特徴:**
- **MCP への自動変換**: OpenAPI/Smithy 仕様の REST API を、コード変更なしに MCP 互換ツールとして提供
- **サポートする統合タイプ**:
  - REST API（S3 上の OpenAPI 仕様またはインライン定義）
  - AWS Lambda 関数（入出力スキーマを定義してツールとして登録）
  - ネイティブ MCP サーバー（外部 MCP サーバーをルーティング）
- **セマンティックツール検索**: 自然言語クエリでエージェントが適切なツールを発見・呼び出し可能
- **双方向認証管理**: インバウンド（OAuth2/JWT）とアウトバウンド（API キー、IAM、OAuth）の認証を統合管理
- **SynchronizeGateway API**: ツール定義のソース（OpenAPI 仕様など）から最新状態を同期

**詳細**: [agentcore-gateway.md](agentcore-gateway.md)

---

### 6. AgentCore Observability

エージェントの動作を可視化するための監視・ロギング基盤です。

**主な特徴:**
- OpenTelemetry ベースのトレーシング・メトリクス・ログ収集
- CloudWatch との統合によるダッシュボードとアラート
- セッション単位の詳細な実行ログとリソース使用状況の記録
- コンプライアンス要件に対応した監査ログ

---

## コンポーネント間の関係と連携パターン

```
[エンドユーザー / クライアント]
        |
        | (IAM SigV4 / JWT)
        ↓
[AgentCore Identity] ←→ [外部 IdP: Cognito, Okta 等]
        |
        | (認証済みリクエスト)
        ↓
[AgentCore Runtime] ──────────────────────────────────────────────────┐
  (microVM でエージェント実行)                                          |
        |                                                              |
        ├──→ [AgentCore Memory]  (セッション内 STM + 長期 LTM 取得)     |
        |                                                              |
        ├──→ [AgentCore Gateway] ──→ [REST API / Lambda / MCP サーバー] |
        |                                                              |
        ├──→ [AgentCore Tools]   (ブラウザ / コードインタープリター)     |
        |                                                              |
        └──→ [AgentCore Observability] (ログ・メトリクス・トレース)──────┘
```

### 主要な連携パターン

1. **基本エージェント実行**
   - `Runtime` でエージェントコンテナを起動
   - `Identity` でユーザー認証を行い、セッションを安全に開始
   - `Memory` から過去の会話履歴（LTM）を取得してコンテキストを注入
   - `Gateway` 経由で外部ツール・API を呼び出し
   - 実行ログを `Observability` に送信

2. **長期記憶を持つパーソナライズドエージェント**
   - セッション中の会話を `Memory` の STM に記録
   - セッション終了後、非同期で LTM に重要情報を抽出・保存
   - 次回セッション開始時に LTM から関連記憶を取得

3. **マルチエージェントオーケストレーション**
   - 複数のエージェントがそれぞれ `Runtime` 上の独立した microVM で動作
   - `Gateway` を共有ツールハブとして活用し、各エージェントが同じツールセットにアクセス
   - `Identity` でエージェント間の委任認証を管理

---

## 参照リソース

### AWS 公式ドキュメント
- [Amazon Bedrock AgentCore Developer Guide](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/)
- [AgentCore Runtime - セッション分離](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-sessions.html)
- [AgentCore Memory - はじめに](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory-get-started.html)
- [AgentCore Identity - 認証・認可](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-oauth.html)

### GitHub サンプルコード
- [awslabs/amazon-bedrock-agentcore-samples](https://github.com/awslabs/amazon-bedrock-agentcore-samples)
  - `01-tutorials/`: コンポーネントごとのハンズオンチュートリアル
  - `02-use-cases/`: ビジネスユースケースの実装例
  - `03-integrations/`: フレームワーク統合パターン
  - `04-infrastructure-as-code/`: CloudFormation / CDK / Terraform テンプレート
  - `05-blueprints/`: 本番対応のフルスタックブループリント
- [aws/bedrock-agentcore-starter-toolkit](https://aws.github.io/bedrock-agentcore-starter-toolkit/)

### AWS ブログ
- [Introducing Amazon Bedrock AgentCore Gateway](https://aws.amazon.com/blogs/machine-learning/introducing-amazon-bedrock-agentcore-gateway-transforming-enterprise-ai-agent-tool-development/)
- [Introducing the Amazon Bedrock AgentCore Code Interpreter](https://aws.amazon.com/blogs/machine-learning/introducing-the-amazon-bedrock-agentcore-code-interpreter/)
- [Transform your MCP architecture: Unite MCP servers through AgentCore Gateway](https://aws.amazon.com/blogs/machine-learning/transform-your-mcp-architecture-unite-mcp-servers-through-agentcore-gateway/)

### コミュニティ・解説記事
- [What is Amazon Bedrock AgentCore? (MissionCloud)](https://www.missioncloud.com/blog/what-is-amazon-bedrock-agentcore-ai-agent-platform-for-enterprise-deployment)
- [Amazon Launches Bedrock AgentCore (InfoQ)](https://www.infoq.com/news/2025/07/amazon-bedrock-agentcore-launch/)
- [AgentCore Identity - Introduction and Overview (dev.to)](https://dev.to/aws-heroes/amazon-bedrock-agentcore-identity-part-1-introduction-and-overview-di1)
