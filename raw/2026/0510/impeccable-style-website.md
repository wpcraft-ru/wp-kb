# Impeccable — Website Content

**Source:** https://impeccable.style/
**Fetched:** 2026-05-10

---

## Highlights since v3.0 (April 10 – May 4, 2026)

- Live mode preserves identity by default. Variants stay on-brand for the existing surface and explore different expression axes within that identity. Departure from the current aesthetic only triggers when PRODUCT.md anti-references explicitly reject the current surface, or the user asks for it. Departure directions derive from brand voice, not a fixed catalog.
- Detector flags the new monoculture. The overused-font rule now catches Fraunces, Geist, Mona Sans, Plus Jakarta Sans, Space Grotesk, Recoleta, and Instrument Sans. Contrast checks run on styled and elements. Brand exceptions for Vercel, Next.js, and GitHub on their own domains.
- Live mode works everywhere. Strict-CSP apps, modal hosts (Radix, Headless UI, vaul), React/TSX projects, and repeated identical-class siblings all handle cleanly through the wrap, preview, accept, and carbonize loop.
- PRODUCT.md + DESIGN.md as shared context. A strategic file (who, what, why) and a visual file (colors, typography, components in the Google Stitch format) that every command reads before generating. Created via /impeccable teach and /impeccable document.
- 18 skills became 1 skill with 23 commands. One /impeccable entry in the menu, shared design vocabulary, and /impeccable pin to promote favorites back as standalone shortcuts.

## Full Version History

### Detector flags italic-serif display heroes
Oversized italic serif (Fraunces, Recoleta, Newsreader, Playfair, Cormorant, Tiempos) running as the primary hero h1 is now caught as a structural fingerprint of late-2025 and early-2026 AI-generated marketing pages. Contributed by @vinaypokharkar in #129.

### Detector flags hero eyebrow chips
The uppercase letter-spaced label sitting directly above a hero h1 now fires, including the pill-chip variant (background plus 999px border-radius). Contributed by @vinaypokharkar in #129.

### Live mode survives disconnects
A durable session journal records every event, so an agent crash, a network blip, or a browser refresh no longer loses the session. Three new sub-commands: live status, live resume, live complete. Contributed by @nqh-packages in #125.

### Reference files stripped of repetitive scaffolding
SKILL.md and 33 sub-command files lost the "Remember:" closer chants, the brochure-style openers on 12 older commands, and 419 em-dashes.

### Contrast checks now run on styled buttons
A styled or with its own opaque background was silently skipped by the contrast rule. Buttons with their own background and direct text now flow through the same checks.

### Detector runs on Windows
CLI path resolution used new URL(...).pathname, which prepends a slash to drive letters. Switched to fileURLToPath. Reported in #95, contributed by @voidborne-d.

### Live mode lands valid TSX
The wrapper now keeps a single JSX-slot child instead of three adjacent siblings. Works with return (...), array .map(...), and asChild parents like Radix's. Closes #114.

### Live mode runs in strict-CSP apps
Silently appends the live-server origin to script-src and connect-src on session start, reverts on stop.

### Live Mode (Alpha)
Run /impeccable live and iterate on your UI in the browser: pick any element, drop a comment or stroke, hit Go, get three production-quality variants swapped in via your framework's HMR, accept the one you want and it writes back to source. Works on Vite, Next.js, SvelteKit, Astro, Nuxt.

### Visualize, then build
Image gen crossed the reference-quality threshold with GPT Image 2, Nano Banana Pro, and Imagen 4 Ultra. /impeccable shape drafts a brand toolkit as real images; /impeccable craft pre-renders the hi-fi mock.

### PRODUCT.md
Shared design memory for your AI. A single file at your project root that names the audience, brand personality, anti-references, and register. Every command reads it before generating. Created via /impeccable teach.

### DESIGN.md generation
/impeccable document scans your tokens and writes a DESIGN.md following the Google Stitch format. Interoperable with other DESIGN.md-aware tools.

### Brand and product registers
Brand (marketing sites, landing pages) vs product (app UI, dashboards). Register-aware commands adjust their vocabulary to match.

### Streamlined commands
Removed overlap: /arrange → /layout, /normalize → merged into /polish, /onboard → merged into /harden, /extract → /impeccable extract.

### Anti-pattern detection engine
27 deterministic rules across typography, color, layout, motion, and quality. CLI: npx impeccable detect. Chrome DevTools extension in Web Store review.

### /critique got teeth
Persona sub-agents review in parallel, score against Nielsen's heuristics, run the detector automatically, and open a live browser overlay.

### Skill rewritten against evals
Fifteen briefs ran through gpt-5.4 and Qwen 3.6 Plus. More font and color variety, fewer purple gradients. The single change that moved the numbers most: before committing to a font or palette, name first three instincts and reject them.

## Supported AI Tools
Cursor, Claude Code, OpenCode, Pi, Gemini CLI, Codex CLI, VS Code Copilot, Kiro, Trae, Rovo Dev, Qoder, Windsurf, Cline, AiderDesk, Augment, CodeBuddy, Codemaker, and more (55 total).

## Installation
```bash
npx skills add pbakaus/impeccable -y
```

## CLI
```bash
npx impeccable detect src/
npx impeccable detect https://example.com
npx impeccable detect --fast --json .
```
