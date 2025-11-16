---
name: web-accessibility-auditor
description: Audit and improve web accessibility compliance with WCAG 2.1 standards, screen reader support, and keyboard navigation
---

# Web Accessibility Auditor Skill

## Purpose

Ensure web applications are accessible to all users, including those with disabilities, by following WCAG 2.1 guidelines and best practices for keyboard navigation, screen readers, and assistive technologies.

---

## WCAG 2.1 Compliance Levels

- **Level A**: Minimum compliance (basic accessibility)
- **Level AA**: Standard compliance (recommended for most sites)
- **Level AAA**: Enhanced compliance (not required for most content)

**Target**: WCAG 2.1 AA compliance at minimum.

---

## Four POUR Principles

1. **Perceivable**: Information must be presentable to users in ways they can perceive
2. **Operable**: UI components must be operable by all users
3. **Understandable**: Information and operation must be understandable
4. **Robust**: Content must be robust enough to work with assistive technologies

---

## Critical Accessibility Checks

### 1. Semantic HTML

❌ **Bad**: Non-semantic elements
```html
<div class="button" onclick="submit()">Submit</div>
<div class="header">Page Title</div>
```

✅ **Good**: Semantic elements
```html
<button type="submit">Submit</button>
<h1>Page Title</h1>
```

**Semantic Elements:**
- `<header>`, `<nav>`, `<main>`, `<aside>`, `<footer>`
- `<article>`, `<section>`, `<figure>`, `<figcaption>`
- `<button>`, `<a>`, `<form>`, `<label>`
- `<h1>` - `<h6>` (logical heading hierarchy)

---

### 2. Color Contrast (WCAG AA)

**Minimum Ratios:**
- Normal text (< 24px): **4.5:1**
- Large text (≥ 24px or 19px bold): **3:1**
- UI components and graphics: **3:1**

❌ **Bad**: Low contrast
```css
.text {
  color: #999999;  /* Light gray */
  background: #ffffff;  /* White */
}
/* Contrast ratio: 2.85:1 (FAIL) */
```

✅ **Good**: High contrast
```css
.text {
  color: #595959;  /* Darker gray */
  background: #ffffff;  /* White */
}
/* Contrast ratio: 7.0:1 (PASS AA) */
```

**Testing Tools:**
- WebAIM Contrast Checker
- Browser DevTools (Chrome/Firefox)
- Axe DevTools extension

---

### 3. Alternative Text for Images

❌ **Bad**: Missing or meaningless alt text
```html
<img src="chart.png">
<img src="logo.png" alt="image">
```

✅ **Good**: Descriptive alt text
```html
<img src="chart.png" alt="Bar chart showing 50% increase in sales from Q1 to Q2">
<img src="logo.png" alt="Acme Corp logo">
```

**Guidelines:**
- **Informative images**: Describe the content/function
- **Decorative images**: Use `alt=""` (not remove alt attribute)
- **Complex images**: Provide longer description via `aria-describedby`
- **Functional images**: Describe the action (e.g., "Search" for search icon button)

```html
<!-- Decorative -->
<img src="divider.png" alt="" role="presentation">

<!-- Complex diagram -->
<img src="diagram.png" alt="System architecture diagram" aria-describedby="diagram-desc">
<div id="diagram-desc">
  The diagram shows three layers: frontend, API gateway, and database...
</div>
```

---

### 4. Keyboard Navigation

All interactive elements must be keyboard accessible.

**Required Keyboard Support:**
- `Tab`: Navigate to next focusable element
- `Shift + Tab`: Navigate to previous element
- `Enter`: Activate buttons and links
- `Space`: Activate buttons, toggle checkboxes
- `Arrow keys`: Navigate within components (radio groups, menus, tabs)
- `Esc`: Close modals/dialogs

❌ **Bad**: Click-only interaction
```html
<div onclick="handleClick()">Click me</div>
```

✅ **Good**: Keyboard accessible
```html
<button onclick="handleClick()">Click me</button>

<!-- Custom interactive element -->
<div
  role="button"
  tabindex="0"
  onclick="handleClick()"
  onkeydown="(e) => (e.key === 'Enter' || e.key === ' ') && handleClick()"
>
  Click me
</div>
```

**Focus Indicators:**
```css
/* Visible focus styles */
:focus-visible {
  outline: 3px solid #005fcc;
  outline-offset: 2px;
}

/* Never remove focus outline without replacement */
button:focus-visible {
  outline: 3px solid var(--color-focus);
  box-shadow: 0 0 0 4px rgba(0, 95, 204, 0.2);
}
```

---

### 5. ARIA Labels and Landmarks

**ARIA Landmarks:**
```html
<header role="banner">
  <nav role="navigation" aria-label="Main navigation">...</nav>
</header>

<main role="main">
  <article>...</article>
</main>

<aside role="complementary">...</aside>

<footer role="contentinfo">...</footer>
```

**ARIA Labels:**
```html
<!-- Label icon-only buttons -->
<button aria-label="Close dialog">
  <XIcon />
</button>

<!-- Describe purpose when visible label insufficient -->
<button aria-label="Search products">
  <SearchIcon />
  Search
</button>

<!-- Group related inputs -->
<fieldset>
  <legend>Shipping address</legend>
  <label for="street">Street</label>
  <input id="street" type="text">
</fieldset>
```

**ARIA States:**
```html
<!-- Expanded/collapsed -->
<button aria-expanded="false" aria-controls="menu">
  Menu
</button>
<div id="menu" hidden>...</div>

<!-- Current page in navigation -->
<nav>
  <a href="/" aria-current="page">Home</a>
  <a href="/about">About</a>
</nav>

<!-- Loading state -->
<button aria-busy="true">
  <span aria-live="polite" aria-atomic="true">Loading...</span>
</button>

<!-- Toggle button -->
<button aria-pressed="false">Mute</button>
```

---

### 6. Form Accessibility

❌ **Bad**: Unlabeled inputs
```html
<input type="text" placeholder="Name">
<input type="email" placeholder="Email">
```

✅ **Good**: Properly labeled and associated
```html
<label for="name">Name</label>
<input id="name" type="text" required aria-required="true">

<label for="email">Email</label>
<input id="email" type="email" required aria-required="true" aria-describedby="email-help">
<span id="email-help">We'll never share your email</span>
```

**Error Handling:**
```html
<label for="email">Email</label>
<input
  id="email"
  type="email"
  aria-invalid="true"
  aria-describedby="email-error"
>
<span id="email-error" role="alert">
  Please enter a valid email address
</span>
```

**Required Fields:**
```html
<!-- Visual + semantic indication -->
<label for="name">
  Name <span aria-label="required">*</span>
</label>
<input id="name" type="text" required aria-required="true">
```

---

### 7. Modal/Dialog Accessibility

```html
<div
  role="dialog"
  aria-labelledby="dialog-title"
  aria-describedby="dialog-desc"
  aria-modal="true"
>
  <h2 id="dialog-title">Confirm deletion</h2>
  <p id="dialog-desc">Are you sure you want to delete this item?</p>

  <button>Cancel</button>
  <button>Delete</button>
</div>
```

**Focus Management:**
```javascript
// Trap focus within modal
const modal = document.querySelector('[role="dialog"]');
const focusableElements = modal.querySelectorAll(
  'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
);

const firstElement = focusableElements[0];
const lastElement = focusableElements[focusableElements.length - 1];

// Focus first element when modal opens
firstElement.focus();

// Trap focus
modal.addEventListener('keydown', (e) => {
  if (e.key !== 'Tab') return;

  if (e.shiftKey) {
    if (document.activeElement === firstElement) {
      lastElement.focus();
      e.preventDefault();
    }
  } else {
    if (document.activeElement === lastElement) {
      firstElement.focus();
      e.preventDefault();
    }
  }
});

// Close on Escape
modal.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') {
    closeModal();
  }
});
```

---

### 8. Skip Links

Allow keyboard users to skip repetitive navigation.

```html
<a href="#main-content" class="skip-link">
  Skip to main content
</a>

<header>
  <nav>...</nav>
</header>

<main id="main-content" tabindex="-1">
  <h1>Page Title</h1>
  ...
</main>
```

```css
.skip-link {
  position: absolute;
  top: -40px;
  left: 0;
  background: #000;
  color: #fff;
  padding: 8px;
  text-decoration: none;
  z-index: 100;
}

.skip-link:focus {
  top: 0;
}
```

---

### 9. Live Regions

Announce dynamic content updates to screen readers.

```html
<!-- Polite: announced when user is idle -->
<div aria-live="polite" aria-atomic="true">
  3 new messages
</div>

<!-- Assertive: announced immediately -->
<div aria-live="assertive" role="alert">
  Error: Form submission failed
</div>

<!-- Status updates -->
<div role="status" aria-live="polite">
  Saving...
</div>
```

**Common Use Cases:**
- Form validation errors: `role="alert"`
- Loading states: `aria-live="polite"`
- Toast notifications: `role="status"`
- Search results count: `aria-live="polite"`

---

### 10. Tables

```html
<table>
  <caption>Sales by quarter</caption>
  <thead>
    <tr>
      <th scope="col">Quarter</th>
      <th scope="col">Revenue</th>
      <th scope="col">Growth</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Q1 2024</th>
      <td>$1.2M</td>
      <td>15%</td>
    </tr>
  </tbody>
</table>
```

**Guidelines:**
- Use `<caption>` for table title
- Use `<th>` for headers with `scope="col"` or `scope="row"`
- Don't use tables for layout

---

## Accessibility Testing Checklist

### Automated Testing
- [ ] **Axe DevTools**: Browser extension scan
- [ ] **Lighthouse**: Accessibility score > 90
- [ ] **Pa11y**: Automated CLI testing
- [ ] **jest-axe**: Unit test accessibility

```javascript
// jest-axe example
import { render } from '@testing-library/react';
import { axe, toHaveNoViolations } from 'jest-axe';
import { Button } from './Button';

expect.extend(toHaveNoViolations);

test('Button has no accessibility violations', async () => {
  const { container } = render(<Button>Click me</Button>);
  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

### Manual Testing
- [ ] **Keyboard only**: Navigate entire site without mouse
- [ ] **Screen reader**: Test with NVDA (Windows) or VoiceOver (Mac)
- [ ] **Zoom**: Test at 200% zoom without horizontal scrolling
- [ ] **Color blindness**: Use Color Oracle simulator
- [ ] **High contrast mode**: Test on Windows high contrast
- [ ] **Touch targets**: Minimum 44x44px on mobile

### Screen Reader Testing
**VoiceOver (Mac):**
- `Cmd + F5`: Toggle VoiceOver
- `Ctrl + Option + →`: Next element
- `Ctrl + Option + Space`: Activate element

**NVDA (Windows):**
- `Insert + Down`: Read next element
- `Tab`: Navigate to next focusable element
- `H`: Jump to next heading
- `K`: Jump to next link

---

## Common Accessibility Issues

### 1. Missing Focus Management
```javascript
// ❌ Bad: No focus management when opening modal
function openModal() {
  modal.style.display = 'block';
}

// ✅ Good: Focus first element in modal
function openModal() {
  modal.style.display = 'block';
  modal.querySelector('button').focus();
  // Store previous focus to restore later
  previousFocus = document.activeElement;
}

function closeModal() {
  modal.style.display = 'none';
  previousFocus.focus(); // Restore focus
}
```

### 2. Insufficient Color Contrast
Use design system tokens with tested contrast ratios.

### 3. Auto-playing Media
```html
<!-- Provide controls and don't autoplay -->
<video controls>
  <source src="video.mp4" type="video/mp4">
  <track kind="captions" src="captions.vtt" srclang="en" label="English">
</video>
```

### 4. Time Limits
Allow users to extend or disable time limits.

```html
<div role="alert">
  Session expiring in 60 seconds.
  <button>Extend session</button>
</div>
```

---

## Tools & Resources

**Testing:**
- Axe DevTools (browser extension)
- Lighthouse (Chrome DevTools)
- WAVE (WebAIM evaluation tool)
- Pa11y (automated testing)
- Color Contrast Analyzer

**Screen Readers:**
- NVDA (Windows, free)
- JAWS (Windows, paid)
- VoiceOver (Mac/iOS, built-in)
- TalkBack (Android, built-in)

**Guidelines:**
- WCAG 2.1 (https://www.w3.org/WAI/WCAG21/quickref/)
- ARIA Authoring Practices (https://www.w3.org/WAI/ARIA/apg/)
- WebAIM resources (https://webaim.org/)

---

## Quick Reference: ARIA Roles

| Role | Use Case |
|------|----------|
| `button` | Custom button (div/span) |
| `checkbox` | Custom checkbox |
| `dialog` | Modal dialog |
| `alert` | Important message |
| `status` | Status update |
| `navigation` | Navigation section |
| `search` | Search form |
| `tablist`, `tab`, `tabpanel` | Tab interface |
| `menu`, `menuitem` | Dropdown menu |
| `listbox`, `option` | Custom select |

---

## Remember

- **Accessibility is not optional**: It's a legal requirement in many jurisdictions
- **Test with real users**: Automated tools catch only ~30% of issues
- **Design for accessibility from the start**: Retrofitting is expensive
- **Semantic HTML first**: ARIA should enhance, not replace, native elements
- **No ARIA is better than bad ARIA**: Incorrect ARIA can make things worse
