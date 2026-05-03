---
title: "S3-хранилища для WordPress: зачем и как"
description: "Обзорная страница: почему WordPress-сайтам нужно S3-совместимое объектное хранилище, какие проблемы это решает и какие провайдеры доступны."
---

## Зачем WordPress нужно объектное хранилище

Стандартная архитектура WordPress хранит все медиафайлы (изображения, видео, документы) в `wp-content/uploads/` на том же сервере, где работает PHP. Это создаёт три проблемы:

1. **Диск раздувается** — чем больше медиа, тем дороже бэкапы и миграции
2. **Нельзя масштабировать горизонтально** — несколько веб-нод не могут работать с одной `uploads/` без общего хранилища
3. **Сервер тратит ресурсы на отдачу статики** — вместо генерации динамического контента

Вынос статики в S3-совместимое объектное хранилище решает все три проблемы и открывает путь к **stateless-архитектуре** — необходимому условию для автоскейлинга и Kubernetes.

## Что даёт S3-хранилище

| Проблема | Решение через S3 |
|----------|-----------------|
| Раздувание диска | Медиа хранятся в облаке, сервер — только код и БД |
| Масштабирование | Несколько веб-нод читают медиа из одного бакета |
| Скорость отдачи | S3 + CDN отдают статику быстрее, чем сервер |
| Бэкапы | Медиа не нужно бэкапить вместе с сервером |
| Надёжность | S3 обеспечивает 99.999999999% durability (11 девяток) |

## Архитектура: как это работает

```
Пользователь → CDN (CloudFront/Cloudflare) → S3-бакет
                    ↑
WordPress-сервер → плагин offload → загружает медиа в бакет
                                   → переписывает URL на CDN/S3
```

Плагин offload перехватывает загрузку файлов в Media Library и отправляет их напрямую в S3-бакет. URL-ы в контенте заменяются на URL бакета или CDN. Опционально локальные копии удаляются — сервер остаётся лёгким.

## Ключевые концепции

- **Бакет (Bucket)** — контейнер для хранения объектов в S3. Аналог папки `uploads/`
- **IAM-пользователь/роль** — учётная запись с правами доступа к бакету (Access Key + Secret Key)
- **Egress-трафик** — исходящий трафик из бакета к пользователям. **Это платный ресурс**
- **Signed URL** — временная ссылка с подписью для доступа к приватным файлам
- **CDN Origin** — S3-бакет как источник для CDN (CloudFront, Cloudflare, Selectel CDN)

## Плагины для offload в S3

Подробное сравнение — на странице [Сравнение плагинов для S3](./s3-plugins-comparison.md). Кратко:

| Плагин | Подход | Цена |
|--------|--------|------|
| [S3 Uploads (Human Made)](./s3-uploads.md) | WP-CLI + wp-config | Бесплатно |
| [WP Offload Media](./wp-offload-media.md) | UI в админке | Lite бесплатно / Pro платно |
| Media Cloud | Автоматизация, Imgix | Freemium |
| Next3 Offload | Простой UI + оптимизация | Платный |

## Провайдеры S3-хранилищ

Полный обзор — на странице [S3-провайдеры](../components/services/s3-providers.md). Ключевые:

**Российские:**
- **Yandex Object Storage** — лучшие бесплатные лимиты, 1 ГБ хранения + 100 ГБ трафика/мес
- **Selectel Object Storage** — низкие цены на холодное хранение, интеграция с Selectel CDN
- **VK Cloud Storage** — классы Hotbox/Icebox/Backup

**Международные:**
- **Amazon S3** — стандарт индустрии, 500+ интеграций
- **Cloudflare R2** — ноль за egress, совместим с S3 API
- **DigitalOcean Spaces** — простой и предсказуемый биллинг

## Когда S3 не нужен

- Маленький сайт-визитка (до 100 МБ медиа)
- Shared-хостинг без возможности установки плагинов offload
- Сайт без перспективы роста трафика

Но даже для среднего блога с 1–2 ГБ изображений вынос в S3 заметно ускоряет TTFB и снижает нагрузку на хостинг.

## Материалы и источники

- [Перенос файлов WordPress в S3‑совместимые хранилища с помощью плагина S3 Uploads (Human Made)](https://wpcraft.ru/blog/s3-uploads-human-made)
- [AWS Best Practices: Static Content Offload](https://docs.aws.amazon.com/whitepapers/latest/best-practices-wordpress/static-content-offload.html)
- [How to Offload WordPress Media to AWS S3 — Alex Rusin Blog](https://blog.alexrusin.com/how-to-offload-wordpress-media-to-aws-s3/)
- [Effortless Ways to Store Images on S3 — Infinite Uploads](https://infiniteuploads.com/blog/effortless-ways-to-wordpress-store-images-on-s3-for-better-performance/)
- [How to Upload WordPress Media Library to AWS S3 — Next3 Offload](https://next3offload.com/blog/upload-wordpress-media-library-to-aws-s3/)
