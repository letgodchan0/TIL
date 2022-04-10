# 🌱 Media files (이미지 업로드)

## FileField()

- 파일 업로드에 사용하는 모델 필드

- 2개의 선택 인자를 가지고 있음

  1. `upload_to`
  2. `storage`

  

## ImageField()

- 이미지 업로드에 사용하는 모델 필드
- `FileField`를 상속받는 서브 클래스이기 때문에 `FileField`의 모든 속성 및 메서드를 사용 가능하며, 더해서 사용자에 의해 업로드 된 객체가 유효한 이미지인지 검사함
- `ImageField` 인스턴스는 최대 길이가 100자인 문자열로 DB에 생성되며, `max_length` 인자를 사용하여 최대 길이를 변경할 수 있음. 그런데 왜 100자인 문자열이 DB에 저장되냐면, 파일 자체를 저장하면 DB에 영향을 많이 끼치기 때문에 파일 자체를 DB에 삽인하는 것이 아니라 파일 경로를 삽입하기 때문이다.

[주의] 사용하려면 반드시 `Pillow` 라이브러리가 필요!!!

```python
# articles/models.py

class Article(models.Model):
    # saved to "MEDIA_ROOT/images/"
    image = models.ImageField(upload_to='images/', blank=True)
```

- `upload_to = 'images/'`

  - 실제 이미지가 저장되는 경로를 지정

- `blank = True`

  - 이미지 필드에 빈 값(빈 문자열)이 허용되도록 설정 (이미지를 선택적으로 업로드할 수 있도록)

  

### `'upload_to'` argument (2가지 방식)

> 'upload_to' argument - 1. 문자열 경로 지정 방식

- 파이썬의 `strftime()` 형식이 포함될 수 있으며, 이는 파일 업로드 날짜/시간으로 대체 됨

```python
# models.py

class MyModel(models.Model):
    # MEDIA_ROOT/uploads/ 경로로 파일 업로드
    upload = models.FileField(upload_to='uploads/')
    
    # 또는
    
    # MEDIA_ROOT/uploads/2021/01/01 경로로 파일 업로드
    upload = models.FileField(upload_to='uploads/%Y/%m/%d')
```

##### [참고] time 모듈의 strftime()

- 날짜 및 시간 객체를 문자열 표현으로 변환하는데 사용됨
- 하나 이상의 형식화된 코드 입력을 받아 문자열 표현을 반환

```python
from time import gmtime, strftime

strftime('%a, %d %b %Y %H:%M:%S', gmtime())

# 'Thu, 28 Jun 2022 14:17:15'
```



> 'upload_to' argument - 2. 함수 호출 방식

- 반드시 2개의 인자 (instance, filename)를 사용해야 한다.

1. `instance`
   - FileField가 정의된 모델의 인스턴스
   - 대부분 이 객체는 아직 데이터베이스에 저장되지 않았으므로 PK 값이 아직 없을 수 있음
2. `filename`
   - 기존 파일에 제공된 파일 이름

```python
# models.py

def articles_image_path(instance, filename):
    # 'MEDIA_ROOT/image_<pk>/' 경로에 '<filename>' 이름으로 업로드
    return f'image_{intsance.pk}/{filename}'

class Article(models.Model):
    image = models.ImageField(upload_to = articles_image_path)
```



### Model field option - "blank"

- 기본 값 : False
- Validation-related
- True인 경우 필드를 비워 둘 수 있음, DB에는 ''(빈 문자열)이 저장됨
- 유효성 검사에서 사용 됨 (is_valid)
  - 모델 필드에 blank=True를 작성하면 form 유효성 검사에서 빈 값을 입력할 수 있음

### Model field option - "null"

- 기본 값 : False
- Database-related
- True인 경우 django는 빈 값에 대해 DB에 NULL로 저장
- 주의 사항
  - `CharField`, `TextField`와 같은 문자열 기반 필드에는 사용하는 것을 피해야 함
  - 문자열 기반 필드에 `True`로 설정 시 '데이터 없음(no data)'에 ''빈 문자열('')'과 'NULL'의 2가지 가능한 값이 있음을 의미하게 됨
  - 대부분의 경우 "데이터 없음"에 대해 두 개의 가능한 값을 갖는 것은 중복되는 것이며, Django는 NULL이 아닌 빈 문자열을 사용하는 것이 규칙



> 다시 한번 주의!
>
> 문자열 기반 및 비문자열 기반 필드 모두에 대해 null option은 DB에만 영향을 미치므로, form에서 빈 값을 허용하려면 blank =True을 설정해야 한다!

```python
# blank & null 비교
# models.py

class Person(models.Model):
    name = models.CharField(max_length=10)
    
    # null = True 금지
    bio = models.TextField(max_length=50, blank = True)
    
    # null, blank 모두 설정 가능 -> 문자열 기반 필드가 아니기 때문
    birth_date = models.DateField(null=True, blank=True)
```



## ImageField (or FileField)를 사용하기 위한 몇 가지 단계

1. `settings.py`에 `MEDIA_ROOT`, `MEDIA_URL` 설정
2. `upload_to` 속성을 정의하여 업로드 된 파일에 사용할 `MEDIA_ROOT`의 하위 경로를 지정
3. 업로드 된 파일의 경로는 django가 제공하는 `url` 속성을 통해 얻을 수 있음

![image-20220410202328407](Django%20-%20Media%20files.assets/image-20220410202328407.png)



#### MEDIA_ROOT

- 사용자가 업로드 한 파일(미디어 파일)들을 보관할 디렉토리의 절대 경로
- Django는 성능을 위해 업로드 파일은 데이터베이스에 저장하지 않음
  - 실제 데이터베이스에 저장되는 것은 파일의 경로
- [주의] `MEDIA_ROOT`는 `STATIC_ROOT`와 반드시 다른 경로로 지정해야 함

```PYTHON
# settings.py 제일 밑

MEDIA_ROOT = BASE_DIR / 'media'
```

#### MEDIA_URL

- `MEDIA_ROOT`에서 제공되는 미디어를 처리하는 URL
- 업로드 된 파일의 주소(URL)를 만들어 주는 역할
  - 웹 서버 사용자가 사용하는 public URL
- 비어 있지 않은 값으로 설정 한다면 반드시 slash(/)로 끝나야 함
- [주의] `MEDIA_URL`은 `STATIC_URL`과 반드시 다른 경로로 지정해야 함

```PYTHON
# settings.py MEDIA_ROOT 밑에

MEDIA_URL = '/media/'
```



### 개발 단계에서 사용자가 업로드 한 파일 제공하기

- `settings.MEDIA_URL`
  - 업로드 된 파일의 URL
- `settings.MEDIA_ROOT`
  - `MEDIA_URL`을 통해 참조하는 파일의 실제 위치

![image-20220410202539726](Django%20-%20Media%20files.assets/image-20220410202539726.png)

```PYTHON
# 프로젝트/urls.py

from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```



> 여기까지 완료하고 마이그레이션을 실행하면 오류가 뜰 수 있다. 이유는 ImageField를 사용하기 위해서는 Pillow 라이브러리 설치가 필요하기 때문!

```bash
# Pillow 라이브러리 설치 및 마이그레이션 진행

$ pip install Pillow

$ python manage.py makemigrations
$ python manage.py migrate

$ pip freeze > requirements.txt
```



## 이미지 업로드 (CREATE)

1. `create.html`의 `enctype` 속성 추가

```html
<--! articles/create.html -->
    
<form action="{% url 'articles:create' %}" method="POST" enctype="multipart/form-data">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
</form>    
```

- `enctype` 속성

> multipart / form-data
>
> 파일 / 이미지 업로드 시에 반드시 사용해야 함 (전송되는 데이터 형식을 지정)
>
> <input type = "file">을 사용할 경우에 사용

- input 요소 - `accept` 속성

  - 입력을 허용할 파일 유형을 나타내는 문자열
  - 쉼표로 구분된 "고유 파일 유형 지정자"
  - 파일을 검증 하는 것은 아님
    - accept 속성 값을 image라고 하더라도 비디오나 오디오 및 다른 형식 파일을 제출할 수 있음
  - 고유 파일 유형 지정자
    - `<input type="file">` 에서 선택할 수 있는 파일의 종류를 설명하는 문자열

  ![image-20220410214834753](Django%20-%20Media%20files.assets/image-20220410214834753.png)



2. `views.py` 수정

- 업로드한 파일은 `request.FILES` 객체로 전달된다.

```PYTHON
# views.py

def create(request):
    if request.method == 'POST':
        form = ArticleForm(request.POST, request.FILES)
        # form = ArticleForm(request.POST, files=request.FILES)
        # form = ArticleForm(data=request.POST, files=request.FILES)
        
        if form.is_valid():
            article = form.save()
            return redirect('articles:detail', article.pk)
    else:
        form = ArticleForm()
    context = {
        'form': form,
    }
    return render(request, 'articles/create.html', context)

```

3. `DB` 및 파일 트리 확인

- 실제 파일 위치

  - `MEDIA_ROOT/images/`

  ![image-20220410215307972](Django%20-%20Media%20files.assets/image-20220410215307972.png)

- `DB`에 저장되는 것은 이미지 파일 자체가 아닌 파일의 경로

![image-20220410215329568](Django%20-%20Media%20files.assets/image-20220410215329568.png)

## 이미지 경로 불러오기

- `article.image.url`
  - 업로드 파일의 경로
- `article.image`
  - 업로드 파일의 파일 이름

```html
<--! detail.html -->

{% extends 'base.html' %}

{% block content %}
  <h1>DETAIL</h1>
  <h3>{{ article.pk }}번째 글</h3>
  <img src="{{ article.image.url }}" alt="{{ article.image }}">
  <hr>
  ...
{% endblock content %}
```

> static, media 파일 모두 서버에 요청해서 조회하는 것이기 때문에 둘다 url이 필요하다!

## 이미지 업로드 (UPDATE)

- 이미지는 바이너리 데이터(하나의 덩어리)이기 때문에 텍스트처럼 일부만 수정하는 것은 불가능
- 때문에 새로운 사진으로 덮어 씌우는 방식을 사용한다.

```html
<--! update.html -->
    
{% extends 'base.html' %}

{% block content %}
  <h1>UPDATE</h1>
  <hr>
  <form action="{% url 'articles:update' article.pk %}" method="POST" enctype="multipart/form-data">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
  <a href="{% url 'articles:detail' article.pk %}">back</a>
{% endblock content %}

```

```python
# views.py

def update(request, pk):
    article = Article.objects.get(pk=pk)
    if request.method == 'POST':
        form = ArticleForm(request.POST, request.FILES, instance=article)
        if form.is_valid():
            article = form.save()
            return redirect('articles:detail', article.pk)
    else:
        form = ArticleForm(instance=article)
    context = {
        'article': article,
        'form': form,
    }
    return render(request, 'articles/update.html', context)

```

> 이런식으로 진행을 하면 이미지가 없이 작성된 게시글은 `detail` 페이지가 출력되지 않는 문제가 발생한다. `image`가 없는 게시글의 경우 출력할 이미지 경로가 없기 때문인데, 그렇다면 `detail` 페이지에서 DTL을 통해 이미지 경로가 있을 경우만 출력하는 구문으로 변경해주면 된다.

```HTML
<--! detail.html -->

{% extends 'base.html' %}

{% block content %}
  <h1>DETAIL</h1>
  <h3>{{ article.pk }}번째 글</h3>
  {% if article.image %}
    <img src="{{ article.image.url }}" alt="{{ article.image }}">
  {% endif %}
  <hr>
  ...
{% endblock content %}
```

## 이미지 Resizing

- 실제 원본 이미지를 서버에 그대로 업로드 하는 것은 서버의 부담이 큰 작업
- `<img>` 태그에서 직접 사이즈를 조정할 수도 있지만 (width와 height 속성 사용), 업로드 될 때 이미지 자체를 `resizing` 하는 것을 고려하기
- `django-imagekit` 라이브러리 활용

1. `django-imagekit` 설치

```bash
$ pip install django-imagekit
$ pip freeze > requirements.txt
```

2. `INSTALLED_APPS`에 추가

```PYTHON
# settings.py

INSTALLED_APP = [
    ...
    'imagekit',
    ...
]
```

### 원본 이미지를 재가공하여 저장 (원본 X, 썸네일 O)

```python
# models.py
from distutils.command.upload import upload
from django.db import models
# 아래 2개 import
from imagekit.models import ProcessedImageField
from imagekit.processors import Thumbnail

# Create your models here.
class Article(models.Model):
    title = models.CharField(max_length=10)
    content = models.TextField()
    # image = models.ImageField(upload_to='images/', blank=True)
    image = ProcessedImageField(
        blank=True,
        upload_to='thumbnails/',
        processors=[Thumbnail(200, 300)],
        format='JPEG',
        options={'quality': 60}
    )
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title

```

- `ProcessedImageField()`의 파라미터로 작성된 값들은 변경하더라도 다시 `makemigrations`를 해줄 필요 없이 즉시 반영 된다.

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```



### 원본 이미지를 재가공하여 저장 (원본 O, 썸네일 O)

```PYTHON
# models.py

from distutils.command.upload import upload
from django.db import models
# 아래 2개 import
from imagekit.models import ProcessedImageField, ImageSpecField
from imagekit.processors import Thumbnail

# Create your models here.
class Article(models.Model):
    title = models.CharField(max_length=10)
    content = models.TextField()
    image = models.ImageField(upload_to='images/', blank=True)
    image_thumbnail = ImageSpecField(
        source='image',		# 원본 ImageField 명
        processors=[Thumbnail(200, 300)],
        format='JPEG',
        options={'quality': 90}
    )
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __str__(self):
        return self.title
```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

```html
<--! detail.html -->

{% extends 'base.html' %}

{% block content %}
  <h1>DETAIL</h1>
  <h3>{{ article.pk }}번째 글</h3>
  {% if article.image %}
    <img src="{{ article.image.url }}" alt="{{ article.image }}">
    <img src="{{ article.image_thumbnail.url }}" alt="{{ article.image_thumbnail }}">
  {% endif %}
  <hr>
  ...
{% endblock content %}
```

- 추가된 `ImageSpecField()` 필드 사용