---
name: wiki-faq
description: Создание и поддержка FAQ-страниц в вики: быстрые ответы на основе существующих wiki-страниц и queries/-артефактов, без gap analysis.
---

# Wiki FAQ

Создаёт короткие FAQ-страницы, дающие прямые ответы на вопросы — в отличие от `wiki-query`, который исследует пробелы.

## Отличие от wiki-query

| | wiki-query | wiki-faq |
|---|---|---|
| Формат | «Что покрыто / что нет» + ingest-предложения | Прямой ответ + ссылки на вики |
| Куда сохраняет | `queries/` | `faq/` |
| Gap analysis | Обязателен | Нет |
| Для кого | Для развития вики | Для читателя |

## Pre-condition

1. Read `src/content/docs/index.md`
2. Read relevant wiki pages
3. If a `queries/` page exists on the topic — use it as research foundation
4. Read related `faq/` pages to avoid duplication

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
