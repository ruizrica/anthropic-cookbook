---
allowed-tools: Read(*),Grep(*),Glob(*)
description: Perform comprehensive code review checking security, performance, and best practices
---

# Code Review Command

Review the code changes in the current branch for:

1. **Security vulnerabilities**:
   - SQL injection risks
   - XSS vulnerabilities
   - Hardcoded secrets
   - Authentication/authorization issues

2. **Performance issues**:
   - N+1 queries
   - Inefficient algorithms
   - Missing indexes
   - Unnecessary API calls

3. **Best practices**:
   - Code duplication
   - Proper error handling
   - Clear naming conventions
   - Function complexity

4. **Testing**:
   - Adequate test coverage
   - Edge cases handled
   - Mocks for external dependencies

Use the `code-reviewer-pro` skill to analyze the code and provide actionable feedback.

**Output Format**:
```markdown
## Security Issues
- [CRITICAL] SQL injection risk in user-service.ts:45
- [HIGH] Hardcoded API key in config.ts:12

## Performance Issues
- [MEDIUM] N+1 query in getUsers() function

## Best Practices
- [LOW] Duplicate code in auth.ts and user.ts

## Recommendations
1. Use parameterized queries
2. Move secrets to environment variables
3. Optimize database queries with joins
```
