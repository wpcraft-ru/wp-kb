# Impeccable — GitHub README

**Source:** https://github.com/pbakaus/impeccable
**Fetched:** 2026-05-10

---

The vocabulary you didn't know you need. 1 skill, 23 commands, and curated anti-patterns for impeccable frontend design.

## Why Impeccable?

Anthropic's frontend-design was the first widely-used design skill for Claude. Impeccable started from there.

Every model trained on the same SaaS templates. Skip the guidance and you get the same handful of tells: Inter for everything, purple-to-blue gradients, cards nested in cards, gray text on colored backgrounds, the rounded-square icon tile above every heading.

Impeccable adds:
- **7 domain reference files** — Typography, color, motion, spatial, interaction, responsive, UX writing
- **23 commands** — polish, audit, critique, distill, animate, bolder, quieter, and more
- **27 deterministic anti-pattern rules** plus a 12-rule LLM critique pass

## Domain References

| Reference | Covers |
|-----------|--------|
| typography | Type systems, font pairing, modular scales, OpenType |
| color-and-contrast | OKLCH, tinted neutrals, dark mode, accessibility |
| spatial-design | Spacing systems, grids, visual hierarchy |
| motion-design | Easing curves, staggering, reduced motion |
| interaction-design | Forms, focus states, loading patterns |
| responsive-design | Mobile-first, fluid design, container queries |
| ux-writing | Button labels, error messages, empty states |

## 23 Commands

| Command | What it does |
|---------|--------------|
| /impeccable craft | Full shape-then-build flow with visual iteration |
| /impeccable teach | One-time setup: write PRODUCT.md and DESIGN.md |
| /impeccable document | Generate DESIGN.md from existing code |
| /impeccable extract | Pull reusable components into design system |
| /impeccable shape | Plan UX/UI before writing code |
| /impeccable critique | UX design review |
| /impeccable audit | Technical quality checks |
| /impeccable polish | Final pass, design system alignment |
| /impeccable bolder | Amplify boring designs |
| /impeccable quieter | Tone down bold designs |
| /impeccable distill | Strip to essence |
| /impeccable harden | Error handling, i18n, edge cases |
| /impeccable onboard | First-run flows, empty states |
| /impeccable animate | Add purposeful motion |
| /impeccable colorize | Introduce strategic color |
| /impeccable typeset | Fix font choices, hierarchy, sizing |
| /impeccable layout | Fix layout, spacing, visual rhythm |
| /impeccable delight | Add moments of joy |
| /impeccable overdrive | Technically extraordinary effects |
| /impeccable clarify | Improve unclear UX copy |
| /impeccable adapt | Adapt for different devices |
| /impeccable optimize | Performance improvements |
| /impeccable live | Visual variant mode in browser |

## Anti-Patterns
- Don't use overused fonts (Arial, Inter, system defaults)
- Don't use gray text on colored backgrounds
- Don't use pure black/gray (always tint)
- Don't wrap everything in cards or nest cards inside cards
- Don't use bounce/elastic easing

## Installation

```bash
npx skills add pbakaus/impeccable -y
```

## CLI

```bash
npx impeccable detect src/
npx impeccable detect index.html
npx impeccable detect https://example.com
npx impeccable detect --fast --json .
```

## License
Apache 2.0. Based on Anthropic's frontend-design skill.
