# Workflow: スキルコンテンツの正確性を検証する

<required_reading>
**今すぐこれらのリファレンスファイルを読んでください:**
1. references/skill-structure.md
</required_reading>

<purpose>
監査は構造をチェックします。**検証は真実性をチェックします。**

スキルには外部のものについての主張が含まれています: API、CLIツール、フレームワーク、サービス。これらは時間の経過とともに変化します。このワークフローはスキルの内容がまだ正確かどうかを確認します。
</purpose>

<process>
## ステップ1: スキルを選択する

```bash
ls ~/.claude/skills/
```

番号付きリストを表示し、「どのスキルの正確性を検証しますか?」と尋ねます。

## ステップ2: 読み取りと分類

スキル全体を読みます（SKILL.md + workflows/ + references/）:
```bash
cat ~/.claude/skills/{skill-name}/SKILL.md
cat ~/.claude/skills/{skill-name}/workflows/*.md 2>/dev/null
cat ~/.claude/skills/{skill-name}/references/*.md 2>/dev/null
```

主な依存タイプで分類します:

| タイプ | 例 | 検証方法 |
|------|----------|---------------------|
| **API/サービス** | manage-stripe, manage-gohighlevel | Context7 + WebSearch |
| **CLIツール** | build-macos-apps (xcodebuild, swift) | コマンド実行 |
| **フレームワーク** | build-iphone-apps (SwiftUI, UIKit) | ドキュメント用Context7 |
| **統合** | setup-stripe-payments | WebFetch + Context7 |
| **純粋なプロセス** | create-agent-skills | 外部依存なし |

報告: 「このスキルは主に[タイプ]ベースです。[方法]を使用して検証します。」

## ステップ3: 検証可能な主張を抽出する

スキルコンテンツをスキャンして抽出します:

**言及されているCLIツール:**
- ツール名 (xcodebuild, swift, npmなど)
- ドキュメントされている特定のフラグ/オプション
- 期待される出力パターン

**APIエンドポイント:**
- サービス名 (Stripe, Metaなど)
- ドキュメントされている特定のエンドポイント
- 認証方法
- SDKバージョン

**フレームワークパターン:**
- フレームワーク名 (SwiftUI, Reactなど)
- ドキュメントされている特定のAPI/パターン
- バージョン固有の機能

**ファイルパス/構造:**
- 期待されるプロジェクト構造
- 設定ファイルの場所

提示: 「チェックする検証可能なX個の項目が見つかりました。」

## ステップ4: タイプ別に検証する

### CLIツールの場合
```bash
# ツールが存在するか確認
which {tool-name}

# バージョンを確認
{tool-name} --version

# ドキュメントされたフラグが機能するか検証
{tool-name} --help | grep "{documented-flag}"
```

### API/サービススキルの場合
Context7を使用して現在のドキュメントを取得します:
```
mcp__context7__resolve-library-id: {service-name}
mcp__context7__get-library-docs: {library-id}, topic: {relevant-topic}
```

スキルのドキュメントされたパターンを現在のドキュメントと比較します:
- エンドポイントはまだ有効ですか？
- 認証が変更されましたか？
- 非推奨のメソッドが使用されていますか？

### フレームワークスキルの場合
Context7を使用します:
```
mcp__context7__resolve-library-id: {framework-name}
mcp__context7__get-library-docs: {library-id}, topic: {specific-api}
```

確認します:
- ドキュメントされたAPIはまだ最新ですか？
- パターンは変わりましたか？
- より新しい推奨アプローチはありますか？

### 統合スキルの場合
最近の変更についてWebSearchします:
```
"[サービス名] API changes 2025"
"[サービス名] breaking changes"
"[サービス名] deprecated endpoints"
```

その後、現在のSDKパターンについてContext7を使用します。

### ステータスページを持つサービスの場合
利用可能な場合、公式ドキュメント/変更履歴をWebFetchします。

## ステップ5: 新鮮さレポートを生成する

所見を提示します:

```
## 検証レポート: {skill-name}

### ✅ 最新確認済み
- [主張]: [まだ正確である証拠]

### ⚠️ 古くなっている可能性
- [主張]: [変わったこと / 見つかった新しい情報]
  → 現在: [ドキュメントが現在言っていること]

### ❌ 壊れている/無効
- [主張]: [なぜ間違っているか]
  → 修正: [何であるべきか]

### ℹ️ 検証できなかった
- [主張]: [検証が不可能だった理由]

---
**総合ステータス:** [新鮮 / 更新が必要 / かなり古い]
**最終検証:** [今日の日付]
```

## ステップ6: 更新を提案する

問題が見つかった場合:

「更新が必要な[N]個の項目が見つかりました。どうしますか:」

1. **すべて更新** - すべての修正を適用
2. **1つずつレビュー** - 適用前に各変更を表示
3. **レポートのみ** - 変更なし

更新する場合:
- 検証された現在の情報に基づいて変更を行う
- 適切な場合、検証日コメントを追加
- 更新された内容を報告

## ステップ7: 検証スケジュールを提案する

スキルタイプに基づいて推奨します:

| スキルタイプ | 推奨頻度 |
|------------|----------------------|
| API/サービス | 1-2ヶ月ごと |
| フレームワーク | 3-6ヶ月ごと |
| CLIツール | 6ヶ月ごと |
| 純粋なプロセス | 年次 |

「このスキルはおよそ[時間枠]で再検証する必要があります。」
</process>

<verification_shortcuts>
## 迅速検証コマンド

**CLIツールが存在するか確認し、バージョンを取得:**
```bash
which {tool} && {tool} --version
```

**任意のライブラリ用のContext7パターン:**
```
1. resolve-library-id: "{library-name}"
2. get-library-docs: "{id}", topic: "{specific-feature}"
```

**WebSearchパターン:**
- 破壊的変更: "{service} breaking changes 2025"
- 非推奨: "{service} deprecated API"
- 現在のベストプラクティス: "{framework} best practices 2025"
</verification_shortcuts>

<success_criteria>
検証は以下が完了したときに完了です:
- [ ] 依存タイプ別にスキルが分類された
- [ ] 検証可能な主張が抽出された
- [ ] 各主張が適切な方法でチェックされた
- [ ] 新鮮さレポートが生成された
- [ ] 更新が適用された（要求された場合）
- [ ] ユーザーがいつ再検証するかを知っている
</success_criteria>
