---
name: chrome-devtools-tester
description: Automated UI testing using Chrome DevTools Protocol and MCP integration for debugging, performance analysis, and accessibility testing
---

# Chrome DevTools Tester Skill

## Purpose

Leverage Chrome DevTools Protocol (CDP) and Model Context Protocol (MCP) integration to perform automated UI testing, debugging, performance profiling, and accessibility audits directly from Claude.

---

## Chrome MCP Integration

### MCP Server Configuration

```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-chrome-devtools"],
      "env": {
        "CHROME_EXECUTABLE_PATH": "/usr/bin/google-chrome"
      }
    }
  }
}
```

### Available Tools via Chrome MCP

1. **`chrome_navigate`** - Navigate to URL
2. **`chrome_screenshot`** - Capture screenshot
3. **`chrome_click`** - Click element
4. **`chrome_fill`** - Fill form field
5. **`chrome_select`** - Select dropdown option
6. **`chrome_evaluate`** - Execute JavaScript
7. **`chrome_console_logs`** - Get console messages
8. **`chrome_network_logs`** - Get network activity
9. **`chrome_performance`** - Performance metrics
10. **`chrome_accessibility`** - Accessibility audit

---

## Testing Patterns

### 1. Basic UI Flow Testing

```typescript
// Test user login flow
async function testLogin() {
  // Navigate to login page
  await chrome_navigate({ url: 'https://app.example.com/login' });

  // Fill credentials
  await chrome_fill({ selector: '#email', value: 'user@example.com' });
  await chrome_fill({ selector: '#password', value: 'password123' });

  // Submit form
  await chrome_click({ selector: 'button[type="submit"]' });

  // Wait for navigation
  await new Promise(resolve => setTimeout(resolve, 2000));

  // Verify successful login
  const result = await chrome_evaluate({
    expression: 'document.querySelector(".user-menu") !== null'
  });

  console.log('Login successful:', result);
}
```

### 2. Visual Testing with Screenshots

```typescript
// Capture screenshots at different viewport sizes
async function captureResponsiveScreenshots(url: string) {
  const viewports = [
    { width: 375, height: 667, name: 'mobile' },
    { width: 768, height: 1024, name: 'tablet' },
    { width: 1920, height: 1080, name: 'desktop' },
  ];

  for (const viewport of viewports) {
    await chrome_evaluate({
      expression: `window.resizeTo(${viewport.width}, ${viewport.height})`
    });

    await chrome_screenshot({
      path: `screenshots/${viewport.name}.png`,
      fullPage: true
    });
  }
}
```

### 3. Performance Testing

```typescript
// Measure page load performance
async function measurePerformance(url: string) {
  await chrome_navigate({ url });

  const metrics = await chrome_performance({
    metrics: [
      'FirstContentfulPaint',
      'LargestContentfulPaint',
      'TimeToInteractive',
      'TotalBlockingTime',
      'CumulativeLayoutShift'
    ]
  });

  // Analyze Core Web Vitals
  const analysis = {
    fcp: metrics.FirstContentfulPaint < 1800 ? 'PASS' : 'FAIL',
    lcp: metrics.LargestContentfulPaint < 2500 ? 'PASS' : 'FAIL',
    tti: metrics.TimeToInteractive < 3800 ? 'PASS' : 'FAIL',
    tbt: metrics.TotalBlockingTime < 200 ? 'PASS' : 'FAIL',
    cls: metrics.CumulativeLayoutShift < 0.1 ? 'PASS' : 'FAIL',
  };

  return { metrics, analysis };
}
```

### 4. Accessibility Auditing

```typescript
// Run automated accessibility audit
async function auditAccessibility(url: string) {
  await chrome_navigate({ url });

  const violations = await chrome_accessibility({
    runOnly: {
      type: 'tag',
      values: ['wcag2a', 'wcag2aa', 'wcag21a', 'wcag21aa']
    }
  });

  // Group violations by severity
  const grouped = {
    critical: violations.filter(v => v.impact === 'critical'),
    serious: violations.filter(v => v.impact === 'serious'),
    moderate: violations.filter(v => v.impact === 'moderate'),
    minor: violations.filter(v => v.impact === 'minor'),
  };

  return grouped;
}
```

### 5. Console Error Detection

```typescript
// Monitor console for errors
async function checkConsoleErrors(url: string) {
  await chrome_navigate({ url });

  const logs = await chrome_console_logs();

  const errors = logs.filter(log =>
    log.level === 'error' || log.level === 'warning'
  );

  return {
    totalLogs: logs.length,
    errors: errors.length,
    errorMessages: errors.map(e => e.text),
  };
}
```

### 6. Network Performance

```typescript
// Analyze network requests
async function analyzeNetwork(url: string) {
  await chrome_navigate({ url });

  const requests = await chrome_network_logs();

  const analysis = {
    totalRequests: requests.length,
    totalSize: requests.reduce((sum, r) => sum + r.encodedDataLength, 0),
    slowRequests: requests.filter(r => r.time > 1000),
    failedRequests: requests.filter(r => r.status >= 400),
    cacheable: requests.filter(r => r.fromCache),
  };

  // Find largest resources
  const largestResources = requests
    .sort((a, b) => b.encodedDataLength - a.encodedDataLength)
    .slice(0, 10);

  return { analysis, largestResources };
}
```

---

## Advanced Testing Scenarios

### 1. Form Validation Testing

```typescript
async function testFormValidation(url: string) {
  await chrome_navigate({ url });

  // Test empty submission
  await chrome_click({ selector: 'button[type="submit"]' });
  const emailError = await chrome_evaluate({
    expression: 'document.querySelector("#email-error")?.textContent'
  });

  // Test invalid email
  await chrome_fill({ selector: '#email', value: 'invalid-email' });
  await chrome_click({ selector: 'button[type="submit"]' });
  const emailFormatError = await chrome_evaluate({
    expression: 'document.querySelector("#email-error")?.textContent'
  });

  // Test valid submission
  await chrome_fill({ selector: '#email', value: 'valid@example.com' });
  await chrome_fill({ selector: '#password', value: 'SecurePass123!' });
  await chrome_click({ selector: 'button[type="submit"]' });

  return { emailError, emailFormatError };
}
```

### 2. Responsive Design Testing

```typescript
async function testResponsiveDesign(url: string) {
  await chrome_navigate({ url });

  const breakpoints = [
    { width: 320, name: 'small-mobile' },
    { width: 375, name: 'mobile' },
    { width: 768, name: 'tablet' },
    { width: 1024, name: 'laptop' },
    { width: 1920, name: 'desktop' },
  ];

  const results = [];

  for (const bp of breakpoints) {
    await chrome_evaluate({
      expression: `window.resizeTo(${bp.width}, 900)`
    });

    // Check if mobile menu is visible
    const mobileMenuVisible = await chrome_evaluate({
      expression: `
        getComputedStyle(document.querySelector('.mobile-menu')).display !== 'none'
      `
    });

    // Check for horizontal overflow
    const hasOverflow = await chrome_evaluate({
      expression: 'document.body.scrollWidth > window.innerWidth'
    });

    results.push({
      breakpoint: bp.name,
      width: bp.width,
      mobileMenuVisible,
      hasOverflow,
    });
  }

  return results;
}
```

### 3. Interaction Testing

```typescript
async function testInteractions(url: string) {
  await chrome_navigate({ url });

  // Test dropdown
  await chrome_click({ selector: '.dropdown-trigger' });
  const dropdownOpen = await chrome_evaluate({
    expression: `
      document.querySelector('.dropdown-menu').classList.contains('open')
    `
  });

  // Test modal
  await chrome_click({ selector: '.open-modal' });
  const modalVisible = await chrome_evaluate({
    expression: 'document.querySelector(".modal").style.display !== "none"'
  });

  // Test keyboard navigation
  await chrome_evaluate({
    expression: `
      const event = new KeyboardEvent('keydown', { key: 'Escape' });
      document.dispatchEvent(event);
    `
  });
  const modalClosedOnEscape = await chrome_evaluate({
    expression: 'document.querySelector(".modal").style.display === "none"'
  });

  return { dropdownOpen, modalVisible, modalClosedOnEscape };
}
```

### 4. Visual Regression Testing

```typescript
async function visualRegression(url: string, baseline: string) {
  await chrome_navigate({ url });

  // Capture current screenshot
  const currentScreenshot = await chrome_screenshot({
    path: 'current.png',
    fullPage: true
  });

  // Compare with baseline (using external library like pixelmatch)
  const diff = await compareImages(baseline, currentScreenshot);

  return {
    pixelDiff: diff.pixelDiff,
    percentDiff: diff.percentDiff,
    passed: diff.percentDiff < 0.1, // 0.1% threshold
  };
}
```

---

## Test Automation Framework

### Test Suite Structure

```typescript
// test-suite.ts
interface TestCase {
  name: string;
  url: string;
  test: () => Promise<TestResult>;
}

interface TestResult {
  passed: boolean;
  message: string;
  details?: any;
}

class ChromeTestSuite {
  private tests: TestCase[] = [];

  add(name: string, url: string, test: () => Promise<TestResult>) {
    this.tests.push({ name, url, test });
  }

  async run() {
    const results = [];

    for (const test of this.tests) {
      console.log(`Running: ${test.name}`);

      try {
        await chrome_navigate({ url: test.url });
        const result = await test.test();

        results.push({
          test: test.name,
          ...result,
        });

        console.log(`${result.passed ? '✓' : '✗'} ${test.name}`);
      } catch (error) {
        results.push({
          test: test.name,
          passed: false,
          message: error.message,
        });

        console.log(`✗ ${test.name} (error)`);
      }
    }

    return results;
  }
}

// Usage
const suite = new ChromeTestSuite();

suite.add('Homepage loads', 'https://example.com', async () => {
  const title = await chrome_evaluate({
    expression: 'document.title'
  });

  return {
    passed: title.includes('Example'),
    message: `Title: ${title}`,
  };
});

suite.add('Search works', 'https://example.com', async () => {
  await chrome_fill({ selector: '#search', value: 'test query' });
  await chrome_click({ selector: '#search-button' });

  await new Promise(resolve => setTimeout(resolve, 1000));

  const results = await chrome_evaluate({
    expression: 'document.querySelectorAll(".search-result").length'
  });

  return {
    passed: results > 0,
    message: `Found ${results} results`,
    details: { resultCount: results },
  };
});

const results = await suite.run();
console.log('Test Summary:', results);
```

---

## Performance Monitoring

### Core Web Vitals Monitoring

```typescript
async function monitorCoreWebVitals(url: string) {
  await chrome_navigate({ url });

  const vitals = await chrome_evaluate({
    expression: `
      new Promise((resolve) => {
        const observer = new PerformanceObserver((list) => {
          const entries = list.getEntries();
          const vitals = {};

          entries.forEach((entry) => {
            if (entry.entryType === 'paint' && entry.name === 'first-contentful-paint') {
              vitals.fcp = entry.startTime;
            }
            if (entry.entryType === 'largest-contentful-paint') {
              vitals.lcp = entry.renderTime || entry.loadTime;
            }
            if (entry.entryType === 'layout-shift' && !entry.hadRecentInput) {
              vitals.cls = (vitals.cls || 0) + entry.value;
            }
          });

          setTimeout(() => resolve(vitals), 5000);
        });

        observer.observe({ entryTypes: ['paint', 'largest-contentful-paint', 'layout-shift'] });
      });
    `
  });

  return vitals;
}
```

---

## Debugging Workflows

### 1. Capture Application State

```typescript
async function captureState(url: string) {
  await chrome_navigate({ url });

  const state = await chrome_evaluate({
    expression: `
      ({
        url: window.location.href,
        cookies: document.cookie,
        localStorage: Object.fromEntries(Object.entries(localStorage)),
        sessionStorage: Object.fromEntries(Object.entries(sessionStorage)),
        domSnapshot: document.documentElement.outerHTML,
      })
    `
  });

  return state;
}
```

### 2. Network Waterfall Analysis

```typescript
async function analyzeWaterfall(url: string) {
  await chrome_navigate({ url });

  const requests = await chrome_network_logs();

  // Sort by start time
  const waterfall = requests
    .sort((a, b) => a.startTime - b.startTime)
    .map(req => ({
      url: req.url,
      method: req.method,
      status: req.status,
      size: req.encodedDataLength,
      time: req.time,
      startTime: req.startTime,
    }));

  // Identify bottlenecks
  const bottlenecks = waterfall.filter(req => req.time > 1000);

  return { waterfall, bottlenecks };
}
```

---

## Best Practices

1. **Wait for Elements**: Use explicit waits instead of arbitrary timeouts
2. **Cleanup**: Close browser instances after tests
3. **Error Handling**: Wrap tests in try-catch blocks
4. **Screenshots on Failure**: Capture evidence when tests fail
5. **Parallel Execution**: Run independent tests concurrently
6. **Retry Logic**: Implement retries for flaky tests
7. **Test Data Isolation**: Use unique test data to avoid conflicts

---

## Integration with CI/CD

```yaml
# .github/workflows/ui-tests.yml
name: UI Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Install Chrome
        uses: browser-actions/setup-chrome@v1

      - name: Run UI Tests
        run: |
          npx @modelcontextprotocol/server-chrome-devtools &
          npm run test:ui

      - name: Upload Screenshots
        if: failure()
        uses: actions/upload-artifact@v3
        with:
          name: screenshots
          path: screenshots/
```

---

## Troubleshooting

**Issue**: Chrome not launching
- Check `CHROME_EXECUTABLE_PATH` environment variable
- Ensure Chrome/Chromium is installed
- Try headless mode for CI environments

**Issue**: Flaky tests
- Increase wait times for dynamic content
- Use explicit waits instead of fixed delays
- Check for race conditions in async operations

**Issue**: Screenshots not capturing full page
- Use `fullPage: true` option
- Ensure page has finished loading
- Check for lazy-loaded content

---

## Remember

- **Always clean up browser instances** to avoid memory leaks
- **Use page selectors carefully** - prefer data-testid over class names
- **Test in headless mode for CI** but headed locally for debugging
- **Monitor performance regularly** to catch regressions early
- **Combine automated and manual testing** for comprehensive coverage
