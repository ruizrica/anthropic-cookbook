---
name: database-schema-designer
description: Design optimized database schemas for SQL and NoSQL with indexing, normalization, and performance tuning
---

# Database Schema Designer

## SQL Best Practices
- **Normalize to 3NF** for OLTP workloads
- **Index foreign keys** and frequently queried columns
- **Use appropriate data types** (INT vs BIGINT, VARCHAR vs TEXT)
- **Add constraints** (NOT NULL, UNIQUE, CHECK)

## Example Schema
```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email VARCHAR(255) UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT NOW(),
  INDEX idx_email (email)
);

CREATE TABLE posts (
  id SERIAL PRIMARY KEY,
  user_id INT REFERENCES users(id) ON DELETE CASCADE,
  title VARCHAR(255) NOT NULL,
  published BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMP DEFAULT NOW(),
  INDEX idx_user_published (user_id, published),
  FULLTEXT INDEX idx_title_content (title, content)
);
```

## NoSQL (MongoDB)
```javascript
// Embed for one-to-few
{ _id, name, addresses: [...] }

// Reference for one-to-many
{ _id, userId, content }

// Indexes
db.posts.createIndex({ userId: 1, createdAt: -1 });
db.users.createIndex({ "email": 1 }, { unique: true });
```
