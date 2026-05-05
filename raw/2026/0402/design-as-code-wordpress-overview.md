# Design as Code в WordPress: Обзор подхода

**Источник:** Perplexity Sonar Pro Search (результаты поиска по запросу "design as code wordpress")
**Дата:** 2026-05-05

---

## Что такое "Design as Code"

"Дизайн как код" — подход, при котором код является основным инструментом для определения дизайна: UI и макеты задаются программно, а не через визуальные drag-and-drop инструменты. Это обеспечивает:
- Версионный контроль
- Итеративность
- Developer-friendly workflows

## Реализация в WordPress

WordPress поддерживает "design as code" в основном через **Gutenberg Block Editor** и **Full Site Editing (FSE)**:

1. **HTML-шаблоны с block-разметкой**: Сайты проектируются через HTML-шаблоны, встроенные с block markup и стилизованные через `theme.json`.

2. **Кастомные блоки (React/PHP/CSS)**: Разработчики создают темы или плагины с помощью JavaScript (React), PHP, CSS и `block.json` конфигураций, получая точный code-driven контроль над макетами и стилями.

## Ключевые подходы к реализации

### 1. Block Development
- Создание переиспользуемых UI-компонентов через `@wordpress/create-block`
- Регистрация блоков через `registerBlockType()`
- Рендеринг для редактора и фронтенда

### 2. Block Themes и FSE
- Дизайн полных сайтов через HTML-шаблоны с block-синтаксисом: `<!-- wp:heading -->`
- Управление в кодовых редакторах (VS Code)
- Экспорт из Site Editor или написание вручную

### 3. Headless WordPress
- WordPress как бэкенд CMS с Gutenberg/GraphQL
- Рендеринг блоков как HTML или React/Vue-компонентов на отдельном фронтенде
- Полный code-first контроль над дизайном

### 4. Custom Code Tools
- Плагины типа Code Snippets для безопасного добавления PHP/JS/CSS
- Редактирование HTML через Custom HTML blocks
- Редактирование theme files для точечных изменений

## Быстрый старт

```bash
npx @wordpress/create-block my-block
```

Отредактировать `block.json` для атрибутов, определить `edit`/`save` функции в JS. Собрать и задеплоить как плагин.

Для тем: использовать Create Block Theme plugin, конфигурировать в коде, итерировать через Git.

## Вывод

Этот подход выравнивает WordPress с современными практиками разработки, снижая зависимость от визуальных билдеров, сохраняя при этом доступность редактора для контент-менеджеров.
