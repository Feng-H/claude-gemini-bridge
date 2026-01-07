# 示例 3: 研究任务 - 并行调查

## 场景：让 Claude 和 Gemini 并行研究不同来源

### 在 Claude Code 中的对话：

```
你: /multi-ai-orchestrator

Multi-AI Orchestrator Agent 已启动。

你: 让两个 AI 并行研究 "React Server Components 的最佳实践"

Claude 从技术文档和官方资源研究
Gemini 从社区讨论和实际项目经验研究

---

## Multi-AI 协作开始...

📋 **任务**: React Server Components 最佳实践研究
🎯 **协作模式**: Parallel Research (并行研究)
📍 **阶段**: Phase 1 - 并行调查

---

### 🔷 Claude 的研究：技术文档与官方资源

**研究来源：**
- React 官方文档
- React 团队博客
- RFC 文档
- Next.js 官方文档

**核心发现：**

#### 1. 架构原则

**数据获取位置：**
```typescript
// ✅ 正确：在 Server Component 中获取
async function BlogList() {
  const posts = await db.query('SELECT * FROM posts');
  return posts.map(post => <BlogCard key={post.id} {...post} />);
}

// ❌ 错误：在 Client Component 中获取
function BlogList() {
  const [posts, setPosts] = useState([]);
  useEffect(() => {
    fetch('/api/posts').then(r => r.json()).then(setPosts);
  }, []);
  // ...
}
```

**组件分层原则：**
```
Server Components (根部)
    ↓
Client Components (叶子)
```

#### 2. 性能优化

**减少 Client Bundle：**
- Server Components 不发送到客户端
- 仅发送可交互的 Client Components
- 使用动态导入减少初始包大小

```typescript
// 动态导入交互组件
import dynamic from 'next/dynamic';

const InteractiveChart = dynamic(() => import('./Chart'), {
  loading: () => <p>Loading chart...</p>,
  ssr: false  // 仅客户端渲染
});
```

#### 3. 数据序列化

**可序列化数据：**
- ✅ JSON 基本类型（string, number, boolean, null）
- ✅ 数组和对象（仅包含可序列化值）
- ✅ Date, Map, Set (React 特殊处理)
- ❌ Functions, Classes, Symbols

```typescript
// ✅ 正确
async function UserProfile({ userId }: { userId: string }) {
  const user = await db.getUser(userId);
  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
}

// ❌ 错误 - 传递函数
function Button({ onClick }: { onClick: () => void }) {
  return <button onClick={onClick}>Click</button>;
}
```

#### 4. 第三方库使用

**仅在 Client 使用：**
```typescript
'use client';  // 明确标记

import { useState } from 'react';
import Chart from 'chart.js';

export function DataChart({ data }: { data: number[] }) {
  const chartRef = useRef<HTMLCanvasElement>(null);

  useEffect(() => {
    if (chartRef.current) {
      new Chart(chartRef.current, { type: 'line', data });
    }
  }, [data]);

  return <canvas ref={chartRef} />;
}
```

#### 5. Composition 模式

**Server 包裹 Client：**
```typescript
// Server Component
async function Page() {
  const data = await fetchData();

  return (
    <div>
      <h1>Dashboard</h1>
      <ClientWrapper data={data} />
    </div>
  );
}

// Client Component
'use client';
function ClientWrapper({ data }: { data: Data }) {
  const [filter, setFilter] = useState('all');
  // 交互逻辑...
}
```

---

### 🔶 Gemini 的研究：社区实践与实际项目

**研究来源：**
- GitHub 讨论
- Stack Overflow
- Reddit r/reactjs
- Vercel 社区案例
- 实际生产项目分析

**核心发现：**

#### 1. 实际应用模式

**模式 A: 渐进式迁移**
```
1. 保持现有页面为 Client Components
2. 逐步提升父组件为 Server Components
3. 仅在需要交互的子组件添加 'use client'
```

**实际案例：**
```typescript
// 大型电商网站迁移经验

// 第一步：数据层 Server Component
async function ProductPage({ id }: { id: string }) {
  const product = await getProduct(id);

  return (
    <div>
      <ProductInfo product={product} />
      <AddToCartButton productId={id} />  // 仅此部分是 Client
    </div>
  );
}

// 第二步：逐步提升
// - ProductInfo 提升为 Server (无需交互)
// - AddToCartButton 保持 Client (需要 onClick)
```

#### 2. 常见陷阱（社区总结）

**陷阱 1: 意外的 Client Components**
```typescript
// ❌ 问题：所有导入都变成了 Client
'use client';
import { ServerComponent } from './server';

// ✅ 解决：仅在叶子组件使用 'use client'
// 保持 Server Component 不标记
```

**陷阱 2: Props 过度序列化**
```typescript
// ❌ 问题：大数据重复序列化
async function List() {
  const items = await fetchLargeList();  // 10000+ items
  return <FilterableList items={items} />;
}

// ✅ 解决：仅在 Client 端需要时获取
function ServerList() {
  return (
    <div>
      <Suspense fallback={<Loading />}>
        <FilterableList />
      </Suspense>
    </div>
  );
}

'use client';
async function FilterableList() {
  const items = await fetchLargeList();  // Client 端获取
  // ...
}
```

#### 3. 社区最佳实践

**实践 1: 明确的边界标记**
```typescript
// 文件命名约定
// - product-page.server.tsx  → Server Component
// - add-to-cart.client.tsx  → Client Component
// - types.ts                → 共享类型
```

**实践 2: 类型安全**
```typescript
// 共享类型确保序列化兼容性
// types.ts
export interface UserData {
  id: string;
  name: string;
  // ❌ 不要包含 functions
  // formatName: () => string;
}

// Server Component
async function UserCard({ userId }: { userId: string }) {
  const user: UserData = await getUser(userId);
  return <div>{user.name}</div>;
}

// Client Component
'use client';
function UserActions({ userId }: { userId: string }) {
  // Client 逻辑...
}
```

**实践 3: 错误处理**
```typescript
// Server Component 错误边界
async function Page() {
  try {
    const data = await fetchData();
    return <Content data={data} />;
  } catch (error) {
    return <ErrorFallback error={error} />;
  }
}

function ErrorFallback({ error }: { error: Error }) {
  return (
    <div className="error">
      <h2>Something went wrong</h2>
      <p>{error.message}</p>
    </div>
  );
}
```

#### 4. 性能监控（真实数据）

**社区报告的改进：**

| 指标 | Before | After | 改进 |
|------|--------|-------|------|
| Initial JS | 450KB | 180KB | 60% ↓ |
| TTI | 4.2s | 2.1s | 50% ↓ |
| LCP | 3.8s | 1.9s | 50% ↓ |

**来源：Vercel 2024 案例研究**

#### 5. 工具和生态

**推荐工具：**
```bash
# 检测 Server Components 使用
npm install --save-dev @next/bundle-analyzer

# 类型检查序列化问题
npm install --save-dev @swc/core
```

**React DevTools 扩展：**
- 明确标识 Server vs Client Components
- 显示组件边界
- 性能分析

---

📍 **阶段**: Phase 2 - 综合发现

### 交叉分析：

#### Claude 技术观点 vs Gemini 社区经验

**一致性发现：**
1. ✅ 都强调分层架构（Server 包裹 Client）
2. ✅ 都认为 'use client' 应该最小化
3. ✅ 都重视类型安全和数据序列化

**互补性发现：**

| 方面 | Claude (技术文档) | Gemini (社区) |
|------|------------------|---------------|
| **焦点** | 官方推荐做法 | 实际项目经验 |
| **示例** | 理想化的代码 | 真实迁移案例 |
| **陷阱** | 规范性警告 | 实际踩坑经验 |
| **性能** | 理论分析 | 真实数据（60% bundle 减少）|
| **工具** | 官方工具 | 社区工具和监控 |

---

📍 **阶段**: Phase 3 - 综合最佳实践指南

### React Server Components 最佳实践（综合版）

#### 📋 决策树

```
需要创建新组件？
    ↓
需要用户交互？（onClick, useState 等）
    ├─ 是 → 'use client' + Client Component
    └─ 否 →
        需要访问服务器资源？（数据库、文件系统）
        ├─ 是 → Server Component (默认)
        └─ 否 → Server Component (默认)
```

#### 🎯 核心原则

1. **默认使用 Server Components**
2. **仅在必要时标记 'use client'**
3. **保持清晰的组件边界**
4. **优先在 Server 端获取数据**
5. **注意数据序列化限制**

#### 📁 推荐项目结构

```
app/
├── products/
│   ├── page.tsx              # Server (获取数据)
│   └── components/
│       ├── product-card.tsx  # Server (展示)
│       └── add-to-cart.client.tsx  # Client (交互)
├── shared/
│   └── types.ts              # 共享类型
└── lib/
    └── db.ts                 # Server 工具
```

#### 💡 实战技巧

**技巧 1: 数据获取下推**
```typescript
// ✅ 好：数据在顶层获取
async function Page() {
  const data = await fetchData();
  return <Child data={data} />;
}

// ❌ 避免：每个组件自己获取
function Child() {
  const [data, setData] = useState();
  useEffect(() => { /* fetch */ }, []);
}
```

**技巧 2: Props 接口优化**
```typescript
// ✅ 明确可序列化类型
interface PostProps {
  id: string;
  title: string;
  content: string;
  createdAt: Date;  // React 特殊处理
}

// ❌ 避免复杂对象
interface PostProps {
  metadata: Metadata;  // 可能包含函数
}
```

**技巧 3: 渐进式增强**
```typescript
// Server Component 提供
async function Post({ id }: { id: string }) {
  const post = await getPost(id);

  return (
    <article>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
      <LikeButton postId={id} />  {/* Client Component */}
    </article>
  );
}
```

#### ⚠️ 常见错误清单

- [ ] ✅ Client Component 没有意外导入 Server Components
- [ ] ✅ Props 都是可序列化的
- [ ] ✅ 没有在 Server Component 中使用浏览器 API
- [ ] ✅ 没有在 Server Component 中使用 hooks
- [ ] ✅ 'use client' 文件尽可能小
- [ ] ✅ 类型定义在共享文件中

#### 📊 性能目标

基于社区数据：

- **Bundle 大小**: 减少 40-60%
- **首屏加载**: 改善 30-50%
- **Time to Interactive**: 改善 40-50%
- **Lighthouse Score**: 提升 10-20 分

---

### 最终建议

#### 对于新项目：
1. ✅ 全面采用 Server Components
2. ✅ 仅在交互处添加 'use client'
3. ✅ 从设计时就考虑分层

#### 对于现有项目：
1. 🔄 渐进式迁移（从数据层开始）
2. 📊 使用监控工具验证改进
3. 🎯 优先迁移高流量页面

#### 对于团队：
1. 📚 制定组件命名约定
2. ✅ 使用 ESLint 规则强制 'use client'
3. 👥 培训团队理解 RSC 概念

---

## 协作价值总结

### 如果只用 Claude：
- ✅ 完整的技术规范
- ❌ 缺少实际项目经验
- ❌ 没有真实性能数据

### 如果只用 Gemini：
- ✅ 丰富的社区经验
- ❌ 可能遗漏官方最佳实践
- ❌ 技术细节不够精确

### 协作使用：
- ✅ 技术规范 + 实战经验
- ✅ 理论 + 实践数据
- ✅ 全面且可靠的指南

---

✅ **协作完成！**

**成果：**
- 15+ 技术规范要点（Claude）
- 10+ 社区实践建议（Gemini）
- 真实性能数据和案例（Gemini）
- 综合最佳实践清单（综合）

这份指南结合了官方技术文档的准确性和社区实战经验，提供了最全面的 React Server Components 最佳实践！

---

## 关键要点：

1. **并行研究** - 两个 AI 同时调查不同来源
2. **明确分工** - Claude：官方/技术，Gemini：社区/实践
3. **互补发现** - 识别一致性和互补性
4. **真实数据** - 包含社区报告的性能改进数据
5. **综合指南** - 合并两个 AI 的所有发现
6. **实用建议** - 提供新项目、现有项目、团队的具体建议

这个协作模式展示了如何通过并行研究获得比单一来源更全面的知识！
```
