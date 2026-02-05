---
name: git-workflow
description: |
  Git 工作流管理和自动化。用于分支管理、提交规范、合并策略、
  冲突解决、版本发布等场景。关键词：git flow, branch, merge,
  rebase, release, tag, conflict
---

# Git Workflow 工作流管理

帮助管理 Git 工作流，包括分支策略、提交规范、版本发布等。

## 分支命名规范

| 分支类型 | 命名格式 | 示例 |
|---------|---------|------|
| 功能分支 | `feat/<描述>` | `feat/user-login` |
| 修复分支 | `fix/<issue-id>-<描述>` | `fix/123-null-pointer` |
| 热修复 | `hotfix/<版本>-<描述>` | `hotfix/1.2.1-security` |
| 发布分支 | `release/<版本>` | `release/2.0.0` |
| 实验分支 | `exp/<描述>` | `exp/new-algorithm` |

## 提交信息规范 (Conventional Commits)

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Type 类型

- `feat`: 新功能
- `fix`: Bug 修复
- `docs`: 文档更新
- `style`: 代码格式（不影响逻辑）
- `refactor`: 重构（非新功能、非修复）
- `perf`: 性能优化
- `test`: 测试相关
- `chore`: 构建/工具变更
- `ci`: CI/CD 配置

### 示例

```bash
# 简单提交
git commit -m "feat(auth): add OAuth2 login support"

# 带 body 的提交
git commit -m "fix(api): resolve race condition in request handler

The previous implementation could cause data corruption when
multiple requests arrived simultaneously.

Closes #456"
```

## 常用工作流操作

### 1. 创建功能分支

```bash
# 从 main 创建并切换
git checkout main
git pull origin main
git checkout -b feat/new-feature

# 开发完成后
git add .
git commit -m "feat: implement new feature"
git push -u origin feat/new-feature
```

### 2. 合并策略

```bash
# Merge (保留完整历史)
git checkout main
git merge --no-ff feat/new-feature

# Rebase (线性历史)
git checkout feat/new-feature
git rebase main
git checkout main
git merge feat/new-feature

# Squash (合并为单个提交)
git checkout main
git merge --squash feat/new-feature
git commit -m "feat: add new feature"
```

### 3. 解决冲突

```bash
# 查看冲突文件
git status

# 手动解决后
git add <resolved-files>
git rebase --continue  # 如果是 rebase
git merge --continue   # 如果是 merge

# 放弃操作
git rebase --abort
git merge --abort
```

### 4. 版本发布

```bash
# 创建发布分支
git checkout -b release/1.0.0

# 更新版本号、CHANGELOG 等
# ...

# 合并到 main 并打 tag
git checkout main
git merge --no-ff release/1.0.0
git tag -a v1.0.0 -m "Release version 1.0.0"
git push origin main --tags

# 合并回 develop (如果使用 git-flow)
git checkout develop
git merge --no-ff release/1.0.0
```

## 实用命令速查

```bash
# 查看分支图
git log --oneline --graph --all

# 查看某文件的修改历史
git log --follow -p -- <file>

# 查找引入 bug 的提交
git bisect start
git bisect bad HEAD
git bisect good v1.0.0

# 暂存当前工作
git stash push -m "WIP: feature description"
git stash list
git stash pop

# 修改最近的提交
git commit --amend

# 交互式 rebase (整理提交)
git rebase -i HEAD~3

# 撤销已推送的提交 (创建新提交)
git revert <commit-hash>

# Cherry-pick 特定提交
git cherry-pick <commit-hash>
```

## 团队协作最佳实践

1. **保持提交原子性**: 每个提交只做一件事
2. **频繁拉取**: 避免大规模冲突
3. **Code Review**: 合并前必须审查
4. **保护主分支**: 禁止直接推送到 main
5. **清理分支**: 合并后删除功能分支
