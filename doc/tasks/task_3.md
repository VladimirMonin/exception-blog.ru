# 📦 Задача 3: Настройка S3-хранилища

## Описание
Настроить интеграцию с S3-совместимым хранилищем Timeweb Cloud для хранения медиафайлов блога. 

## Подготовка перед началом
1. Создать аккаунт в Timeweb Cloud (если отсутствует)
2. В панели управления Timeweb:
   - Создать S3-совместимое хранилище (Bucket)
   - Сгенерировать ключи доступа (Access Key и Secret Key)
   - Задать политику публичного доступа (если требуется)
3. Сохранить полученные ключи в надежном месте

## Требования
1. Установить необходимые зависимости:
   ```bash
   poetry add django-storages boto3 python-dotenv
   ```
2. Добавить настройки в `settings.py`:
   ```python
   # Настройки S3
   DEFAULT_FILE_STORAGE = "storages.backends.s3boto3.S3Boto3Storage"
   AWS_ACCESS_KEY_ID = env("S3_KEY")  # Ключ доступа
   AWS_SECRET_ACCESS_KEY = env("S3_SECRET")  # Секретный ключ
   AWS_STORAGE_BUCKET_NAME = env("S3_BUCKET")  # Имя бакета
   AWS_S3_ENDPOINT_URL = "https://s3.timeweb.cloud"  # Конечная точка
   AWS_S3_REGION_NAME = "ru-1"  # Регион
   AWS_QUERYSTRING_AUTH = False  # Публичные URL без подписи
   ```
3. Создать файлы:
   - `.env` в корне проекта (добавить в .gitignore):
     ```ini
     S3_KEY=your_access_key_here
     S3_SECRET=your_secret_key_here
     S3_BUCKET=your_bucket_name_here
     ```
   - `.env.example` (для примера, без реальных ключей):
     ```ini
     S3_KEY=your_access_key
     S3_SECRET=your_secret_key
     S3_BUCKET=your_bucket_name
     ```
4. Настроить модель Post для работы с S3:
   ```python
   cover = models.ImageField(
       upload_to='covers/',
       storage=storages_backends.S3Boto3Storage(),
       null=True,
       blank=True
   )
   ```

## Рабочий процесс
1. Загрузка файлов через Django-admin
2. Автоматическое сохранение в S3-хранилище
3. Получение публичного URL для вставки в Markdown:
   ```markdown
   ![Описание](https://cdn.example.com/path/to/image.jpg)
   ```

## Проверка
- Возможность загружать файлы через админку
- Файлы доступны по публичным URL
- Отсутствие ошибок при сохранении постов
- Корректное отображение загруженных изображений