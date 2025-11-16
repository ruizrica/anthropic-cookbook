---
name: dependency-security-scanner
description: Scan dependencies for vulnerabilities, license compliance, and supply chain security risks
---

# Dependency Security Scanner

## Tools

### 1. npm audit
```bash
npm audit  # Check for vulnerabilities
npm audit fix  # Auto-fix vulnerabilities
npm audit fix --force  # Force major version updates
```

### 2. Snyk
```bash
npx snyk test  # Scan for vulnerabilities
npx snyk monitor  # Continuous monitoring
npx snyk protect  # Apply patches
```

### 3. OWASP Dependency-Check
```bash
dependency-check --project myapp --scan ./package.json
```

### 4. GitHub Dependabot
```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: npm
    directory: "/"
    schedule:
      interval: weekly
    open-pull-requests-limit: 10
```

## Security Best Practices

### 1. Pin Dependencies
```json
{
  "dependencies": {
    "express": "4.18.2",  // ✅ Exact version
    "lodash": "^4.17.21"  // ⚠️ Allow minor updates
  }
}
```

### 2. Use Lock Files
- `package-lock.json` (npm)
- `yarn.lock` (Yarn)
- `pnpm-lock.yaml` (pnpm)

### 3. Verify Package Integrity
```bash
# Check package signatures
npm install --ignore-scripts  # Prevent postinstall scripts

# Verify checksums
npm install --integrity
```

### 4. License Compliance
```bash
npx license-checker --summary
npx license-checker --json > licenses.json
```

## Common Vulnerabilities

### 1. Prototype Pollution
```javascript
// ❌ Vulnerable
function merge(target, source) {
  for (let key in source) {
    target[key] = source[key];
  }
}

// ✅ Safe
function merge(target, source) {
  for (let key in source) {
    if (Object.hasOwnProperty.call(source, key)) {
      target[key] = source[key];
    }
  }
}
```

### 2. ReDoS (Regular Expression Denial of Service)
```javascript
// ❌ Vulnerable to ReDoS
const re = /(a+)+$/;

// ✅ Safe
const re = /a+$/;
```

## CI/CD Integration

```yaml
# GitHub Actions
- name: Run security scan
  run: |
    npm audit --audit-level=high
    npx snyk test --severity-threshold=high
```

## Monitoring
- Snyk Dashboard
- GitHub Security Advisories
- NPM Security Advisories
