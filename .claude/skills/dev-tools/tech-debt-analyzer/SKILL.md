---
name: tech-debt-analyzer
description: Identify, categorize, and prioritize technical debt for systematic improvement
---

# Tech Debt Analyzer

## Categories of Technical Debt

### 1. Code Quality Debt
- Duplicate code
- Complex functions (cyclomatic complexity > 10)
- Missing tests
- Outdated dependencies

### 2. Architecture Debt
- Tight coupling
- Missing abstractions
- Monolithic structure
- Poor separation of concerns

### 3. Documentation Debt
- Missing API docs
- Outdated README
- No architecture diagrams
- Unclear code comments

### 4. Infrastructure Debt
- Manual deployment processes
- Missing monitoring
- No disaster recovery plan
- Outdated server configurations

## Analysis Tools

```bash
# Code complexity
npx ts-prune  # Find unused exports
npx madge --circular src/  # Detect circular dependencies

# Test coverage
npm run test -- --coverage

# Dependencies
npm audit
npm outdated

# Code quality
npx eslint src/
npx tsc --noEmit  # Type check

# Bundle size
npx webpack-bundle-analyzer
```

## Prioritization Matrix

| Impact | Effort | Priority | Action |
|--------|--------|----------|--------|
| High | Low | Critical | Do immediately |
| High | High | Important | Schedule soon |
| Low | Low | Nice to have | Backlog |
| Low | High | Avoid | Skip |

## Tracking Technical Debt

```typescript
// Document technical debt in code
/**
 * @todo Refactor to use async/await instead of callbacks
 * @debt Performance: This function has O(n²) complexity
 * @impact High - Used in critical path
 * @effort Medium - Requires refactoring entire module
 */
function processItems(items, callback) {
  // Legacy callback-based code
}
```

## Metrics to Track

1. **Code Coverage**: > 80%
2. **Cyclomatic Complexity**: < 10 per function
3. **Dependency Freshness**: < 6 months old
4. **Build Time**: < 5 minutes
5. **CI/CD Success Rate**: > 95%
