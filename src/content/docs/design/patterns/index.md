---
title: "Паттерны (Block Patterns)"
description: "Полное руководство по блочным паттернам WordPress: что это, типы, регистрация, PHP, шаблоны, block locking и источники готовых паттернов."
---

## Что такое паттерны

Блочные паттерны (block patterns) — предварительно сконфигурированные группы блоков. По сути, это **переиспользуемые композиции блоков** — один из самых мощных инструментов для авторов тем. Появились в WordPress 5.4.

Паттерны можно вставлять:
- В контент страниц и записей через Inserter
- В шаблоны и template parts через `<!-- wp:pattern -->`
- Как стартовые точки для новых страниц или шаблонов

В отличие от **template parts** (HTML-файлы), паттерны — PHP-файлы. Это даёт доступ к интернационализации, динамическим URL изображений, циклам и другим PHP-возможностям.

## Типы паттернов

| Тип | Описание | Создание |
|-----|----------|----------|
| **Not synced** (несинхронизированные) | При вставке создаётся независимая копия. Изменения не затрагивают другие вставки | Тема или админка |
| **Synced** (синхронизированные) | Изменение оригинала обновляет все вставки. Ранее назывались «reusable blocks» | Только админка |

**Все паттерны, регистрируемые темой — Not synced.** Синхронизация темо-регистрируемых паттернов пока невозможна ([открыт тикет](https://github.com/WordPress/gutenberg/issues/59272)).

## Где управлять паттернами

- **Блочная тема:** Внешний вид → Редактор сайта → Паттерны
- **Классическая тема (WP 6.5+):** Внешний вид → Паттерны
- **Каталог WordPress.org:** [wordpress.org/patterns](https://wordpress.org/patterns/) — 2200+ готовых паттернов

## Разделы руководства

- [Регистрация паттернов](./registering.md) — `/patterns`-директория, PHP-регистрация, file headers, категории, unregistering
- [PHP в паттернах](./php.md) — Интернационализация, изображения, циклы `foreach()`, ограничения `init`
- [Паттерны в шаблонах](./templates.md) — `<!-- wp:pattern -->`, DRY-принцип, практические примеры
- [Block Type Patterns](./block-type.md) — Привязка паттернов к конкретным блокам, Query Loop, Template Parts
- [Стартовые паттерны](./starter.md) — Страничные и шаблонные стартовые точки для пользователей темы
- [Block Locking и паттерны](./locking.md) — Курирование через запрет перемещения, удаления, content-only editing
- [Источники паттернов](./directory.md) — WordPress.org Patterns, TypeUI, Flowbite: каталоги готовых решений

## Материалы и источники

- [Patterns — Theme Handbook (developer.wordpress.org)](https://developer.wordpress.org/themes/patterns/)
- [Introduction to Patterns (developer.wordpress.org)](https://developer.wordpress.org/themes/patterns/introduction-to-patterns/)
- [Block Pattern Directory (wordpress.org)](https://wordpress.org/patterns/)

## Связанные страницы

- [Иерархия блочной темы](../block-theme-hierarchy.md)
- [Дизайн-системы в WordPress](../design-systems.md)
- [Шаблоны WordPress](../../themes/templates.md)
- [Block-first мышление](../block-first-mindset.md)
- [theme.json — основа дизайн-системы](../theme-json.md)
