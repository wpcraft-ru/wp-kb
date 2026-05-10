---
title: "Дизайн-системы в WordPress"
description: "Построение дизайн-систем для WordPress: от Figma до theme.json, Atomic Design, компонентный подход, блоки и переиспользуемые паттерны."
---

## Что такое дизайн-система

Дизайн-система (Design System) — набор переиспользуемых строительных блоков, правил и стандартов, обеспечивающих визуальную и функциональную консистентность продукта. В контексте WordPress дизайн-система превращается в блочную тему с `theme.json`, паттернами и кастомными блоками.

**Состав дизайн-системы:**
- **Design tokens** — цвета, типографика, отступы, скругления
- **Компоненты** — кнопки, формы, карточки, хедеры
- **Паттерны** — готовые секции: hero, pricing, testimonials
- **Правила** — когда и как использовать компоненты

## Преимущества дизайн-системы

1. **Переиспользуемость** — компоненты создаются один раз и применяются на всех страницах
2. **Консистентность** — единый визуальный язык без расхождений
3. **Скорость** — новые страницы собираются из готовых блоков, а не рисуются с нуля
4. **DRY** — изменение токена обновляет все элементы автоматически
5. **Масштабирование** — несколько сайтов на одной дизайн-системе

## Atomic Design в WordPress

Методология Брэда Фроста идеально ложится на блочный редактор WordPress:

| Уровень | Atomic Design | WordPress |
|---------|--------------|-----------|
| **Atoms** | Цвета, шрифты, отступы | `theme.json` → CSS-переменные |
| **Molecules** | Кнопка + поле = форма поиска | Блоки: Search, Buttons, Social Icons |
| **Organisms** | Хедер, футер, сайдбар | Template Parts |
| **Templates** | Шаблон страницы | `templates/single.html`, `page.html` |
| **Pages** | Конкретная страница с контентом | Страница в админке, наполненная блоками |

## Инструменты дизайна

| Инструмент | Для чего | Связь с WordPress |
|-----------|----------|-------------------|
| **Figma** | Создание макетов, компонентов, токенов | Экспорт токенов в theme.json через плагины |
| **Tokens Studio** (Figma-плагин) | Управление design tokens, Git-синхронизация | Прямой экспорт в W3C DTCG-формат |
| **Style Dictionary** (Amazon) | Трансформация токенов в CSS/Sass/Android/iOS | Генерация `--wp--custom--*` переменных |
| **Storybook** | Документирование и тестирование компонентов | Визуальные регрессии блоков |
| **Figma Make** | AI-генерация кода из фреймов | HTML/CSS/React из дизайн-макетов |

## Практический подход: от простого к сложному

По Contentful, построение дизайн-системы идёт поэтапно:

1. **Один primary color** — минимальная палитра
2. **Stepping system** — шкалы оттенков (50-900)
3. **Semantic tokens** — именованные токены: `color-accent`, `text-primary`
4. **Interactive states** — hover, active, focus, disabled
5. **Component tokens** — `button-primary-bg`, `card-border-radius`

## WordPress-специфика

Для WordPress дизайн-система реализуется через:

### 1. theme.json — фундамент

Глобальные настройки стилей, цветов, типографики. В кастомной секции `settings.custom` хранятся design tokens — единый source of truth.

### 2. Блоки как компоненты

Кастомные блоки (React/PHP/CSS) — это molecules и organisms дизайн-системы. `block.json` определяет атрибуты и настройки блока, как props в React-компоненте.

### 3. Паттерны — готовые секции

Group-блок с вложенной структурой, сохранённый как паттерн = готовый organism. Может быть синхронизированным (изменения применяются везде) или несинхронизированным (копия).

### 4. Template Parts — глобальные элементы

Header, Footer, Sidebar — стабильные части всех страниц.

## Построение дизайн-системы: пошагово

1. **Определите grid spacing unit** — базовый шаг отступов (4px, 8px), не меняйте его потом
2. **Вынесите цвета в токены** — не используйте raw HEX в блоках
3. **Создайте базовые компоненты** — кнопки, формы, типографика
4. **Соберите паттерны** — hero, карточки, pricing tables
5. **Разделите по брендам** — если несколько брендов, отдельные библиотеки токенов
6. **Документируйте** — Style Book в WordPress + Storybook для кастомных блоков

## Когда дизайн-система оправдана

- Несколько страниц с повторяющимися элементами
- Команда из 2+ человек работает над дизайном
- Есть несколько сайтов с общим брендом
- Частота изменений дизайна высока
- Нужна консистентность между платформами (web + mobile)

## Материалы и источники

- [WebDevStudios: Building Design Systems for WordPress](https://webdevstudios.com/2020/02/20/design-systems-for-wordpress/)
- [WordPress VIP: Design System with Block Editor — Theme.json](https://wpvip.com/blog/using-a-design-system-with-the-wordpress-block-editor-pt-1-theme-json/)
- [DDpa: Design as Code — Design Tokens и архитектура](https://ddpa.ru/kb/ai/design/design-as-code/)
- [Contentful: Design Tokens Explained](https://www.contentful.com/blog/design-tokens-explained/)
- [Atomic Design by Brad Frost](http://atomicdesign.bradfrost.com/table-of-contents/)
- [Impeccable: AI-дизайн для WordPress](./impeccable.md) — AI-навык с `/impeccable extract` и `/impeccable polish` для работы с дизайн-системами.
