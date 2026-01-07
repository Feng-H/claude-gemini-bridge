# CLAUDE.md

This file provides guidance to Claude Code when working with the claude-gemini-bridge project.

## Project Overview

**claude-gemini-bridge** is a comprehensive multi-AI collaboration toolkit that enables seamless integration between Claude Code and Google's Gemini AI through three layers:

1. **MCP Server** (`gemini-mcp-server/`) - Core infrastructure providing Gemini API access
2. **Skills** (`ai-orchestrator/.claude/skills/`) - Knowledge packages for best practices
3. **Agents** (`ai-orchestrator/.claude/agents/`) - Complex multi-AI task orchestration

**Project Goal**: Allow users to leverage both Claude and Gemini AI models within Claude Code, either individually or in collaboration.

## Project Structure

```
claude-gemini-bridge/
├── gemini-mcp-server/              # MCP Server (Infrastructure Layer)
│   ├── src/index.js               # Server implementation with 3 tools
│   ├── package.json               # NPM dependencies
│   ├── README.md                  # MCP Server documentation
│   ├── CLAUDE.md                  # Claude Code usage guide
│   └── .gitignore
│
├── ai-orchestrator/               # Application Layer
│   ├── .claude/
│   │   ├── skills/
│   │   │   └── ai-compare/        # AI response comparison skill
│   │   │       └── SKILL.md
│   │   └── agents/
│   │       └── multi-ai-orchestrator/  # Multi-AI collaboration agent
│   │           └── AGENT.md
│   └── examples/
│       ├── QUICKSTART.md          # 5-minute setup guide
│       ├── example-1-basic-usage.md
│       ├── example-2-orchestration.md
│       └── example-3-research.md
│
├── README.md                       # Project overview
├── PROJECT_SUMMARY.md              # Completion summary
└── CLAUDE.md                       # This file
```

## Component Responsibilities

### MCP Server (gemini-mcp-server)

**Purpose**: Provides tools to call Gemini CLI from Claude Code

**Key Files**:
- `src/index.js` - Main server with 2 tools:
  1. `gemini_chat` - Send prompt to Gemini, get response
  2. `gemini_model_info` - Get Gemini CLI and model info

**Tech Stack**:
- Node.js with @modelcontextprotocol/sdk
- Child process to execute `gemini` CLI command
- Stdio transport for MCP protocol

**Configuration**:
- Must be added to `~/.claude/settings.json` or project `.claude/settings.local.json`
- Command: `node /path/to/gemini-mcp-server/src/index.js`

**Dependencies**: Requires `@modelcontextprotocol/sdk` (run `npm install`)

### AI Compare Skill (ai-compare)

**Purpose**: Provides structured framework for comparing Claude and Gemini responses

**Location**: `ai-orchestrator/.claude/skills/ai-compare/SKILL.md`

**Trigger Keywords**:
- "compare", "both AIs", "ask both", "second opinion"
- "what does Gemini think"

**Capabilities**:
- 5 comparison dimensions (coding, explanation, creative, analysis, testing)
- 4 common patterns (quick, detailed, verification, complementary)
- Weighted scoring and synthesis methods

### Multi-AI Orchestrator Agent

**Purpose**: Orchestrates complex multi-AI collaboration tasks

**Location**: `ai-orchestrator/.claude/agents/multi-ai-orchestrator/AGENT.md`

**Invocation**: `/multi-ai-orchestrator` or `/multi-ai`

**Collaboration Patterns**:
1. **Sequential** (🔁) - Pipeline, one AI's output feeds into next
2. **Parallel** (⚡) - Both AIs work simultaneously on different aspects
3. **Competitive** (🏆) - Both AIs solve same problem independently
4. **Collaborative** (🤝) - Joint integrated solution
5. **Supervisor-Worker** (👔) - Claude orchestrates, Gemini executes

**Workflow**: 5 phases - Analysis → Strategy → Execution → Integration → Review

## Development Guidelines

### When Working on MCP Server

**Common Tasks**:

1. **Add a new tool**:
   ```javascript
   // In src/index.js, add to tools array in ListToolsRequestSchema handler
   {
     name: 'new_tool',
     description: 'What it does',
     inputSchema: {
       type: 'object',
       properties: {
         param1: { type: 'string', description: 'Description' }
       },
       required: ['param1']
     }
   }

   // Then add case in CallToolRequestSchema handler
   case 'new_tool': {
     // Implementation
   }
   ```

2. **Test MCP Server**:
   ```bash
   cd gemini-mcp-server
   npm start
   ```

3. **View debug logs**: MCP Server logs to stderr

**Important**:
- Always handle errors gracefully with try-catch
- Use 60-second timeout for Gemini CLI calls
- Return structured error messages in content

### When Working on Skills

**Common Tasks**:

1. **Modify existing skill**: Edit `SKILL.md` file directly
2. **Create new skill**: Create new directory under `.claude/skills/`

**Skill Structure**:
```yaml
---
name: skill-name
description: Keywords for auto-triggering
allowed-tools: tool1, tool2  # Optional
---

# Skill Content

## Instructions
## Examples
## Best Practices
```

**Important**:
- `description` field is crucial for auto-triggering
- Keep instructions clear and step-by-step
- Provide concrete examples
- Include use cases and anti-patterns

### When Working on Agents

**Common Tasks**:

1. **Modify existing agent**: Edit `AGENT.md` file directly
2. **Create new agent**: Create new directory under `.claude/agents/`

**Agent Structure**:
```yaml
---
name: agent-name
description: When to use this agent
allowed-tools: tool1, tool2
---

# Agent Content

## Agent Purpose
## When to Use
## Workflow/Patterns
## Task Templates
## Best Practices
```

**Important**:
- Agents are for complex, multi-step tasks
- Focus on orchestration and coordination
- Provide clear phase-by-phase workflows
- Include progress reporting and exit criteria

### When Working on Documentation

**Documentation Files**:
- `README.md` - User-facing overview
- `CLAUDE.md` - Claude Code guidance (like this file)
- `PROJECT_SUMMARY.md` - Project completion summary
- `examples/*.md` - Practical usage examples

**Guidelines**:
- Keep examples up-to-date with code
- Provide both conceptual and practical information
- Include troubleshooting sections
- Use clear formatting (tables, code blocks, emojis)

## Common Development Tasks

### Task 1: Test MCP Server Connection

```bash
# 1. Ensure dependencies installed
cd gemini-mcp-server && npm install

# 2. Test Gemini CLI works
gemini --version

# 3. Start MCP Server manually to test
npm start

# 4. In Claude Code, test tool:
# "使用 gemini_model_info 查看模型信息"
```

### Task 2: Add New Collaboration Pattern

Edit `ai-orchestrator/.claude/agents/multi-ai-orchestrator/AGENT.md`:

```markdown
### Pattern X: [Name] [emoji]

**Best for:**
- [Use case 1]
- [Use case 2]

**Example:**
[Detailed example]
```

### Task 3: Debug MCP Server Issues

**Checklist**:
1. ✅ Gemini CLI installed and working (`gemini --version`)
2. ✅ Dependencies installed (`npm install` completed)
3. ✅ File is executable (`chmod +x src/index.js`)
4. ✅ Configuration in `~/.claude/settings.json` is correct
5. ✅ Path in config is absolute path
6. ✅ No syntax errors in index.js

**Debug Commands**:
```bash
# Check file exists
ls -la gemini-mcp-server/src/index.js

# Test Gemini CLI
gemini "test"

# Check Node version
node --version  # Should be >= 18

# View server errors
npm start  # Watch stderr output
```

### Task 4: Update Examples

When adding features, update relevant example in `ai-orchestrator/examples/`:

- Add new use case to existing example OR
- Create new example file: `example-N-name.md`

**Example Format**:
```markdown
# 示例 N: [Title]

## 场景：[Description]

### 在 Claude Code 中的对话：

[Conversation transcript]

## 关键要点：
[Summary bullets]
```

## Project-Specific Context

### Design Decisions

**Why three components (MCP + Skill + Agent)?**

- **MCP Server**: Provides raw tools (like an API)
- **Skill**: Packages knowledge and best practices (like a guide)
- **Agent**: Orchestrates complex workflows (like a manager)

This separation allows:
- Independent use of each component
- Easy testing and debugging
- Clear separation of concerns
- Flexible combination for different needs

**Why separate MCP Server from Skills/Agents?**

- MCP Server can be used in ANY project (global config)
- Skills and Agents are typically project-specific
- Allows reusing MCP Server across multiple projects
- Cleaner dependency management

### Known Limitations

1. **Gemini CLI dependency**: Requires Gemini CLI to be installed and configured
2. **No conversation context**: Gemini doesn't have access to Claude Code conversation history
3. **Timeout**: 60-second timeout on Gemini calls (configurable in code)
4. **Error handling**: Gemini CLI errors are surfaced but not all are actionable
5. **Model availability**: Depends on user's Gemini CLI configuration

### Future Enhancements (Ideas)

1. **Stream responses**: Add `gemini_stream` tool for real-time streaming
2. **Batch processing**: Add `gemini_batch` for multiple prompts at once
3. **Multimodal**: Add image/file support if Gemini CLI supports it
4. **Caching**: Cache Gemini responses for repeated queries
5. **Metrics**: Track usage patterns and performance
6. **UI Dashboard**: Web interface for monitoring and configuration

## Testing Strategies

### Unit Testing MCP Server

```bash
# Currently: No formal unit tests
# Manual testing approach:
# 1. Start server: npm start
# 2. Send JSON-RPC requests manually
# 3. Verify responses

# Future: Add Jest/Mocha tests
```

### Integration Testing

```bash
# Test full stack:

# 1. In Claude Code:
你: 使用 gemini_chat 询问 "test"

# 2. Verify Gemini CLI was called:
# Check terminal for output

# 3. Test skill triggering:
你: 对比 Claude 和 Gemini 的回答

# 4. Test agent:
你: /multi-ai-orchestrator
```

### User Acceptance Testing

Use the three examples:
1. `example-1-basic-usage.md` - Basic comparison
2. `example-2-orchestration.md` - Code review collaboration
3. `example-3-research.md` - Research task

## Troubleshooting Guide

### Issue: "MCP Server not loading"

**Diagnosis**:
```bash
# Check if file exists
ls -la gemini-mcp-server/src/index.js

# Check configuration
cat ~/.claude/settings.json

# Check Node.js version
node --version
```

**Solutions**:
1. Ensure absolute path in config
2. Verify JSON syntax (commas, quotes)
3. Restart Claude Code after config changes
4. Check Claude Code terminal for error messages

### Issue: "gemini command not found"

**Diagnosis**:
```bash
which gemini
gemini --version
```

**Solutions**:
1. Install Gemini CLI (refer to official docs)
2. Add to PATH if in custom location
3. Verify installation with `gemini --help`

### Issue: "Skill/Agent not triggering"

**Diagnosis**:
- Are you using trigger keywords?
- Is the .claude directory structure correct?

**Solutions**:
1. Use explicit keywords: "对比", "ask Gemini", "协作"
2. Verify file paths with `ls -la ai-orchestrator/.claude/`
3. Try manual invocation: `/multi-ai-orchestrator`

### Issue: "Timeout errors"

**Diagnosis**:
- Complex prompts taking too long?
- Gemini API rate limits?

**Solutions**:
1. Simplify prompts
2. Break into smaller tasks
3. Increase timeout in `src/index.js` (currently 60000ms)

## Maintenance Checklist

### Regular Maintenance

- [ ] Keep dependencies updated (`npm update` in gemini-mcp-server)
- [ ] Test MCP Server after Claude Code updates
- [ ] Review and update examples for clarity
- [ ] Check Gemini CLI for updates/new features

### After Major Changes

- [ ] Update README.md if functionality changes
- [ ] Add/update examples for new features
- [ ] Update PROJECT_SUMMARY.md with new stats
- [ ] Test all three components still work together

### Documentation Sync

Ensure these are consistent:
- MCP Server capabilities in README vs actual tools
- Skill description matches actual behavior
- Agent patterns match implementation
- Examples use current APIs and patterns

## Performance Considerations

### MCP Server
- Each Gemini call takes 1-5 seconds typically
- Consider parallel calls when possible
- Implement caching if repeated queries
- Monitor stderr for performance bottlenecks

### Skills
- Auto-trigger based on description matching
- Keep descriptions specific to reduce false triggers
- Load entire SKILL.md into context (be mindful of size)

### Agents
- Long-running tasks may need progress updates
- Consider checkpoint/resume for complex workflows
- Balance thoroughness with user patience

## Security Notes

1. **API Keys**: Gemini CLI manages keys (not in this codebase)
2. **Data Privacy**: Prompts sent to Gemini API
3. **Code Execution**: MCP Server executes `gemini` command (user's environment)
4. **File Access**: MCP Server has access based on Claude Code permissions
5. **Input Validation**: Always validate user input in tool calls

## Related Resources

- [Claude Code Documentation](https://code.claude.com/docs)
- [MCP Protocol](https://modelcontextprotocol.io)
- [Gemini CLI Documentation](https://github.com/google/gemini-cli)
- [Project README](./README.md)
- [Project Summary](./PROJECT_SUMMARY.md)

## Quick Reference Card

### MCP Tools
```
gemini_chat        - Ask Gemini a question
gemini_model_info  - View available models
```

### Skills
```
ai-compare         - Compare Claude and Gemini responses
                   Trigger: "对比", "compare", "both AIs"
```

### Agents
```
/multi-ai-orchestrator  - Coordinate complex multi-AI tasks
                        Trigger: "/multi-ai", "协作", "orchestrate"
```

### Common Commands
```bash
# Install dependencies
cd gemini-mcp-server && npm install

# Test MCP Server
npm start

# View project structure
find . -type f -name "*.md" -o -name "*.js"

# Check configuration
cat ~/.claude/settings.json
```

## Notes for Future Claude Instances

1. **This is a working project**, not a template. All components are functional.
2. **MCP Server must be configured** in settings.json before use (see QUICKSTART.md)
3. **Gemini CLI is external dependency**, user must have it installed
4. **Three distinct layers**: Infrastructure (MCP) → Knowledge (Skills) → Orchestration (Agents)
5. **Examples are comprehensive**, they demonstrate real-world usage patterns
6. **Project is modular**, each component can be used independently
7. **Location matters**: This project is at `/Users/apple/claudecode/claude-gemini-bridge`
8. **All paths in docs are absolute**, adjust if moving project
9. **Documentation is extensive** (2400+ lines), use search/grep to find specific topics
10. **User has Gemini CLI installed**, confirmed during project creation

## When User Asks for Help

Common requests and how to handle:

1. **"How do I use this?"**
   → Point to `ai-orchestrator/examples/QUICKSTART.md`

2. **"Something isn't working"**
   → Follow troubleshooting guide above
   → Check: Gemini CLI, dependencies, configuration

3. **"Add a feature"**
   → Identify which component (MCP/Skill/Agent)
   → Follow development guidelines above
   → Update relevant examples

4. **"Explain the architecture"**
   → Three-layer design: MCP (tools) → Skills (knowledge) → Agents (orchestration)
   → Each layer independently useful but powerful together

5. **"How do I contribute?"**
   → Fork and modify
   → Test thoroughly
   → Update documentation
   → Keep examples in sync

---

**Last Updated**: 2026-01-07
**Project Status**: ✅ Complete and Functional
**Claude Code Version**: Compatible with latest Claude Code
**Dependencies**: Node.js >= 18, Gemini CLI, @modelcontextprotocol/sdk
