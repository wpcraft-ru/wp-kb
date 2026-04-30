---
title: "WordPress 101: базовая карта терминов и действий"
description: "Конспект WordPress 101: базовые сущности WordPress, как начать, где настраивать сайт и как развивать практику."
---

Источник: `raw/wordpress-101-updated-260427.pdf`

## Ключевые тезисы

- WordPress рассматривается как универсальная CMS для сайтов разных типов.
- Стартовая тройка: домен, хостинг, установленный WordPress.
- Ключевые сущности новичка: Dashboard, Pages/Posts, Plugins, Themes, Customizer.
- Не изменять WordPress Core напрямую; расширять через темы, плагины и hooks.

## Быстрый чеклист первого запуска

1. Войти в `your-site/wp-admin`.
2. Создать базовые страницы (`About`, `Contact`, `Privacy Policy`).
3. Настроить постоянные ссылки.
4. Поставить 2-4 обязательных плагина (backup, security, cache/optimization).
5. Проверить роли пользователей и права доступа.

## PHP пример: короткий шорткод для контактного блока

```php
add_shortcode('kb_contact_note', function () {
    return '<p><strong>Контакты:</strong> используйте страницу Contact для заявок.</p>';
});
```

Что делает код:
- Добавляет управляемый контентный элемент без правки ядра.
- Позволяет повторно использовать блок на страницах и в записях.

## JS пример: клиентская валидация простого поля

```js
function validateEmailInput(value) {
  return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value);
}

console.log(validateEmailInput('hello@example.com'));
```

Что делает код:
- Демонстрирует минимальную проверку формата email.
- Подходит для базовой фронтенд-валидации пользовательского ввода.

## Связанные страницы

- [Getting Started with WordPress: структурный конспект](./getting-started-with-wordpress-guide/)
- [Панель управления WordPress (`wp-admin`): базовая навигация](../console/wordpress-dashboard-wp-admin-basics/)
- [Страницы, записи, категории и теги: базовая модель контента](../content/pages-posts-and-taxonomies-basics/)
- [Плагины WordPress: основы выбора и установки](../plugins/plugins-basics/)
- [Темы, child theme и Customizer: основы](../themes/themes-child-themes-customizer/)
- [Как вести базу знаний WordPress](./knowledge-base-workflow/)
