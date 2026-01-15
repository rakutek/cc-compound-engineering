# スキル作成のベストプラクティス

Source: [platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)

## コアプリンシプル

### 簡潔さが鍵

コンテキストウィンドウは公共財です。あなたのスキルは、Claudeが知る必要のある他のすべてのものとコンテキストウィンドウを共有します。

**デフォルトの前提**: Claudeはすでに非常に賢いです。Claudeがまだ持っていないコンテキストのみを追加してください。

各情報に問いかけます:
- "Claudeは本当にこの説明が必要か?"
- "Claudeがこれを知っていると仮定できるか?"
- "この段落はそのトークンコストを正当化できるか?"

**良い例 (簡潔、約50トークン):**
```markdown
## Extract PDF text

Use pdfplumber for text extraction:

```python
import pdfplumber
with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```
```

**悪い例 (冗長すぎる、約150トークン):**
```markdown
## Extract PDF text

PDF (Portable Document Format) files are a common file format that contains
text, images, and other content. To extract text from a PDF, you'll need to
use a library. There are many libraries available...
```

### 適切な自由度を設定する

タスクの脆弱性と変動性に合わせて具体性を調整します。

**高い自由度** (複数の有効なアプローチ):
```markdown
## Code review process

1. Analyze the code structure and organization
2. Check for potential bugs or edge cases
3. Suggest improvements for readability
4. Verify adherence to project conventions
```

**中程度の自由度** (バリエーションを持つ推奨パターン):
```markdown
## Generate report

Use this template and customize as needed:

```python
def generate_report(data, format="markdown"):
    # Process data
    # Generate output in specified format
```
```

**低い自由度** (脆弱、正確な順序が必要):
```markdown
## Database migration

Run exactly this script:

```bash
python scripts/migrate.py --verify --backup
```

Do not modify the command or add flags.
```

### すべてのモデルでテストする

スキルはモデルへの追加として機能します。Haiku、Sonnet、Opusでテストしてください。

- **Haiku**: スキルは十分なガイダンスを提供していますか?
- **Sonnet**: スキルは明確で効率的ですか?
- **Opus**: スキルは過度な説明を避けていますか?

## 命名規則

スキル名には**動名詞形式** (動詞 + -ing) を使用します:

**良い:**
- `processing-pdfs`
- `analyzing-spreadsheets`
- `managing-databases`
- `testing-code`
- `writing-documentation`

**許容可能な代替案:**
- 名詞句: `pdf-processing`, `spreadsheet-analysis`
- Action-oriented: `process-pdfs`, `analyze-spreadsheets`

**Avoid:**
- Vague: `helper`, `utils`, `tools`
- Generic: `documents`, `data`, `files`
- Reserved: `anthropic-*`, `claude-*`

## Writing Effective Descriptions

**Always write in third person.** The description is injected into the system prompt.

**Be specific and include key terms:**

```yaml
# PDF Processing skill
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.

# Excel Analysis skill
description: Analyze Excel spreadsheets, create pivot tables, generate charts. Use when analyzing Excel files, spreadsheets, tabular data, or .xlsx files.

# Git Commit Helper skill
description: Generate descriptive commit messages by analyzing git diffs. Use when the user asks for help writing commit messages or reviewing staged changes.
```

**曖昧な説明を避ける:**
```yaml
description: ドキュメントを支援  # 曖昧すぎる!
description: データを処理       # 一般的すぎる!
description: ファイルで何かをする # 無意味!
```

## 漸進的開示パターン

### パターン1: 参照付き高レベルガイド

```markdown
---
name: pdf-processing
description: Extracts text and tables from PDF files, fills forms, merges documents.
---

# PDF処理

## クイックスタート

```python
import pdfplumber
with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

## 高度な機能

**フォーム入力**: [FORMS.md](FORMS.md)を参照
**APIリファレンス**: [REFERENCE.md](REFERENCE.md)を参照
**例**: [EXAMPLES.md](EXAMPLES.md)を参照
```

### パターン2: ドメイン固有の組織

```
bigquery-skill/
├── SKILL.md (overview and navigation)
└── reference/
    ├── finance.md (revenue, billing)
    ├── sales.md (opportunities, pipeline)
    ├── product.md (API usage, features)
    └── marketing.md (campaigns, attribution)
```

### パターン3: 条件付き詳細

```markdown
# DOCX処理

## ドキュメントの作成

新しいドキュメントにはdocx-jsを使用します。[DOCX-JS.md](DOCX-JS.md)を参照してください。

## ドキュメントの編集

簡単な編集の場合は、XMLを直接修正します。

**変更追跡の場合**: [REDLINING.md](REDLINING.md)を参照
**OOXMLの詳細の場合**: [OOXML.md](OOXML.md)を参照
```

## 参照を一階層の深さに保つ

Claudeは、他の参照ファイルから参照されるファイルを部分的にしか読み取らない場合があります。

**悪い (深すぎる):**
```markdown
# SKILL.md
See [advanced.md](advanced.md)...

# advanced.md
See [details.md](details.md)...

# details.md
Here's the actual information...
```

**Good (one level deep):**
```markdown
# SKILL.md

**Basic usage**: [in SKILL.md]
**Advanced features**: See [advanced.md](advanced.md)
**API reference**: See [reference.md](reference.md)
**Examples**: See [examples.md](examples.md)
```

## Workflows and Feedback Loops

### Workflow with Checklist

```markdown
## Research synthesis workflow

Copy this checklist:

```
- [ ] Step 1: Read all source documents
- [ ] Step 2: Identify key themes
- [ ] Step 3: Cross-reference claims
- [ ] Step 4: Create structured summary
- [ ] Step 5: Verify citations
```

**Step 1: Read all source documents**

Review each document in `sources/`. Note main arguments.
...
```

### Feedback Loop Pattern

```markdown
## Document editing process

1. Make your edits to `word/document.xml`
2. **Validate immediately**: `python scripts/validate.py unpacked_dir/`
3. If validation fails:
   - Review the error message
   - Fix the issues
   - Run validation again
4. **Only proceed when validation passes**
5. Rebuild: `python scripts/pack.py unpacked_dir/ output.docx`
```

## Common Patterns

### Template Pattern

```markdown
## Report structure

Use this template:

```markdown
# [Analysis Title]

## Executive summary
[One-paragraph overview]

## Key findings
- Finding 1 with supporting data
- Finding 2 with supporting data

## Recommendations
1. Specific actionable recommendation
2. Specific actionable recommendation
```
```

### Examples Pattern

```markdown
## Commit message format

**Example 1:**
Input: Added user authentication with JWT tokens
Output:
```
feat(auth): implement JWT-based authentication

Add login endpoint and token validation middleware
```

**Example 2:**
Input: Fixed bug where dates displayed incorrectly
Output:
```
fix(reports): correct date formatting in timezone conversion
```
```

### Conditional Workflow Pattern

```markdown
## Document modification

1. Determine the modification type:

   **Creating new content?** → Follow "Creation workflow"
   **Editing existing?** → Follow "Editing workflow"

2. Creation workflow:
   - Use docx-js library
   - Build document from scratch

3. Editing workflow:
   - Unpack existing document
   - Modify XML directly
   - Validate after each change
```

## Content Guidelines

### Avoid Time-Sensitive Information

**Bad:**
```markdown
If you're doing this before August 2025, use the old API.
```

**Good:**
```markdown
## Current method

Use the v2 API endpoint: `api.example.com/v2/messages`

## Old patterns

<details>
<summary>Legacy v1 API (deprecated 2025-08)</summary>
The v1 API used: `api.example.com/v1/messages`
</details>
```

### Use Consistent Terminology

**Good - Consistent:**
- Always "API endpoint"
- Always "field"
- Always "extract"

**Bad - Inconsistent:**
- Mix "API endpoint", "URL", "API route", "path"
- Mix "field", "box", "element", "control"

## Anti-Patterns to Avoid

### Windows-Style Paths

- **Good**: `scripts/helper.py`, `reference/guide.md`
- **Avoid**: `scripts\helper.py`, `reference\guide.md`

### Too Many Options

**Bad:**
```markdown
You can use pypdf, or pdfplumber, or PyMuPDF, or pdf2image, or...
```

**Good:**
```markdown
Use pdfplumber for text extraction:
```python
import pdfplumber
```

For scanned PDFs requiring OCR, use pdf2image with pytesseract instead.
```

## Checklist for Effective Skills

### Core Quality
- [ ] Description is specific and includes key terms
- [ ] Description includes both what and when
- [ ] SKILL.md body under 500 lines
- [ ] Additional details in separate files
- [ ] No time-sensitive information
- [ ] Consistent terminology
- [ ] Examples are concrete
- [ ] References one level deep
- [ ] Progressive disclosure used appropriately
- [ ] Workflows have clear steps

### Code and Scripts
- [ ] Scripts handle errors explicitly
- [ ] No "voodoo constants" (all values justified)
- [ ] Required packages listed
- [ ] Scripts have clear documentation
- [ ] No Windows-style paths
- [ ] Validation steps for critical operations
- [ ] Feedback loops for quality-critical tasks

### Testing
- [ ] At least three test scenarios
- [ ] Tested with Haiku, Sonnet, and Opus
- [ ] Tested with real usage scenarios
- [ ] Team feedback incorporated
