---
allowed-tools: Read(*),Grep(*),Glob(*)
description: Perform comprehensive accessibility audit against WCAG 2.1 guidelines with remediation steps
---

# Accessibility Audit Command

Perform comprehensive accessibility (a11y) audit of the application.

**Audit Checklist**:

1. **Semantic HTML**:
   - Proper heading hierarchy (h1-h6)
   - Semantic elements (nav, main, article, etc.)
   - Button vs div with onClick
   - Form labels associated with inputs

2. **Keyboard Navigation**:
   - All interactive elements focusable
   - Logical tab order
   - Visible focus indicators
   - No keyboard traps

3. **Screen Reader Support**:
   - ARIA labels on icon-only buttons
   - ARIA live regions for dynamic content
   - ARIA landmarks
   - Alternative text for images

4. **Color Contrast**:
   - WCAG AA compliance (4.5:1 for normal text)
   - WCAG AAA compliance (7:1) where possible
   - Don't rely solely on color

5. **Forms**:
   - Labels for all inputs
   - Error messages announced
   - Required fields indicated
   - Validation feedback

6. **Touch Targets**:
   - Minimum 44x44px on mobile
   - Adequate spacing between targets

Use the `web-accessibility-auditor` skill.

**Automated Tools**:
- axe-core for automated checks
- Lighthouse accessibility score
- WAVE evaluation

**Manual Testing**:
- Keyboard-only navigation
- Screen reader testing (NVDA/VoiceOver)
- Color contrast checking
- Zoom to 200% testing

**Output Format**:
```markdown
# Accessibility Audit Report

## Summary
- WCAG Level: AA
- Compliance Score: 78%
- Critical Issues: 5
- Warnings: 12

## Critical Issues (WCAG Failures)

### 1. Missing Form Labels
**Impact**: High - Screen readers can't identify inputs
**Location**: login.tsx:45, signup.tsx:67
**WCAG**: 3.3.2 Labels or Instructions (Level A)
**Fix**:
```html
<!-- Before -->
<input type="email" placeholder="Email" />

<!-- After -->
<label htmlFor="email">Email</label>
<input id="email" type="email" />
```

### 2. Insufficient Color Contrast
**Impact**: High - Users with low vision can't read text
**Location**: button-primary.css:12
**WCAG**: 1.4.3 Contrast (Level AA)
**Current**: 3.2:1
**Required**: 4.5:1
**Fix**: Change text color from #999 to #595959

## Recommendations
1. Fix all critical issues immediately
2. Test with real screen readers
3. Add automated a11y tests (jest-axe)
4. Train team on accessibility
```
