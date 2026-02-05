---
name: doc-generator
description: |
  自动生成项目文档。用于 API 文档、README、CHANGELOG、
  代码注释、架构文档等。关键词：documentation, readme,
  changelog, jsdoc, typedoc, swagger
---

# Doc Generator 文档生成器

帮助生成各类项目文档。

## README 模板

```markdown
# 项目名称

简短描述项目功能。

## 功能特性

- 特性 1
- 特性 2
- 特性 3

## 快速开始

### 安装

\`\`\`bash
pnpm install
\`\`\`

### 配置

复制环境变量文件：

\`\`\`bash
cp .env.example .env
\`\`\`

### 运行

\`\`\`bash
pnpm dev
\`\`\`

## 使用示例

\`\`\`typescript
import { Client } from 'my-package';

const client = new Client({ apiKey: 'xxx' });
const result = await client.doSomething();
\`\`\`

## 开发

\`\`\`bash
pnpm test      # 运行测试
pnpm build     # 构建项目
pnpm lint      # 代码检查
\`\`\`

## 许可证

MIT
```

## CHANGELOG 格式

```markdown
# Changelog

## [1.2.0] - 2024-01-15

### Added
- 新增用户导出功能
- 支持批量操作

### Changed
- 优化查询性能

### Fixed
- 修复登录超时问题

## [1.1.0] - 2024-01-01

### Added
- 初始版本发布
```

## 代码注释规范

### JSDoc

```typescript
/**
 * 创建新用户
 * @param name - 用户名
 * @param email - 邮箱地址
 * @returns 创建的用户对象
 * @throws {ValidationError} 参数验证失败
 * @example
 * const user = await createUser('张三', 'test@example.com');
 */
async function createUser(name: string, email: string): Promise<User> {
  // ...
}
```

### Go Doc

```go
// CreateUser 创建新用户
//
// 参数:
//   - name: 用户名，长度 2-50
//   - email: 邮箱地址
//
// 返回创建的用户和可能的错误
func CreateUser(name, email string) (*User, error) {
    // ...
}
```
