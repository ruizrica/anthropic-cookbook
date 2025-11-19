# Developer Tools Skills Suite

A comprehensive collection of 30 developer skills and 10 slash commands for the complete software development lifecycle (SDLC). Built following the design principles from [Claude's blog on improving frontend design through skills](https://www.claude.com/blog/improving-frontend-design-through-skills).

## 📋 Table of Contents

- [Skills Overview](#skills-overview)
- [Slash Commands](#slash-commands)
- [Quick Start](#quick-start)
- [Skills Reference](#skills-reference)
- [Best Practices](#best-practices)

---

## 🎯 Skills Overview

### Frontend Development (6 skills)

| Skill | Description | Key Features |
|-------|-------------|--------------|
| **modern-frontend-design** | Create distinctive, modern UI avoiding generic "AI slop" aesthetics | Typography, color theory, motion, backgrounds |
| **component-library-builder** | Generate production-ready React/Vue/Angular components | TypeScript, accessibility, testing, Storybook |
| **responsive-design-expert** | Build mobile-first responsive designs | Breakpoints, fluid layouts, touch targets |
| **ui-animation-specialist** | Create performant UI animations | CSS, Framer Motion, WAAPI, reduced motion |
| **design-system-architect** | Build scalable design systems | Design tokens, theming, component patterns |
| **web-accessibility-auditor** | Ensure WCAG 2.1 compliance | Screen readers, keyboard nav, ARIA |

### UI Testing (4 skills)

| Skill | Description | Key Features |
|-------|-------------|--------------|
| **chrome-devtools-tester** | Automated testing via Chrome DevTools Protocol & MCP | Performance, accessibility, screenshots |
| **visual-regression-tester** | Pixel-perfect screenshot comparison | Percy, BackstopJS, Playwright |
| **e2e-test-generator** | End-to-end test generation | Playwright, Cypress, page objects |
| **cross-browser-validator** | Multi-browser compatibility testing | BrowserStack, Playwright, polyfills |

### Backend/API Development (5 skills)

| Skill | Description | Key Features |
|-------|-------------|--------------|
| **api-design-expert** | RESTful & GraphQL API design | Versioning, auth, rate limiting, OpenAPI |
| **database-schema-designer** | Optimized SQL/NoSQL schemas | Normalization, indexing, performance |
| **microservices-architect** | Microservices architecture patterns | Service discovery, API gateway, circuit breaker |
| **api-documentation-generator** | Generate comprehensive API docs | OpenAPI/Swagger, TSDoc, Postman |
| **backend-performance-optimizer** | Optimize backend performance | Caching, query optimization, connection pooling |

### Code Quality (4 skills)

| Skill | Description | Key Features |
|-------|-------------|--------------|
| **code-reviewer-pro** | Comprehensive code review | Security, performance, best practices |
| **refactoring-assistant** | Code smell detection and refactoring | SOLID principles, design patterns |
| **tech-debt-analyzer** | Identify and prioritize technical debt | Metrics, prioritization matrix |
| **dependency-security-scanner** | Scan dependencies for vulnerabilities | npm audit, Snyk, license compliance |

### Testing & QA (4 skills)

| Skill | Description | Key Features |
|-------|-------------|--------------|
| **test-strategy-planner** | Design comprehensive testing strategies | Test pyramid, coverage goals, metrics |
| **unit-test-generator** | Generate unit tests | Jest, Vitest, pytest, mocking |
| **integration-test-builder** | Build integration tests | API testing, databases, test containers |
| **load-test-designer** | Performance and load testing | k6, Artillery, stress testing |

### DevOps & CI/CD (4 skills)

| Skill | Description | Key Features |
|-------|-------------|--------------|
| **cicd-pipeline-builder** | Build CI/CD pipelines | GitHub Actions, GitLab CI, Jenkins |
| **docker-optimizer** | Optimize Docker images | Multi-stage builds, security, size reduction |
| **kubernetes-manifest-generator** | Generate K8s manifests | Deployments, services, ingress, HPA |
| **infrastructure-as-code** | Define infrastructure as code | Terraform, CloudFormation, Pulumi |

### Documentation & Security (3 skills)

| Skill | Description | Key Features |
|-------|-------------|--------------|
| **technical-doc-writer** | Write technical documentation | README, ADRs, API docs, architecture |
| **security-audit-expert** | OWASP Top 10 security audits | Vulnerability scanning, best practices |
| **auth-implementation-guide** | Implement authentication/authorization | JWT, OAuth 2.0, RBAC, 2FA |

---

## ⚡ Slash Commands

Quick access to common developer workflows:

| Command | Description | Skills Used |
|---------|-------------|-------------|
| **/code-review** | Comprehensive code review | code-reviewer-pro |
| **/generate-tests** | Generate unit & integration tests | unit-test-generator, integration-test-builder |
| **/optimize-performance** | Analyze & optimize performance | backend-performance-optimizer |
| **/security-scan** | Security audit (OWASP Top 10) | security-audit-expert, dependency-security-scanner |
| **/refactor-code** | Detect code smells & refactor | refactoring-assistant |
| **/create-api** | Generate RESTful/GraphQL API | api-design-expert, api-documentation-generator |
| **/deploy-config** | Generate deployment configs | docker-optimizer, kubernetes-manifest-generator, cicd-pipeline-builder |
| **/ui-component** | Generate accessible UI components | component-library-builder, modern-frontend-design, web-accessibility-auditor |
| **/tech-debt-report** | Technical debt analysis | tech-debt-analyzer |
| **/accessibility-audit** | WCAG 2.1 accessibility audit | web-accessibility-auditor |

---

## 🚀 Quick Start

### Using Skills

Skills are activated automatically when you describe a task that matches their domain:

```
"Create a responsive navigation component with accessibility support"
→ Uses: component-library-builder, responsive-design-expert, web-accessibility-auditor

"Review this code for security vulnerabilities"
→ Uses: code-reviewer-pro, security-audit-expert

"Generate Kubernetes deployment manifests"
→ Uses: kubernetes-manifest-generator
```

### Using Slash Commands

Execute commands directly for quick workflows:

```
/code-review
→ Reviews current changes for security, performance, best practices

/generate-tests src/user-service.ts
→ Generates comprehensive tests for the specified file

/security-scan
→ Runs full security audit including dependency scan

/ui-component Button
→ Generates accessible Button component with tests and stories
```

---

## 📚 Skills Reference

### Frontend Development

#### modern-frontend-design

**Purpose**: Create distinctive, non-generic frontend designs

**Key Principles** (from Claude blog):
- **Avoid distributional convergence**: Don't default to Inter/Roboto fonts, timid colors, static layouts
- **Four design dimensions**: Typography, color/theme, motion, backgrounds
- **Prompting altitude**: Guide creatively without hardcoding (e.g., "vibrant blue inspired by Tokyo Night" vs "#7aa2f7")

**Typography**:
- ❌ Avoid: Inter, Roboto, Open Sans, Space Grotesk
- ✅ Use: High-contrast pairings, extreme weights (100/900), distinctive fonts (Playfair Display, Fraunces)

**Color & Theme**:
- Use CSS variables for theming
- Draw from IDE themes (Dracula, Tokyo Night, Nord)
- Dominant color + sharp accents

**Motion**:
- CSS-only for HTML/CSS projects
- Framer Motion for React
- Staggered animations with `animation-delay`

**Backgrounds**:
- Layered gradients for atmosphere
- Geometric patterns for depth
- Glass morphism effects

**Example**:
```css
:root {
  --font-display: 'Fraunces', serif;
  --font-body: 'Manrope', sans-serif;
  --color-primary: #7aa2f7;  /* Tokyo Night blue */
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
}
```

#### component-library-builder

**Generates**:
- React/Vue/Angular components with TypeScript
- Accessibility (ARIA, keyboard nav)
- Unit & accessibility tests
- Storybook documentation
- Multiple variants (size, visual, state)

**Structure**:
```
ComponentName/
├── ComponentName.tsx
├── ComponentName.types.ts
├── ComponentName.module.css
├── ComponentName.test.tsx
├── ComponentName.stories.tsx
└── index.ts
```

#### responsive-design-expert

**Breakpoints**:
- Mobile: < 576px
- Tablet: 768px - 1023px
- Desktop: 1024px - 1439px
- Large: ≥ 1440px

**Patterns**:
- Mobile-first approach
- Fluid grids with CSS Grid
- Container queries (modern)
- Responsive typography with `clamp()`

**Touch Targets**: Minimum 44x44px (WCAG 2.1 AAA)

#### ui-animation-specialist

**Performance First**:
- ✅ Animate: `transform`, `opacity`
- ❌ Avoid: `width`, `height`, `top`, `left`

**Timing**:
- Micro-interactions: 100-200ms
- Standard transitions: 200-300ms
- Complex animations: 400-600ms

**Accessibility**:
```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

#### design-system-architect

**Components**:
- Design tokens (colors, spacing, typography)
- Component library
- Layout primitives (Stack, Inline, Container)
- Theming system (light/dark/multi-brand)

**Token Structure**:
```json
{
  "color": {
    "brand": { "primary": { "500": "#3b82f6" } },
    "semantic": { "error": "{color.red.500}" },
    "text": { "primary": "{color.gray.900}" }
  },
  "spacing": { "4": "1rem", "8": "2rem" }
}
```

#### web-accessibility-auditor

**WCAG 2.1 Compliance**:
- Level A: Minimum
- Level AA: Standard (recommended)
- Level AAA: Enhanced

**Checks**:
- Semantic HTML
- Color contrast (4.5:1 for text)
- Keyboard navigation
- Screen reader support
- Form accessibility
- ARIA labels

**Tools**:
- Axe DevTools
- Lighthouse
- WAVE
- jest-axe (automated testing)

---

### UI Testing

#### chrome-devtools-tester

**Chrome MCP Integration**:
```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-chrome-devtools"]
    }
  }
}
```

**Available Operations**:
- Navigate, screenshot, click, fill forms
- Execute JavaScript
- Get console/network logs
- Performance metrics
- Accessibility audits

#### visual-regression-tester

**Tools**:
- **Percy**: Hosted solution with PR integration
- **BackstopJS**: Self-hosted with detailed reports
- **Playwright**: Built-in screenshot comparison

**Workflow**:
1. Capture baseline screenshots
2. Make code changes
3. Capture new screenshots
4. Compare pixel-by-pixel
5. Review & approve/reject

**Thresholds**:
```typescript
{
  maxDiffPixels: 100,
  threshold: 0.2  // 20% difference allowed
}
```

#### e2e-test-generator

**Patterns**:
- Page Object Model
- Test fixtures
- API mocking
- Parallel execution

**Playwright Example**:
```typescript
test('user can login', async ({ page }) => {
  await page.goto('/login');
  await page.fill('#email', 'user@example.com');
  await page.fill('#password', 'password123');
  await page.click('button[type="submit"]');
  await expect(page).toHaveURL('/dashboard');
});
```

#### cross-browser-validator

**Browsers Tested**:
- Chrome, Firefox, Safari, Edge
- Mobile: iOS Safari, Android Chrome
- Multiple viewports & OS combinations

**Polyfill Strategy**:
- Feature detection with Modernizr
- Babel + core-js for ES6+
- CSS fallbacks with `@supports`

---

### Backend/API Development

#### api-design-expert

**REST Principles**:
- Resource-based URLs (`/api/users`, not `/api/getUsers`)
- Proper HTTP methods (GET, POST, PUT, PATCH, DELETE)
- Status codes (200, 201, 400, 401, 403, 404, 500)
- Pagination, filtering, sorting

**GraphQL Schema**:
```graphql
type User {
  id: ID!
  email: String!
  posts: [Post!]!
}

type Query {
  user(id: ID!): User
  users(limit: Int): [User!]!
}
```

#### database-schema-designer

**SQL Best Practices**:
- Normalize to 3NF
- Index foreign keys
- Appropriate data types
- Constraints (NOT NULL, UNIQUE, CHECK)

**NoSQL Patterns**:
- Embed for one-to-few
- Reference for one-to-many
- Denormalize for read performance

#### microservices-architect

**Patterns**:
- API Gateway (routing, auth)
- Service Registry (Consul, Eureka)
- Circuit Breaker (resilience)
- Event-Driven (message queues)
- Saga pattern (distributed transactions)

#### api-documentation-generator

**Formats**:
- OpenAPI 3.0 / Swagger
- TSDoc / JSDoc
- Postman collections
- ReDoc / Swagger UI

#### backend-performance-optimizer

**Optimizations**:
- Query optimization (avoid N+1)
- Caching (Redis, HTTP caching)
- Connection pooling
- Load balancing
- Compression (gzip, brotli)

---

### Code Quality

#### code-reviewer-pro

**Review Areas**:
- Security (injection, XSS, secrets)
- Performance (N+1 queries, algorithms)
- Best practices (DRY, SOLID)
- Testing (coverage, edge cases)

#### refactoring-assistant

**Code Smells**:
- Long methods (> 50 lines)
- Large classes (> 300 lines)
- Duplicate code
- Primitive obsession

**Refactorings**:
- Extract Method/Class
- Replace Conditional with Polymorphism
- Introduce Parameter Object

#### tech-debt-analyzer

**Categories**:
- Code quality debt
- Architecture debt
- Documentation debt
- Infrastructure debt

**Prioritization**:
- High impact + Low effort = Do immediately
- High impact + High effort = Schedule soon
- Low impact + Low effort = Backlog
- Low impact + High effort = Skip

#### dependency-security-scanner

**Tools**:
- npm audit
- Snyk
- OWASP Dependency-Check
- GitHub Dependabot

---

### Testing & QA

#### test-strategy-planner

**Test Pyramid**:
- 70% Unit tests
- 20% Integration tests
- 10% E2E tests

**Coverage Goals**:
- Branches: 80%
- Functions: 85%
- Lines: 85%

#### unit-test-generator

**Frameworks**:
- Jest/Vitest (JavaScript/TypeScript)
- pytest (Python)
- JUnit (Java)
- Go testing

**Patterns**:
- Given-When-Then
- Arrange-Act-Assert
- Mocking with jest.mock()

#### integration-test-builder

**Test Types**:
- API integration (Supertest)
- Database integration
- External service mocking (Nock)
- Test containers (Docker)

#### load-test-designer

**Tools**:
- k6 (modern, scriptable)
- Artillery (YAML-based)
- JMeter (GUI-based)

**Test Types**:
- Load test (sustained)
- Spike test (sudden increase)
- Stress test (find limits)
- Soak test (endurance)

---

### DevOps & CI/CD

#### cicd-pipeline-builder

**Platforms**:
- GitHub Actions
- GitLab CI
- Jenkins
- CircleCI

**Stages**:
1. Build
2. Test (lint, type-check, unit, integration)
3. Security scan
4. Deploy

#### docker-optimizer

**Optimizations**:
- Multi-stage builds
- Layer caching
- Alpine base images
- .dockerignore
- Non-root user
- Security scanning (Trivy)

#### kubernetes-manifest-generator

**Resources**:
- Deployment
- Service (LoadBalancer, ClusterIP)
- Ingress
- ConfigMap & Secret
- HorizontalPodAutoscaler

#### infrastructure-as-code

**Tools**:
- Terraform (HCL)
- CloudFormation (YAML)
- Pulumi (TypeScript/Python)

**Best Practices**:
- Version control
- Remote state management
- Modularization
- Separate environments

---

### Documentation & Security

#### technical-doc-writer

**Document Types**:
- README.md
- Architecture Decision Records (ADRs)
- API documentation
- Developer guides
- Code documentation (JSDoc, TSDoc)

#### security-audit-expert

**OWASP Top 10 (2021)**:
1. Broken Access Control
2. Cryptographic Failures
3. Injection
4. Insecure Design
5. Security Misconfiguration
6. Vulnerable Components
7. Authentication Failures
8. Software/Data Integrity Failures
9. Logging/Monitoring Failures
10. SSRF

#### auth-implementation-guide

**Methods**:
- JWT (stateless)
- Session-based (stateful)
- OAuth 2.0 (third-party)
- 2FA (TOTP)

**RBAC**:
```typescript
function authorize(...roles: Role[]) {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Forbidden' });
    }
    next();
  };
}
```

---

## 🎨 Design Principles (from Claude Blog)

This skill suite is built following the principles from [Claude's blog post on improving frontend design](https://www.claude.com/blog/improving-frontend-design-through-skills):

### Avoiding "Distributional Convergence"

**Problem**: Without direction, models default to safe, generic choices:
- Typography: Inter, Roboto
- Colors: Timid, evenly-distributed palettes
- Motion: Minimal or none
- Backgrounds: Solid colors

**Solution**: Skills provide targeted guidance across four design dimensions:

1. **Typography**: Distinctive fonts, high contrast, extreme weights
2. **Color**: Cohesive themes, dominant colors with sharp accents
3. **Motion**: High-impact moments, staggered animations
4. **Backgrounds**: Layered gradients, geometric patterns, depth

### Prompting Altitude Principle

**Guide creatively, don't hardcode**:
- ❌ "Use #7aa2f7 for primary color"
- ✅ "Choose vibrant blue inspired by Tokyo Night theme"

### Reusable Assets

Skills become organizational assets that encode:
- Design systems
- Component patterns
- Industry conventions
- Security best practices

---

## 🛠️ Best Practices

### When to Use Each Skill

**Frontend Development**:
- New UI component → component-library-builder
- Design system setup → design-system-architect
- Mobile responsiveness → responsive-design-expert
- Animations → ui-animation-specialist
- Accessibility issues → web-accessibility-auditor

**Backend Development**:
- New API → api-design-expert
- Database design → database-schema-designer
- Microservices → microservices-architect
- Performance issues → backend-performance-optimizer

**Quality & Testing**:
- Code review → code-reviewer-pro
- Legacy code → refactoring-assistant
- Test creation → unit-test-generator, integration-test-builder
- Load testing → load-test-designer

**DevOps**:
- Deployment → docker-optimizer, kubernetes-manifest-generator
- CI/CD setup → cicd-pipeline-builder
- Infrastructure → infrastructure-as-code

**Security**:
- Security audit → security-audit-expert
- Auth implementation → auth-implementation-guide
- Dependency check → dependency-security-scanner

### Combining Skills

Skills work together for comprehensive solutions:

```
Task: "Create a secure, accessible user profile page"

Skills activated:
1. component-library-builder (UI components)
2. responsive-design-expert (mobile-first layout)
3. web-accessibility-auditor (WCAG compliance)
4. auth-implementation-guide (profile access control)
5. security-audit-expert (XSS prevention, input validation)
```

---

## 📊 Coverage Matrix

| SDLC Phase | Skills Available | Slash Commands |
|------------|-----------------|----------------|
| Design | 6 frontend, 1 design system | /ui-component |
| Development | 11 (frontend + backend) | /create-api |
| Testing | 8 (UI + backend testing) | /generate-tests, /accessibility-audit |
| Quality | 4 code quality skills | /code-review, /refactor-code, /tech-debt-report |
| Security | 3 security skills | /security-scan |
| Deployment | 4 DevOps skills | /deploy-config |
| Documentation | 1 doc writer | - |

---

## 🚀 Getting Started Examples

### Example 1: Create New Feature

```
Task: Build a user dashboard with charts

1. /ui-component Dashboard
   → Generates accessible dashboard component

2. /ui-component Chart
   → Generates responsive chart component with D3/Recharts

3. /generate-tests src/components/Dashboard.tsx
   → Generates unit and accessibility tests

4. /accessibility-audit
   → Ensures WCAG compliance

5. /optimize-performance
   → Checks for rendering performance issues
```

### Example 2: Deploy Application

```
Task: Deploy Node.js API to Kubernetes

1. /security-scan
   → Check for vulnerabilities

2. /deploy-config
   → Generates:
     - Dockerfile (optimized)
     - docker-compose.yml
     - Kubernetes manifests
     - CI/CD pipeline (GitHub Actions)

3. /tech-debt-report
   → Identify issues before production
```

### Example 3: Refactor Legacy Code

```
Task: Improve legacy codebase quality

1. /tech-debt-report
   → Identify and prioritize issues

2. /code-review
   → Find security and performance issues

3. /refactor-code src/services/
   → Apply refactoring patterns

4. /generate-tests src/services/
   → Add missing test coverage

5. /security-scan
   → Verify security improvements
```

---

## 📝 Contributing

When adding new skills:

1. **Follow naming conventions**: `kebab-case` for directories and files
2. **Include frontmatter**: name, description in SKILL.md
3. **Provide examples**: Code snippets demonstrating usage
4. **Document patterns**: Common use cases and best practices
5. **Reference sources**: Link to authoritative guides
6. **Add to README**: Update this file with new skill details

---

## 📄 License

MIT

---

## 🙏 Acknowledgments

- Design principles from [Claude's blog on frontend design](https://www.claude.com/blog/improving-frontend-design-through-skills)
- WCAG guidelines from W3C
- OWASP Top 10 security standards
- Community best practices from the developer ecosystem

---

**Total Tools**: 30 skills + 10 slash commands = 40 comprehensive developer tools

**Coverage**: Complete SDLC from design to deployment, with emphasis on quality, security, and accessibility.
