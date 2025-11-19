---
name: ui-animation-specialist
description: Create performant, accessible UI animations using CSS, Framer Motion, and modern web animation techniques
---

# UI Animation Specialist Skill

## Purpose

Design and implement smooth, performant UI animations that enhance user experience without compromising accessibility or performance.

---

## Animation Principles

### 1. Purpose-Driven Animation
Every animation should serve a purpose:
- **Feedback**: Confirm user actions (button press, form submit)
- **Transition**: Guide attention during state changes
- **Context**: Show relationships between elements
- **Delight**: Add personality without distraction

### 2. Performance First
Only animate properties that don't trigger layout/paint:
- ✅ **Transform**: translate, scale, rotate
- ✅ **Opacity**: fade in/out
- ❌ **Width/Height**: Causes reflow
- ❌ **Top/Left**: Use transform instead
- ❌ **Background-color**: Use opacity on overlay instead (when possible)

### 3. Timing & Easing
```css
:root {
  /* Duration */
  --duration-instant: 100ms;
  --duration-fast: 200ms;
  --duration-normal: 300ms;
  --duration-slow: 500ms;

  /* Easing curves */
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);  /* Snappy exit */
  --ease-in: cubic-bezier(0.7, 0, 0.84, 0);   /* Accelerating */
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1); /* Smooth both ways */
  --ease-spring: cubic-bezier(0.34, 1.56, 0.64, 1); /* Bounce effect */
}
```

**Timing Guidelines:**
- Micro-interactions: 100-200ms
- Standard transitions: 200-300ms
- Complex animations: 400-600ms
- Page transitions: 300-500ms

---

## CSS Animations

### 1. Fade In
```css
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.fade-in {
  animation: fadeIn 300ms var(--ease-out) forwards;
}
```

### 2. Fade In Up (Entry Animation)
```css
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.fade-in-up {
  animation: fadeInUp 400ms var(--ease-out) forwards;
}

/* Staggered children */
.stagger-children > * {
  animation: fadeInUp 400ms var(--ease-out) forwards;
  opacity: 0;
}

.stagger-children > *:nth-child(1) { animation-delay: 0ms; }
.stagger-children > *:nth-child(2) { animation-delay: 100ms; }
.stagger-children > *:nth-child(3) { animation-delay: 200ms; }
.stagger-children > *:nth-child(4) { animation-delay: 300ms; }
```

### 3. Scale In
```css
@keyframes scaleIn {
  from {
    opacity: 0;
    transform: scale(0.9);
  }
  to {
    opacity: 1;
    transform: scale(1);
  }
}

.modal {
  animation: scaleIn 300ms var(--ease-out) forwards;
}
```

### 4. Slide In (Drawer/Sheet)
```css
@keyframes slideInRight {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(0);
  }
}

.drawer {
  animation: slideInRight 300ms var(--ease-out) forwards;
}
```

### 5. Skeleton Loading (Shimmer Effect)
```css
@keyframes shimmer {
  0% {
    background-position: -1000px 0;
  }
  100% {
    background-position: 1000px 0;
  }
}

.skeleton {
  background: linear-gradient(
    90deg,
    #f0f0f0 25%,
    #e0e0e0 50%,
    #f0f0f0 75%
  );
  background-size: 1000px 100%;
  animation: shimmer 2s infinite linear;
}
```

### 6. Pulse (Loading Indicator)
```css
@keyframes pulse {
  0%, 100% {
    opacity: 1;
  }
  50% {
    opacity: 0.5;
  }
}

.loading {
  animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}
```

### 7. Spin (Spinner)
```css
@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.spinner {
  animation: spin 1s linear infinite;
}
```

### 8. Bounce (Attention)
```css
@keyframes bounce {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-10px);
  }
}

.notification-badge {
  animation: bounce 500ms var(--ease-spring);
}
```

---

## Transition Patterns

### 1. Hover Interactions
```css
.card {
  transition: transform 300ms var(--ease-out),
              box-shadow 300ms var(--ease-out);
}

.card:hover {
  transform: translateY(-4px);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.15);
}

/* Button press feedback */
.button {
  transition: transform 100ms var(--ease-out);
}

.button:active {
  transform: scale(0.98);
}
```

### 2. Accordion/Collapse
```css
.accordion-content {
  max-height: 0;
  overflow: hidden;
  transition: max-height 400ms var(--ease-in-out);
}

.accordion-content[data-open="true"] {
  max-height: 1000px; /* Set to large value */
}

/* Better approach with grid */
.accordion-content {
  display: grid;
  grid-template-rows: 0fr;
  transition: grid-template-rows 400ms var(--ease-in-out);
}

.accordion-content[data-open="true"] {
  grid-template-rows: 1fr;
}

.accordion-content > div {
  overflow: hidden;
}
```

### 3. Page Transitions (View Transitions API)
```css
/* Modern browsers with View Transitions API */
::view-transition-old(root) {
  animation: fadeOut 300ms var(--ease-out);
}

::view-transition-new(root) {
  animation: fadeIn 300ms var(--ease-out);
}

@keyframes fadeOut {
  to {
    opacity: 0;
  }
}
```

```javascript
// JavaScript to trigger
function navigateWithTransition(url) {
  if (!document.startViewTransition) {
    window.location.href = url;
    return;
  }

  document.startViewTransition(() => {
    window.location.href = url;
  });
}
```

---

## Framer Motion (React)

### 1. Basic Animation
```tsx
import { motion } from 'framer-motion';

<motion.div
  initial={{ opacity: 0, y: 20 }}
  animate={{ opacity: 1, y: 0 }}
  transition={{ duration: 0.4, ease: [0.16, 1, 0.3, 1] }}
>
  Content
</motion.div>
```

### 2. Stagger Children
```tsx
const containerVariants = {
  hidden: { opacity: 0 },
  show: {
    opacity: 1,
    transition: {
      staggerChildren: 0.1,
      delayChildren: 0.2,
    },
  },
};

const itemVariants = {
  hidden: { opacity: 0, y: 20 },
  show: { opacity: 1, y: 0 },
};

<motion.ul
  variants={containerVariants}
  initial="hidden"
  animate="show"
>
  {items.map((item) => (
    <motion.li key={item.id} variants={itemVariants}>
      {item.text}
    </motion.li>
  ))}
</motion.ul>
```

### 3. Layout Animations
```tsx
/* Automatically animate layout changes */
<motion.div layout>
  {isExpanded ? <ExpandedContent /> : <CollapsedContent />}
</motion.div>

/* Shared element transition */
<motion.div layoutId="card">
  <img src={image} />
</motion.div>

/* On another page/state: */
<motion.div layoutId="card">
  <img src={image} />
</motion.div>
```

### 4. Gesture Animations
```tsx
<motion.button
  whileHover={{ scale: 1.05 }}
  whileTap={{ scale: 0.95 }}
  transition={{ type: 'spring', stiffness: 400, damping: 17 }}
>
  Click me
</motion.button>

/* Drag */
<motion.div
  drag
  dragConstraints={{ left: 0, right: 300, top: 0, bottom: 300 }}
  dragElastic={0.2}
/>
```

### 5. Scroll-Triggered Animations
```tsx
import { useInView } from 'framer-motion';
import { useRef } from 'react';

function Component() {
  const ref = useRef(null);
  const isInView = useInView(ref, { once: true, amount: 0.5 });

  return (
    <motion.div
      ref={ref}
      initial={{ opacity: 0, y: 50 }}
      animate={isInView ? { opacity: 1, y: 0 } : { opacity: 0, y: 50 }}
      transition={{ duration: 0.6 }}
    >
      Content appears when scrolled into view
    </motion.div>
  );
}
```

### 6. Exit Animations (with AnimatePresence)
```tsx
import { AnimatePresence, motion } from 'framer-motion';

<AnimatePresence mode="wait">
  {isVisible && (
    <motion.div
      initial={{ opacity: 0, scale: 0.9 }}
      animate={{ opacity: 1, scale: 1 }}
      exit={{ opacity: 0, scale: 0.9 }}
      transition={{ duration: 0.2 }}
    >
      Modal content
    </motion.div>
  )}
</AnimatePresence>
```

---

## Web Animations API (WAAPI)

### 1. Basic JavaScript Animation
```javascript
const element = document.querySelector('.box');

element.animate(
  [
    { opacity: 0, transform: 'translateY(20px)' },
    { opacity: 1, transform: 'translateY(0)' },
  ],
  {
    duration: 400,
    easing: 'cubic-bezier(0.16, 1, 0.3, 1)',
    fill: 'forwards',
  }
);
```

### 2. Scroll-Linked Animation
```javascript
// Intersection Observer for scroll triggers
const observer = new IntersectionObserver(
  (entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.animate(
          [
            { opacity: 0, transform: 'translateY(50px)' },
            { opacity: 1, transform: 'translateY(0)' },
          ],
          {
            duration: 600,
            easing: 'cubic-bezier(0.16, 1, 0.3, 1)',
            fill: 'forwards',
          }
        );
        observer.unobserve(entry.target);
      }
    });
  },
  { threshold: 0.5 }
);

document.querySelectorAll('.animate-on-scroll').forEach((el) => {
  observer.observe(el);
});
```

---

## Performance Optimization

### 1. GPU Acceleration
```css
/* Force GPU acceleration for smoother animations */
.animated {
  will-change: transform, opacity;
  transform: translateZ(0); /* Create new layer */
}

/* Remove will-change after animation */
.animated.done {
  will-change: auto;
}
```

### 2. Reduce Motion for Accessibility
```css
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

```tsx
// Framer Motion: respect user preferences
import { useReducedMotion } from 'framer-motion';

function Component() {
  const shouldReduceMotion = useReducedMotion();

  return (
    <motion.div
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      transition={{
        duration: shouldReduceMotion ? 0 : 0.4,
      }}
    />
  );
}
```

### 3. Lazy Load Animations
```javascript
// Only load animation library when needed
const loadAnimations = async () => {
  const { animate } = await import('framer-motion');
  return animate;
};
```

---

## Common Animation Patterns

### Loading States
```tsx
// Spinner
<motion.div
  animate={{ rotate: 360 }}
  transition={{ duration: 1, repeat: Infinity, ease: 'linear' }}
>
  <SpinnerIcon />
</motion.div>

// Progress Bar
<motion.div
  initial={{ width: 0 }}
  animate={{ width: `${progress}%` }}
  transition={{ duration: 0.5 }}
/>

// Skeleton
<div className="skeleton" /> {/* CSS shimmer animation */}
```

### List Animations
```tsx
// Add/Remove with stagger
<AnimatePresence>
  {items.map((item, index) => (
    <motion.li
      key={item.id}
      initial={{ opacity: 0, x: -20 }}
      animate={{ opacity: 1, x: 0 }}
      exit={{ opacity: 0, x: 20 }}
      transition={{ delay: index * 0.05 }}
    >
      {item.text}
    </motion.li>
  ))}
</AnimatePresence>
```

### Modal/Dialog
```tsx
<AnimatePresence>
  {isOpen && (
    <>
      {/* Backdrop */}
      <motion.div
        initial={{ opacity: 0 }}
        animate={{ opacity: 1 }}
        exit={{ opacity: 0 }}
        className="backdrop"
        onClick={onClose}
      />

      {/* Modal */}
      <motion.div
        initial={{ opacity: 0, scale: 0.9, y: 20 }}
        animate={{ opacity: 1, scale: 1, y: 0 }}
        exit={{ opacity: 0, scale: 0.9, y: 20 }}
        className="modal"
      >
        {children}
      </motion.div>
    </>
  )}
</AnimatePresence>
```

### Toast Notifications
```tsx
<AnimatePresence>
  {toasts.map((toast) => (
    <motion.div
      key={toast.id}
      initial={{ opacity: 0, y: 50, scale: 0.3 }}
      animate={{ opacity: 1, y: 0, scale: 1 }}
      exit={{ opacity: 0, scale: 0.5, transition: { duration: 0.2 } }}
      layout
    >
      {toast.message}
    </motion.div>
  ))}
</AnimatePresence>
```

---

## Animation Checklist

- [ ] **Performance**: Only animate `transform` and `opacity`
- [ ] **Duration**: Appropriate timing (100-600ms)
- [ ] **Easing**: Smooth cubic-bezier curves
- [ ] **Purpose**: Animation serves a clear purpose
- [ ] **Accessibility**: Respects `prefers-reduced-motion`
- [ ] **GPU**: Uses `will-change` or `transform: translateZ(0)` for heavy animations
- [ ] **Exit Animations**: Elements animate out before removal
- [ ] **Stagger**: Related elements stagger for visual flow
- [ ] **Loading States**: Clear feedback during async operations
- [ ] **Tested**: Verified on low-end devices

---

## Tools & Libraries

**CSS:**
- Native CSS animations and transitions
- View Transitions API (modern browsers)

**React:**
- Framer Motion (comprehensive, 55kb)
- React Spring (physics-based, 28kb)
- GSAP React (powerful, larger bundle)

**Vue:**
- Vue transitions built-in
- GSAP for complex sequences

**JavaScript:**
- Web Animations API (WAAPI)
- GSAP (industry standard)
- Anime.js (lightweight alternative)

---

## Remember

- **Less is more**: Too much animation is distracting
- **Performance first**: Smooth 60fps on all devices
- **Accessibility**: Respect user preferences
- **Purposeful**: Every animation should enhance UX
- **Consistent**: Reuse easing curves and durations
