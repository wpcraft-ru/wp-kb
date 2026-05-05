---
title: "Figma → WordPress: мост через Design Tokens"
description: "Синхронизация дизайн-системы из Figma в WordPress: vip-design-system-bridge, Tokens Studio, Style Dictionary и автоматическое обновление theme.json."
---

## Проблема

При масштабной работе с сайтами сложно поддерживать консистентность между дизайном в Figma и реализацией в WordPress. Изменения в Figma вносятся дизайнерами, но разработчикам приходится вручную переносить цвета, отступы и типографику в `theme.json`. Результат — рассинхронизация и дрейф дизайна.

## Решение: пайплайн Figma → WordPress

```
Figma
  │
  └─→ Tokens Studio (плагин)
        │
        └─→ GitHub (tokens.json)
              │
              └─→ vip-design-system-bridge
                    │
                    ├─→ theme.json (custom section)
                    └─→ CSS-переменные (--wp--custom--*)
```

## Инструменты

### 1. Tokens Studio (Figma-плагин)

[Tokens Studio](https://www.figma.com/community/plugin/843461159747178978) — профессиональный менеджер design tokens внутри Figma:

- Создание иерархических сетов токенов (global, theme-specific, component-level)
- Двусторонняя Git-синхронизация (изменения в Figma → PR в GitHub, и наоборот)
- Поддержка W3C DTCG-формата
- Валидация references между токенами

**Два типа сетов:**
- **global** — полная палитра и базовые значения (не попадают в theme.json)
- **semantic / component** — именованные токены конкретных компонентов (попадают в theme.json)

### 2. vip-design-system-bridge

[Open-source инструмент](https://github.com/Automattic/vip-design-system-bridge) от WordPress VIP (Automattic), преобразующий tokens.json из Figma в WordPress-совместимый `theme.json`:

```bash
node ingest-tokens.js \
  --tokenPath=~/Downloads/tokens.json \
  --themePath=./wp-content/themes/my-theme \
  --sourceSet=global \
  --layerSets=material-3-color,material-3-text \
  --overwrite
```

**Ключевые параметры:**
- `--tokenPath` — путь к tokens.json из Figma
- `--themePath` — путь к теме WordPress
- `--sourceSet` — сет-источник (глобальная палитра, исключается из финального вывода)
- `--layerSets` — сеты, формирующие финальный вывод (через запятую)
- `--overwrite` — перезаписать существующий theme.json

Внутри инструмент использует:
- **token-transformer** — обработка токенов из Figma-формата
- **Style Dictionary** — сборка в WordPress-совместимый формат с `--wp--custom--*` переменными

### 3. Style Dictionary (Amazon)

Трансформирует абстрактные токены в платформо-специфичный код:

- CSS (CSS-переменные)
- Sass (SCSS-переменные)
- Android (XML resources)
- iOS (Swift)
- WordPress (`theme.json` через плагин)

## Пошаговая инструкция

### Шаг 1: Создать дизайн-систему в Figma

Настроить цвета, типографику, отступы как токены в Tokens Studio.

### Шаг 2: Настроить Git-синхронизацию

В Tokens Studio: Settings → Sync → GitHub → указать репозиторий и путь. Токены будут автоматически пушиться при изменениях.

### Шаг 3: Настроить bridge в CI/CD

```yaml
# .github/workflows/sync-tokens.yml
name: Sync Design Tokens

on:
  push:
    paths:
      - 'tokens/**'

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install bridge
        run: |
          git clone https://github.com/Automattic/vip-design-system-bridge.git
          cd vip-design-system-bridge && npm ci

      - name: Apply tokens to theme
        run: |
          node vip-design-system-bridge/ingest-tokens.js \
            --tokenPath=./tokens/tokens.json \
            --themePath=./wp-content/themes/my-theme \
            --sourceSet=global \
            --layerSets=theme-light,theme-components \
            --overwrite

      - name: Commit updated theme.json
        run: |
          git config user.name "Design Bot"
          git config user.email "bot@example.com"
          git add wp-content/themes/my-theme/theme.json
          git commit -m "chore: sync design tokens from Figma" || true
          git push
```

### Шаг 4: Использовать токены в теме

В `theme.json` и кастомном CSS используем сгенерированные переменные:

```json
{
  "styles": {
    "color": {
      "text": "var(--wp--custom--color--text--primary)",
      "background": "var(--wp--custom--color--surface--primary)"
    }
  }
}
```

```css
/* style.css */
.hero-section {
  background: var(--wp--custom--color--surface--accent);
  padding: var(--wp--custom--spacing--2xl) var(--wp--custom--spacing--lg);
}
```

### Шаг 5: Итерация

Дизайнер меняет токен в Figma → Tokens Studio пушит в GitHub → CI/CD запускает bridge → theme.json обновляется автоматически. Без ручных правок кода.

## Альтернативы VIP Bridge

| Инструмент | Подход | Для кого |
|-----------|--------|----------|
| **VIP Bridge** | Прямая конвертация tokens.json → theme.json | Enterprise на WordPress VIP |
| **Style Dictionary** (ручная настройка) | Программируемая трансформация | Кастомные пайплайны |
| **Figma Make** (AI) | Генерация кода из фреймов | Быстрое прототипирование |
| **Builder.io** | Визуальное редактирование → production code | Headless-решения |

## Когда пайплайн Figma → WordPress оправдан

- Команда из дизайнеров и разработчиков работает параллельно
- Частота изменений дизайна высока
- Есть несколько сайтов/тем, использующих общую дизайн-систему
- Нужна гарантия консистентности между макетом и продакшеном

Для одиночного проекта с редкими изменениями — ручной перенос токенов в `theme.json` может быть достаточен.

## Материалы и источники

- [WordPress VIP: Exporting Design System Tokens From Figma to WordPress](https://wpvip.com/blog/figma-to-wordpress/)
- [vip-design-system-bridge (GitHub)](https://github.com/Automattic/vip-design-system-bridge)
- [Tokens Studio for Figma](https://www.figma.com/community/plugin/843461159747178978)
- [DDpa: Design as Code — Design Tokens и архитектура](https://ddpa.ru/kb/ai/design/design-as-code/)
- [Style Dictionary by Amazon](https://amzn.github.io/style-dictionary/)
