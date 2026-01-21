<overview>
コアプリンシプルはスキル作成の決定を導きます。これらの原則は、スキルが異なるモデルやユースケースで効率的、効果的、かつ保守可能であることを保証します。
</overview>

<xml_structure_principle>
<description>
スキルは一貫した解析、効率的なトークン使用、および向上したClaudeパフォーマンスのために純粋XML構造を使用します。
</description>

<why_xml>
<consistency>
XMLはすべてのスキルで一貫した構造を強制します。すべてのスキルは同じ目的に同じタグ名を使用します:
- `<objective>`は常にスキルが何をするかを定義します
- `<quick_start>`は常に即座のガイダンスを提供します
- `<success_criteria>`は常に完了を定義します

この一貫性により、スキルは予測可能で保守が容易になります。
</consistency>

<parseability>
XMLは明確な境界と意味的な意味を提供します。Claudeは確実に:
- セクションの境界を特定します (コンテンツがどこで始まり終わるか)
- コンテンツの目的を理解します (各セクションが果たす役割)
- 関連性のないセクションをスキップします (漸進的開示)
- プログラムで解析します (検証ツールが構造をチェックできる)

Markdownの見出しは単なる視覚的フォーマットです。Claudeは見出しテキストから意味を推測する必要があり、これは信頼性が低いです。
</parseability>

<token_efficiency>
XMLタグはmarkdownの見出しより効率的です:

**Markdownの見出し**:
```markdown
## Quick start
## Workflow
## Advanced features
## Success criteria
```
合計: 約20トークン、Claudeにとって意味的な意味なし

**XMLタグ**:
```xml
<quick_start>
<workflow>
<advanced_features>
<success_criteria>
```
合計: 約15トークン、意味的な意味が組み込まれている

節約はエコシステム全体のすべてのスキルで複合します。
</token_efficiency>

<claude_performance>
Claudeは純粋XMLでより良いパフォーマンスを発揮します:
- 明確なセクション境界が解析エラーを減らす
- 意味的なタグが意図を直接伝える (推測不要)
- ネストされたタグが明確な階層を作成する
- スキル全体で一貫した構造が認知負荷を減らす
- 漸進的開示がより確実に機能する

純粋XML構造は単なるスタイルの好みではありません—これはパフォーマンスの最適化です。
</claude_performance>
</why_xml>

<critical_rule>
**スキル本文コンテンツからすべてのMarkdown見出し (#, ##, ###) を削除してください。** 意味的なXMLタグに置き換えます。コンテンツ内のMarkdownフォーマット (太字、斜体、リスト、コードブロック、リンク) は保持します。
</critical_rule>

<required_tags>
すべてのスキルに必須:
- `<objective>` - スキルが何をし、なぜ重要か
- `<quick_start>` - 即座の、実行可能なガイダンス
- `<success_criteria>` または `<when_successful>` - 機能したかを知る方法
</required_tags>
</xml_structure_principle>

<conciseness_principle>
<description>
コンテキストウィンドウは共有されます。あなたのスキルは、システムプロンプト、会話履歴、他のスキルのメタデータ、および実際のリクエストとコンテキストウィンドウを共有します。
</description>

<guidance>
Claudeがまだ持っていないコンテキストのみを追加します。各情報に問いかけます:
- "Claudeは本当にこの説明が必要か?"
- "Claudeがこれを知っていると仮定できるか?"
- "この段落はそのトークンコストを正当化できるか?"

Claudeが賢いと仮定します。明白な概念を説明しないでください。
</guidance>

<concise_example>
**Concise** (~50 tokens):
```xml
<quick_start>
Extract PDF text with pdfplumber:

```python
import pdfplumber

with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```
</quick_start>
```

**Verbose** (~150 tokens):
```xml
<quick_start>
PDF files are a common file format used for documents. To extract text from them, we'll use a Python library called pdfplumber. First, you'll need to import the library, then open the PDF file using the open method, and finally extract the text from each page. Here's how to do it:

```python
import pdfplumber

with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

This code opens the PDF and extracts text from the first page.
</quick_start>
```

The concise version assumes Claude knows what PDFs are, understands Python imports, and can read code. All those assumptions are correct.
</concise_example>

<when_to_elaborate>
Add explanation when:
- Concept is domain-specific (not general programming knowledge)
- Pattern is non-obvious or counterintuitive
- Context affects behavior in subtle ways
- Trade-offs require judgment

Don't add explanation for:
- Common programming concepts (loops, functions, imports)
- Standard library usage (reading files, making HTTP requests)
- Well-known tools (git, npm, pip)
- Obvious next steps
</when_to_elaborate>
</conciseness_principle>

<degrees_of_freedom_principle>
<description>
Match the level of specificity to the task's fragility and variability. Give Claude more freedom for creative tasks, less freedom for fragile operations.
</description>

<high_freedom>
<when>
- Multiple approaches are valid
- Decisions depend on context
- Heuristics guide the approach
- Creative solutions welcome
</when>

<example>
```xml
<objective>
Review code for quality, bugs, and maintainability.
</objective>

<workflow>
1. Analyze the code structure and organization
2. Check for potential bugs or edge cases
3. Suggest improvements for readability and maintainability
4. Verify adherence to project conventions
</workflow>

<success_criteria>
- All major issues identified
- Suggestions are actionable and specific
- Review balances praise and criticism
</success_criteria>
```

Claude has freedom to adapt the review based on what the code needs.
</example>
</high_freedom>

<medium_freedom>
<when>
- A preferred pattern exists
- Some variation is acceptable
- Configuration affects behavior
- Template can be adapted
</when>

<example>
```xml
<objective>
Generate reports with customizable format and sections.
</objective>

<report_template>
Use this template and customize as needed:

```python
def generate_report(data, format="markdown", include_charts=True):
    # Process data
    # Generate output in specified format
    # Optionally include visualizations
```
</report_template>

<success_criteria>
- Report includes all required sections
- Format matches user preference
- Data accurately represented
</success_criteria>
```

Claude can customize the template based on requirements.
</example>
</medium_freedom>

<low_freedom>
<when>
- Operations are fragile and error-prone
- Consistency is critical
- A specific sequence must be followed
- Deviation causes failures
</when>

<example>
```xml
<objective>
Run database migration with exact sequence to prevent data loss.
</objective>

<workflow>
Run exactly this script:

```bash
python scripts/migrate.py --verify --backup
```

**Do not modify the command or add additional flags.**
</workflow>

<success_criteria>
- Migration completes without errors
- Backup created before migration
- Verification confirms data integrity
</success_criteria>
```

Claude must follow the exact command with no variation.
</example>
</low_freedom>

<matching_specificity>
The key is matching specificity to fragility:

- **Fragile operations** (database migrations, payment processing, security): Low freedom, exact instructions
- **Standard operations** (API calls, file processing, data transformation): Medium freedom, preferred pattern with flexibility
- **Creative operations** (code review, content generation, analysis): High freedom, heuristics and principles

Mismatched specificity causes problems:
- Too much freedom on fragile tasks → errors and failures
- Too little freedom on creative tasks → rigid, suboptimal outputs
</matching_specificity>
</degrees_of_freedom_principle>

<model_testing_principle>
<description>
Skills act as additions to models, so effectiveness depends on the underlying model. What works for Opus might need more detail for Haiku.
</description>

<testing_across_models>
Test your skill with all models you plan to use:

<haiku_testing>
**Claude Haiku** (fast, economical)

Questions to ask:
- Does the skill provide enough guidance?
- Are examples clear and complete?
- Do implicit assumptions become explicit?
- Does Haiku need more structure?

Haiku benefits from:
- More explicit instructions
- Complete examples (no partial code)
- Clear success criteria
- Step-by-step workflows
</haiku_testing>

<sonnet_testing>
**Claude Sonnet** (balanced)

Questions to ask:
- Is the skill clear and efficient?
- Does it avoid over-explanation?
- Are workflows well-structured?
- Does progressive disclosure work?

Sonnet benefits from:
- Balanced detail level
- XML structure for clarity
- Progressive disclosure
- Concise but complete guidance
</sonnet_testing>

<opus_testing>
**Claude Opus** (powerful reasoning)

Questions to ask:
- Does the skill avoid over-explaining?
- Can Opus infer obvious steps?
- Are constraints clear?
- Is context minimal but sufficient?

Opus benefits from:
- Concise instructions
- Principles over procedures
- High degrees of freedom
- Trust in reasoning capabilities
</opus_testing>
</testing_across_models>

<balancing_across_models>
Aim for instructions that work well across all target models:

**Good balance**:
```xml
<quick_start>
Use pdfplumber for text extraction:

```python
import pdfplumber
with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

For scanned PDFs requiring OCR, use pdf2image with pytesseract instead.
</quick_start>
```

This works for all models:
- Haiku gets complete working example
- Sonnet gets clear default with escape hatch
- Opus gets enough context without over-explanation

**Too minimal for Haiku**:
```xml
<quick_start>
Use pdfplumber for text extraction.
</quick_start>
```

**Too verbose for Opus**:
```xml
<quick_start>
PDF files are documents that contain text. To extract that text, we use a library called pdfplumber. First, import the library at the top of your Python file. Then, open the PDF file using the pdfplumber.open() method. This returns a PDF object. Access the pages attribute to get a list of pages. Each page has an extract_text() method that returns the text content...
</quick_start>
```
</balancing_across_models>

<iterative_improvement>
1. Start with medium detail level
2. Test with target models
3. Observe where models struggle or succeed
4. Adjust based on actual performance
5. Re-test and iterate

Don't optimize for one model. Find the balance that works across your target models.
</iterative_improvement>
</model_testing_principle>

<progressive_disclosure_principle>
<description>
SKILL.md serves as an overview. Reference files contain details. Claude loads reference files only when needed.
</description>

<token_efficiency>
Progressive disclosure keeps token usage proportional to task complexity:

- Simple task: Load SKILL.md only (~500 tokens)
- Medium task: Load SKILL.md + one reference (~1000 tokens)
- Complex task: Load SKILL.md + multiple references (~2000 tokens)

Without progressive disclosure, every task loads all content regardless of need.
</token_efficiency>

<implementation>
- Keep SKILL.md under 500 lines
- Split detailed content into reference files
- Keep references one level deep from SKILL.md
- Link to references from relevant sections
- Use descriptive reference file names

See [skill-structure.md](skill-structure.md) for progressive disclosure patterns.
</implementation>
</progressive_disclosure_principle>

<validation_principle>
<description>
Validation scripts are force multipliers. They catch errors that Claude might miss and provide actionable feedback.
</description>

<characteristics>
Good validation scripts:
- Provide verbose, specific error messages
- Show available valid options when something is invalid
- Pinpoint exact location of problems
- Suggest actionable fixes
- Are deterministic and reliable

See [workflows-and-validation.md](workflows-and-validation.md) for validation patterns.
</characteristics>
</validation_principle>

<principle_summary>
<xml_structure>
Use pure XML structure for consistency, parseability, and Claude performance. Required tags: objective, quick_start, success_criteria.
</xml_structure>

<conciseness>
Only add context Claude doesn't have. Assume Claude is smart. Challenge every piece of content.
</conciseness>

<degrees_of_freedom>
Match specificity to fragility. High freedom for creative tasks, low freedom for fragile operations, medium for standard work.
</degrees_of_freedom>

<model_testing>
Test with all target models. Balance detail level to work across Haiku, Sonnet, and Opus.
</model_testing>

<progressive_disclosure>
Keep SKILL.md concise. Split details into reference files. Load reference files only when needed.
</progressive_disclosure>

<validation>
Make validation scripts verbose and specific. Catch errors early with actionable feedback.
</validation>
</principle_summary>
