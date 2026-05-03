---
title: "WP Offload Media — плагин для S3 и CDN"
description: "Самый популярный плагин для выноса медиа WordPress в Amazon S3, DigitalOcean Spaces и Google Cloud Storage. Lite-версия бесплатно, Pro с CDN и ассетами."
---

## Что такое WP Offload Media

WP Offload Media (ранее WP Offload S3) от [DeliciousBrains](https://deliciousbrains.com/wp-offload-media/) — самый популярный плагин для выноса медиафайлов WordPress в облачное хранилище. В отличие от [S3 Uploads](./s3-uploads.md), имеет полноценный графический интерфейс в админке.

**Две версии:**
- **Lite** (бесплатно) — базовый offload в S3, URL-rewriting
- **Pro** (платно, от $50/год) — CDN, offload CSS/JS/шрифтов, существующая медиатека, multisite

## Поддерживаемые хранилища

- Amazon S3
- Amazon CloudFront (CDN)
- DigitalOcean Spaces
- Google Cloud Storage
- Любое S3-совместимое через кастомный endpoint

## Ключевые возможности

| Функция | Lite | Pro |
|---------|:----:|:---:|
| Offload новых файлов в S3 | ✅ | ✅ |
| URL-rewriting (замена ссылок) | ✅ | ✅ |
| Удаление локальных копий | ✅ | ✅ |
| Offload существующей медиатеки | ❌ | ✅ |
| Offload CSS, JS, шрифтов | ❌ | ✅ |
| CDN-интеграция (CloudFront) | ❌ | ✅ |
| Multisite | ❌ | ✅ |
| WP-CLI команды | ❌ | ✅ |

## Быстрая настройка (Lite)

### 1. Создание IAM-пользователя в AWS

1. Зайти в [AWS Console → IAM → Users](https://console.aws.amazon.com/iamv2/home#/users)
2. **Create user** → имя (например `wp-offload`)
3. **Attach policies directly** → выбрать `AmazonS3FullAccess`
4. Создать access key: **Security credentials** → **Create access key** → **Application running outside AWS**
5. Скачать `.csv` с Access Key ID и Secret Access Key

### 2. Установка и активация

```bash
# Через админку: Plugins → Add New → "WP Offload Media Lite"
# Или WP-CLI:
wp plugin install amazon-s3-and-cloudfront --activate
```

### 3. Настройка в админке

1. **WP Offload Media → Settings**
2. Выбрать **Amazon S3** как Storage Provider
3. Ввести Access Key ID и Secret Access Key
4. Выбрать бакет (создастся автоматически или выбрать существующий)
5. Настроить URL: либо S3-URL, либо свой домен/CDN

### 4. Определение ключей через wp-config (рекомендуется)

```php
define('AS3CF_SETTINGS', serialize(array(
    'provider' => 'aws',
    'access-key-id' => 'AKIAIOSFODNN7EXAMPLE',
    'secret-access-key' => 'wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY',
)));
```

Это безопаснее, чем хранить ключи в базе данных.

### 5. Offload существующей медиатеки (Pro)

В Pro-версии: **WP Offload Media → Media Library → Offload Now** — массовый перенос всех существующих файлов в S3.

## Настройка CDN (CloudFront)

1. В AWS Console создать CloudFront Distribution
2. Указать S3-бакет как Origin
3. В WP Offload Media Settings → **Delivery** → включить CloudFront
4. Указать CloudFront Domain (например `d123.cloudfront.net`)

После этого URL-ы медиафайлов будут заменены на CDN-домен — изображения отдаются с ближайшего edge-узла.

## Лучшие практики

- **IAM-роль вместо ключей** — если сайт на AWS EC2, используйте IAM-роль с привязанной политикой. Не храните ключи в базе
- **CDN обязателен** — S3 не оптимизирован для отдачи конечным пользователям. CloudFront или Cloudflare снижают latency и стоимость egress
- **Оптимизация изображений до offload** — сжимайте изображения плагином (EWWW, Smush) до отправки в S3
- **Мониторинг затрат** — следите за egress-трафиком в AWS Billing. S3-хранение дёшево, исходящий трафик — нет
- **Cloudflare R2 как альтернатива** — бесплатный egress, совместим с S3 API

## Типовые ошибки

| Ошибка | Причина | Решение |
|--------|---------|---------|
| Изображения не грузятся | Бакет приватный | Включить public access на бакете |
| CORS-ошибки в админке | Нет CORS-заголовков | Настроить CORS на бакете |
| 403 Forbidden из S3 | Неверные права IAM | Проверить IAM-политику |
| Двойное списание за egress | Нет CDN | Настроить CloudFront |

## Вывод

WP Offload Media — золотой стандарт для S3-интеграции с WordPress. Lite-версия закрывает 80% потребностей, Pro добавляет CDN и массовый offload. Лучший выбор, если нужен UI и минимум ручной настройки.

Сравнение с другими плагинами — [Сравнение плагинов для S3](../faq/s3-plugins-comparison.md). Альтернатива для инженерных команд — [S3 Uploads](./s3-uploads.md).

## Материалы и источники

- [Amazon S3 Quick Start Guide — DeliciousBrains](https://deliciousbrains.com/wp-offload-media/doc/amazon-s3-quick-start-guide/)
- [WP Offload Media Lite на WordPress.org](https://wordpress.org/plugins/amazon-s3-and-cloudfront/)
- [How to Offload WordPress Media to AWS S3 — Alex Rusin Blog](https://blog.alexrusin.com/how-to-offload-wordpress-media-to-aws-s3/)
- [AWS S3 Bucket Creation — AWS Whitepaper](https://docs.aws.amazon.com/whitepapers/latest/best-practices-wordpress/amazon-s3-bucket-creation.html)
