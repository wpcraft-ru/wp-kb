---
title: "Источники паттернов"
description: "Где брать готовые блочные паттерны для WordPress: WordPress.org Patterns, TypeUI prompts, Flowbite Blocks — каталоги и инструменты генерации."
---

## Обзор источников

Помимо создания паттернов вручную, есть несколько источников готовых решений и вдохновения:

| Источник | Формат | Количество | Бесплатно |
|----------|--------|-----------|-----------|
| [WordPress.org Patterns](https://wordpress.org/patterns/) | Block markup | 2 200+ | Да |
| [TypeUI Prompts](https://typeui.sh/prompts) | AI-промпты | 85 | Да (open source) |
| [Flowbite Blocks](https://flowbite.com/blocks/) | Tailwind HTML | 459 | 277 free + 182 pro |

## WordPress.org Patterns

Официальный каталог блочных паттернов WordPress.org. Каждый паттерн можно скопировать (copy-paste) и вставить в редактор.

**Категории:** Featured, Posts, Text, Gallery, Call to Action, Banners, Headers, Footers, Wireframe.

**Как использовать:**
1. Открыть [wordpress.org/patterns](https://wordpress.org/patterns/)
2. Выбрать паттерн
3. Нажать **Copy pattern**
4. Вставить в редактор WordPress (Ctrl+V / ⌘V)

**Для тем:** скопированный block markup можно превратить в темо-регистрируемый паттерн — обернуть в file header и поместить в `/patterns/`.

## TypeUI Prompts

[TypeUI](https://typeui.sh) — open-source CLI для AI-генерации UI. Включает 85 дизайн-промптов для AI-инструментов (Claude Code, Cursor, Copilot).

**Категории промптов:**

| Категория | Вариантов | Примеры |
|-----------|-----------|---------|
| Hero Sections | 17 | Hero with image, centered, split |
| Pricing Sections | 20 | 3-tier, comparison table, with FAQ |
| Feature Sections | 16 | Grid, alternating, with icons |
| CTA Sections | 11 | Banner, split, centered |
| Content Sections | 12 | Two-column, timeline, accordion |
| Contact Sections | 9 | Form + map, minimal, full-width |

Эти промпты генерируют HTML/CSS, который можно адаптировать в WordPress-паттерны. Полезно как отправная точка для дизайна.

## Flowbite Blocks

[Flowbite Blocks](https://flowbite.com/blocks/) — коллекция Tailwind CSS UI-компонентов. 459 блоков, сгруппированных по типам интерфейсов.

### Marketing UI (156 компонентов)

| Категория | Количество |
|-----------|------------|
| Hero Sections | 18 |
| Feature Sections | 10 |
| CTA Sections | 10 |
| Pricing Tables | 7 |
| Headers | 8 |
| Footer Sections | 7 |
| Contact Forms | 6 |
| FAQ Sections | 5 |
| Blog Sections | 5 |
| Portfolio | 5 |
| Testimonials | 5 |

### Application UI (140+ компонентов)

Table headers, side navigations, application shells, CRUD-модалки, advanced tables, dashboard navbars, Kanban boards.

### E-commerce UI (80+ компонентов)

Payment forms, storefront hero, product categories, shopping cart, order summary, product overview, mega footers.

### Адаптация под WordPress

Flowbite использует Tailwind CSS — для использования в WordPress-паттернах требуется:
1. Tailwind, интегрированный в тему (см. [Tailwind CSS v4 + daisyUI в блочной теме WordPress](../tailwind-daisyui-wordpress.md))
2. Замена Tailwind-классов на WordPress-стили (если Tailwind не используется)

## Рабочий процесс: от источника к паттерну

1. **Выбрать дизайн** — WordPress.org / TypeUI / Flowbite
2. **Скопировать/сгенерировать код** — HTML-разметку или block markup
3. **Адаптировать** — перевести на WordPress-блоки, добавить i18n
4. **Обернуть в file header** — `Title`, `Slug`, `Categories`
5. **Сохранить в `/patterns/`**
6. **Протестировать** в редакторе

## Материалы и источники

- [Block Pattern Directory (wordpress.org)](https://wordpress.org/patterns/)
- [TypeUI — UI Design Prompts](https://typeui.sh/prompts)
- [Flowbite Blocks — Tailwind UI Components](https://flowbite.com/blocks/)

## Связанные страницы

- [Паттерны: обзор](./index.md)
- [Регистрация паттернов](./registering.md)
- [Tailwind CSS v4 + daisyUI в блочной теме WordPress](../tailwind-daisyui-wordpress.md)
- [Дизайн-системы в WordPress](../design-systems.md)
