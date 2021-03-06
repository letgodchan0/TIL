# ๐ ์ํ ๊ฒ์ํ ๊ธฐ๋ฅ ๊ตฌํ(+ django)

> django๋ฅผ ํตํ ์ปค๋ฎค๋ํฐ ์๋น์ค์ ๊ฒ์ํ ๊ธฐ๋ฅ์ ๊ฐ๋ฐํ๋ ๋จ๊ณ๋ก, ์ํ ๋ฐ์ดํฐ์ ์์ฑ, ์กฐํ, ์์ , ์ญ์ ๊ฐ ๊ฐ๋ฅํ ์ดํ๋ฆฌ์ผ์ด์์ ์์ฑํ๋ ๊ฒ์ ๋ชฉํ๋ก ํ๋ค. 



## ํ๊ฒฝ ์ค์  ๋ฐ ๊ธฐ๋ณธ ์ธํ

### 1. ๊ฐ์ํ๊ฒฝ ์์ฑ ๋ฐ ํ์ฑํ

```bash
$ python -m venv venv
$ source venv/Scripts/activate
```

### 2. ์ฅ๊ณ  ๋ฐ ๋ชจ๋ ์ค์น

```bash
$ pip install -r requirements.txt
```

### 3. ์ฅ๊ณ  ํ๋ก์ ํธ ์์ฑ ๋ฐ ์๋ฒ ์คํ

```bash
$ django-admin startproject pjt05 .
$ python manage.py runserver
```

### 4. ์ฑ ์์ฑ ๋ฐ `INSTALLED_APPS`์ ๋ฑ๋ก

```bash
$ python manage.py startapp movies
```

```python
# pjt05/settings.py

INSTALLED_APPS = [
    'movies',
    'django.contrib.admin',
    ....,
]
```

### 5. `models.py` - ํด๋์ค ์์ฑ

```python
# movies/models.py
from django.db import models

# Create your models here.
class Movie(models.Model):
    title = models.CharField(max_length=20)
    audience = models.IntegerField()
    release_date = models.DateField()
    genre = models.CharField(max_length=30)
    score = models.FloatField()
    poster_url = models.TextField()
    description = models.TextField()

    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

```bash
# ๋ง์ด๊ทธ๋ ์ด์ ์์ฑ
$ python manage.py makemigrations
```

```bash
# DB์ ๋ฐ์
$ python manage.py migrate
```

### 6. `urls.py` ๋ถ๋ฆฌ

```python
# movies/urls.py
from django.urls import path
from . import views

app_name = 'movies'   # app_name ์์ฑ

urlpatterns = [
    path('', views.index, name='index'),
]
```

```python
# pjt05/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('movies/', include('movies.urls')),
]
```

### 7. ๊ธฐ๋ณธ ํํ๋ฆฟ ์์ฑ, ๋ฑ๋ก

> ํ๋ก์ ํธ, app๊ณผ ๊ฐ์ ๋ ๋ฒจ์ `templates` ํด๋๋ฅผ ์์ฑ ํ `base.html` ์์ฑ

```html
# templates/base.html

<body>
  <div class="container">
  {% block content %}
  {% endblock content %}
  ......
</body>
```

```python
# pjt05/settings.py

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates',],
    	...,
    }
]
```

### + ์ถ๊ฐ

```python
# pjt05/settings.py

LANGUAGE_CODE = 'ko-kr'
TIME_ZONE = 'Asia/Seoul'
```

## index - ์ ์ฒด ์ํ ์กฐํ

```python
# movies/urls.py
from django.urls import path
from . import views

app_name = 'movies'

urlpatterns = [
    path('', views.index, name='index'),
]
```

```python
# movies/views.py

def index(request):
    return render(request, 'movies/index.html')
```

- ์ง๊ธ์ `index` ํจ์๊ฐ ๋จ์ํ `index.html`์ ๋ ๋๋ง ํ์ง๋ง, ์ถํ ์ฌ์ฉ์๊ฐ ์์ฑํด์ DB์ ์ ์ฅ๋์ด ์๋ ์ํ๋ค์ ์ ๋ณด๋ฅผ ๊ฐ์ ธ์์ `context`๋ก ๊ฐ์ด ๋ ๋๋ง ํด์ค ์์ 

## new - ์๋ก์ด ์ํ ์์ฑ ํ์ด์ง

```python
# movies/urls.py
urlpatterns = [
    ...,
    path('new', views.new, name='new'),
]
```

```python
# movies/views.py

def new(request):
    return render(request, 'movies/new.html')
```

```html
# new.html
<form action="{% url 'movies:create' %}" method="GET">
  <label for="title">Title</label>
  <input type="text" id="title" name="title" autofocus>
	....
</form>
```

- `new.html` ์์๋ `form` ํ๊ทธ๋ฅผ ํ์ฉํด์ ์ฌ์ฉ์๊ฐ ์ํ์ ์์ธ์ ๋ณด๋ฅผ ์๋ ฅํ๊ณ , ๊ทธ ๋ด์ฉ์ DB์ ์ ์ฅ์ ํด์ผํ๋ค. ๊ฐ ํญ๋ชฉ์ ๋ํด ์ฌ์ฉ์๋ค์ด ์์ฑํ ๋ด์ฉ์ `input`์ `name`์ผ๋ก ํ ๋น๋๊ณ   `action` ์์ฑ์ ๊ฐ์ผ๋ก ์๋ ฅํ `url`๋ก ์ด๋ํ๊ฒ ๋๋ค. ์ฌ๊ธฐ์๋ `movies/create`๋ก ์ด๋ํ๊ฒ ๋๋ค.  

### `number` - ์ ์ ์๋ ฅ๋ฐ๊ธฐ

```html
# new.html
<form action="{% url 'movies:create' %}" method="GET">
    ...
  <label for="score">Score</label>
  <input type="number" step="0.5" id="score" name="score">
	....
</form>
```

- ์ซ์๋ฅผ ์๋ ฅ๋ฐ์ ๋๋ `input` ์์ฑ์ `type`์ `number`๋ก ์ฃผ๋ฉด ๋๊ณ  `float`๋ก ์ฃผ๊ณ  ์ถ๋ค๋ฉด, ๋ค์ `step`๋ผ๋ ์์ฑ์ ์ฃผ๊ณ  ๊ฐ์ `0.5`๋ก ์ฃผ๋ฉด ๋๋ค!

### `date` - ๋ฌ๋ ฅ๋ชจ์ ํ์

```html
# new.html
<form action="{% url 'movies:create' %}" method="GET">
    ...
  <label for="release">Release_Date</label>
  <input type="date" id="release" name="release">
	....
</form>
```

- ๋ ์ง๋ฅผ ์๋ ฅ ๋ฐ๊ณ  ์ถ์ ๋๋ `input` ์์ฑ์ `type`์ `date`๋ก ์ฃผ๋ฉด ๋๋ค. ๊ทธ๋ผ ์ฐ๋ฆฌ๊ฐ ํํ ๋ณด๋ ๋ฌ๋ ฅ ๋ชจ์์ด ๋์ค๊ณ  ์ ํํ  ์ ์๊ฒ ๋๋ค. 

![image-20220313005900713](Django%20-%20%EA%B2%8C%EC%8B%9C%ED%8C%90.assets/image-20220313005900713.png)

### `select` - ์ ํ๋ชฉ๋ก ์์ฑ

```html
<form action="{% url 'movies:create' %}" method="GET">
    ...
  <select name="genre" id="genre">
    <option value="์ฝ๋ฏธ๋">์ฝ๋ฏธ๋</option>
    <option value="์ค๋ฆด๋ฌ">์ค๋ฆด๋ฌ</option>
    <option value="์ก์">์ก์</option>
    <option value="๊ฐ๋">๊ฐ๋</option>
  </select>
	....
</form>
```

- ์ ํ ๋ชฉ๋ก์ ์์ฑํ  ๋๋ `select` ํ๊ทธ๋ฅผ ์์ฑํ๊ณ  ํ์์ `option` ํ๊ทธ๋ฅผ ์์ฑํ๋ค. ์ฌ๊ธฐ์ `value` ๊ฐ์ด ์๋ฒ์ ์ ๋ฌ๋๋ค.

![image-20220313010552935](Django%20-%20%EA%B2%8C%EC%8B%9C%ED%8C%90.assets/image-20220313010552935.png)

### `textarea` - ๊ธด ๊ธ(?) ๋ฐ๋ ํ๊ทธ

```html
<form action="{% url 'movies:create' %}" method="GET">
    ...
	<label for="description">Description</label>
  	<textarea name="description" id="description" cols="30" rows="10"></textarea>
	....
</form>
```

- ์ผ๋ฐ์ ์ผ๋ก ์ข ๊ธด ๊ธ(?)์ ์์ฑํ  ๋๋ `input` ํ๊ทธ๋ฅผ ์ฌ์ฉํ๊ธฐ๋ณด๋ค๋ `textarea`์์ฑ์ ์ฌ์ฉํ๋ค. `cols`์ `rows` ๊ฐ์ ์์ ๋กญ๊ฒ ํด์ ์ฐ๋ฆฌ๊ฐ ๊ธ์ ์์ฑํ๋ ํ๋๋ฆฌ(?)์ ์ฌ์ด์ฆ๋ฅผ ์กฐ์ ํด์ค ์ ์๋ค.

## create - ์๋ ฅํ ์ํ ๋ฐ์ดํฐ ์ ์ฅ

```python
# movies/urls.py
urlpatterns = [
    ...,
    path('create', views.create, name='create'),
]
```

```python
# movies/views.py
from django.shortcuts import render, redirect
from .models import Movie

def create(request):
    title = request.GET.get('title')
	...
    # DB ์ ์ฅ
    movie = Movie()
    movie.title = title
	...
    movie.save()

    context = {
        'movie' : movie
    }
    return redirect('movies:detail', movie.pk)
```

- `request.GET`๋ฅผ ํ๋ฉด, ์ฌ์ฉ์๊ฐ ์๋ ฅํ ๊ฐ๋ค์ด `QueryDict`ํํ๋ก ๋ค์ด ์ค๊ธฐ ๋๋ฌธ์ `get` ๋ฉ์๋๋ฅผ ์ฌ์ฉํ๋๋ฐ ๊ดํธ ์์ ๋ค์ด๊ฐ๋ ๋ด์ฉ์ `input`์์ `name`์ผ๋ก ์ฃผ์๋ ๊ฐ์ด๋ค.
- ์ฌ์ฉ์๊ฐ ์๋ ฅํ ๊ฐ์ ๋ฐ์์ค๋ฉด ์ด๋ฅผ DB์ ์ ์ฅํด์ฃผ์ด์ผ ํ๋ค. ๋ฐ๋ผ์ `models.py`์์ ์์ฑํ๋ `Movie` ํด๋์ค๋ฅผ ๊ฐ์ ธ์์ `movie = Movie()`๋ฅผ ํตํด `movie` ์ธ์คํด์ค๋ฅผ ์์ฑํ๊ณ  ์ฌ์ฉ์๋ก๋ถํฐ ์๋ ฅ๋ฐ์ ๊ฐ์ ๊ฐ๊ฐ ํ ๋นํ๊ณ  ์ ์ฅํ๋ค. 
- `create` ํจ์๋ `create.html`๋ก ๋ ๋๋ง์ ํ๋ ๊ฒ์ด ์๋๋ผ `(movies:detail, pk)` ์ ๋ฆฌ๋ค์ด๋ ํธ ํ๋๋ฐ, `new.html`์์ ์ฌ์ฉ์๊ฐ ์๋ ฅ์ ํ๊ณ  ์ ์ถ์ ํ๋ฉด, ์๋ ฅํ ๋ด์ฉ์ ๋ณด์ฌ์ฃผ๋ `detail.html`์ด ํ๋ฉด์ ๋ ๋๋ง ๋์ด์ผ ํ๋ค.  ๋ฐ๋ผ์ `create`์์๋ ์ฌ์ฉ์๊ฐ ์์ฑํ ๋ฐ์ดํฐ๋ค์ DB์ ์ ์ฅ๋ง ํ๊ฒ ๋๊ณ , ์์ฑ๋๋ `pk` ๊ฐ์ ํตํด `(movies:detail, pk)` ์ ๋ฆฌ๋ค์ด๋ ํธ ํ๋ฉด์, `pk`์ ํด๋นํ๋ ์ํ ๋ฐ์ดํฐ์ ์์ธ์ ๋ณด๊ฐ ํ๋ฉด์ ๋ณด์ฌ์ง ์ ์๋๋ก  `detail.html`์ ๋ฐ๋ก ์์ฑํด์ผ ํ๋ค.

## detail - ๋จ์ผ ์ํ ๋ฐ์ดํฐ ์กฐํ

```python
# movies/urls.py
urlpatterns = [
    ...,
    path('<int:pk>', views.detail, name='detail'),
]
```

- `detail`์์๋ ๋จ์ผ ์ํ ๋ฐ์ดํฐ๋ฅผ ํ๋ฉด์ ๋ณด์ฌ์ฃผ๊ฒ ๋๋๋ฐ, ์ํ๋ง๋ค ๋ค๋ฅธ ํ๋ฉด์ด ๋ฟ๋ ค์ง๊ฒ ๋๊ณ  ์ผ์ผํ `url`์ ์์ฑํ๋ ๊ฒ์ด ์๋๋ผ, `url` ์ฃผ์๋ฅผ ๋ณ์๋ก ์ง์ ํด์ `view` ํจ์์ ์ธ์๋ก ๋๊ฒจ, ํ๋์ `path`๋ก ์ฌ๋ฌ ํ์ด์ง๋ฅผ ์ฐ๊ฒฐ์ํค๋ `Variable Routing` ๋ฐฉ๋ฒ์ ์ฌ์ฉํ๋ค. 

```python
# movies/views.py

def detail(request, pk):
    movie = Movie.objects.get(pk=pk)
    context = {
        'movie' : movie
    }
    return render(request, 'movies/detail.html', context)
```

- `movie = Movie.objects.get(pk=pk)` ์ ํตํด DB์์ Movie์ ํ๋ํ์์ ์์ฑ์ผ๋ก ํ๋ ํ์ด๋ธ์์ `pk`๊ฐ ์ธ์๋ก ์๋ ฅ๋๋ `pk`๊ฐ์ผ ๋์ ๋ฐ์ดํฐ๋ค์ `movie`์ ๋ด์์ค๋ค. ์ด ๋ฐ์ดํฐ๋ค์ ๊ฐ์ง๊ณ  `detail.html`์ ๋ ๋๋งํ๋ค.

![image-20220313015245406](Django%20-%20%EA%B2%8C%EC%8B%9C%ED%8C%90.assets/image-20220313015245406.png)

- `pk` ๊ฐ์ ๋ฐ๋ผ ์์ฑํ๋ ์ํ ๋ฐ์ดํฐ๋ฅผ ๊ธฐ๋ฐ์ผ๋ก ํ `detail.html` ํ๋ฉด์ผ๋ก ์๋ตํ๊ฒ ๋๋ค. 

## edit - ์์  ๋์ ์ํ ๋ฐ์ดํฐ ์กฐํ

```python
# movies/urls.py
urlpatterns = [
    ...,
     path('<int:pk>/edit', views.edit, name='edit'),
]
```

```python
# movies/views.py

def edit(request, pk):
    movie = Movie.objects.get(pk=pk)
    genres = ['์ฝ๋ฏธ๋', '๊ฐ๋', '์ค๋ฆด๋ฌ', '์ก์']

    context = {
        'movie' : movie,
        'genres' : genres
    }
    return render(request,'movies/edit.html', context)
```

- `detail.html`์์ `Edit` ๋ฒํผ์ ๋๋ฅด๊ฒ ๋๋ฉด `movies/pk/edit` ์ฃผ์๋ก ์ด๋ํ๊ฒ ๋๊ณ  ๊ทธ๋ `edit` ํจ์๋ `edit.html`์ ๋ ๋๋งํ๊ฒ ๋๋ค.
- ์ฌ๊ธฐ์ ๋ฐ๋ก `genres`๋ฅผ ๋ฆฌ์คํธ๋ก ๋ง๋ค์ด์ค ์ด์ ๋ `input` ํ๊ทธ์์ ์ด์ ์ ์์ฑํ๋ ๋ด์ฉ์ ๊ทธ๋๋ก ํ๋ฉด์ ๋ณด์ฌ์ฃผ๋ ค๋ฉด `value` ์์ฑ์ ์ด์ ์ ์์ฑํ ๋ด์ฉ์ ๊ฐ์ผ๋ก ์ฃผ๋ฉด ๋์ง๋ง, `select` ํ๊ทธ์์๋ ์กฐ๊ธ ๋ค๋ฅธ ๋ฐฉ์์ผ๋ก ์ ๊ทผํด์ผ ํ๋ค.

```html
# edit.html
<form action="{% url 'movies:update' movie.pk %}" method="GET">
    ....
    <select name="genre" id="genre" size="4">
        {% for genre in genres %}
          {% if genre == movie.genre %}
            <option value="{{genre}}" selected>{{genre}}</option>
          {% else %}    
            <option value="{{genre}}">{{genre}}</option>
          {% endif %}
        {% endfor %}    
     </select>
</form>
```

- ๋ค์๊ณผ ๊ฐ์ด `selected`๋ฅผ ํตํด์ ์ด์ ์ ์๋ ฅํ๋ ๊ฐ์ ํ๋ฉด์ ๊ทธ๋๋ก ๋ณด์ฌ์ค ์ ์๋ค.

- ์ถ๊ฐ์ ์ผ๋ก `select` ํ๊ทธ์ `size` ์์ฑ์ ์ฃผ๊ฒ ๋๋ฉด ํ๋ฉด์ ์ ํ ๋ชฉ๋ก๋ค์ด ์ ํ์์ง ์๊ณ  ์ด๋ ค์๊ฒ ๋๋ค. ์ด๊ฑฐ ์ ๊ณ  ์ถ์ด์ ์์ฒญ ์ฐพ์๋ดค๋๋ฐ... ๊ทธ๋ฅ `size` ์์ฑ ์์ฃผ๋ฉด ๋๋ค....

![image-20220313010552935](Django%20-%20%EA%B2%8C%EC%8B%9C%ED%8C%90.assets/image-20220313010552935-16471106518311.png)

## update - ์ํ ๋ฐ์ดํฐ ์์ 

```python
# movies/urls.py
urlpatterns = [
    ...,
    path('<int:pk>/update', views.update, name="update"),
]
```

```python
# movies/views.py

def update(request, pk):
    title = request.GET.get('title')
	...
    # DB ์์ 
    movie = Movie.objects.get(pk=pk)
    movie.title = title
	...
    # ์ ์ฅ
    movie.save()

    return redirect('movies:detail', movie.pk)
```

- `create`์ ๋ง์ฐฌ๊ฐ์ง๋ก ์์ ํ ๋ด์ฉ์ DB์ ์ ์ฅํ๋ ๊ณผ์ ์ด ํ์ํ๋ค. ์์ ํ ๋ด์ฉ์ ์ ์ถํ๊ฒ ๋๋ฉด `movies:create`๋ก ์ด๋ํ๊ณ  `create` ํจ์๋ฅผ ์คํํ๊ฒ ๋๋๋ฐ ์ฌ๊ธฐ์ ์์ ํ๋ ๋ด์ฉ์ DB์ ์ ์ฅํ๊ณ  `movies:deatil`์ ๋ฆฌ๋ค์ด๋ ํธ ํ๋ค. ์ด ๊ณผ์ ์ ํตํด ์ฌ์ฉ์๊ฐ ์ํ ์์  ํ ์ ์ถ์ ํ๋ฉด ์์ ํ ๋ด์ฉ์ด ๋ฐ์๋ `detail.html`์ด ํ๋ฉด์ ๋ณด์ด๊ฒ ๋๋ค.

## delete - ๋จ์ผ ์ํ ๋ฐ์ดํฐ ์ญ์ 

```python
# movies/urls.py
urlpatterns = [
    ...,
    path('<int:pk>/delete/', views.delete, name='delete'),
]
```

```python
# movies/views.py

def delete(request, pk):
    movie = Movie.objects.get(pk=pk)
    movie.delete()
    return redirect('movies:index')
```

- `detail.html`์์ `delete` ๋ฒํผ์ ๋๋ฅด๊ฒ ๋๋ฉด `movies:delete`๋ก ์ด๋ํด์ `delete` ํจ์๊ฐ ์คํํ๊ฒ ๋๋ค. ์ด ํจ์ ์์์ pk์ ํด๋นํ๋ ์ํ ๋ฐ์ดํฐ๊ฐ DB์์ ์ญ์ ํ๊ฒ ๋๊ณ  ์ด๋ฅผ ๋ฐ์ํด์ผ ํ๊ธฐ ๋๋ฌธ์ `redirect`๋ฅผ ํ๊ฒ ๋๋ค. 

## index - ์ ์ฒด ์ํ ์กฐํ

```python
# movies/index.py

def index(request):
    movies = Movie.objects.all()
    context ={
        'movies' : movies
    }
    return render(request, 'movies/index.html', context)
```

- DB์ ์ ์ฅ๋์ด ์๋ `Movie` ํ์ด๋ธ(?)์ ์๋ ๋ชจ๋  ๋ด์ฉ์ ๊ฐ์ ธ์จ๋ค.

```html
# index.html

{% block content %}
	...
    {% for movie in movies %}
      <a href="{% url 'movies:detail' movie.pk  %}">{{ movie.title }}</a>
      <br>
      <p>{{ movie.score }}</p>
      <hr>
    {% endfor %}
	...
{% endblock content %}
```

- ์ ์ผ ์ฒ์ `index.html`์ ์์ฑํ์ ๋๋ DB์ ์ ์ฅ๋์ด ์๋ ๋ด์ฉ์ด ์์๊ธฐ ๋๋ฌธ์ ์กฐํ๋ฅผ ํ  ์ ์์๋ค. ๋ชจ๋  ํ๋ฉด๊ณผ ํจ์๊ฐ ๊ตฌ์ฑ๋ ์ง๊ธ DB์์ ๊ฐ์ ธ์จ ๊ฐ์ ํตํด ํ๋ฉด์ ๋ค์๊ณผ ๊ฐ์ด ๊ตฌ์ฑํ  ์ ์๋ค.

![image-20220313022032836](Django%20-%20%EA%B2%8C%EC%8B%9C%ED%8C%90.assets/image-20220313022032836.png)

## Admin ํ์ด์ง ์์ฑ

### `admin` ๊ณ์  ์์ฑ

```bash
# ๊ด๋ฆฌ์ ๊ณ์  ์์ฑ
$ python manage.py createsuperuser
```

![image-20220311003026402](Django%20-%20%EA%B2%8C%EC%8B%9C%ED%8C%90.assets/image-20220311003026402.png)

### `admin` ๋ฑ๋ก

```python
# movies/admin.py
from django.contrib import admin
from .models import Movie

class MovieAdmin(admin.ModelAdmin):
    list_display = ('pk', 'title', 'release_date', 'genre', 'score', 'poster_url', 'description')

# Register your models here.
admin.site.register(Movie)
```

- `list_display`์ `models.py`์์ ์ ์ํ ๊ฐ๊ฐ์ ์์ฑ๋ค์ ๋ ์ฝ๋๋ฅผ `admin`ํ์ด์ง์ ์ถ๋ ฅํ๋๋ก ํด์ค๋ค!

![image-20220313022931474](Django%20-%20%EA%B2%8C%EC%8B%9C%ED%8C%90.assets/image-20220313022931474.png)





