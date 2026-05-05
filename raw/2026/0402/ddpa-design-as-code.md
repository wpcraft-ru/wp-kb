---
source: https://ddpa.ru/kb/ai/design/design-as-code/
fetched: 2026-05-05
---

# Design as Code — дизайн-токены и архитектура

Design as Code — подход, при котором дизайн-решения представляются как версионированные файлы кода (JSON), которые автоматически генерируют стили для всех платформ. Design tokens служат единым source of truth между дизайнерами и разработчиками, устраняя ручной handoff.

Ядро подхода: дизайн-решения → JSON tokens → Style Dictionary → CSS/Swift/Kotlin/документация.

Это upstream-парадигма по отношению к [DESIGN.md](../design-md) — если DESIGN.md адаптирует дизайн-систему для AI-агентов, то Design as Code охватывает полный цикл от дизайна в Figma до продакшен-кода на всех платформах.

## Design Tokens

Именованные переменные, хранящие значения цветов, отступов, типографики, z-index и других визуальных свойств. W3C Design Tokens Community Group (DTCG) разрабатывает официальную спецификацию формата.

| Слой | Описание | Пример |
|------|----------|--------|
| Option tokens (primitive) | Сырая палитра, базовые значения | blue-500: #78bbfa |
| Decision tokens (semantic) | Как и где применять | color-accent: {color.options.blue-900} |
| Component tokens | Контекст для конкретных компонентов | button-primary-bg: {color.decisions.accent} |

Токены ссылаются друг на друга через синтаксис {path.to.token}:

```json
{
  "color": {
    "options": { "blue-900": "#0D47A1" },
    "decisions": {
      "accent": "{color.options.blue-900}",
      "text-primary": "{color.options.gray-900}"
    }
  }
}
```

## Пайплайн

Рабочий пайплайн из 5 стадий (по Thoughtworks):

| Стадия | Инструменты |
|--------|-------------|
| Check | Линтинг токенов, валидация references |
| Build | Style Dictionary → CSS/Sass/iOS/Android |
| Test | Storybook визуальные регрессии |
| Publish | npm-пакет, обновление документации |
| Notify | Slack/Teams уведомления командам |

## Инструменты

| Инструмент | Назначение | Ключевое |
|-----------|-----------|---------|
| Tokens Studio | Figma-плагин для design tokens | Bidirectional Git-синхронизация, W3C DTCG-формат |
| Style Dictionary (Amazon) | Трансформация токенов → код | CSS, Sass, Android XML, iOS Swift |
| Figma Make | AI-генерация кода из фреймов | HTML, CSS, JS, React из Figma frames |
| Builder.io | Mapping Figma → code components | Визуальное редактирование → production code |
| Storybook | Тестирование UI + visual regression | Документация + тесты компонентов |

## Методология (Contentful)

От простого к сложному: один primary color → stepping system → semantic tokens → interactive states → component tokens.

## Когда оправдан

- Есть несколько платформ (web + iOS + Android)
- Есть multiple themes (dark/light, white-label)
- Дизайн-система переиспользуется несколькими командами
- Частота дизайн-изменений высока

## Ссылки

- Martin Fowler: Design Token-Based UI Architecture (Thoughtworks)
- Contentful: Design Tokens Explained
- Browser London: Do Design Tokens Really Make Design Systems Smarter?
- W3C Design Tokens Community Group
- Tokens Studio + Style Dictionary
