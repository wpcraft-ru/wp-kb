---
source: https://rtcamp.com/handbook/designing-for-wordpress-site-editor/design-tokens/
fetched: 2026-05-05
---

# Design Tokens in WordPress Site Editor

Источник: rtCamp Handbook

## Design tokens in the system

Commonalities between a designer's design system and developer's theme.json. Knowing what to define in the design system saves time.

## Color Palette

theme.json supports as many colors as needed. Design system should have:
- Primary, secondary, tertiary and accent colors
- Text and background color combinations
- Feedback colors: info, success, warning, error

Aim for WCAG Compliance of at least AA. WordPress supports gradients in theme.json.

## Typography

- Heading and body text styles
- Font size and line height scales
- Font weight variations
- Special typography styles (quotes, captions)
- Responsive typography settings
- RTL / LTR support

Use rem units (16px = 1rem). Simple scales from small (S) to extra large (XXL).

## Spacing

Consistent spacing tokens:
- Spacing values
- Margin and padding values
- Inter-block gap

Spacing scales: multiplicative (0, 0.13, 0.25, 0.5, 1, 2, 4, 8 rem) or additive (0, 1, 3, 5, 7, 9, 11, 13 rem).
