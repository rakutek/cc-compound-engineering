<golden_rule>
最小限のコンテキストでスキルを誰かに見せ、指示に従ってもらいましょう。もし彼らが混乱したら、Claudeもおそらく混乱します。
</golden_rule>

<overview>
明確さと直接性は、効果的なスキル作成の基礎です。明確な指示はエラーを減らし、実行品質を向上させ、トークンの無駄を最小限に抑えます。
</overview>

<guidelines>
<contextual_information>
Claudeにタスクのフレームとなる文脈情報を与えます:

- タスクの結果が何に使用されるか
- 出力が誰を対象としているか
- タスクがどのワークフローの一部であるか
- 最終目標または成功した完了が何を意味するか

コンテキストはClaudeがより良い判断を下し、より適切な出力を生成するのに役立ちます。

<example>
```xml
<context>
この分析は、透明性と実行可能な洞察を重視する投資家に提示されます。財務指標と明確な推奨事項に焦点を当ててください。
</context>
```
</example>
</contextual_information>

<specificity>
Claudeに何をしてほしいか具体的に伝えましょう。コードのみが必要で他に何もいらない場合は、そう言いましょう。

**曖昧**: "レポートを手伝って"
**具体的**: "Executive Summary、Key Findings、Recommendationsの3つのセクションを持つMarkdownレポートを生成してください"

**曖昧**: "データを処理して"
**具体的**: "CSVファイルから顧客名とメールアドレスを抽出し、重複を削除してJSON形式で保存してください"

具体性は曖昧さを排除し、反復サイクルを減らします。
</specificity>

<sequential_steps>
指示を順次ステップとして提供します。番号付きリストまたは箇条書きを使用します。

```xml
<workflow>
1. ソースファイルからデータを抽出
2. ターゲット形式に変換
3. 変換を検証
4. 出力ファイルに保存
5. 出力の正確性を確認
</workflow>
```

順次ステップは明確な期待値を作成し、Claudeが重要な操作をスキップする可能性を減らします。
</sequential_steps>
</guidelines>

<example_comparison>
<unclear_example>
```xml
<quick_start>
これらの顧客フィードバックメッセージから個人を特定できる情報をすべて削除してください: {{FEEDBACK_DATA}}
</quick_start>
```

**問題点**:
- 何がPIIとしてカウントされるか?
- PIIは何に置き換えるべきか?
- 出力はどのフォーマットにすべきか?
- PIIが見つからない場合はどうするか?
- 製品名は削除すべきか?
</unclear_example>

<clear_example>
```xml
<objective>
四半期レビュープレゼンテーションのために顧客フィードバックを匿名化します。
</objective>

<quick_start>
<instructions>
1. すべての顧客名を"CUSTOMER_[ID]"に置き換える (例: "Jane Doe" → "CUSTOMER_001")
2. メールアドレスを"EMAIL_[ID]@example.com"に置き換える
3. 電話番号を"PHONE_[ID]"として削除する
4. メッセージが特定の製品(例: "AcmeCloud")に言及している場合は、そのままにします
5. PIIが見つからない場合は、メッセージをそのままコピーします
6. 処理されたメッセージのみを出力し、"---"で区切ります
</instructions>

処理するデータ: {{FEEDBACK_DATA}}
</quick_start>

<success_criteria>
- すべての顧客名がIDに置き換えられている
- すべてのメールと電話が削除されている
- 製品名が保持されている
- 出力形式が仕様に一致している
</success_criteria>
```

**Why this is better**:
- States the purpose (quarterly review)
- Provides explicit step-by-step rules
- Defines output format clearly
- Specifies edge cases (product names, no PII found)
- Defines success criteria
</clear_example>
</example_comparison>

<key_differences>
The clear version:
- States the purpose (quarterly review)
- Provides explicit step-by-step rules
- Defines output format
- Specifies edge cases (product names, no PII found)
- Includes success criteria

The unclear version leaves all these decisions to Claude, increasing the chance of misalignment with expectations.
</key_differences>

<show_dont_just_tell>
<principle>
When format matters, show an example rather than just describing it.
</principle>

<telling_example>
```xml
<commit_messages>
Generate commit messages in conventional format with type, scope, and description.
</commit_messages>
```
</telling_example>

<showing_example>
```xml
<commit_message_format>
Generate commit messages following these examples:

<example number="1">
<input>Added user authentication with JWT tokens</input>
<output>
```
feat(auth): implement JWT-based authentication

Add login endpoint and token validation middleware
```
</output>
</example>

<example number="2">
<input>Fixed bug where dates displayed incorrectly in reports</input>
<output>
```
fix(reports): correct date formatting in timezone conversion

Use UTC timestamps consistently across report generation
```
</output>
</example>

Follow this style: type(scope): brief description, then detailed explanation.
</commit_message_format>
```
</showing_example>

<why_showing_works>
Examples communicate nuances that text descriptions can't:
- Exact formatting (spacing, capitalization, punctuation)
- Tone and style
- Level of detail
- Pattern across multiple cases

Claude learns patterns from examples more reliably than from descriptions.
</why_showing_works>
</show_dont_just_tell>

<avoid_ambiguity>
<principle>
Eliminate words and phrases that create ambiguity or leave decisions open.
</principle>

<ambiguous_phrases>
❌ **"Try to..."** - Implies optional
✅ **"Always..."** or **"Never..."** - Clear requirement

❌ **"Should probably..."** - Unclear obligation
✅ **"Must..."** or **"May optionally..."** - Clear obligation level

❌ **"Generally..."** - When are exceptions allowed?
✅ **"Always... except when..."** - Clear rule with explicit exceptions

❌ **"Consider..."** - Should Claude always do this or only sometimes?
✅ **"If X, then Y"** or **"Always..."** - Clear conditions
</ambiguous_phrases>

<example>
❌ **Ambiguous**:
```xml
<validation>
You should probably validate the output and try to fix any errors.
</validation>
```

✅ **Clear**:
```xml
<validation>
Always validate output before proceeding:

```bash
python scripts/validate.py output_dir/
```

If validation fails, fix errors and re-validate. Only proceed when validation passes with zero errors.
</validation>
```
</example>
</avoid_ambiguity>

<define_edge_cases>
<principle>
Anticipate edge cases and define how to handle them. Don't leave Claude guessing.
</principle>

<without_edge_cases>
```xml
<quick_start>
Extract email addresses from the text file and save to a JSON array.
</quick_start>
```

**Questions left unanswered**:
- What if no emails are found?
- What if the same email appears multiple times?
- What if emails are malformed?
- What JSON format exactly?
</without_edge_cases>

<with_edge_cases>
```xml
<quick_start>
Extract email addresses from the text file and save to a JSON array.

<edge_cases>
- **No emails found**: Save empty array `[]`
- **Duplicate emails**: Keep only unique emails
- **Malformed emails**: Skip invalid formats, log to stderr
- **Output format**: Array of strings, one email per element
</edge_cases>

<example_output>
```json
[
  "user1@example.com",
  "user2@example.com"
]
```
</example_output>
</quick_start>
```
</with_edge_cases>
</define_edge_cases>

<output_format_specification>
<principle>
When output format matters, specify it precisely. Show examples.
</principle>

<vague_format>
```xml
<output>
Generate a report with the analysis results.
</output>
```
</vague_format>

<specific_format>
```xml
<output_format>
Generate a markdown report with this exact structure:

```markdown
# Analysis Report: [Title]

## Executive Summary
[1-2 paragraphs summarizing key findings]

## Key Findings
- Finding 1 with supporting data
- Finding 2 with supporting data
- Finding 3 with supporting data

## Recommendations
1. Specific actionable recommendation
2. Specific actionable recommendation

## Appendix
[Raw data and detailed calculations]
```

**Requirements**:
- Use exactly these section headings
- Executive summary must be 1-2 paragraphs
- List 3-5 key findings
- Provide 2-4 recommendations
- Include appendix with source data
</output_format>
```
</specific_format>
</output_format_specification>

<decision_criteria>
<principle>
When Claude must make decisions, provide clear criteria.
</principle>

<no_criteria>
```xml
<workflow>
Analyze the data and decide which visualization to use.
</workflow>
```

**Problem**: What factors should guide this decision?
</no_criteria>

<with_criteria>
```xml
<workflow>
Analyze the data and select appropriate visualization:

<decision_criteria>
**Use bar chart when**:
- Comparing quantities across categories
- Fewer than 10 categories
- Exact values matter

**Use line chart when**:
- Showing trends over time
- Continuous data
- Pattern recognition matters more than exact values

**Use scatter plot when**:
- Showing relationship between two variables
- Looking for correlations
- Individual data points matter
</decision_criteria>
</workflow>
```

**Benefits**: Claude has objective criteria for making the decision rather than guessing.
</with_criteria>
</decision_criteria>

<constraints_and_requirements>
<principle>
Clearly separate "must do" from "nice to have" from "must not do".
</principle>

<unclear_requirements>
```xml
<requirements>
The report should include financial data, customer metrics, and market analysis. It would be good to have visualizations. Don't make it too long.
</requirements>
```

**Problems**:
- Are all three content types required?
- Are visualizations optional or required?
- How long is "too long"?
</unclear_requirements>

<clear_requirements>
```xml
<requirements>
<must_have>
- Financial data (revenue, costs, profit margins)
- Customer metrics (acquisition, retention, lifetime value)
- Market analysis (competition, trends, opportunities)
- Maximum 5 pages
</must_have>

<nice_to_have>
- Charts and visualizations
- Industry benchmarks
- Future projections
</nice_to_have>

<must_not>
- Include confidential customer names
- Exceed 5 pages
- Use technical jargon without definitions
</must_not>
</requirements>
```

**Benefits**: Clear priorities and constraints prevent misalignment.
</clear_requirements>
</constraints_and_requirements>

<success_criteria>
<principle>
Define what success looks like. How will Claude know it succeeded?
</principle>

<without_success_criteria>
```xml
<objective>
Process the CSV file and generate a report.
</objective>
```

**Problem**: When is this task complete? What defines success?
</without_success_criteria>

<with_success_criteria>
```xml
<objective>
Process the CSV file and generate a summary report.
</objective>

<success_criteria>
- All rows in CSV successfully parsed
- No data validation errors
- Report generated with all required sections
- Report saved to output/report.md
- Output file is valid markdown
- Process completes without errors
</success_criteria>
```

**Benefits**: Clear completion criteria eliminate ambiguity about when the task is done.
</with_success_criteria>
</success_criteria>

<testing_clarity>
<principle>
Test your instructions by asking: "Could I hand these instructions to a junior developer and expect correct results?"
</principle>

<testing_process>
1. Read your skill instructions
2. Remove context only you have (project knowledge, unstated assumptions)
3. Identify ambiguous terms or vague requirements
4. Add specificity where needed
5. Test with someone who doesn't have your context
6. Iterate based on their questions and confusion

If a human with minimal context struggles, Claude will too.
</testing_process>
</testing_clarity>

<practical_examples>
<example domain="data_processing">
❌ **Unclear**:
```xml
<quick_start>
Clean the data and remove bad entries.
</quick_start>
```

✅ **Clear**:
```xml
<quick_start>
<data_cleaning>
1. Remove rows where required fields (name, email, date) are empty
2. Standardize date format to YYYY-MM-DD
3. Remove duplicate entries based on email address
4. Validate email format (must contain @ and domain)
5. Save cleaned data to output/cleaned_data.csv
</data_cleaning>

<success_criteria>
- No empty required fields
- All dates in YYYY-MM-DD format
- No duplicate emails
- All emails valid format
- Output file created successfully
</success_criteria>
</quick_start>
```
</example>

<example domain="code_generation">
❌ **Unclear**:
```xml
<quick_start>
Write a function to process user input.
</quick_start>
```

✅ **Clear**:
```xml
<quick_start>
<function_specification>
Write a Python function with this signature:

```python
def process_user_input(raw_input: str) -> dict:
    """
    Validate and parse user input.

    Args:
        raw_input: Raw string from user (format: "name:email:age")

    Returns:
        dict with keys: name (str), email (str), age (int)

    Raises:
        ValueError: If input format is invalid
    """
```

**Requirements**:
- Split input on colon delimiter
- Validate email contains @ and domain
- Convert age to integer, raise ValueError if not numeric
- Return dictionary with specified keys
- Include docstring and type hints
</function_specification>

<success_criteria>
- Function signature matches specification
- All validation checks implemented
- Proper error handling for invalid input
- Type hints included
- Docstring included
</success_criteria>
</quick_start>
```
</example>
</practical_examples>
