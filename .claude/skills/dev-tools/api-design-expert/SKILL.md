---
name: api-design-expert
description: Design RESTful and GraphQL APIs with best practices, versioning, authentication, rate limiting, and comprehensive documentation
---

# API Design Expert Skill

## RESTful API Design Principles

### 1. Resource-Based URLs
```
✅ Good:
GET    /api/users
GET    /api/users/123
POST   /api/users
PUT    /api/users/123
PATCH  /api/users/123
DELETE /api/users/123

❌ Bad:
GET /api/getUser?id=123
POST /api/createUser
```

### 2. HTTP Methods & Status Codes
| Method | Purpose | Success Status | Example |
|--------|---------|----------------|---------|
| GET | Retrieve resource | 200 OK | Get user list |
| POST | Create resource | 201 Created | Create user |
| PUT | Replace resource | 200 OK | Update entire user |
| PATCH | Partial update | 200 OK | Update user email |
| DELETE | Remove resource | 204 No Content | Delete user |

### 3. Standard Response Format
```json
{
  "data": {
    "id": "123",
    "type": "user",
    "attributes": {
      "name": "John Doe",
      "email": "john@example.com"
    }
  },
  "meta": {
    "timestamp": "2024-01-01T00:00:00Z"
  }
}
```

### 4. Error Handling
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid email format",
    "details": [
      {
        "field": "email",
        "message": "Must be a valid email address"
      }
    ]
  }
}
```

### 5. Pagination
```
GET /api/users?page=2&limit=20
GET /api/users?offset=40&limit=20
GET /api/users?cursor=eyJpZCI6MTIzfQ==
```

```json
{
  "data": [...],
  "pagination": {
    "page": 2,
    "limit": 20,
    "total": 100,
    "pages": 5
  },
  "links": {
    "first": "/api/users?page=1",
    "prev": "/api/users?page=1",
    "next": "/api/users?page=3",
    "last": "/api/users?page=5"
  }
}
```

### 6. Filtering & Sorting
```
GET /api/users?role=admin&status=active
GET /api/users?sort=-created_at,name
GET /api/products?price[gte]=100&price[lte]=500
```

### 7. Versioning
```
# URL versioning
/api/v1/users
/api/v2/users

# Header versioning
Accept: application/vnd.myapi.v2+json

# Query parameter versioning
/api/users?version=2
```

## GraphQL API Design

### 1. Schema Definition
```graphql
type User {
  id: ID!
  name: String!
  email: String!
  posts: [Post!]!
  createdAt: DateTime!
}

type Post {
  id: ID!
  title: String!
  content: String!
  author: User!
  published: Boolean!
}

type Query {
  user(id: ID!): User
  users(limit: Int, offset: Int): [User!]!
  post(id: ID!): Post
}

type Mutation {
  createUser(input: CreateUserInput!): User!
  updateUser(id: ID!, input: UpdateUserInput!): User!
  deleteUser(id: ID!): Boolean!
}

input CreateUserInput {
  name: String!
  email: String!
  password: String!
}
```

### 2. Queries
```graphql
query GetUser {
  user(id: "123") {
    id
    name
    email
    posts {
      id
      title
    }
  }
}

query GetUsers {
  users(limit: 10) {
    id
    name
    email
  }
}
```

### 3. Mutations
```graphql
mutation CreateUser {
  createUser(input: {
    name: "John Doe"
    email: "john@example.com"
    password: "secure123"
  }) {
    id
    name
    email
  }
}
```

## Authentication & Authorization

### 1. JWT (JSON Web Tokens)
```typescript
// Generate token
const token = jwt.sign(
  { userId: user.id, role: user.role },
  process.env.JWT_SECRET,
  { expiresIn: '1h' }
);

// Verify token
const decoded = jwt.verify(token, process.env.JWT_SECRET);

// Middleware
function authenticateToken(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1];
  if (!token) return res.sendStatus(401);

  jwt.verify(token, process.env.JWT_SECRET, (err, user) => {
    if (err) return res.sendStatus(403);
    req.user = user;
    next();
  });
}
```

### 2. API Keys
```typescript
function validateApiKey(req, res, next) {
  const apiKey = req.headers['x-api-key'];

  if (!apiKey || !isValidApiKey(apiKey)) {
    return res.status(401).json({ error: 'Invalid API key' });
  }

  next();
}
```

### 3. OAuth 2.0
```typescript
// Authorization Code Flow
app.get('/oauth/authorize', (req, res) => {
  const { client_id, redirect_uri, scope, state } = req.query;
  // Show login/consent screen
});

app.post('/oauth/token', async (req, res) => {
  const { code, client_id, client_secret } = req.body;

  // Exchange code for access token
  const token = await generateAccessToken(code);

  res.json({
    access_token: token,
    token_type: 'Bearer',
    expires_in: 3600
  });
});
```

## Rate Limiting

```typescript
import rateLimit from 'express-rate-limit';

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP to 100 requests per window
  message: 'Too many requests, please try again later',
  standardHeaders: true,
  legacyHeaders: false,
});

app.use('/api/', limiter);

// Response headers
// X-RateLimit-Limit: 100
// X-RateLimit-Remaining: 99
// X-RateLimit-Reset: 1640995200
```

## API Documentation (OpenAPI/Swagger)

```yaml
openapi: 3.0.0
info:
  title: My API
  version: 1.0.0
  description: Comprehensive API for managing users

servers:
  - url: https://api.example.com/v1

paths:
  /users:
    get:
      summary: List users
      parameters:
        - name: page
          in: query
          schema:
            type: integer
        - name: limit
          in: query
          schema:
            type: integer
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                type: object
                properties:
                  data:
                    type: array
                    items:
                      $ref: '#/components/schemas/User'

    post:
      summary: Create user
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUserInput'
      responses:
        '201':
          description: User created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'

components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: string
        name:
          type: string
        email:
          type: string
          format: email

  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

security:
  - BearerAuth: []
```

## Best Practices

1. **Use nouns, not verbs** in URLs
2. **Version your API** from the start
3. **Always use HTTPS** in production
4. **Implement rate limiting** to prevent abuse
5. **Provide comprehensive error messages**
6. **Use consistent naming** (camelCase or snake_case)
7. **Support CORS** for web clients
8. **Implement caching** with ETags/Cache-Control
9. **Log all requests** for debugging
10. **Monitor performance** and set SLAs
