---
name: docker-compose
description: |
  Docker 容器编排和服务管理。用于编写 docker-compose 配置、
  多容器部署、网络配置、数据卷管理等。关键词：docker, compose,
  container, volume, network, service
---

# Docker Compose 容器编排

帮助编写和管理 Docker Compose 配置。

## 基础模板

```yaml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    image: myapp:latest
    container_name: myapp
    restart: unless-stopped
    ports:
      - "8080:80"
    environment:
      - NODE_ENV=production
    env_file:
      - .env
    volumes:
      - ./data:/app/data
    depends_on:
      - db
      - redis
    networks:
      - app-network

  db:
    image: postgres:15-alpine
    container_name: myapp-db
    restart: unless-stopped
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres-data:/var/lib/postgresql/data
    networks:
      - app-network

  redis:
    image: redis:7-alpine
    container_name: myapp-redis
    restart: unless-stopped
    command: redis-server --appendonly yes
    volumes:
      - redis-data:/data
    networks:
      - app-network

volumes:
  postgres-data:
  redis-data:

networks:
  app-network:
    driver: bridge
```

## 常用服务配置

### Nginx 反向代理

```yaml
nginx:
  image: nginx:alpine
  ports:
    - "80:80"
    - "443:443"
  volumes:
    - ./nginx.conf:/etc/nginx/nginx.conf:ro
    - ./certs:/etc/nginx/certs:ro
  depends_on:
    - app
```

### MySQL

```yaml
mysql:
  image: mysql:8
  environment:
    MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
    MYSQL_DATABASE: myapp
    MYSQL_USER: app
    MYSQL_PASSWORD: ${MYSQL_PASSWORD}
  volumes:
    - mysql-data:/var/lib/mysql
    - ./init.sql:/docker-entrypoint-initdb.d/init.sql
  command: --default-authentication-plugin=mysql_native_password
```

### MongoDB

```yaml
mongo:
  image: mongo:6
  environment:
    MONGO_INITDB_ROOT_USERNAME: admin
    MONGO_INITDB_ROOT_PASSWORD: ${MONGO_PASSWORD}
  volumes:
    - mongo-data:/data/db
```

## 健康检查

```yaml
services:
  app:
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s
```

## 资源限制

```yaml
services:
  app:
    deploy:
      resources:
        limits:
          cpus: '0.5'
          memory: 512M
        reservations:
          cpus: '0.25'
          memory: 256M
```

## 常用命令

```bash
# 启动服务
docker-compose up -d

# 查看日志
docker-compose logs -f app

# 重建并启动
docker-compose up -d --build

# 停止服务
docker-compose down

# 停止并删除数据卷
docker-compose down -v

# 查看服务状态
docker-compose ps

# 进入容器
docker-compose exec app sh

# 扩展服务实例
docker-compose up -d --scale app=3
```

## 多环境配置

```bash
# docker-compose.override.yml (开发环境自动加载)
# docker-compose.prod.yml (生产环境)

# 使用生产配置
docker-compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```
