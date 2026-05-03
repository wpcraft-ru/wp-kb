Перенос файлов WordPress в S3‑совместимые хранилища с помощью плагина S3 Uploads (Human Made) | WP Craft
[Перейти к сути](#main)
Ничего не найдено
[Вход](/my/)
[Помощь с сайтом](https://wpcraft.ru/services)
* [Главное](/)
* [База знаний](https://wpcraft.ru/kb/)
* [Клуб про WP & Woo](https://wpcraft.ru/club-wordpress-woocommerce)
* [Свобода WordPress](https://wpcraft.ru/free-wordpress-woocommerce)
* [Услуги WordPress](https://wpcraft.ru/services)
* [Консультация](https://wpcraft.ru/services/ask)
* [Блог](https://wpcraft.ru/blog)
* [Контакты](https://wpcraft.ru/contacts)
* [Написать в Телеграм](https://t.me/wpcraftbot)
* [Подписаться на Телеграм](https://t.me/wpcraft)
Поиск
[Помощь с сайтом](https://wpcraft.ru/services)
* [Клуб](https://wpcraft.ru/club-wordpress-woocommerce)
* [Услуги](https://wpcraft.ru/services)
* [Проекты](https://wpcraft.ru/projects)
* [Решения](https://wpcraft.ru/use-cases)
* [Каталог](https://wpcraft.ru/catalog)
* [Контакты](https://wpcraft.ru/contacts)
[Вход](/my/)
* [База знаний](https://wpcraft.ru/kb-wordpress-woocommerce)
* [Нейросети + ИИ](https://wpcraft.ru/ai)
* [Сайты WordPress](https://wpcraft.ru/wordpress)
* [Магазины WooCommerce](https://wpcraft.ru/woocommerce)
* [Веб-разработка](https://wpcraft.ru/web)
* [HL&BD](https://wpcraft.ru/highload-bigdata)
* [Хостинг](https://wpcraft.ru/hosting-wordpress-woocommerce)
* [Блоки](https://wpcraft.ru/block-based-websites)
* [Кейсы](https://wpcraft.ru/showcase)
* [Мониторинг сайта](https://wpcraft.ru/website-monitoring-wordpress-woocommerce)
* [Отзывы](https://wpcraft.ru/reviews-wordpress-woocommerce)
* [Лучшие агентства](https://wpcraft.ru/wordpress-agency)
Меню
[WPCraft](https://wpcraft.ru/)/[Блог](https://wpcraft.ru/blog)/[Составляющие](https://wpcraft.ru/blog/category/includes)/[Плагины](https://wpcraft.ru/blog/category/includes/wordpress-plugins)
* [Antony I](https://wpcraft.ru/blog/author/ai)
* 16.03.2026
* 16.03.2026
* [Обзоры](https://wpcraft.ru/blog/category/articles/overview), [Плагины](https://wpcraft.ru/blog/category/includes/wordpress-plugins)
# Перенос файлов WordPress в S3‑совместимые хранилища с помощью плагина S3 Uploads (Human Made)
Если медиа‑библиотека раздувает диск, усложняет бэкапы и мешает масштабированию, S3 Uploads закрывает задачу «вынос uploads в объектное хранилище» без лишней обвязки.
## Что делает плагин
* Заменяет локальную `[wp](https://wpcraft.ru/wordpress)-content/uploads` на хранилище S3.
* Перехватывает загрузку и чтение медиа, работает с миниатюрами.
* Переписывает URL‑ы на S3 (если это включено в конфиге).
* Поддерживает приватные файлы через signed URL.
Важно: у плагина почти нет UI. Настройка — через `wp-config.php`, код и WP‑CLI. Это плюс для команд, которым нужен контроль, и минус для тех, кто хочет «всё в админке».
## Когда подходит
* Высоконагруженные сайты и несколько веб‑нод (stateless).
* Kubernetes / autoscaling.
* Экономия на egress (часто выбирают Cloudflare R2).
* Приватные файлы (курсы, документы) со временными ссылками.
* Миграции и переносы без хранения медиа на сервере.
* Локальная разработка с возможностью отключать S3.
## Плюсы и ограничения
Плюсы:
* Open‑source и лёгкий.
* Минимум «магии» и настроек в интерфейсе.
* WP‑CLI для проверки и миграции.
* Работает с S3‑совместимыми хранилищами через endpoint.
Ограничения:
* Нет красивой админки.
* Нужно руками настроить IAM и конфиг.
* Для новичков проще WP Offload Media.
## Провайдеры S3 хранилища
### Облачные (managed)
* Amazon S3 — оригинал
* Yandex Object Storage
* Cloudflare R2
* DigitalOcean Spaces
* Другие S3‑совместимые облака (по endpoint)
### Self-hosted
* MinIO
* Ceph (RGW)
* Любые S3‑совместимые self‑hosted решения (по endpoint)
## Быстрая настройка (скелет)
1. В облаке: бакет + IAM‑пользователь с правами на чтение/запись/листинг.
2. Установка (рекомендуется Composer): `composer require humanmade/s3-uploads`
3. Константы в `wp-config.php`: `define( 'S3_UPLOADS_BUCKET', 'bucket-name' ); define( 'S3_UPLOADS_REGION', 'eu-central-1' ); define( 'S3_UPLOADS_KEY', 'AKIA…' ); define( 'S3_UPLOADS_SECRET', '…' );`
4. Для R2/MinIO и других не‑AWS: задайте endpoint через фильтр `s3_uploads_s3_client_params` (mu‑plugin).
5. Миграция старых файлов: `wp s3-uploads upload-directory wp-content/uploads uploads --verbose`
6. Проверка: `wp s3-uploads verify`
## Вывод
S3 Uploads — хороший выбор, если нужен «инженерный» способ хранить медиа WordPress в S3: без UI, но с предсказуемым поведением и удобным WP‑CLI.
Если нужен интерфейс и настройка в один клик — обычно смотрят в сторону WP Offload Media.
Antony I
Веб разработчик, специализация на лучших мировых практиках: WordPress, WooCommerce, NextJS, Strapi, JAMStack ...
Основные типы проектов: CMS, eCommerce, SEO, LMS, ECM, BPM
## Связанное
### [Как создать сайт базу знаний с использованием AI WIKI LLM, Markdown, Astro JS & Starlight KB](https://wpcraft.ru/blog/ai-wiki-llm-markdown-astro-js-starlight-kb)
* [JAM-stack](https://wpcraft.ru/blog/category/development/jam-stack), [Как сделать](https://wpcraft.ru/blog/category/articles/how-to), [Кейсы](https://wpcraft.ru/blog/category/cases)
### [Laravel Mobile Pass: создание пропусков для Apple и Google Wallet](https://wpcraft.ru/blog/laravel-mobile-pass-sozdanie-propuskov-dlya-apple-i-google-wallet)
* [TALL stack](https://wpcraft.ru/blog/category/development/tall-stack), [Компоненты](https://wpcraft.ru/blog/category/includes/components), [Обзоры](https://wpcraft.ru/blog/category/articles/overview)
### [Цена ошибок в разработке сайтов и магазинов на WordPress и WooCommerce](https://wpcraft.ru/blog/the-cost-of-mistakes-when-developing-websites-and-online-shops-using-wordpress-and-woocommerce)
* [WooCommerce](https://wpcraft.ru/blog/category/development/woocommerce), [WordPress](https://wpcraft.ru/blog/category/development/wordpress), [Аналитика](https://wpcraft.ru/blog/category/articles/analitika), [Мнения](https://wpcraft.ru/blog/category/articles/mneniya)
### [Server-Driven UI: когда нужен, как работает и чем отличается от классического Content Driven](https://wpcraft.ru/blog/ui-server-driven-vs-content-driven)
* [Gutenberg](https://wpcraft.ru/blog/category/development/gutenberg), [WooCommerce](https://wpcraft.ru/blog/category/development/woocommerce), [WordPress](https://wpcraft.ru/blog/category/development/wordpress), [Аналитика](https://wpcraft.ru/blog/category/articles/analitika), [Сравнения](https://wpcraft.ru/blog/category/articles/sravneniya)
### [Фасетный поиск для магазина на 40 млн товаров: переход с InstantSearch на TanStack Query](https://wpcraft.ru/blog/facet-search-ecommerce-nextjs-tanstack)
* [JAM-stack](https://wpcraft.ru/blog/category/development/jam-stack), [Кейсы](https://wpcraft.ru/blog/category/cases)
### [Новости про WordPress, WooCommerce & Web-разработку. Дайджест за неделю — 25 апреля 2026](https://wpcraft.ru/blog/novosti-pro-wordpress-woocommerce-web-razrabotku-dajdzhest-za-nedelyu-25-aprelya-2026)
* [Новости & Дайджест](https://wpcraft.ru/blog/category/news)
## Ответить[Отменить ответ](/blog/s3-uploads-human-made#respond)
Ваш адрес email не будет опубликован. Обязательные поля помечены \*
Имя \*
Email \*
Оставьте свой комментарий \*
Отправить комментарий
### Рубрики
* [Кейсы](https://wpcraft.ru/blog/category/cases)
* [Новости & Дайджест](https://wpcraft.ru/blog/category/news)
* [Разработка & Интеграция](https://wpcraft.ru/blog/category/development)
+ [Agile](https://wpcraft.ru/blog/category/development/agile)
+ [Gutenberg](https://wpcraft.ru/blog/category/development/gutenberg)
+ [JAM-stack](https://wpcraft.ru/blog/category/development/jam-stack)
+ [TALL stack](https://wpcraft.ru/blog/category/development/tall-stack)
+ [WooCommerce](https://wpcraft.ru/blog/category/development/woocommerce)
+ [WordPress](https://wpcraft.ru/blog/category/development/wordpress)
+ [Веб дизайн](https://wpcraft.ru/blog/category/development/web-design)
* [Составляющие](https://wpcraft.ru/blog/category/includes)
+ [Компоненты](https://wpcraft.ru/blog/category/includes/components)
+ [Плагины](https://wpcraft.ru/blog/category/includes/wordpress-plugins)
+ [Сервисы](https://wpcraft.ru/blog/category/includes/services)
+ [Темы](https://wpcraft.ru/blog/category/includes/wordpress-themes)
* [Статьи & Исследования](https://wpcraft.ru/blog/category/articles)
+ [Аналитика](https://wpcraft.ru/blog/category/articles/analitika)
+ [Безопасность](https://wpcraft.ru/blog/category/articles/security)
+ [Как сделать](https://wpcraft.ru/blog/category/articles/how-to)
+ [Мнения](https://wpcraft.ru/blog/category/articles/mneniya)
+ [Нейросети + AI (ИИ)](https://wpcraft.ru/blog/category/articles/ai)
+ [Обзоры](https://wpcraft.ru/blog/category/articles/overview)
+ [Ошибки](https://wpcraft.ru/blog/category/articles/errors)
+ [Подборки](https://wpcraft.ru/blog/category/articles/collections)
+ [Продвижение сайта](https://wpcraft.ru/blog/category/articles/website-promotion)
+ [Сниппеты](https://wpcraft.ru/blog/category/articles/snippets)
+ [Сравнения](https://wpcraft.ru/blog/category/articles/sravneniya)
+ [Ускорение сайта](https://wpcraft.ru/blog/category/articles/website-performance)
+ [ЧаВО](https://wpcraft.ru/blog/category/articles/faq)
[headless CMS](https://wpcraft.ru/blog/tag/headless-cms)
[NextJS](https://wpcraft.ru/blog/tag/nextjs)
[WordPress 7.0](https://wpcraft.ru/blog/tag/wordpress-7-0)
[Избранное](https://wpcraft.ru/blog/tag/favorites)
[Кеширование](https://wpcraft.ru/blog/tag/cache-wordpress)
[Локальное окружение и разработка](https://wpcraft.ru/blog/tag/local-environment-and-development-wordpress-woocommerce)
[Примеры сайтов на базе WordPress](https://wpcraft.ru/blog/tag/wordpress-websites-examples)
[Сеть сайтов - MultiSite](https://wpcraft.ru/blog/tag/wordpress-multisite)
[Управление сайтом](https://wpcraft.ru/blog/tag/website-management)
[Хостинг и сервера](https://wpcraft.ru/blog/tag/hosting)
[Хуки (actions + filters + events)](https://wpcraft.ru/blog/tag/wp-hooks)
[Чек-листы](https://wpcraft.ru/blog/tag/checklists)
**Клуб WPCraft**
* лучшие мировые практики и паттерны
* премиум темы и плагины - бесплатно
* экспертная помощь от опытных практиков
**Лучшие хостинги**
* для сайтов на WordPress и WooCommerce
* рейтинг от опытных разработчиков
* решения проверенные временем
**Помощь с сайтом**
* консультации и диагностика
* техническая поддержка
* управление сайтом
## Разработка современных и удобных сайтов на базе WordPress & WooCommerce
Лучшие мировые практики, которые помогают вывести сайты на качественно новый уровень.
[Подписаться: Telegram](https://t.me/wpcraft)
## [Консультация специалиста](https://wpcraft.ru/services/ask)
1 час общения с опытным специалистом, заменяет месяцы и иногда годы потерянного времени и денег
[Консультация 1 час](https://wpcraft.ru/services/ask)
[Премиум клуб WPCraft](https://wpcraft.ru/club-wordpress-woocommerce/?ref=footer)
### Услуги
* [Аудит сайта](https://wpcraft.ru/services/audit-websites-projects-wordpress-woocommerce)
* [Консультация](https://wpcraft.ru/services/ask)
* [Диагностика сайта](https://wpcraft.ru/services/troubleshooting)
* [Ускорение сайта](https://wpcraft.ru/services/page-speed-cwvs-wordpress-woocommerce)
* [Разработка сайта](https://wpcraft.ru/services/development-wordpress-woocommerce)
* [Управление сайтом](https://wpcraft.ru/services/managed-wordpress-woocommerce)
* [Лечение вирусов](https://wpcraft.ru/services/troubleshooting/clear-virus-wordpress)
* [Исправление ошибок](https://wpcraft.ru/services/troubleshooting/errors-wordpress-woocommerce)
### Проекты
* [WordPress](https://wpcraft.ru/wordpress)
* [WooCommerce](https://wpcraft.ru/woocommerce)
* [Решения](https://wpcraft.ru/use-cases)
* [Плагины](https://wpcraft.ru/wordpress/plugins)
* [База знаний](https://wpcraft.ru/kb/)
* [Мониторинг сайта](https://wpcraft.ru/website-monitoring-wordpress-woocommerce)
* [Бесплатно WordPress](https://wpcraft.ru/free-wordpress-woocommerce)
* [Дайджест](https://wpcraft.ru/digest)
### Контакты
* Email:
m@wpcraft.ru
* Website:
[wpcraft.ru](https://wpcraft.ru)
[Написать в Телеграм](https://t.me/wpcraftbot)
[Контакты](https://wpcraft.ru/contacts/)
WP Craft © 2026
* [llms.txt](/llms.txt)
* [Блог](https://wpcraft.ru/blog)
* [О сайте](https://wpcraft.ru/about)
* [Цены](https://wpcraft.ru/pricing)
* [Правила](https://wpcraft.ru/legal)
