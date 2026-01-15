# Workflow: 既存のスキルにリファレンスを追加する

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

番号付きリストを表示し、「どのスキルに新しいリファレンスが必要ですか？」と尋ねます。

## ステップ2: 現在の構造を分析する

```bash
cat ~/.claude/skills/{skill-name}/SKILL.md
ls ~/.claude/skills/{skill-name}/references/ 2>/dev/null
```

以下を判断します:
- **references/ フォルダがあるか？** → 良好、直接追加できます
- **シンプルなスキルか？** → 最初にreferences/ を作成する必要があるかもしれません
- **どのようなリファレンスが存在するか？** → 知識の全体像を理解します

現在のリファレンスをユーザーに報告します。

## ステップ3: リファレンス要件を収集する

以下を尋ねます:
- このリファレンスにはどのような知識を含める必要がありますか？
- どのワークフローで使用されますか？
- これは複数のワークフローで再利用可能ですか、それとも1つに特化していますか？

**1つのワークフローに特化している場合** → 代わりにそのワークフローにインラインで入れることを検討してください。

## ステップ4: リファレンスファイルを作成する

`references/{reference-name}.md` を作成します:

セマンティックなXMLタグを使用してコンテンツを構造化します:
```xml
<overview>
このリファレンスが何をカバーするかの簡単な説明
</overview>

<patterns>
## 共通パターン
[再利用可能なパターン、例、コードスニペット]
</patterns>

<guidelines>
## ガイドライン
[ベストプラクティス、ルール、制約]
</guidelines>

<examples>
## 例
[説明付きの具体的な例]
</examples>
```

## ステップ5: SKILL.mdを更新する

新しいリファレンスを`<reference_index>`に追加します:
```markdown
**Category:** existing.md, new-reference.md
```

## ステップ6: 必要なワークフローを更新する

このリファレンスを使用すべき各ワークフローについて:

1. ワークフローファイルを読む
2. その`<required_reading>`セクションに追加する
3. この追加でワークフローがまだ意味をなすことを確認する

## ステップ7: 検証する

- [ ] リファレンスファイルが存在し、適切に構造化されている
- [ ] リファレンスがSKILL.mdのreference_indexにある
- [ ] 関連するワークフローのrequired_readingにある
- [ ] 壊れたリファレンスがない
</process>

<success_criteria>
リファレンスの追加は以下が完了したときに完了です:
- [ ] 有用なコンテンツを含むリファレンスファイルが作成された
- [ ] SKILL.mdのreference_indexに追加された
- [ ] 関連するワークフローがそれを読むように更新された
- [ ] コンテンツが再利用可能である（ワークフロー固有ではない）
</success_criteria>
