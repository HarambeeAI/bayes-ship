---
description: "WCAG accessibility patterns: ARIA labels, keyboard navigation, screen reader support, color contrast, focus management. Use during QA phase for any product with a UI. Source: everything-claude-code."
---

# Accessibility

## Core requirements (WCAG 2.1 AA)

### Semantic HTML
- Use `<button>` for actions, `<a>` for navigation. Never use `<div onClick>`.
- Use heading hierarchy (`h1` > `h2` > `h3`). Never skip levels.
- Use `<nav>`, `<main>`, `<aside>`, `<footer>` landmarks.

### Keyboard navigation
- All interactive elements must be reachable via Tab.
- Visible focus indicators on all focusable elements (never `outline: none` without replacement).
- Escape closes modals and dropdowns.
- Enter/Space activates buttons and links.

### ARIA
- `aria-label` for icon-only buttons: `<button aria-label="Close">`
- `aria-expanded` for collapsible sections
- `aria-live="polite"` for dynamic content updates (toasts, status messages)
- `role="alert"` for error messages

### Color and contrast
- Minimum 4.5:1 contrast ratio for normal text
- Minimum 3:1 for large text (18px+ or 14px+ bold)
- Never use color alone to convey information (add icons or text labels)

### Forms
- Every input has a visible `<label>` (not just placeholder text)
- Error messages linked to inputs via `aria-describedby`
- Required fields marked with `aria-required="true"`

### Images
- All `<img>` tags have `alt` text
- Decorative images use `alt=""`
- Complex images (charts, diagrams) have detailed descriptions

## Testing

- Tab through the entire page. Can you reach and operate everything?
- Use a screen reader (VoiceOver on Mac, NVDA on Windows) on key flows
- Check contrast with browser devtools
- Verify all forms work without a mouse
