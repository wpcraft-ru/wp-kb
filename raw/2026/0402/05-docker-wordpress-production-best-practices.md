# Production WordPress + WooCommerce with Docker Compose — Best Practices

Источник: web_search — "docker compose wordpress woocommerce production setup"
Дата: 2026-05-05

## Архитектура для production

4 основных сервиса:
1. **Reverse Proxy** (Nginx) — единственная публичная точка входа
2. **WordPress** (PHP-FPM) — приложение
3. **MySQL/MariaDB** — база данных
4. **Кеширование** (Redis) — опционально

## Сетевая изоляция

- **Публичная сеть**: только proxy
- **Приватная сеть**: WordPress + БД + кеш
- Только порты 80/443 публикуются наружу
- DNS-коммуникация между сервисами внутри

## HTTPS и TLS

- TLS termination на reverse proxy
- Let's Encrypt + Certbot для автоматических сертификатов
- Force HTTPS redirect
- Security headers: X-Frame-Options, X-Content-Type-Options, CSP

## Персистентность

- Named volumes для БД (/var/lib/mysql) и wp-content (/var/www/html/wp-content)
- Named volumes безопаснее bind mounts для production
- Ежедневные бэкапы (7-30 restore points)
- Шифрование бэкапов

## Управление секретами

- .env файлы для sensitive values
- .env исключён из Git (.gitignore + .dockerignore)
- Сильные уникальные пароли

## PHP оптимизация

- OPcache включён
- Реалистичные memory limits
- Правильная конфигурация PHP-FPM workers

## Кеширование

- Page caching на уровне proxy
- Redis для object caching (для high-query сайтов)
- Кеширование статики с long expiration

## Resource limits

- CPU/memory limits per container
- Health checks (не только uptime, а реальные проверки)

## Pre-production checklist

- HTTPS активен и форсирован
- Только proxy порты публично
- Persistent volumes для БД и wp-content
- Автоматические бэкапы настроены и протестированы
- Staging окружение зеркалит production
- Rollback план задокументирован

## Update strategy

1. Тест на staging
2. Бэкап БД и wp-content перед обновлением
3. Деплой в low-traffic часы
4. Ре-тест критических функций (checkout, admin)
5. Документированный rollback

## Common pitfalls

- Публичный доступ к портам БД
- Использование "latest" image tag без версионирования
- Пропуск тестов восстановления из бэкапов
- Dev-настройки в production
