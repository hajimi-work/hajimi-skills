---
name: error-handler
description: |
  错误处理和日志记录。用于异常处理、错误边界、
  日志格式化、监控告警等。关键词：error, exception,
  logging, sentry, monitoring, alert
---

# Error Handler 错误处理

帮助设计错误处理和日志系统。

## 错误类型定义

```typescript
// 自定义错误基类
class AppError extends Error {
  constructor(
    public code: string,
    message: string,
    public statusCode: number = 500
  ) {
    super(message);
    this.name = 'AppError';
  }
}

// 具体错误类型
class ValidationError extends AppError {
  constructor(message: string) {
    super('VALIDATION_ERROR', message, 400);
  }
}

class NotFoundError extends AppError {
  constructor(resource: string) {
    super('NOT_FOUND', `${resource} not found`, 404);
  }
}

class UnauthorizedError extends AppError {
  constructor() {
    super('UNAUTHORIZED', 'Authentication required', 401);
  }
}
```

## 全局错误处理

### Express

```typescript
app.use((err: Error, req: Request, res: Response, next: NextFunction) => {
  if (err instanceof AppError) {
    return res.status(err.statusCode).json({
      code: err.code,
      message: err.message
    });
  }

  console.error(err);
  res.status(500).json({
    code: 'INTERNAL_ERROR',
    message: 'Something went wrong'
  });
});
```

### React 错误边界

```tsx
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  componentDidCatch(error: Error, info: React.ErrorInfo) {
    console.error('Error:', error, info);
  }

  render() {
    if (this.state.hasError) {
      return <ErrorFallback />;
    }
    return this.props.children;
  }
}
```

## 日志格式

```json
{
  "timestamp": "2024-01-15T10:30:00Z",
  "level": "error",
  "message": "Failed to process order",
  "context": {
    "orderId": "12345",
    "userId": "67890"
  },
  "error": {
    "name": "ValidationError",
    "message": "Invalid amount",
    "stack": "..."
  }
}
```

## 日志级别

| 级别 | 用途 |
|-----|------|
| error | 需要立即处理的错误 |
| warn | 潜在问题，需要关注 |
| info | 重要业务事件 |
| debug | 调试信息 |
