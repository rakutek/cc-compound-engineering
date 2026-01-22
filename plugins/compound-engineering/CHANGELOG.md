# 変更履歴

compound-engineeringプラグインのすべての重要な変更は、このファイルに記録されます。

この形式は[Keep a Changelog](https://keepachangelog.com/en/1.0.0/)に基づいており、
このプロジェクトは[セマンティックバージョニング](https://semver.org/spec/v2.0.0.html)に準拠しています。

## [2.29.7] - 2026-01-22

### 修正

- **スキル** - Context7 MCPツール名を正しい形式に更新
  - `verify-skill.md` - `mcp__context7__resolve-library-id` → `mcp__plugin_compound-engineering_context7__resolve-library-id`
  - `verify-skill.md` - `mcp__context7__get-library-docs` → `mcp__plugin_compound-engineering_context7__query-docs`
  - `create-domain-expertise-skill.md` - Context7ツール名を正しい形式に更新
- **README.md** - Context7ツール名を更新
  - `get-library-docs` → `query-docs`
- **コマンド** - 存在しないレビュアーへの参照を削除
  - `plan.md` - /plan-reviewの説明から「Simplicity」レビュアーを削除（code-simplicity-reviewerはkieran-code-reviewerに統合済み）

---

## [2.29.6] - 2026-01-21

### 修正

- **エージェント** - Playwright MCPへの古い参照をagent-browserスキルに更新
  - `bug-reproduction-validator.md` - 行22でPlaywright MCPをagent-browserスキルに置き換え
  - `figma-design-sync.md` - 行23と189でPlaywright MCPをagent-browserスキルに置き換え
- **コマンド** - 存在しないスキルへの参照を削除
  - `deepen-plan.md` - 行140でsecurity-patternsスキルの例をカスタムスキルの例に置き換え

---

## [2.29.5] - 2026-01-21

### 修正

- **workflows/work.md** - Playwright MCPツールへの古い参照をagent-browser CLIコマンドに更新
  - `browser_navigate` → `agent-browser open <url>`
  - `browser_resize` → 削除（agent-browserで自動設定）
  - `browser_snapshot` → `agent-browser snapshot -i`
  - `browser_take_screenshot` → `agent-browser screenshot`
- **resolve-pr-parallel.md** - 存在しないbin/スクリプトへの参照をgh CLIコマンドに置き換え
  - `bin/get-pr-comments` → `gh pr view PR_NUMBER --comments`
  - `bin/resolve-pr-thread` → コメントで直接解決するフローに変更

### 削除

- **release-docs.mdコマンド** - 存在しないdocs/とmcp-servers/ディレクトリを参照していたため削除
- **CLAUDE.md** - 存在しないdocs/solutions/plugin-versioning-requirements.mdへの参照を削除

### 改善

- すべてのツール参照が現在の実装と一致（Playwright MCP → agent-browser CLI）
- すべてのスクリプト参照が公式CLIツールを使用（bin/ → gh CLI）
- ドキュメントから存在しないファイルへの参照を削除
- コマンドカウントを13から12に更新（plugin.json、marketplace.json、README.md）

---

## [2.29.4] - 2026-01-21

### 修正

- **コマンド命名規則** - コマンドファイル名をハイフン区切りに統一（アンダースコアから変更）
  - `plan_review.md` → `plan-review.md`（`/plan-review`コマンド）
  - `resolve_pr_parallel.md` → `resolve-pr-parallel.md`（`/resolve-pr-parallel`コマンド）
  - すべての参照を新しい命名規則に更新（README.md、deepen-plan.md、workflows/plan.md）

### 削除

- **未使用のリファレンスファイル** - create-agent-skillsスキルから4つの未参照ファイルを削除
  - `references/be-clear-and-direct.md`
  - `references/api-security.md`
  - `references/common-patterns.md`
  - `references/executable-code.md`

### 改善

- コードベース全体で一貫した命名規則（すべてのコマンドがハイフン区切り）
- 保守性の向上（未使用ファイルの削除）

---

## [2.29.3] - 2026-01-21

### 変更

- **エージェント統合** - 冗長なエージェントを統合し、重複機能を排除（21個→19個）
  - `design-implementation-reviewer`を`figma-design-sync`に統合（レビュー専用モードと修正モードの両方をサポート）
  - `code-simplicity-reviewer`を`kieran-code-reviewer`に統合（YAGNI原則とシンプルさフォーカスを追加）

### 修正

- **スキル参照** - 存在しないファイルへの参照を削除
  - `create-agent-skills`ワークフローから`references/use-xml-tags.md`への参照を削除
  - `compound-docs/SKILL.md`から`hotwire-native`と`skill-creator`への古い参照を汎用化

### 改善

- エージェント数が削減され、保守性が向上
- 統合エージェントでより一貫した機能を提供
- ドキュメントの正確性が向上（存在しない参照を削除）

---

## [2.29.2] - 2026-01-20

### 修正

- **README.md** - レビューエージェントカウントを (11) から (10) に修正（実際のファイル数と一致）
- **design-iterator.md** - `<frontend_aesthetics>` セクションの重複を削除し、`frontend-design` スキルへの参照を追加
- **file-todos/SKILL.md** - description を日本語に変更（他のスキルとの一貫性のため）

---

## [2.29.1] - 2026-01-20

### 修正

- **削除されたコマンド参照** - `resolve_todo_parallel`から`resolve`への参照を修正（commands/triage.md、commands/workflows/review.md、skills/file-todos/SKILL.md）
- **削除されたスキルのキーワード** - plugin.jsonとmarketplace.jsonから`image-generation`キーワードを削除（gemini-imagegenスキルは2.27.0で削除済み）
- **古いPlaywright参照** - commands/browser-test.mdのTodoファイル命名規約を`playwright`から`browser`に更新（agent-browser CLIに合わせる）

---

## [2.29.0] - 2026-01-20

### 変更

- **スキル統合** - `skill-creator`と`create-agent-skills`を統合し、より包括的な`create-agent-skills`に集約（10個→9個）
- **コマンド統合** - `resolve_parallel`と`resolve_todo_parallel`を自動検出機能付きの`resolve`コマンドに統合（10個→9個）
- **データエージェント統合** - `data-integrity-guardian`、`data-migration-expert`、`deployment-verification-agent`を包括的な`production-data-guardian`に統合（3個→1個）
- **言語別レビュアー統合** - `kieran-python-reviewer`、`kieran-rails-reviewer`、`kieran-typescript-reviewer`を言語自動検出機能付きの`kieran-code-reviewer`に統合（3個→1個）

### 改善

- コンポーネント総数が45個から39個に削減（13%削減）
- 冗長性を排除し、保守性を向上
- 統合エージェントとコマンドでより一貫した体験を提供
- 言語検出により、開発者が適切なレビュアーを選ぶ必要がなくなる
- データ関連のレビューが統合され、包括的なチェックを提供

---

## [2.28.0] - 2026-01-20

### 削除

- **`deploy-docs` コマンド** - ドキュメントサイトデプロイコマンドを削除。
- **`generate_command` コマンド** - 新しいスラッシュコマンド生成コマンドを削除。
- **`reproduce-bug` コマンド** - バグ再現コマンドを削除。

---

## [2.27.0] - 2026-01-20

### 削除

- **`gemini-imagegen` スキル** - Gemini APIを使用した画像生成・編集スキルを削除。

---

## [2.26.0] - 2026-01-20

### 削除

- **`every-style-editor` スキル** - Everyのスタイルガイド準拠のためのコピーレビュースキルを削除。

---

## [2.25.0] - 2026-01-20

### 削除

- **`andrew-kane-gem-writer` スキル** - Andrew Kaneのパターンと哲学に従ったRuby gem作成のスキルを削除。

---

## [2.24.0] - 2026-01-13

### 追加

- **`agent-browser` スキル** - VercelのagentブラウザCLIの包括的なドキュメント。Webテスト、フォーム入力、スクリーンショット、データ抽出のためのブラウザ操作を自動化。機能：
  - Node.jsフォールバック付きの高速RustCLI
  - 正確な操作のための要素参照（`@e1`、`@e2`）
  - 認証のためのセッション状態永続化
  - 並列ブラウザセッション
  - セマンティックロケーター（役割、テキスト、ラベルで検索）

### 変更

- **`/browser-test` コマンド**（旧`/playwright-test`）- Playwright MCPからagent-browser CLIに名前変更および移行。コマンドは現在、ブラウザ自動化のためにBash経由で`agent-browser`コマンドを使用：
  - ナビゲーションのための`agent-browser open <url>`
  - ページ状態キャプチャのための`agent-browser snapshot -i`
  - エラーチェックのための`agent-browser console`
  - 操作のための`agent-browser click @ref`

### 削除

- **Playwright MCPサーバー** - `@playwright/mcp`依存関係を削除。ブラウザ自動化は現在、agent-browser CLI（npx経由でインストール）によって処理されます。

---

## [2.23.0] - 2026-01-08

### 追加

- **`kieran-code-quality` スキル** - すべてのKieranレビュアーから共通のコード品質原則を抽出。このスキルは、言語固有のレビュアーが構築する基盤を提供：
  - 重複 > 複雑さの哲学
  - 既存コードには厳格に、新しいコードには実用的に
  - 品質指標としてのテスト
  - 5秒命名ルール
  - モジュール抽出シグナル

### 変更

- **Kieranレビュアーのリファクタリング** - 3つの言語固有のレビュアーすべてが`kieran-code-quality`スキルを参照するようになりました：
  - `kieran-rails-reviewer` - Rails固有の規約に焦点（Turbo Streams、名前空間、サービス抽出）
  - `kieran-python-reviewer` - Python固有のパターンに焦点（型ヒント、PEP 8、最新のPython機能）
  - `kieran-typescript-reviewer` - TypeScript固有のルールに焦点（型安全性、Reactパターン、ES6+）

  これにより、重複が削減され、すべてのKieranレビュアー間で一貫性が確保されます。

---

## [2.22.0] - 2026-01-07

### 削除

- **`dhh-rails-style` スキル** - `dhh-ruby-style`に統合。2つのスキルは同一の説明と重複するコンテンツを持っていました。`dhh-rails-style`からのすべての参照（controllers.md、models.md、frontend.md、architecture.md、gems.md）は`dhh-ruby-style`にマージされました。
- **`every-style-editor` エージェント** - 重複エージェントを削除。`every-style-editor`スキルが、より良い参照ドキュメントで同じ機能を提供します。

### 変更

- **`dhh-ruby-style` スキル** - 削除された`dhh-rails-style`からの包括的な参照を含むようになりました：
  - controllers.md - RESTマッピング、コンサーン、Turboレスポンス、APIパターン
  - models.md - コンサーン、ステートレコード、コールバック、スコープ、PORO
  - frontend.md - Turbo、Stimulus、CSSアーキテクチャ、ビューパターン
  - architecture.md - ルーティング、認証、ジョブ、キャッシング、マルチテナンシー、設定
  - gems.md - 使用するものと避けるもの、その理由

---

## [2.21.0] - 2026-01-05

### 削除

- **`/xcode-test` コマンド** - iOSシミュレーターテストコマンドを削除
- **`mobile-patterns.md` リファレンス** - agent-native-architectureスキルからiOS/Swiftモバイルパターンを削除

このリリースでは、Web開発ワークフローに焦点を当てるため、すべてのiOS固有の実装を削除しました。

---

## [2.20.0] - 2026-01-01

### 変更

- **`create-agent-skills` スキル** - Anthropicの公式スキル仕様に合わせて完全書き直し：
  - **形式変更**: スキルは現在、標準のマークダウン見出し（`## Quick Start`、`## Instructions`）を使用し、XMLタグは使用しません。以前のバージョンはXMLタグを誤って推奨していましたが、これは公式形式ではありませんでした。
  - **命名規約**: 公式仕様に従い、動名詞形式（`creating-agent-skills`、`processing-pdfs`）を使用するように更新
  - **説明形式**: 三人称を使用し、何をするかといつ使用するかの両方を含む必要があります
  - `references/official-spec.md`を追加 - code.claude.com/docs/en/skillsからのAnthropicの公式スキル仕様
  - `references/best-practices.md`を追加 - platform.claude.comからのスキルオーサリングベストプラクティス
  - 古い`references/use-xml-tags.md`を削除 - これは誤ったガイダンスでした

### 哲学

この更新により、スキルがAnthropicの公式ドキュメントと一致します。主な洞察：**スキルはプロンプトです**。すべての標準的なプロンプティングベストプラクティスが適用されます。カスタムXMLタグではなく、標準のマークダウンを使用してください。SKILL.mdは500行未満に保ち、参照ファイルへの段階的な開示を行います。

---

## [2.19.0] - 2025-12-31

### 追加

- **`/deepen-plan` コマンド** - 計画のパワー強化。既存の計画を取り、各主要セクションに並列リサーチサブエージェントを実行して以下を追加：
  - ベストプラクティスと業界パターン
  - パフォーマンス最適化
  - UI/UX改善（該当する場合）
  - 品質向上とエッジケース
  - 実世界の実装例

  結果は、具体的な実装詳細を持つ、深く根拠のある本番対応の計画です。

### 変更

- **`/workflows:plan` コマンド** - 生成後メニューのオプション2として`/deepen-plan`を追加。注記を追加：ultrathinkを有効にして実行する場合、最大の深さのためにdeepen-planを自動実行します。

## [2.18.0] - 2025-12-25

### 追加

- **`agent-native-architecture` スキル** - **動的機能発見**パターンと**アーキテクチャレビューチェックリスト**を追加：

  **mcp-tool-design.mdの新パターン：**
  - **動的機能発見** - 外部API（HealthKit、HomeKit、GraphQL）の場合、実行時に利用可能な機能を返す発見ツール（`list_*`）と、文字列を受け取る汎用アクセスツール（列挙型ではない）を構築します。コードではなく、APIが検証します。これにより、エージェントはコード変更なしで新しいAPI機能を使用できます。
  - **CRUD完全性** - エージェントが作成できるすべてのエンティティは、読み取り、更新、削除も可能である必要があります。不完全なCRUD = 壊れたアクション同等性。

  **SKILL.mdの新規追加：**
  - **アーキテクチャレビューチェックリスト** - レビュアーの発見を設計フェーズの早期に押し上げます。ツール設計（動的vs静的、CRUD完全性）、アクション同等性（機能マップ、編集/削除）、UI統合（エージェント→UI通信）、コンテキストインジェクションをカバー。
  - **オプション11: API統合** - HealthKit、HomeKit、GraphQLなどの外部APIに接続するための新しい取り込みオプション
  - **新しいアンチパターン：** 静的ツールマッピング（各APIエンドポイントに個別のツールを構築）、不完全なCRUD（作成のみのツール）
  - 成功基準チェックリストに**ツール設計基準**セクションを追加

  **shared-workspace-architecture.mdの新規追加：**
  - **マルチデバイス同期のためのiCloudファイルストレージ** - 共有ワークスペースにiCloud Documentsを使用して、同期レイヤーを構築することなく、無料で自動のマルチデバイス同期を取得。実装パターン、競合処理、資格、使用すべきでない場合を含みます。

### 哲学

この更新は、**エージェントネイティブアプリ**の重要な洞察を体系化します：エージェントがユーザーと同じアクセス権を持つべき外部APIと統合する場合、静的ツールマッピングの代わりに**動的機能発見**を使用します。`read_steps`、`read_heart_rate`、`read_sleep`...を構築する代わりに、`list_health_types` + `read_health_data(dataType: string)`を構築します。エージェントが利用可能なものを発見し、APIが型を検証します。

注：このパターンは、「ユーザーができることは何でも、エージェントができる」という哲学に従ったエージェントネイティブアプリに特に適しています。意図的に制限された機能を持つ制約付きエージェントの場合、静的ツールマッピングが適切な場合があります。

---

## [2.17.0] - 2025-12-25

### 強化

- **`agent-native-architecture` スキル** - Every Reader iOSアプリの構築から得られた実世界の学びに基づく大幅な拡張。5つの新しい参照ドキュメントを追加し、既存のものを拡張：

  **新しいリファレンス：**
  - **dynamic-context-injection.md** - 実行時のアプリ状態をエージェントシステムプロンプトに注入する方法。コンテキストインジェクションパターン、注入するコンテキスト（リソース、アクティビティ、機能、語彙）、Swift/iOSとTypeScriptの実装パターン、コンテキストの鮮度をカバー。
  - **action-parity-discipline.md** - エージェントがユーザーができるすべてのことを実行できることを確保するためのワークフロー。機能マッピングテンプレート、同等性監査プロセス、PRチェックリスト、同等性のためのツール設計、コンテキスト同等性ガイドラインを含みます。
  - **shared-workspace-architecture.md** - エージェントとユーザーが同じデータスペースで作業するためのパターン。ディレクトリ構造、ファイルツール、UI統合（ファイル監視、共有ストア）、エージェント-ユーザーコラボレーションパターン、セキュリティ考慮事項をカバー。
  - **agent-native-testing.md** - エージェントネイティブアプリのテストパターン。「Can Agent Do It?」テスト、サプライズテスト、自動化された同等性テスト、統合テスト、CI/CD統合を含みます。
  - **mobile-patterns.md** - iOS/Android向けのモバイル固有パターン。バックグラウンド実行（チェックポイント/再開）、権限処理、コスト意識の設計（モデルティア、トークン予算、ネットワーク認識）、オフライン処理、バッテリー認識をカバー。

  **更新されたリファレンス：**
  - **architecture-patterns.md** - 3つの新しいパターンを追加：統合エージェントアーキテクチャ（1つのオーケストレーター、多数のエージェントタイプ）、エージェント-UI通信（共有データストア、ファイル監視、イベントバス）、モデルティア選択（高速/バランス/強力）。

  **更新されたスキルルート：**
  - **SKILL.md** - 取り込みメニューを拡張（現在10オプションで、コンテキストインジェクション、アクション同等性、共有ワークスペース、テスト、モバイルパターンを含む）。5つの新しいエージェントネイティブアンチパターンを追加（コンテキスト枯渇、孤立した機能、サンドボックス分離、サイレントアクション、機能の隠蔽）。エージェントネイティブおよびモバイル固有のチェックリストで成功基準を拡張。

- **`agent-native-reviewer` エージェント** - すべての新しいパターンをカバーする包括的なレビュープロセスで大幅に強化。現在、アクション同等性、コンテキスト同等性、共有ワークスペース、ツール設計（プリミティブvsワークフロー）、動的コンテキストインジェクション、モバイル固有の懸念をチェックします。詳細なアンチパターン、出力形式テンプレート、クイックチェック（「Write to Location」テスト、サプライズテスト）、モバイル固有の検証を含みます。

### 哲学

これらの更新は、エージェントネイティブモバイルアプリの構築から得られた重要な洞察を運用化します：**「エージェントは、UI機能をミラーするツールを通じて、アプリ状態に関する完全なコンテキストを持って、ユーザーができることは何でもできるべきです。」** これらの変更のきっかけとなった失敗事例：ユーザーが「私のリーディングフィードに何か書いて」と言ったときにエージェントが「どのリーディングフィード？」と尋ねました—`publish_to_feed`ツールがなく、「フィード」が何を意味するかについてのコンテキストがなかったためです。

## [2.16.0] - 2025-12-21

### 強化

- **`dhh-rails-style` スキル** - Marc Köhlbruggeの非公式37signalsコーディングスタイルガイドからのパターンを組み込んで、参照ドキュメントを大幅に拡張：
  - **controllers.md** - 認可パターン、レート制限、Sec-Fetch-Site CSRF保護、リクエストコンテキストコンサーンを追加
  - **models.md** - 検証の哲学、クラッシュさせる哲学（bangメソッド）、ラムダを使用したデフォルト値、Rails 7.1+パターン（normalizes、delegated types、store accessor）、タッチチェーンを持つコンサーンガイドラインを追加
  - **frontend.md** - Turboモーフィングのベストプラクティス、Turboフレームパターン、6つの新しいStimulusコントローラー（auto-submit、dialog、local-timeなど）、Stimulusのベストプラクティス、ビューヘルパー、パーソナライゼーションを持つキャッシング、ブロードキャストパターンを追加
  - **architecture.md** - パスベースのマルチテナンシー、データベースパターン（UUID、レコードとしての状態、ハード削除、カウンターキャッシュ）、バックグラウンドジョブパターン（トランザクションの安全性、エラー処理、バッチ処理）、メールパターン、セキュリティパターン（XSS、SSRF、CSP）、Active Storageパターンを追加
  - **gems.md** - 避けるものセクションを拡張（サービスオブジェクト、フォームオブジェクト、デコレーター、CSSプリプロセッサ、React/Vue）、Minitest/fixturesパターンでテストの哲学を追加

- **`dhh-ruby-style` スキル** - patterns.mdを以下で拡張：
  - 開発の哲学（出荷/検証/洗練、根本原因の修正、バニラRailsファースト）
  - Rails 7.1+イディオム（params.expect、StringInquirer、肯定的な命名規約）
  - 抽出ガイドライン（3のルール、コントローラーで開始し複雑になったら抽出）

### クレジット

- 参照パターンは[Marc Köhlbruggeの非公式37signalsコーディングスタイルガイド](https://github.com/marckohlbrugge/unofficial-37signals-coding-style-guide)から派生

## [2.15.2] - 2025-12-21

### 修正

- **すべてのスキル** - 13のスキル全体で仕様準拠の問題を修正：
  - 参照ファイルは現在、バッククォートテキストではなく、適切なマークダウンリンク（`[file.md](./references/file.md)`）を使用
  - 説明は現在、skill-creator仕様に従って三人称（「このスキルは...に使用されるべき」）を使用
  - 影響を受けるスキル：agent-native-architecture、andrew-kane-gem-writer、compound-docs、create-agent-skills、dhh-rails-style、dhh-ruby-style、dspy-ruby、every-style-editor、file-todos、frontend-design、gemini-imagegen

### 追加

- **CLAUDE.md** - 新しいスキルが仕様要件を満たすことを確保するための検証コマンドを含むスキルコンプライアンスチェックリストを追加

## [2.15.1] - 2025-12-18

### 変更

- **`/workflows:review` コマンド** - セクション7は現在、プロジェクトタイプ（Web、iOS、またはハイブリッド）を検出し、適切なテストを提供します。Webプロジェクトは`/playwright-test`を取得し、iOSプロジェクトは`/xcode-test`を取得し、ハイブリッドプロジェクトは両方を実行できます。

## [2.15.0] - 2025-12-18

### 追加

- **`/xcode-test` コマンド** - XcodeBuildMCPを使用してシミュレーターでiOSアプリをビルドおよびテスト。Xcodeプロジェクトを自動的に検出し、アプリをビルドし、シミュレーターを起動し、テストスイートを実行します。不安定なテストの再試行を含みます。

- **`/playwright-test` コマンド** - 現在のPRまたはブランチの影響を受けたページでPlaywrightブラウザテストを実行。変更されたファイルを検出し、影響を受けたルートにマッピングし、対象テストを生成/実行し、スクリーンショット付きで結果を報告します。
