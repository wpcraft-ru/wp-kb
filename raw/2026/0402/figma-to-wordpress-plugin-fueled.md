# From Figma Tokens to WordPress Styles in Minutes — Figma Plugin (Fueled/10up)

**Источник:** https://fueled.com/blog/figma-to-wordpress-in-minutes/
**Дата:** 2025

---

## О проекте

Open-source Figma Plugin от Fueled (10up): [figma-to-wordpress-theme-json-exporter](https://github.com/10up/figma-to-wordpress-theme-json-exporter)

### Проблема
Перевод токенов из Figma в WordPress theme.json вручную занимает 10–15 часов.

### Решение
Плагин читает переменные напрямую из Figma и генерирует готовый `theme.json` (или дописывает к существующему).

---

## Что экспортирует

- Colors
- Spacing
- Font sizes и text styles
- Fluid typography (опционально)
- Mode-based variations (light/dark, desktop/mobile — на основе конвенций именования)

Работает с любым Figma-файлом, содержащим именованные коллекции переменных, и с любой современной темой на `theme.json`.

---

## Результаты

- Время: с 10–15 часов до ~1 часа
- Не требует Dev Mode лицензий Figma для миграции свойств
- Прототип сделан за несколько часов через Cursor (AI-assisted development)

---

## Что дальше

- Border-radius presets с адаптацией под экран
- Component-level export → маппинг целых дизайн-элементов на Gutenberg блоки

---

## Ссылки

- Репозиторий: https://github.com/10up/figma-to-wordpress-theme-json-exporter
- Компания: Fueled (https://fueled.com)
