---
name: security-audit-expert
description: Audit applications for OWASP Top 10 vulnerabilities, implement security best practices, and perform penetration testing
---

# Security Audit Expert

## OWASP Top 10 (2021)

### 1. Broken Access Control
```typescript
// ❌ Insecure: No authorization check
app.get('/api/users/:id/profile', async (req, res) => {
  const profile = await db.getProfile(req.params.id);
  res.json(profile);
});

// ✅ Secure: Verify user owns resource
app.get('/api/users/:id/profile', authenticate, async (req, res) => {
  if (req.user.id !== req.params.id && !req.user.isAdmin) {
    return res.status(403).json({ error: 'Forbidden' });
  }
  const profile = await db.getProfile(req.params.id);
  res.json(profile);
});
```

### 2. Cryptographic Failures
```typescript
// ❌ Weak password hashing
const hash = crypto.createHash('md5').update(password).digest('hex');

// ✅ Strong password hashing
import bcrypt from 'bcrypt';
const hash = await bcrypt.hash(password, 12);

// ❌ Storing sensitive data in plaintext
db.save({ creditCard: '4111111111111111' });

// ✅ Encrypt sensitive data
const encrypted = encrypt(creditCard, encryptionKey);
db.save({ creditCard: encrypted });
```

### 3. Injection (SQL, NoSQL, OS)
```typescript
// ❌ SQL Injection
const query = `SELECT * FROM users WHERE email = '${email}'`;

// ✅ Parameterized query
const query = 'SELECT * FROM users WHERE email = $1';
const result = await db.query(query, [email]);

// ❌ Command injection
exec(`ping ${userInput}`);

// ✅ Input validation
if (!/^[a-z0-9.-]+$/i.test(hostname)) {
  throw new Error('Invalid hostname');
}
exec(`ping ${hostname}`);
```

### 4. Insecure Design
```typescript
// ❌ No rate limiting
app.post('/api/login', async (req, res) => {
  // Login logic
});

// ✅ Rate limiting
import rateLimit from 'express-rate-limit';

const loginLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 5,
  message: 'Too many login attempts'
});

app.post('/api/login', loginLimiter, async (req, res) => {
  // Login logic
});
```

### 5. Security Misconfiguration
```typescript
// ❌ Exposing error details
app.use((err, req, res, next) => {
  res.status(500).json({ error: err.stack });
});

// ✅ Generic error messages
app.use((err, req, res, next) => {
  logger.error(err);
  res.status(500).json({ error: 'Internal server error' });
});

// Security headers
import helmet from 'helmet';
app.use(helmet());
```

### 6. Vulnerable Components
```bash
# Regular dependency audits
npm audit
npm audit fix

# Automated scanning
npx snyk test
```

### 7. Authentication Failures
```typescript
// ✅ Strong session management
import session from 'express-session';

app.use(session({
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: false,
  cookie: {
    secure: true,  // HTTPS only
    httpOnly: true,  // No JavaScript access
    maxAge: 1000 * 60 * 30,  // 30 minutes
    sameSite: 'strict'
  }
}));
```

### 8. Software and Data Integrity Failures
```typescript
// ✅ Verify package integrity
npm install --integrity

// ✅ Subresource Integrity
<script
  src="https://cdn.example.com/script.js"
  integrity="sha384-..."
  crossorigin="anonymous"
></script>
```

### 9. Logging and Monitoring Failures
```typescript
import winston from 'winston';

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

// Log security events
logger.warn('Failed login attempt', {
  email: req.body.email,
  ip: req.ip,
  timestamp: new Date()
});
```

### 10. Server-Side Request Forgery (SSRF)
```typescript
// ❌ Unvalidated URL fetch
app.post('/api/fetch', async (req, res) => {
  const data = await fetch(req.body.url);
  res.json(data);
});

// ✅ Whitelist allowed domains
const ALLOWED_DOMAINS = ['api.example.com', 'cdn.example.com'];

app.post('/api/fetch', async (req, res) => {
  const url = new URL(req.body.url);
  if (!ALLOWED_DOMAINS.includes(url.hostname)) {
    return res.status(400).json({ error: 'Invalid domain' });
  }
  const data = await fetch(url);
  res.json(data);
});
```

## Security Headers

```typescript
import helmet from 'helmet';

app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'", "'unsafe-inline'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      imgSrc: ["'self'", "data:", "https:"],
    }
  },
  hsts: {
    maxAge: 31536000,
    includeSubDomains: true,
    preload: true
  }
}));
```

## Security Checklist

- [ ] Input validation on all user inputs
- [ ] Parameterized queries (no string concatenation)
- [ ] Strong password hashing (bcrypt, Argon2)
- [ ] HTTPS everywhere
- [ ] Security headers (Helmet.js)
- [ ] Rate limiting on sensitive endpoints
- [ ] CSRF protection
- [ ] XSS prevention (escape output)
- [ ] Dependency audits (npm audit, Snyk)
- [ ] Secrets in environment variables (not code)
- [ ] Logging security events
- [ ] Regular security updates
