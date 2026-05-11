---
title: "Регистрация паттернов"
description: "Как регистрировать и разрегистрировать блочные паттерны в теме WordPress: /patterns-директория, PHP-функция register_block_pattern(), file headers, категории."
---

## Создание паттерна в админке

Перед регистрацией в теме паттерн удобно создать через админку:

1. **Внешний вид → Редактор сайта → Паттерны → + (Создать паттерн)**
2. Заполните: **Название**, **Категории**, флажок **Синхронизированный** (для темы — выключить)
3. В редакторе соберите композицию из блоков
4. **Сохранить**

Чтобы скопировать код паттерна: **⋮ → Копировать все блоки**. Полученный block markup — это HTML-разметка блоков, которую вы вставите в файл темы.

## File Header паттерна

Каждый файл в `/patterns/` начинается с PHP-комментария с метаданными:

```php
<?php
/**
 * Title: Hero
 * Slug: themeslug/hero
 * Categories: banner, featured
 * Description: Hero-секция с заголовком, текстом и кнопкой
 * Viewport Width: 1376
 * Inserter: true
 * Keywords: hero, баннер, главный экран
 * Block Types: core/cover
 * Post Types: page, post
 * Template Types: front-page, home
 */
?>
<!-- wp:group {"layout":{"type":"constrained"}} -->
<!-- ... блоки паттерна ... -->
<!-- /wp:group -->
```

### Обязательные поля

| Поле | Описание |
|------|----------|
| **`Title`** | Человекочитаемое название (переводимое) |
| **`Slug`** | Уникальный slug в формате `namespace/pattern-name` |
| **`Categories`** | Одна или несколько категорий через запятую |

### Опциональные поля

| Поле | Назначение |
|------|-----------|
| **`Description`** | Подробное описание (только для скринридеров) |
| **`Viewport Width`** | Ширина viewport для превью (в px), обычно 1376 |
| **`Inserter`** | Показывать ли в инсертере. По умолчанию `true`. `no`/`false` скрывает |
| **`Keywords`** | Ключевые слова для поиска в инсертере, через запятую |
| **`Block Types`** | Привязка к типам блоков: `core/cover`, `core/query`, `core/post-content` |
| **`Post Types`** | Ограничение по типам записей: `page`, `post` |
| **`Template Types`** | Привязка к типам шаблонов: `front-page`, `home`, `single` |

## Регистрация через /patterns

WordPress автоматически регистрирует любой файл из `/patterns/` с валидным file header. Это **рекомендуемый способ**.

Структура файла:

```php
<?php
/**
 * Title: Hero
 * Slug: themeslug/hero
 * Categories: featured
 */
?>
<!-- wp:cover {"url":"<?php echo esc_url( get_theme_file_uri( 'assets/images/hero-bg.jpg' ) ); ?>","dimRatio":50} -->
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

## Регистрация через PHP

Для условной регистрации используется `register_block_pattern()`:

```php
add_action( 'init', function() {
    register_block_pattern(
        'themeslug/hero',
        [
            'title'       => __( 'Hero', 'themeslug' ),
            'categories'  => [ 'featured' ],
            'content'     => '
                <!-- wp:cover {"dimRatio":50} -->
                <div class="wp-block-cover">
                    <span aria-hidden="true" class="wp-block-cover__background has-background-dim"></span>
                    <div class="wp-block-cover__inner-container">
                        <!-- wp:heading {"level":1} -->
                        <h1>' . esc_html__( 'Добро пожаловать', 'themeslug' ) . '</h1>
                        <!-- /wp:heading -->
                    </div>
                </div>
                <!-- /wp:cover -->
            ',
        ]
    );
} );
```

PHP-метод удобен для **условной регистрации** — например, показать разные паттерны в зависимости от активных плагинов.

## Разрегистрация паттернов

### Удаление core-паттернов

```php
add_action( 'after_setup_theme', function() {
    remove_theme_support( 'core-block-patterns' );
} );
```

### Отключение удалённых паттернов (с wordpress.org/patterns)

```php
add_filter( 'should_load_remote_block_patterns', '__return_false' );
```

## Категории паттернов

WordPress предопределяет категории: `featured`, `banner`, `header`, `footer`, `gallery`, `text`, `call-to-action`, `posts`, `wireframe`.

### Регистрация своей категории

```php
add_action( 'init', function() {
    register_block_pattern_category(
        'hero-sections',
        [ 'label' => __( 'Hero-секции', 'themeslug' ) ]
    );
} );
```

### Удаление категории

```php
add_action( 'init', function() {
    unregister_block_pattern_category( 'wireframe' );
} );
```

## Практика: оборачивание в контейнер

Хорошая практика — оборачивать весь паттерн в Group или Cover. Это позволяет пользователю перемещать паттерн целиком и добавлять CSS-класс к внешнему блоку:

```php
<?php
/**
 * Title: Hero
 * Slug: themeslug/hero
 * Categories: featured
 */
?>
<!-- wp:group {"className":"my-hero-pattern","layout":{"type":"constrained"}} -->
<div class="wp-block-group my-hero-pattern">
  <!-- wp:heading -->
  <h2><?php esc_html_e( 'Заголовок', 'themeslug' ); ?></h2>
  <!-- /wp:heading -->
  <!-- wp:paragraph -->
  <p><?php esc_html_e( 'Описание.', 'themeslug' ); ?></p>
  <!-- /wp:paragraph -->
</div>
<!-- /wp:group -->
```

## Материалы и источники

- [Registering Patterns — Theme Handbook (developer.wordpress.org)](https://developer.wordpress.org/themes/patterns/registering-patterns/)

## Связанные страницы

- [Паттерны: обзор](./index.md)
- [PHP в паттернах](./php.md)
- [Паттерны в шаблонах](./templates.md)
- [Стартовые паттерны](./starter.md)
