---
source: https://wpvip.com/blog/using-a-design-system-with-the-wordpress-block-editor-pt-1-theme-json/
fetched: 2026-05-05
---

# Using a Design System with the WordPress Block Editor: Theme.json

Источник: WordPress VIP (Automattic) — Alec Geatches, Gopal Krishnan

## Understanding theme.json

Configuration file for global settings and styles. Allows controlling global settings and styles for WordPress site. Block themes use theme.json as foundation.

## Storing Design Tokens

Always keep tokens in the `custom` section of settings:
```json
{
  "settings": {
    "custom": {
      "text": {
        "primary": "#00FF00",
        "background": "000000"
      }
    }
  }
}
```

Reference via CSS variables: `var(--wp--custom--text--primary)`

## Using Design Tokens

```json
{
  "styles": {
    "color": {
      "text": "var(--wp--custom--text--primary)",
      "background": "var(--wp--custom--text--background)"
    }
  }
}
```

Schema: `var(--wp--custom--<location to the value with -- as separator>)`

## Best Practices

1. Use WordPress-generated design token variables everywhere — not raw values
2. Tokens are accessible in custom CSS outside theme.json too
3. WordPress VIP's bridge tool connects Figma → theme.json automatically

## Modify Editor and Block Styles

Two key locations:
- `settings` / `settings.blocks` — block editor settings
- `styles` / `styles.blocks` — preset styling

### Styling Warning
Only use theme.json's styles for block styling. Do not use CSS overrides — they're not stable across editor updates.

### Lock Down Approach

Globally disable all controls (border, color, typography, spacing), then selectively enable per block:
```json
{
  "settings": {
    "appearanceTools": true,
    "border": { "color": false, "radius": false, "style": false, "width": false },
    "color": { "text": false, "background": false, "link": false, "custom": false },
    "spacing": { "margin": false, "padding": false, "units": ["rem", "%"] },
    "typography": { "customFontSize": false, "fontStyle": false, "fontWeight": false }
  }
}
```

## Useful Links

- Part 2: Block Types and Styles
- What is Theme.json? — developer.wordpress.org
- Global Settings & Styles (theme.json) — developer.wordpress.org
- WordPress VIP Design System Bridge (Figma → theme.json) — github.com/Automattic/vip-design-system-bridge
