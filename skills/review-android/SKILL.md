---
name: review-android
description: |
  Android 项目审查。用于审查 Kotlin/Java Android 项目的
  架构、性能、内存、UI/UX 等。关键词：android, kotlin,
  jetpack compose, activity, fragment, memory leak
---

# Review Android 安卓项目审查

全面审查 Android 应用项目的代码质量。

## 审查清单

### 1. 架构设计

- [ ] 使用 MVVM/MVI 架构
- [ ] ViewModel 正确管理 UI 状态
- [ ] Repository 模式分离数据层
- [ ] 依赖注入 (Hilt/Koin)

### 2. 生命周期

- [ ] 正确处理 Activity/Fragment 生命周期
- [ ] 避免在 onDestroy 后更新 UI
- [ ] 使用 lifecycleScope 管理协程
- [ ] 配置变更时状态保持正确

### 3. 内存管理

```kotlin
// 常见内存泄漏检查：
// 1. 静态引用 Context/Activity
// 2. 未取消的回调和监听器
// 3. 内部类持有外部引用
// 4. Handler 泄漏
```

- [ ] 无内存泄漏 (LeakCanary 检测)
- [ ] 大图片正确处理和回收
- [ ] 列表使用 RecyclerView + DiffUtil

### 4. 网络与数据

- [ ] 使用 Retrofit/Ktor 进行网络请求
- [ ] 正确处理网络错误和超时
- [ ] 本地缓存策略合理
- [ ] Room 数据库使用规范

### 5. UI/UX

- [ ] 支持深色模式
- [ ] 适配不同屏幕尺寸
- [ ] 动画流畅无卡顿
- [ ] 遵循 Material Design 规范

### 6. 安全性

- [ ] 敏感数据加密存储
- [ ] 网络请求使用 HTTPS
- [ ] ProGuard/R8 混淆配置
- [ ] 避免日志泄露敏感信息

## 审查报告格式

```markdown
## Android 项目审查报告

### 项目信息
- 项目名称: xxx
- 审查日期: yyyy-mm-dd

### 审查结果
| 类别 | 状态 | 问题数 |
|-----|------|-------|
| 架构设计 | ✅/⚠️/❌ | n |
| 生命周期 | ✅/⚠️/❌ | n |
| 内存管理 | ✅/⚠️/❌ | n |

### 问题列表
1. [严重] 问题描述 - 文件:行号
2. [警告] 问题描述 - 文件:行号

### 改进建议
- 建议 1
- 建议 2
```
