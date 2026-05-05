# Design Tokens в WordPress Themes — Результаты поиска

**Дата:** 2026-05-05
**Запрос:** "design tokens" wordpress theme

---

## Основная концепция

Design tokens в WordPress — атомарные значения дизайна (цвета, отступы, типографика, тени), определяемые в `theme.json` для обеспечения консистентности в block themes и Site Editor.

- Токены = единый источник истины (single source of truth)
- Экспортируются как CSS custom properties: `--wp--custom--color--primary`
- Разделение на **primitive tokens** (raw hex/px) и **semantic tokens** (purpose-driven имена)

## Реализация в theme.json

**Секция `settings.custom`** — добавление токенов для:
- Цветов
- Размеров шрифтов
- Шкал отступов
- И др.

**Ссылка:** `var(--wp--custom--text--primary)`

**Presets** (settings.palette, settings.fontSizes и т.д.) — генерируют пользовательские опции в Global Styles.
**Custom tokens** (settings.custom) — под контролем разработчика, не показываются пользователю.

## Инструменты и плагины

- **Figma Integration** — экспорт токенов напрямую в theme.json (Figma Tokens → VIP bridge)
- **Design Tokens Manager for Elementor** — bulk edit + синхронизация для Elementor
- **@wordpress/theme** — официальные токены как CSS-переменные для Gutenberg-компонентов

## Best Practices

- Хранить токены централизованно в `theme.json` → секция `custom` → избежать конфликтов
- Использовать семантические имена
- Ссылаться через var() — для theming (light/dark modes)
- Автоматизация: PostCSS плагины для fallbacks и валидации

## Ссылки

| Ресурс | URL |
|--------|-----|
| @wordpress/theme package | https://developer.wordpress.org/block-editor/reference-guides/packages/packages-theme/ |
| rtCamp Design Tokens Handbook | https://rtcamp.com/handbook/designing-for-wordpress-site-editor/design-tokens/ |
| WP VIP Theme.json Design System | https://wpvip.com/blog/using-a-design-system-with-the-wordpress-block-editor-pt-1-theme-json/ |
| Figma to WordPress (Fueled) | https://fueled.com/blog/figma-to-wordpress-in-minutes/ |
