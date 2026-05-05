# Exporting Design System Tokens From Figma to WordPress

**Источник:** https://wpvip.com/blog/figma-to-wordpress/
**Авторы:** Alec Geatches, Gopal Krishnan (Automattic / WordPress VIP)
**Дата:** ~2022

---

## Проблема

При ведении сайтов в масштабе сложно поддерживать консистентность дизайна между Figma (дизайн-система) и кодом. Изменения в Figma рассинхронизируются с реализацией в WordPress.

## Решение: vip-design-system-bridge

Open-source tool [vip-design-system-bridge](https://github.com/Automattic/vip-design-system-bridge), который синхронизирует DS из Figma с WordPress темами.

### Цепочка работы

```
Figma → Figma Tokens Plugin → GitHub Storage → vip-design-system-bridge → theme.json
```

1. Дизайн-система определена в Figma-документе
2. **Figma Tokens plugin** ([ссылка](https://www.figma.com/community/plugin/843461159747178978)) — определяет design tokens
3. **GitHub storage** Figma Tokens → токены экспортируются в репозиторий
4. **vip-design-system-bridge** → вставляет DT в WordPress через `theme.json` custom section
5. В коде и CSS используются WordPress-generated `--wp--custom` классы

### Преимущество
При изменении токенов в Figma → синхронизация через Figma Tokens → запуск bridge tool → автоматическое обновление в теме без правок кода/CSS.

---

## Пример: пошаговая инструкция

### 1. Figma: Design System
- Копия [Material 3 Design Kit Figma Template](https://www.figma.com/file/5NZf8UfaZCPhcZRTjpRfmX/Material-3-Design-Kit---Figma-Tokens-Example)
- Figma Tokens → URL storage → указать на tokens.json

### 2. Два типа токен-сетов:
- **global** — полная цветовая палитра и типографика
- **material-3-color** и **material-3-text** — именованные токены конкретного использования

### 3. Экспорт
Изменить токен → Export → tokens.json

### 4. WordPress: Локальный запуск
```bash
vip dev-env create --slug=token-site
vip dev-env start --slug=token-site
vip dev-env exec --slug=token-site -- wp theme activate token-theme
```

### 5. Применение токенов
```bash
node ingest-tokens.js \
  --tokenPath=~/Downloads/tokens.json \
  --themePath=./docs/design-tokens-example/token-site/themes/token-theme \
  --sourceSet=global \
  --layerSets=material-3-color,material-3-text \
  --overwrite
```

---

## Ключевые параметры bridge tool

- `--tokenPath` — путь к tokens.json из Figma
- `--themePath` — путь к теме WordPress
- `--sourceSet` — какой сет токенов использовать как источник (исключается из итогового вывода)
- `--layerSets` — какие сеты формируют итоговый вывод
- `--overwrite` — перезаписывать существующий theme.json

Инструмент использует:
- **token-transformer** — обработка токенов
- **Style Dictionary** — сборка в WordPress-совместимый формат

---

## Полезные ссылки

- [VIP Design System Bridge репозиторий](https://github.com/Automattic/vip-design-system-bridge)
- [Figma Tokens Plugin](https://www.figma.com/community/plugin/843461159747178978)
- [Figma Tokens GitHub Storage](https://docs.tokens.studio/sync/github)
- [Пример tokens.json](https://github.com/Automattic/vip-design-system-bridge/blob/trunk/docs/design-tokens-example/tokens.json)
- [Пример token-темы](https://github.com/Automattic/vip-design-system-bridge/tree/trunk/docs/design-tokens-example/token-site/themes/token-theme)
