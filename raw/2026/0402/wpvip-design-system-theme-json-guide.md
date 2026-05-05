# Использование Design System с WordPress Block Editor: Theme.json

**Источник:** https://wpvip.com/blog/using-a-design-system-with-the-wordpress-block-editor-pt-1-theme-json/
**Авторы:** Alec Geatches, Gopal Krishnan (Automattic / WordPress VIP)
**Part 1 of 2**
**Дата:** ~2022 (статья)

---

## Что такое theme.json?

`theme.json` — конфигурационный файл для настройки и темизации блочного редактора, изменения настроек блоков и создания фундамента для полной блочной темы.

Позволяет контролировать глобальные настройки и стили всего WordPress-сайта.

---

## Хранение Design Tokens

Токены должны храниться в секции `settings.custom` theme.json:

```json
{
    "settings": {
        "custom": {
            "text": {
                "primary": "#00FF00",
                "background": "#000000"
            }
        }
    }
}
```

Ссылаться на токены можно через CSS-переменные:
```
var(--wp--custom--text--primary)
var(--wp--custom--text--background)
```

**Схема:** `var(--wp--custom–<путь к значению через -- как разделитель>)`

### Best Practices
1. Использовать WordPress-генерированные переменные токенов везде (не хардкодить значения)
2. Токены из `theme.json` доступны в CSS и за пределами theme.json — для блоков и других компонентов темы

---

## Настройка редактора и стилей блоков

### Ключевые секции theme.json:
- `settings` / `settings.blocks` — глобальные настройки редактора + индивидуальные для блоков
- `styles` / `styles.blocks` — предустановленные стили для всего редактора + для конкретных блоков

### Пример: настройка заголовков

```json
{
    "settings": {
        "blocks": {
            "core/heading": {
                "color": {
                    "text": true,
                    "palette": [
                        {
                            "slug": "primary",
                            "color": "var(--wp--custom--text--primary)",
                            "name": "Primary"
                        },
                        {
                            "slug": "secondary",
                            "color": "var(--wp--custom--text--secondary)",
                            "name": "Secondary"
                        }
                    ]
                }
            }
        }
    },
    "styles": {
        "blocks": {
            "core/heading": {
                "color": {
                    "text": "var(--wp--custom--text--primary)"
                }
            }
        }
    }
}
```

### Важно о стилизации
Используйте секции `styles` и `styles.blocks` для стилизации — **не используйте CSS-переопределения** для core block styles, т.к. обновления редактора часто ломают CSS-обходы.

### Шаблон для блокировки настроек

Глобально отключить все настройки (border, color, typography, spacing), а затем точечно включать для конкретных блоков:

```json
{
    "$schema": "https://schemas.wp.org/trunk/theme.json",
    "version": 2,
    "settings": {
        "appearanceTools": true,
        "border": {
            "color": false, "radius": false, "style": false, "width": false
        },
        "color": {
            "text": false, "background": false, "link": false,
            "custom": false, "defaultPalette": false,
            "defaultGradients": false, "defaultDuotone": false,
            "customGradient": false, "customDuotone": false,
            "palette": [],
            "gradients": []
        },
        "spacing": {
            "margin": false, "padding": false, "units": ["rem", "%"]
        },
        "typography": {
            "customFontSize": false, "fontStyle": false, "fontWeight": false,
            "letterSpacing": false, "lineHeight": false,
            "textDecoration": false, "textTransform": false,
            "dropCap": false, "fontFamilies": [], "fontSizes": []
        }
    }
}
```

---

## Вывод

`theme.json` — мощный инструмент для организации глобальных design tokens, установки глобальных стилей темы и кастомизации отдельных блоков. Использование всех возможностей блочного редактора делает реализацию design system централизованной и совместимой со встроенным редактором.

### Полезные ссылки
- [Part 2: Block Types and Styles](https://wpvip.com/2022/12/22/using-a-design-system-with-wordpress-block-types-styles/)
- [What is Theme.json?](https://developer.wordpress.org/themes/advanced-topics/theme-json/)
- [Global Settings & Styles (theme.json)](https://developer.wordpress.org/block-editor/how-to-guides/themes/theme-json/)
