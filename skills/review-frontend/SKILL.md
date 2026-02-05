---
name: review-frontend
description: |
  前端项目审查。用于审查 React/Vue/Angular 等前端项目的
  代码质量、性能、可访问性、SEO 等。关键词：frontend, react,
  vue, angular, web, accessibility, SEO, bundle size
---

# Review Frontend 前端项目审查

全面审查前端 Web 项目的代码质量和最佳实践。

## 审查清单

### 1. 项目结构

- [ ] 目录结构清晰合理
- [ ] 组件划分符合单一职责
- [ ] 公共组件与业务组件分离
- [ ] 样式文件组织有序

### 2. 代码质量

- [ ] TypeScript 类型完整，避免 any
- [ ] 组件 props 有明确类型定义
- [ ] 无未使用的变量和导入
- [ ] 代码风格一致 (ESLint/Prettier)

### 3. 性能优化

```typescript
// 检查项：
// 1. 组件是否正确使用 memo/useMemo/useCallback
// 2. 列表渲染是否有唯一 key
// 3. 是否有不必要的重渲染
// 4. 图片是否懒加载
// 5. 路由是否懒加载
```

- [ ] 首屏加载时间 < 3s
- [ ] Bundle 大小合理 (主包 < 200KB)
- [ ] 图片已压缩和懒加载
- [ ] 使用代码分割

### 4. 状态管理

- [ ] 状态层级合理，避免 prop drilling
- [ ] 全局状态与局部状态区分清晰
- [ ] 异步状态处理完善 (loading/error)

### 5. 可访问性 (A11y)

- [ ] 图片有 alt 属性
- [ ] 表单有 label 关联
- [ ] 颜色对比度符合 WCAG
- [ ] 支持键盘导航

### 6. SEO (如适用)

- [ ] 页面有正确的 title 和 meta
- [ ] 使用语义化 HTML 标签
- [ ] 支持 SSR/SSG (如需要)

## 审查报告格式

```markdown
## 前端项目审查报告

### 项目信息
- 项目名称: xxx
- 框架: React/Vue/Angular
- 审查日期: yyyy-mm-dd

### 审查结果
| 类别 | 状态 | 问题数 |
|-----|------|-------|
| 项目结构 | ✅/⚠️/❌ | n |
| 代码质量 | ✅/⚠️/❌ | n |
| 性能优化 | ✅/⚠️/❌ | n |

### 问题列表
1. [严重] 问题描述 - 文件:行号

### 改进建议
- 建议内容
```
