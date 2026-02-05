---
name: refactor-assistant
description: |
  代码重构辅助。用于提取函数、重命名、消除重复代码、
  优化结构、设计模式应用等。关键词：refactor, extract,
  rename, DRY, SOLID, pattern
---

# Refactor Assistant 重构助手

帮助进行代码重构，提升代码质量。

## 常见重构模式

### 1. 提取函数

```typescript
// Before
function processOrder(order: Order) {
  // 验证订单
  if (!order.items.length) throw new Error('Empty order');
  if (!order.userId) throw new Error('No user');
  if (order.total < 0) throw new Error('Invalid total');

  // 计算折扣
  let discount = 0;
  if (order.total > 100) discount = order.total * 0.1;
  if (order.total > 500) discount = order.total * 0.2;

  // 处理支付...
}

// After
function processOrder(order: Order) {
  validateOrder(order);
  const discount = calculateDiscount(order.total);
  processPayment(order, discount);
}

function validateOrder(order: Order) {
  if (!order.items.length) throw new Error('Empty order');
  if (!order.userId) throw new Error('No user');
  if (order.total < 0) throw new Error('Invalid total');
}

function calculateDiscount(total: number): number {
  if (total > 500) return total * 0.2;
  if (total > 100) return total * 0.1;
  return 0;
}
```

### 2. 消除重复 (DRY)

```typescript
// Before
async function getUser(id: number) {
  try {
    const res = await fetch(`/api/users/${id}`);
    if (!res.ok) throw new Error('Failed');
    return res.json();
  } catch (e) {
    console.error(e);
    throw e;
  }
}

async function getOrder(id: number) {
  try {
    const res = await fetch(`/api/orders/${id}`);
    if (!res.ok) throw new Error('Failed');
    return res.json();
  } catch (e) {
    console.error(e);
    throw e;
  }
}

// After
async function fetchApi<T>(endpoint: string): Promise<T> {
  try {
    const res = await fetch(endpoint);
    if (!res.ok) throw new Error('Failed');
    return res.json();
  } catch (e) {
    console.error(e);
    throw e;
  }
}

const getUser = (id: number) => fetchApi<User>(`/api/users/${id}`);
const getOrder = (id: number) => fetchApi<Order>(`/api/orders/${id}`);
```

### 3. 策略模式替换条件

```typescript
// Before
function calculatePrice(type: string, base: number) {
  if (type === 'vip') return base * 0.8;
  if (type === 'member') return base * 0.9;
  if (type === 'new') return base * 0.95;
  return base;
}

// After
const priceStrategies: Record<string, (base: number) => number> = {
  vip: (base) => base * 0.8,
  member: (base) => base * 0.9,
  new: (base) => base * 0.95,
  default: (base) => base,
};

function calculatePrice(type: string, base: number) {
  const strategy = priceStrategies[type] || priceStrategies.default;
  return strategy(base);
}
```

## 重构检查清单

- [ ] 函数是否过长 (>30行)?
- [ ] 是否有重复代码?
- [ ] 命名是否清晰?
- [ ] 参数是否过多 (>3个)?
- [ ] 嵌套是否过深 (>3层)?
- [ ] 是否违反单一职责?

## SOLID 原则速查

| 原则 | 含义 |
|-----|------|
| S | 单一职责 - 一个类只做一件事 |
| O | 开闭原则 - 对扩展开放，对修改关闭 |
| L | 里氏替换 - 子类可替换父类 |
| I | 接口隔离 - 接口要小而专 |
| D | 依赖倒置 - 依赖抽象而非具体 |
