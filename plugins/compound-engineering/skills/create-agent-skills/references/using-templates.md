# スキルでテンプレートを使用する

<purpose>
テンプレートは、Claudeがコピーして入力する再利用可能な出力構造です。毎回構造を再生成することなく、一貫した高品質な出力を保証します。
</purpose>

<when_to_use>
以下の場合にテンプレートを使用します:
- 出力が呼び出し全体で一貫した構造を持つべき
- 構造が創造的生成よりも重要
- プレースホルダーを埋める方が白紙生成より信頼性が高い
- ユーザーが予測可能でプロフェッショナルな見た目の出力を期待する

一般的なテンプレートタイプ:
- **計画** - プロジェクト計画、実装計画、マイグレーション計画
- **仕様** - 技術仕様、機能仕様、API仕様
- **ドキュメント** - レポート、提案、要約
- **設定** - 設定ファイル、設定、環境セットアップ
- **雛形** - ファイル構造、ボイラープレートコード
</when_to_use>

<template_structure>
Templates live in `templates/` within the skill directory:

```
skill-name/
├── SKILL.md
├── workflows/
├── references/
└── templates/
    ├── plan-template.md
    ├── spec-template.md
    └── report-template.md
```

A template file contains:
1. Clear section markers
2. Placeholder indicators (use `{{placeholder}}` or `[PLACEHOLDER]`)
3. Inline guidance for what goes where
4. Example content where helpful
</template_structure>

<template_example>
```markdown
# {{PROJECT_NAME}} Implementation Plan

## Overview
{{1-2 sentence summary of what this plan covers}}

## Goals
- {{Primary goal}}
- {{Secondary goals...}}

## Scope
**In scope:**
- {{What's included}}

**Out of scope:**
- {{What's explicitly excluded}}

## Phases

### Phase 1: {{Phase name}}
**Duration:** {{Estimated duration}}
**Deliverables:**
- {{Deliverable 1}}
- {{Deliverable 2}}

### Phase 2: {{Phase name}}
...

## Success Criteria
- [ ] {{Measurable criterion 1}}
- [ ] {{Measurable criterion 2}}

## Risks
| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| {{Risk}} | {{H/M/L}} | {{H/M/L}} | {{Strategy}} |
```
</template_example>

<workflow_integration>
Workflows reference templates like this:

```xml
<process>
## Step 3: Generate Plan

1. Read `templates/plan-template.md`
2. Copy the template structure
3. Fill each placeholder based on gathered requirements
4. Review for completeness
</process>
```

The workflow tells Claude WHEN to use the template. The template provides WHAT structure to produce.
</workflow_integration>

<best_practices>
**Do:**
- Keep templates focused on structure, not content
- Use clear placeholder syntax consistently
- Include brief inline guidance where sections might be ambiguous
- Make templates complete but minimal

**Don't:**
- Put excessive example content that might be copied verbatim
- Create templates for outputs that genuinely need creative generation
- Over-constrain with too many required sections
- Forget to update templates when requirements change
</best_practices>
