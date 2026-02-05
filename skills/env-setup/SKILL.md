---
name: env-setup
description: |
  开发环境配置和管理。用于配置环境变量、开发工具、
  IDE 设置、项目初始化等。关键词：environment, env,
  config, setup, dotenv, vscode
---

# Env Setup 环境配置

帮助配置和管理开发环境。

## 环境变量管理

### .env 文件规范

```bash
# .env.example - 提交到仓库的模板
# 应用配置
APP_NAME=myapp
APP_ENV=development
APP_PORT=3000

# 数据库
DATABASE_URL=postgres://user:pass@localhost:5432/myapp

# Redis
REDIS_URL=redis://localhost:6379

# 第三方服务
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
```

### 多环境配置

```
.env                # 默认配置
.env.local          # 本地覆盖 (不提交)
.env.development    # 开发环境
.env.production     # 生产环境
.env.test           # 测试环境
```

## VSCode 配置

### settings.json

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  },
  "typescript.preferences.importModuleSpecifier": "relative"
}
```

### extensions.json

```json
{
  "recommendations": [
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint",
    "bradlc.vscode-tailwindcss"
  ]
}
```

## 项目初始化检查清单

- [ ] 创建 .env.example
- [ ] 配置 .gitignore
- [ ] 设置 EditorConfig
- [ ] 配置 ESLint/Prettier
- [ ] 添加 pre-commit hooks
