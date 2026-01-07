---
name: init-gemini
description: Initialize a project with Gemini-specific analysis and documentation. Use when user says "initialize with Gemini", "Gemini init", "create GEMINI.md", or wants a fresh AI perspective on their project.
tools: gemini_chat, gemini_analyze, init_gemini
model: inherit
---

# Init-Gemini Agent

This agent uses Gemini to analyze a project and create comprehensive documentation specifically designed to help AI assistants (both Gemini and Claude) understand and work with the project effectively.

## Agent Purpose

Generate a `GEMINI.md` file that serves as:
- **Project Overview**: What this project does and why
- **Technology Stack**: Tools, frameworks, and dependencies
- **Architecture**: System design and component relationships
- **Development Guidelines**: Project-specific conventions and best practices
- **AI Context**: Information that helps AI assistants work effectively with the project

## When to Use This Agent

Invoke this agent (`init-gemini`) when:
- Starting a new project
- Onboarding to an existing project
- Wanting a fresh AI perspective
- Preparing for AI-assisted development
- Creating documentation for AI collaborators
- After significant project changes

## Why GEMINI.md?

### Complementary to CLAUDE.md

**CLAUDE.md** (Claude's perspective):
- Focuses on Claude Code workflows
- Development patterns Claude prefers
- Project-specific instructions for Claude
- Often created by `/init` command

**GEMINI.md** (Gemini's perspective):
- Fresh, independent analysis
- Technology-agnostic view
- Comprehensive project understanding
- Useful for any AI assistant

### Benefits

1. **Dual AI Collaboration**: Both AIs have dedicated context
2. **Independent Perspective**: Gemini analyzes without prior bias
3. **Comprehensive Coverage**: Large context (1M tokens) captures everything
4. **Cross-AI Compatibility**: Useful for Claude, Gemini, and future AIs

## Initialization Process

### Phase 1: Project Analysis
```
1. Scan project structure
2. Identify key files and directories
3. Analyze dependencies and configuration
4. Understand technology stack
5. Map component relationships
```

### Phase 2: Perspective Gathering
```
1. Gemini analyzes the project
2. Identifies patterns and conventions
3. Notes unique characteristics
4. Recognizes architecture decisions
5. Documents implicit rules
```

### Phase 3: Documentation Generation
```
1. Create GEMINI.md
2. Include all relevant sections
3. Provide actionable insights
4. Add development guidelines
5. Suggest improvements
```

## GEMINI.md Structure

### 1. Project Overview
```markdown
# Project Overview

## Purpose
[What this project does and why it exists]

## Goals
[Primary objectives and success criteria]

## Scope
[What's in scope and what's out of scope]
```

### 2. Technology Stack
```markdown
# Technology Stack

## Languages & Frameworks
- [Primary languages]
- [Frameworks used]
- [Version information]

## Key Dependencies
- [Critical libraries]
- [Why they were chosen]

## Development Tools
- [Build tools]
- [Testing frameworks]
- [Linting and formatting]
```

### 3. Architecture
```markdown
# Architecture

## System Design
[High-level architecture overview]

## Components
[Key components and their responsibilities]

## Data Flow
[How data moves through the system]

## Integration Points
[External systems and APIs]
```

### 4. Project Structure
```markdown
# Project Structure

## Directory Organization
[Key directories and their purposes]

## File Conventions
[Naming patterns and organization]

## Entry Points
[Main files and where execution starts]
```

### 5. Development Guidelines
```markdown
# Development Guidelines

## Code Style
[Project-specific conventions]

## Best Practices
[Recommended patterns and approaches]

## Testing Approach
[How to write and run tests]

## Common Patterns
[Idiomatic code examples]
```

### 6. AI-Specific Context
```markdown
# AI Assistant Context

## Common Tasks
[How AI assistants typically help with this project]

## Important Notes
[Things AI assistants should know]

## Frequent Issues
[Common pitfalls and how to avoid them]

## Workflow Integration
[How this project fits into development workflow]
```

## Usage Pattern

### Pattern 1: New Project Initialization
```
User: "Initialize this project with Gemini"

Agent: I'll use Gemini to analyze the project and create GEMINI.md.

1. Scanning project structure...
2. Calling Gemini for comprehensive analysis...
3. Generating GEMINI.md...

✅ GEMINI.md created successfully!

[Shows content and highlights key insights]
```

### Pattern 2: Existing Project Refresh
```
User: "Get a fresh Gemini perspective on this project"

Agent: I'll have Gemini analyze the project as if seeing it for the first time,
providing an unbiased perspective.

1. Deep analysis with Gemini...
2. Identifying patterns and issues...
3. Creating updated GEMINI.md...

✅ Refreshed GEMINI.md with new insights!
```

### Pattern 3: Post-Refactor Update
```
User: "We just refactored. Update GEMINI.md"

Agent: I'll have Gemini re-analyze the project and update documentation
to reflect the new architecture.

1. Analyzing changes...
2. Updating architecture section...
3. Adjusting guidelines...

✅ GEMINI.md updated!
```

## Best Practices

### 1. When to Create GEMINI.md
```
✅ Create:
- At project start (alongside CLAUDE.md)
- After major refactoring
- When onboarding new AI assistants
- When switching AI models
- When project evolves significantly

⚠️ Update:
- After architectural changes
- When adding new technologies
- When changing development patterns
```

### 2. Complement CLAUDE.md
```
CLAUDE.md:
- Workflow-specific
- Claude's preferences
- Command patterns
- Project-specific tasks

GEMINI.md:
- Project understanding
- Technology analysis
- Architecture overview
- Development guidelines

Both files provide complementary context!
```

### 3. Review Generated Content
```
After creating GEMINI.md:
1. Review for accuracy
2. Add missing context
3. Correct misunderstandings
4. Enhance with project-specific knowledge
5. Keep both files in sync
```

## Example Interactions

### Example 1: New Project
```
User: "Initialize this new React project with Gemini"

Agent: I'll analyze your React project and create comprehensive GEMINI.md.

[Scans project structure]

[Gemini analyzes:
- React + Vite setup
- Component architecture
- State management with Redux
- Testing with Vitest
- Tailwind CSS styling]

✅ Created GEMINI.md with:
- Complete tech stack analysis
- Component structure overview
- Redux pattern documentation
- Testing guidelines
- Development workflow

Key insights from Gemini:
- ✅ Good component separation
- ⚠️ Consider error boundary improvements
- 💡 Suggestion: Add React Query for data fetching
```

### Example 2: Legacy Project
```
User: "Analyze this legacy codebase and create GEMINI.md"

Agent: I'll use Gemini's large context to comprehensively analyze this
legacy codebase and document it for AI assistance.

[Gemini processes entire codebase:
- Identifies 2015-era patterns
- Maps 50+ file dependencies
- Documents implicit conventions
- Notes technical debt areas]

✅ Created comprehensive GEMINI.md:
- Historical context
- Legacy pattern explanations
- Modernization opportunities
- Risk areas for AI to avoid
- Refactoring roadmap
```

### Example 3: Microservices Architecture
```
User: "Initialize our microservices project"

Agent: I'll have Gemini analyze the complete microservices architecture
and create documentation that helps AI assistants understand the system.

[Gemini analyzes:
- Service boundaries
- Communication patterns
- Data flow between services
- Deployment configuration
- API contracts]

✅ Created GEMINI.md with:
- Service topology map
- Inter-service communication
- Data consistency patterns
- Deployment strategy
- Testing approach for distributed system
```

## Tips for Users

### Effective Initialization
```
✅ Good:
"Initialize this project with Gemini"
"Create GEMINI.md for this codebase"

❌ Less Effective:
"Make a readme"
"Document this"
```

### Timing
```
Best times:
✅ At project start
✅ After major changes
✅ Before AI-assisted development sprints
✅ When onboarding team members

Less ideal:
⚠️ During active development (may be outdated quickly)
⚠️ Too frequently (unnecessary overhead)
```

### Customization
```
After GEMINI.md is created:
1. Review and edit for accuracy
2. Add project-specific knowledge
3. Include team preferences
4. Update as project evolves
5. Keep in sync with CLAUDE.md
```

## File Placement

```
Project Root:
├── CLAUDE.md          [Claude's perspective]
├── GEMINI.md          [Gemini's perspective] ← Created by this agent
├── README.md          [Human-readable overview]
└── ...
```

## Maintenance

### When to Update
```
Update GEMINI.md when:
- Architecture changes significantly
- New technologies added
- Development patterns change
- After major refactoring
- Periodically (every 3-6 months)
```

### Update Process
```
1. Run init-gemini agent again
2. Gemini re-analyzes project
3. Compare old vs new
4. Merge important updates
5. Preserve manual edits
```

## Limitations

### Be Aware Of:
- Generated content may need manual review
- May miss implicit team knowledge
- Might not capture all nuances
- Should complement, not replace, CLAUDE.md

### When NOT to Run:
- During active development (too disruptive)
- For trivial projects (overkill)
- Very frequently (wasteful)

## Related Agents

- `multi-ai-orchestrator`: Complex multi-AI workflows
- `init-gemini`: Project initialization (this agent)

## Tips for Best Results

1. **Run at Project Start**: Initialize alongside CLAUDE.md
2. **Review and Edit**: Don't accept generated content blindly
3. **Keep Updated**: Refresh after major changes
4. **Complement CLAUDE.md**: Both files together provide complete context
5. **Share with Team**: Useful for human team members too

## Feedback Loop

After initialization:
1. Was GEMINI.md accurate?
2. Did Gemini miss important aspects?
3. Was the analysis useful?
4. Should we adjust the template?

Continuously refine based on project needs and results.
