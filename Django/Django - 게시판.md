# 🎞영화 게시판 기능 구현(+ django)

> django를 통한 커뮤니티 서비스의 게시판 기능을 개발하는 단계로, 영화 데이터의 생성, 조회, 수정, 삭제가 가능한 어플리케이션을 완성하는 것을 목표로 한다. 



## 환경 설정 및 기본 세팅

### 1. 가상환경 생성 및 활성화

```bash
$ python -m venv venv
$ source venv/Scripts/activate
```

### 2. 장고 및 모듈 설치

```bash
$ pip install -r requirements.txt
```

### 3. 장고 프로젝트 생성 및 서버 실행

```bash
$ django-admin startproject pjt05 .
$ python manage.py runserver
```

### 4. 앱 생성 및 `INSTALLED_APPS`에 등록

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

### 5. `models.py` - 클래스 생성

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
# 마이그레이션 생성
$ python manage.py makemigrations
```

```bash
# DB에 반영
$ python manage.py migrate
```

### 6. `urls.py` 분리

```python
# movies/urls.py
from django.urls import path
from . import views

app_name = 'movies'   # app_name 생성

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

### 7. 기본 템플릿 생성, 등록

> 프로젝트, app과 같은 레벨에 `templates` 폴더를 생성 후 `base.html` 생성

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

### + 추가

```python
# pjt05/settings.py

LANGUAGE_CODE = 'ko-kr'
TIME_ZONE = 'Asia/Seoul'
```

## `index` - 전체 영화 조회

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

- 지금은 `index` 함수가 단순히 `index.html`을 렌더링 하지만, 추후 사용자가 작성해서 DB에 저장되어 있는 영화들의 정보를 가져와서 `context`로 같이 렌더링 해줄 예정

## `new` - 새로운 영화 작성 페이지

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

- `new.html` 에서는 `form` 태그를 활용해서 사용자가 영화의 상세정보를 입력하고, 그 내용을 DB에 저장을 해야한다. 각 항목에 대해 사용자들이 작성한 내용은 `input`의 `name`으로 할당되고  `action` 속성의 값으로 입력한 `url`로 이동하게 된다. 여기서는 `movies/create`로 이동하게 된다.  

### `number` - 정수 입력받기

```html
# new.html
<form action="{% url 'movies:create' %}" method="GET">
    ...
  <label for="score">Score</label>
  <input type="number" step="0.5" id="score" name="score">
	....
</form>
```

- 숫자를 입력받을 때는 `input` 속성의 `type`을 `number`로 주면 되고 `float`로 주고 싶다면, 뒤에 `step`라는 속성을 주고 값을 `0.5`로 주면 된다!

### `date` - 달력모양 표시

```html
# new.html
<form action="{% url 'movies:create' %}" method="GET">
    ...
  <label for="release">Release_Date</label>
  <input type="date" id="release" name="release">
	....
</form>
```

- 날짜를 입력 받고 싶을 때는 `input` 속성의 `type`을 `date`로 주면 된다. 그럼 우리가 흔히 보는 달력 모양이 나오고 선택할 수 있게 된다. 

![image-20220313005900713](Django%20-%20%EA%B2%8C%EC%8B%9C%ED%8C%90.assets/image-20220313005900713.png)

### `select` - 선택목록 생성

```html
<form action="{% url 'movies:create' %}" method="GET">
    ...
  <select name="genre" id="genre">
    <option value="코미디">코미디</option>
    <option value="스릴러">스릴러</option>
    <option value="액션">액션</option>
    <option value="감동">감동</option>
  </select>
	....
</form>
```

- 선택 목록을 생성할 때는 `select` 태그를 생성하고 하위에 `option` 태그를 생성한다. 여기서 `value` 값이 서버에 전달된다.

![image-20220313010552935](Django%20-%20%EA%B2%8C%EC%8B%9C%ED%8C%90.assets/image-20220313010552935.png)

### `textarea` - 긴 글(?) 받는 태그

```html
<form action="{% url 'movies:create' %}" method="GET">
    ...
	<label for="description">Description</label>
  	<textarea name="description" id="description" cols="30" rows="10"></textarea>
	....
</form>
```

- 일반적으로 좀 긴 글(?)을 작성할 때는 `input` 태그를 사용하기보다는 `textarea`속성을 사용한다. `cols`와 `rows` 값을 자유롭게 해서 우리가 글을 작성하는 테두리(?)의 사이즈를 조정해줄 수 있다.

## `create` - 입력한 영화 데이터 저장

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
    # DB 저장
    movie = Movie()
    movie.title = title
	...
    movie.save()

    context = {
        'movie' : movie
    }
    return redirect('movies:detail', movie.pk)
```

- `request.GET`를 하면, 사용자가 입력한 값들이 `QueryDict`형태로 들어 오기 때문에 `get` 메서드를 사용하는데 괄호 안에 들어가는 내용은 `input`에서 `name`으로 주었던 값이다.
- 사용자가 입력한 값을 받아오면 이를 DB에 저장해주어야 한다. 따라서 `models.py`에서 생성했던 `Movie` 클래스를 가져와서 `movie = Movie()`를 통해 `movie` 인스턴스를 생성하고 사용자로부터 입력받은 값을 각각 할당하고 저장한다. 
- `create` 함수는 `create.html`로 렌더링을 하는 것이 아니라 `(movies:detail, pk) ` 을 리다이렉트 하는데, `new.html`에서 사용자가 입력을 하고 제출을 하면, 입력한 내용을 보여주는 `detail.html`이 화면에 렌더링 되어야 한다.  따라서 `create`에서는 사용자가 작성한 데이터들을 DB에 저장만 하게 되고, 생성되는 `pk` 값을 통해 `(movies:detail, pk) ` 을 리다이렉트 하면서, `pk`에 해당하는 영화 데이터의 상세정보가 화면에 보여질 수 있도록  `detail.html`을 따로 생성해야 한다.

## `detail` - 단일 영화 데이터 조회

```python
# movies/urls.py
urlpatterns = [
    ...,
    path('<int:pk>', views.detail, name='detail'),
]
```

- `detail`에서는 단일 영화 데이터를 화면에 보여주게 되는데, 영화마다 다른 화면이 뿌려지게 되고 일일히 `url`을 생성하는 것이 아니라, `url` 주소를 변수로 지정해서 `view` 함수의 인자로 넘겨, 하나의 `path`로 여러 페이지를 연결시키는 `Variable Routing` 방법을 사용한다. 

```python
# movies/views.py

def detail(request, pk):
    movie = Movie.objects.get(pk=pk)
    context = {
        'movie' : movie
    }
    return render(request, 'movies/detail.html', context)
```

- `movie = Movie.objects.get(pk=pk)` 을 통해 DB에서 Movie의 필드타입을 속성으로 하는 테이블에서 `pk`가 인자로 입력되는 `pk`값일 때의 데이터들을 `movie`에 담아준다. 이 데이터들을 가지고 `detail.html`을 렌더링한다.

![image-20220313015245406](Django%20-%20%EA%B2%8C%EC%8B%9C%ED%8C%90.assets/image-20220313015245406.png)

- `pk` 값에 따라 작성했던 영화 데이터를 기반으로 한 `detail.html` 화면으로 응답하게 된다. 

## `edit` - 수정 대상 영화 데이터 조회

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
    genres = ['코미디', '감동', '스릴러', '액션']

    context = {
        'movie' : movie,
        'genres' : genres
    }
    return render(request,'movies/edit.html', context)
```

- `detail.html`에서 `Edit` 버튼을 누르게 되면 `movies/pk/edit` 주소로 이동하게 되고 그때 `edit` 함수는 `edit.html`을 렌더링하게 된다.
- 여기서 따로 `genres`를 리스트로 만들어준 이유는 `input` 태그에서 이전에 작성했던 내용을 그대로 화면에 보여주려면 `value` 속성에 이전에 작성한 내용을 값으로 주면 되지만, `select` 태그에서는 조금 다른 방식으로 접근해야 한다.

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

- 다음과 같이 `selected`를 통해서 이전에 입력했던 값을 화면에 그대로 보여줄 수 있다.

- 추가적으로 `select` 태그에 `size` 속성을 주게 되면 화면에 선택 목록들이 접혀있지 않고 열려있게 된다. 이거 접고 싶어서 엄청 찾아봤는데... 그냥 `size` 속성 안주면 된다....

![image-20220313010552935](Django%20-%20%EA%B2%8C%EC%8B%9C%ED%8C%90.assets/image-20220313010552935-16471106518311.png)

## `update` - 영화 데이터 수정

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
    # DB 수정
    movie = Movie.objects.get(pk=pk)
    movie.title = title
	...
    # 저장
    movie.save()

    return redirect('movies:detail', movie.pk)
```

- `create`와 마찬가지로 수정한 내용을 DB에 저장하는 과정이 필요하다. 수정한 내용을 제출하게 되면 `movies:create`로 이동하고 `create` 함수를 실행하게 되는데 여기서 수정했던 내용을 DB에 저장하고 `movies:deatil`을 리다이렉트 한다. 이 과정을 통해 사용자가 영화 수정 후 제출을 하면 수정한 내용이 반영된 `detail.html`이 화면에 보이게 된다.

## `delete` - 단일 영화 데이터 삭제

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

- `detail.html`에서 `delete` 버튼을 누르게 되면 `movies:delete`로 이동해서 `delete` 함수가 실행하게 된다. 이 함수 안에서 pk에 해당하는 영화 데이터가 DB에서 삭제하게 되고 이를 반영해야 하기 때문에 `redirect`를 하게 된다. 

## `index` - 전체 영화 조회

```python
# movies/index.py

def index(request):
    movies = Movie.objects.all()
    context ={
        'movies' : movies
    }
    return render(request, 'movies/index.html', context)
```

- DB에 저장되어 있는 `Movie` 테이블(?)에 있는 모든 내용을 가져온다.

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

- 제일 처음 `index.html`을 생성했을 때는 DB에 저장되어 있는 내용이 없었기 때문에 조회를 할 수 없었다. 모든 화면과 함수가 구성된 지금 DB에서 가져온 값을 통해 화면을 다음과 같이 구성할 수 있다.

![image-20220313022032836](Django%20-%20%EA%B2%8C%EC%8B%9C%ED%8C%90.assets/image-20220313022032836.png)

## Admin 페이지 생성

### `admin` 계정 생성

```bash
# 관리자 계정 생성
$ python manage.py createsuperuser
```

![image-20220311003026402](Django%20-%20%EA%B2%8C%EC%8B%9C%ED%8C%90.assets/image-20220311003026402.png)

### `admin` 등록

```python
# movies/admin.py
from django.contrib import admin
from .models import Movie

class MovieAdmin(admin.ModelAdmin):
    list_display = ('pk', 'title', 'release_date', 'genre', 'score', 'poster_url', 'description')

# Register your models here.
admin.site.register(Movie)
```

- `list_display`은 `models.py`에서 정의한 각각의 속성들의 레코드를 `admin`페이지에 출력하도록 해준다!

![image-20220313022931474](Django%20-%20%EA%B2%8C%EC%8B%9C%ED%8C%90.assets/image-20220313022931474.png)





