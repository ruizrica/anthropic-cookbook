---
allowed-tools: Read(*),Grep(*),Glob(*),Write(*)
description: Analyze and optimize application performance including database queries, API calls, and resource usage
---

# Optimize Performance Command

Analyze the application for performance bottlenecks and provide optimization recommendations.

**Analysis Areas**:

1. **Database Performance**:
   - Scan for N+1 queries
   - Identify missing indexes
   - Find slow queries
   - Suggest query optimizations

2. **API Performance**:
   - Identify excessive API calls
   - Find opportunities for caching
   - Suggest batching strategies
   - Check for proper pagination

3. **Frontend Performance**:
   - Bundle size analysis
   - Lazy loading opportunities
   - Image optimization needs
   - Unused code detection

4. **Backend Performance**:
   - Memory leaks
   - CPU-intensive operations
   - Connection pooling
   - Async/await usage

Use the `backend-performance-optimizer` skill.

**Output**:
- List of performance issues ranked by impact
- Specific code locations
- Optimization recommendations with code examples
- Expected performance improvements
