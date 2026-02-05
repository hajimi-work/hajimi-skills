---
name: ci-pipeline
description: |
  CI/CD 流水线配置。用于 GitHub Actions、GitLab CI、
  自动化测试、部署流程等。关键词：CI, CD, pipeline,
  GitHub Actions, deploy, automation
---

# CI Pipeline 持续集成

帮助配置 CI/CD 流水线。

## GitHub Actions 模板

```yaml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: pnpm/action-setup@v2
        with:
          version: 8

      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'pnpm'

      - run: pnpm install
      - run: pnpm lint
      - run: pnpm test

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: pnpm install
      - run: pnpm build
```

## 常用 Actions

| 用途 | Action |
|-----|--------|
| 检出代码 | actions/checkout@v4 |
| Node.js | actions/setup-node@v4 |
| Go | actions/setup-go@v5 |
| Docker | docker/build-push-action@v5 |
| 缓存 | actions/cache@v4 |

## Docker 构建发布

```yaml
  docker:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - uses: docker/build-push-action@v5
        with:
          push: true
          tags: ghcr.io/${{ github.repository }}:latest
```

## 部署触发条件

```yaml
# 仅在 tag 时部署
on:
  push:
    tags:
      - 'v*'

# 仅在特定路径变更时
on:
  push:
    paths:
      - 'src/**'
      - 'package.json'
```

## 环境变量和密钥

```yaml
env:
  NODE_ENV: production

jobs:
  deploy:
    environment: production
    steps:
      - run: echo ${{ secrets.API_KEY }}
```
