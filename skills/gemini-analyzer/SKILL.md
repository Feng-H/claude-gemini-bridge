---
name: gemini-analyzer
description: Analyze code, files, or projects using Gemini's large context capabilities. Use when user mentions "analyze with Gemini", "Gemini analyze", "large file analysis", "codebase review", or wants deep analysis beyond Claude's context limits.
allowed-tools: gemini_analyze, gemini_chat
---

# Gemini Analyzer Skill

This skill provides specialized analysis capabilities using Gemini's large context window (up to 1M tokens) for tasks that exceed typical AI context limits.

## When to Use This Skill

Trigger this skill when user mentions:
- "Analyze with Gemini"
- "Gemini analyze this"
- "Large file analysis"
- "Codebase review"
- "Deep analysis"
- "Cross-file analysis"
- "Big project analysis"

## Why Use Gemini for Analysis?

**Gemini's Advantages:**
- **Massive Context**: Up to 1M tokens vs Claude's 200K tokens
- **Cross-File Analysis**: Can analyze entire codebases at once
- **Pattern Recognition**: Excellent at finding patterns across large datasets
- **Google Integration**: Strong web search and documentation retrieval

**When Gemini Shines:**
- Large files (10,000+ lines of code)
- Multi-file projects (100+ files)
- Cross-file dependency analysis
- Comprehensive codebase reviews
- Legacy system analysis

## Analysis Types

### 1. Security Analysis
```
User: "Use Gemini to analyze security vulnerabilities in this codebase"

Focus areas:
- Injection attacks (SQL, XSS, Command)
- Authentication/authorization issues
- Data exposure risks
- Cryptography weaknesses
- Dependency vulnerabilities
```

### 2. Performance Analysis
```
User: "Gemini analyze performance bottlenecks"

Focus areas:
- Algorithmic inefficiencies
- Memory leaks
- Database query optimization
- Caching opportunities
- Concurrency issues
```

### 3. Architecture Analysis
```
User: "Let Gemini review the architecture"

Focus areas:
- Design patterns
- Modularity and separation
- Scalability concerns
- Coupling and cohesion
- Technology stack choices
```

### 4. Bug Detection
```
User: "Gemini find potential bugs"

Focus areas:
- Edge cases
- Error handling gaps
- Null/undefined checks
- Race conditions
- Logic errors
```

### 5. Code Review
```
User: "Comprehensive review with Gemini"

Focus areas:
- Code quality
- Readability
- Maintainability
- Documentation
- Best practices
```

## Usage Pattern

### Step 1: Identify Large-Scale Analysis Needs
```
User: "I have a 50,000 line codebase that needs review"

Claude: This exceeds my typical context limits. Let me use Gemini's
large context capabilities for comprehensive analysis.
```

### Step 2: Call gemini_analyze Tool
```
User: "Analyze this entire project for security issues"

Claude: I'll use Gemini's large context to analyze the full codebase.

[Calls gemini_analyze with target and analysis_type]
```

### Step 3: Present and Enhance
```
Claude: Here's Gemini's comprehensive analysis:

[Gemini's detailed analysis]

My additional insights based on Gemini's findings:
- [Claude's synthesis]
- [Actionable recommendations]
- [Prioritized improvements]
```

## Best Practices

### 1. Know When to Use Gemini
```
✅ Use Gemini when:
- File > 5,000 lines
- Project > 20 files
- Need cross-file analysis
- Comprehensive review needed

❌ Don't use Gemini when:
- Simple single-file questions
- Quick clarifications needed
- Real-time collaboration required
```

### 2. Provide Context
```
Good: "Analyze the authentication system in this codebase for security issues"

Better: "Analyze the authentication system in src/auth/ for security vulnerabilities.
Focus on JWT handling, password storage, and session management.
```

### 3. Specify Analysis Type
```
Always tell Gemini what type of analysis you want:
- "security" - Vulnerabilities and risks
- "performance" - Bottlenecks and optimization
- "architecture" - Design and structure
- "bugs" - Potential defects
- "review" - Comprehensive quality check
- "general" - Broad overview
```

### 4. Follow Up on Findings
```
After Gemini's analysis:
1. Prioritize issues by severity
2. Create actionable tasks
3. Provide code examples for fixes
4. Suggest testing strategies
```

## Common Patterns

### Pattern 1: Large Codebase Security Review
```
User: "Security review of this entire project"

Claude: This is a large project. Let me use Gemini for comprehensive security analysis.

[Calls gemini_analyze with analysis_type="security"]

Gemini provides:
- Vulnerability list
- Risk severity
- Exploit scenarios
- Fix recommendations

Claude enhances:
- Prioritized action plan
- Code examples
- Testing strategies
```

### Pattern 2: Performance Optimization
```
User: "Find performance issues across all modules"

Claude: I'll use Gemini to analyze the entire codebase for performance bottlenecks.

[Calls gemini_analyze with analysis_type="performance"]

Gemini identifies:
- Slow algorithms
- Inefficient database queries
- Missing caching
- Resource leaks

Claude provides:
- Specific optimization code
- Performance benchmarks
- Implementation plan
```

### Pattern 3: Architecture Assessment
```
User: "Review the architecture of this microservices system"

Claude: Let me use Gemini to analyze the complete system architecture.

[Calls gemini_analyze with analysis_type="architecture"]

Gemini evaluates:
- Service boundaries
- Communication patterns
- Data flow
- Scalability

Claude synthesizes:
- Architecture scorecard
- Improvement recommendations
- Migration path
```

## Tips for Users

### Effective Prompts
```
✅ Good:
"Use Gemini to analyze src/ for security vulnerabilities"

❌ Less Effective:
"Analyze this"
```

### Combine with Claude's Strengths
```
1. Use Gemini for: Large-scale analysis, pattern finding
2. Use Claude for: Code generation, implementation, testing
3. Combine: Gemini analysis → Claude implementation
```

### Iterative Refinement
```
First pass: "Gemini analyze the codebase generally"
Follow-up: "Focus on authentication system"
Final: "Show me specific fixes for these issues"
```

## Limitations

### Be Aware Of:
- Gemini doesn't have conversation context (stateless)
- Each call is independent
- No access to Claude's ongoing analysis
- May need multiple calls for very large projects

### When Not to Use:
- Single small files (< 1000 lines)
- Quick syntax questions
- Real-time code generation
- Interactive debugging

## Example Interactions

### Example 1: Legacy Code Analysis
```
User: "I have this legacy codebase from 2010, can Gemini analyze it?"

Claude: Absolutely! Gemini's large context is perfect for analyzing legacy systems.
Let me have it review the entire codebase.

[Calls gemini_analyze]

Gemini provides comprehensive analysis of:
- Outdated patterns
- Security risks
- Refactoring opportunities
- Modernization path

Claude adds: "Based on Gemini's analysis, here's a modernization plan..."
```

### Example 2: Cross-File Bug Detection
```
User: "Find bugs that span multiple files"

Claude: This requires cross-file analysis. Let me use Gemini.

[Calls gemini_analyze with analysis_type="bugs"]

Gemini identifies:
- Race conditions in async operations
- Inconsistent error handling
- State synchronization issues

Claude: "Gemini found 12 cross-file bugs. Let me prioritize them..."
```

### Example 3: Comprehensive Review
```
User: "Full project review before launch"

Claude: I'll use Gemini for a comprehensive review across all dimensions.

[Calls gemini_analyze with analysis_type="review"]

Gemini provides:
- Security assessment
- Performance analysis
- Architecture evaluation
- Code quality metrics

Claude synthesizes: "Here's your launch readiness report..."
```

## Related Skills

- `ai-compare`: Compare Claude and Gemini responses
- `multi-ai-orchestrator`: Complex multi-AI workflows
- `gemini-helper`: Basic Gemini interactions

## Feedback Loop

After using this skill:
1. Was Gemini's analysis useful?
2. Did it find issues Claude missed?
3. Was the analysis too general or too specific?
4. Should we adjust the analysis prompts?

Continuously refine based on project needs and results.
