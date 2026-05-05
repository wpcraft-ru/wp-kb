---
title: "Design Tokens в theme.json"
description: "Как хранить и использовать дизайн-токены в WordPress через theme.json: custom-секция, CSS-переменные, цветовая палитра, типографика и spacing."
---

## Что такое Design Tokens

Design tokens — именованные переменные, хранящие значения визуальных свойств: цветов, отступов, типографики, скруглений. В WordPress они хранятся в кастомной секции `theme.json` и доступны как CSS-переменные.

**Три слоя токенов (по W3C DTCG):**

| Слой | Описание | Пример |
|------|----------|--------|
| **Option tokens** (primitive) | Сырая палитра, базовые значения | `blue-500: #78bbfa` |
| **Decision tokens** (semantic) | Как и где применять | `color-accent: {color.option.blue-900}` |
| **Component tokens** | Контекст для конкретных компонентов | `button-primary-bg: {color.decision.accent}` |

## Хранение токенов в theme.json

**Всегда используйте секцию `settings.custom`** — это стандартный подход WordPress:

```json
{
  "settings": {
    "custom": {
      "color": {
        "primary": "#2563EB",
        "secondary": "#7C3AED",
        "accent": "#F59E0B"
      },
      "text": {
        "primary": "#111827",
        "secondary": "#6B7280",
        "inverse": "#FFFFFF"
      },
      "spacing": {
        "xs": "4px",
        "sm": "8px",
        "md": "16px",
        "lg": "24px",
        "xl": "32px",
        "2xl": "48px"
      },
      "border": {
        "radius": {
          "sm": "4px",
          "md": "8px",
          "lg": "16px"
        }
      },
      "typography": {
        "fontSize": {
          "sm": "0.875rem",
          "base": "1rem",
          "lg": "1.125rem",
          "xl": "1.25rem",
          "2xl": "1.5rem",
          "3xl": "2rem"
        }
      }
    }
  }
}
```

## Использование токенов через CSS-переменные

WordPress автоматически генерирует CSS-переменные из custom-секции:

```json
{
  "styles": {
    "color": {
      "text": "var(--wp--custom--text--primary)",
      "background": "var(--wp--custom--color--secondary)"
    },
    "elements": {
      "heading": {
        "color": {
          "text": "var(--wp--custom--text--primary)"
        }
      },
      "button": {
        "color": {
          "text": "var(--wp--custom--text--inverse)",
          "background": "var(--wp--custom--color--primary)"
        },
        "border": {
          "radius": "var(--wp--custom--border--radius--md)"
        }
      }
    }
  }
}
```

**Схема именования:** `var(--wp--custom--<путь с -- как разделителем>)`
- `settings.custom.color.primary` → `--wp--custom--color--primary`
- `settings.custom.spacing.lg` → `--wp--custom--spacing--lg`

Токены доступны также в кастомном CSS, вне `theme.json`:
```css
.my-custom-class {
  color: var(--wp--custom--color--primary);
  margin-bottom: var(--wp--custom--spacing--lg);
}
```

## Цветовая палитра

Минимальный набор цветов для дизайн-системы WordPress:

```json
{
  "settings": {
    "custom": {
      "palette": {
        "primary": {
          "50": "#EFF6FF",
          "500": "#3B82F6",
          "900": "#1E3A5F"
        },
        "neutral": {
          "50": "#F9FAFB",
          "900": "#111827"
        },
        "feedback": {
          "success": "#10B981",
          "warning": "#F59E0B",
          "error": "#EF4444",
          "info": "#3B82F6"
        }
      }
    }
  }
}
```

**Требования WCAG AA:**
- Основной текст на фоне: контраст ≥ 4.5:1
- Крупный текст (≥ 18px bold): контраст ≥ 3:1
- Интерактивные элементы: контраст ≥ 3:1

WordPress поддерживает градиенты в `theme.json`:
```json
{
  "styles": {
    "color": {
      "gradient": "linear-gradient(135deg, var(--wp--custom--palette--primary--500), var(--wp--custom--palette--primary--900))"
    }
  }
}
```

## Типографика

```json
{
  "settings": {
    "custom": {
      "typography": {
        "heading": {
          "fontFamily": "'Inter', sans-serif",
          "fontWeight": "700",
          "lineHeight": "1.2"
        },
        "body": {
          "fontFamily": "'Inter', sans-serif",
          "fontWeight": "400",
          "lineHeight": "1.6"
        }
      },
      "fontSize": {
        "scale": {
          "sm": "0.875rem",
          "base": "1rem",
          "lg": "1.125rem",
          "xl": "1.25rem",
          "2xl": "1.5rem",
          "3xl": "1.875rem",
          "4xl": "2.25rem"
        }
      }
    }
  }
}
```

**Рекомендации:**
- Используйте `rem`-единицы (16px = 1rem) — это сохраняет масштабируемость
- Поддерживайте RTL/LTR через WordPress-механизмы
- Задавайте адаптивную типографику через `clamp()` или медиа-запросы

## Spacing scale

Два подхода к шкале отступов:

**Мультипликативная** (удвоение): `0, 0.13, 0.25, 0.5, 1, 2, 4, 8 rem`

**Аддитивная** (накопление): `0, 1, 3, 5, 7, 9, 11, 13 rem`

В WordPress spacing определяется через `settings.spacing.spacingScale`:

```json
{
  "settings": {
    "spacing": {
      "spacingScale": {
        "steps": 7,
        "operator": "*",
        "increment": 1.5
      }
    }
  }
}
```

## Lock Down Approach (VIP)

Глобально заблокировать все элементы управления, затем выборочно включить нужные:

```json
{
  "settings": {
    "appearanceTools": false,
    "border": {
      "color": false,
      "radius": false,
      "style": false,
      "width": false
    },
    "color": {
      "text": false,
      "background": false,
      "link": false,
      "custom": false
    },
    "spacing": {
      "margin": false,
      "padding": false
    },
    "typography": {
      "customFontSize": false,
      "fontStyle": false,
      "fontWeight": false
    },
    "blocks": {
      "core/button": {
        "color": {
          "text": true,
          "background": true
        }
      }
    }
  }
}
```

Это гарантирует, что контент-редакторы не нарушат дизайн-систему произвольными цветами и отступами.

## Best Practices

1. **Используйте сгенерированные WordPress-переменные везде** — не сырые значения
2. **Храните токены только в `settings.custom`** — единый source of truth
3. **Не дублируйте токены в CSS** — используйте `var(--wp--custom--...)`
4. **Ограничьте редактора** — lock down лишние настройки для контент-менеджеров

## Материалы и источники

- [WordPress VIP: Design System with Block Editor — Theme.json](https://wpvip.com/blog/using-a-design-system-with-the-wordpress-block-editor-pt-1-theme-json/)
- [rtCamp: Design Tokens in WordPress Site Editor](https://rtcamp.com/handbook/designing-for-wordpress-site-editor/design-tokens/)
- [DDpa: Design as Code — Design Tokens и архитектура](https://ddpa.ru/kb/ai/design/design-as-code/)
- [W3C Design Tokens Community Group](https://www.w3.org/community/design-tokens/)
- [Contentful: Design Tokens Explained](https://www.contentful.com/blog/design-tokens-explained/)
