---
title: "Docker для WordPress"
description: "Локальная разработка и production-развёртывание WordPress через Docker Compose: Nginx, PHP-FPM, MySQL, Redis, HTTPS, бэкапы и best practices."
---

## Локальная разработка: WordPress + WooCommerce

Для разработки плагинов и тем WordPress с WooCommerce — один `docker-compose.yml`:

```yaml
services:
  db:
    image: mysql:8.0
    volumes:
      - db_data:/var/lib/mysql
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    volumes:
      - ./wp-content:/var/www/html/wp-content
    ports:
      - "8000:80"
    restart: unless-stopped
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_CONFIG_EXTRA: |
        define('WP_DEBUG', true);
        define('WP_DEBUG_LOG', true);

volumes:
  db_data:
```

**После старта** (`docker compose up -d`) — установить WooCommerce через Plugins → Add New → «WooCommerce», активировать тему Storefront.

## Production-архитектура

Четыре сервиса с сетевой изоляцией:

```
       ┌──────────────────────┐
       │   Nginx Reverse      │
       │   Proxy (80/443)     │ ← единственная точка входа
       └──────┬───────────────┘
              │
    ┌─────────┴───────────────┐ приватная сеть
    │         │               │
┌───▼────┐ ┌──▼──────┐ ┌─────▼──────┐
│ PHP-FPM│ │  MySQL  │ │   Redis    │
│   WP   │ │         │ │ (опционально)│
└────────┘ └─────────┘ └────────────┘
```

## Production docker-compose.yml

```yaml
services:
  proxy:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/snippets:/etc/nginx/snippets:ro
      - certbot_www:/var/www/certbot
      - certbot_conf:/etc/letsencrypt
    depends_on:
      - wordpress
    restart: unless-stopped
    networks:
      - public
      - private

  wordpress:
    image: wordpress:6.4-php8.2-fpm-alpine
    volumes:
      - wordpress_data:/var/www/html
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: ${WORDPRESS_DB_USER}
      WORDPRESS_DB_PASSWORD: ${WORDPRESS_DB_PASSWORD}
      WORDPRESS_DB_NAME: ${WORDPRESS_DB_NAME}
      WORDPRESS_CONFIG_EXTRA: |
        define('WP_CACHE_KEY_SALT', '${SITE_DOMAIN}');
    restart: unless-stopped
    networks:
      - private

  db:
    image: mysql:8.0
    volumes:
      - db_data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${WORDPRESS_DB_NAME}
      MYSQL_USER: ${WORDPRESS_DB_USER}
      MYSQL_PASSWORD: ${WORDPRESS_DB_PASSWORD}
    restart: unless-stopped
    networks:
      - private

  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data
    restart: unless-stopped
    networks:
      - private

networks:
  public:
  private:
    internal: true
```

### .env (не коммитить в Git!)

```bash
MYSQL_ROOT_PASSWORD=strong_root_pass_here
WORDPRESS_DB_NAME=wordpress
WORDPRESS_DB_USER=wordpress
WORDPRESS_DB_PASSWORD=strong_user_pass_here
SITE_DOMAIN=example.com
```

## HTTPS с Let's Encrypt

Nginx конфигурация с TLS-termination и автоматическим получением сертификатов через Certbot:

```nginx
server {
    listen 80;
    server_name example.com;
    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }
    location / {
        return 301 https://$host$request_uri;
    }
}

server {
    listen 443 ssl http2;
    server_name example.com;

    ssl_certificate     /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    # Security headers
    add_header X-Frame-Options "SAMEORIGIN" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;

    root /var/www/html;
    index index.php;

    location / {
        try_files $uri $uri/ /index.php?$args;
    }

    location ~ \.php$ {
        fastcgi_pass wordpress:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    # Кеширование статики
    location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg|woff2?)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

## Управление секретами

- **`.env` файл** — чувствительные значения, исключён из Git (`.gitignore`)
- **Docker Secrets** — для Docker Swarm
- **Сильные уникальные пароли** — не `wordpress`/`wordpress`

## PHP-FPM оптимизация

```ini
; php.ini (кастомный или через environment)
opcache.enable=1
opcache.memory_consumption=256
opcache.max_accelerated_files=20000
opcache.revalidate_freq=2
memory_limit=256M
max_execution_time=300
upload_max_filesize=64M
post_max_size=64M
```

## Кеширование

| Уровень | Инструмент | Что кеширует |
|---------|-----------|-------------|
| Страничное | Nginx fastcgi_cache | HTML-страницы |
| Объектное | Redis | Результаты запросов к БД, опции |
| Статика | Nginx expires | CSS, JS, изображения |
| OPcache | PHP OPcache | Скомпилированный PHP-код |

## Бэкапы

- **Named volumes** для БД и wp-content (безопаснее bind mounts)
- Ежедневные бэкапы базы: `mysqldump` по cron + загрузка в S3
- **Не хранить бэкапы на том же сервере** — использовать внешнее S3-хранилище
- Тестировать восстановление из бэкапов регулярно

## Pre-production чек-лист

- [ ] HTTPS активен и форсирован
- [ ] Только proxy-порты (80/443) публично доступны
- [ ] `.env` исключён из Git
- [ ] Persistent volumes для БД и wp-content
- [ ] Автоматические бэкапы настроены и протестированы
- [ ] Staging-окружение зеркалит production
- [ ] План отката задокументирован
- [ ] Resource limits на контейнеры проставлены
- [ ] Health checks на все сервисы

## Частые ошибки

- **Публичный доступ к портам БД** — MySQL не должен быть открыт наружу
- **Тег `latest`** — внезапные обновления ломают сайт, используйте конкретную версию
- **Dev-настройки в production** — `WP_DEBUG=true`, `display_errors=on`
- **Пропуск тестов восстановления** — бэкап без проверки восстановления — не бэкап
- **Бэкапы на том же диске** — при сбое хостинга теряется всё

## Материалы и источники

- [dev.to: WordPress + WooCommerce Locally with Docker](https://dev.to/daniloab/how-to-run-wordpress-woocommerce-locally-with-docker-42cd)
- [Docker Compose WordPress production best practices (web search, 2026)](https://docs.docker.com/compose/wordpress/)
- [DDpa: Infrastructure as Code](https://ddpa.ru/kb/it/devops/infrastructure-as-code/)
