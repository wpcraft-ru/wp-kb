# "Дизайн как код" в WordPress — Материалы на русском

**Дата:** 2026-05-05
**Запрос:** "дизайн как код" wordpress

---

## Компонентный подход в WordPress

Разбивка дизайна на переиспользуемые функции (аналогично React):
- Карточка поста собирается из атомарных элементов (обложка, заголовок, кнопка)
- Код короче, быстрее грузится, проще переносится между проектами

## Full Site Editing (FSE) — Дизайн-система без кода

Начиная с WP 5.9+:
- **Global styles** — задать цвета, типографику, отступы глобально → применяются ко всему сайту
- **Patterns** — готовые секции (тестимониалы, pricing), адаптирующиеся автоматически

## Headless WordPress

WordPress как бэкенд (API) + фронтенд на React/Next.js:
- Полный контроль над кодом дизайна
- Высокая производительность
- REST API или GraphQL для данных

## Добавление кода в WordPress

1. **WPCode** — вставка PHP/JS/CSS в header/footer без редактирования файлов
2. **Редактор темы** — Внешний вид → Редактор (style.css для стилей, functions.php для PHP)
3. **Блоки Gutenberg** — Custom HTML или Code block для фрагментов
4. **Дочерние темы** — для кастомных тем (не потерять изменения при обновлениях)

## Ссылки на русском

- https://thecode.media/wp-template/ — создание шаблонов WordPress
- https://clickfraud.ru/kak-otobrazit-kod-v-wordpress-i-sdelat-ego-krasivym/ — отображение кода в WP
- https://egoroff.info/blog/wp/kod-v-wordpress/ — код в WordPress
- https://wordpress.org/plugins/insert-headers-and-footers/ — WPCode плагин
