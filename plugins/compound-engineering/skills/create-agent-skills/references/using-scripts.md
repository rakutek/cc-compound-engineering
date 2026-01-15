# スキルでスクリプトを使用する

<purpose>
スクリプトは、Claudeが毎回再生成するのではなく、そのまま実行する実行可能コードです。繰り返し操作の信頼性が高く、エラーのない実行を保証します。
</purpose>

<when_to_use>
以下の場合にスクリプトを使用します:
- 同じコードが複数のスキル呼び出しで実行される
- ゼロから書き直すとエラーが発生しやすい操作
- 複雑なシェルコマンドまたはAPI対話が関与する
- 柔軟性より一貫性が重要

一般的なスクリプトタイプ:
- **デプロイ** - Vercelへのデプロイ、パッケージの公開、リリースのプッシュ
- **セットアップ** - プロジェクトの初期化、依存関係のインストール、環境の構成
- **API呼び出し** - 認証されたリクエスト、Webhookハンドラー、データフェッチ
- **データ処理** - ファイル変換、バッチ操作、マイグレーション
- **ビルドプロセス** - コンパイル、バンドル、テストランナー
</when_to_use>

<script_structure>
スクリプトはスキルディレクトリ内の`scripts/`に配置します:

```
skill-name/
├── SKILL.md
├── workflows/
├── references/
├── templates/
└── scripts/
    ├── deploy.sh
    ├── setup.py
    └── fetch-data.ts
```

適切に構造化されたスクリプトには以下が含まれます:
1. 上部に明確な目的コメント
2. 入力検証
3. エラー処理
4. 可能な限りべべ等性のある操作
5. 明確な出力/フィードバック
</script_structure>

<script_example>
```bash
#!/bin/bash
# deploy.sh - Deploy project to Vercel
# Usage: ./deploy.sh [environment]
# Environments: preview (default), production

set -euo pipefail

ENVIRONMENT="${1:-preview}"

# Validate environment
if [[ "$ENVIRONMENT" != "preview" && "$ENVIRONMENT" != "production" ]]; then
    echo "Error: Environment must be 'preview' or 'production'"
    exit 1
fi

echo "Deploying to $ENVIRONMENT..."

if [[ "$ENVIRONMENT" == "production" ]]; then
    vercel --prod
else
    vercel
fi

echo "Deployment complete."
```
</script_example>

<workflow_integration>
Workflows reference scripts like this:

```xml
<process>
## Step 5: Deploy

1. Ensure all tests pass
2. Run `scripts/deploy.sh production`
3. Verify deployment succeeded
4. Update user with deployment URL
</process>
```

The workflow tells Claude WHEN to run the script. The script handles HOW the operation executes.
</workflow_integration>

<best_practices>
**すべきこと:**
- スクリプトをべべ等性にする (複数回実行しても安全)
- 明確な使用方法コメントを含める
- 実行前に入力を検証する
- 意味のあるエラーメッセージを提供する
- bashスクリプトでは`set -euo pipefail`を使用する

**すべきでないこと:**
- シークレットや認証情報をハードコードする (環境変数を使用)
- 一回限りの操作用にスクリプトを作成する
- エラー処理をスキップする
- スクリプトに関連のないことを多数行わせる
- スクリプトを実行可能にするのを忘れる (`chmod +x`)
</best_practices>

<security_considerations>
- スクリプトにAPIキー、トークン、またはシークレットを埋め込まない
- 機密設定には環境変数を使用する
- ユーザー提供の入力を検証およびサニタイズする
- データを削除または修正するスクリプトには注意する
- 破壊的な操作には`--dry-run`オプションを追加することを検討する
</security_considerations>
