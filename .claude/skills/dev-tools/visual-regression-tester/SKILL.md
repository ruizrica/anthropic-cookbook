---
name: visual-regression-tester
description: Automated visual regression testing with screenshot comparison, pixel-perfect validation, and CI/CD integration
---

# Visual Regression Tester Skill

## Purpose

Detect unintended visual changes in UI by comparing screenshots across versions, ensuring pixel-perfect consistency and catching CSS regressions before they reach production.

---

## Visual Regression Testing Workflow

```
1. Capture baseline screenshots (approved version)
2. Make code changes
3. Capture new screenshots
4. Compare pixel-by-pixel
5. Review differences
6. Approve or reject changes
```

---

## Tools & Libraries

### 1. Percy (Visual Testing Platform)
**Best for**: Teams wanting hosted solution with automatic PR integration

```bash
npm install --save-dev @percy/cli @percy/puppeteer
```

```javascript
// percy-test.js
const puppeteer = require('puppeteer');
const percySnapshot = require('@percy/puppeteer');

describe('Visual regression tests', () => {
  let browser, page;

  beforeAll(async () => {
    browser = await puppeteer.launch();
    page = await browser.newPage();
  });

  afterAll(async () => {
    await browser.close();
  });

  test('Homepage', async () => {
    await page.goto('http://localhost:3000');
    await percySnapshot(page, 'Homepage');
  });

  test('Homepage - Mobile', async () => {
    await page.setViewport({ width: 375, height: 667 });
    await page.goto('http://localhost:3000');
    await percySnapshot(page, 'Homepage Mobile');
  });

  test('Dashboard - Dark Mode', async () => {
    await page.goto('http://localhost:3000/dashboard');
    await page.evaluate(() => {
      document.documentElement.setAttribute('data-theme', 'dark');
    });
    await percySnapshot(page, 'Dashboard Dark');
  });
});
```

### 2. BackstopJS (Open Source)
**Best for**: Self-hosted solution with detailed reporting

```bash
npm install --save-dev backstopjs
```

```json
// backstop.json
{
  "id": "my_project",
  "viewports": [
    {
      "label": "phone",
      "width": 375,
      "height": 667
    },
    {
      "label": "tablet",
      "width": 768,
      "height": 1024
    },
    {
      "label": "desktop",
      "width": 1920,
      "height": 1080
    }
  ],
  "scenarios": [
    {
      "label": "Homepage",
      "url": "http://localhost:3000",
      "delay": 1000,
      "misMatchThreshold": 0.1
    },
    {
      "label": "Product Page",
      "url": "http://localhost:3000/products/1",
      "delay": 1000,
      "clickSelector": ".tabs li:nth-child(2)",
      "postInteractionWait": 500
    },
    {
      "label": "Form Validation",
      "url": "http://localhost:3000/contact",
      "clickSelector": "button[type='submit']",
      "expect": 1,
      "misMatchThreshold": 0.5
    }
  ],
  "paths": {
    "bitmaps_reference": "backstop_data/bitmaps_reference",
    "bitmaps_test": "backstop_data/bitmaps_test",
    "html_report": "backstop_data/html_report"
  },
  "report": ["browser", "CI"],
  "engine": "puppeteer"
}
```

```bash
# Create baseline
backstop reference

# Run tests
backstop test

# Approve changes
backstop approve
```

### 3. Playwright Visual Comparisons
**Best for**: Projects already using Playwright

```typescript
// visual.spec.ts
import { test, expect } from '@playwright/test';

test.describe('Visual regression', () => {
  test('Homepage matches baseline', async ({ page }) => {
    await page.goto('http://localhost:3000');

    // Wait for fonts and images
    await page.waitForLoadState('networkidle');

    // Compare with baseline
    await expect(page).toHaveScreenshot('homepage.png', {
      maxDiffPixels: 100,
      threshold: 0.2,
    });
  });

  test('Button states', async ({ page }) => {
    await page.goto('http://localhost:3000');

    // Default state
    const button = page.locator('.primary-button');
    await expect(button).toHaveScreenshot('button-default.png');

    // Hover state
    await button.hover();
    await expect(button).toHaveScreenshot('button-hover.png');

    // Focus state
    await button.focus();
    await expect(button).toHaveScreenshot('button-focus.png');
  });

  test('Responsive design', async ({ page }) => {
    const viewports = [
      { width: 375, height: 667, name: 'mobile' },
      { width: 768, height: 1024, name: 'tablet' },
      { width: 1920, height: 1080, name: 'desktop' },
    ];

    for (const viewport of viewports) {
      await page.setViewportSize(viewport);
      await page.goto('http://localhost:3000');
      await expect(page).toHaveScreenshot(`homepage-${viewport.name}.png`);
    }
  });
});
```

### 4. Pixelmatch (Low-Level Library)
**Best for**: Custom comparison logic

```javascript
const fs = require('fs');
const PNG = require('pngjs').PNG;
const pixelmatch = require('pixelmatch');

function compareImages(img1Path, img2Path, diffPath) {
  const img1 = PNG.sync.read(fs.readFileSync(img1Path));
  const img2 = PNG.sync.read(fs.readFileSync(img2Path));

  const { width, height } = img1;
  const diff = new PNG({ width, height });

  const numDiffPixels = pixelmatch(
    img1.data,
    img2.data,
    diff.data,
    width,
    height,
    {
      threshold: 0.1, // 0-1 range, lower = more strict
      includeAA: false, // Ignore anti-aliasing differences
    }
  );

  fs.writeFileSync(diffPath, PNG.sync.write(diff));

  const percentDiff = (numDiffPixels / (width * height)) * 100;

  return {
    numDiffPixels,
    percentDiff,
    passed: percentDiff < 0.1, // 0.1% threshold
  };
}

// Usage
const result = compareImages(
  'baseline/homepage.png',
  'current/homepage.png',
  'diff/homepage-diff.png'
);

console.log(`Diff: ${result.percentDiff.toFixed(2)}%`);
console.log(`Status: ${result.passed ? 'PASS' : 'FAIL'}`);
```

---

## Best Practices

### 1. Stabilize Before Capture

```javascript
async function stabilizePage(page) {
  // Wait for network idle
  await page.waitForLoadState('networkidle');

  // Wait for fonts
  await page.evaluate(() => document.fonts.ready);

  // Wait for animations to complete
  await page.waitForTimeout(500);

  // Disable animations for consistent screenshots
  await page.addStyleTag({
    content: `
      *, *::before, *::after {
        animation-duration: 0s !important;
        animation-delay: 0s !important;
        transition-duration: 0s !important;
        transition-delay: 0s !important;
      }
    `,
  });
}
```

### 2. Mask Dynamic Content

```javascript
// Mask elements that change frequently (dates, random IDs, etc.)
await page.screenshot({
  path: 'screenshot.png',
  mask: [
    page.locator('.timestamp'),
    page.locator('.session-id'),
    page.locator('.random-content'),
  ],
});
```

### 3. Set Deterministic Viewport

```javascript
// Always use consistent viewport sizes
const viewports = {
  mobile: { width: 375, height: 667 },
  tablet: { width: 768, height: 1024 },
  desktop: { width: 1920, height: 1080 },
};

await page.setViewportSize(viewports.desktop);
```

### 4. Handle Third-Party Content

```javascript
// Block third-party requests for consistency
await page.route('**/*', (route) => {
  const url = route.request().url();

  // Block external resources
  if (url.includes('google-analytics.com') ||
      url.includes('facebook.com') ||
      url.includes('ads.')) {
    route.abort();
  } else {
    route.continue();
  }
});
```

### 5. Use Data Fixtures

```javascript
// Use consistent test data
before(() => {
  cy.task('db:seed', {
    users: [
      { id: 1, name: 'Test User', avatar: 'test-avatar.png' },
    ],
    products: [
      { id: 1, name: 'Test Product', price: 99.99 },
    ],
  });
});
```

---

## Testing Strategies

### 1. Component-Level Testing

```typescript
// Test individual components in isolation
test('Button component variants', async ({ page }) => {
  await page.goto('http://localhost:6006/iframe.html?id=button--variants');

  const variants = ['primary', 'secondary', 'outline', 'ghost'];

  for (const variant of variants) {
    const button = page.locator(`[data-variant="${variant}"]`);
    await expect(button).toHaveScreenshot(`button-${variant}.png`);
  }
});
```

### 2. Page-Level Testing

```typescript
// Test complete pages
test('Complete homepage', async ({ page }) => {
  await page.goto('http://localhost:3000');

  // Full page screenshot
  await expect(page).toHaveScreenshot('homepage-full.png', {
    fullPage: true,
  });
});
```

### 3. Critical User Flows

```typescript
// Test multi-step user journeys
test('Checkout flow', async ({ page }) => {
  await page.goto('http://localhost:3000/cart');
  await expect(page).toHaveScreenshot('01-cart.png');

  await page.click('text=Proceed to Checkout');
  await expect(page).toHaveScreenshot('02-checkout.png');

  await page.fill('#email', 'test@example.com');
  await page.fill('#card', '4242424242424242');
  await expect(page).toHaveScreenshot('03-payment-filled.png');

  await page.click('text=Complete Purchase');
  await page.waitForURL('**/confirmation');
  await expect(page).toHaveScreenshot('04-confirmation.png');
});
```

### 4. Cross-Browser Testing

```typescript
// playwright.config.ts
import { PlaywrightTestConfig } from '@playwright/test';

const config: PlaywrightTestConfig = {
  projects: [
    {
      name: 'chromium',
      use: { browserName: 'chromium' },
    },
    {
      name: 'firefox',
      use: { browserName: 'firefox' },
    },
    {
      name: 'webkit',
      use: { browserName: 'webkit' },
    },
  ],
};

export default config;
```

---

## CI/CD Integration

### GitHub Actions with Percy

```yaml
# .github/workflows/visual-regression.yml
name: Visual Regression Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Start server
        run: npm start &

      - name: Run Percy tests
        run: npx percy exec -- npm run test:visual
        env:
          PERCY_TOKEN: ${{ secrets.PERCY_TOKEN }}
```

### GitHub Actions with BackstopJS

```yaml
name: Visual Regression

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Install dependencies
        run: npm ci

      - name: Start server
        run: npm start &

      - name: Run BackstopJS tests
        run: |
          npm run backstop:test || true

      - name: Upload report
        if: always()
        uses: actions/upload-artifact@v3
        with:
          name: backstop-report
          path: backstop_data/html_report/

      - name: Check for failures
        run: |
          if [ -f backstop_data/ci_report/xunit.xml ]; then
            exit 1
          fi
```

### GitLab CI with Playwright

```yaml
# .gitlab-ci.yml
visual-regression:
  image: mcr.microsoft.com/playwright:v1.40.0
  stage: test

  script:
    - npm ci
    - npm run build
    - npm start &
    - npx playwright test --project=chromium

  artifacts:
    when: always
    paths:
      - test-results/
      - playwright-report/
    reports:
      junit: test-results/junit.xml
```

---

## Advanced Techniques

### 1. Ignore Regions

```javascript
// BackstopJS: Ignore dynamic regions
{
  "scenarios": [
    {
      "label": "Dashboard",
      "url": "http://localhost:3000/dashboard",
      "removeSelectors": [".ad-banner", ".live-chat"],
      "hideSelectors": [".timestamp", ".user-avatar"]
    }
  ]
}
```

### 2. Custom Diff Threshold Per Test

```typescript
// Playwright: Different thresholds for different tests
test('Hero section - strict', async ({ page }) => {
  await page.goto('/');
  await expect(page.locator('.hero')).toHaveScreenshot({
    maxDiffPixels: 10, // Very strict
  });
});

test('Footer - relaxed', async ({ page }) => {
  await page.goto('/');
  await expect(page.locator('footer')).toHaveScreenshot({
    maxDiffPixels: 500, // More lenient
  });
});
```

### 3. Storybook Integration

```javascript
// Use Storybook stories as test cases
import initStoryshots from '@storybook/addon-storyshots';
import { imageSnapshot } from '@storybook/addon-storyshots-puppeteer';

initStoryshots({
  suite: 'Visual regression',
  test: imageSnapshot({
    storybookUrl: 'http://localhost:6006',
  }),
});
```

### 4. Visual Testing Matrix

```typescript
// Test multiple combinations
const combinations = {
  themes: ['light', 'dark', 'high-contrast'],
  locales: ['en', 'es', 'fr'],
  viewports: ['mobile', 'tablet', 'desktop'],
};

for (const theme of combinations.themes) {
  for (const locale of combinations.locales) {
    for (const viewport of combinations.viewports) {
      test(`${theme}-${locale}-${viewport}`, async ({ page }) => {
        await page.goto(`/?theme=${theme}&locale=${locale}`);
        await page.setViewportSize(viewports[viewport]);
        await expect(page).toHaveScreenshot(
          `homepage-${theme}-${locale}-${viewport}.png`
        );
      });
    }
  }
}
```

---

## Debugging Visual Failures

### 1. Review Diff Images

Most tools generate three images:
- **Baseline**: Approved version
- **Current**: New version
- **Diff**: Highlighted differences

### 2. Interactive Comparison

```bash
# BackstopJS: Open interactive report
backstop openReport

# Playwright: Open HTML report
npx playwright show-report
```

### 3. Update Baselines Selectively

```bash
# Playwright: Update specific test
npx playwright test --update-snapshots test-name

# BackstopJS: Approve specific scenario
backstop approve --filter="Homepage"
```

---

## Common Issues & Solutions

**Issue**: Tests failing on CI but passing locally
- **Solution**: Ensure same OS (use Docker), disable animations, use consistent fonts

**Issue**: Anti-aliasing differences across systems
- **Solution**: Increase threshold, use `includeAA: false` in pixelmatch

**Issue**: Flaky tests due to loading timing
- **Solution**: Use explicit waits, `waitForLoadState('networkidle')`

**Issue**: Large image storage
- **Solution**: Use image optimization, compress PNGs, store only diffs in git

---

## Performance Tips

1. **Parallelize tests**: Run multiple browsers concurrently
2. **Cache baselines**: Don't regenerate baselines on every run
3. **Optimize images**: Use PNG compression tools
4. **Target critical paths**: Don't screenshot everything
5. **Use component screenshots**: Faster than full-page
6. **Skip unchanged files**: Only test modified components

---

## Checklist

- [ ] Choose appropriate tool (Percy, BackstopJS, Playwright)
- [ ] Configure viewports for responsive testing
- [ ] Disable animations before screenshots
- [ ] Wait for fonts and images to load
- [ ] Mask dynamic content (timestamps, IDs)
- [ ] Set diff thresholds per test
- [ ] Integrate with CI/CD pipeline
- [ ] Store baselines in version control or cloud
- [ ] Review diffs before approving
- [ ] Document approval process for team

---

## Remember

- **Visual regression is not a replacement** for functional testing
- **Review diffs carefully** - not all differences are bugs
- **Keep baselines up to date** with approved changes
- **Use appropriate thresholds** - too strict = false positives, too loose = missed bugs
- **Test across browsers** for true cross-platform compatibility
