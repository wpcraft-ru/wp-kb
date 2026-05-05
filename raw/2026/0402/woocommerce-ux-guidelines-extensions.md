# WooCommerce Extension Guidelines — UX Design System

**Источник:** https://developer.woocommerce.com/docs/extensions/ux-guidelines-extensions/
**Дата:** актуально

---

## Общие принципы

Расширения WooCommerce должны использовать:
- Существующие WordPress/WooCommerce UI
- Встроенные компоненты (text fields, checkboxes, etc.)
- Существующие меню-структуры

Плагины, следующие дизайну WordPress core, выигрывают от будущих обновлений этого дизайна.

---

## Компонентные библиотеки

### WordPress Components Library
https://wordpress.github.io/gutenberg/?path=/story/docs-introduction--page

### Figma для WordPress
https://make.wordpress.org/design/2018/11/19/figma-for-wordpress/

### WooCommerce Component Library (Storybook)
https://woocommerce.github.io/woocommerce/

---

## Storybook Integration

WooCommerce использует Storybook для:
- Разработки UI-компонентов в изоляции (React)
- Тестирования generic JavaScript модулей без backend-зависимости
- Сборки переиспользуемых компонентов

Локальный запуск:
```bash
pnpm --filter=@woocommerce/storybook watch:build
```

Trunk-версия Storybook: https://woocommerce.github.io/woocommerce

---

## Рекомендации

1. Проверить текущий [WooCommerce setup experience](https://woocommerce.com/documentation/plugins/woocommerce/getting-started/)
2. Изучить [WordPress core plugin guidelines](https://developer.wordpress.org/plugins/wordpress-org/detailed-plugin-guidelines/)
3. Соблюдать [content style guide](https://woocommerce.com/document/grammar-punctuation-style-guide/)
4. Исключения из стандартного UI — только с валидным use case
