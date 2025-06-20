# 🧭 Задача 6: Настройка маршрутизации (URLs)

## Описание
Настроить URL-маршруты для блога в соответствии со спецификацией README.md. Маршруты реализуются в файле `blog/urls.py` и включаются в корневой `urls.py` проекта.

## Требования

### Основные маршруты
1. **Список постов**: `/` (имя: `blog:list`)
2. **Список категорий**: `/categories/` (имя: `blog:categories`)
3. **Детали категории**: `/categories/<slug:category_slug>/` (имя: `blog:category-detail`)
4. **Список тегов**: `/tags/` (имя: `blog:tags`)
5. **Детали тега**: `/tags/<slug:tag_slug>/` (имя: `blog:tag-detail`)
6. **Детали поста**: `/posts/<slug:post_slug>/` (имя: `blog:detail`)

### Особенности реализации
- Все маршруты группируются в пространстве имен `blog`
- Использовать `path()` для простых маршрутов и `re_path()` для сложных
- Для slug-параметров использовать валидацию через `slug` конвертер
- В корневом `urls.py` проекта:
  - Импортировать `include` из `django.urls`
  - Добавить `path('', include('blog.urls', namespace='blog'))`

### Пример структуры `blog/urls.py`
```python
from django.urls import path
from . import views

app_name = 'blog'

urlpatterns = [
    path('', views.PostListView.as_view(), name='list'),
    path('categories/', views.CategoryListView.as_view(), name='categories'),
    path('categories/<slug:slug>/', views.CategoryDetailView.as_view(), name='category-detail'),
    path('tags/', views.TagListView.as_view(), name='tags'),
    path('tags/<slug:slug>/', views.TagDetailView.as_view(), name='tag-detail'),
    path('posts/<slug:slug>/', views.PostDetailView.as_view(), name='detail'),
]
```

## Проверка
- Все URL-адреса разрешаются корректно
- Пространство имен `blog` работает правильно
- Параметры slug передаются в представления
- Ошибка 404 возвращается для несуществующих путей