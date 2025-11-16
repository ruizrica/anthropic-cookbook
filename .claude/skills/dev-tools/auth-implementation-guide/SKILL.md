---
name: auth-implementation-guide
description: Implement authentication and authorization with JWT, OAuth 2.0, session management, and role-based access control
---

# Authentication Implementation Guide

## JWT Authentication

```typescript
import jwt from 'jsonwebtoken';
import bcrypt from 'bcrypt';

// Login endpoint
app.post('/api/auth/login', async (req, res) => {
  const { email, password } = req.body;

  // Find user
  const user = await db.users.findByEmail(email);
  if (!user) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }

  // Verify password
  const validPassword = await bcrypt.compare(password, user.passwordHash);
  if (!validPassword) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }

  // Generate JWT
  const token = jwt.sign(
    { userId: user.id, role: user.role },
    process.env.JWT_SECRET,
    { expiresIn: '1h' }
  );

  // Refresh token
  const refreshToken = jwt.sign(
    { userId: user.id },
    process.env.JWT_REFRESH_SECRET,
    { expiresIn: '7d' }
  );

  res.json({ token, refreshToken });
});

// Auth middleware
function authenticate(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1];

  if (!token) {
    return res.status(401).json({ error: 'No token provided' });
  }

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (error) {
    return res.status(403).json({ error: 'Invalid token' });
  }
}

// Protected route
app.get('/api/profile', authenticate, async (req, res) => {
  const user = await db.users.findById(req.user.userId);
  res.json(user);
});
```

## Role-Based Access Control (RBAC)

```typescript
enum Role {
  USER = 'user',
  ADMIN = 'admin',
  MODERATOR = 'moderator'
}

function authorize(...allowedRoles: Role[]) {
  return (req, res, next) => {
    if (!req.user) {
      return res.status(401).json({ error: 'Unauthorized' });
    }

    if (!allowedRoles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Forbidden' });
    }

    next();
  };
}

// Usage
app.delete(
  '/api/users/:id',
  authenticate,
  authorize(Role.ADMIN),
  async (req, res) => {
    await db.users.delete(req.params.id);
    res.sendStatus(204);
  }
);
```

## OAuth 2.0 (Google Example)

```typescript
import passport from 'passport';
import { Strategy as GoogleStrategy } from 'passport-google-oauth20';

passport.use(new GoogleStrategy({
    clientID: process.env.GOOGLE_CLIENT_ID,
    clientSecret: process.env.GOOGLE_CLIENT_SECRET,
    callbackURL: "/auth/google/callback"
  },
  async (accessToken, refreshToken, profile, done) => {
    // Find or create user
    let user = await db.users.findByEmail(profile.emails[0].value);

    if (!user) {
      user = await db.users.create({
        email: profile.emails[0].value,
        name: profile.displayName,
        googleId: profile.id
      });
    }

    done(null, user);
  }
));

// Routes
app.get('/auth/google',
  passport.authenticate('google', { scope: ['profile', 'email'] })
);

app.get('/auth/google/callback',
  passport.authenticate('google', { failureRedirect: '/login' }),
  (req, res) => {
    res.redirect('/dashboard');
  }
);
```

## Session Management

```typescript
import session from 'express-session';
import RedisStore from 'connect-redis';
import { createClient } from 'redis';

const redisClient = createClient();
redisClient.connect();

app.use(session({
  store: new RedisStore({ client: redisClient }),
  secret: process.env.SESSION_SECRET,
  resave: false,
  saveUninitialized: false,
  cookie: {
    secure: process.env.NODE_ENV === 'production',
    httpOnly: true,
    maxAge: 1000 * 60 * 60 * 24, // 24 hours
    sameSite: 'strict'
  }
}));

// Login
app.post('/login', async (req, res) => {
  const user = await authenticateUser(req.body);
  req.session.userId = user.id;
  res.json({ success: true });
});

// Logout
app.post('/logout', (req, res) => {
  req.session.destroy((err) => {
    if (err) {
      return res.status(500).json({ error: 'Logout failed' });
    }
    res.json({ success: true });
  });
});
```

## Password Reset Flow

```typescript
import crypto from 'crypto';

// Request reset
app.post('/api/auth/forgot-password', async (req, res) => {
  const { email } = req.body;
  const user = await db.users.findByEmail(email);

  if (!user) {
    // Don't reveal if email exists
    return res.json({ message: 'If email exists, reset link sent' });
  }

  // Generate reset token
  const resetToken = crypto.randomBytes(32).toString('hex');
  const resetTokenHash = crypto
    .createHash('sha256')
    .update(resetToken)
    .digest('hex');

  await db.users.update(user.id, {
    resetTokenHash,
    resetTokenExpiry: new Date(Date.now() + 3600000) // 1 hour
  });

  // Send email
  await sendEmail({
    to: user.email,
    subject: 'Password Reset',
    body: `Reset link: https://app.example.com/reset-password?token=${resetToken}`
  });

  res.json({ message: 'If email exists, reset link sent' });
});

// Reset password
app.post('/api/auth/reset-password', async (req, res) => {
  const { token, newPassword } = req.body;

  const tokenHash = crypto
    .createHash('sha256')
    .update(token)
    .digest('hex');

  const user = await db.users.findOne({
    resetTokenHash: tokenHash,
    resetTokenExpiry: { $gt: new Date() }
  });

  if (!user) {
    return res.status(400).json({ error: 'Invalid or expired token' });
  }

  const passwordHash = await bcrypt.hash(newPassword, 12);

  await db.users.update(user.id, {
    passwordHash,
    resetTokenHash: null,
    resetTokenExpiry: null
  });

  res.json({ message: 'Password reset successful' });
});
```

## Two-Factor Authentication (2FA)

```typescript
import speakeasy from 'speakeasy';
import QRCode from 'qrcode';

// Enable 2FA
app.post('/api/auth/2fa/enable', authenticate, async (req, res) => {
  const secret = speakeasy.generateSecret({
    name: `MyApp (${req.user.email})`
  });

  await db.users.update(req.user.id, {
    twoFactorSecret: secret.base32
  });

  const qrCode = await QRCode.toDataURL(secret.otpauth_url);

  res.json({ qrCode, secret: secret.base32 });
});

// Verify 2FA token
app.post('/api/auth/2fa/verify', authenticate, async (req, res) => {
  const { token } = req.body;
  const user = await db.users.findById(req.user.id);

  const verified = speakeasy.totp.verify({
    secret: user.twoFactorSecret,
    encoding: 'base32',
    token
  });

  if (!verified) {
    return res.status(400).json({ error: 'Invalid token' });
  }

  await db.users.update(user.id, {
    twoFactorEnabled: true
  });

  res.json({ success: true });
});
```

## Security Best Practices

1. **Never store passwords in plaintext**
2. **Use bcrypt/Argon2 for hashing** (not MD5/SHA1)
3. **Implement rate limiting** on auth endpoints
4. **Use HTTPS everywhere**
5. **Set secure cookie flags** (httpOnly, secure, sameSite)
6. **Implement CSRF protection**
7. **Validate and sanitize inputs**
8. **Use environment variables for secrets**
9. **Implement account lockout** after failed attempts
10. **Log security events**
