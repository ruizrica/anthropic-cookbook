---
name: test-strategy-planner
description: Design comprehensive testing strategies with the test pyramid, coverage goals, and quality metrics
---

# Test Strategy Planner

## Test Pyramid

```
        /\
       /E2E\        10% - End-to-End (Slow, Brittle)
      /------\
     /Integr-\      20% - Integration (Medium)
    /----------\
   /  Unit Tests \  70% - Unit Tests (Fast, Stable)
  /--------------\
```

## Test Types

### 1. Unit Tests (70%)
- Test individual functions/methods
- Fast execution (< 10ms each)
- No external dependencies
- High code coverage (> 80%)

### 2. Integration Tests (20%)
- Test module interactions
- Database, API calls
- Medium speed
- Critical paths covered

### 3. E2E Tests (10%)
- Test complete user journeys
- Real browser, full stack
- Slow but high confidence
- Happy path + critical flows

## Coverage Goals

```json
{
  "branches": 80,
  "functions": 85,
  "lines": 85,
  "statements": 85
}
```

## Test Naming Convention

```typescript
// Given-When-Then pattern
describe('UserService', () => {
  describe('createUser', () => {
    it('should create user when valid data provided', async () => {
      // Given
      const userData = { email: 'test@example.com', name: 'Test' };

      // When
      const user = await userService.createUser(userData);

      // Then
      expect(user.id).toBeDefined();
      expect(user.email).toBe(userData.email);
    });

    it('should throw error when email already exists', async () => {
      // Given
      await userService.createUser({ email: 'test@example.com' });

      // When/Then
      await expect(
        userService.createUser({ email: 'test@example.com' })
      ).rejects.toThrow('Email already exists');
    });
  });
});
```

## What to Test

✅ **Do test:**
- Business logic
- Edge cases (null, undefined, empty)
- Error handling
- Public APIs
- Critical user journeys

❌ **Don't test:**
- Third-party libraries
- Framework internals
- Trivial getters/setters
- Private methods (test through public API)

## Quality Metrics

1. **Test Coverage**: > 80%
2. **Test Speed**: Unit tests < 5 min total
3. **Flakiness**: < 1% failure rate
4. **CI Success Rate**: > 95%
5. **Bug Escape Rate**: < 5%
