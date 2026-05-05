---
title: "CI/CD для WordPress с GitHub Actions"
description: "Автоматизация тестирования и деплоя WordPress: CI/CD пайплайны через GitHub Actions, WP-CLI, матричное тестирование и деплой без FTP."
---

## CI/CD для WordPress

**Continuous Integration (CI):** автоматическое тестирование при каждом push — проверка кода, security, совместимость с версиями PHP/WP.

**Continuous Deployment (CD):** автоматический деплой после прохождения тестов — на staging или production.

## Структура репозитория

```
wordpress-project/
├── .github/workflows/
│   ├── ci.yml          # Тестирование при PR и push
│   └── deploy.yml      # Деплой на staging/production
├── wp-content/
│   ├── themes/my-theme/
│   └── plugins/my-plugin/
├── scripts/
│   ├── deploy.sh
│   ├── test.sh
│   └── migrate.sh
├── tests/
│   └── test-wordpress.php
├── composer.json
└── .env.example
```

### Что версионировать (✅) и что нет (❌)

- ✅ Кастомные темы, плагины, mu-plugins
- ✅ Конфигурационные файлы, скрипты деплоя
- ❌ Ядро WordPress (подключать через Composer)
- ❌ Сторонние плагины (управлять через Composer или отдельно)
- ❌ `wp-config.php`, `.env` (в каждом окружении свои)
- ❌ `uploads/` (медиафайлы)

## Простой деплой темы (без FTP)

Минимальный workflow: пуш в main → деплой на сервер:

```yaml
# .github/workflows/deploy.yml
name: Deploy WordPress Theme

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Build assets
        run: |
          npm ci
          npm run build

      - name: Deploy Theme to Server
        uses: SamKirkland/FTP-Deploy-Action@4.3.3
        with:
          server: ${{ secrets.FTP_SERVER }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          server-dir: /wp-content/themes/my-theme/
```

**GitHub Secrets** (Settings → Secrets and variables → Actions):
- `FTP_SERVER` — адрес сервера
- `FTP_USERNAME` — FTP/SFTP пользователь
- `FTP_PASSWORD` — пароль

## Полноценный CI с wp-env

```yaml
# .github/workflows/ci.yml
name: CI - WordPress Tests

on:
  pull_request:
  push:
    branches: [main, develop]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        php: ['7.4', '8.0', '8.1', '8.2']
        wordpress: ['6.3', '6.4', '6.5']

    services:
      mysql:
        image: mysql:8.0
        env:
          MYSQL_ROOT_PASSWORD: root
          MYSQL_DATABASE: wordpress_test
        ports:
          - 3306:3306
        options: >-
          --health-cmd="mysqladmin ping --silent"
          --health-interval=10s
          --health-timeout=5s
          --health-retries=3

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup PHP
        uses: shivammathur/setup-php@v2
        with:
          php-version: ${{ matrix.php }}
          extensions: mysql, xml, mbstring

      - name: Setup WP-CLI
        run: |
          curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
          chmod +x wp-cli.phar
          sudo mv wp-cli.phar /usr/local/bin/wp

      - name: Install WordPress
        run: |
          wp core download --path=/tmp/wordpress --version=${{ matrix.wordpress }}
          wp config create --dbname=wordpress_test --dbuser=root --dbpass=root --dbhost=127.0.0.1 --path=/tmp/wordpress
          wp core install --url=http://localhost --title=Test --admin_user=admin --admin_password=password --admin_email=test@test.com --path=/tmp/wordpress

      - name: Copy & Activate Plugin
        run: |
          cp -r wp-content/plugins/my-plugin /tmp/wordpress/wp-content/plugins/
          wp plugin activate my-plugin --path=/tmp/wordpress

      - name: Run Tests
        run: bash scripts/test.sh
```

### Матричное тестирование

Матрица `php × wordpress` тестирует код на 16 комбинациях (4 PHP × 4 WP) параллельно. Это находит проблемы совместимости до того, как они попадут к пользователям.

### Тесты: что проверять

```bash
#!/bin/bash
# scripts/test.sh

# Sanity: WordPress установлен
wp core is-installed --path=/tmp/wordpress || exit 1

# Плагин активен
wp plugin is-active my-plugin --path=/tmp/wordpress || exit 1

# Проверка целостности ядра
wp core verify-checksums --path=/tmp/wordpress

# PHP CodeSniffer
vendor/bin/phpcs --standard=WordPress wp-content/plugins/my-plugin/

# PHPUnit (если есть)
vendor/bin/phpunit
```

## Деплой по SSH

Более безопасный, чем FTP-деплой — SSH с ключами:

```yaml
# .github/workflows/deploy-ssh.yml
name: Deploy via SSH

on:
  push:
    branches: [production]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Deploy via rsync
        uses: burnett01/rsync-deployments@6.0.0
        with:
          switches: -avzr --delete
          path: wp-content/themes/my-theme/
          remote_path: /var/www/site/wp-content/themes/my-theme/
          remote_host: ${{ secrets.DEPLOY_HOST }}
          remote_user: ${{ secrets.DEPLOY_USER }}
          remote_key: ${{ secrets.DEPLOY_SSH_KEY }}
```

## Staging и Production окружения

```yaml
name: Deploy

on:
  push:
    branches:
      - staging
      - production

jobs:
  deploy-staging:
    if: github.ref == 'refs/heads/staging'
    # ... deploy to staging server

  deploy-production:
    if: github.ref == 'refs/heads/production'
    # ... manual approval, then deploy
    environment: production
```

Staging деплоится автоматически, production — через environment protection rules (manual approval).

## Преимущества CI/CD перед ручным FTP

| Ручной FTP | CI/CD |
|-----------|-------|
| Минуты на загрузку | Секунды |
| Человеческий фактор, ошибки | Идентичный процесс каждый раз |
| «Я забыл какие файлы менял» | Вся история в Git |
| Откат = загрузка старых файлов наугад | Откат = revert commit |
| Нет тестов | Автоматическое тестирование |

## Материалы и источники

- [WP-CLI Mastery: CI/CD Pipeline with GitHub Actions](https://wpclimastery.com/blog/wordpress-ci-cd-pipeline-with-github-actions-and-wp-cli/)
- [Mark Ward Design: Stop Using FTP — WordPress CI/CD with GitHub Actions](https://markwarddesign.com/2025/08/04/stop-using-ftp-a-guide-to-wordpress-ci-cd-with-github-actions/)
- [Automattic: DevOps for WordPress](https://automattic.com/for-agencies/blog/wordpress-devops/)
