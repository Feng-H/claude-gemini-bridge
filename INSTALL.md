# 安装指南

本指南将帮助你完成 Claude-Gemini Bridge 的完整安装和配置。

## 目录

- [前置要求](#前置要求)
- [安装步骤](#安装步骤)
- [配置说明](#配置说明)
- [验证安装](#验证安装)
- [故障排除](#故障排除)
- [升级指南](#升级指南)

---

## 前置要求

### 1. Node.js 环境

**必需版本**: Node.js >= 18.0.0

**检查安装**:
```bash
node --version
# 应显示 v18.x.x 或更高
```

**如未安装**:
```bash
# macOS (使用 Homebrew)
brew install node

# 或访问 https://nodejs.org 下载安装包
```

### 2. Gemini CLI

**必需**: 已安装并配置好的 Gemini CLI

**检查安装**:
```bash
gemini --version
# 应显示版本号，如 0.23.0
```

**如未安装**:
```bash
# 使用 npm 全局安装
npm install -g @google/gemini-cli

# 配置 API Key
gemini auth login

# 验证安装
gemini "test"
```

**获取 API Key**:
访问 [Google AI Studio](https://makersuite.google.com/app/apikey) 获取免费 API Key

### 3. Claude Code

**必需**: 已安装 Claude Code

**检查安装**:
```bash
claude --version
# 或通过应用菜单查看版本
```

**如未安装**:
访问 [Claude Code官网](https://code.claude.com) 下载安装

---

## 安装步骤

### 步骤 1: 获取项目代码

#### 选项 A: Git Clone（推荐）

```bash
# 克隆项目
git clone https://github.com/your-username/claude-gemini-bridge.git
cd claude-gemini-bridge
```

#### 选项 B: 下载 ZIP

1. 访问 [GitHub Releases](https://github.com/your-username/claude-gemini-bridge/releases)
2. 下载最新版本的 ZIP 文件
3. 解压到任意目录

### 步骤 2: 安装 MCP Server 依赖

```bash
cd gemini-mcp-server
npm install
```

**预期输出**:
```
added XX packages, and audited XX packages in Xs
found 0 vulnerabilities
```

### 步骤 3: 配置 MCP Server

#### 方式 A: 全局配置（推荐）

编辑 `~/.claude/mcp.json`（如果不存在则创建）:

```json
{
  "mcpServers": {
    "gemini": {
      "command": "node",
      "args": [
        "/absolute/path/to/claude-gemini-bridge/gemini-mcp-server/src/index.js"
      ]
    }
  }
}
```

**重要**: 将 `/absolute/path/to/...` 替换为实际路径

**获取绝对路径**:
```bash
pwd  # 在 gemini-mcp-server 目录下执行
```

#### 方式 B: 项目本地配置

在项目根目录创建 `.claude/settings.json`:

```json
{
  "mcpServers": {
    "gemini": {
      "command": "node",
      "args": [
        "./gemini-mcp-server/src/index.js"
      ]
    }
  }
}
```

**注意**: 相对路径相对于项目根目录

### 步骤 4: 配置 Skills 和 Agents

Skills 和 Agents 有两种配置方式：

#### 方式 A: 项目本地（推荐用于 GitHub 分享）

Skills 和 Agents 已包含在项目的 `skills/` 和 `agents/` 目录下。

Claude Code 会自动加载项目目录下的 `.claude/` 目录。

**验证结构**:
```bash
ls -la skills/
# 应显示: ai-compare, dual-review, gemini-analyzer

ls -la agents/
# 应显示: init-gemini, multi-ai-orchestrator
```

#### 方式 B: 全局配置

如果想在所有项目中使用，复制到全局目录：

```bash
# 复制 Skills
cp -r skills/* ~/.claude/skills/

# 复制 Agents
cp -r agents/* ~/.claude/agents/
```

### 步骤 5: 配置代理（可选）

如果需要使用代理访问 Gemini API：

#### 方式 A: 环境变量

```bash
# 在 ~/.zshrc 或 ~/.bashrc 中添加
export HTTPS_PROXY=http://127.0.0.1:7890
export HTTP_PROXY=http://127.0.0.1:7890
export all_proxy=http://127.0.0.1:7890

# 重新加载配置
source ~/.zshrc
```

#### 方式 B: MCP Server 配置

编辑 `mcp.json`:
```json
{
  "mcpServers": {
    "gemini": {
      "command": "node",
      "args": ["./gemini-mcp-server/src/index.js"],
      "env": {
        "HTTPS_PROXY": "http://127.0.0.1:7890",
        "HTTP_PROXY": "http://127.0.0.1:7890"
      }
    }
  }
}
```

### 步骤 6: 配置超时（可选）

如需调整超时时间：

```bash
# 设置环境变量（单位：毫秒）
export GEMINI_TIMEOUT=120000  # 120秒

# 或在 MCP Server 配置中设置
```

### 步骤 7: 重启 Claude Code

配置完成后，**必须重启 Claude Code** 才能生效：

```bash
# 完全退出 Claude Code
# 然后重新打开
```

---

## 配置说明

### MCP Server 配置详解

**完整配置示例**:
```json
{
  "mcpServers": {
    "gemini": {
      "command": "node",
      "args": [
        "/Users/yourname/claude-gemini-bridge/gemini-mcp-server/src/index.js"
      ],
      "env": {
        "HTTPS_PROXY": "http://127.0.0.1:7890",
        "GEMINI_TIMEOUT": "120000"
      }
    }
  }
}
```

**参数说明**:
- `command`: 执行命令，通常是 `node`
- `args`: 参数数组，第二个参数是脚本路径
- `env`: 环境变量（可选）

### Skills 配置

Skills 无需额外配置，Claude Code 会自动：
1. 扫描 `~/.claude/skills/` 目录
2. 读取每个 Skill 的 `SKILL.md`
3. 根据 `description` 字段自动触发

**验证 Skills 已加载**:
```bash
# 在 Claude Code 中
你: 列出所有可用的 skills
```

### Agents 配置

Agents 同样无需配置，但需要手动调用：

```bash
# 在 Claude Code 中
你: /multi-ai-orchestrator
你: /init-gemini
```

---

## 验证安装

### 测试 MCP Server

#### 1. 检查配置文件

```bash
cat ~/.claude/mcp.json
# 或
cat .claude/settings.json
```

#### 2. 测试 Gemini CLI

```bash
gemini "What is 2+2?"
# 应返回答案
```

#### 3. 在 Claude Code 中测试

**测试 1: 基础查询**
```bash
# 在 Claude Code 中
你: 使用 gemini_chat 询问 "What is AI?"
```

**预期结果**: Claude 应调用 Gemini 并返回结果

**测试 2: 模型信息**
```bash
你: 使用 gemini_model_info 查看可用模型
```

**预期结果**: 显示 Gemini CLI 版本和模型列表

**测试 3: 项目分析**
```bash
你: 使用 gemini_analyze 分析当前项目，类型为 review
```

**预期结果**: 分析项目并返回结果

### 测试 Skills

**测试 ai-compare Skill**:
```bash
你: 对比 Claude 和 Gemini 对这个问题的回答："什么是微服务架构？"
```

**预期结果**: 提供两个 AI 的对比分析

**测试 gemini-analyzer Skill**:
```bash
你: 用 Gemini 分析这个文件的安全性
```

**预期结果**: 触发 gemini_analyze 工具进行分析

### 测试 Agents

**测试 init-gemini Agent**:
```bash
你: /init-gemini
```

**预期结果**: Agent 启动，询问项目路径

**测试 multi-ai-orchestrator Agent**:
```bash
你: /multi-ai-orchestrator
```

**预期结果**: Agent 启动，询问协作模式

---

## 故障排除

### 问题 1: MCP Server 未加载

**症状**: Claude Code 中无法使用 `gemini_chat` 等工具

**解决方案**:

1. **检查配置文件**
   ```bash
   cat ~/.claude/mcp.json
   # 确保路径是绝对路径
   # 确保 JSON 格式正确
   ```

2. **检查文件权限**
   ```bash
   ls -la gemini-mcp-server/src/index.js
   # 应显示 -rwxr-xr-x 或类似权限
   ```

3. **检查 Node.js 版本**
   ```bash
   node --version
   # 必须 >= 18.0.0
   ```

4. **手动测试 MCP Server**
   ```bash
   cd gemini-mcp-server
   npm start
   # 按 Ctrl+C 停止
   # 如果没有错误，说明 Server 正常
   ```

5. **查看 Claude Code 日志**
   - 在启动 Claude Code 的终端查看错误信息
   - 日志输出到 stderr

### 问题 2: Gemini CLI 错误

**症状**: 调用工具时报错 "Gemini CLI not found"

**解决方案**:

1. **验证安装**
   ```bash
   gemini --version
   ```

2. **重新安装**
   ```bash
   npm uninstall -g @google/gemini-cli
   npm install -g @google/gemini-cli
   gemini auth login
   ```

3. **检查 PATH**
   ```bash
   which gemini
   # 应显示 gemini 的路径
   ```

### 问题 3: Skills/Agents 未触发

**症状**: 使用关键词没有触发对应的 Skill/Agent

**解决方案**:

1. **检查目录结构**
   ```bash
   ls -la ~/.claude/skills/
   ls -la ~/.claude/agents/
   # 或者检查项目目录
   ls -la skills/
   ls -la agents/
   ```

2. **验证 SKILL.md/AGENT.md**
   - 确保文件存在
   - 检查 `description` 字段是否包含关键词

3. **尝试手动调用**
   ```bash
   # Skill 会自动触发，无需手动调用
   # Agent 需要使用 /agent-name 调用
   你: /multi-ai-orchestrator
   ```

4. **重启 Claude Code**
   - 配置更改后需要重启

### 问题 4: 代理配置无效

**症状**: 代理设置后仍无法访问 Gemini API

**解决方案**:

1. **验证代理环境变量**
   ```bash
   echo $HTTPS_PROXY
   echo $HTTP_PROXY
   ```

2. **测试代理连接**
   ```bash
   curl -x http://127.0.0.1:7890 https://www.google.com
   ```

3. **使用 MCP Server 配置**
   ```json
   {
     "env": {
       "HTTPS_PROXY": "http://127.0.0.1:7890"
     }
   }
   ```

### 问题 5: 超时错误

**症状**: 大文件分析时经常超时

**解决方案**:

1. **增加超时时间**
   ```bash
   export GEMINI_TIMEOUT=180000  # 3分钟
   ```

2. **或使用更快模型**
   ```javascript
   {
     "model": "gemini-2.0-flash-exp"  // 更快
   }
   ```

### 问题 6: 文件未找到错误

**症状**: `Error: ENOENT: no such file or directory`

**解决方案**:

1. **使用绝对路径**
   ```bash
   # 错误：相对路径
   ./gemini-mcp-server/src/index.js

   # 正确：绝对路径
   /Users/yourname/claude-gemini-bridge/gemini-mcp-server/src/index.js
   ```

2. **验证文件存在**
   ```bash
   ls -la /path/to/gemini-mcp-server/src/index.js
   ```

---

## 升级指南

### 升级到最新版本

#### 1. 备份配置

```bash
# 备份 MCP 配置
cp ~/.claude/mcp.json ~/.claude/mcp.json.backup

# 备份自定义 Skills/Agents（如有）
cp -r ~/.claude/skills ~/.claude/skills.backup
cp -r ~/.claude/agents ~/.claude/agents.backup
```

#### 2. 拉取最新代码

```bash
cd claude-gemini-bridge
git pull origin main
```

#### 3. 更新依赖

```bash
cd gemini-mcp-server
npm install
```

#### 4. 验证升级

```bash
# 检查版本
cat package.json | grep version

# 在 Claude Code 中测试
你: 使用 gemini_model_info
```

### 回滚版本

```bash
cd claude-gemini-bridge
git log --oneline
git checkout <previous-commit-hash>

cd gemini-mcp-server
npm install
```

---

## 卸载

### 完全卸载

```bash
# 1. 删除项目目录
rm -rf ~/claude-gemini-bridge

# 2. 删除 MCP 配置
# 编辑 ~/.claude/mcp.json，删除 gemini 配置

# 3. 删除全局 Skills/Agents（如果已复制）
rm -rf ~/.claude/skills/ai-compare
rm -rf ~/.claude/skills/dual-review
rm -rf ~/.claude/skills/gemini-analyzer
rm -rf ~/.claude/agents/init-gemini
rm -rf ~/.claude/agents/multi-ai-orchestrator

# 4. （可选）卸载 Gemini CLI
npm uninstall -g @google/gemini-cli
```

---

## 下一步

安装完成后，建议：

1. 阅读 [README.md](./README.md) 了解使用场景
2. 阅读 [ARCHITECTURE.md](./ARCHITECTURE.md) 了解架构设计
3. 查看 [examples/](./ai-orchestrator/examples/) 学习实际使用案例
4. 尝试不同的 Skills 和 Agents

---

## 获取帮助

如果遇到问题：

1. 查看 [故障排除](#故障排除) 部分
2. 搜索 [GitHub Issues](https://github.com/your-username/claude-gemini-bridge/issues)
3. 创建新 Issue 并提供：
   - 操作系统版本
   - Node.js 版本
   - Gemini CLI 版本
   - Claude Code 版本
   - 错误信息和复现步骤

---

**Happy Coding with Multi-AI Collaboration!** 🚀
