---
name: modern-frontend-design
description: Create distinctive, modern frontend designs avoiding generic "AI slop" aesthetics using typography, color theory, motion, and backgrounds
---

# Modern Frontend Design Skill

## Purpose

This skill helps you create frontend designs that stand out from generic, cookie-cutter AI-generated interfaces. It addresses the "distributional convergence" problem where models default to safe, predictable design choices (Inter fonts, timid colors, static layouts).

## Core Problem: Avoiding Generic Outputs

**Without direction**, Claude samples from high-probability design patterns:
- Typography: Inter, Roboto, Open Sans, system fonts
- Colors: Evenly distributed, timid palettes
- Motion: Minimal or no animations
- Backgrounds: Solid colors

**With this skill**, you'll create distinctive designs across four key dimensions.

---

## Design Dimensions

### 1. Typography

**Avoid These Fonts** (overused in AI-generated designs):
- Inter, Roboto, Open Sans, Lato, Montserrat
- System fonts (Arial, Helvetica, sans-serif)
- Space Grotesk (new convergence point)

**Instead, Choose:**
- **High-contrast pairings**: Combine serif + sans-serif
- **Extreme weight variations**: Ultra-thin (100-200) with ultra-bold (800-900)
- **Distinctive character**: Fonts with personality and unique letterforms
- **Google Fonts recommendations**:
  - Display: Playfair Display, Bai Jamjuree, DM Serif Display, Fraunces
  - Body: Karla, IBM Plex Sans, Work Sans, Manrope
  - Mono: JetBrains Mono, Fira Code, IBM Plex Mono

**Implementation:**
```html
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700;900&family=Karla:wght@300;400;700&display=swap" rel="stylesheet">
```

```css
:root {
  --font-display: 'Playfair Display', serif;
  --font-body: 'Karla', sans-serif;
  --font-mono: 'JetBrains Mono', monospace;
}

h1, h2, h3 {
  font-family: var(--font-display);
  font-weight: 900;
}

body {
  font-family: var(--font-body);
  font-weight: 400;
}
```

---

### 2. Color & Theme

**Avoid:**
- Evenly distributed color palettes
- Timid, safe color choices
- Generic blue/gray combinations

**Instead, Create:**
- **Cohesive aesthetics using CSS variables**
- **Dominant colors with sharp accents**
- **IDE theme inspiration**: Dracula, Nord, Tokyo Night, Monokai
- **Cultural aesthetics**: Vaporwave, Brutalism, Swiss Design, Memphis

**Color Strategy:**
- Choose 1-2 dominant colors (60-70% of palette)
- 1 secondary color (20-30%)
- 1-2 accent colors for emphasis (10%)
- Use semantic naming for clarity

**Implementation:**
```css
:root {
  /* Inspired by Tokyo Night theme */
  --color-bg-primary: #1a1b26;
  --color-bg-secondary: #24283b;
  --color-bg-elevated: #414868;

  --color-text-primary: #c0caf5;
  --color-text-secondary: #a9b1d6;
  --color-text-muted: #565f89;

  --color-accent-primary: #7aa2f7;
  --color-accent-secondary: #bb9af7;
  --color-accent-success: #9ece6a;
  --color-accent-warning: #e0af68;
  --color-accent-error: #f7768e;
}
```

---

### 3. Motion & Animation

**Avoid:**
- No animations (static pages)
- Scattered micro-interactions everywhere
- Heavy JavaScript animation libraries by default

**Instead, Prioritize:**
- **CSS-only solutions** for HTML/CSS projects
- **High-impact moments** over constant motion
- **Staggered reveals** with animation-delay
- **Motion libraries** for React (Framer Motion) or Vue (GSAP)

**CSS Animation Patterns:**
```css
/* Fade-in on load */
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

.hero-title {
  animation: fadeInUp 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
}

.hero-subtitle {
  animation: fadeInUp 0.8s cubic-bezier(0.16, 1, 0.3, 1) 0.2s forwards;
  opacity: 0;
}

/* Smooth hover interactions */
.card {
  transition: transform 0.3s cubic-bezier(0.16, 1, 0.3, 1),
              box-shadow 0.3s cubic-bezier(0.16, 1, 0.3, 1);
}

.card:hover {
  transform: translateY(-4px);
  box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
}
```

**React with Framer Motion:**
```jsx
import { motion } from 'framer-motion';

const containerVariants = {
  hidden: { opacity: 0 },
  show: {
    opacity: 1,
    transition: {
      staggerChildren: 0.1
    }
  }
};

const itemVariants = {
  hidden: { opacity: 0, y: 20 },
  show: { opacity: 1, y: 0 }
};

<motion.div variants={containerVariants} initial="hidden" animate="show">
  <motion.div variants={itemVariants}>Item 1</motion.div>
  <motion.div variants={itemVariants}>Item 2</motion.div>
</motion.div>
```

---

### 4. Backgrounds

**Avoid:**
- Solid color backgrounds only
- No visual depth or texture

**Instead, Create:**
- **Layered CSS gradients** for atmosphere
- **Geometric patterns** for depth
- **Subtle textures** without overwhelming content

**Gradient Patterns:**
```css
/* Ambient gradient background */
.hero {
  background:
    radial-gradient(circle at 20% 50%, rgba(120, 119, 198, 0.3), transparent 50%),
    radial-gradient(circle at 80% 80%, rgba(255, 122, 122, 0.3), transparent 50%),
    radial-gradient(circle at 40% 80%, rgba(138, 180, 248, 0.3), transparent 50%),
    linear-gradient(135deg, #1a1b26 0%, #24283b 100%);
}

/* Geometric pattern overlay */
.pattern-bg {
  background-color: #1a1b26;
  background-image:
    repeating-linear-gradient(45deg, transparent, transparent 35px, rgba(255,255,255,.05) 35px, rgba(255,255,255,.05) 70px);
}

/* Glass morphism effect */
.glass-card {
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.1);
}
```

---

## Prompting Altitude Principle

**Guide creatively, don't hardcode.** Instead of specifying exact hex values, use **target language** that engages the model's critical thinking:

❌ **Too specific:** "Use #7aa2f7 for the primary color"
✅ **Just right:** "Choose a vibrant blue accent that evokes trust and energy, inspired by Tokyo Night theme"

❌ **Too vague:** "Make it look good"
✅ **Just right:** "Create high contrast between background and text, using layered gradients for depth and a distinctive serif font for headings"

---

## Complete Design Checklist

When creating frontend designs, ensure you address:

- [ ] **Typography**: Distinctive fonts loaded from Google Fonts, high contrast pairings
- [ ] **Font Weights**: Extreme variations (100-200 vs 800-900)
- [ ] **Color System**: CSS variables with semantic naming
- [ ] **Color Palette**: Dominant color + accents, inspired by themes or culture
- [ ] **Motion**: Staggered animations on key elements
- [ ] **Easing**: Smooth cubic-bezier curves, not linear
- [ ] **Backgrounds**: Gradients, patterns, or layered effects
- [ ] **Depth**: Use shadows, overlays, blur for visual hierarchy
- [ ] **Consistency**: Design tokens defined in :root
- [ ] **Accessibility**: Color contrast meets WCAG AA (4.5:1 for text)

---

## Example Application: SaaS Landing Page

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Product - Distinctive SaaS</title>
  <link href="https://fonts.googleapis.com/css2?family=Fraunces:wght@300;700;900&family=Manrope:wght@400;600;800&display=swap" rel="stylesheet">
  <style>
    :root {
      /* Tokyo Night inspired palette */
      --bg-primary: #1a1b26;
      --bg-secondary: #24283b;
      --text-primary: #c0caf5;
      --text-muted: #565f89;
      --accent-blue: #7aa2f7;
      --accent-purple: #bb9af7;
      --accent-green: #9ece6a;

      --font-display: 'Fraunces', serif;
      --font-body: 'Manrope', sans-serif;
    }

    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      font-family: var(--font-body);
      background: var(--bg-primary);
      color: var(--text-primary);
      line-height: 1.6;
    }

    .hero {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 2rem;
      background:
        radial-gradient(circle at 20% 50%, rgba(187, 154, 247, 0.15), transparent 50%),
        radial-gradient(circle at 80% 80%, rgba(122, 162, 247, 0.15), transparent 50%),
        var(--bg-primary);
      position: relative;
      overflow: hidden;
    }

    .hero::before {
      content: '';
      position: absolute;
      inset: 0;
      background-image:
        repeating-linear-gradient(45deg, transparent, transparent 50px, rgba(255,255,255,.02) 50px, rgba(255,255,255,.02) 100px);
      pointer-events: none;
    }

    .hero-content {
      max-width: 800px;
      text-align: center;
      position: relative;
      z-index: 1;
    }

    h1 {
      font-family: var(--font-display);
      font-size: clamp(2.5rem, 8vw, 5rem);
      font-weight: 900;
      margin-bottom: 1.5rem;
      line-height: 1.1;
      background: linear-gradient(135deg, var(--accent-blue), var(--accent-purple));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      animation: fadeInUp 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    }

    @keyframes fadeInUp {
      from {
        opacity: 0;
        transform: translateY(30px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    p {
      font-size: 1.25rem;
      color: var(--text-muted);
      margin-bottom: 2rem;
      animation: fadeInUp 0.8s cubic-bezier(0.16, 1, 0.3, 1) 0.2s forwards;
      opacity: 0;
    }

    .cta {
      display: inline-block;
      padding: 1rem 2.5rem;
      background: var(--accent-blue);
      color: var(--bg-primary);
      text-decoration: none;
      font-weight: 800;
      border-radius: 8px;
      transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1);
      animation: fadeInUp 0.8s cubic-bezier(0.16, 1, 0.3, 1) 0.4s forwards;
      opacity: 0;
    }

    .cta:hover {
      transform: translateY(-2px);
      box-shadow: 0 10px 30px rgba(122, 162, 247, 0.4);
    }
  </style>
</head>
<body>
  <section class="hero">
    <div class="hero-content">
      <h1>Build Something Extraordinary</h1>
      <p>The modern platform for teams who refuse to settle for ordinary</p>
      <a href="#" class="cta">Start Building</a>
    </div>
  </section>
</body>
</html>
```

---

## Usage Guidelines

1. **Always load custom fonts** from Google Fonts or font CDN
2. **Define CSS variables** in `:root` for consistency
3. **Use semantic color naming** (--color-accent-primary, not --blue)
4. **Implement motion** on key moments (hero entrance, card hovers)
5. **Create depth** with gradients, shadows, blur effects
6. **Test contrast** using browser DevTools or WebAIM checker
7. **Avoid defaults** mentioned in the "Avoid" sections

---

## Advanced: Component Library Integration

For React/Vue/Angular projects using component libraries:

### Tailwind CSS Custom Theme
```js
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      fontFamily: {
        display: ['Fraunces', 'serif'],
        body: ['Manrope', 'sans-serif'],
      },
      colors: {
        primary: {
          50: '#e7e9f5',
          100: '#c0caf5',
          500: '#7aa2f7',
          900: '#1a1b26',
        },
        accent: {
          purple: '#bb9af7',
          green: '#9ece6a',
        }
      },
      animation: {
        'fade-in-up': 'fadeInUp 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards',
      },
      keyframes: {
        fadeInUp: {
          '0%': { opacity: '0', transform: 'translateY(20px)' },
          '100%': { opacity: '1', transform: 'translateY(0)' },
        }
      }
    }
  }
}
```

### Shadcn/UI Custom Theming
```css
/* app/globals.css */
@layer base {
  :root {
    --background: 210 40% 8%;
    --foreground: 230 30% 80%;
    --primary: 217 91% 60%;
    --primary-foreground: 222 47% 11%;
    --accent: 266 85% 76%;
    --accent-foreground: 222 47% 11%;

    --font-display: 'Fraunces', serif;
    --font-body: 'Manrope', sans-serif;
  }
}
```

---

## Remember

The goal is **distinctive, not generic**. Every design choice should be intentional and move away from the high-probability center of AI-generated outputs. Think critically about typography, color, motion, and backgrounds for each project.
