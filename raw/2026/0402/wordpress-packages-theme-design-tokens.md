# @wordpress/theme — Design Tokens и Theme System

**Источник:** https://developer.wordpress.org/block-editor/reference-guides/packages/packages-theme/
**Дата публикации:** 2025-10-22
**Последнее обновление:** 2026-04-23
**Статус:** Экспериментальный пакет (early implementation, subject to breaking changes)

---

## Обзор

`@wordpress/theme` — пакет темизации, часть WordPress Design System. Состоит из двух частей:

1. **Design Tokens** — комплексная система дизайн-токенов для цветов, отступов, типографики и т.д.
2. **Theme System** — гибкий ThemeProvider для консистентной темизации внутри приложений.

---

## Design Tokens: Архитектура

### Двухуровневая архитектура токенов

- **Primitive tokens (примитивы)**: Сырые значения (hex-цвета, px-размеры). Хранятся в `/tokens` как JSON-файлы. Внутренняя деталь реализации.
- **Semantic tokens (семантические)**: Целевые токены с осмысленными именами, ссылающиеся на примитивы и описывающие их назначение. Экспортируются как CSS custom properties.

Такое разделение позволяет обновлять примитивы без изменения семантических имён, используемых в коде.

### Файлы токенов

| Файл | Описание |
|------|----------|
| `color.json` | Цветовые палитры (примитивы и семантические токены для background, foreground, stroke, focus) |
| `dimension.json` | Шкала отступов и семантические токены для padding, margins, sizing |
| `typography.json` | Font family stacks, font sizes, line heights |
| `border.json` | Border radius и width |
| `elevation.json` | Тени для создания глубины и слоистости |

### Соглашение об именовании

```
--wpds-<type>-<property>-<target>[-<modifier>]
```

**Type** (тип значения):
- `color` — цвета
- `dimension` — отступы, размеры
- `border` — радиусы и толщины границ
- `elevation` — тени
- `typography` — шрифты

**Property** (свойство дизайна):
- `bg` — background color
- `fg` — foreground/text color
- `stroke` — border/outline color
- `padding` — внутренние отступы
- `gap` — внешние отступы между элементами
- `radius` — border radius
- `width` — border width
- `font-size`, `font-family`, `font-weight`, `line-height`

**Target** (компонент/элемент):
- `surface` — контейнеры/layouts
- `interactive` — кнопки, инпуты, контролы
- `content` — статический контент (текст, иконки)
- `track` — треки (скроллбары, слайдеры)
- `thumb` — ползунки
- `focus` — фокусные индикаторы

**Modifier** (размер/интенсивность):
- `xs`, `sm`, `md`, `lg`, `xl`, `2xl`, `3xl`

### Цветовые токены (расширенный паттерн)

```
--wpds-color-<property>-<target>-<tone>[-<emphasis>][-<state>]
```

**Tone** (семантическое намерение):
- `neutral` — нейтральные UI-элементы
- `brand` — брендовые/primary цвета
- `success`, `info`, `caution`, `warning`, `error`

**Emphasis**: `strong` (высокий контраст), `weak` (приглушённый)
**State**: `active` (hover/pressed/selected), `disabled`

### Использование токенов

Токены поставляются как CSS custom properties:

```css
.element {
    color: var(--wpds-color-fg-content-neutral);
}
```

**Внутри WordPress** — стили управляются автоматически.
**Снаружи WordPress** — нужно установить:
```bash
npm install @wordpress/theme
```
```javascript
import '@wordpress/theme/design-tokens.css';
```

---

## ThemeProvider (React)

Компонент, который оборачивает приложение и предоставляет design tokens и theme context.

```tsx
import { ThemeProvider } from '@wordpress/theme';

function App() {
    return (
        <ThemeProvider color={{ primary: 'blue' }} density="compact">
            {/* ваш контент */}
        </ThemeProvider>
    );
}
```

**Props:**
- `color.primary` — primary/accent seed (default: `'#3858e9'`)
- `color.bg` — background seed (default: `'#f8f8f8'`)
- `cursor.control` — курсор для интерактивных элементов (default: `'pointer'`)
- `density` — `'default'`, `'compact'` (плотные UI), `'comfortable'` (разреженные UI)

**Вложенность:** Provider можно вкладывать для переопределения темы в конкретной подветке DOM.

---

## Инструменты разработчика

### Stylelint Plugins
1. `plugin-wpds/no-unknown-ds-tokens` — проверяет, что `--wpds-*` ссылаются на существующие токены
2. `plugin-wpds/no-setting-wpds-custom-properties` — запрещает переопределение токенов
3. `plugin-wpds/no-token-fallback-values` — запрещает ручные fallback-значения (build-плагины вставляют их автоматически)

### Build Plugins
- PostCSS
- esbuild
- Vite

Автоматически вставляют fallback-значения для `var(--wpds-*)` и выбрасывают ошибку при ссылке на неизвестный токен.

---

## Build Process

При `npm run build`:
1. Генерация primitive tokens
2. Сборка CSS и JavaScript файлов токенов
3. Обновление документации
4. Форматирование всех сгенерированных файлов

Сгенерированные файлы коммитятся в репозиторий.

---

**Важно:** Всё основано на спецификации [Design Tokens Community Group (DTCG)](https://design-tokens.github.io/community-group/format/).
