---
source: web_search synthesis
fetched: 2026-05-05
---

# Design as Code + WordPress: Search Results Summary

## Key Findings

### WordPress и Design as Code

WordPress реализует "Design as Code" через:
1. **Gutenberg Block Editor** + **Full Site Editing (FSE)** — UI/layouts определяются программно через HTML templates с block markup и theme.json
2. **Block Development** — `npx @wordpress/create-block` для создания переиспользуемых UI-компонентов как кода
3. **theme.json** — единый source of truth для design tokens (цвета, типографика, spacing)
4. **Headless WordPress** — WP как CMS backend с GraphQL/API, рендеринг через React/Vue на отдельном frontend

### Design Tokens в WordPress

- Хранятся в `theme.json` в секции `settings.custom`
- Экспортируются как CSS custom properties: `--wp--custom--color--primary`
- Three levels: primitive → semantic → component
- **Figma Integration**: WordPress VIP Bridge Tool экспортирует токены из Figma в theme.json
- **Elementor**: плагин Design Tokens Manager for Elementor

### Инструменты

- `@wordpress/create-block` — скаффолдинг кастомных блоков
- Create Block Theme plugin — создание block themes
- WordPress VIP Design System Bridge — Figma → theme.json
- Tokens Studio → Style Dictionary → CSS pipeline (для не-WP workflow)

## Marketing as Code + WordPress: Search Results Summary

### Текущее состояние

Прямого "Marketing as Code" плагина для WordPress не существует. Но можно приблизиться через:

1. **WPCode** — управление PHP/JS/CSS сниппетами, marketing pixels, conditional logic
2. **Uncanny Automator** — no-code/low-code автоматизация маркетинга
3. **FluentCRM** — email-автоматизация, сегментация
4. **Git-версионирование** маркетинговых стратегий
5. **IaC для DevOps** WordPress (автодеплой)

### Подход Marchcroft/Mayfield

- Objectives & Guardrails вместо ручных кампаний
- AI-агенты для генерации/тестирования/оптимизации
- IBM 2026: "Agentic Leaders" показывают +60% к эффективности
- Firecrawl: Claude Code для маркетинга — автоскрейпинг конкурентов, обновление контента

## Sources

- rtCamp: Design Tokens in WP Site Editor
- WordPress VIP: Design System with Block Editor
- Perplexity/Sonar search results
