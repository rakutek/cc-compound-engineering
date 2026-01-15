<overview>
認証情報(APIキー、トークン、シークレット)を必要とするAPIコールを行うスキルを構築する際、チャットに認証情報が表示されないようにするため、このプロトコルに従ってください。
</overview>

<the_problem>
環境変数を含む生のcurlコマンドは認証情報を露出します:

```bash
# ❌ 悪い例 - APIキーがチャットに表示される
curl -H "Authorization: Bearer $API_KEY" https://api.example.com/data
```

Claudeがこれを実行すると、展開された`$API_KEY`を含む完全なコマンドが会話に表示されます。
</the_problem>

<the_solution>
`~/.claude/scripts/secure-api.sh`を使用します - 認証情報を内部で読み込むラッパーです。

<for_supported_services>
```bash
# ✅ 良い例 - 認証情報は表示されない
~/.claude/scripts/secure-api.sh <service> <operation> [args]

# 例:
~/.claude/scripts/secure-api.sh facebook list-campaigns
~/.claude/scripts/secure-api.sh ghl search-contact "email@example.com"
```
</for_supported_services>

<adding_new_services>
APIコールを必要とする新しいスキルを構築する場合:

1. **ラッパーに操作を追加**(`~/.claude/scripts/secure-api.sh`):

```bash
case "$SERVICE" in
    yourservice)
        case "$OPERATION" in
            list-items)
                curl -s -G \
                    -H "Authorization: Bearer $YOUR_API_KEY" \
                    "https://api.yourservice.com/items"
                ;;
            get-item)
                ITEM_ID=$1
                curl -s -G \
                    -H "Authorization: Bearer $YOUR_API_KEY" \
                    "https://api.yourservice.com/items/$ITEM_ID"
                ;;
            *)
                echo "Unknown operation: $OPERATION" >&2
                exit 1
                ;;
        esac
        ;;
esac
```

2. **ラッパーにプロファイルサポートを追加**(サービスが複数のアカウントを必要とする場合):

```bash
# secure-api.sh内のプロファイルリマッピングセクションに追加:
yourservice)
    SERVICE_UPPER="YOURSERVICE"
    YOURSERVICE_API_KEY=$(eval echo \$${SERVICE_UPPER}_${PROFILE_UPPER}_API_KEY)
    YOURSERVICE_ACCOUNT_ID=$(eval echo \$${SERVICE_UPPER}_${PROFILE_UPPER}_ACCOUNT_ID)
    ;;
```

3. **`~/.claude/.env`に認証情報プレースホルダーを追加**プロファイル命名を使用:

```bash
# エントリが既に存在するか確認
grep -q "YOURSERVICE_MAIN_API_KEY=" ~/.claude/.env 2>/dev/null || \
  echo -e "\n# Your Service - Main profile\nYOURSERVICE_MAIN_API_KEY=\nYOURSERVICE_MAIN_ACCOUNT_ID=" >> ~/.claude/.env

echo "Added credential placeholders to ~/.claude/.env - user needs to fill them in"
```

4. **SKILL.mdにプロファイルワークフローを文書化**:

```markdown
## プロファイル選択ワークフロー

**重要:** 誤ったアカウント認証情報の使用を防ぐため、常にプロファイル選択を使用してください。

### ユーザーがYourService操作をリクエストした時:

1. **保存されたプロファイルを確認:**
   ```bash
   ~/.claude/scripts/profile-state get yourservice
   ```

2. **プロファイルが保存されていない場合、利用可能なプロファイルを探す:**
   ```bash
   ~/.claude/scripts/list-profiles yourservice
   ```

3. **プロファイルが1つのみの場合:** 自動的に使用し、通知:
   ```
   "Using YourService profile 'main' to list items..."
   ```

4. **複数のプロファイルがある場合:** ユーザーに選択を求める:
   ```
   "Which YourService profile: main, clienta, or clientb?"
   ```

5. **ユーザーの選択を保存:**
   ```bash
   ~/.claude/scripts/profile-state set yourservice <selected_profile>
   ```

6. **API呼び出し前に常に使用するプロファイルを通知:**
   ```
   "Using YourService profile 'main' to list items..."
   ```

7. **プロファイルを指定してAPIコール:**
   ```bash
   ~/.claude/scripts/secure-api.sh yourservice:<profile> list-items
   ```

## セキュアなAPIコール

すべてのAPIコールはプロファイル構文を使用:

```bash
~/.claude/scripts/secure-api.sh yourservice:<profile> <operation> [args]

# 例:
~/.claude/scripts/secure-api.sh yourservice:main list-items
~/.claude/scripts/secure-api.sh yourservice:main get-item <ITEM_ID>
```

**プロファイルはセッション中持続:** 一度選択されると、ユーザーが明示的に変更しない限り、後続の操作で同じプロファイルを使用します。
```
</adding_new_services>
</the_solution>

<pattern_guidelines>
<simple_get_requests>
```bash
curl -s -G \
    -H "Authorization: Bearer $API_KEY" \
    "https://api.example.com/endpoint"
```
</simple_get_requests>

<post_with_json_body>
```bash
ITEM_ID=$1
curl -s -X POST \
    -H "Authorization: Bearer $API_KEY" \
    -H "Content-Type: application/json" \
    -d @- \
    "https://api.example.com/items/$ITEM_ID"
```

使用法:
```bash
echo '{"name":"value"}' | ~/.claude/scripts/secure-api.sh service create-item
```
</post_with_json_body>

<post_with_form_data>
```bash
curl -s -X POST \
    -F "field1=value1" \
    -F "field2=value2" \
    -F "access_token=$API_TOKEN" \
    "https://api.example.com/endpoint"
```
</post_with_form_data>
</pattern_guidelines>

<credential_storage>
**場所:** `~/.claude/.env` (すべてのスキルで使用できるグローバル設定、どのディレクトリからでもアクセス可能)

**形式:**
```bash
# Service credentials
SERVICE_API_KEY=your-key-here
SERVICE_ACCOUNT_ID=account-id-here

# Another service
OTHER_API_TOKEN=token-here
OTHER_BASE_URL=https://api.other.com
```

**スクリプトでの読み込み:**
```bash
set -a
source ~/.claude/.env 2>/dev/null || { echo "Error: ~/.claude/.env not found" >&2; exit 1; }
set +a
```
</credential_storage>

<best_practices>
1. **スキルの例では`$VARIABLE`を含む生のcurlを使用しない** - 常にラッパーを使用
2. **すべての操作をラッパーに追加** - ユーザーにcurl構文を考えさせない
3. **認証情報プレースホルダーを自動作成** - スキル作成時に即座に`~/.claude/.env`に空のフィールドを追加
4. **認証情報は`~/.claude/.env`に保存** - 1つの中央管理場所、どこでも機能
5. **各操作を文書化** - SKILL.mdに例を表示
6. **エラーを適切に処理** - 環境変数の欠落をチェック、役立つエラーメッセージを表示
</best_practices>

<testing>
認証情報を露出せずにラッパーをテスト:

```bash
# このコマンドがチャットに表示される
~/.claude/scripts/secure-api.sh facebook list-campaigns

# しかしAPIキーは表示されない - スクリプト内部で読み込まれる
```

認証情報が読み込まれているか確認:
```bash
# .envが存在するか確認
ls -la ~/.claude/.env

# 特定の変数を確認(値は表示しない)
grep -q "YOUR_API_KEY=" ~/.claude/.env && echo "API key configured" || echo "API key missing"
```
</testing>
