---
name: test-generator
description: |
  自动生成测试用例。用于单元测试、集成测试、E2E 测试、
  Mock 数据生成等场景。关键词：unit test, integration,
  e2e, mock, jest, pytest, vitest
---

# Test Generator 测试生成器

帮助生成各类测试用例和测试数据。

## 单元测试模板

### JavaScript/TypeScript (Jest/Vitest)

```typescript
import { describe, it, expect, beforeEach, vi } from 'vitest';
import { UserService } from './user.service';

describe('UserService', () => {
  let service: UserService;

  beforeEach(() => {
    service = new UserService();
    vi.clearAllMocks();
  });

  describe('createUser', () => {
    it('should create user with valid data', async () => {
      const input = { name: 'Test', email: 'test@example.com' };
      const result = await service.createUser(input);

      expect(result).toMatchObject({
        id: expect.any(Number),
        name: 'Test',
        email: 'test@example.com'
      });
    });

    it('should throw error for duplicate email', async () => {
      const input = { name: 'Test', email: 'existing@example.com' };

      await expect(service.createUser(input))
        .rejects.toThrow('Email already exists');
    });
  });
});
```

### Python (pytest)

```python
import pytest
from unittest.mock import Mock, patch
from user_service import UserService

class TestUserService:
    @pytest.fixture
    def service(self):
        return UserService()

    def test_create_user_success(self, service):
        result = service.create_user(
            name="Test",
            email="test@example.com"
        )

        assert result["id"] is not None
        assert result["name"] == "Test"

    def test_create_user_duplicate_email(self, service):
        with pytest.raises(ValueError, match="Email already exists"):
            service.create_user(
                name="Test",
                email="existing@example.com"
            )

    @patch('user_service.send_email')
    def test_send_welcome_email(self, mock_send, service):
        service.create_user(name="Test", email="test@example.com")

        mock_send.assert_called_once_with(
            to="test@example.com",
            subject="Welcome!"
        )
```

### Go

```go
package user

import (
    "testing"
    "github.com/stretchr/testify/assert"
    "github.com/stretchr/testify/require"
)

func TestUserService_CreateUser(t *testing.T) {
    t.Run("success", func(t *testing.T) {
        svc := NewUserService()

        user, err := svc.CreateUser("Test", "test@example.com")

        require.NoError(t, err)
        assert.NotZero(t, user.ID)
        assert.Equal(t, "Test", user.Name)
    })

    t.Run("duplicate email", func(t *testing.T) {
        svc := NewUserService()

        _, err := svc.CreateUser("Test", "existing@example.com")

        assert.ErrorContains(t, err, "email already exists")
    })
}
```

## Mock 数据生成

```typescript
// 使用 faker.js
import { faker } from '@faker-js/faker';

const mockUser = () => ({
  id: faker.number.int({ min: 1, max: 10000 }),
  name: faker.person.fullName(),
  email: faker.internet.email(),
  avatar: faker.image.avatar(),
  createdAt: faker.date.past()
});

const mockUsers = (count: number) =>
  Array.from({ length: count }, mockUser);
```

## 测试覆盖率目标

| 类型 | 最低覆盖率 |
|-----|----------|
| 核心业务逻辑 | 80% |
| 工具函数 | 90% |
| API 端点 | 70% |
| UI 组件 | 60% |
