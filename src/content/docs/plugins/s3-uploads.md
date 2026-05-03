---
title: "S3 Uploads (Human Made) — плагин для S3"
description: "Open-source плагин S3 Uploads от Human Made: вынос wp-content/uploads в S3 через wp-config и WP-CLI, без UI — для инженерных команд."
---

## Что такое S3 Uploads

S3 Uploads — это легковесный open-source плагин от [Human Made](https://github.com/humanmade/S3-Uploads), который полностью заменяет локальную директорию `wp-content/uploads` на S3-совместимое объектное хранилище. В отличие от WP Offload Media, у него практически нет графического интерфейса — вся настройка через `wp-config.php` и WP-CLI.

**Ключевая философия:** минимум «магии», максимум контроля. Идеален для команд, которые хотят предсказуемое поведение и не боятся конфигов.

## Что делает плагин

- Перехватывает загрузку и чтение медиафайлов через WordPress Media API
- Работает со всеми размерами миниатюр (thumbnails)
- Переписывает URL-ы на S3 (опционально, через конфиг)
- Поддерживает приватные файлы через **signed URL** (временные ссылки с подписью)
- Не трогает CSS/JS темы — работает только с `uploads`

## Когда подходит S3 Uploads

- **Высоконагруженные сайты** с несколькими веб-нодами (stateless)
- **Kubernetes / Docker Swarm / автоскейлинг**
- **Экономия на egress** — часто используют с Cloudflare R2 (ноль за исходящий трафик)
- **Приватные файлы** (курсы, LMS, документы) через signed URL
- **Миграции** — медиа не нужно копировать, достаточно подключить бакет
- **Локальная разработка** — плагин можно отключить одной константой

## Плюсы и ограничения

**Плюсы:**
- Полностью open-source (MIT-лицензия)
- Лёгкий — никакого лишнего кода и интерфейса
- WP-CLI для миграции и верификации
- Работает с любым S3-совместимым хранилищем через endpoint
- Предсказуемое поведение — никаких неожиданных побочных эффектов

**Ограничения:**
- Нет админки — всё через конфиги и консоль
- Нужно руками настроить IAM, бакет, права
- Нет встроенного CDN — нужно настраивать отдельно
- Для новичков проще WP Offload Media

## Установка и настройка

### 1. Создание бакета и IAM

```bash
# В консоли облачного провайдера:
# - Создать бакет (например, my-site-uploads)
# - Создать IAM-пользователя с правами s3:GetObject, s3:PutObject, s3:DeleteObject, s3:ListBucket
# - Записать Access Key ID и Secret Access Key
```

### 2. Установка плагина

Рекомендуется через Composer:

```bash
composer require humanmade/s3-uploads
```

Или скачать с [GitHub](https://github.com/humanmade/S3-Uploads) и поместить в `wp-content/plugins/s3-uploads/`, затем активировать:

```bash
wp plugin activate s3-uploads
```

### 3. Конфигурация в wp-config.php

```php
// Базовые константы (обязательные)
define('S3_UPLOADS_BUCKET', 'my-site-uploads');
define('S3_UPLOADS_REGION', 'eu-central-1'); // или ru-central1 для Yandex
define('S3_UPLOADS_KEY', 'AKIAIOSFODNN7EXAMPLE');
define('S3_UPLOADS_SECRET', 'wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY');

// Опционально
define('S3_UPLOADS_BUCKET_URL', 'https://my-site-uploads.s3.eu-central-1.amazonaws.com');
define('S3_UPLOADS_USE_INSTANCE_PROFILE', false); // для EC2 IAM-ролей
```

### 4. Не-AWS эндпоинты (Yandex, R2, MinIO)

Для S3-совместимых провайдеров, отличных от AWS, нужно задать endpoint через mu-plugin:

```php
// wp-content/mu-plugins/s3-endpoint.php
add_filter('s3_uploads_s3_client_params', function($params) {
    $params['endpoint'] = 'https://storage.yandexcloud.net'; // Yandex Object Storage
    // $params['endpoint'] = 'https://s3.selcdn.ru';          // Selectel
    // $params['endpoint'] = 'https://<account>.r2.cloudflarestorage.com'; // Cloudflare R2
    $params['use_path_style_endpoint'] = true;
    return $params;
});
```

### 5. Миграция существующих файлов

```bash
# Загрузить локальный uploads в бакет
wp s3-uploads upload-directory wp-content/uploads uploads --verbose

# Проверить целостность
wp s3-uploads verify

# Проверить, что плагин активен
wp s3-uploads is-installed
```

## Локальная разработка

Чтобы отключить S3 на локальной среде:

```php
if (defined('WP_ENV') && WP_ENV === 'local') {
    define('S3_UPLOADS_USE_LOCAL', true);
}
```

## Вывод

**S3 Uploads — выбор для инженерных команд**, которым нужен контроль, предсказуемость и работа через CLI. Минимум зависимостей, максимум гибкости.

Если нужен графический интерфейс и настройка в пару кликов — смотрите [WP Offload Media](./wp-offload-media.md). Полное сравнение — [Сравнение плагинов для S3](../faq/s3-plugins-comparison.md).

## Материалы и источники

- [Перенос файлов WordPress в S3‑совместимые хранилища с помощью плагина S3 Uploads (Human Made)](https://wpcraft.ru/blog/s3-uploads-human-made)
- [S3 Uploads на GitHub](https://github.com/humanmade/S3-Uploads)
