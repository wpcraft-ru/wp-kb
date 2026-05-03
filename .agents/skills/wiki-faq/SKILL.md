---
name: wiki-faq
description: Создание и поддержка FAQ-страниц в вики: быстрые ответы на основе существующих wiki-страниц, без gap analysis.
---

# Wiki FAQ

Создаёт короткие FAQ-страницы — прямые ответы на вопросы со ссылками на вики.

## Pre-condition

1. Read `src/content/docs/index.md`
2. Read relevant wiki pages
3. Read related `faq/` pages to avoid duplication

## Research: Validate (Perplexity через OpenRouter)

Для каждого FAQ — проверить факты через Perplexity Sonar (краулит веб, возвращает ответ с источниками):

```bash
curl -s https://openrouter.ai/api/v1/chat/completions \
  -H "Authorization: Bearer $OPENROUTER_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "perplexity/sonar",
    "messages": [
      {"role": "system", "content": "Валидатор фактов. Отвечай кратко со ссылками. Язык: русский."},
      {"role": "user", "content": "Актуальная цена домена .ru на reg.ru?"}
    ]
  }' | jq -r '.choices[0].message.content'
```

**Модели через OpenRouter:**
| Model ID | Что делает |
|---|---|
| `perplexity/sonar` | Быстрый поиск, ссылки в тексте |
| `perplexity/sonar-pro` | Глубокий поиск с дополнительными источниками |
| `perplexity/sonar-reasoning` | Поиск + reasoning для сложных сравнений |

### Что проверять

1. **Цены и тарифы** — хостинги/регистраторы часто меняют стоимость
2. **Версии ПО** — актуальный PHP, WordPress, MySQL
3. **Конкретные шаги** — не изменился ли интерфейс/процесс
4. **Альтернативы** — есть ли новые плагины/сервисы, не упомянутые в вики

### Результат

- `content` → добавить как уточнение в FAQ-ответ
- Ссылки из ответа → добавить в «Материалы и источники»

> **Требуется:** `PERPLEXITY_API_KEY` в переменных окружения. Модели: `sonar` (быстрый) или `sonar-pro` (глубокий поиск). OpenCrawl встроен — Perplexity сам краулит веб при `search`-режиме.

## Process

1. **Answer concisely** — 3-5 предложений основной мысли, без gap analysis
2. **Add how-to steps** — если вопрос процедурный (ordered list)
3. **Add comparison table** — если вопрос сравнительный
4. **Link to wiki** — 2-5 ссылок на релевантные страницы
5. **Create page** in `faq/` with frontmatter:
   - `title` = вопрос (например, «Как установить WordPress?»)
   - `description` = ответ в одно предложение
   - Content: answer → steps/table → links → «Материалы и источники»
   - Do NOT add `# H1` heading — Starlight renders `title` as H1
6. **Update** `index.md` — add entry in FAQ section
7. **Append** `log.md`: `## [YYYY-MM-DD] faq | <question>`

## FAQ Page Format

```markdown
---
title: "Вопрос?"
description: "Короткий ответ в одно предложение."
---

## Краткий ответ

2-4 предложения сути. Без gap analysis, без «что покрыто».

## Как сделать (если how-to)

1. Шаг 1
2. Шаг 2
3. Шаг 3

## Сравнение (если comparison)

| Критерий | Вариант А | Вариант Б |
|---|---|---|
| ... | ... | ... |

## Материалы и источники

- [Страница вики](../path/to/page.md)
- [Оригинальная статья](https://...)
```

## Quality Bar

- Ru-RU
- Коротко: страница не длиннее 80 строк
- Прямой ответ, без gap analysis
- Ссылки на wiki-страницы обязательны
- Frontmatter обязателен (`title`, `description`)
- Не дублировать существующие faq-страницы

## Done Criteria

- FAQ page created in `faq/`
- `index.md` updated
- `log.md` appended
