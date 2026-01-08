---
name: google
description: Explicitly call Gemini AI (Google's AI) for any task. Use when user says "/google" or wants Gemini's response specifically. Bypasses web search and other tools.
allowed-tools: gemini_chat, gemini_model_info
---

# Google Gemini Direct Call Skill

This skill provides a direct way to call Google's Gemini AI using the `/google` slash command.

## How to Use

### Slash Command
```
/google [your prompt or question]
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

Use the `/google` command when you want to:
1. Get Gemini's opinion specifically
2. Compare with your own knowledge
3. See how Gemini approaches a problem
4. Use Gemini for specific tasks (coding, writing, analysis)
5. Bypass web search and get direct AI response

## Workflow

When user invokes `/google`:

1. **Parse the prompt**: Extract everything after `/google`
2. **Call Gemini**: Use `gemini_chat` tool with the prompt
3. **Present response**: Display Gemini's response clearly
4. **Optional follow-up**: Ask if user wants Claude's analysis/comparison

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

The `/google` command is:
- **Direct**: Straight to Gemini AI, no detours
- **Clear**: Always labeled as Gemini's response
- **Fast**: No unnecessary processing
- **Optional**: Easy to ask for Claude's input afterward
- **Bypass**: Avoids web search and other tools
- **Simple**: Just "/google" followed by your question

Use it whenever you want Gemini's specific input on anything!
