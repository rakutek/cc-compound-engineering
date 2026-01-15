# 推奨スキル構造

複雑なスキルの最適な構造は、ルーティング、ワークフロー、および知識を分離します。

<structure>
```
skill-name/
├── SKILL.md              # ルーター + 不可欠な原則 (不可避)
├── workflows/            # 段階的な手順 (方法)
│   ├── workflow-a.md
│   ├── workflow-b.md
│   └── ...
└── references/           # ドメイン知識 (何か)
    ├── reference-a.md
    ├── reference-b.md
    └── ...
```
</structure>

<why_this_works>
## これが解決する問題

**問題1: コンテキストがスキップされる**
重要な原則が別ファイルにあると、Claudeはそれらを読まない可能性があります。
**解決策:** 不可欠な原則をSKILL.mdに直接置きます。自動的に読み込まれます。

**問題2: 間違ったコンテキストが読み込まれる**
"ビルド"タスクがデバッグ参照を読み込みます。"デバッグ"タスクがビルド参照を読み込みます。
**解決策:** 受付質問が意図を決定 → 特定のワークフローにルーティング → ワークフローが読むべき参照を指定。

**問題3: モノリシックなスキルは圧倒的**
500以上の混合コンテンツ行は関連部分を見つけるのを難しくします。
**解決策:** 小さなルーター (SKILL.md) + 焦点を絞ったワークフロー + 参照ライブラリ。

**問題4: 手順と知識が混在**
"Xをする方法"と"Xが何を意味するか"が混在すると混乱が生じます。
**解決策:** ワークフローは手順 (ステップ)。参照は知識 (パターン、例)。
</why_this_works>

<skill_md_template>
## SKILL.md Template

```markdown
---
name: skill-name
description: What it does and when to use it.
---

<essential_principles>
## How This Skill Works

[Inline principles that apply to ALL workflows. Cannot be skipped.]

### Principle 1: [Name]
[Brief explanation]

### Principle 2: [Name]
[Brief explanation]
</essential_principles>

<intake>
**Ask the user:**

What would you like to do?
1. [Option A]
2. [Option B]
3. [Option C]
4. Something else

**Wait for response before proceeding.**
</intake>

<routing>
| Response | Workflow |
|----------|----------|
| 1, "keyword", "keyword" | `workflows/option-a.md` |
| 2, "keyword", "keyword" | `workflows/option-b.md` |
| 3, "keyword", "keyword" | `workflows/option-c.md` |
| 4, other | Clarify, then select |

**After reading the workflow, follow it exactly.**
</routing>

<reference_index>
All domain knowledge in `references/`:

**Category A:** file-a.md, file-b.md
**Category B:** file-c.md, file-d.md
</reference_index>

<workflows_index>
| Workflow | Purpose |
|----------|---------|
| option-a.md | [What it does] |
| option-b.md | [What it does] |
| option-c.md | [What it does] |
</workflows_index>
```
</skill_md_template>

<workflow_template>
## Workflow Template

```markdown
# Workflow: [Name]

<required_reading>
**Read these reference files NOW:**
1. references/relevant-file.md
2. references/another-file.md
</required_reading>

<process>
## Step 1: [Name]
[What to do]

## Step 2: [Name]
[What to do]

## Step 3: [Name]
[What to do]
</process>

<success_criteria>
This workflow is complete when:
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3
</success_criteria>
```
</workflow_template>

<when_to_use_this_pattern>
## When to Use This Pattern

**Use router + workflows + references when:**
- Multiple distinct workflows (build vs debug vs ship)
- Different workflows need different references
- Essential principles must not be skipped
- Skill has grown beyond 200 lines

**Use simple single-file skill when:**
- One workflow
- Small reference set
- Under 200 lines total
- No essential principles to enforce
</when_to_use_this_pattern>

<key_insight>
## The Key Insight

**SKILL.md is always loaded. Use this guarantee.**

Put unavoidable content in SKILL.md:
- Essential principles
- Intake question
- Routing logic

Put workflow-specific content in workflows/:
- Step-by-step procedures
- Required references for that workflow
- Success criteria for that workflow

Put reusable knowledge in references/:
- Patterns and examples
- Technical details
- Domain expertise
</key_insight>
