---
name: api-documentation-generator
description: Generate API docs using OpenAPI/Swagger, TSDoc, and interactive documentation
---

# API Documentation Generator

## OpenAPI 3.0
```yaml
openapi: 3.0.0
paths:
  /users/{id}:
    get:
      summary: Get user
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      responses:
        '200':
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/User'
components:
  schemas:
    User:
      type: object
      properties:
        id: { type: string }
        email: { type: string }
```

## TSDoc
```typescript
/**
 * Get user by ID
 * @param id - User ID
 * @returns User object
 * @throws {NotFoundError}
 */
async getUser(id: string): Promise<User>
```

## Tools
- Swagger UI
- ReDoc
- Postman
- API Blueprint
