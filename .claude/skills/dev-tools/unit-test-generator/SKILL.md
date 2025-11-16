---
name: unit-test-generator
description: Generate comprehensive unit tests with Jest, Vitest, and pytest for various programming languages
---

# Unit Test Generator

## Jest/Vitest (JavaScript/TypeScript)

```typescript
// math.ts
export function add(a: number, b: number): number {
  return a + b;
}

export function divide(a: number, b: number): number {
  if (b === 0) throw new Error('Division by zero');
  return a / b;
}

// math.test.ts
import { add, divide } from './math';

describe('Math functions', () => {
  describe('add', () => {
    it('should add two positive numbers', () => {
      expect(add(2, 3)).toBe(5);
    });

    it('should handle negative numbers', () => {
      expect(add(-2, 3)).toBe(1);
    });

    it('should handle zero', () => {
      expect(add(0, 5)).toBe(5);
    });
  });

  describe('divide', () => {
    it('should divide two numbers', () => {
      expect(divide(10, 2)).toBe(5);
    });

    it('should throw error when dividing by zero', () => {
      expect(() => divide(10, 0)).toThrow('Division by zero');
    });

    it('should handle negative results', () => {
      expect(divide(-10, 2)).toBe(-5);
    });
  });
});
```

## Mocking

```typescript
// userService.test.ts
import { UserService } from './userService';
import { Database } from './database';

jest.mock('./database');

describe('UserService', () => {
  let userService: UserService;
  let mockDb: jest.Mocked<Database>;

  beforeEach(() => {
    mockDb = new Database() as jest.Mocked<Database>;
    userService = new UserService(mockDb);
  });

  it('should fetch user from database', async () => {
    const mockUser = { id: '1', name: 'John' };
    mockDb.findUser.mockResolvedValue(mockUser);

    const user = await userService.getUser('1');

    expect(mockDb.findUser).toHaveBeenCalledWith('1');
    expect(user).toEqual(mockUser);
  });
});
```

## Pytest (Python)

```python
# calculator.py
def add(a: int, b: int) -> int:
    return a + b

def divide(a: int, b: int) -> float:
    if b == 0:
        raise ValueError("Division by zero")
    return a / b

# test_calculator.py
import pytest
from calculator import add, divide

def test_add_positive_numbers():
    assert add(2, 3) == 5

def test_add_negative_numbers():
    assert add(-2, 3) == 1

def test_divide():
    assert divide(10, 2) == 5.0

def test_divide_by_zero_raises_error():
    with pytest.raises(ValueError, match="Division by zero"):
        divide(10, 0)

@pytest.mark.parametrize("a,b,expected", [
    (10, 2, 5),
    (20, 4, 5),
    (100, 10, 10),
])
def test_divide_parametrized(a, b, expected):
    assert divide(a, b) == expected
```

## Test Fixtures

```typescript
// fixtures.ts
export const mockUser = {
  id: '123',
  email: 'test@example.com',
  name: 'Test User'
};

export const mockPost = {
  id: '456',
  userId: '123',
  title: 'Test Post',
  content: 'Content'
};

// test.ts
import { mockUser, mockPost } from './fixtures';

it('should link user to post', () => {
  expect(mockPost.userId).toBe(mockUser.id);
});
```

## Snapshot Testing

```typescript
it('should render component correctly', () => {
  const component = render(<UserCard user={mockUser} />);
  expect(component).toMatchSnapshot();
});
```
