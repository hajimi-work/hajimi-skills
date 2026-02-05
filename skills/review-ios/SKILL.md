---
name: review-ios
description: |
  iOS 项目审查。用于审查 Swift/SwiftUI 项目的架构、
  内存管理、性能、UI 等。关键词：ios, swift, swiftui,
  uikit, memory, retain cycle, core data
---

# Review iOS 苹果项目审查

全面审查 iOS 应用项目的代码质量。

## 审查清单

### 1. 架构设计

- [ ] 使用 MVVM/VIPER/TCA 架构
- [ ] 视图与业务逻辑分离
- [ ] 依赖注入合理使用
- [ ] 协议抽象接口

### 2. 内存管理

```swift
// 循环引用检查：
// 1. 闭包中使用 [weak self]
// 2. delegate 使用 weak 修饰
// 3. 避免强引用循环
```

- [ ] 无循环引用 (Instruments 检测)
- [ ] 正确使用 weak/unowned
- [ ] 大资源及时释放

### 3. 并发处理

- [ ] 使用 async/await 或 Combine
- [ ] UI 更新在主线程
- [ ] 避免数据竞争
- [ ] 正确取消异步任务

### 4. 数据持久化

- [ ] UserDefaults 使用合理
- [ ] Core Data/SwiftData 配置正确
- [ ] Keychain 存储敏感数据
- [ ] 文件管理规范

### 5. UI/UX

- [ ] 支持 Dark Mode
- [ ] 适配不同设备尺寸
- [ ] 遵循 Human Interface Guidelines
- [ ] 动画流畅自然

### 6. 安全性

- [ ] App Transport Security 配置
- [ ] 敏感数据加密
- [ ] 证书固定 (如需要)
- [ ] 越狱检测 (如需要)

## 审查报告格式

```markdown
## iOS 项目审查报告

### 项目信息
- 项目名称: xxx
- 审查日期: yyyy-mm-dd

### 审查结果
| 类别 | 状态 | 问题数 |
|-----|------|-------|
| 架构设计 | ✅/⚠️/❌ | n |
| 内存管理 | ✅/⚠️/❌ | n |

### 问题列表
1. [严重] 问题描述 - 文件:行号

### 改进建议
- 建议内容
```
