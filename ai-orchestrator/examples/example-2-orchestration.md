# 示例 2: 多 AI 协作 - 代码审查

## 场景：让 Claude 和 Gemini 协作进行全面的代码审查

### 在 Claude Code 中的对话：

```
你: /multi-ai-orchestrator

Multi-AI Orchestrator Agent 已启动。

我准备好协调 Claude 和 Gemini 的协作了。请告诉我你想完成什么任务？

你: 让两个 AI 协作审查这个 React Hook 代码：

```typescript
import { useState, useEffect } from 'react';

export function useUserData(userId: string) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    async function fetchData() {
      try {
        setLoading(true);
        const response = await fetch(`/api/users/${userId}`);
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    }

    fetchData();
  }, [userId]);

  return { data, loading, error };
}
```

## Multi-AI 协作开始...

📋 **任务**: React Hook 代码全面审查
🎯 **协作模式**: Parallel (并行) + Cross-Review
📍 **阶段**: Phase 1 - 并行分析

---

### 🔷 Claude 的分析：安全性与性能

**安全性问题：**

1. **XSS 风险** - 没有 XSS 防护
```typescript
// 建议：验证和清理数据
import DOMPurify from 'dompurify';

const result = DOMPurify.sanitize(await response.json());
```

2. **错误信息泄露** - 直接暴露错误消息
```typescript
// ❌ 当前
setError(err.message);

// ✅ 改进
setError('Unable to load user data');
console.error('Fetch error:', err);
```

3. **TypeScript 类型不完整**
```typescript
// 建议：定义完整类型
interface User {
  id: string;
  name: string;
  email: string;
}

const [data, setData] = useState<User | null>(null);
```

**性能优化：**

1. **缺少请求取消**
```typescript
useEffect(() => {
  const abortController = new AbortController();

  async function fetchData() {
    try {
      const response = await fetch(`/api/users/${userId}`, {
        signal: abortController.signal
      });
      // ...
    } catch (err) {
      if (err.name !== 'AbortError') {
        setError(err.message);
      }
    }
  }

  fetchData();

  return () => abortController.abort();
}, [userId]);
```

2. **考虑缓存策略**
```typescript
// 建议：使用 React Query 或 SWR
import { useQuery } from '@tanstack/react-query';

function useUserData(userId: string) {
  return useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetch(`/api/users/${userId}`).then(r => r.json())
  });
}
```

---

### 🔶 Gemini 的分析：最佳实践与可维护性

**代码质量：**

1. **命名可以更具体**
```typescript
// ❌ 当前
const [data, setData] = useState(null);

// ✅ 改进
const [user, setUser] = useState<User | null>(null);
```

2. **文档缺失**
```typescript
/**
 * Fetches and manages user data by ID
 * @param userId - The unique identifier of the user
 * @returns Object containing user data, loading state, and error
 */
export function useUserData(userId: string) {
  // ...
}
```

**最佳实践：**

1. **添加依赖验证**
```typescript
if (!userId) {
  return { data: null, loading: false, error: 'Invalid userId' };
}
```

2. **考虑重试机制**
```typescript
const [retryCount, setRetryCount] = useState(0);

useEffect(() => {
  async function fetchData() {
    try {
      // ...
    } catch (err) {
      if (retryCount < 3) {
        setTimeout(() => setRetryCount(c => c + 1), 1000);
      }
      setError(err.message);
    }
  }

  fetchData();
}, [userId, retryCount]);
```

**可访问性：**
- 考虑添加 loading 状态的辅助函数
```typescript
const isLoading = loading && !data;
const isError = error !== null;
const isSuccess = data !== null;
```

---

📍 **阶段**: Phase 2 - 交叉评审

### Claude 评审 Gemini 的建议：

✅ **优秀的建议：**
- 文档和类型定义对可维护性很重要
- 重试机制增加了可靠性
- 命名改进提升了代码可读性

💡 **补充建议：**
- 重试机制应该使用指数退避
```typescript
const delay = Math.pow(2, retryCount) * 1000;
setTimeout(() => setRetryCount(c => c + 1), delay);
```

### Gemini 评审 Claude 的建议：

✅ **关键的安全性考量：**
- AbortController 对防止内存泄漏很重要
- 错误处理可以更用户友好

💡 **补充建议：**
- React Query 确实是最优方案，但也应该展示基础实现

---

📍 **阶段**: Phase 3 - 综合改进版本

### 整合后的最佳实践版本：

```typescript
import { useState, useEffect, useCallback } from 'react';

/**
 * Custom hook for fetching and managing user data
 *
 * @param userId - The unique identifier of the user
 * @returns Object containing:
 *   - user: User data or null
 *   - loading: Boolean indicating if data is being fetched
 *   - error: Error message or null
 *   - retry: Function to manually retry the request
 *   - isLoading: Convenience boolean (true only on first load)
 *   - isError: Convenience boolean (true if error exists)
 *   - isSuccess: Convenience boolean (true if data exists)
 */
interface User {
  id: string;
  name: string;
  email: string;
}

interface UseUserDataResult {
  user: User | null;
  loading: boolean;
  error: string | null;
  retry: () => void;
  isLoading: boolean;
  isError: boolean;
  isSuccess: boolean;
}

export function useUserData(userId: string): UseUserDataResult {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
  const [retryCount, setRetryCount] = useState(0);

  const fetchData = useCallback(async (signal?: AbortSignal) => {
    if (!userId) {
      setError('Invalid userId');
      setLoading(false);
      return;
    }

    try {
      setLoading(true);
      setError(null);

      const response = await fetch(`/api/users/${userId}`, {
        signal,
        headers: {
          'Content-Type': 'application/json',
        },
      });

      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }

      const result: User = await response.json();
      setUser(result);
    } catch (err) {
      if (err instanceof Error) {
        if (err.name !== 'AbortError') {
          setError('Unable to load user data');
          console.error('Fetch error:', err);
        }
      }
    } finally {
      setLoading(false);
    }
  }, [userId]);

  useEffect(() => {
    const abortController = new AbortController();

    fetchData(abortController.signal);

    return () => abortController.abort();
  }, [fetchData, retryCount]);

  const retry = () => setRetryCount(c => c + 1);

  return {
    user,
    loading,
    error,
    retry,
    isLoading: loading && !user,
    isError: error !== null,
    isSuccess: user !== null,
  };
}
```

### 改进要点总结：

| 方面 | Claude 贡献 | Gemini 贡献 |
|------|------------|------------|
| **安全性** | XSS 防护、错误处理 | 输入验证 |
| **性能** | AbortController、React Query | 重试机制 |
| **类型安全** | 完整的 TypeScript 类型 | - |
| **可维护性** | - | 文档、命名 |
| **用户体验** | 友好错误消息 | 辅助布尔值 |
| **可靠性** | 错误边界 | 重试逻辑 |

---

### 最终建议：

1. **立即实施**（关键问题）：
   - ✅ 添加 AbortController
   - ✅ 改进错误处理
   - ✅ 添加 TypeScript 类型

2. **短期改进**（1-2 天）：
   - 📝 添加完整文档
   - 🔄 实现重试机制
   - ✨ 添加辅助布尔值

3. **长期优化**（考虑重构）：
   - 🚀 迁移到 React Query
   - 🧪 添加单元测试
   - 📊 添加性能监控

### 备选方案：

如果项目已有 React Query，推荐使用这个简化版本：

```typescript
import { useQuery } from '@tanstack/react-query';

interface User {
  id: string;
  name: string;
  email: string;
}

export function useUserData(userId: string) {
  return useQuery({
    queryKey: ['user', userId],
    queryFn: async () => {
      const response = await fetch(`/api/users/${userId}`);
      if (!response.ok) {
        throw new Error('Failed to fetch user');
      }
      return response.json() as Promise<User>;
    },
    enabled: !!userId,
    retry: 3,
  });
}
```

---

✅ **协作完成！**

**成果：**
- Claude 发现了 6 个关键问题（安全性、性能）
- Gemini 提出了 5 个改进建议（最佳实践、可维护性）
- 交叉评审确保了方案的全面性
- 最终版本整合了两个 AI 的所有最佳实践

**价值：**
- 单独使用 Claude：可能缺少可维护性建议
- 单独使用 Gemini：可能忽略安全性细节
- 协作使用：获得全面、可靠的代码审查

---

## 关键要点：

1. **并行分析** - 两个 AI 同时工作，节省时间
2. **明确分工** - Claude：安全/性能，Gemini：最佳实践
3. **交叉评审** - 每个 AI 评估对方的建议
4. **综合方案** - 整合所有最佳实践
5. **优先级分级** - 关键、短期、长期改进
6. **备选方案** - 考虑不同场景的需求

这个协作模式展示了如何让两个 AI 互补，提供比单独使用任一 AI 更全面的代码审查！
```
