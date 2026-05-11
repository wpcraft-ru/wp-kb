---
title: "Block Type Patterns"
description: "Паттерны, привязанные к конкретным типам блоков: регистрация, Query Loop patterns, Template Part patterns и практические примеры."
---

## Что такое Block Type Patterns

Block Type Patterns — паттерны, которые появляются только при работе с определённым блоком. Когда пользователь вставляет блок (например, Cover) и нажимает его иконку в тулбаре, он видит пункт **Patterns** со списком подходящих вариантов.

Это элегантный способ предложить пользователю готовые стилизации конкретного блока.

## Регистрация Block Type Pattern

В file header нужно указать `Block Types` с namespace/slug блока:

```php
<?php
/**
 * Title: Cover с контрастным фоном
 * Slug: themeslug/cover-contrast
 * Categories: banner
 * Block Types: core/cover
 * Viewport Width: 1376
 */
?>
<!-- wp:cover {"overlayColor":"contrast","minHeight":400,"minHeightUnit":"px"} -->
<div class="wp-block-cover" style="min-height:400px">
  <span aria-hidden="true" class="wp-block-cover__background has-contrast-background-color has-background-dim-100 has-background-dim"></span>
  <div class="wp-block-cover__inner-container">
    <!-- wp:heading {"textAlign":"center","level":1} -->
    <h1 class="wp-block-heading has-text-align-center">
      <?php esc_html_e( 'Простой заголовок', 'themeslug' ); ?>
    </h1>
    <!-- /wp:heading -->
  </div>
</div>
<!-- /wp:cover -->
```

Можно указать несколько типов блоков через запятую:

```
Block Types: core/cover, core/group
```

### Как это работает в редакторе

1. Пользователь вставляет Cover block
2. Нажимает иконку Cover в тулбаре
3. Выбирает **Patterns**
4. Видит ваш «Cover с контрастным фоном»
5. При выборе — текущий Cover **трансформируется** в паттерн

## Query Loop Patterns

Query Loop — один из самых полезных блоков для Block Type Patterns. Позволяет быстро переключить макет вывода записей.

**Пример: двухколоночная сетка**

```php
<?php
/**
 * Title: Query: Две колонки
 * Slug: themeslug/query-two-columns
 * Categories: posts
 * Block Types: core/query
 * Viewport Width: 1376
 */
?>
<!-- wp:query {"queryId":0,"query":{"perPage":6,"pages":0,"offset":0,"postType":"post","order":"desc","orderBy":"date","author":"","search":"","exclude":[],"sticky":"","inherit":false},"align":"wide"} -->
<div class="wp-block-query alignwide">
  <!-- wp:post-template {"layout":{"type":"grid","columnCount":2}} -->
    <!-- wp:post-featured-image {"isLink":true,"aspectRatio":"16/9"} /-->
    <!-- wp:group {"style":{"spacing":{"padding":{"right":"var:preset|spacing|20","left":"var:preset|spacing|20"}}}} -->
    <div class="wp-block-group" style="padding-right:var(--wp--preset--spacing--20);padding-left:var(--wp--preset--spacing--20)">
      <!-- wp:post-title {"isLink":true} /-->
      <!-- wp:post-excerpt {"moreText":"<?php esc_html_e( 'Читать далее →', 'themeslug' ); ?>"} /-->
      <!-- wp:post-date /-->
    </div>
    <!-- /wp:group -->
  <!-- /wp:post-template -->
  <!-- wp:query-pagination -->
    <!-- wp:query-pagination-numbers /-->
  <!-- /wp:query-pagination -->
</div>
<!-- /wp:query -->
```

### Варианты Query Loop-паттернов

| Название | Описание | Колонок |
|----------|----------|---------|
| Query: Сетка 3 колонки | Карточки с featured image, title, excerpt | 3 |
| Query: Список | Горизонтальные записи с featured image слева | 1 |
| Query: Журнал | Первая запись на всю ширину, остальные — сетка | 2+1 |
| Query: Без изображений | Только заголовок и дата, компактный вид | 3 |

## Template Part Patterns

Template Parts тоже поддерживают Block Type Patterns. Полезно для Header и Footer:

```php
<?php
/**
 * Title: Header с логотипом по центру
 * Slug: themeslug/header-centered
 * Categories: header
 * Block Types: core/template-part
 */
?>
<!-- wp:group {"align":"full","layout":{"type":"constrained"}} -->
<div class="wp-block-group alignfull">
  <!-- wp:site-logo {"width":120,"align":"center"} /-->
  <!-- wp:site-title {"textAlign":"center"} /-->
  <!-- wp:navigation {"layout":{"type":"flex","justifyContent":"center"}} /-->
</div>
<!-- /wp:group -->
```

## Материалы и источники

- [Block Type Patterns — Theme Handbook (developer.wordpress.org)](https://developer.wordpress.org/themes/patterns/block-type-patterns/)

## Связанные страницы

- [Паттерны: обзор](./index.md)
- [Регистрация паттернов](./registering.md)
- [Стартовые паттерны](./starter.md)
