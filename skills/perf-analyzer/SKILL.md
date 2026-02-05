---
name: perf-analyzer
description: |
  性能分析和优化。用于识别性能瓶颈、内存泄漏、
  慢查询优化、前端性能等。关键词：performance,
  profiling, memory leak, slow query, optimization
---

# Perf Analyzer 性能分析器

帮助识别和解决性能问题。

## 常见性能问题

### 1. N+1 查询

```typescript
// Bad: N+1 查询
const users = await db.query('SELECT * FROM users');
for (const user of users) {
  user.orders = await db.query(
    'SELECT * FROM orders WHERE user_id = ?',
    [user.id]
  );
}

// Good: JOIN 或批量查询
const users = await db.query(`
  SELECT u.*, o.*
  FROM users u
  LEFT JOIN orders o ON o.user_id = u.id
`);
```

### 2. 内存泄漏

```typescript
// Bad: 事件监听未清理
useEffect(() => {
  window.addEventListener('resize', handler);
  // 缺少清理
}, []);

// Good: 清理监听器
useEffect(() => {
  window.addEventListener('resize', handler);
  return () => window.removeEventListener('resize', handler);
}, []);
```

### 3. 不必要的重渲染

```typescript
// Bad: 每次渲染创建新对象
<Component style={{ color: 'red' }} />

// Good: 使用 useMemo
const style = useMemo(() => ({ color: 'red' }), []);
<Component style={style} />
```

## SQL 优化检查

```sql
-- 使用 EXPLAIN 分析
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'test@example.com';

-- 检查是否使用索引
-- 避免 type: ALL (全表扫描)
-- 期望 type: ref, eq_ref, const
```

## 性能指标

| 指标 | 目标值 |
|-----|-------|
| API 响应时间 | < 200ms |
| 页面首屏 (FCP) | < 1.8s |
| 可交互时间 (TTI) | < 3.8s |
| 数据库查询 | < 50ms |

## 工具推荐

- **Node.js**: clinic.js, 0x
- **Go**: pprof
- **前端**: Lighthouse, Chrome DevTools
- **数据库**: EXPLAIN, slow query log
