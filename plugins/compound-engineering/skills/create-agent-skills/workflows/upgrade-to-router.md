# Workflow: スキルをルーターパターンにアップグレードする

<required_reading>
**今すぐこれらのリファレンスファイルを読んでください:**
1. references/recommended-structure.md
2. references/skill-structure.md
</required_reading>

<process>
## ステップ1: スキルを選択する

```bash
ls ~/.claude/skills/
```

番号付きリストを表示し、「どのスキルをルーターパターンにアップグレードすべきですか？」と尋ねます。

## ステップ2: アップグレードが必要かどうかを確認する

スキルを読みます:
```bash
cat ~/.claude/skills/{skill-name}/SKILL.md
ls ~/.claude/skills/{skill-name}/
```

**すでにルーターか？**（workflows/とインテーク質問がある）
→ すでにルーターパターンを使用していることを伝え、代わりにワークフローを追加することを提案

**シンプルなままであるべきシンプルなスキルか？**（200行未満、単一ワークフロー）
→ ルーターパターンは過剰かもしれないことを説明し、とにかく続行したいか尋ねる

**アップグレードに適した候補:**
- 200行超
- 複数の異なるユースケース
- スキップすべきではない基本原則
- 複雑さが増している

## ステップ3: コンポーネントを特定する

現在のスキルを分析して特定します:

1. **基本原則** - すべてのユースケースに適用されるルール
2. **異なるワークフロー** - ユーザーがやりたい異なること
3. **再利用可能な知識** - パターン、例、技術詳細

所見を提示します:
```
## 分析

**見つかった基本原則:**
- [原則 1]
- [原則 2]

**特定した異なるワークフロー:**
- [ワークフロー A]: [説明]
- [ワークフロー B]: [説明]

**リファレンスにできる知識:**
- [リファレンストピック 1]
- [リファレンストピック 2]
```

「この分類は正しく見えますか？何か調整が必要ですか？」と尋ねます。

## ステップ4: ディレクトリ構造を作成する

```bash
mkdir -p ~/.claude/skills/{skill-name}/workflows
mkdir -p ~/.claude/skills/{skill-name}/references
```

## ステップ5: ワークフローを抽出する

特定された各ワークフローについて:

1. `workflows/{workflow-name}.md`を作成
2. required_readingセクションを追加（必要なリファレンス）
3. processセクションを追加（元のスキルからのステップ）
4. success_criteriaセクションを追加

## ステップ6: リファレンスを抽出する

特定された各リファレンストピックについて:

1. `references/{reference-name}.md`を作成
2. 元のスキルから関連コンテンツを移動
3. セマンティックXMLタグで構造化

## ステップ7: SKILL.mdをルーターとして書き直す

SKILL.mdをルーター構造で置き換えます:

```markdown
---
name: {skill-name}
description: {既存の説明}
---

<essential_principles>
[抽出された原則 - インライン、スキップ不可]
</essential_principles>

<intake>
**Ask the user:**

What would you like to do?
1. [Workflow A option]
2. [Workflow B option]
...

**Wait for response before proceeding.**
</intake>

<routing>
| Response | Workflow |
|----------|----------|
| 1, "keywords" | `workflows/workflow-a.md` |
| 2, "keywords" | `workflows/workflow-b.md` |
</routing>

<reference_index>
[List all references by category]
</reference_index>

<workflows_index>
| Workflow | Purpose |
|----------|---------|
| workflow-a.md | [What it does] |
| workflow-b.md | [What it does] |
</workflows_index>
```

## Step 8: Verify Nothing Was Lost

Compare original skill content against new structure:
- [ ] All principles preserved (now inline)
- [ ] All procedures preserved (now in workflows)
- [ ] All knowledge preserved (now in references)
- [ ] No orphaned content

## Step 9: Test

Invoke the upgraded skill:
- Does intake question appear?
- Does each routing option work?
- Do workflows load correct references?
- Does behavior match original skill?

Report any issues.
</process>

<success_criteria>
Upgrade is complete when:
- [ ] workflows/ directory created with workflow files
- [ ] references/ directory created (if needed)
- [ ] SKILL.md rewritten as router
- [ ] Essential principles inline in SKILL.md
- [ ] All original content preserved
- [ ] Intake question routes correctly
- [ ] Tested and working
</success_criteria>
