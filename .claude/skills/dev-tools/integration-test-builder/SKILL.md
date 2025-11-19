---
name: integration-test-builder
description: Build integration tests for APIs, databases, and external services with proper setup and teardown
---

# Integration Test Builder

## API Integration Tests

```typescript
import request from 'supertest';
import { app } from '../app';
import { db } from '../database';

describe('User API', () => {
  beforeAll(async () => {
    await db.connect();
  });

  afterAll(async () => {
    await db.disconnect();
  });

  beforeEach(async () => {
    await db.clear();
  });

  describe('POST /api/users', () => {
    it('should create new user', async () => {
      const response = await request(app)
        .post('/api/users')
        .send({ email: 'test@example.com', name: 'Test User' })
        .expect(201);

      expect(response.body.id).toBeDefined();
      expect(response.body.email).toBe('test@example.com');

      // Verify in database
      const user = await db.users.findById(response.body.id);
      expect(user).toBeDefined();
    });

    it('should return 400 for invalid email', async () => {
      await request(app)
        .post('/api/users')
        .send({ email: 'invalid-email', name: 'Test' })
        .expect(400);
    });
  });

  describe('GET /api/users/:id', () => {
    it('should return user by id', async () => {
      const user = await db.users.create({
        email: 'test@example.com',
        name: 'Test'
      });

      const response = await request(app)
        .get(`/api/users/${user.id}`)
        .expect(200);

      expect(response.body.id).toBe(user.id);
    });

    it('should return 404 for non-existent user', async () => {
      await request(app)
        .get('/api/users/999')
        .expect(404);
    });
  });
});
```

## Database Integration Tests

```typescript
describe('UserRepository', () => {
  beforeEach(async () => {
    await db.migrate.latest();
    await db.seed.run();
  });

  afterEach(async () => {
    await db.migrate.rollback();
  });

  it('should save and retrieve user', async () => {
    const userData = { email: 'test@example.com', name: 'Test' };
    const userId = await userRepo.create(userData);

    const user = await userRepo.findById(userId);

    expect(user.email).toBe(userData.email);
    expect(user.createdAt).toBeInstanceOf(Date);
  });

  it('should enforce unique email constraint', async () => {
    await userRepo.create({ email: 'test@example.com' });

    await expect(
      userRepo.create({ email: 'test@example.com' })
    ).rejects.toThrow('Email already exists');
  });
});
```

## External Service Mocking

```typescript
import nock from 'nock';

describe('Payment Service', () => {
  afterEach(() => {
    nock.cleanAll();
  });

  it('should process payment via Stripe', async () => {
    nock('https://api.stripe.com')
      .post('/v1/charges')
      .reply(200, {
        id: 'ch_123',
        status: 'succeeded',
        amount: 1000
      });

    const result = await paymentService.charge({
      amount: 1000,
      token: 'tok_123'
    });

    expect(result.status).toBe('succeeded');
  });

  it('should handle payment failure', async () => {
    nock('https://api.stripe.com')
      .post('/v1/charges')
      .reply(402, {
        error: { message: 'Insufficient funds' }
      });

    await expect(
      paymentService.charge({ amount: 1000, token: 'tok_123' })
    ).rejects.toThrow('Payment failed');
  });
});
```

## Test Containers (Docker)

```typescript
import { GenericContainer } from 'testcontainers';

describe('Redis Integration', () => {
  let container;
  let redisClient;

  beforeAll(async () => {
    container = await new GenericContainer('redis')
      .withExposedPorts(6379)
      .start();

    const port = container.getMappedPort(6379);
    redisClient = createRedisClient({ port });
  });

  afterAll(async () => {
    await redisClient.quit();
    await container.stop();
  });

  it('should cache data in Redis', async () => {
    await redisClient.set('key', 'value');
    const result = await redisClient.get('key');

    expect(result).toBe('value');
  });
});
```
