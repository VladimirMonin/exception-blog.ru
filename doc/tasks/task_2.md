# 🗃️ Задача 2: Реализация моделей данных

## Описание
Создать модели данных для блога: Category, Tag и Post. Модели должны полностью соответствовать спецификации из README.md.

## Требования

### Модель Category
- Поля:
  - `name`: CharField (макс. 150 символов)
  - `slug`: SlugField (уникальный, генерируется с помощью `unicode-slugify`)
  - `description`: TextField (необязательный, макс. 1000 символов)
  - `icon`: CharField (необязательный, макс. 50 символов, для иконок Bootstrap)
- Сортировка: по полю `name`
- Метод `__str__`: возвращает название категории

### Модель Tag
- Поля:
  - `name`: CharField (макс. 100 символов)
  - `slug`: SlugField (уникальный)
- Метод `__str__`: возвращает название тега

### Модель Post
- Поля:
  - `title`: CharField (макс. 200 символов)
  - `slug`: SlugField (уникальный)
  - `description`: CharField (макс. 300 символов, краткое описание)
  - `markdown_content`: TextField (исходный Markdown-контент)
  - `html_content`: TextField (кеш HTML, не редактируется)
  - `cover`: ImageField (необязательный, путь в S3)
  - `status`: CharField (выбор: Draft/Published)
  - `published_at`: DateTimeField (необязательный, дата публикации)
  - `views`: PositiveIntegerField (счетчик просмотров, по умолчанию 0)
  - `category`: ForeignKey к Category
  - `tags`: ManyToMany к Tag
  - `created_at`: DateTimeField (автозаполнение при создании)
  - `updated_at`: DateTimeField (автообновление при изменении)
- Поведение:
  - При сохранении (`save()`): конвертировать `markdown_content` в HTML только если контент изменился
  - Автоматически инкрементировать `views` при каждом отображении детальной страницы (с проверкой сессии во избежании накруток)

## Проверка
- Все поля соответствуют спецификации
- Конвертация Markdown → HTML работает корректно
- Счетчик просмотров увеличивается при каждом просмотре поста
- Модели доступны и работают в Django admin