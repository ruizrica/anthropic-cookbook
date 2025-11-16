---
name: design-system-architect
description: Build scalable design systems with design tokens, component patterns, theming, and documentation
---

# Design System Architect Skill

## Purpose

Create comprehensive, scalable design systems that ensure consistency across products, enable theming, and provide clear guidelines for designers and developers.

---

## Design System Structure

```
design-system/
├── tokens/                    # Design tokens (colors, spacing, typography)
│   ├── colors.json
│   ├── spacing.json
│   ├── typography.json
│   ├── shadows.json
│   └── ...
├── components/                # UI components
│   ├── Button/
│   ├── Input/
│   ├── Card/
│   └── ...
├── patterns/                  # Composition patterns
│   ├── forms/
│   ├── layouts/
│   └── navigation/
├── foundations/               # Core styles
│   ├── reset.css
│   ├── variables.css
│   └── utilities.css
├── docs/                      # Documentation
│   ├── getting-started.md
│   ├── principles.md
│   └── components/
└── tooling/                   # Build tools
    ├── token-transformer.js
    └── component-generator.js
```

---

## Design Tokens

Design tokens are the **visual design atoms** of your system: colors, typography, spacing, etc.

### Color Tokens
```json
{
  "color": {
    "brand": {
      "primary": {
        "50": { "value": "#eff6ff" },
        "100": { "value": "#dbeafe" },
        "500": { "value": "#3b82f6" },
        "900": { "value": "#1e3a8a" }
      }
    },
    "semantic": {
      "success": { "value": "{color.green.500}" },
      "error": { "value": "{color.red.500}" },
      "warning": { "value": "{color.amber.500}" },
      "info": { "value": "{color.blue.500}" }
    },
    "text": {
      "primary": { "value": "{color.gray.900}" },
      "secondary": { "value": "{color.gray.600}" },
      "muted": { "value": "{color.gray.400}" }
    },
    "background": {
      "primary": { "value": "#ffffff" },
      "secondary": { "value": "{color.gray.50}" },
      "elevated": { "value": "#ffffff" }
    }
  }
}
```

### Spacing Tokens
```json
{
  "spacing": {
    "0": { "value": "0" },
    "1": { "value": "0.25rem" },
    "2": { "value": "0.5rem" },
    "3": { "value": "0.75rem" },
    "4": { "value": "1rem" },
    "6": { "value": "1.5rem" },
    "8": { "value": "2rem" },
    "12": { "value": "3rem" },
    "16": { "value": "4rem" }
  }
}
```

### Typography Tokens
```json
{
  "typography": {
    "fontFamily": {
      "display": { "value": "'Fraunces', serif" },
      "body": { "value": "'Inter', sans-serif" },
      "mono": { "value": "'JetBrains Mono', monospace" }
    },
    "fontSize": {
      "xs": { "value": "0.75rem" },
      "sm": { "value": "0.875rem" },
      "base": { "value": "1rem" },
      "lg": { "value": "1.125rem" },
      "xl": { "value": "1.25rem" },
      "2xl": { "value": "1.5rem" },
      "3xl": { "value": "1.875rem" },
      "4xl": { "value": "2.25rem" }
    },
    "fontWeight": {
      "normal": { "value": "400" },
      "medium": { "value": "500" },
      "semibold": { "value": "600" },
      "bold": { "value": "700" },
      "extrabold": { "value": "800" }
    },
    "lineHeight": {
      "tight": { "value": "1.25" },
      "normal": { "value": "1.5" },
      "relaxed": { "value": "1.75" }
    }
  }
}
```

### Shadow Tokens
```json
{
  "shadow": {
    "sm": { "value": "0 1px 2px 0 rgba(0, 0, 0, 0.05)" },
    "md": { "value": "0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06)" },
    "lg": { "value": "0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05)" },
    "xl": { "value": "0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04)" }
  }
}
```

---

## CSS Variables from Tokens

### Generate CSS Custom Properties
```css
:root {
  /* Colors */
  --color-primary-50: #eff6ff;
  --color-primary-500: #3b82f6;
  --color-primary-900: #1e3a8a;

  --color-success: var(--color-green-500);
  --color-error: var(--color-red-500);

  --color-text-primary: #111827;
  --color-text-secondary: #6b7280;

  --color-bg-primary: #ffffff;
  --color-bg-secondary: #f9fafb;

  /* Spacing */
  --spacing-1: 0.25rem;
  --spacing-2: 0.5rem;
  --spacing-4: 1rem;
  --spacing-8: 2rem;

  /* Typography */
  --font-display: 'Fraunces', serif;
  --font-body: 'Inter', sans-serif;
  --font-mono: 'JetBrains Mono', monospace;

  --font-size-sm: 0.875rem;
  --font-size-base: 1rem;
  --font-size-lg: 1.125rem;
  --font-size-xl: 1.25rem;

  --font-weight-normal: 400;
  --font-weight-bold: 700;

  --line-height-tight: 1.25;
  --line-height-normal: 1.5;

  /* Shadows */
  --shadow-sm: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  --shadow-lg: 0 10px 15px -3px rgba(0, 0, 0, 0.1);

  /* Border Radius */
  --radius-sm: 0.25rem;
  --radius-md: 0.375rem;
  --radius-lg: 0.5rem;
  --radius-full: 9999px;

  /* Transitions */
  --transition-fast: 150ms cubic-bezier(0.16, 1, 0.3, 1);
  --transition-normal: 300ms cubic-bezier(0.16, 1, 0.3, 1);
  --transition-slow: 500ms cubic-bezier(0.16, 1, 0.3, 1);
}
```

---

## Theming System

### Light/Dark Mode
```css
:root {
  --color-bg-primary: #ffffff;
  --color-bg-secondary: #f9fafb;
  --color-text-primary: #111827;
  --color-text-secondary: #6b7280;
}

[data-theme="dark"] {
  --color-bg-primary: #1a1b26;
  --color-bg-secondary: #24283b;
  --color-text-primary: #c0caf5;
  --color-text-secondary: #a9b1d6;
}

/* Respect system preferences */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) {
    --color-bg-primary: #1a1b26;
    --color-text-primary: #c0caf5;
  }
}
```

### Multi-Brand Theming
```css
/* Default brand */
:root {
  --color-brand-primary: #3b82f6;
  --color-brand-secondary: #8b5cf6;
}

/* Brand A */
[data-brand="brand-a"] {
  --color-brand-primary: #ef4444;
  --color-brand-secondary: #f97316;
}

/* Brand B */
[data-brand="brand-b"] {
  --color-brand-primary: #10b981;
  --color-brand-secondary: #14b8a6;
}
```

### Theme Switcher (JavaScript)
```typescript
// theme.ts
type Theme = 'light' | 'dark' | 'auto';

class ThemeManager {
  private theme: Theme;

  constructor() {
    this.theme = this.getStoredTheme() || 'auto';
    this.apply();
  }

  setTheme(theme: Theme) {
    this.theme = theme;
    localStorage.setItem('theme', theme);
    this.apply();
  }

  private apply() {
    const effectiveTheme = this.theme === 'auto'
      ? this.getSystemTheme()
      : this.theme;

    document.documentElement.setAttribute('data-theme', effectiveTheme);
  }

  private getSystemTheme(): 'light' | 'dark' {
    return window.matchMedia('(prefers-color-scheme: dark)').matches
      ? 'dark'
      : 'light';
  }

  private getStoredTheme(): Theme | null {
    return localStorage.getItem('theme') as Theme | null;
  }
}

export const themeManager = new ThemeManager();
```

---

## Component API Patterns

### Consistent Prop Naming
```typescript
// Size variants
type Size = 'sm' | 'md' | 'lg' | 'xl';

// Visual variants
type Variant = 'primary' | 'secondary' | 'outline' | 'ghost' | 'danger';

// State props
interface ComponentProps {
  disabled?: boolean;
  isLoading?: boolean;
  isActive?: boolean;
  isSelected?: boolean;
}

// Composition props
interface CompositionProps {
  leftIcon?: ReactNode;
  rightIcon?: ReactNode;
  prefix?: ReactNode;
  suffix?: ReactNode;
}
```

### Polymorphic Components
```typescript
// Component that can render as different elements
type AsProp<C extends React.ElementType> = {
  as?: C;
};

type PropsToOmit<C extends React.ElementType, P> = keyof (AsProp<C> & P);

type PolymorphicComponentProp<
  C extends React.ElementType,
  Props = {}
> = React.PropsWithChildren<Props & AsProp<C>> &
  Omit<React.ComponentPropsWithoutRef<C>, PropsToOmit<C, Props>>;

// Usage
function Box<C extends React.ElementType = 'div'>({
  as,
  children,
  ...props
}: PolymorphicComponentProp<C>) {
  const Component = as || 'div';
  return <Component {...props}>{children}</Component>;
}

// Can be used as:
<Box>Div</Box>
<Box as="button">Button</Box>
<Box as={Link} to="/home">Link</Box>
```

---

## Typography System

### Type Scale
```css
.text-xs { font-size: var(--font-size-xs); line-height: 1.25; }
.text-sm { font-size: var(--font-size-sm); line-height: 1.5; }
.text-base { font-size: var(--font-size-base); line-height: 1.5; }
.text-lg { font-size: var(--font-size-lg); line-height: 1.75; }
.text-xl { font-size: var(--font-size-xl); line-height: 1.75; }
.text-2xl { font-size: var(--font-size-2xl); line-height: 1.5; }
.text-3xl { font-size: var(--font-size-3xl); line-height: 1.33; }
.text-4xl { font-size: var(--font-size-4xl); line-height: 1.25; }
```

### Heading Styles
```css
h1, .heading-1 {
  font-family: var(--font-display);
  font-size: var(--font-size-4xl);
  font-weight: var(--font-weight-bold);
  line-height: var(--line-height-tight);
  letter-spacing: -0.02em;
}

h2, .heading-2 {
  font-family: var(--font-display);
  font-size: var(--font-size-3xl);
  font-weight: var(--font-weight-bold);
  line-height: var(--line-height-tight);
}

/* Body text */
body {
  font-family: var(--font-body);
  font-size: var(--font-size-base);
  line-height: var(--line-height-normal);
  color: var(--color-text-primary);
}

.prose {
  max-width: 65ch;
  color: var(--color-text-primary);
}

.prose p {
  margin-bottom: 1.25em;
}

.prose strong {
  font-weight: var(--font-weight-semibold);
  color: var(--color-text-primary);
}
```

---

## Layout Primitives

### Stack (Vertical Spacing)
```tsx
// Stack.tsx
interface StackProps {
  gap?: keyof typeof spacing;
  children: ReactNode;
}

const spacing = {
  sm: 'var(--spacing-2)',
  md: 'var(--spacing-4)',
  lg: 'var(--spacing-8)',
};

export function Stack({ gap = 'md', children }: StackProps) {
  return (
    <div style={{ display: 'flex', flexDirection: 'column', gap: spacing[gap] }}>
      {children}
    </div>
  );
}
```

### Inline (Horizontal Spacing)
```tsx
export function Inline({ gap = 'md', children }: StackProps) {
  return (
    <div style={{ display: 'flex', gap: spacing[gap], flexWrap: 'wrap' }}>
      {children}
    </div>
  );
}
```

### Container (Max Width)
```tsx
interface ContainerProps {
  size?: 'sm' | 'md' | 'lg' | 'xl' | 'full';
  children: ReactNode;
}

const sizes = {
  sm: '640px',
  md: '768px',
  lg: '1024px',
  xl: '1280px',
  full: '100%',
};

export function Container({ size = 'lg', children }: ContainerProps) {
  return (
    <div style={{ maxWidth: sizes[size], margin: '0 auto', padding: '0 1rem' }}>
      {children}
    </div>
  );
}
```

---

## Documentation Structure

### Component Documentation Template
```markdown
# Button

Primary action trigger for user interactions.

## Usage

\`\`\`tsx
import { Button } from '@design-system/components';

<Button variant="primary">Click me</Button>
\`\`\`

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | 'primary' \| 'secondary' \| 'outline' \| 'ghost' \| 'danger' | 'primary' | Visual style variant |
| size | 'sm' \| 'md' \| 'lg' | 'md' | Button size |
| isLoading | boolean | false | Shows loading spinner |
| disabled | boolean | false | Disables interaction |
| leftIcon | ReactNode | - | Icon before text |
| rightIcon | ReactNode | - | Icon after text |

## Variants

### Primary
Use for the main call-to-action on a page.

### Secondary
Use for secondary actions that complement primary actions.

### Outline
Use for less prominent actions.

### Ghost
Use for tertiary actions or inline with text.

### Danger
Use for destructive actions (delete, remove, etc.).

## Accessibility

- Uses semantic `<button>` element
- Supports keyboard navigation (Enter/Space)
- Loading state announced via `aria-busy`
- Disabled state prevents interaction and announces via `aria-disabled`

## Best Practices

✅ **Do:**
- Use primary variant sparingly (1-2 per page)
- Provide clear, action-oriented labels
- Use icons to reinforce meaning
- Disable buttons during async operations

❌ **Don't:**
- Use multiple primary buttons in the same section
- Use vague labels like "Click here" or "Submit"
- Rely solely on color to convey meaning
- Forget to handle loading states

## Examples

[Interactive Storybook examples]
```

---

## Tools & Workflows

### Token Transformation
```javascript
// token-transformer.js
// Convert design tokens JSON to CSS variables

const tokens = require('./tokens.json');

function transformTokens(obj, prefix = '') {
  let css = '';

  for (const [key, value] of Object.entries(obj)) {
    const cssVar = `${prefix}-${key}`;

    if (value.value) {
      css += `  --${cssVar}: ${value.value};\n`;
    } else {
      css += transformTokens(value, cssVar);
    }
  }

  return css;
}

const cssVariables = `:root {\n${transformTokens(tokens)}}`;
console.log(cssVariables);
```

### Storybook Configuration
```typescript
// .storybook/preview.ts
import { Preview } from '@storybook/react';
import '../src/foundations/variables.css';
import '../src/foundations/reset.css';

const preview: Preview = {
  parameters: {
    controls: {
      matchers: {
        color: /(background|color)$/i,
        date: /Date$/,
      },
    },
  },
  globalTypes: {
    theme: {
      description: 'Global theme for components',
      defaultValue: 'light',
      toolbar: {
        title: 'Theme',
        icon: 'circlehollow',
        items: ['light', 'dark'],
        dynamicTitle: true,
      },
    },
  },
};

export default preview;
```

---

## Design System Principles

1. **Consistency**: Use tokens and components uniformly across products
2. **Flexibility**: Support theming and customization
3. **Accessibility**: WCAG AA compliance minimum
4. **Performance**: Optimize bundle size, lazy load when possible
5. **Documentation**: Clear examples and guidelines
6. **Versioning**: Semantic versioning for breaking changes
7. **Composability**: Small primitives compose into complex UIs
8. **Developer Experience**: TypeScript types, autocomplete, clear APIs

---

## Checklist

- [ ] Design tokens defined (colors, spacing, typography, shadows)
- [ ] CSS variables generated from tokens
- [ ] Light/dark theme support
- [ ] Component API consistency (variant, size, state props)
- [ ] Layout primitives (Stack, Inline, Container)
- [ ] Typography system with type scale
- [ ] Documentation for each component
- [ ] Storybook with interactive examples
- [ ] Accessibility testing (jest-axe)
- [ ] TypeScript types exported
- [ ] Versioning strategy
- [ ] Migration guides for breaking changes

---

## Remember

A design system is a **product** that serves designers and developers. Treat it with the same care as user-facing products: gather feedback, iterate, and maintain quality.
