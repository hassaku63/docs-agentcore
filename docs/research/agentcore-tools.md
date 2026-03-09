# AgentCore Tools 詳細

> **調査日**: 2026-03-09  
> **情報の鮮度**: 2025年下半期時点の公式情報に基づく

---

## 概要

AgentCore Tools は、エージェントがウェブを操作・閲覧したり、コードを実行したりするためのビルトインツールを提供します。ブラウザツールとコードインタープリターの 2 種類が提供されており、いずれも専用の隔離 VM 内で安全に実行されます。

---

## ブラウザツール（Browser Tool）

### 概要

AI エージェントが制御可能なフルマネージドブラウザセッションを提供します。Playwright / Puppeteer ベースのブラウザ自動化を安全な環境で実行できます。

### 主な機能

- **ウェブナビゲーション**: URL への移動、フォーム入力、ボタンクリック等の自動操作
- **データ抽出**: Webページからの情報スクレイピング
- **ライブビューストリーミング**: 人間がエージェントのブラウザ操作をリアルタイム監視・介入可能
- **カスタム設定**: カスタムプロファイル、プロキシ設定、ブラウザ拡張機能のサポート
- **ドメインフィルタリング**: アクセス可能なドメインを制限するポリシー設定

### セキュリティ

- セッションごとに独立した使い捨て VM で動作
- エフェメラルストレージ（セッション終了時にすべて削除）
- クロスセッションのデータ漏洩やラテラルムーブメントを防止
- VPC 接続・AWS PrivateLink 対応（規制が厳しい環境向け）

### ユースケース

- ウェブリサーチの自動化
- フォーム入力・データ収集の自動化
- 内部 Web アプリケーションとのインタラクション
- コンプライアンス監視が必要なウェブ操作タスク

---

## コードインタープリター（Code Interpreter）

### 概要

AI エージェントが Python 等のコードを完全管理された隔離 VM 上で安全に実行するためのサンドボックス環境です。

### 主な機能

- **安全なコード実行**: 完全管理された VM 上でコードを実行し、本番インフラへの直接露出を防止
- **データ分析・可視化**: Pandas、Matplotlib 等を使ったデータ処理とグラフ生成
- **レポート生成**: 計算結果・分析結果のレポート自動生成
- **ワークフロー検証**: 複雑な計算・ロジックの検証処理
- **コンプライアンス対応**: OpenTelemetry / CloudWatch との統合による監査ログ

### セキュリティ

- セッション分離による独立した実行環境
- エフェメラルファイルシステム（セッション終了時に破棄）
- サンドボックス内での特権操作の閉じ込め
- AWS が VM のプロビジョニング・スケーリング・セキュリティ更新を管理

### ユースケース

- エージェントによるオンデマンドデータ分析
- 数値計算・シミュレーション
- コード生成後の実行・テスト検証
- ドキュメントや画像の処理・変換

---

## 共通のインフラ仕様

| 項目 | 値 |
|------|-----|
| VM 分離 | セッションごとに専用 VM |
| ストレージ | エフェメラル（セッション終了時に削除） |
| スケーリング | アカウントあたり最大 3,000 同時セッション |
| ネットワーク | VPC 接続・AWS PrivateLink 対応 |
| 監視 | CloudWatch + OpenTelemetry |

---

## 参照リソース

- [AWS Blog: Introducing the AgentCore Code Interpreter](https://aws.amazon.com/blogs/machine-learning/introducing-the-amazon-bedrock-agentcore-code-interpreter/)
- [GitHub チュートリアル: Code Interpreter](https://github.com/awslabs/amazon-bedrock-agentcore-samples/tree/main/01-tutorials/05-AgentCore-tools/01-Agent-Core-code-interpreter)
- [GitHub チュートリアル: Browser Tool](https://github.com/awslabs/amazon-bedrock-agentcore-samples/tree/main/01-tutorials/05-AgentCore-tools/02-Agent-Core-browser-tool)
- [Browser Tool Architecture and Security (DeepWiki)](https://deepwiki.com/awslabs/amazon-bedrock-agentcore-samples/8.1-browser-tool-overview)
- [VPC・PrivateLink 対応のアナウンス](https://www.westloop.io/post/amazon-bedrock-agentcore-runtime-browser-and-code-interpreter-add-support-for-vpc-aws-privatelink-cloudformation-and-tagging)
