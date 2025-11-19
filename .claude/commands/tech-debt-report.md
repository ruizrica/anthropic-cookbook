---
allowed-tools: Read(*),Grep(*),Glob(*),Bash(npm)
description: Generate technical debt report with prioritized action items and effort estimates
---

# Tech Debt Report Command

Generate comprehensive technical debt report with prioritization.

**Analysis Areas**:

1. **Code Quality**:
   - Cyclomatic complexity
   - Code duplication
   - Long functions/classes
   - Test coverage gaps

2. **Dependencies**:
   - Outdated packages
   - Security vulnerabilities
   - License compatibility

3. **Architecture**:
   - Circular dependencies
   - Tight coupling
   - Missing abstractions
   - Monolithic structures

4. **Documentation**:
   - Missing API docs
   - Outdated README
   - No architecture diagrams
   - Insufficient code comments

5. **Infrastructure**:
   - Manual deployment processes
   - Missing monitoring
   - No disaster recovery
   - Performance bottlenecks

Use the `tech-debt-analyzer` skill.

**Prioritization Matrix**:

| Priority | Impact | Effort | Action |
|----------|--------|--------|--------|
| P0 | High | Low | Do immediately |
| P1 | High | High | Schedule soon |
| P2 | Low | Low | Backlog |
| P3 | Low | High | Consider skipping |

**Output Format**:
```markdown
# Technical Debt Report

## Executive Summary
- Total debt items: 45
- Critical issues: 8
- Estimated effort: 3 weeks

## P0 - Critical (Do Immediately)
1. [Security] Outdated dependencies with CVEs
   - Impact: High
   - Effort: 2 hours
   - Files: package.json

## P1 - Important (Schedule Soon)
1. [Performance] N+1 queries in user service
   - Impact: High
   - Effort: 4 hours
   - Files: user-service.ts

## Metrics
- Test coverage: 65% (Target: 80%)
- Code duplication: 15% (Target: < 5%)
- Cyclomatic complexity: Avg 8 (Target: < 10)

## Recommendations
1. Allocate 20% of sprint capacity to tech debt
2. Focus on security issues first
3. Set up automated dependency updates
```
