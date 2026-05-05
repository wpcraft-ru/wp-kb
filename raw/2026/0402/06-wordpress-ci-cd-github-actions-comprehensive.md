# WordPress CI/CD Pipeline with GitHub Actions and WP-CLI

Источник: https://wpclimastery.com/blog/wordpress-ci-cd-pipeline-with-github-actions-and-wp-cli/
Дата: ~2024-2025

## CI/CD defined

**Continuous Integration (CI)**:
- Automated testing code changes
- Verify WordPress functionality
- Security checks
- Code quality

**Continuous Deployment (CD)**:
- Auto-deploy to staging
- Production deploy on approval
- Database migrations
- Zero-downtime deployments

## Репозиторий структура

```
wordpress-project/
├── .github/workflows/
│   ├── ci.yml
│   └── deploy.yml
├── wp-content/themes/my-theme/
├── wp-content/plugins/my-plugin/
├── scripts/
│   ├── deploy.sh
│   ├── test.sh
│   └── migrate.sh
├── tests/
│   └── test-wordpress.php
├── composer.json
```

## Что версионировать (✅) и что нет (❌)

- ✅ Custom themes, plugins, mu-plugins
- ✅ Configuration files, deployment scripts
- ❌ WordPress core (использовать Composer)
- ❌ Third-party plugins (управлять отдельно)
- ❌ wp-config.php
- ❌ uploads/
- ❌ .env

## Тестовое окружение в GitHub Actions

```yaml
services:
  mysql:
    image: mysql:5.7
    env:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: wordpress_test
    ports:
      - 3306:3306
    options: --health-cmd="mysqladmin ping" ...
```

### CI workflow (ci.yml)

1. Setup PHP (8.1) + WP-CLI
2. Установка WordPress в /tmp/wordpress
3. Копирование кода темы/плагина
4. Активация плагина
5. Запуск тестов

### Тесты (test.sh)

- Sanity: WP установлен, БД доступна, плагин активен
- Security: целостность ядра WP
- Performance: database query count
- Matrix testing: PHP 7.4/8.0/8.1 × WP 6.2/6.3/6.4

## Деплой

### SSH key setup для GitHub Actions

```bash
ssh-keygen -t ed25519 -C "github-actions@example.com"
ssh-copy-id user@server.com
```

Secrets: SSH_PRIVATE_KEY, SSH_HOST_STAGING/PRODUCTION, SSH_USER, WP_PATH

### Staging auto-deploy (на push в develop)

- Бэкап БД
- Git pull + reset --hard origin/develop
- Composer install --no-dev
- Database migrations
- Clear caches (wp cache flush, wp rewrite flush)
- Slack notification

### Production deploy (manual approval)

- Workflow dispatch или release
- Pre-deployment checks (curl staging)
- Maintenance mode activate
- Git pull + reset --hard origin/main
- Database migrations
- Cache clear
- Maintenance mode deactivate
- Smoke tests
- Rollback on failure (git reset --hard HEAD~1)

## Database migrations

Version-controlled SQL/Shell scripts:
```
migrations/001-create-custom-table.sql
migrations/002-add-meta-field.sh
```

## Rollback strategy

1. Capture current commit BEFORE deploy
2. Run deploy
3. Verify (curl + wp-cli checks)
4. On failure: restore БД backup + git reset --hard to previous commit

## Best practices

- Protected branches (require PR + status checks)
- GitHub Environments (staging/production с approvals)
- .env excluded from version control
- Always test on staging first
