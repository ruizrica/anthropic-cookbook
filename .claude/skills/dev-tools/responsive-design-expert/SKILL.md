---
name: responsive-design-expert
description: Build mobile-first responsive designs with accessibility, performance, and cross-device compatibility
---

# Responsive Design Expert Skill

## Purpose

Create responsive web applications that work seamlessly across all devices and screen sizes, prioritizing mobile-first design, accessibility, and performance.

---

## Mobile-First Approach

### Philosophy
Start with the smallest screen and progressively enhance for larger viewports. This ensures:
- Faster mobile load times (most users)
- Simpler CSS (additive, not subtractive)
- Better performance on constrained devices

### Base Styles (Mobile)
```css
/* Mobile-first base styles */
.container {
  padding: 1rem;
  max-width: 100%;
}

.grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

.button {
  width: 100%;
  padding: 1rem;
  font-size: 1rem;
}
```

### Progressive Enhancement (Tablet & Desktop)
```css
/* Tablet (768px and up) */
@media (min-width: 48rem) {
  .container {
    padding: 2rem;
  }

  .grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 2rem;
  }

  .button {
    width: auto;
    padding: 0.75rem 2rem;
  }
}

/* Desktop (1024px and up) */
@media (min-width: 64rem) {
  .container {
    max-width: 1200px;
    margin: 0 auto;
  }

  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

/* Large screens (1440px and up) */
@media (min-width: 90rem) {
  .container {
    max-width: 1440px;
  }

  .grid {
    grid-template-columns: repeat(4, 1fr);
  }
}
```

---

## Standard Breakpoints

Use consistent breakpoints across your application:

```css
:root {
  --breakpoint-xs: 20rem;    /* 320px - Small phones */
  --breakpoint-sm: 36rem;    /* 576px - Large phones */
  --breakpoint-md: 48rem;    /* 768px - Tablets */
  --breakpoint-lg: 64rem;    /* 1024px - Small laptops */
  --breakpoint-xl: 80rem;    /* 1280px - Desktops */
  --breakpoint-2xl: 96rem;   /* 1536px - Large screens */
}
```

**Common Device Targets:**
- **Mobile Portrait**: < 576px
- **Mobile Landscape**: 576px - 767px
- **Tablet Portrait**: 768px - 1023px
- **Tablet Landscape / Small Laptop**: 1024px - 1279px
- **Desktop**: 1280px - 1535px
- **Large Desktop**: ≥ 1536px

---

## Responsive Layout Patterns

### 1. Fluid Grid with CSS Grid
```css
.grid-auto-fit {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
}

.grid-auto-fill {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
}
```

**Difference:**
- `auto-fit`: Columns expand to fill available space
- `auto-fill`: Creates empty columns if space available

### 2. Flexbox Responsive Wrapper
```css
.flex-wrap {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.flex-item {
  flex: 1 1 300px; /* grow, shrink, basis */
  min-width: 250px;
}
```

### 3. Container Queries (Modern Approach)
```css
.card-container {
  container-type: inline-size;
  container-name: card;
}

.card {
  padding: 1rem;
}

/* Style based on container width, not viewport */
@container card (min-width: 400px) {
  .card {
    display: grid;
    grid-template-columns: 200px 1fr;
    padding: 2rem;
  }
}
```

### 4. Sidebar Layout
```css
.layout {
  display: grid;
  gap: 2rem;
  grid-template-columns: 1fr; /* Mobile: stacked */
}

@media (min-width: 64rem) {
  .layout {
    grid-template-columns: 250px 1fr; /* Desktop: sidebar + main */
  }
}

@media (min-width: 90rem) {
  .layout {
    grid-template-columns: 300px 1fr 250px; /* Large: sidebar + main + aside */
  }
}
```

---

## Responsive Typography

### Fluid Typography with clamp()
```css
:root {
  /* Fluid font sizes that scale with viewport */
  --font-size-sm: clamp(0.875rem, 0.8rem + 0.2vw, 1rem);
  --font-size-base: clamp(1rem, 0.9rem + 0.5vw, 1.125rem);
  --font-size-lg: clamp(1.125rem, 1rem + 0.625vw, 1.5rem);
  --font-size-xl: clamp(1.5rem, 1.2rem + 1.5vw, 2.5rem);
  --font-size-2xl: clamp(2rem, 1.5rem + 2.5vw, 4rem);
}

body {
  font-size: var(--font-size-base);
}

h1 {
  font-size: var(--font-size-2xl);
}

h2 {
  font-size: var(--font-size-xl);
}

h3 {
  font-size: var(--font-size-lg);
}
```

**clamp() syntax:** `clamp(min, preferred, max)`

### Responsive Line Height
```css
body {
  line-height: 1.6; /* Mobile: tighter for easier reading */
}

@media (min-width: 48rem) {
  body {
    line-height: 1.7; /* Desktop: more breathing room */
  }
}
```

### Responsive Measure (Line Length)
```css
.prose {
  max-width: 65ch; /* Optimal: 45-75 characters per line */
  margin: 0 auto;
}
```

---

## Responsive Images

### 1. Fluid Images
```css
img {
  max-width: 100%;
  height: auto;
  display: block;
}
```

### 2. Picture Element with Art Direction
```html
<picture>
  <!-- Desktop: landscape crop -->
  <source
    media="(min-width: 1024px)"
    srcset="hero-desktop.jpg 1x, hero-desktop@2x.jpg 2x"
  />
  <!-- Tablet: square crop -->
  <source
    media="(min-width: 768px)"
    srcset="hero-tablet.jpg 1x, hero-tablet@2x.jpg 2x"
  />
  <!-- Mobile: portrait crop -->
  <img
    src="hero-mobile.jpg"
    srcset="hero-mobile@2x.jpg 2x"
    alt="Hero image"
    loading="lazy"
  />
</picture>
```

### 3. Responsive Background Images
```css
.hero {
  background-image: url('hero-mobile.jpg');
  background-size: cover;
  background-position: center;
}

@media (min-width: 48rem) {
  .hero {
    background-image: url('hero-tablet.jpg');
  }
}

@media (min-width: 64rem) {
  .hero {
    background-image: url('hero-desktop.jpg');
  }
}

/* High DPI screens */
@media (-webkit-min-device-pixel-ratio: 2), (min-resolution: 192dpi) {
  .hero {
    background-image: url('hero-desktop@2x.jpg');
  }
}
```

### 4. Modern Image Formats
```html
<picture>
  <source type="image/avif" srcset="image.avif" />
  <source type="image/webp" srcset="image.webp" />
  <img src="image.jpg" alt="Fallback" />
</picture>
```

---

## Touch-Friendly UI

### Minimum Touch Targets
```css
/* WCAG 2.1 Level AAA: 44x44px minimum */
.button,
.link,
.interactive {
  min-height: 44px;
  min-width: 44px;
  padding: 12px 16px;
}

/* Mobile: even larger for thumbs */
@media (max-width: 48rem) {
  .button {
    min-height: 48px;
    padding: 14px 20px;
  }
}
```

### Hover vs Touch States
```css
/* Only apply hover on devices that support it */
@media (hover: hover) and (pointer: fine) {
  .button:hover {
    background-color: var(--color-primary-dark);
  }
}

/* Touch devices: use active state */
.button:active {
  transform: scale(0.98);
}
```

### Prevent Double-Tap Zoom
```html
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
```

**Note:** Only use `maximum-scale=1` for app-like experiences, not content sites (reduces accessibility).

---

## Responsive Navigation Patterns

### 1. Hamburger Menu (Mobile)
```html
<nav class="nav">
  <button class="nav__toggle" aria-label="Toggle menu" aria-expanded="false">
    <span class="hamburger"></span>
  </button>
  <ul class="nav__menu">
    <li><a href="/">Home</a></li>
    <li><a href="/about">About</a></li>
    <li><a href="/contact">Contact</a></li>
  </ul>
</nav>
```

```css
/* Mobile: hidden menu */
.nav__menu {
  display: none;
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  background: white;
  padding: 1rem;
}

.nav__menu[data-open="true"] {
  display: flex;
  flex-direction: column;
}

/* Desktop: horizontal menu */
@media (min-width: 48rem) {
  .nav__toggle {
    display: none;
  }

  .nav__menu {
    display: flex;
    position: static;
    flex-direction: row;
    gap: 2rem;
  }
}
```

### 2. Priority+ Navigation
Show primary items, hide overflow in "More" menu
```html
<nav class="nav">
  <ul class="nav__primary">
    <li>Home</li>
    <li>About</li>
    <li>Services</li>
    <li>Contact</li>
  </ul>
  <details class="nav__more">
    <summary>More</summary>
    <ul>
      <!-- Overflow items added via JS -->
    </ul>
  </details>
</nav>
```

---

## Performance Optimization

### 1. Critical CSS
```html
<head>
  <!-- Inline critical CSS for above-the-fold content -->
  <style>
    /* Critical mobile styles */
  </style>

  <!-- Load full CSS asynchronously -->
  <link rel="preload" href="styles.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
  <noscript><link rel="stylesheet" href="styles.css"></noscript>
</head>
```

### 2. Lazy Loading
```html
<!-- Images below the fold -->
<img src="image.jpg" loading="lazy" alt="Description">

<!-- Iframe embeds -->
<iframe src="video.html" loading="lazy"></iframe>
```

### 3. Reduce Layout Shift (CLS)
```css
/* Reserve space for images */
img {
  aspect-ratio: 16 / 9;
  object-fit: cover;
}

/* Or use explicit dimensions */
.hero-image {
  width: 100%;
  height: 400px;
}
```

---

## Accessibility Considerations

### 1. Responsive Text Zoom
```css
/* Use rem/em, not px, for font sizes */
body {
  font-size: 1rem; /* Respects user's browser settings */
}

/* Users can zoom text to 200% without breaking layout */
.container {
  max-width: 100%;
  overflow-wrap: break-word;
}
```

### 2. Focus Indicators
```css
/* Visible focus for keyboard navigation */
:focus-visible {
  outline: 3px solid var(--color-focus);
  outline-offset: 2px;
}

/* Mobile: larger focus areas */
@media (max-width: 48rem) {
  :focus-visible {
    outline-width: 4px;
  }
}
```

### 3. Reduced Motion
```css
/* Respect user's motion preferences */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

### 4. Dark Mode
```css
@media (prefers-color-scheme: dark) {
  :root {
    --color-bg: #1a1b26;
    --color-text: #c0caf5;
  }
}
```

---

## Testing Checklist

- [ ] **Viewport Meta Tag**: Properly configured
- [ ] **Breakpoint Testing**: All major breakpoints (320px, 768px, 1024px, 1440px)
- [ ] **Touch Targets**: Minimum 44x44px
- [ ] **Text Readability**: Line length 45-75 characters
- [ ] **Image Optimization**: Responsive images with srcset
- [ ] **Performance**: Lighthouse score > 90 on mobile
- [ ] **Accessibility**: WCAG AA compliance, keyboard navigation
- [ ] **Cross-Browser**: Chrome, Firefox, Safari, Edge
- [ ] **Device Testing**: iOS Safari, Android Chrome
- [ ] **Orientation**: Portrait and landscape modes
- [ ] **Text Zoom**: Layout doesn't break at 200% zoom
- [ ] **Slow Networks**: Test on throttled 3G connection

---

## Tools & Resources

**Testing:**
- Chrome DevTools Device Mode
- Firefox Responsive Design Mode
- BrowserStack for real device testing
- Lighthouse for performance auditing

**Design:**
- Figma/Sketch with responsive frames
- Responsively App for multi-device preview

**Code:**
- Tailwind CSS utility classes
- Bootstrap responsive grid
- CSS Grid and Flexbox

---

## Common Patterns Summary

| Pattern | Use Case | Implementation |
|---------|----------|----------------|
| Fluid Grid | Card layouts, galleries | `grid-template-columns: repeat(auto-fit, minmax(250px, 1fr))` |
| Sidebar Layout | Dashboard, docs | Grid with columns changing at breakpoints |
| Hamburger Menu | Mobile navigation | Hidden menu shown via JS toggle |
| Responsive Images | Art direction | `<picture>` with multiple sources |
| Fluid Typography | Scalable text | `clamp(min, preferred, max)` |
| Container Queries | Component-based responsive | `@container` queries |
| Touch Targets | Mobile UI | `min-height: 44px; min-width: 44px` |

---

## Remember

- **Mobile-first** always (easier to enhance than strip down)
- **Test on real devices** (simulators don't catch everything)
- **Performance matters** (especially on mobile networks)
- **Accessibility is not optional** (screen readers, keyboard, zoom)
- **Content over containers** (let content determine breakpoints, not devices)
