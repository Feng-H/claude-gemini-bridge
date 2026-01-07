# 示例 1: 基础使用

## 场景：对比 Claude 和 Gemini 对技术问题的回答

### 在 Claude Code 中的对话：

```
你: 对比 Claude 和 Gemini 对这个问题的回答：
"React Server Components 和普通组件有什么区别？"

Claude: 我先提供我的分析，然后获取 Gemini 的回答进行对比。

## Claude 的分析：

React Server Components (RSC) 和普通组件的主要区别：

### 1. 执行环境
- **Server Components**: 在服务器端渲染
- **普通组件**: 在客户端（浏览器）渲染

### 2. 能力限制
**Server Components:**
- ✅ 可以直接访问数据库、文件系统
- ✅ 可以使用服务器机密信息
- ❌ 不能使用 hooks (useState, useEffect)
- ❌ 不能处理浏览器事件

**普通组件:**
- ✅ 完整的 React hooks 支持
- ✅ 可以处理用户交互
- ❌ 不能直接访问服务器资源

### 3. 数据获取
```javascript
// Server Component - 直接访问数据库
async function BlogPost({ id }) {
  const post = await db.query(`SELECT * FROM posts WHERE id = ${id}`);
  return <article>{post.content}</article>;
}

// 普通组件 - 需要 API 调用
function BlogPost({ id }) {
  const [post, setPost] = useState(null);

  useEffect(() => {
    fetch(`/api/posts/${id}`)
      .then(res => res.json())
      .then(data => setPost(data));
  }, [id]);

  return post ? <article>{post.content}</article> : <Loading />;
}
```

### 4. 性能影响
- **Server Components**: 减少 JS bundle 大小，不发送到客户端
- **普通组件**: 增加 bundle 大小，发送到客户端

### 5. 使用场景
**使用 Server Components 当：**
- 需要直接访问服务器资源
- 组件不需要交互
- 需要优化首次加载性能

**使用普通组件当：**
- 需要用户交互（按钮、表单等）
- 需要使用浏览器 API
- 需要状态管理

现在让我获取 Gemini 的观点...

[调用 gemini_chat 工具]

## Gemini 的分析：

[Gemini CLI 返回的回答...]

## 对比总结：

### 相同点：
✅ 都认为执行环境是核心区别
✅ 都提到了 hooks 使用限制
✅ 都强调了性能优势

### 不同点：

| 方面 | Claude | Gemini |
|------|--------|--------|
| 解释重点 | 技术实现和代码示例 | 概念理解和使用场景 |
| 代码示例 | 提供了具体的数据库查询示例 | 更多概念性说明 |
| 性能讨论 | 详细的 bundle 大小分析 | 更关注用户体验 |
| 使用建议 | 明确的决策树 | 更广泛的场景描述 |

### 最佳实践建议：

结合两个 AI 的观点：

1. **架构决策**: 使用 Claude 的技术分析理解实现细节
2. **应用场景**: 参考 Gemini 的场景描述选择合适方案
3. **代码实现**: 遵循 Claude 的代码示例模式
4. **用户体验**: 考虑 Gemini 提到的性能和体验因素

### 结论：

对于你的项目，建议：
- 数据展示页面 → Server Components
- 交互式表单 → 普通组件
- 混合使用 → 在 Server Component 中导入普通 Client Component

这样就结合了两个 AI 的最佳建议！
```

---

## 关键要点：

1. **Claude 先回答** - 展示自己的能力
2. **调用 gemini_chat** - 获取 Gemini 的观点
3. **结构化对比** - 使用表格和列表清晰展示
4. **综合建议** - 结合两个 AI 的优势
5. **实用结论** - 给出可操作的建议

## 其他基础使用示例：

### 示例 A: 直接询问 Gemini
```
你: 使用 gemini_chat 询问 "TypeScript 的类型推断是如何工作的？"
```

### 示例 B: 获取 Gemini 模型信息
```
你: 查看 Gemini 有哪些可用模型
```

### 示例 C: 快速验证
```
你: 让 Gemini 验证这段代码的正确性：[代码]
```
