---
name: docker-optimizer
description: Optimize Docker images with multi-stage builds, layer caching, and security best practices
---

# Docker Optimizer

## Optimized Dockerfile

```dockerfile
# Multi-stage build
FROM node:18-alpine AS builder

WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy source code
COPY . .

# Build
RUN npm run build

# Production image
FROM node:18-alpine

WORKDIR /app

# Create non-root user
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

# Copy only necessary files
COPY --from=builder --chown=nodejs:nodejs /app/dist ./dist
COPY --from=builder --chown=nodejs:nodejs /app/node_modules ./node_modules
COPY --from=builder --chown=nodejs:nodejs /app/package.json ./

# Switch to non-root user
USER nodejs

EXPOSE 3000

CMD ["node", "dist/index.js"]
```

## Best Practices

### 1. Use .dockerignore
```
node_modules
npm-debug.log
.git
.env
*.md
tests/
```

### 2. Layer Optimization
```dockerfile
# ❌ Bad: Rebuilds dependencies on every code change
COPY . .
RUN npm install

# ✅ Good: Cache dependencies layer
COPY package*.json ./
RUN npm install
COPY . .
```

### 3. Security Scanning
```bash
# Scan for vulnerabilities
docker scan myapp:latest

# Use Trivy
trivy image myapp:latest
```

### 4. Image Size Optimization
- Use Alpine base images
- Remove unnecessary files
- Use multi-stage builds
- Combine RUN commands

```dockerfile
# ❌ Multiple layers
RUN apk add curl
RUN apk add git
RUN apk add vim

# ✅ Single layer
RUN apk add --no-cache curl git vim && \
    rm -rf /var/cache/apk/*
```

## Docker Compose

```yaml
version: '3.8'

services:
  app:
    build:
      context: .
      target: production
    ports:
      - "3000:3000"
    environment:
      NODE_ENV: production
      DATABASE_URL: postgres://db:5432/mydb
    depends_on:
      - db
      - redis
    restart: unless-stopped

  db:
    image: postgres:15-alpine
    volumes:
      - postgres-data:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: mydb
      POSTGRES_PASSWORD: ${DB_PASSWORD}

  redis:
    image: redis:7-alpine
    volumes:
      - redis-data:/data

volumes:
  postgres-data:
  redis-data:
```
