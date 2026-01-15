---
name: {{SKILL_NAME}}
description: {{何をするか}} {{トリガー条件}}の場合に使用します。
---

<essential_principles>
## {{コアコンセプト}}

{{どのワークフローが実行されても常に適用される原則}}

### 1. {{最初の原則}}
{{説明}}

### 2. {{2番目の原則}}
{{説明}}

### 3. {{3番目の原則}}
{{説明}}
</essential_principles>

<intake>
**ユーザーに質問：**

何をしたいですか？
1. {{最初のオプション}}
2. {{2番目のオプション}}
3. {{3番目のオプション}}

**進む前に返答を待ちます。**
</intake>

<routing>
| 返答 | ワークフロー |
|----------|----------|
| 1、"{{キーワード}}" | `workflows/{{first-workflow}}.md` |
| 2、"{{キーワード}}" | `workflows/{{second-workflow}}.md` |
| 3、"{{キーワード}}" | `workflows/{{third-workflow}}.md` |

**ワークフローを読んだ後、それに正確に従います。**
</routing>

<quick_reference>
## {{スキル名}} クイックリファレンス

{{常に表示されていると便利な簡単なリファレンス情報}}
</quick_reference>

<reference_index>
## ドメイン知識

すべて`references/`内：
- {{reference-1.md}} - {{目的}}
- {{reference-2.md}} - {{目的}}
</reference_index>

<workflows_index>
## ワークフロー

すべて`workflows/`内：

| ワークフロー | 目的 |
|----------|---------|
| {{first-workflow}}.md | {{目的}} |
| {{second-workflow}}.md | {{目的}} |
| {{third-workflow}}.md | {{目的}} |
</workflows_index>

<success_criteria>
よく実行された{{スキル名}}：
- {{最初の基準}}
- {{2番目の基準}}
- {{3番目の基準}}
</success_criteria>
