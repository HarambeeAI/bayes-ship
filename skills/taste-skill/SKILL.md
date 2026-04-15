---
description: "Premium frontend design skill: layout, typography, colors, spacing, and motion. Prevents generic AI-generated UI. Auto-invoked when building any frontend component. Source: Leonxlnx/taste-skill. Has 3 tunable settings: DESIGN_VARIANCE, MOTION_INTENSITY, VISUAL_DENSITY."
---

# Taste Skill — Premium Frontend Design

## Settings (adjust per project)

- **DESIGN_VARIANCE: 5** — How experimental the layout is. (1-3: clean/centered | 8-10: asymmetric/modern)
- **MOTION_INTENSITY: 4** — How much animation. (1-3: simple hover | 8-10: magnetic/scroll-triggered)
- **VISUAL_DENSITY: 5** — Content per screen. (1-3: spacious/luxury | 8-10: dense dashboards)

## Layout rules

- Never center everything. Create visual hierarchy through asymmetry and intentional alignment.
- Use a grid system (12-column or custom). No arbitrary positioning.
- Give elements room to breathe. Minimum 24px between unrelated elements.
- Cards and containers: consistent padding (16px minimum, 24px preferred).
- Max content width: 1200px for reading content, wider for dashboards.

## Typography

- Maximum 2 font families per project (1 preferred).
- Establish a type scale: use consistent sizes across the project (e.g., 14, 16, 20, 24, 32, 48).
- Line height: 1.5 for body text, 1.2 for headings.
- Never use font weights below 400 for body text.
- Headings: use size AND weight to establish hierarchy (not just size).

## Color

- Define a palette of 5-7 colors maximum. Primary, secondary, accent, success, warning, error, neutral.
- Use HSL for consistency. Adjust lightness for variants, keep hue constant.
- Background hierarchy: use subtle shade differences (not borders) to separate sections.
- Never use pure black (#000) on pure white (#fff). Soften both: #0f0f0f on #fafafa.

## Spacing

- Use a spacing scale (4, 8, 12, 16, 24, 32, 48, 64, 96).
- Consistent padding within components. Consistent margins between components.
- Section spacing: 64px+ between major sections on landing pages.

## Motion and animation

- Entrance animations: fade + slight translate (8-16px). Duration 200-400ms.
- Hover effects: scale(1.02) or subtle shadow elevation. Duration 150-200ms.
- Transitions: use ease-out for entrances, ease-in for exits.
- Never animate layout properties (width, height, top, left). Use transform and opacity.
- Respect prefers-reduced-motion media query.

## Anti-patterns (what makes AI UI look like "slop")

- Generic centered hero with gradient background — do something interesting
- Rounded corners on everything with the same radius — vary intentionally
- Blue/purple default palette — choose colors that match the brand
- Stock icons with no relationship to the content
- Equal spacing everywhere — use spacing to create hierarchy
- "Lorem ipsum" or obviously placeholder content
