# How to Build Design Systems for WordPress

**Источник:** https://webdevstudios.com/2020/02/20/design-systems-for-wordpress/
**Дата:** 2020-02-20

---

## Что такое Design System?

Набор строительных блоков для веб-дизайна: wireframes, шрифты, стили кнопок, использование логотипа, хедер/футер и т.д. Может быть как generic (общие блоки), так и специфичным (полный brand & style guide компании).

## Преимущества

1. **Reusability** — компоненты переиспользуются, экономия времени
2. **Consistency** — единый вид на всех страницах
3. **DRY** — "Don't repeat yourself"
4. **Скорость и экономия** — меньше времени на создание нового дизайна
5. **Лёгкая документация** — только новые элементы требуют документирования

---

## Инструменты для разработки Design System

| Инструмент | Цена |
|------------|------|
| **Sketch** | $99/год после 30-дневного триала |
| **Adobe XD** | Бесплатно с ограничениями / $10-23/мес |
| **Figma** | Бесплатно (2 пользователя, 3 проекта) / $144/год |

---

## Применение к WordPress

DS в WordPress-разработке помогает визуализировать специфические шаблоны:

- Компоненты хедера, футера, сайдбара
- Компоненты для template parts:
  - Content archive
  - Content page
  - Gutenberg-блоки (Hero, CTA, Carousel, Recent Posts)

---

## Рекомендации

1. **Определите grid spacing unit в начале — и никогда не меняйте**
2. **Разделите компоненты каждого бренда в отдельную библиотеку**
3. **Стройте блоки отдельно от mock-up'ов**
4. **Переиспользуйте символы/компоненты**
5. **Начинайте с базового:**
   - Headers, footers
   - Multi-device mock-ups
   - Buttons, forms, typography, menus
   - Colors, styles, states
6. **Затем добавляйте:**
   - Prototyping
   - Animations
   - User personas
   - Flows

---

## Полезные ссылки

- [Atomic Design](http://atomicdesign.bradfrost.com/table-of-contents/)
- [Proposal: consistent spacing system for WordPress](https://make.wordpress.org/design/2019/10/31/proposal-a-consistent-spacing-system-for-wordpress/)
- [Design Systems as a Shared Product Language](https://writing.mariusz.cc/design-systems-as-a-shared-product-language)
- [Build your design system in Figma](https://www.figma.com/blog/how-to-build-your-design-system-in-figma/)
