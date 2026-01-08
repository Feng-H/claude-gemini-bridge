---
name: google
description: Call Gemini AI (Google's AI) for any task. Triggered by "/google" command OR when user explicitly asks to "Google搜索", "用Gemini检索", "让Gemini搜索", "问Gemini", "Google一下", "通过Gemini查询", "使用Gemini", "Gemini回答". Bypasses web search and provides direct AI response.
allowed-tools: gemini_chat, gemini_model_info
---

# Google Gemini Direct Call Skill

This skill provides a direct way to call Google's Gemini AI using either the `/google` slash command OR semantic recognition when you explicitly ask to search/query with Gemini.

## How to Use

### Method 1: Slash Command
```
/google [your prompt or question]
```

### Method 2: Semantic Trigger (Natural Language)

You can also trigger this skill by using natural language - no slash command needed! Just use phrases like:

**中文触发词:**
- "用 Gemini 检索..."
- "让 Gemini 搜索..."
- "问 Gemini..."
- "Google 一下..."
- "通过 Gemini 查询..."
- "使用 Gemini..."
- "让 Gemini 回答..."

**English trigger phrases:**
- "Use Gemini to search..."
- "Let Gemini query..."
- "Ask Gemini..."
- "Google this with Gemini..."
- "Through Gemini search..."
- "Use Gemini for..."

**Examples:**
```
User: 用 Gemini 检索最新的 React 19 特性
→ Automatically triggers gemini_chat

User: Let Gemini search for Python best practices
→ Automatically triggers gemini_chat

User: 问 Gemini: 什么是量子计算？
→ Automatically triggers gemini_chat
```

### Examples

```
User: /google What is the capital of France?

Claude: Let me ask Gemini for you...
[Uses gemini_chat tool]

Gemini's Response:
[The capital of France is Paris...]
```

```
User: /google Write a Python function to sort a list

Claude: I'll get Gemini to help with that...
[Uses gemini_chat tool with the prompt]

Gemini's Code:
[def sort_list(lst):
    return sorted(lst)]
```

```
User: /google Explain quantum computing

Claude: Asking Gemini to explain quantum computing...
[Uses gemini_chat tool]

Gemini's Explanation:
[Quantum computing is...]
```

## When to Use This Skill

This skill activates when you want Gemini's specific input:

**Explicit Intent Indicators:**
1. You use the `/google` slash command
2. You explicitly say "用Gemini检索" (Use Gemini to search)
3. You say "让Gemini搜索" (Let Gemini search)
4. You say "问Gemini" (Ask Gemini)
5. You say "Google一下" or "Google检索"
6. You say "通过Gemini查询" (Query through Gemini)
7. You say "使用Gemini" (Use Gemini)
8. You say "让Gemini回答" (Let Gemini answer)

**Use Cases:**
- Get Gemini's opinion specifically
- Compare with Claude's knowledge
- See how Gemini approaches a problem
- Use Gemini for specific tasks (coding, writing, analysis)
- Bypass web search and get direct AI response
- Get a second AI perspective on any topic

## Workflow

### Trigger Methods

**Method 1: Slash Command (`/google`)**
1. **Parse the prompt**: Extract everything after `/google`
2. **Call Gemini**: Use `gemini_chat` tool with the prompt
3. **Present response**: Display Gemini's response clearly
4. **Optional follow-up**: Ask if user wants Claude's analysis/comparison

**Method 2: Semantic Recognition**
1. **Detect intent**: User uses phrases like "用Gemini检索", "让Gemini搜索", "问Gemini", etc.
2. **Extract query**: Parse the search/question content from user message
3. **Call Gemini**: Use `gemini_chat` tool with the extracted query
4. **Present response**: Display Gemini's response clearly
5. **Optional follow-up**: Ask if user wants Claude's analysis/comparison

### Response Handling

For BOTH trigger methods:
- Send the user's query directly to Gemini (no modification)
- Label the response clearly as "Gemini's Response"
- Don't add Claude's own analysis unless asked
- Offer comparison/analysis if relevant

## Response Format

```markdown
## Gemini's Response:

[Gemini's full response here]

---
💡 **Tip**: Want to compare with my analysis? Just ask "compare" or "what do you think?"
```

## Advanced Usage

### With Context
```
User: /google Given that I'm using Next.js 14, how should I implement routing?

Claude: I'll ask Gemini with that context...
[Uses gemini_chat with full prompt]
```

### Multi-step Queries
```
User: /google First, explain React hooks. Then, show an example of useState.

Claude: Getting Gemini's explanation...
[Uses gemini_chat with the multi-part prompt]
```

### Code Generation
```
User: /google Write a TypeScript interface for a user object with name, email, and age

Claude: Asking Gemini to generate the TypeScript code...
[Uses gemini_chat]
```

## Best Practices

### For Users
1. **Be specific**: Clear prompts get better responses
2. **Provide context**: Gemini doesn't know our conversation history
3. **Use for comparison**: Great for getting second opinions
4. **Combine with Claude**: Ask me to analyze or compare after

### For Claude (When Executing This Skill)
1. **Direct pass-through**: Send user's prompt exactly as provided
2. **Clear attribution**: Always label it as "Gemini's Response"
3. **No modification**: Don't alter Gemini's response unless asked
4. **Offer follow-up**: Suggest comparison or analysis if relevant
5. **Quick execution**: This should be fast, no extra fluff

## Examples of Interactions

### Example 1: Simple Question
```
User: /google What is 2+2?

Claude: Let me ask Gemini...

## Gemini's Response:
2+2 equals 4.

---
💡 Want me to verify or explain this?
```

### Example 2: Code Request
```
User: /google Write a function to reverse a string in JavaScript

Claude: Getting Gemini's JavaScript solution...

## Gemini's Response:
Here's a function to reverse a string:

```javascript
function reverseString(str) {
  return str.split('').reverse().join('');
}

// Example usage:
console.log(reverseString('hello')); // 'olleh'
```

---
💡 I can explain how this works or suggest alternatives if you'd like!
```

### Example 3: Explanation
```
User: /google Explain how DNS works

Claude: Asking Gemini to explain DNS...

## Gemini's Response:
DNS (Domain Name System) is like the phonebook of the Internet...
[Full explanation]

---
💡 Want a comparison with my explanation or deeper dive into any part?
```

### Example 4: Creative Task
```
User: /google Write a haiku about coding

Claude: Getting Gemini's creative output...

## Gemini's Response:
Lines of logic flow
Bugs emerge then disappear
Digital dreams bloom

---
💡 I can write one too if you want to compare!
```

### Example 5: Web-like Query (Without Web Search!)
```
User: /google latest React features in 2024

Claude: Asking Gemini about React 2024 features...

## Gemini's Response:
React 2024 introduces several new features including...
[Information from Gemini's training data]

---
💡 Note: This is from Gemini's knowledge, not live web search.
```

### Example 6: Semantic Trigger - Chinese (Natural Language)
```
User: 用 Gemini 检索 Python 机器学习库推荐

Claude: 我来让 Gemini 为你检索...

## Gemini's Response:
以下是几个推荐的Python机器学习库...
[Full response]

---
💡 想要我的分析或对比吗？
```

### Example 7: Semantic Trigger - English (Natural Language)
```
User: Ask Gemini to explain the difference between SQL and NoSQL databases

Claude: Let me ask Gemini that for you...

## Gemini's Response:
The main differences between SQL and NoSQL databases are...
[Full explanation]

---
💡 Want me to add my perspective or compare with Gemini's answer?
```

### Example 8: Semantic Trigger with "Google一下"
```
User: Google一下 2025年最流行的编程语言

Claude: 让 Gemini 帮你搜索一下...

## Gemini's Response:
根据2025年的趋势，最流行的编程语言包括...
[Full response]

---
💡 需要我补充说明吗？
```

### Example 9: Semantic Trigger with "问Gemini"
```
User: 问 Gemini: 如何优化 React 组件性能？

Claude: 我来询问 Gemini...

## Gemini's Response:
优化React组件性能的几种方法...
[Detailed answer]

---
💡 要我提供额外的优化建议吗？
```

## Error Handling

If `gemini_chat` fails:

```markdown
## ❌ Error Calling Gemini

[Error message from tool]

**Possible causes:**
1. Gemini CLI not installed or configured
2. Network connectivity issues
3. API key problems

**Solutions:**
1. Check `gemini --version` in terminal
2. Verify Gemini CLI configuration
3. Try again or use Claude instead

---
💡 I can help you with this task using my own capabilities if you'd prefer?
```

## Comparison Workflow (Optional)

If user wants comparison after seeing Gemini's response:

```
User: /google [prompt]

[Claude shows Gemini's response]

User: What do you think?

Claude: Here's my analysis...
[Claude's response]

## Comparison:
[Quick comparison highlighting key similarities/differences]
```

## Related Skills

- `ai-compare`: For structured comparison between Claude and Gemini
- `multi-ai-orchestrator`: For complex multi-AI workflows

## Tips

1. **Quick queries**: Perfect for fast Gemini responses
2. **Learning**: See how Gemini approaches vs Claude
3. **Verification**: Cross-check facts and explanations
4. **Creative**: Get different creative perspectives
5. **Specialized**: Gemini may excel at specific tasks
6. **No web search**: This bypasses web search completely!

## Why `/google`?

The `/google` command:
- **Familiar**: Everyone knows "Google it"
- **Short**: Quick to type
- **Clear**: Unambiguous intent
- **Bypass**: Avoids triggering actual web search
- **Direct**: Goes straight to Gemini AI

## Anti-Patterns to Avoid

❌ Don't add your own commentary unless asked
❌ Don't modify Gemini's response
❌ Don't judge or critique unless requested
❌ Don't add extra steps - keep it fast
❌ Don't trigger web search when `/google` is used
❌ Don't confuse with actual Google search - this is Gemini AI

## Summary

This Google Gemini skill provides TWO ways to access Gemini AI:

### 🎯 Trigger Methods:
1. **Slash Command**: `/google [your query]`
2. **Semantic Recognition**: Natural language triggers like:
   - "用Gemini检索..." (Use Gemini to search...)
   - "让Gemini搜索..." (Let Gemini search...)
   - "问Gemini..." (Ask Gemini...)
   - "Google一下..." (Google it...)
   - "通过Gemini查询..." (Query through Gemini...)
   - "使用Gemini..." (Use Gemini...)
   - "让Gemini回答..." (Let Gemini answer...)

### ✨ Characteristics:
- **Direct**: Straight to Gemini AI, no detours
- **Clear**: Always labeled as Gemini's response
- **Fast**: No unnecessary processing
- **Flexible**: Use slash command OR natural language
- **Bypass**: Avoids web search and other tools
- **Smart**: Semantic recognition understands your intent

### 💡 Best For:
- Getting Gemini's specific input on anything
- Comparing AI perspectives
- Quick second opinions
- Bypassing web search for direct AI responses

Use it whenever you want Gemini's input - either with `/google` or just ask naturally!
