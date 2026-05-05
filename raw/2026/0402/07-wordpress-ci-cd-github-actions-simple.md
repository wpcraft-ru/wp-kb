# Stop Using FTP: A Guide to WordPress CI/CD with GitHub Actions

Источник: https://markwarddesign.com/2025/08/04/stop-using-ftp-a-guide-to-wordpress-ci-cd-with-github-actions/
Дата: 2025-08-04

## Основная идея

Автоматизация деплоя через GitHub Actions вместо ручного FTP/SFTP.

## Простой деплой темы

```yaml
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
        uses: actions/checkout@v3

      - name: Deploy Theme to Server
        uses: SamKirkland/FTP-Deploy-Action@4.3.3
        with:
          server: ${{ secrets.FTP_SERVER }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          server-dir: /wp-content/themes/your-theme-name/
```

## GitHub Secrets (Settings > Secrets and variables > Actions)

- `FTP_SERVER` — адрес сервера
- `FTP_USERNAME` — SFTP/FTP пользователь
- `FTP_PASSWORD` — SFTP/FTP пароль

## Расширения

- Build steps для CSS/JS (npm run build)
- Cache clearing
- Staging и production окружения с разными ветками

## Преимущества

- Скорость: секунды вместо минут
- Надёжность: одинаковый процесс каждый раз
- Безопасность: вся история в Git, rollback = revert commit
- Профессиональный workflow
