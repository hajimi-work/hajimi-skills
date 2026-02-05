---
name: db-migration
description: |
  数据库迁移和版本管理。用于创建迁移脚本、数据库变更、
  回滚操作、数据迁移等场景。关键词：migration, schema,
  alter table, rollback, seed, SQL
---

# DB Migration 数据库迁移

帮助管理数据库 schema 变更和数据迁移。

## 迁移文件命名规范

```
<timestamp>_<description>.<up|down>.sql

# 示例
20240101120000_create_users_table.up.sql
20240101120000_create_users_table.down.sql
20240102100000_add_email_to_users.up.sql
20240102100000_add_email_to_users.down.sql
```

## 常用迁移模板

### 创建表

```sql
-- up
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(255) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    status SMALLINT DEFAULT 1,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_status ON users(status);

-- down
DROP TABLE IF EXISTS users;
```

### 添加字段

```sql
-- up
ALTER TABLE users ADD COLUMN phone VARCHAR(20);
ALTER TABLE users ADD COLUMN avatar_url TEXT;

-- down
ALTER TABLE users DROP COLUMN IF EXISTS phone;
ALTER TABLE users DROP COLUMN IF EXISTS avatar_url;
```

### 修改字段

```sql
-- up
ALTER TABLE users ALTER COLUMN username TYPE VARCHAR(100);
ALTER TABLE users ALTER COLUMN status SET DEFAULT 0;

-- down
ALTER TABLE users ALTER COLUMN username TYPE VARCHAR(50);
ALTER TABLE users ALTER COLUMN status SET DEFAULT 1;
```

### 添加索引

```sql
-- up
CREATE INDEX CONCURRENTLY idx_users_created_at
ON users(created_at DESC);

-- down
DROP INDEX IF EXISTS idx_users_created_at;
```

### 添加外键

```sql
-- up
ALTER TABLE orders
ADD CONSTRAINT fk_orders_user_id
FOREIGN KEY (user_id) REFERENCES users(id)
ON DELETE CASCADE;

-- down
ALTER TABLE orders
DROP CONSTRAINT IF EXISTS fk_orders_user_id;
```

## 数据迁移示例

```sql
-- 拆分姓名字段
-- up
ALTER TABLE users ADD COLUMN first_name VARCHAR(50);
ALTER TABLE users ADD COLUMN last_name VARCHAR(50);

UPDATE users SET
    first_name = SPLIT_PART(full_name, ' ', 1),
    last_name = SPLIT_PART(full_name, ' ', 2)
WHERE full_name IS NOT NULL;

ALTER TABLE users DROP COLUMN full_name;

-- down
ALTER TABLE users ADD COLUMN full_name VARCHAR(100);

UPDATE users SET
    full_name = CONCAT(first_name, ' ', last_name)
WHERE first_name IS NOT NULL;

ALTER TABLE users DROP COLUMN first_name;
ALTER TABLE users DROP COLUMN last_name;
```

## 迁移工具命令

### golang-migrate

```bash
# 创建迁移
migrate create -ext sql -dir migrations -seq create_users

# 执行迁移
migrate -path migrations -database "postgres://..." up

# 回滚
migrate -path migrations -database "postgres://..." down 1

# 强制版本
migrate -path migrations -database "postgres://..." force 20240101
```

### Prisma

```bash
# 创建迁移
npx prisma migrate dev --name add_users

# 部署迁移
npx prisma migrate deploy

# 重置数据库
npx prisma migrate reset
```

## 最佳实践

1. **始终编写回滚脚本**: 每个 up 必须有对应的 down
2. **小步迭代**: 每次迁移只做一件事
3. **测试迁移**: 在测试环境验证后再上生产
4. **备份数据**: 重大变更前备份
5. **避免锁表**: 大表操作使用 CONCURRENTLY
