---
name: microservices-architect
description: Design microservices with service discovery, API gateways, and resilient communication patterns
---

# Microservices Architect

## Patterns

### 1. API Gateway
```typescript
// Route requests to appropriate services
app.get('/api/orders/:id', async (req, res) => {
  const order = await orderService.get(req.params.id);
  const user = await userService.get(order.userId);
  res.json({ order, user });
});
```

### 2. Service Registry
```javascript
// Consul/Eureka registration
consul.agent.service.register({
  name: 'user-service',
  port: 3000,
  check: { http: 'http://localhost:3000/health', interval: '10s' }
});
```

### 3. Circuit Breaker
```typescript
const breaker = new CircuitBreaker(externalCall, {
  timeout: 3000,
  errorThresholdPercentage: 50,
  resetTimeout: 30000
});
```

### 4. Event-Driven (Message Queue)
```typescript
// Publisher
await queue.publish('order.created', { orderId, userId });

// Subscriber
queue.subscribe('order.created', async (msg) => {
  await updateInventory(msg.orderId);
});
```
