---
name: ai-compare
description: Compare responses from Claude and Gemini AI models. Use when user asks to "compare", "get both opinions", "ask both AIs", or wants different perspectives on the same question.
allowed-tools: gemini_chat
---

# AI Response Comparison Skill

This skill provides a structured framework for comparing responses from Claude and Gemini AI models.

## When to Use This Skill

Trigger this skill when user mentions:
- "Compare Claude and Gemini"
- "Ask both AIs"
- "Get different perspectives"
- "What does Gemini think?"
- "Second opinion"
- "Compare AI responses"

## Comparison Framework

### Step 1: Understand the Task
First, clarify what the user wants to compare:
- Factual accuracy?
- Different approaches?
- Writing style?
- Code quality?
- Creative solutions?

### Step 2: Get Claude's Response
Provide Claude's analysis first:
```
## Claude's Analysis:

[Your detailed response to the prompt]
```

### Step 3: Get Gemini's Response
Use the `gemini_chat` or `gemini_compare` tool:
```
## Gemini's Analysis:

[Response from Gemini]
```

### Step 4: Structured Comparison
Use this template:

```markdown
## Comparison Summary

### Similarities:
- [Points where both AIs agree]
- [Common approaches]
- [Shared conclusions]

### Differences:
| Aspect | Claude | Gemini |
|--------|--------|--------|
| Approach | [Claude's method] | [Gemini's method] |
| Key Points | [Main points] | [Main points] |
| Strengths | [What Claude did well] | [What Gemini did well] |
| Weaknesses | [Limitations] | [Limitations] |

### Best of Both:
[Combine strengths from both responses]

### Recommendation:
[Which approach to use and why]
```

## Comparison Dimensions

### For Coding Questions:
- **Correctness**: Does the code work?
- **Best Practices**: Following language conventions
- **Efficiency**: Performance considerations
- **Readability**: Code clarity and documentation
- **Error Handling**: Edge cases covered

### For Explanations:
- **Clarity**: How easy to understand
- **Depth**: Level of detail
- **Accuracy**: Factual correctness
- **Examples**: Practical illustrations
- **Structure**: Logical organization

### For Creative Tasks:
- **Originality**: Unique ideas
- **Creativity**: Novel approaches
- **Quality**: Overall excellence
- **Relevance**: Matches requirements
- **Appeal**: Engagement factor

### For Analysis:
- **Thoroughness**: Coverage of aspects
- **Insights**: Deep understanding
- **Evidence**: Supporting details
- **Logic**: Reasoning quality
- **Conclusions**: Validity of findings

## Best Practices

### 1. Be Objective
- Don't favor one AI over the other
- Focus on actual differences in responses
- Base comparison on content, not preferences

### 2. Be Specific
- Quote exact responses when relevant
- Point to specific differences
- Provide concrete examples

### 3. Be Constructive
- Highlight strengths of both
- Note weaknesses gently
- Suggest improvements for both

### 4. Be Transparent
- Clearly indicate which response is from which AI
- Explain your comparison criteria
- Acknowledge subjective judgments

### 5. Add Value
- Don't just present both responses
- Synthesize insights from both
- Provide actionable recommendations
- Help user understand trade-offs

## Common Patterns

### Pattern 1: Quick Comparison
```
User: "Compare both on X"

1. Provide Claude's answer
2. Call gemini_chat for Gemini's answer
3. Quick 3-point comparison:
   - Main similarity
   - Key difference
   - Recommendation
```

### Pattern 2: Detailed Analysis
```
User: "Thoroughly compare both approaches"

1. Provide detailed Claude response
2. Get Gemini's detailed response
3. Use full comparison framework
4. Provide synthesis and recommendation
```

### Pattern 3: Verification
```
User: "Do both AIs agree on this?"

1. Get both responses
2. Focus on agreement/disagreement
3. Highlight consensus points
4. Investigate disagreements
```

### Pattern 4: Complementary Use
```
User: "Use both to solve this"

1. Claude: Analytical approach
2. Gemini: Creative approach
3. Combine both perspectives
4. Create comprehensive solution
```

## Example Interactions

### Example 1: Code Comparison
```
User: "Compare how Claude and Gemini would implement a binary search tree"

Claude: [Provides Claude's BST implementation]
"Now let me get Gemini's approach:"
[Calls gemini_chat]
## Comparison:
- Both use recursive approach
- Claude includes more error handling
- Gemini uses more modern ES6+ syntax
- Best of both: Combine error handling + modern syntax
```

### Example 2: Explanation Comparison
```
User: "Do both AIs explain React hooks the same way?"

Claude: [Claude's explanation]
[Gemini's response via gemini_compare]
## Comparison:
✅ Both agree on core concepts
⚠️ Different emphasis:
  - Claude: Practical usage patterns
  - Gemini: Theoretical underpinnings
💡 Use Claude for implementation, Gemini for deep understanding
```

### Example 3: Creative Task
```
User: "Compare blog post ideas about AI"

Claude: [List of ideas]
[Gemini's ideas via gemini_chat]
## Comparison:
Claude: More technical, dev-focused
Gemini: More business, strategy-focused
Best: Combine for comprehensive content strategy
```

## Tips for Better Comparisons

### For Users:
1. **Be Specific**: Clear questions get better comparisons
2. **Define Criteria**: What matters to you?
3. **Provide Context**: Help both AIs understand
4. **Ask Follow-ups**: Dive deeper into differences

### For Claude:
1. **Set Expectations**: Tell user what you'll compare
2. **Use Tools**: Leverage gemini_compare when appropriate
3. **Stay Neutral**: Objective comparison over preference
4. **Synthesize**: Don't just present, analyze and combine

## Advanced Techniques

### Weighted Scoring
For quantitative comparisons:
```
| Criterion | Weight | Claude | Gemini |
|-----------|--------|--------|--------|
| Accuracy  | 40%    | 9/10   | 8/10   |
| Clarity   | 30%    | 8/10   | 9/10   |
| Speed     | 20%    | 7/10   | 9/10   |
| Depth     | 10%    | 9/10   | 7/10   |
| **Total** | **100%** | **8.4** | **8.5** |
```

### Scenario-Based Comparison
```
"For learning: Use Claude's response [reasons]"
"For production: Use Gemini's response [reasons]"
"For quick reference: Use [recommendation]"
```

### Synthesis Approach
```
## Comprehensive Answer:

Combining both AIs' insights:

1. [Claude's strong point]
2. [Gemini's unique contribution]
3. [Integrated solution]
```

## Limitations

### Be Aware Of:
- Gemini doesn't have conversation context
- Each AI has different training data
- Responses vary by model version
- Some questions may not need comparison
- Comparison takes more time

### When NOT to Compare:
- Simple factual questions (one correct answer)
- Very similar expected responses
- Time-critical situations
- User only wants one opinion
- Task is trivial/comparison not valuable

## Related Skills

- `ai-orchestrator`: For complex multi-AI workflows
- `gemini-helper`: For Gemini-specific guidance
- `prompt-engineering`: For better prompts to both AIs

## Feedback Loop

After each comparison:
1. What was most useful to the user?
2. Which comparison format worked best?
3. Should you adjust comparison dimensions?
4. Any patterns in user preferences?

Continuously refine your comparison approach based on user feedback and results.
