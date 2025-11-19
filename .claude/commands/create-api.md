---
allowed-tools: Write(*),Read(*),Glob(*)
description: Generate RESTful API or GraphQL API with routes, controllers, validation, and OpenAPI documentation
---

# Create API Command

Generate a complete API implementation with best practices.

**Input Required**:
- API type: REST or GraphQL
- Resource name (e.g., "users", "products")
- Fields and their types
- Relationships to other resources

**Generated Components**:

1. **Routes/Resolvers**:
   - GET /api/resource (list)
   - GET /api/resource/:id (get one)
   - POST /api/resource (create)
   - PUT /api/resource/:id (update)
   - PATCH /api/resource/:id (partial update)
   - DELETE /api/resource/:id (delete)

2. **Controllers/Services**:
   - Business logic
   - Validation
   - Error handling
   - Authentication/authorization

3. **Data Models**:
   - Database schema
   - TypeScript interfaces
   - Validation schemas (Zod, Joi, etc.)

4. **Documentation**:
   - OpenAPI/Swagger spec
   - Request/response examples
   - Authentication requirements

5. **Tests**:
   - Unit tests for controllers
   - Integration tests for endpoints

Use the `api-design-expert` and `api-documentation-generator` skills.

**Best Practices Applied**:
- RESTful URL conventions
- Proper HTTP status codes
- Pagination for lists
- Filtering and sorting
- Rate limiting
- Error handling
- Input validation
