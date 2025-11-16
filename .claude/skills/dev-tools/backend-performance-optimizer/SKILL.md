---
name: backend-performance-optimizer
description: Optimize backend with caching, query optimization, and performance profiling
---

# Backend Performance Optimizer

## Database Optimization
```typescript
// ❌ N+1 query
const users = await User.findAll();
for (const user of users) {
  user.posts = await Post.find({ userId: user.id });
}

// ✅ Single query with join
const users = await User.findAll({ include: [Post] });
```

## Redis Caching
```typescript
async function getUser(id: string) {
  const cached = await redis.get(`user:${id}`);
  if (cached) return JSON.parse(cached);

  const user = await User.findById(id);
  await redis.setex(`user:${id}`, 3600, JSON.stringify(user));
  return user;
}
```

## Connection Pooling
```typescript
const pool = new Pool({ max: 20, idleTimeoutMillis: 30000 });
const client = await pool.connect();
try {
  return await client.query('SELECT * FROM users');
} finally {
  client.release();
}
```

## HTTP Caching
```typescript
res.set('Cache-Control', 'public, max-age=300');
res.set('ETag', generateETag(data));
```
