---
description: "UI audit and upgrade skill. Analyzes existing interfaces for design problems (spacing, contrast, hierarchy, motion, consistency) and provides structured fixes. Use during QA phase or when iterating on existing products. Source: Leonxlnx/taste-skill."
---

# Redesign Skill — UI Audit and Upgrade

## When to use

When reviewing an existing UI for design quality. Used by the QA phase (ship-test) or manually when iterating on a product's visual quality.

## Audit checklist

### 1. Spacing audit
- Is spacing consistent? (same gaps between similar elements)
- Is there a spacing scale being followed? (4/8/16/24/32 or similar)
- Are there cramped areas? (elements too close together)
- Are there orphaned elements? (floating with too much space around them)

### 2. Color audit
- Is there a defined palette? (or are colors ad-hoc?)
- Contrast ratio meets WCAG AA? (4.5:1 for text, 3:1 for large text)
- Is color used consistently? (same color = same meaning everywhere)
- Too many colors? (more than 5-7 indicates palette bloat)

### 3. Typography audit
- Is there a type scale? (consistent sizes, not random px values)
- Is hierarchy clear? (can you tell headings from body from captions at a glance?)
- Line length appropriate? (45-75 characters for body text)
- Line height appropriate? (1.4-1.6 for body)

### 4. Layout audit
- Is there a grid? (or are elements positioned arbitrarily?)
- Is alignment consistent? (all left-aligned, or intentionally mixed?)
- Does the layout work on mobile? (responsive breakpoints)
- Are cards/containers consistent in padding and radius?

### 5. Motion audit
- Are animations meaningful? (convey state change, not just decoration)
- Are durations consistent? (same type of animation = same timing)
- Is prefers-reduced-motion respected?
- Any janky animations? (layout thrashing, jumpy transitions)

### 6. Consistency audit
- Same element styled the same way everywhere? (buttons, inputs, cards)
- Same interaction pattern everywhere? (click, hover, focus states)
- Component reuse? (or are there one-off implementations of the same pattern?)

## Output format

For each issue found, report:
- **Location**: Which page/component
- **Problem**: What's wrong (with screenshot or description)
- **Fix**: Specific CSS/JSX change to make
- **Severity**: Critical (breaks usability) / Warning (looks unprofessional) / Note (polish opportunity)
