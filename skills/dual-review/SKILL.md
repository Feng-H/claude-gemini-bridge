---
name: dual-review
description: Coordinate dual AI code review where both Claude and Gemini independently analyze code, then Claude synthesizes insights. Use when user says "dual review", "both AIs review", "comprehensive review", or wants maximum code quality assurance.
allowed-tools: gemini_analyze, gemini_chat
---

# Dual AI Review Skill

This skill orchestrates comprehensive code reviews using both Claude and Gemini AI models independently, then synthesizes their insights for maximum code quality assurance.

## When to Use This Skill

Trigger this skill when user mentions:
- "Dual review"
- "Both AIs review"
- "Comprehensive review"
- "Maximum quality check"
- "Get both opinions on this code"
- "Cross-check with Gemini"

## Why Dual Review?

**Independent Analysis Benefits:**
- **Different Perspectives**: Each AI has different training and strengths
- **Reduced Blind Spots**: What one misses, the other catches
- **Cross-Validation**: Consensus increases confidence
- **Comprehensive Coverage**: Broader range of issues identified
- **Quality Assurance**: Higher confidence in code quality

**When Dual Review Shines:**
- Critical security code
- Production releases
- Complex algorithms
- Architecture decisions
- High-risk features
- Compliance requirements

## Review Process

### Phase 1: Claude's Independent Review
```
1. Claude analyzes code first
2. Identifies issues from Claude's perspective
3. Notes Claude's unique insights
4. Documents Claude's recommendations
```

### Phase 2: Gemini's Independent Review
```
1. Gemini analyzes same code independently
2. Uses large context for cross-file analysis
3. Identifies issues from Gemini's perspective
4. Documents Gemini's recommendations
```

### Phase 3: Synthesis and Comparison
```
1. Claude compares both reviews
2. Identifies agreements (consensus)
3. Identifies disagreements (different perspectives)
4. Prioritizes issues by severity and consensus
5. Provides unified recommendations
```

## Review Dimensions

### 1. Security Review
```
Claude focuses on:
- Input validation
- Output encoding
- Authentication/authorization
- Session management
- Cryptography

Gemini focuses on:
- OWASP Top 10
- Known vulnerabilities
- Dependency risks
- Configuration security
- Attack surface

Synthesis:
- Combined security findings
- Prioritized by risk level
- Comprehensive fix recommendations
```

### 2. Performance Review
```
Claude focuses on:
- Algorithmic complexity
- Memory efficiency
- Code-level optimizations
- Database queries
- Caching strategy

Gemini focuses on:
- System-level bottlenecks
- Scalability concerns
- Resource utilization
- Concurrency issues
- Network efficiency

Synthesis:
- Full-stack performance analysis
- Optimization priorities
- Before/after metrics
```

### 3. Architecture Review
```
Claude focuses on:
- Design patterns
- Code organization
- Modularity
- Interface design
- SOLID principles

Gemini focuses on:
- System architecture
- Component relationships
- Data flow
- Technology choices
- Architectural patterns

Synthesis:
- Complete architecture assessment
- Refactoring recommendations
- Improvement roadmap
```

### 4. Code Quality Review
```
Claude focuses on:
- Readability
- Maintainability
- Naming conventions
- Documentation
- Test coverage

Gemini focuses on:
- Code complexity
- Duplication
- Dead code
- Code smells
- Anti-patterns

Synthesis:
- Quality scorecard
- Specific improvements
- Refactoring suggestions
```

## Usage Pattern

### Pattern 1: Full Code Review
```
User: "Dual review this file before we commit"

Claude: I'll perform an independent review first, then get Gemini's perspective,
and finally synthesize both reviews.

## Claude's Review:
[Claude's detailed analysis]

Now getting Gemini's independent review...

[Calls gemini_analyze]

## Gemini's Review:
[Gemini's detailed analysis]

## Synthesis:
### Consensus (Both AIs agree):
- [List of agreed-upon issues]

### Differences (Different perspectives):
- [Claude's view] vs [Gemini's view]

### Prioritized Action Items:
1. [Critical - Both agree]
2. [High - One AI, other agrees]
3. [Medium - Valuable but not critical]

### Recommendations:
[Unified recommendations]
```

### Pattern 2: Security-Focused Review
```
User: "Both AIs review security"

Claude: I'll lead security review, get Gemini's security analysis,
and provide comprehensive security assessment.

[Claude's security review]
+ [Gemini's security review]
= [Comprehensive security report]
```

### Pattern 3: Architecture Review
```
User: "Dual review the architecture"

Claude: Let me review from code perspective, get Gemini's system
perspective, and synthesize architectural insights.

[Code-level architecture]
+ [System-level architecture]
= [Complete architecture review]
```

## Best Practices

### 1. Clear Scope
```
✅ Good: "Dual review the authentication module"
❌ Vague: "Review everything"
```

### 2. Specify Focus
```
✅ Good: "Dual review for security and performance"
❌ Broad: "Comprehensive review"
```

### 3. Provide Context
```
✅ Good: "This is payment processing code, needs dual security review"
❌ No context: "Review this"
```

### 4. Action-Oriented
```
After review, provide:
- Prioritized fix list
- Code examples for improvements
- Testing recommendations
- Documentation updates needed
```

## Comparison Framework

### Agreement Levels
```
🔴 CRITICAL: Both AIs identify as critical
🟡 HIGH: One AI critical, other agrees it's important
🟢 MEDIUM: One AI identifies, other neutral
⚪ LOW: One AI identifies, other disagrees
```

### Consensus Categories
```
✅ Full Consensus: Both AIs agree on issue and solution
⚠️ Partial Consensus: Both see issue, different solutions
❌ Disagreement: Different perspectives on validity
🤷 Unique Finding: Only one AI identified the issue
```

## Example Interactions

### Example 1: Critical Security Code
```
User: "This is our payment processing code. Dual review please."

Claude: This is critical code. Let me do a thorough security review,
then get Gemini's independent analysis.

## Claude's Security Review:
- SQL injection risk in payment_query()
- Missing input validation on amount
- Insufficient logging for audit trail

Now getting Gemini's review...

[Calls gemini_analyze with analysis_type="security"]

## Gemini's Security Review:
- SQL injection confirmed
- Additional: Timing attack vulnerability
- Additional: Insufficient rate limiting
- Additional: Weak encryption key management

## Synthesis:
### 🔴 CRITICAL (Full Consensus):
1. SQL injection - FIX IMMEDIATELY
2. Input validation - ADD BEFORE DEPLOY

### 🟡 HIGH (Gemini identified, critical):
3. Timing attack vulnerability
4. Rate limiting needed

### ⚠️ Action Plan:
[Detailed remediation steps]
```

### Example 2: Performance Optimization
```
User: "Dual review for performance issues"

Claude: I'll analyze algorithmic efficiency, get Gemini's system-level
analysis, and provide comprehensive optimization plan.

## Claude's Performance Review:
- O(n²) algorithm in search function
- Inefficient database queries
- Missing caching layer

## Gemini's Performance Review:
- Confirms algorithmic issues
- Additional: Memory leak in connection pool
- Additional: Unnecessary re-renders in UI
- Additional: Network request optimization

## Synthesis:
### Performance Optimization Plan:
1. Algorithm fix (both agree)
2. Database optimization (both agree)
3. Memory leak fix (Gemini found)
4. Caching strategy (both agree)
5. UI optimization (Gemini found)

Expected improvement: 60% faster, 40% less memory
```

### Example 3: Comprehensive Pre-Release Review
```
User: "Full dual review before release"

Claude: I'll conduct comprehensive review across all dimensions,
get Gemini's independent analysis, and provide release readiness assessment.

## Review Dimensions:
1. Security: [Both AIs' findings]
2. Performance: [Both AIs' findings]
3. Architecture: [Both AIs' findings]
4. Code Quality: [Both AIs' findings]

## Release Readiness Score:
Security: 8/10 (Both agree good)
Performance: 6/10 (Both see issues)
Architecture: 7/10 (Minor disagreements)
Quality: 9/10 (Both excellent)

Overall: 7.5/10 - Ready with fixes

### Required Before Release:
- [Critical] Fix performance bottleneck
- [High] Add error handling
- [Medium] Improve documentation

### Optional Improvements:
- [Low] Refactor module X
- [Low] Add more tests

### Recommendation:
✅ Ready for release after addressing critical issues
```

## Tips for Users

### When to Request Dual Review
```
✅ Good times:
- Before merging to main branch
- Before production deployment
- For critical security code
- For complex algorithms
- For architecture changes
- For compliance verification

⚠️ Consider:
- Development time (takes 2-3x longer)
- Is the code critical enough?
- Are there simpler alternatives?

❌ Avoid:
- Trivial changes
- Documentation updates
- Test file changes
```

### Effective Dual Review Requests
```
✅ Good:
"Dual review the authentication module for security"
"Both AIs review performance of this API endpoint"
"Comprehensive dual review before production release"

❌ Less Effective:
"Review this"
"Check for bugs"
```

### Interpreting Results
```
🔴 Full Consensus (CRITICAL):
- Both AIs agree it's critical
- Fix immediately
- High confidence in severity

🟡 High Priority:
- One AI critical, other agrees important
- Fix soon
- Medium-high confidence

🟢 Medium Priority:
- Valuable improvements
- Fix when possible
- Lower confidence

⚪ Consider:
- Nice to have
- Fix if time permits
- Low confidence
```

## Limitations

### Time Considerations
- Dual review takes 2-3x longer than single review
- Consider if code is critical enough
- Use for high-risk or high-value code

### Overhead
- More complex process
- Requires synthesis time
- May identify non-critical issues

### When NOT to Use
- Simple, low-risk code
- Trivial changes
- Time-critical situations
- Experimental code

## Related Skills

- `gemini-analyzer`: Large-scale analysis with Gemini
- `ai-compare`: Quick comparison of responses
- `multi-ai-orchestrator`: Complex multi-AI workflows

## Feedback Loop

After dual review:
1. Was the extra time worth it?
2. Did both AIs find valuable issues?
3. Were disagreements resolved well?
4. Should we adjust the process?

Continuously refine based on project needs and results.
