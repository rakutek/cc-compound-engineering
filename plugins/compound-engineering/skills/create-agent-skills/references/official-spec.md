# Anthropic公式スキル仕様

Source: [code.claude.com/docs/en/skills](https://code.claude.com/docs/en/skills)

## SKILL.mdファイル構造

すべてのスキルには、YAMLフロントマターとそれに続くMarkdown指示を含む`SKILL.md`ファイルが必要です。

### 基本形式

```markdown
---
name: your-skill-name
description: Brief description of what this Skill does and when to use it
---

# あなたのスキル名

## 指示
Claudeに明確で段階的なガイダンスを提供します。

## 例
このスキルを使用する具体的な例を示します。
```

## 必須フロントマターフィールド

| フィールド | 必須 | 説明 |
|-------|----------|-------------|
| `name` | はい | 小文字、数字、ハイフンのみを使用したスキル名 (最大64文字)。ディレクトリ名と一致する必要があります。 |
| `description` | はい | スキルが何をし、いつ使用するか (最大1024文字)。Claudeはこれを使用してスキルを適用するタイミングを決定します。 |
| `allowed-tools` | いいえ | このスキルがアクティブなときにClaudeが許可を求めずに使用できるツール。例: `Read, Grep, Glob` |
| `model` | いいえ | このスキルがアクティブなときに使用する特定のモデル (例: `claude-sonnet-4-20250514`)。デフォルトは会話のモデルです。 |

## スキルの場所と優先順位

```
企業 (最高優先) → 個人 → プロジェクト → プラグイン (最低優先)
```

| タイプ | パス | 適用先 |
|------|------|----------|
| **企業** | 管理設定を参照 | 組織内のすべてのユーザー |
| **個人** | `~/.claude/skills/` | あなた、すべてのプロジェクトで |
| **プロジェクト** | `.claude/skills/` | リポジトリで作業するすべての人 |
| **プラグイン** | プラグインとバンドル | プラグインをインストールしたすべての人 |

## スキルの仕組み

1. **発見**: Claudeは起動時に名前と説明のみを読み込みます
2. **アクティベーション**: あなたのリクエストがスキルの説明と一致すると、Claudeは確認を求めます
3. **実行**: Claudeはスキルの指示に従い、参照されたファイルを読み込みます

**重要な原則**: スキルは**モデル呼び出し**です — Claudeはあなたのリクエストに基づいて使用するスキルを自動的に決定します。

## 漸進的開示パターン

サポートファイルへのリンクで`SKILL.md`を500行以下に保ちます:

```
my-skill/
├── SKILL.md (required - overview and navigation)
├── reference.md (detailed API docs - loaded when needed)
├── examples.md (usage examples - loaded when needed)
└── scripts/
    └── helper.py (utility script - executed, not loaded)
```

### Example SKILL.md with References

```markdown
---
name: pdf-processing
description: Extract text, fill forms, merge PDFs. Use when working with PDF files, forms, or document extraction. Requires pypdf and pdfplumber packages.
allowed-tools: Read, Bash(python:*)
---

# PDF Processing

## Quick start

Extract text:
```python
import pdfplumber
with pdfplumber.open("doc.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

For form filling, see [FORMS.md](FORMS.md).
For detailed API reference, see [REFERENCE.md](REFERENCE.md).

## Requirements

Packages must be installed:
```bash
pip install pypdf pdfplumber
```
```

## Restricting Tool Access

```yaml
---
name: reading-files-safely
description: Read files without making changes. Use when you need read-only file access.
allowed-tools: Read, Grep, Glob
---
```

Benefits:
- Read-only Skills that shouldn't modify files
- Limited scope for specific tasks
- Security-sensitive workflows

## Writing Effective Descriptions

The `description` field enables Skill discovery and should include both what the Skill does and when to use it.

**Always write in third person.** The description is injected into the system prompt.

- **Good:** "Processes Excel files and generates reports"
- **Avoid:** "I can help you process Excel files"
- **Avoid:** "You can use this to process Excel files"

**Be specific and include key terms:**

```yaml
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
```

**Avoid vague descriptions:**

```yaml
description: Helps with documents  # Too vague!
```

## Complete Example: Commit Message Generator

```markdown
---
name: generating-commit-messages
description: Generates clear commit messages from git diffs. Use when writing commit messages or reviewing staged changes.
---

# Generating Commit Messages

## Instructions

1. Run `git diff --staged` to see changes
2. I'll suggest a commit message with:
   - Summary under 50 characters
   - Detailed description
   - Affected components

## Best practices

- Use present tense
- Explain what and why, not how
```

## Complete Example: Code Explanation Skill

```markdown
---
name: explaining-code
description: Explains code with visual diagrams and analogies. Use when explaining how code works, teaching about a codebase, or when the user asks "how does this work?"
---

# Explaining Code

When explaining code, always include:

1. **Start with an analogy**: Compare the code to something from everyday life
2. **Draw a diagram**: Use ASCII art to show the flow, structure, or relationships
3. **Walk through the code**: Explain step-by-step what happens
4. **Highlight a gotcha**: What's a common misconception?

Keep explanations conversational. For complex concepts, use multiple analogies.
```

## Distribution

- **Project Skills**: Commit `.claude/skills/` to version control
- **Plugins**: Add `skills/` directory to plugin with Skill folders
- **Enterprise**: Deploy organization-wide through managed settings
