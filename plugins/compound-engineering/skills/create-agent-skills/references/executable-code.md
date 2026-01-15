<when_to_use_scripts>
Claudeがスクリプトを書ける場合でも、事前に作成されたスクリプトには利点があります:
- 生成されたコードより信頼性が高い
- トークンを節約 (コンテキストにコードを含める必要がない)
- 時間を節約 (コード生成不要)
- 使用全体で一貫性を保証

<execution_vs_reference>
Claudeが何をすべきかを明確にします:
- **スクリプトを実行する** (最も一般的): "フィールドを抽出するには`analyze_form.py`を実行します"
- **参照として読む** (複雑なロジックの場合): "抽出アルゴリズムについては`analyze_form.py`を参照してください"

ほとんどのユーティリティスクリプトでは、実行が推奨されます。
</execution_vs_reference>

<how_scripts_work>
Claudeがbash経由でスクリプトを実行するとき:
1. スクリプトコードはコンテキストウィンドウに入りません
2. スクリプト出力のみがトークンを消費します
3. Claudeが同等のコードを生成するよりはるかに効率的です
</how_scripts_work>
</when_to_use_scripts>

<file_organization>
<scripts_directory>
**Best practice**: Place all executable scripts in a `scripts/` subdirectory within the skill folder.

```
skill-name/
├── SKILL.md
├── scripts/
│   ├── main_utility.py
│   ├── helper_script.py
│   └── validator.py
└── references/
    └── api-docs.md
```

**Benefits**:
- Keeps skill root clean and organized
- Clear separation between documentation and executable code
- Consistent pattern across all skills
- Easy to reference: `python scripts/script_name.py`

**Reference pattern**: In SKILL.md, reference scripts using the `scripts/` path:

```bash
python ~/.claude/skills/skill-name/scripts/analyze.py input.har
```
</scripts_directory>
</file_organization>

<utility_scripts_pattern>
<example>
## Utility scripts

**analyze_form.py**: Extract all form fields from PDF

```bash
python scripts/analyze_form.py input.pdf > fields.json
```

Output format:
```json
{
  "field_name": { "type": "text", "x": 100, "y": 200 },
  "signature": { "type": "sig", "x": 150, "y": 500 }
}
```

**validate_boxes.py**: Check for overlapping bounding boxes

```bash
python scripts/validate_boxes.py fields.json
# Returns: "OK" or lists conflicts
```

**fill_form.py**: Apply field values to PDF

```bash
python scripts/fill_form.py input.pdf fields.json output.pdf
```
</example>
</utility_scripts_pattern>

<solve_dont_punt>
Handle error conditions rather than punting to Claude.

<example type="good">
```python
def process_file(path):
    """Process a file, creating it if it doesn't exist."""
    try:
        with open(path) as f:
            return f.read()
    except FileNotFoundError:
        print(f"File {path} not found, creating default")
        with open(path, 'w') as f:
            f.write('')
        return ''
    except PermissionError:
        print(f"Cannot access {path}, using default")
        return ''
```
</example>

<example type="bad">
```python
def process_file(path):
    # Just fail and let Claude figure it out
    return open(path).read()
```
</example>

<configuration_values>
Document configuration parameters to avoid "voodoo constants":

<example type="good">
```python
# HTTP requests typically complete within 30 seconds
REQUEST_TIMEOUT = 30

# Three retries balances reliability vs speed
MAX_RETRIES = 3
```
</example>

<example type="bad">
```python
TIMEOUT = 47  # Why 47?
RETRIES = 5   # Why 5?
```
</example>
</configuration_values>
</solve_dont_punt>

<package_dependencies>
<runtime_constraints>
Skills run in code execution environment with platform-specific limitations:
- **claude.ai**: Can install packages from npm and PyPI
- **Anthropic API**: No network access and no runtime package installation
</runtime_constraints>

<guidance>
List required packages in your SKILL.md and verify they're available.

<example type="good">
Install required package: `pip install pypdf`

Then use it:

```python
from pypdf import PdfReader
reader = PdfReader("file.pdf")
```
</example>

<example type="bad">
"Use the pdf library to process the file."
</example>
</guidance>
</package_dependencies>

<mcp_tool_references>
If your Skill uses MCP (Model Context Protocol) tools, always use fully qualified tool names.

<format>ServerName:tool_name</format>

<examples>
- Use the BigQuery:bigquery_schema tool to retrieve table schemas.
- Use the GitHub:create_issue tool to create issues.
</examples>

Without the server prefix, Claude may fail to locate the tool, especially when multiple MCP servers are available.
</mcp_tool_references>
