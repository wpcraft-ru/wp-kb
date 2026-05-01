Вот готовый вариант в формате **Markdown**, подходящий для размещения в репозитории (например, как `INSTALL.md` или `docs/INSTALLATION.md`):

```markdown
# Инструкция по установке Summarize

Эта инструкция поможет установить **Summarize** на **macOS** и **Windows**.

## 1. Установка через пакетный менеджер (рекомендуется)

### macOS

**Через Homebrew:**

```bash
brew install summarize
```

### Windows / macOS (универсальный способ)

**Через npm:**

1. Установите [Node.js](https://nodejs.org) (рекомендуется версия 22 или выше).
2. Выполните команду:

```bash
npm install -g @steipete/summarize
```

## 2. Настройка расширения браузера

1. Установите расширение **Summarize** из:
   - [Chrome Web Store](https://chromewebstore.google.com/detail/summarize/)
   - Firefox Add-ons

2. Откройте боковую панель (Side Panel) в Chrome или Sidebar в Firefox.

3. Скопируйте токен, который покажет расширение.

4. Выполните команду для подключения:

```bash
summarize daemon install --token ВАШ_ТОКЕН_ЗДЕСЬ
```

> **Важно для Windows:** Запускайте PowerShell или Командную строку **от имени администратора**.

## 3. Установка дополнительных зависимостей (рекомендуется)

### macOS

```bash
brew install ffmpeg yt-dlp tesseract
```

### Windows

Установите следующие программы:
- `ffmpeg`
- `yt-dlp`
- `tesseract` (опционально)

Рекомендуемый способ — через **Scoop**:

```powershell
scoop install ffmpeg yt-dlp
```

## 4. Проверка установки

```bash
summarize --version
```

Пример использования:

```bash
summarize https://example.com
```

## 5. Запуск без установки (одноразово)

```bash
npx -y @steipete/summarize https://your-link.com
```

## Полезные ссылки

- **GitHub репозиторий**: [https://github.com/steipete/summarize](https://github.com/steipete/summarize)
- **Официальный сайт**: [https://summarize.sh](https://summarize.sh)

```

