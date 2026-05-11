---
title: "Паттерны в шаблонах"
description: "Как использовать блочные паттерны внутри шаблонов и template parts: блок wp:pattern, DRY-принцип, практические примеры."
---

## Зачем включать паттерны в шаблоны

Две главные причины:

1. **PHP-доступ.** Шаблоны и template parts — HTML-файлы. Если нужна интернационализация, динамические URL или циклы — выносите логику в паттерн.

2. **DRY (Don't Repeat Yourself).** Один и тот же дизайн Query Loop используется на Home, Archive и Search? Создайте один паттерн и вставьте его во все три шаблона.

## Блок `wp:pattern`

Для вставки паттерна в шаблон используется блок Pattern. Он не отображается в UI-инсертере — только в коде шаблонов:

```html
<!-- wp:pattern {"slug":"themeslug/hero"} /-->
```

Параметр `slug` должен включать namespace и имя паттерна (как в file header при регистрации).

Можно вставлять любые зарегистрированные паттерны: из темы, ядра WordPress, плагинов.

## Пример: паттерн в Home-шаблоне

**Шаг 1.** Создайте паттерн `/patterns/hero.php`:

```php
<?php
/**
 * Title: Hero
 * Slug: themeslug/hero
 * Categories: featured
 */
?>
<!-- wp:cover {"url":"<?php echo esc_url( get_theme_file_uri( 'assets/images/hero.jpg' ) ); ?>","dimRatio":50} -->
<div class="wp-block-cover">
  <span aria-hidden="true" class="wp-block-cover__background has-background-dim"></span>
  <div class="wp-block-cover__inner-container">
    <!-- wp:heading {"level":1} -->
    <h1><?php esc_html_e( 'Добро пожаловать', 'themeslug' ); ?></h1>
    <!-- /wp:heading -->
  </div>
</div>
<!-- /wp:cover -->
```

**Шаг 2.** Вставьте в `templates/home.html`:

```html
<!-- wp:template-part {"slug":"header","theme":"themeslug"} /-->

<!-- wp:pattern {"slug":"themeslug/hero"} /-->

<!-- wp:query {"queryId":1,"query":{"perPage":3,"pages":0,"offset":0,"postType":"post","order":"desc","orderBy":"date","author":"","search":"","exclude":[],"sticky":"","inherit":true}} -->
<div class="wp-block-query">
  <!-- wp:post-template -->
    <!-- wp:post-title /-->
    <!-- wp:post-excerpt /-->
  <!-- /wp:post-template -->
</div>
<!-- /wp:query -->

<!-- wp:template-part {"slug":"footer","theme":"themeslug"} /-->
```

Результат: Hero появляется на главной странице и в редакторе сайта.

## DRY: общий Query Loop

**Паттерн** `/patterns/query-grid.php`:

```php
<?php
/**
 * Title: Query: Сетка 3 колонки
 * Slug: themeslug/query-grid
 * Categories: posts
 * Block Types: core/query
 */
?>
<!-- wp:query {"queryId":0,"query":{"perPage":6,"pages":0,"offset":0,"postType":"post","order":"desc","orderBy":"date","author":"","search":"","exclude":[],"sticky":"","inherit":true},"align":"wide"} -->
<div class="wp-block-query alignwide">
  <!-- wp:post-template {"layout":{"type":"grid","columnCount":3}} -->
    <!-- wp:post-featured-image {"isLink":true} /-->
    <!-- wp:post-title {"isLink":true} /-->
    <!-- wp:post-excerpt /-->
    <!-- wp:post-date /-->
  <!-- /wp:post-template -->
  <!-- wp:query-pagination -->
    <!-- wp:query-pagination-previous /-->
    <!-- wp:query-pagination-numbers /-->
    <!-- wp:query-pagination-next /-->
  <!-- /wp:query-pagination -->
</div>
<!-- /wp:query -->
```

Теперь один и тот же паттерн используется в `templates/home.html`, `templates/archive.html`, `templates/search.html`:

```html
<!-- wp:pattern {"slug":"themeslug/query-grid"} /-->
```

## Template Parts vs Паттерны в шаблонах

| | Template Part | Паттерн в шаблоне |
|---|---|---|
| **Файл** | `.html` | `.php` |
| **PHP** | Нет | Да |
| **Редактирование** | Глобальное, через Site Editor | Через админку паттернов |
| **Вставка** | `<!-- wp:template-part -->` | `<!-- wp:pattern -->` |
| **Когда использовать** | Shared-секции (header, footer) | Секции, которым нужен PHP или DRY |

## Материалы и источники

- [Usage in Templates — Theme Handbook (developer.wordpress.org)](https://developer.wordpress.org/themes/patterns/usage-in-templates/)

## Связанные страницы

- [Паттерны: обзор](./index.md)
- [Регистрация паттернов](./registering.md)
- [PHP в паттернах](./php.md)
- [Шаблоны WordPress](../../themes/templates.md)
