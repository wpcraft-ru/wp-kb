---
title: "Стартовые паттерны"
description: "Страничные и шаблонные стартовые паттерны: как дать пользователям темы готовые отправные точки для новых страниц и шаблонов."
---

## Что такое стартовые паттерны

Стартовые паттерны — готовые композиции блоков, которые пользователь видит при создании новой страницы или шаблона. Это «onboarding» для темы: вместо пустого холста — выбор из готовых дизайнов.

Два типа:
- **Страничные (Page Patterns)** — при создании новой страницы (Страницы → Добавить новую)
- **Шаблонные (Template Patterns)** — при создании нового шаблона (Редактор сайта → Шаблоны → +)

## Страничные стартовые паттерны

### Как это выглядит

При создании новой страницы пользователь видит модальное окно с выбором паттерна:

> **Choose a pattern** — набор готовых макетов, любой из которых вставляется в область контента.

### Как создать

Нужны два поля в file header:

| Поле | Значение | Зачем |
|------|----------|-------|
| **`Block Types`** | `core/post-content` | Говорит WordPress: этот паттерн для содержимого поста |
| **`Post Types`** | `page` (или `post`, `page,post`) | Ограничивает типы записей |

**Пример: страничный паттерн «О нас»**

```php
<?php
/**
 * Title: Страница «О нас»
 * Slug: themeslug/about-page
 * Categories: page
 * Block Types: core/post-content
 * Post Types: page
 * Viewport width: 1376
 */
?>
<!-- wp:group {"layout":{"type":"constrained"}} -->
<div class="wp-block-group">
  <!-- wp:heading -->
  <h2><?php esc_html_e( 'О нас', 'themeslug' ); ?></h2>
  <!-- /wp:heading -->

  <!-- wp:paragraph -->
  <p><?php esc_html_e( 'Мы — команда профессионалов.', 'themeslug' ); ?></p>
  <!-- /wp:paragraph -->

  <!-- wp:gallery {"linkTo":"none"} -->
  <figure class="wp-block-gallery has-nested-images columns-3">
    <!-- wp:image {"id":100,"sizeSlug":"large"} -->
    <figure class="wp-block-image size-large">
      <img src="<?php echo esc_url( get_theme_file_uri( 'assets/images/team-1.jpg' ) ); ?>" alt=""/>
    </figure>
    <!-- /wp:image -->
  </figure>
  <!-- /wp:gallery -->
</div>
<!-- /wp:group -->
```

### Важно

- Страничные паттерны — это контент, поэтому **не включайте header/footer** — только то, что идёт в область содержимого.
- Работает с любыми post types, поддерживающими блочный редактор (`post`, `page`, CPT).

## Шаблонные стартовые паттерны

### Как это выглядит

При создании нового шаблона (например, Front Page) пользователь видит модальное окно **Choose a pattern** поверх редактора.

### Как создать

Нужно поле `Template Types` в file header:

```php
<?php
/**
 * Title: Главная страница
 * Slug: themeslug/front-page
 * Template Types: front-page, home
 * Viewport width: 1376
 * Inserter: no
 */
?>
```

| Поле | Описание |
|------|----------|
| **`Template Types`** | Типы шаблонов через запятую: `front-page`, `home`, `single`, `archive`, `search`, `404`, `page`, `index` |
| **`Inserter`** | Обычно `no`/`false` — шаблонные паттерны не должны быть доступны в общем инсертере |

### Поддерживаемые типы шаблонов

| Тип | Когда появляется |
|-----|-----------------|
| `front-page` | При создании шаблона главной страницы |
| `home` | При создании шаблона блога |
| `single` | Шаблон отдельной записи |
| `page` | Шаблон статической страницы |
| `archive` | Шаблон архива |
| `search` | Шаблон поиска |
| `404` | Шаблон страницы не найдена |
| `index` | Универсальный fallback-шаблон |

### Пример: шаблонный паттерн Front Page

```php
<?php
/**
 * Title: Главная — Hero + Услуги + Блог
 * Slug: themeslug/front-page-full
 * Template Types: front-page, home
 * Viewport width: 1376
 * Inserter: no
 */
?>
<!-- wp:template-part {"slug":"header","theme":"themeslug"} /-->

<!-- wp:pattern {"slug":"themeslug/hero"} /-->

<!-- wp:pattern {"slug":"themeslug/services"} /-->

<!-- wp:pattern {"slug":"themeslug/query-grid"} /-->

<!-- wp:template-part {"slug":"footer","theme":"themeslug"} /-->
```

Шаблонный паттерн может собираться из других паттернов через `<!-- wp:pattern -->`, template parts и отдельных блоков.

### Отличия от страничных паттернов

| | Страничный | Шаблонный |
|---|---|---|
| **Где появляется** | Страницы → Добавить новую | Редактор сайта → Шаблоны → + |
| **Содержит** | Только контент | Весь шаблон (header, content, footer) |
| **Header field** | `Block Types: core/post-content` + `Post Types` | `Template Types` |
| **Inserter** | По умолчанию `true` | Обычно `no` |

## Материалы и источники

- [Starter Patterns — Theme Handbook (developer.wordpress.org)](https://developer.wordpress.org/themes/patterns/starter-patterns/)

## Связанные страницы

- [Паттерны: обзор](./index.md)
- [Регистрация паттернов](./registering.md)
- [Паттерны в шаблонах](./templates.md)
- [Block Type Patterns](./block-type.md)
- [Шаблоны WordPress](../../themes/templates.md)
