---
allowed-tools: Read(*),Grep(*),Bash(npm audit,snyk),Glob(*)
description: Perform comprehensive security audit checking for OWASP Top 10 vulnerabilities and dependency issues
---

# Security Scan Command

Perform comprehensive security audit of the codebase.

**Scans**:

1. **Dependency Vulnerabilities**:
   ```bash
   npm audit
   npm audit fix --dry-run
   ```

2. **OWASP Top 10 Checks**:
   - Broken Access Control
   - Cryptographic Failures
   - Injection vulnerabilities
   - Insecure Design
   - Security Misconfiguration
   - Vulnerable Components
   - Authentication Failures
   - Software/Data Integrity Failures
   - Logging/Monitoring Failures
   - SSRF

3. **Code Security Issues**:
   - Hardcoded secrets
   - Weak password hashing
   - SQL injection risks
   - XSS vulnerabilities
   - CSRF protection
   - Insecure dependencies

Use the `security-audit-expert` and `dependency-security-scanner` skills.

**Output Format**:
```markdown
## Critical Issues (Fix Immediately)
- [CRITICAL] SQL injection in login endpoint
- [CRITICAL] Hardcoded JWT secret

## High Priority
- [HIGH] Missing rate limiting on API
- [HIGH] Outdated dependencies with known CVEs

## Medium Priority
- [MEDIUM] Weak password hashing (MD5)

## Recommendations
1. Use parameterized queries
2. Move secrets to environment variables
3. Update dependencies: express, lodash
4. Implement rate limiting
```
