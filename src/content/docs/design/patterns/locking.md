---
title: "Block Locking и паттерны"
description: "Использование Block Locking API в паттернах WordPress: запрет перемещения, удаления, вставки блоков, content-only editing для курирования опыта редактирования."
---

## Что такое Block Locking

[Block Locking API](https://developer.wordpress.org/block-editor/how-to-guides/curating-the-editor-experience/block-locking/) позволяет контролировать взаимодействие пользователя с блоками. В контексте паттернов — это способ **защитить дизайн** от случайных изменений.

## Типы блокировок

### Block-level locking (на уровне отдельного блока)

| Атрибут | Эффект |
|---------|--------|
| `"move":true` | Блок нельзя перемещать |
| `"remove":true` | Блок нельзя удалить |

### Template-level locking (на уровне контейнера)

Задаётся на групповом блоке (Group, Cover, Columns). Работает для всех вложенных блоков:

| Атрибут | Эффект |
|---------|--------|
| `"lock":{"move":true,"remove":true}` | Все вложенные блоки зафиксированы |
| `"templateLock":"insert"` | Нельзя добавлять новые блоки внутрь |
| `"templateLock":"contentOnly"` | Можно редактировать только контент и медиа |

## Block Locking: запрет перемещения и удаления

```php
<?php
/**
 * Title: Событие (с блокировкой изображения)
 * Slug: themeslug/event-locked
 * Categories: banner
 * Viewport Width: 1376
 */
?>
<!-- wp:group {"layout":{"type":"constrained"}} -->
<div class="wp-block-group">
  <!-- wp:columns -->
  <div class="wp-block-columns">
    <!-- wp:column -->
    <div class="wp-block-column">
      <!-- wp:image {"lock":{"move":true,"remove":true}} -->
      <figure class="wp-block-image">
        <img src="<?php echo esc_url( get_theme_file_uri( 'assets/images/event.jpg' ) ); ?>" alt=""/>
      </figure>
      <!-- /wp:image -->
    </div>
    <!-- /wp:column -->

    <!-- wp:column -->
    <div class="wp-block-column">
      <!-- wp:heading -->
      <h2><?php esc_html_e( 'Расположение:', 'themeslug' ); ?></h2>
      <!-- /wp:heading -->
      <!-- wp:paragraph -->
      <p><?php esc_html_e( '82 Main St. Brooklyn, NY', 'themeslug' ); ?></p>
      <!-- /wp:paragraph -->

      <!-- wp:heading -->
      <h2><?php esc_html_e( 'Дата:', 'themeslug' ); ?></h2>
      <!-- /wp:heading -->
      <!-- wp:paragraph -->
      <p><?php esc_html_e( '24 октября 2021', 'themeslug' ); ?></p>
      <!-- /wp:paragraph -->

      <!-- wp:button -->
      <div class="wp-block-button">
        <a class="wp-block-button__link wp-element-button">
          <?php esc_html_e( 'Купить билеты', 'themeslug' ); ?>
        </a>
      </div>
      <!-- /wp:button -->
    </div>
    <!-- /wp:column -->
  </div>
  <!-- /wp:columns -->
</div>
<!-- /wp:group -->
```

Атрибут `"lock":{"move":true,"remove":true}` на `wp:image` запрещает перемещение и удаление изображения.

## Template Locking: защита целых секций

### insert — запрет добавления новых блоков

```php
<!-- wp:group {"templateLock":"insert","layout":{"type":"constrained"}} -->
<div class="wp-block-group">
  <!-- wp:heading -->
  <h2><?php esc_html_e( 'Наша команда', 'themeslug' ); ?></h2>
  <!-- /wp:heading -->
  <!-- wp:paragraph -->
  <p><?php esc_html_e( 'Описание команды.', 'themeslug' ); ?></p>
  <!-- /wp:paragraph -->
</div>
<!-- /wp:group -->
```

Пользователь может редактировать существующие блоки, но не может добавить новые внутрь `wp:group`.

### contentOnly — только контент и медиа

```php
<!-- wp:group {"templateLock":"contentOnly","layout":{"type":"constrained"}} -->
<div class="wp-block-group">
  <!-- wp:heading -->
  <h2><?php esc_html_e( 'Заголовок', 'themeslug' ); ?></h2>
  <!-- /wp:heading -->
  <!-- wp:image -->
  <figure class="wp-block-image">
    <img src="<?php echo esc_url( get_theme_file_uri( 'assets/images/photo.jpg' ) ); ?>" alt=""/>
  </figure>
  <!-- /wp:image -->
  <!-- wp:paragraph -->
  <p><?php esc_html_e( 'Редактируемый текст.', 'themeslug' ); ?></p>
  <!-- /wp:paragraph -->
</div>
<!-- /wp:group -->
```

Content-only editing позволяет менять **только текст и медиа**, но не структуру, цвета или отступы. Идеально для клиентских проектов, где дизайн должен оставаться неизменным.

### all — полная блокировка

```php
<!-- wp:group {"templateLock":"all","layout":{"type":"constrained"}} -->
<div class="wp-block-group">
  <!-- ... зафиксированные блоки ... -->
</div>
<!-- /wp:group -->
```

Ни перемещать, ни удалять, ни добавлять новые блоки.

## Блокировка через UI и через код

Блокировки, заданные в коде паттерна, можно переопределить через UI (⋮ → Lock). Для клиентских проектов может потребоваться ограничить доступ к UI блокировок — см. [Change permissions to control locking ability](https://developer.wordpress.org/block-editor/how-to-guides/curating-the-editor-experience/block-locking/#change-permissions-to-control-locking-ability).

## Когда что использовать

| Ситуация | Тип блокировки |
|----------|---------------|
| Защита конкретного изображения в паттерне | Block-level `remove` + `move` |
| Фиксированная структура секции, контент редактируемый | Template `contentOnly` |
| Запрет добавления блоков в контейнер | Template `insert` |
| Полностью зафиксированная секция | Template `all` |
| Header, Footer — от случайного удаления | Block-level `remove` |

## Материалы и источники

- [Patterns and Block Locking — Theme Handbook (developer.wordpress.org)](https://developer.wordpress.org/themes/patterns/patterns-and-block-locking/)
- [Block Locking API — Block Editor Handbook (developer.wordpress.org)](https://developer.wordpress.org/block-editor/how-to-guides/curating-the-editor-experience/block-locking/)

## Связанные страницы

- [Паттерны: обзор](./index.md)
- [Регистрация паттернов](./registering.md)
- [Паттерны в шаблонах](./templates.md)
