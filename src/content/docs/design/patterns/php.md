---
title: "PHP в паттернах"
description: "Использование PHP в блочных паттернах WordPress: интернационализация, динамические URL изображений, foreach-циклы, ограничения хука init."
---

## Когда выполняется PHP в паттернах

Паттерны регистрируются на хуке **`init`**. В этот момент WordPress компилирует PHP-содержимое паттерна в HTML-разметку блоков и сохраняет её. При рендеринге (в редакторе или на фронте) паттерн — уже чистый HTML.

> **Ключевое ограничение:** На `init` недоступны глобальный запрос, пост, таксономии. Функции `is_home()`, `is_single()`, `get_post()`, `the_title()` — **не работают** в паттернах.

```php
<?php if ( is_page() ) : ?>
  <?php the_title(); ?>
<?php endif; ?>
```
Этот код **не выполнится корректно** — на `init` нет глобального контекста поста.

Если нужен PHP на этапе рендера — используйте [Block Bindings API](https://developer.wordpress.org/news/tag/block-bindings/), но это за рамками паттернов.

## Интернационализация текста

Главное преимущество PHP в паттернах — **i18n**. В отличие от HTML-шаблонов, текст в паттернах можно обернуть в функции перевода:

```php
<!-- wp:heading {"level":1} -->
<h1><?php esc_html_e( 'Добро пожаловать на мой сайт', 'themeslug' ); ?></h1>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p><?php esc_html_e( 'Это мой маленький уголок в интернете.', 'themeslug' ); ?></p>
<!-- /wp:paragraph -->

<!-- wp:button -->
<div class="wp-block-button">
  <a class="wp-block-button__link wp-element-button">
    <?php esc_html_e( 'Узнать больше', 'themeslug' ); ?>
  </a>
</div>
<!-- /wp:button -->
```

**Функции i18n, доступные в паттернах:**
- `esc_html_e()` — вывести и эскейпнуть
- `esc_html__()` — вернуть и эскейпнуть
- `_e()`, `__()` — без эскейпинга (только для доверенного текста)

## Изображения и ассеты

Динамические URL через `get_theme_file_uri()`:

```php
<!-- wp:cover {"url":"<?php echo esc_url( get_theme_file_uri( 'assets/images/hero-bg.jpg' ) ); ?>","dimRatio":50} -->
<div class="wp-block-cover">
  <span aria-hidden="true" class="wp-block-cover__background has-background-dim"></span>
  <div class="wp-block-cover__inner-container">
    <!-- wp:heading -->
    <h2><?php esc_html_e( 'Заголовок', 'themeslug' ); ?></h2>
    <!-- /wp:heading -->
  </div>
</div>
<!-- /wp:cover -->
```

Для изображений внутри `wp:image`:

```php
<!-- wp:image {"id":123,"src":"<?php echo esc_url( get_theme_file_uri( 'assets/images/logo.png' ) ); ?>"} -->
<figure class="wp-block-image">
  <img src="<?php echo esc_url( get_theme_file_uri( 'assets/images/logo.png' ) ); ?>" alt="<?php esc_attr_e( 'Логотип', 'themeslug' ); ?>"/>
</figure>
<!-- /wp:image -->
```

## Циклы foreach()

PHP-циклы позволяют генерировать повторяющиеся структуры без дублирования кода:

```php
<?php
$features = [
    [
        'title' => __( 'Быстро', 'themeslug' ),
        'text'  => __( 'Мгновенная загрузка страниц.', 'themeslug' ),
    ],
    [
        'title' => __( 'Безопасно', 'themeslug' ),
        'text'  => __( 'Шифрование и защита данных.', 'themeslug' ),
    ],
    [
        'title' => __( 'Удобно', 'themeslug' ),
        'text'  => __( 'Интуитивный интерфейс.', 'themeslug' ),
    ],
];
?>

<!-- wp:columns -->
<div class="wp-block-columns">
  <?php foreach ( $features as $feature ) : ?>
    <!-- wp:column -->
    <div class="wp-block-column">
      <!-- wp:heading {"level":3} -->
      <h3><?php echo esc_html( $feature['title'] ); ?></h3>
      <!-- /wp:heading -->
      <!-- wp:paragraph -->
      <p><?php echo esc_html( $feature['text'] ); ?></p>
      <!-- /wp:paragraph -->
    </div>
    <!-- /wp:column -->
  <?php endforeach; ?>
</div>
<!-- /wp:columns -->
```

## Доступные PHP-функции

| Доступно | Недоступно |
|----------|-----------|
| `esc_html_e()`, `__()`, `_e()` | `is_home()`, `is_single()`, `is_page()` |
| `esc_url()`, `esc_attr()` | `get_post()`, `the_title()`, `the_content()` |
| `get_theme_file_uri()` | `get_the_ID()`, `get_queried_object()` |
| `wp_kses_post()` | `have_posts()`, `the_post()` |
| `home_url()`, `site_url()` | `wp_nav_menu()` |
| Циклы `foreach()`, массивы | Глобальный `$post`, `$wp_query` |

## Материалы и источники

- [Using PHP in Patterns — Theme Handbook (developer.wordpress.org)](https://developer.wordpress.org/themes/patterns/using-php-in-patterns/)

## Связанные страницы

- [Паттерны: обзор](./index.md)
- [Регистрация паттернов](./registering.md)
- [Паттерны в шаблонах](./templates.md)
