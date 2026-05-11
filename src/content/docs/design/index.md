---
title: "Дизайн в WordPress"
description: "Полное руководство по дизайну сайтов на WordPress: Style Book, блочное мышление, композиция макетов, доступность, работа с медиа-блоками."
---

## Страницы раздела

### Основы дизайна в WordPress

- [WordPress для дизайнеров](./wordpress-for-designers.md) — Обзор возможностей WordPress как дизайн-платформы: Block Editor, Style Book, Каталог паттернов, Site Editor.
- [Style Book: гид по дизайну](./style-book.md) — Ключевой инструмент дизайнера для визуального проектирования без контента.
- [Иерархия блочной темы](./block-theme-hierarchy.md) — Как устроена блочная тема: от блоков до шаблонов.
- [Block-first мышление](./block-first-mindset.md) — Философия модульного дизайна: проектирование переиспользуемых компонентов.

### Дизайн-системы и Design as Code

- [Дизайн-системы в WordPress](./design-systems.md) — Atomic Design, компонентный подход, от Figma до theme.json.
- [theme.json — основа дизайн-системы WordPress и WooCommerce](./theme-json.md) — Структура, design tokens, CSS-переменные, WooCommerce-специфика, lock down, интеграции, border radius presets (WP 6.9+).
- [Figma → WordPress: мост через Design Tokens](./figma-to-wordpress.md) — Пайплайн синхронизации: Tokens Studio → vip-design-system-bridge → theme.json.

### Инструменты и интеграции

- [DESIGN.md для WordPress](./design-md.md) — Контракт дизайна для AI-агентов и синхронизация с theme.json, Tailwind и daisyUI.
- [Tailwind CSS v4 + daisyUI в блочной теме WordPress](./tailwind-daisyui-wordpress.md) — Интеграция utility-first и компонентного слоя с block theme и theme.json.
- [Impeccable: AI-дизайн для WordPress](./impeccable.md) — Open-source навык для AI-ассистентов: 23 команды, детектор антипаттернов, генерация DESIGN.md.
- [Site Editor для прототипирования](./site-editor-prototyping.md) — Почему стоит проектировать прямо в редакторе сайта.
- [Доступность (a11y)](./accessibility.md) — Руководство по созданию инклюзивных сайтов и инструменты тестирования.
- [Настройки темы](./theme-settings.md) — Цвета, шрифты, типографика и макет в Global Styles.

### Композиция и макеты

- [Композиция макетов](./layout-composition.md) — Group, Row/Stack, Columns: построение структуры страницы.
- [Отступы и размеры](./dimensions-spacing.md) — Padding, margin и block spacing: управление пространством.
- [Медиа-блоки](./media-blocks.md) — Галерея, Media & Text, Cover: работа с изображениями и видео.
- [Карусели и слайдеры](./carousel-slider.md) — Создание слайдеров через плагины с учётом доступности.

### Паттерны (Block Patterns)

- [Паттерны: обзор](./patterns/index.md) — Что такое блочные паттерны, типы, иерархия, каталог подстраниц.
- [Регистрация паттернов](./patterns/registering.md) — `/patterns`-директория, PHP-регистрация, file headers, категории, unregistering.
- [PHP в паттернах](./patterns/php.md) — Интернационализация, динамические URL изображений, `foreach()`-циклы, ограничения `init`.
- [Паттерны в шаблонах](./patterns/templates.md) — `<!-- wp:pattern -->`, DRY-принцип, практические примеры.
- [Block Type Patterns](./patterns/block-type.md) — Привязка паттернов к конкретным блокам, Query Loop, Template Parts.
- [Стартовые паттерны](./patterns/starter.md) — Страничные и шаблонные стартовые точки для пользователей темы.
- [Block Locking и паттерны](./patterns/locking.md) — Курирование через запрет перемещения, удаления, content-only editing.
- [Источники паттернов](./patterns/directory.md) — WordPress.org Patterns, TypeUI, Flowbite: каталоги готовых решений.

## Материалы и источники

- [Beginner WordPress Designer (learn.wordpress.org)](https://learn.wordpress.org/course/beginner-wordpress-designer/)
