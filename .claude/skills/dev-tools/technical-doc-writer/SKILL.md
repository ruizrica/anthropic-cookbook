---
name: technical-doc-writer
description: Write comprehensive technical documentation including architecture docs, ADRs, and developer guides
---

# Technical Documentation Writer

## README.md Template

```markdown
# Project Name

Brief description of what this project does.

## Features

- Feature 1
- Feature 2
- Feature 3

## Prerequisites

- Node.js >= 18
- PostgreSQL >= 15
- Redis >= 7

## Installation

\`\`\`bash
npm install
cp .env.example .env
npm run db:migrate
\`\`\`

## Usage

\`\`\`bash
npm run dev     # Development
npm run build   # Production build
npm start       # Start production
\`\`\`

## Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| PORT | Server port | 3000 |
| DATABASE_URL | PostgreSQL connection | - |
| REDIS_URL | Redis connection | localhost:6379 |

## Testing

\`\`\`bash
npm test              # Run all tests
npm run test:watch    # Watch mode
npm run test:coverage # Coverage report
\`\`\`

## API Documentation

See [API.md](./docs/API.md) for detailed API documentation.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md)

## License

MIT
```

## Architecture Decision Record (ADR)

```markdown
# ADR-001: Use PostgreSQL for Primary Database

## Status

Accepted

## Context

We need a relational database for our application that supports:
- Complex queries with joins
- ACID transactions
- JSON data types
- Full-text search

## Decision

We will use PostgreSQL as our primary database.

## Consequences

### Positive

- Robust ACID compliance
- Excellent JSON support via JSONB
- Strong community and tooling
- Good performance for complex queries

### Negative

- Requires more operational overhead than managed NoSQL
- Horizontal scaling is more complex
- Team needs PostgreSQL expertise

## Alternatives Considered

- MySQL: Less robust JSON support
- MongoDB: Eventual consistency challenges
- DynamoDB: Vendor lock-in, query limitations
```

## API Documentation

```markdown
# API Documentation

## Authentication

All endpoints require Bearer token authentication:

\`\`\`
Authorization: Bearer <token>
\`\`\`

## Endpoints

### Get User

\`\`\`
GET /api/users/:id
\`\`\`

**Parameters:**
- `id` (string) - User ID

**Response:**
\`\`\`json
{
  "id": "123",
  "email": "user@example.com",
  "name": "John Doe",
  "createdAt": "2024-01-01T00:00:00Z"
}
\`\`\`

**Errors:**
- 404: User not found
- 401: Unauthorized
```

## Code Documentation

```typescript
/**
 * User service for managing user accounts
 *
 * @example
 * ```typescript
 * const user = await userService.getUser('123');
 * console.log(user.email);
 * ```
 */
export class UserService {
  /**
   * Get user by ID
   *
   * @param id - User ID
   * @returns User object
   * @throws {NotFoundError} When user doesn't exist
   */
  async getUser(id: string): Promise<User> {
    // Implementation
  }
}
```
