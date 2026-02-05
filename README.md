# Hajimi Skills

一套为 AI 编程助手设计的样例 Skills 集合，可供 hajimi-cli 及其他支持 Skills 规范的工具使用。

## 功能特性

- 17 个开箱即用的 Skills
- 覆盖开发全流程：设计、编码、测试、审查、部署
- 支持多种项目类型：前端、后端、移动端、桌面端
- 遵循 [Agent Skills Spec](https://agentskills.io/specification) 规范

## Skills 列表

### 开发工具类

| Skill | 用途 |
|-------|------|
| [api-designer](./skills/api-designer/) | RESTful/GraphQL API 设计、OpenAPI 规范 |
| [git-workflow](./skills/git-workflow/) | Git 分支管理、提交规范、版本发布 |
| [docker-compose](./skills/docker-compose/) | Docker 容器编排、服务配置 |
| [db-migration](./skills/db-migration/) | 数据库迁移脚本、schema 变更 |
| [ci-pipeline](./skills/ci-pipeline/) | GitHub Actions、CI/CD 流水线 |
| [env-setup](./skills/env-setup/) | 环境变量、IDE 配置、项目初始化 |

### 代码质量类

| Skill | 用途 |
|-------|------|
| [test-generator](./skills/test-generator/) | 单元测试、集成测试、Mock 数据 |
| [refactor-assistant](./skills/refactor-assistant/) | 代码重构、设计模式、SOLID 原则 |
| [perf-analyzer](./skills/perf-analyzer/) | 性能分析、N+1 查询、内存泄漏 |
| [error-handler](./skills/error-handler/) | 异常处理、日志记录、错误边界 |
| [security-check](./skills/security-check/) | 安全审计、OWASP、依赖扫描 |
| [doc-generator](./skills/doc-generator/) | README、CHANGELOG、代码注释 |

### 项目审查类

| Skill | 用途 |
|-------|------|
| [review-frontend](./skills/review-frontend/) | React/Vue/Angular 前端项目审查 |
| [review-backend](./skills/review-backend/) | Go/Java/Python/Node.js 后端审查 |
| [review-android](./skills/review-android/) | Kotlin/Java Android 应用审查 |
| [review-ios](./skills/review-ios/) | Swift/SwiftUI iOS 应用审查 |
| [review-desktop](./skills/review-desktop/) | Electron/Tauri/Qt 桌面端审查 |

## 使用方法

### 在 hajimi-cli 中使用

```bash
# 克隆仓库到本地
git clone https://github.com/hajimi-work/hajimi-skills.git

# 在 hajimi-cli 配置中添加 skills 路径
```

### Skill 文件结构

每个 Skill 遵循标准结构：

```
skills/<skill-name>/
└── SKILL.md    # Skill 定义文件
```

### SKILL.md 格式

```yaml
---
name: skill-name
description: |
  Skill 描述和关键词
---

# Skill 标题

具体内容...
```

## 贡献指南

欢迎提交 PR 添加新的 Skills：

1. 在 `skills/` 目录下创建新文件夹
2. 添加 `SKILL.md` 文件
3. 遵循现有 Skill 的格式规范

## 许可证

MIT
