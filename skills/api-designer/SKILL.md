---
name: api-designer
description: |
  RESTful API 和 GraphQL 接口设计。用于设计 API 端点、
  定义数据模型、编写 OpenAPI 规范、处理认证授权等。
  关键词：REST, GraphQL, OpenAPI, Swagger, endpoint, schema
---

# API Designer 接口设计

帮助设计清晰、一致、易用的 API 接口。

## RESTful API 设计原则

### URL 命名规范

```
# 资源命名使用复数名词
GET    /users              # 获取用户列表
GET    /users/{id}         # 获取单个用户
POST   /users              # 创建用户
PUT    /users/{id}         # 完整更新用户
PATCH  /users/{id}         # 部分更新用户
DELETE /users/{id}         # 删除用户

# 嵌套资源
GET    /users/{id}/orders  # 获取用户的订单
POST   /users/{id}/orders  # 为用户创建订单

# 操作类接口使用动词
POST   /users/{id}/activate
POST   /orders/{id}/cancel
```

### HTTP 状态码

| 状态码 | 含义 | 使用场景 |
|-------|------|---------|
| 200 | OK | 成功获取/更新 |
| 201 | Created | 成功创建 |
| 204 | No Content | 成功删除 |
| 400 | Bad Request | 请求参数错误 |
| 401 | Unauthorized | 未认证 |
| 403 | Forbidden | 无权限 |
| 404 | Not Found | 资源不存在 |
| 409 | Conflict | 资源冲突 |
| 422 | Unprocessable | 业务逻辑错误 |
| 429 | Too Many Requests | 限流 |
| 500 | Internal Error | 服务器错误 |

### 响应格式

```json
// 成功响应
{
  "code": 0,
  "message": "success",
  "data": {
    "id": 1,
    "name": "张三"
  }
}

// 列表响应 (带分页)
{
  "code": 0,
  "message": "success",
  "data": {
    "items": [...],
    "pagination": {
      "page": 1,
      "page_size": 20,
      "total": 100,
      "total_pages": 5
    }
  }
}

// 错误响应
{
  "code": 40001,
  "message": "用户名已存在",
  "details": {
    "field": "username",
    "reason": "duplicate"
  }
}
```

## OpenAPI 3.0 规范模板

```yaml
openapi: 3.0.3
info:
  title: My API
  version: 1.0.0
  description: API 描述

servers:
  - url: https://api.example.com/v1
    description: 生产环境
  - url: https://api-staging.example.com/v1
    description: 测试环境

paths:
  /users:
    get:
      summary: 获取用户列表
      tags: [Users]
      parameters:
        - name: page
          in: query
          schema:
            type: integer
            default: 1
        - name: page_size
          in: query
          schema:
            type: integer
            default: 20
      responses:
        '200':
          description: 成功
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/UserList'

    post:
      summary: 创建用户
      tags: [Users]
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUser'
      responses:
        '201':
          description: 创建成功
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'

components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: integer
        username:
          type: string
        email:
          type: string
          format: email
        created_at:
          type: string
          format: date-time
      required: [id, username, email]

    CreateUser:
      type: object
      properties:
        username:
          type: string
          minLength: 3
          maxLength: 50
        email:
          type: string
          format: email
        password:
          type: string
          minLength: 8
      required: [username, email, password]

  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

security:
  - BearerAuth: []
```

## 认证方案

### JWT Token

```
Authorization: Bearer <token>
```

### API Key

```
X-API-Key: <api-key>
```

### OAuth 2.0 流程

```
1. 获取授权码: GET /oauth/authorize
2. 换取 Token: POST /oauth/token
3. 刷新 Token: POST /oauth/refresh
```

## 版本控制策略

```bash
# URL 路径版本 (推荐)
/v1/users
/v2/users

# Header 版本
Accept: application/vnd.api+json; version=1

# Query 参数版本
/users?version=1
```

## 最佳实践

1. **使用 HTTPS**: 所有 API 必须使用 HTTPS
2. **幂等性**: PUT/DELETE 操作应该是幂等的
3. **限流**: 实现请求限流保护服务
4. **文档**: 保持 API 文档与代码同步
5. **向后兼容**: 新版本不破坏旧客户端
