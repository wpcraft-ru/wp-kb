# WP Knowledge

Персональная база знаний по WordPress на основе паттерна [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) Андрея Карпаты.

## Стек

- **SSG:** Astro + Starlight
- **Хостинг:** GitHub Pages
- **Поиск:** Pagefind (локальный) → Algolia DocSearch (после одобрения)
- **Управление:** OpenClaw skill `llm-wiki`

## Структура

```
wp-knowledge/
├── raw/                    # Immutable источники
├── src/content/docs/       # Wiki pages (LLM пишет и поддерживает)
├── AGENTS.md               # LLM Wiki schema
├── astro.config.mjs        # Конфиг сайта
└── .github/workflows/      # CI/CD
```

## Команды

```bash
npm install
npm run dev       # Локальный сервер
npm run build     # Сборка сайта
npm run preview   # Предпросмотр сборки
```

## Лицензия

MIT
