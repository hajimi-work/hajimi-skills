---
name: security-check
description: |
  安全检查和漏洞修复。用于代码安全审计、依赖漏洞扫描、
  OWASP 检查、密钥管理等。关键词：security, vulnerability,
  OWASP, XSS, SQL injection, secrets
---

# Security Check 安全检查

帮助识别和修复安全漏洞。

## OWASP Top 10 检查清单

### 1. 注入攻击防护

```typescript
// 错误示例: SQL 注入风险
const query = `SELECT * FROM users WHERE id = ${userId}`;

// 正确做法: 参数化查询
const query = 'SELECT * FROM users WHERE id = $1';
await db.query(query, [userId]);
```

### 2. XSS 防护

```typescript
// 正确做法: 使用 textContent 设置纯文本
element.textContent = userInput;

// 如需渲染 HTML，使用 DOMPurify 清理
import DOMPurify from 'dompurify';
const clean = DOMPurify.sanitize(userInput);
```

### 3. 敏感数据保护

```typescript
// 密码哈希
import bcrypt from 'bcrypt';
const hash = await bcrypt.hash(password, 12);

// 敏感字段脱敏
const maskEmail = (email: string) =>
  email.replace(/(.{2})(.*)(@.*)/, '$1***$3');
```

## 依赖安全扫描

```bash
# npm
npm audit
npm audit fix

# pnpm
pnpm audit
```

## 密钥管理规范

- 永远不要提交密钥到代码仓库
- 使用环境变量或密钥管理服务
- 定期轮换密钥

## CSRF 防护

```typescript
// 使用 CSRF Token
app.use(csrf({ cookie: true }));
```

## 认证安全

- [ ] 密码强度要求
- [ ] 登录失败限制
- [ ] Session 过期策略
- [ ] JWT 安全配置
