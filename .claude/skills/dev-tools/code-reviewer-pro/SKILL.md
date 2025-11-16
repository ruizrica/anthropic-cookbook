---
name: code-reviewer-pro
description: Comprehensive code review focusing on security, performance, best practices, and maintainability
---

# Code Reviewer Pro

## Review Checklist

### Security
- [ ] No hardcoded secrets/API keys
- [ ] Input validation and sanitization
- [ ] SQL injection prevention (parameterized queries)
- [ ] XSS prevention (escape output)
- [ ] CSRF tokens on forms
- [ ] Proper authentication/authorization
- [ ] Secure password hashing (bcrypt, Argon2)

### Performance
- [ ] No N+1 queries
- [ ] Efficient algorithms (O(n) not O(n²))
- [ ] Proper indexing on database queries
- [ ] Caching where appropriate
- [ ] Lazy loading for heavy resources
- [ ] Pagination for large datasets

### Best Practices
- [ ] DRY (Don't Repeat Yourself)
- [ ] SOLID principles
- [ ] Clear variable/function names
- [ ] Small, focused functions (< 50 lines)
- [ ] Proper error handling
- [ ] Consistent code style

### Testing
- [ ] Unit tests for business logic
- [ ] Edge cases covered
- [ ] Mock external dependencies
- [ ] Test coverage > 80%

## Common Issues

```typescript
// ❌ Security risk
const query = `SELECT * FROM users WHERE id = ${req.params.id}`;

// ✅ Parameterized query
const query = 'SELECT * FROM users WHERE id = $1';
const result = await db.query(query, [req.params.id]);

// ❌ Performance issue
const users = await User.find();
for (const user of users) {
  user.posts = await Post.find({ userId: user.id });
}

// ✅ Optimized
const users = await User.find().populate('posts');

// ❌ Error swallowing
try {
  await riskyOperation();
} catch (e) {
  console.log(e);
}

// ✅ Proper error handling
try {
  await riskyOperation();
} catch (error) {
  logger.error('Operation failed', { error, context });
  throw new AppError('Operation failed', 500);
}
```
