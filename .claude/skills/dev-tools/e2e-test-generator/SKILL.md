---
name: e2e-test-generator
description: Generate end-to-end tests using Playwright, Cypress, and modern testing patterns for comprehensive application testing
---

# E2E Test Generator Skill

## Purpose

Create comprehensive end-to-end tests that validate complete user workflows, from frontend interactions to backend API calls, ensuring application quality across the entire stack.

---

## Playwright Test Generation

### 1. Installation & Setup

```bash
npm init playwright@latest
```

```typescript
// playwright.config.ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './tests',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: [
    ['html'],
    ['junit', { outputFile: 'test-results/junit.xml' }],
  ],
  use: {
    baseURL: 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },
  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
    {
      name: 'Mobile Chrome',
      use: { ...devices['Pixel 5'] },
    },
    {
      name: 'Mobile Safari',
      use: { ...devices['iPhone 12'] },
    },
  ],
  webServer: {
    command: 'npm run start',
    url: 'http://localhost:3000',
    reuseExistingServer: !process.env.CI,
  },
});
```

### 2. Page Object Model Pattern

```typescript
// pages/LoginPage.ts
import { Page, Locator } from '@playwright/test';

export class LoginPage {
  readonly page: Page;
  readonly emailInput: Locator;
  readonly passwordInput: Locator;
  readonly submitButton: Locator;
  readonly errorMessage: Locator;

  constructor(page: Page) {
    this.page = page;
    this.emailInput = page.locator('#email');
    this.passwordInput = page.locator('#password');
    this.submitButton = page.locator('button[type="submit"]');
    this.errorMessage = page.locator('.error-message');
  }

  async goto() {
    await this.page.goto('/login');
  }

  async login(email: string, password: string) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.submitButton.click();
  }

  async getErrorMessage() {
    return await this.errorMessage.textContent();
  }
}

// pages/DashboardPage.ts
export class DashboardPage {
  readonly page: Page;
  readonly userMenu: Locator;
  readonly logoutButton: Locator;
  readonly welcomeMessage: Locator;

  constructor(page: Page) {
    this.page = page;
    this.userMenu = page.locator('[data-testid="user-menu"]');
    this.logoutButton = page.locator('text=Logout');
    this.welcomeMessage = page.locator('.welcome-message');
  }

  async logout() {
    await this.userMenu.click();
    await this.logoutButton.click();
  }

  async getWelcomeMessage() {
    return await this.welcomeMessage.textContent();
  }
}
```

### 3. Complete Test Example

```typescript
// tests/auth.spec.ts
import { test, expect } from '@playwright/test';
import { LoginPage } from '../pages/LoginPage';
import { DashboardPage } from '../pages/DashboardPage';

test.describe('Authentication', () => {
  test('successful login flow', async ({ page }) => {
    const loginPage = new LoginPage(page);
    const dashboardPage = new DashboardPage(page);

    await loginPage.goto();
    await loginPage.login('user@example.com', 'password123');

    // Verify redirect to dashboard
    await expect(page).toHaveURL('/dashboard');

    // Verify welcome message
    const message = await dashboardPage.getWelcomeMessage();
    expect(message).toContain('Welcome back');
  });

  test('login with invalid credentials', async ({ page }) => {
    const loginPage = new LoginPage(page);

    await loginPage.goto();
    await loginPage.login('invalid@example.com', 'wrongpassword');

    // Should remain on login page
    await expect(page).toHaveURL('/login');

    // Should show error message
    const error = await loginPage.getErrorMessage();
    expect(error).toBe('Invalid email or password');
  });

  test('logout flow', async ({ page }) => {
    const loginPage = new LoginPage(page);
    const dashboardPage = new DashboardPage(page);

    // Login first
    await loginPage.goto();
    await loginPage.login('user@example.com', 'password123');
    await expect(page).toHaveURL('/dashboard');

    // Logout
    await dashboardPage.logout();

    // Should redirect to login
    await expect(page).toHaveURL('/login');
  });
});
```

### 4. API Testing with Playwright

```typescript
// tests/api/users.spec.ts
import { test, expect } from '@playwright/test';

test.describe('Users API', () => {
  const baseURL = 'https://api.example.com';
  let authToken: string;

  test.beforeAll(async ({ request }) => {
    // Login to get auth token
    const response = await request.post(`${baseURL}/auth/login`, {
      data: {
        email: 'test@example.com',
        password: 'password123',
      },
    });

    const data = await response.json();
    authToken = data.token;
  });

  test('GET /users - list users', async ({ request }) => {
    const response = await request.get(`${baseURL}/users`, {
      headers: {
        'Authorization': `Bearer ${authToken}`,
      },
    });

    expect(response.ok()).toBeTruthy();
    expect(response.status()).toBe(200);

    const users = await response.json();
    expect(Array.isArray(users)).toBeTruthy();
    expect(users.length).toBeGreaterThan(0);
  });

  test('POST /users - create user', async ({ request }) => {
    const newUser = {
      name: 'Test User',
      email: `test-${Date.now()}@example.com`,
      role: 'user',
    };

    const response = await request.post(`${baseURL}/users`, {
      headers: {
        'Authorization': `Bearer ${authToken}`,
      },
      data: newUser,
    });

    expect(response.status()).toBe(201);

    const created = await response.json();
    expect(created.name).toBe(newUser.name);
    expect(created.email).toBe(newUser.email);
    expect(created.id).toBeDefined();
  });

  test('PUT /users/:id - update user', async ({ request }) => {
    const userId = '123';
    const updates = { name: 'Updated Name' };

    const response = await request.put(`${baseURL}/users/${userId}`, {
      headers: {
        'Authorization': `Bearer ${authToken}`,
      },
      data: updates,
    });

    expect(response.status()).toBe(200);

    const updated = await response.json();
    expect(updated.name).toBe(updates.name);
  });

  test('DELETE /users/:id - delete user', async ({ request }) => {
    const userId = '123';

    const response = await request.delete(`${baseURL}/users/${userId}`, {
      headers: {
        'Authorization': `Bearer ${authToken}`,
      },
    });

    expect(response.status()).toBe(204);
  });
});
```

---

## Cypress Test Generation

### 1. Installation & Setup

```bash
npm install --save-dev cypress
npx cypress open
```

```javascript
// cypress.config.js
const { defineConfig } = require('cypress');

module.exports = defineConfig({
  e2e: {
    baseUrl: 'http://localhost:3000',
    viewportWidth: 1280,
    viewportHeight: 720,
    video: true,
    screenshotOnRunFailure: true,
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
  },
});
```

### 2. Custom Commands

```javascript
// cypress/support/commands.js
Cypress.Commands.add('login', (email, password) => {
  cy.session([email, password], () => {
    cy.visit('/login');
    cy.get('#email').type(email);
    cy.get('#password').type(password);
    cy.get('button[type="submit"]').click();
    cy.url().should('include', '/dashboard');
  });
});

Cypress.Commands.add('createUser', (userData) => {
  return cy.request({
    method: 'POST',
    url: '/api/users',
    body: userData,
    headers: {
      'Authorization': `Bearer ${Cypress.env('authToken')}`,
    },
  });
});

Cypress.Commands.add('resetDatabase', () => {
  return cy.exec('npm run db:reset');
});
```

### 3. Complete Test Example

```javascript
// cypress/e2e/shopping.cy.js
describe('Shopping Cart Flow', () => {
  beforeEach(() => {
    cy.resetDatabase();
    cy.login('user@example.com', 'password123');
    cy.visit('/products');
  });

  it('adds product to cart', () => {
    // Find and click first product
    cy.get('[data-testid="product-card"]').first().click();

    // Verify product details page
    cy.url().should('include', '/products/');
    cy.get('h1').should('be.visible');

    // Add to cart
    cy.get('[data-testid="add-to-cart"]').click();

    // Verify cart badge updates
    cy.get('[data-testid="cart-badge"]').should('have.text', '1');

    // Verify toast notification
    cy.get('.toast').should('contain', 'Added to cart');
  });

  it('completes checkout flow', () => {
    // Add product to cart
    cy.get('[data-testid="product-card"]').first().click();
    cy.get('[data-testid="add-to-cart"]').click();

    // Go to cart
    cy.get('[data-testid="cart-icon"]').click();
    cy.url().should('include', '/cart');

    // Verify cart contents
    cy.get('[data-testid="cart-item"]').should('have.length', 1);

    // Proceed to checkout
    cy.get('[data-testid="checkout-button"]').click();

    // Fill shipping information
    cy.get('#shipping-name').type('John Doe');
    cy.get('#shipping-address').type('123 Main St');
    cy.get('#shipping-city').type('San Francisco');
    cy.get('#shipping-zip').type('94102');

    // Fill payment information
    cy.get('#card-number').type('4242424242424242');
    cy.get('#card-expiry').type('12/25');
    cy.get('#card-cvc').type('123');

    // Submit order
    cy.get('[data-testid="place-order"]').click();

    // Verify confirmation page
    cy.url().should('include', '/confirmation');
    cy.get('[data-testid="order-number"]').should('be.visible');
    cy.get('.success-message').should('contain', 'Order confirmed');
  });

  it('removes product from cart', () => {
    // Add two products
    cy.get('[data-testid="product-card"]').eq(0).click();
    cy.get('[data-testid="add-to-cart"]').click();
    cy.visit('/products');
    cy.get('[data-testid="product-card"]').eq(1).click();
    cy.get('[data-testid="add-to-cart"]').click();

    // Go to cart
    cy.get('[data-testid="cart-icon"]').click();

    // Remove first item
    cy.get('[data-testid="remove-item"]').first().click();

    // Verify cart updated
    cy.get('[data-testid="cart-item"]').should('have.length', 1);
    cy.get('[data-testid="cart-badge"]').should('have.text', '1');
  });
});
```

### 4. API Testing with Cypress

```javascript
// cypress/e2e/api/products.cy.js
describe('Products API', () => {
  let authToken;

  before(() => {
    cy.request({
      method: 'POST',
      url: '/api/auth/login',
      body: {
        email: 'admin@example.com',
        password: 'admin123',
      },
    }).then((response) => {
      authToken = response.body.token;
    });
  });

  it('GET /api/products - returns product list', () => {
    cy.request('/api/products').then((response) => {
      expect(response.status).to.eq(200);
      expect(response.body).to.be.an('array');
      expect(response.body.length).to.be.greaterThan(0);

      // Verify product structure
      const product = response.body[0];
      expect(product).to.have.property('id');
      expect(product).to.have.property('name');
      expect(product).to.have.property('price');
    });
  });

  it('POST /api/products - creates new product', () => {
    const newProduct = {
      name: 'Test Product',
      price: 29.99,
      category: 'Electronics',
      stock: 100,
    };

    cy.request({
      method: 'POST',
      url: '/api/products',
      headers: {
        Authorization: `Bearer ${authToken}`,
      },
      body: newProduct,
    }).then((response) => {
      expect(response.status).to.eq(201);
      expect(response.body.name).to.eq(newProduct.name);
      expect(response.body.id).to.exist;
    });
  });

  it('validates required fields', () => {
    cy.request({
      method: 'POST',
      url: '/api/products',
      headers: {
        Authorization: `Bearer ${authToken}`,
      },
      body: {},
      failOnStatusCode: false,
    }).then((response) => {
      expect(response.status).to.eq(400);
      expect(response.body.errors).to.include('name is required');
    });
  });
});
```

---

## Advanced Testing Patterns

### 1. Visual Testing Integration

```typescript
// Playwright with Percy
import percySnapshot from '@percy/playwright';

test('homepage visual test', async ({ page }) => {
  await page.goto('/');
  await percySnapshot(page, 'Homepage');
});
```

### 2. Network Interception & Mocking

```typescript
// Playwright
test('mock API responses', async ({ page }) => {
  await page.route('**/api/products', (route) => {
    route.fulfill({
      status: 200,
      contentType: 'application/json',
      body: JSON.stringify([
        { id: 1, name: 'Mocked Product', price: 19.99 },
      ]),
    });
  });

  await page.goto('/products');
  await expect(page.locator('text=Mocked Product')).toBeVisible();
});
```

```javascript
// Cypress
cy.intercept('GET', '/api/products', {
  statusCode: 200,
  body: [
    { id: 1, name: 'Mocked Product', price: 19.99 },
  ],
}).as('getProducts');

cy.visit('/products');
cy.wait('@getProducts');
cy.contains('Mocked Product').should('be.visible');
```

### 3. File Upload Testing

```typescript
// Playwright
test('upload file', async ({ page }) => {
  await page.goto('/upload');

  const fileInput = page.locator('input[type="file"]');
  await fileInput.setInputFiles('path/to/file.pdf');

  await page.click('button[type="submit"]');

  await expect(page.locator('.success-message')).toBeVisible();
});
```

### 4. Accessibility Testing

```typescript
// Playwright with axe-core
import AxeBuilder from '@axe-core/playwright';

test('homepage accessibility', async ({ page }) => {
  await page.goto('/');

  const accessibilityScanResults = await new AxeBuilder({ page }).analyze();

  expect(accessibilityScanResults.violations).toEqual([]);
});
```

---

## Test Data Management

### 1. Fixtures

```typescript
// tests/fixtures/users.json
{
  "admin": {
    "email": "admin@example.com",
    "password": "admin123",
    "role": "admin"
  },
  "user": {
    "email": "user@example.com",
    "password": "user123",
    "role": "user"
  }
}
```

### 2. Factory Functions

```typescript
// tests/factories/user.factory.ts
export function createUser(overrides = {}) {
  return {
    name: 'Test User',
    email: `test-${Date.now()}@example.com`,
    role: 'user',
    active: true,
    ...overrides,
  };
}

// Usage
const user = createUser({ role: 'admin' });
```

### 3. Database Seeding

```typescript
// tests/setup/seed.ts
import { db } from '../db';

export async function seedDatabase() {
  await db.users.deleteMany();
  await db.products.deleteMany();

  await db.users.createMany([
    { email: 'user1@example.com', name: 'User 1' },
    { email: 'user2@example.com', name: 'User 2' },
  ]);

  await db.products.createMany([
    { name: 'Product 1', price: 29.99 },
    { name: 'Product 2', price: 39.99 },
  ]);
}
```

---

## CI/CD Integration

### GitHub Actions

```yaml
name: E2E Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps

      - name: Run E2E tests
        run: npm run test:e2e

      - name: Upload test results
        if: always()
        uses: actions/upload-artifact@v3
        with:
          name: playwright-report
          path: playwright-report/
```

---

## Best Practices

1. **Use data-testid attributes** for stable selectors
2. **Avoid hard-coded waits** - use explicit waits
3. **Keep tests independent** - no dependencies between tests
4. **Use Page Object Model** for maintainability
5. **Mock external dependencies** when appropriate
6. **Test critical paths first** - prioritize business value
7. **Run tests in parallel** for faster feedback
8. **Clean up test data** after each test

---

## Remember

- **E2E tests are slow** - use sparingly for critical paths
- **Combine with unit and integration tests** for comprehensive coverage
- **Keep tests maintainable** - refactor as application evolves
- **Monitor test flakiness** - fix or remove flaky tests
- **Use visual testing** to catch CSS regressions
