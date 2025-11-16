---
name: component-library-builder
description: Generate production-ready React, Vue, or Angular components with TypeScript, accessibility, testing, and documentation
---

# Component Library Builder Skill

## Purpose

Build reusable, production-ready UI components following modern best practices for React, Vue, or Angular applications. This skill ensures components are accessible, type-safe, testable, and well-documented.

---

## Component Architecture Principles

### 1. Composition Over Configuration
- Small, focused components with single responsibility
- Compose complex UIs from simple building blocks
- Use slots/children for flexible content injection

### 2. Type Safety
- TypeScript for all components
- Strict prop/event type definitions
- Generic components where appropriate

### 3. Accessibility First
- ARIA attributes for semantic meaning
- Keyboard navigation support
- Screen reader compatibility
- Focus management

### 4. Testing
- Unit tests for logic
- Component tests for rendering
- Accessibility tests with jest-axe or similar

---

## React Component Pattern

### Component Structure
```
ComponentName/
├── ComponentName.tsx           # Main component
├── ComponentName.types.ts      # TypeScript types
├── ComponentName.module.css    # Scoped styles
├── ComponentName.test.tsx      # Tests
├── ComponentName.stories.tsx   # Storybook stories
└── index.ts                    # Public exports
```

### Base Template
```tsx
// Button.tsx
import React, { forwardRef } from 'react';
import { ButtonProps } from './Button.types';
import styles from './Button.module.css';

export const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  (
    {
      children,
      variant = 'primary',
      size = 'medium',
      disabled = false,
      isLoading = false,
      leftIcon,
      rightIcon,
      className,
      ...rest
    },
    ref
  ) => {
    const buttonClasses = [
      styles.button,
      styles[variant],
      styles[size],
      isLoading && styles.loading,
      className,
    ]
      .filter(Boolean)
      .join(' ');

    return (
      <button
        ref={ref}
        className={buttonClasses}
        disabled={disabled || isLoading}
        aria-busy={isLoading}
        {...rest}
      >
        {leftIcon && <span className={styles.icon}>{leftIcon}</span>}
        <span className={styles.content}>{children}</span>
        {rightIcon && <span className={styles.icon}>{rightIcon}</span>}
        {isLoading && (
          <span className={styles.spinner} role="status" aria-label="Loading">
            <LoadingSpinner />
          </span>
        )}
      </button>
    );
  }
);

Button.displayName = 'Button';
```

### TypeScript Types
```typescript
// Button.types.ts
import { ButtonHTMLAttributes, ReactNode } from 'react';

export type ButtonVariant = 'primary' | 'secondary' | 'outline' | 'ghost' | 'danger';
export type ButtonSize = 'small' | 'medium' | 'large';

export interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  /** Button content */
  children: ReactNode;
  /** Visual style variant */
  variant?: ButtonVariant;
  /** Size of the button */
  size?: ButtonSize;
  /** Loading state */
  isLoading?: boolean;
  /** Icon to display on the left */
  leftIcon?: ReactNode;
  /** Icon to display on the right */
  rightIcon?: ReactNode;
  /** Additional CSS class */
  className?: string;
}
```

### Component Tests
```tsx
// Button.test.tsx
import { render, screen, fireEvent } from '@testing-library/react';
import { axe, toHaveNoViolations } from 'jest-axe';
import { Button } from './Button';

expect.extend(toHaveNoViolations);

describe('Button', () => {
  it('renders children correctly', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });

  it('handles click events', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    fireEvent.click(screen.getByText('Click me'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  it('applies variant classes', () => {
    const { container } = render(<Button variant="secondary">Click me</Button>);
    expect(container.firstChild).toHaveClass('secondary');
  });

  it('disables button when disabled prop is true', () => {
    render(<Button disabled>Click me</Button>);
    expect(screen.getByText('Click me')).toBeDisabled();
  });

  it('shows loading state', () => {
    render(<Button isLoading>Click me</Button>);
    expect(screen.getByRole('status')).toHaveAttribute('aria-label', 'Loading');
  });

  it('has no accessibility violations', async () => {
    const { container } = render(<Button>Click me</Button>);
    const results = await axe(container);
    expect(results).toHaveNoViolations();
  });
});
```

### Storybook Documentation
```tsx
// Button.stories.tsx
import type { Meta, StoryObj } from '@storybook/react';
import { Button } from './Button';
import { ArrowRight, Plus } from 'lucide-react';

const meta: Meta<typeof Button> = {
  title: 'Components/Button',
  component: Button,
  parameters: {
    layout: 'centered',
  },
  tags: ['autodocs'],
  argTypes: {
    variant: {
      control: 'select',
      options: ['primary', 'secondary', 'outline', 'ghost', 'danger'],
    },
    size: {
      control: 'select',
      options: ['small', 'medium', 'large'],
    },
  },
};

export default meta;
type Story = StoryObj<typeof Button>;

export const Primary: Story = {
  args: {
    variant: 'primary',
    children: 'Button',
  },
};

export const WithIcons: Story = {
  args: {
    children: 'Continue',
    rightIcon: <ArrowRight size={16} />,
  },
};

export const Loading: Story = {
  args: {
    children: 'Loading',
    isLoading: true,
  },
};

export const AllVariants: Story = {
  render: () => (
    <div style={{ display: 'flex', gap: '1rem' }}>
      <Button variant="primary">Primary</Button>
      <Button variant="secondary">Secondary</Button>
      <Button variant="outline">Outline</Button>
      <Button variant="ghost">Ghost</Button>
      <Button variant="danger">Danger</Button>
    </div>
  ),
};
```

---

## Vue 3 Component Pattern (Composition API)

### Component Template
```vue
<!-- Button.vue -->
<template>
  <button
    :class="buttonClasses"
    :disabled="disabled || isLoading"
    :aria-busy="isLoading"
    v-bind="$attrs"
  >
    <span v-if="$slots.leftIcon" class="button__icon">
      <slot name="leftIcon" />
    </span>
    <span class="button__content">
      <slot />
    </span>
    <span v-if="$slots.rightIcon" class="button__icon">
      <slot name="rightIcon" />
    </span>
    <span v-if="isLoading" class="button__spinner" role="status" aria-label="Loading">
      <LoadingSpinner />
    </span>
  </button>
</template>

<script setup lang="ts">
import { computed } from 'vue';
import LoadingSpinner from './LoadingSpinner.vue';

export interface ButtonProps {
  variant?: 'primary' | 'secondary' | 'outline' | 'ghost' | 'danger';
  size?: 'small' | 'medium' | 'large';
  disabled?: boolean;
  isLoading?: boolean;
}

const props = withDefaults(defineProps<ButtonProps>(), {
  variant: 'primary',
  size: 'medium',
  disabled: false,
  isLoading: false,
});

const buttonClasses = computed(() => [
  'button',
  `button--${props.variant}`,
  `button--${props.size}`,
  { 'button--loading': props.isLoading },
]);
</script>

<style scoped>
/* Component styles */
</style>
```

### Vue Component Tests
```typescript
// Button.spec.ts
import { mount } from '@vue/test-utils';
import { describe, it, expect, vi } from 'vitest';
import Button from './Button.vue';

describe('Button', () => {
  it('renders slot content', () => {
    const wrapper = mount(Button, {
      slots: {
        default: 'Click me',
      },
    });
    expect(wrapper.text()).toContain('Click me');
  });

  it('emits click event', async () => {
    const wrapper = mount(Button);
    await wrapper.trigger('click');
    expect(wrapper.emitted('click')).toHaveLength(1);
  });

  it('applies variant prop', () => {
    const wrapper = mount(Button, {
      props: { variant: 'secondary' },
    });
    expect(wrapper.classes()).toContain('button--secondary');
  });

  it('disables when disabled prop is true', () => {
    const wrapper = mount(Button, {
      props: { disabled: true },
    });
    expect(wrapper.attributes('disabled')).toBeDefined();
  });
});
```

---

## Angular Component Pattern

### Component TypeScript
```typescript
// button.component.ts
import { Component, Input, Output, EventEmitter, ChangeDetectionStrategy } from '@angular/core';

type ButtonVariant = 'primary' | 'secondary' | 'outline' | 'ghost' | 'danger';
type ButtonSize = 'small' | 'medium' | 'large';

@Component({
  selector: 'app-button',
  templateUrl: './button.component.html',
  styleUrls: ['./button.component.scss'],
  changeDetection: ChangeDetectionStrategy.OnPush,
})
export class ButtonComponent {
  @Input() variant: ButtonVariant = 'primary';
  @Input() size: ButtonSize = 'medium';
  @Input() disabled = false;
  @Input() isLoading = false;
  @Input() type: 'button' | 'submit' | 'reset' = 'button';

  @Output() clicked = new EventEmitter<MouseEvent>();

  get buttonClasses(): string[] {
    return [
      'button',
      `button--${this.variant}`,
      `button--${this.size}`,
      this.isLoading ? 'button--loading' : '',
    ].filter(Boolean);
  }

  handleClick(event: MouseEvent): void {
    if (!this.disabled && !this.isLoading) {
      this.clicked.emit(event);
    }
  }
}
```

### Component Template
```html
<!-- button.component.html -->
<button
  [ngClass]="buttonClasses"
  [disabled]="disabled || isLoading"
  [attr.aria-busy]="isLoading"
  [type]="type"
  (click)="handleClick($event)"
>
  <span class="button__icon" *ngIf="leftIcon">
    <ng-content select="[leftIcon]"></ng-content>
  </span>
  <span class="button__content">
    <ng-content></ng-content>
  </span>
  <span class="button__icon" *ngIf="rightIcon">
    <ng-content select="[rightIcon]"></ng-content>
  </span>
  <span *ngIf="isLoading" class="button__spinner" role="status" aria-label="Loading">
    <app-loading-spinner></app-loading-spinner>
  </span>
</button>
```

---

## Common Component Patterns

### 1. Form Controls
- Input, Textarea, Select, Checkbox, Radio
- Label association with htmlFor/for
- Error states and validation messages
- Required/optional indicators

### 2. Data Display
- Card, Table, List, Badge, Avatar
- Responsive layouts
- Loading skeletons
- Empty states

### 3. Feedback
- Alert, Toast, Modal, Tooltip
- Focus trap for modals
- Portal rendering for overlays
- Dismiss actions

### 4. Navigation
- Tabs, Breadcrumbs, Pagination, Menu
- Active state indication
- Keyboard navigation (arrow keys, Enter, Escape)
- Route integration

---

## Accessibility Checklist

- [ ] Semantic HTML elements (button, not div with onClick)
- [ ] ARIA labels for icon-only buttons
- [ ] ARIA live regions for dynamic content
- [ ] Keyboard navigation support
- [ ] Focus visible styles
- [ ] Color contrast meets WCAG AA (4.5:1)
- [ ] Screen reader tested with NVDA/JAWS
- [ ] Form controls have associated labels
- [ ] Error messages announced to screen readers
- [ ] Modal focus trap with Escape to close

---

## Component Documentation Template

```markdown
# ComponentName

Brief description of what this component does.

## Usage

\`\`\`tsx
import { ComponentName } from './ComponentName';

<ComponentName prop="value">Content</ComponentName>
\`\`\`

## Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| variant | 'primary' \| 'secondary' | 'primary' | Visual style |
| size | 'small' \| 'medium' \| 'large' | 'medium' | Component size |
| disabled | boolean | false | Disables interaction |

## Accessibility

- Uses semantic `<button>` element
- Supports keyboard navigation
- ARIA attributes for loading state
- Focus visible styles included

## Examples

[Link to Storybook examples]
```

---

## Best Practices

1. **Keep components small** - If it's doing too much, split it
2. **Props over state** - Controlled components are more flexible
3. **Avoid prop drilling** - Use context/provide-inject for deep hierarchies
4. **Name props semantically** - `isLoading` not `loading`, `onClose` not `close`
5. **Export types** - Make TypeScript types available to consumers
6. **Document edge cases** - What happens with empty content? Long text?
7. **Test user interactions** - Not just rendering, but clicks, keyboard, focus
8. **Version your library** - Use semantic versioning for changes

---

## Integration with Design Systems

### CSS-in-JS (styled-components)
```tsx
import styled from 'styled-components';

const StyledButton = styled.button<{ $variant: ButtonVariant }>`
  padding: ${({ theme }) => theme.spacing.md};
  background: ${({ theme, $variant }) => theme.colors[$variant]};
  border-radius: ${({ theme }) => theme.radii.md};
  transition: all 0.2s ease;

  &:hover {
    transform: translateY(-2px);
  }
`;
```

### CSS Modules
```typescript
// Use CSS Modules for scoped styles
import styles from './Button.module.css';
```

### Tailwind CSS
```tsx
const buttonVariants = {
  primary: 'bg-blue-600 hover:bg-blue-700 text-white',
  secondary: 'bg-gray-200 hover:bg-gray-300 text-gray-900',
  outline: 'border-2 border-blue-600 text-blue-600 hover:bg-blue-50',
};

<button className={`px-4 py-2 rounded ${buttonVariants[variant]}`}>
  {children}
</button>
```

---

## Tools & Libraries

**Testing:**
- Jest + React Testing Library
- Vitest + Vue Test Utils
- Jasmine + Karma (Angular)
- jest-axe for accessibility testing

**Documentation:**
- Storybook for component showcase
- React Docgen / Vue Docgen for auto-docs
- Compodoc for Angular documentation

**Build:**
- Rollup or Vite for library bundling
- tsup for simple TypeScript builds
- Turborepo for monorepo management

**Linting:**
- ESLint with react/vue/angular plugins
- Prettier for formatting
- Stylelint for CSS

---

## When to Use This Skill

- Creating reusable UI components
- Building a design system or component library
- Ensuring accessibility compliance
- Setting up component testing
- Documenting components for team use
- Migrating from one framework to another (pattern comparison)
