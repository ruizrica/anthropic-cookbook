---
name: cross-browser-validator
description: Validate web applications across multiple browsers, devices, and operating systems to ensure consistent user experience
---

# Cross-Browser Validator Skill

## Purpose

Ensure web applications work consistently across different browsers (Chrome, Firefox, Safari, Edge), devices (desktop, mobile, tablet), and operating systems (Windows, macOS, Linux, iOS, Android).

---

## Browser Compatibility Matrix

| Feature | Chrome | Firefox | Safari | Edge | IE11 |
|---------|--------|---------|--------|------|------|
| ES6+ | ✅ | ✅ | ✅ | ✅ | ❌ |
| CSS Grid | ✅ | ✅ | ✅ | ✅ | ❌ |
| Flexbox | ✅ | ✅ | ✅ | ✅ | ⚠️ |
| CSS Custom Properties | ✅ | ✅ | ✅ | ✅ | ❌ |
| Service Workers | ✅ | ✅ | ✅ | ✅ | ❌ |
| WebP Images | ✅ | ✅ | ✅ | ✅ | ❌ |
| Intersection Observer | ✅ | ✅ | ✅ | ✅ | ❌ |

---

## Testing Tools

### 1. BrowserStack (Cloud Testing)

```javascript
// browserstack.config.js
exports.config = {
  user: process.env.BROWSERSTACK_USERNAME,
  key: process.env.BROWSERSTACK_ACCESS_KEY,

  capabilities: [
    {
      browserName: 'Chrome',
      browserVersion: 'latest',
      os: 'Windows',
      osVersion: '11',
    },
    {
      browserName: 'Firefox',
      browserVersion: 'latest',
      os: 'Windows',
      osVersion: '11',
    },
    {
      browserName: 'Safari',
      browserVersion: 'latest',
      os: 'OS X',
      osVersion: 'Ventura',
    },
    {
      browserName: 'Edge',
      browserVersion: 'latest',
      os: 'Windows',
      osVersion: '11',
    },
    {
      deviceName: 'iPhone 14',
      platformName: 'iOS',
      platformVersion: '16',
      browserName: 'Safari',
    },
    {
      deviceName: 'Samsung Galaxy S22',
      platformName: 'Android',
      platformVersion: '12.0',
      browserName: 'Chrome',
    },
  ],
};
```

### 2. Playwright Cross-Browser Testing

```typescript
// playwright.config.ts
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  projects: [
    // Desktop browsers
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
      name: 'edge',
      use: { ...devices['Desktop Edge'], channel: 'msedge' },
    },

    // Mobile browsers
    {
      name: 'Mobile Chrome',
      use: { ...devices['Pixel 5'] },
    },
    {
      name: 'Mobile Safari',
      use: { ...devices['iPhone 13'] },
    },
    {
      name: 'iPad',
      use: { ...devices['iPad Pro'] },
    },

    // Tablet browsers
    {
      name: 'Samsung Galaxy Tab',
      use: {
        ...devices['Galaxy Tab S4'],
      },
    },
  ],
});
```

### 3. Selenium Grid

```javascript
// selenium-grid.config.js
const { Builder } = require('selenium-webdriver');

const browsers = [
  { browserName: 'chrome', platform: 'WINDOWS' },
  { browserName: 'firefox', platform: 'WINDOWS' },
  { browserName: 'safari', platform: 'MAC' },
  { browserName: 'MicrosoftEdge', platform: 'WINDOWS' },
];

async function runTestOnBrowser(browserConfig) {
  const driver = await new Builder()
    .usingServer('http://localhost:4444/wd/hub')
    .forBrowser(browserConfig.browserName)
    .build();

  try {
    await driver.get('http://localhost:3000');
    const title = await driver.getTitle();
    console.log(`${browserConfig.browserName}: ${title}`);
  } finally {
    await driver.quit();
  }
}

browsers.forEach(runTestOnBrowser);
```

---

## Feature Detection & Polyfills

### 1. Modernizr

```html
<script src="modernizr-custom.js"></script>
<script>
  if (!Modernizr.flexbox) {
    // Load flexbox polyfill
    document.write('<script src="flexibility.js"><\/script>');
  }

  if (!Modernizr.webp) {
    // Fallback to PNG/JPG
    document.documentElement.className += ' no-webp';
  }
</script>
```

### 2. Core-js & Babel

```javascript
// babel.config.js
module.exports = {
  presets: [
    [
      '@babel/preset-env',
      {
        useBuiltIns: 'usage',
        corejs: 3,
        targets: {
          browsers: [
            'last 2 versions',
            'not dead',
            'not ie 11',
          ],
        },
      },
    ],
  ],
};
```

### 3. Polyfill.io

```html
<script src="https://polyfill.io/v3/polyfill.min.js?features=fetch,Promise,IntersectionObserver"></script>
```

---

## Browser-Specific CSS

### 1. Autoprefixer

```css
/* Input CSS */
.box {
  display: flex;
  transition: transform 0.3s;
}

/* Autoprefixer output */
.box {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-transition: -webkit-transform 0.3s;
  transition: -webkit-transform 0.3s;
  transition: transform 0.3s;
  transition: transform 0.3s, -webkit-transform 0.3s;
}
```

### 2. Feature Queries

```css
/* Fallback for browsers without grid */
.container {
  display: flex;
  flex-wrap: wrap;
}

/* Grid for modern browsers */
@supports (display: grid) {
  .container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
  }
}
```

### 3. Browser-Specific Hacks (Use sparingly!)

```css
/* Safari only */
_::-webkit-full-page-media, _:future, :root .safari-only {
  color: red;
}

/* Firefox only */
@-moz-document url-prefix() {
  .firefox-only {
    color: blue;
  }
}

/* Edge only */
@supports (-ms-ime-align:auto) {
  .edge-only {
    color: green;
  }
}
```

---

## Common Browser Issues

### 1. Flexbox in IE11

```css
/* IE11 needs explicit flex-basis */
.flex-item {
  flex: 1 1 auto; /* Don't use shorthand 'flex: 1' */
  min-width: 0; /* Prevent overflow issues */
}
```

### 2. Safari Date Input

```html
<!-- Safari doesn't support type="date" well -->
<input
  type="date"
  placeholder="MM/DD/YYYY"
  pattern="\d{2}/\d{2}/\d{4}"
>

<script>
  // Polyfill for Safari
  if (!Modernizr.inputtypes.date) {
    $('input[type="date"]').datepicker();
  }
</script>
```

### 3. Firefox Button Padding

```css
/* Firefox adds extra padding to buttons */
button::-moz-focus-inner {
  border: 0;
  padding: 0;
}
```

### 4. Safari Rubber Band Scrolling

```css
/* Prevent overscroll bounce on iOS */
body {
  overscroll-behavior-y: none;
}
```

### 5. Chrome Autofill Styles

```css
/* Override Chrome's yellow autofill background */
input:-webkit-autofill {
  -webkit-box-shadow: 0 0 0 1000px white inset;
  -webkit-text-fill-color: #000;
}
```

---

## Automated Cross-Browser Testing

### Playwright Test Example

```typescript
// tests/cross-browser.spec.ts
import { test, expect } from '@playwright/test';

test.describe('Cross-browser compatibility', () => {
  test('renders correctly on all browsers', async ({ page, browserName }) => {
    await page.goto('/');

    // Check title
    await expect(page).toHaveTitle(/My App/);

    // Check main content
    const heading = page.locator('h1');
    await expect(heading).toBeVisible();

    // Screenshot for visual comparison
    await page.screenshot({
      path: `screenshots/${browserName}-homepage.png`,
      fullPage: true,
    });
  });

  test('flexbox layout works', async ({ page }) => {
    await page.goto('/');

    const container = page.locator('.flex-container');
    const boundingBox = await container.boundingBox();

    expect(boundingBox?.width).toBeGreaterThan(0);
    expect(boundingBox?.height).toBeGreaterThan(0);
  });

  test('CSS Grid layout works', async ({ page, browserName }) => {
    // Skip on browsers that don't support grid
    if (browserName === 'ie') {
      test.skip();
    }

    await page.goto('/grid-page');

    const grid = page.locator('.grid-container');
    const computedStyle = await grid.evaluate((el) =>
      window.getComputedStyle(el).display
    );

    expect(computedStyle).toBe('grid');
  });
});
```

---

## Mobile Device Testing

### 1. Chrome DevTools Device Emulation

```typescript
test('responsive on mobile', async ({ page }) => {
  await page.setViewportSize({ width: 375, height: 667 }); // iPhone SE
  await page.goto('/');

  // Check mobile menu is visible
  const mobileMenu = page.locator('.mobile-menu');
  await expect(mobileMenu).toBeVisible();

  // Check desktop menu is hidden
  const desktopMenu = page.locator('.desktop-menu');
  await expect(desktopMenu).toBeHidden();
});
```

### 2. Touch Event Simulation

```typescript
test('swipe gesture works', async ({ page }) => {
  await page.goto('/carousel');

  const carousel = page.locator('.carousel');

  // Simulate swipe left
  await carousel.dispatchEvent('touchstart', {
    touches: [{ clientX: 300, clientY: 100 }],
  });

  await carousel.dispatchEvent('touchmove', {
    touches: [{ clientX: 100, clientY: 100 }],
  });

  await carousel.dispatchEvent('touchend', {});

  // Verify next slide is visible
  const activeSlide = page.locator('.slide.active');
  await expect(activeSlide).toHaveAttribute('data-index', '2');
});
```

---

## Accessibility Across Browsers

```typescript
import AxeBuilder from '@axe-core/playwright';

test('accessibility on all browsers', async ({ page, browserName }) => {
  await page.goto('/');

  const results = await new AxeBuilder({ page }).analyze();

  // Log browser-specific violations
  if (results.violations.length > 0) {
    console.log(`${browserName} violations:`, results.violations);
  }

  expect(results.violations).toEqual([]);
});
```

---

## Performance Across Browsers

```typescript
test('performance metrics', async ({ page, browserName }) => {
  await page.goto('/');

  const metrics = await page.evaluate(() => {
    const navigation = performance.getEntriesByType('navigation')[0] as PerformanceNavigationTiming;

    return {
      domContentLoaded: navigation.domContentLoadedEventEnd - navigation.domContentLoadedEventStart,
      loadComplete: navigation.loadEventEnd - navigation.loadEventStart,
      firstPaint: performance.getEntriesByType('paint')[0]?.startTime || 0,
    };
  });

  console.log(`${browserName} metrics:`, metrics);

  // Assert performance thresholds
  expect(metrics.domContentLoaded).toBeLessThan(2000);
  expect(metrics.loadComplete).toBeLessThan(3000);
});
```

---

## CI/CD Cross-Browser Testing

```yaml
# .github/workflows/cross-browser.yml
name: Cross-Browser Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ${{ matrix.os }}

    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        browser: [chromium, firefox, webkit]

    steps:
      - uses: actions/checkout@v3

      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Install dependencies
        run: npm ci

      - name: Install Playwright
        run: npx playwright install --with-deps ${{ matrix.browser }}

      - name: Run tests
        run: npx playwright test --project=${{ matrix.browser }}

      - name: Upload results
        if: always()
        uses: actions/upload-artifact@v3
        with:
          name: test-results-${{ matrix.os }}-${{ matrix.browser }}
          path: playwright-report/
```

---

## Browser Testing Checklist

- [ ] **Chrome** (latest, 2 versions back)
- [ ] **Firefox** (latest, ESR)
- [ ] **Safari** (latest macOS, latest iOS)
- [ ] **Edge** (latest)
- [ ] **Mobile Chrome** (Android)
- [ ] **Mobile Safari** (iOS)
- [ ] **Viewport sizes**: 320px, 768px, 1024px, 1920px
- [ ] **Touch interactions** on mobile
- [ ] **Keyboard navigation** on desktop
- [ ] **Screen readers** (NVDA, VoiceOver)
- [ ] **Right-to-left (RTL)** languages
- [ ] **High DPI** displays
- [ ] **Color schemes** (light, dark, high contrast)
- [ ] **Reduced motion** preference
- [ ] **Print stylesheets**

---

## Remember

- **Don't support everything** - check analytics for user browsers
- **Use feature detection** over browser detection
- **Test on real devices** when possible
- **Automate repetitive tests** with Playwright/Selenium
- **Monitor browser market share** - adjust support matrix accordingly
- **Use polyfills judiciously** - consider bundle size impact
