---
title: "Кэширование в WooCommerce"
description: "Настройка кэширующих плагинов и серверов для WooCommerce: исключение динамических страниц, cookies, сессии и конфигурация Varnish."
---

# Кэширование в WooCommerce

Кэширование критически важно для производительности магазина, но WooCommerce генерирует динамический контент (корзина, заказы, профиль). Неправильная настройка кэша приводит к сломанной корзине, рассинхронизации цен и невозможности оформить заказ.

Подробнее об общих принципах кэширования: [Кэширование WordPress](./cache.md).

## Страницы, которые нельзя кэшировать

Три страницы WooCommerce всегда должны оставаться динамическими:

- **Cart** (Корзина) — показывает товары текущего пользователя
- **Checkout** (Оформление заказа) — содержит персональные данные и рассчитывает итоги
- **My Account** (Личный кабинет) — заказы, адреса, профиль

Большинство кэширующих плагинов уже исключают эти страницы автоматически, но стоит проверять.

## Сессии WooCommerce

Если кэширующая система поддерживает кэширование базы данных, исключите `_wc_session_` из кэширования. Иначе корзина и данные пользователя могут не обновляться.

## Cookies WooCommerce

При настройке кэширования убедитесь, что эти cookies не приводят к отмене кэширования статических страниц:

| Cookie | Срок жизни | Назначение |
|--------|-----------|------------|
| `woocommerce_cart_hash` | Сессия | Определяет изменения содержимого корзины |
| `woocommerce_items_in_cart` | Сессия | Определяет изменения содержимого корзины |
| `wp_woocommerce_session_` | 2 дня | Уникальный код клиента для поиска данных корзины в БД |
| `woocommerce_recently_viewed` | Сессия | Виджет «Недавно просмотренные товары» |
| `store_notice[notice id]` | Сессия | Закрытие уведомления магазина |

## Настройка популярных плагинов

### W3 Total Cache

В секции **Minify** добавьте `mfunc` в поле **Ignored comment stems**.

### WP Rocket

Полная совместимость с WooCommerce. Убедитесь, что Cart, Checkout, My Account **не кэшируются**. Рекомендуется **отключить минификацию JavaScript** — WooCommerce сильно зависит от JS, и агрегация может сломать функциональность.

### WP Super Cache

Нативная совместимость. WooCommerce автоматически передаёт WP Super Cache список страниц для исключения (Cart, Checkout, My Account).

## Настройка Varnish

Минимальная конфигурация для `vcl_recv`:

```varnish
if (req.url ~ "^/(cart|my-account|checkout|addons)") {
    return (pass);
}
if ( req.url ~ "\\?add-to-cart=" ) {
    return (pass);
}
```

### Полная конфигурация Varnish

Расширенный вариант с обработкой cookies:

```varnish
# vcl_recv — входящий запрос

# Убираем cookies для статических страниц
if (!(req.url ~ "(wp-login|wp-admin|cart|my-account/*|wc-api*|checkout|addons|logout|lost-password|product/*)")) {
    unset req.http.cookie;
}

# Пропускаем динамические страницы WooCommerce
if (req.url ~ "^/(cart|my-account/*|checkout|wc-api/*|addons|logout|lost-password|product/*)") {
    return (pass);
}

# Пропускаем запросы добавления в корзину
if (req.url ~ "\?add-to-cart=" ) {
    return (pass);
}

# Пропускаем WooCommerce API
if (req.url ~ "\?wc-api=" ) {
    return (pass);
}
```

```varnish
# vcl_fetch — исходящий ответ

# Убираем cookies для GET-запросов к статическим страницам
if ( (!(req.url ~ "(wp-(login|admin)|login|cart|my-account/*|wc-api*|checkout|addons|logout|lost-password|product/*)")) || (req.request == "GET") ) {
    unset beresp.http.set-cookie;
}
```

## Типичные проблемы

### Сброс пароля зацикливается

**Причина:** страница My Account закэширована на уровне сервера.

**Решение:** обратитесь к хостинг-провайдеру и убедитесь, что My Account исключена из серверного кэширования. Некоторые хостинги кэшируют PHP-страницы на своём уровне.

### Корзина не обновляется

**Причина:** кэш страницы отдаётся вместо динамического контента.

**Решение:** проверьте, что:
1. Страница Cart исключена из кэширования в плагине
2. Cookies WooCommerce не вызывают преждевременную инвалидацию кэша статических страниц
3. Если используется Varnish — проверьте правила в `vcl_recv`

## Материалы и источники

- [How to configure caching plugins for WooCommerce](https://developer.woocommerce.com/docs/best-practices/performance/configuring-caching-plugins/) — официальная документация WooCommerce (источник)
- [Кэширование WordPress](./cache.md) — общие принципы кэширования
- [Оптимизация WordPress](./optimization.md) — полный обзор оптимизации
- [Varnish configuration for WooCommerce](https://wordpress.org/support/topic/varnish-configuration-not-working-in-woocommerce/) — обсуждение на WordPress.org
