# How to run WordPress + WooCommerce Locally with Docker

Источник: https://dev.to/daniloab/how-to-run-wordpress-woocommerce-locally-with-docker-42cd
Дата: ~2023

## docker-compose.yml

```yaml
version: "3.9"
services:
  db:
    image: mysql:5.7
    volumes:
      - ./db_data:/var/lib/mysql
    restart: always
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
      - ./wordpress:/var/www/html
      - ./plugins:/var/www/html/wp-content/plugins
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
```

## Локальные volume

- `./db_data` — MySQL данные
- `./wordpress` — WP файлы
- `./plugins` — плагины (для разработки)

## Запуск

```bash
docker compose up -d
```

## Установка WooCommerce

1. Dashboard > Appearance > Themes > Add New > "Storefront"
2. Dashboard > Plugins > Add New > "WooCommerce"

## Репозиторий

- https://github.com/daniloab/docker-wordpress
