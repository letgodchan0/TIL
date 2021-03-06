# π± Media files (μ΄λ―Έμ§ μλ‘λ)

## FileField()

- νμΌ μλ‘λμ μ¬μ©νλ λͺ¨λΈ νλ

- 2κ°μ μ ν μΈμλ₯Ό κ°μ§κ³  μμ

  1. `upload_to`
  2. `storage`

  

## ImageField()

- μ΄λ―Έμ§ μλ‘λμ μ¬μ©νλ λͺ¨λΈ νλ
- `FileField`λ₯Ό μμλ°λ μλΈ ν΄λμ€μ΄κΈ° λλ¬Έμ `FileField`μ λͺ¨λ  μμ± λ° λ©μλλ₯Ό μ¬μ© κ°λ₯νλ©°, λν΄μ μ¬μ©μμ μν΄ μλ‘λ λ κ°μ²΄κ° μ ν¨ν μ΄λ―Έμ§μΈμ§ κ²μ¬ν¨
- `ImageField` μΈμ€ν΄μ€λ μ΅λ κΈΈμ΄κ° 100μμΈ λ¬Έμμ΄λ‘ DBμ μμ±λλ©°, `max_length` μΈμλ₯Ό μ¬μ©νμ¬ μ΅λ κΈΈμ΄λ₯Ό λ³κ²½ν  μ μμ. κ·Έλ°λ° μ 100μμΈ λ¬Έμμ΄μ΄ DBμ μ μ₯λλλ©΄, νμΌ μμ²΄λ₯Ό μ μ₯νλ©΄ DBμ μν₯μ λ§μ΄ λΌμΉκΈ° λλ¬Έμ νμΌ μμ²΄λ₯Ό DBμ μ½μΈνλ κ²μ΄ μλλΌ νμΌ κ²½λ‘λ₯Ό μ½μνκΈ° λλ¬Έμ΄λ€.

[μ£Όμ] μ¬μ©νλ €λ©΄ λ°λμ `Pillow` λΌμ΄λΈλ¬λ¦¬κ° νμ!!!

```python
# articles/models.py

class Article(models.Model):
    # saved to "MEDIA_ROOT/images/"
    image = models.ImageField(upload_to='images/', blank=True)
```

- `upload_to = 'images/'`

  - μ€μ  μ΄λ―Έμ§κ° μ μ₯λλ κ²½λ‘λ₯Ό μ§μ 

- `blank = True`

  - μ΄λ―Έμ§ νλμ λΉ κ°(λΉ λ¬Έμμ΄)μ΄ νμ©λλλ‘ μ€μ  (μ΄λ―Έμ§λ₯Ό μ νμ μΌλ‘ μλ‘λν  μ μλλ‘)

  

### `'upload_to'` argument (2κ°μ§ λ°©μ)

> 'upload_to' argument - 1. λ¬Έμμ΄ κ²½λ‘ μ§μ  λ°©μ

- νμ΄μ¬μ `strftime()` νμμ΄ ν¬ν¨λ  μ μμΌλ©°, μ΄λ νμΌ μλ‘λ λ μ§/μκ°μΌλ‘ λμ²΄ λ¨

```python
# models.py

class MyModel(models.Model):
    # MEDIA_ROOT/uploads/ κ²½λ‘λ‘ νμΌ μλ‘λ
    upload = models.FileField(upload_to='uploads/')
    
    # λλ
    
    # MEDIA_ROOT/uploads/2021/01/01 κ²½λ‘λ‘ νμΌ μλ‘λ
    upload = models.FileField(upload_to='uploads/%Y/%m/%d')
```

##### [μ°Έκ³ ] time λͺ¨λμ strftime()

- λ μ§ λ° μκ° κ°μ²΄λ₯Ό λ¬Έμμ΄ ννμΌλ‘ λ³ννλλ° μ¬μ©λ¨
- νλ μ΄μμ νμνλ μ½λ μλ ₯μ λ°μ λ¬Έμμ΄ ννμ λ°ν

```python
from time import gmtime, strftime

strftime('%a, %d %b %Y %H:%M:%S', gmtime())

# 'Thu, 28 Jun 2022 14:17:15'
```



> 'upload_to' argument - 2. ν¨μ νΈμΆ λ°©μ

- λ°λμ 2κ°μ μΈμ (instance, filename)λ₯Ό μ¬μ©ν΄μΌ νλ€.

1. `instance`
   - FileFieldκ° μ μλ λͺ¨λΈμ μΈμ€ν΄μ€
   - λλΆλΆ μ΄ κ°μ²΄λ μμ§ λ°μ΄ν°λ² μ΄μ€μ μ μ₯λμ§ μμμΌλ―λ‘ PK κ°μ΄ μμ§ μμ μ μμ
2. `filename`
   - κΈ°μ‘΄ νμΌμ μ κ³΅λ νμΌ μ΄λ¦

```python
# models.py

def articles_image_path(instance, filename):
    # 'MEDIA_ROOT/image_<pk>/' κ²½λ‘μ '<filename>' μ΄λ¦μΌλ‘ μλ‘λ
    return f'image_{intsance.pk}/{filename}'

class Article(models.Model):
    image = models.ImageField(upload_to = articles_image_path)
```



### Model field option - "blank"

- κΈ°λ³Έ κ° : False
- Validation-related
- TrueμΈ κ²½μ° νλλ₯Ό λΉμ λ μ μμ, DBμλ ''(λΉ λ¬Έμμ΄)μ΄ μ μ₯λ¨
- μ ν¨μ± κ²μ¬μμ μ¬μ© λ¨ (is_valid)
  - λͺ¨λΈ νλμ blank=Trueλ₯Ό μμ±νλ©΄ form μ ν¨μ± κ²μ¬μμ λΉ κ°μ μλ ₯ν  μ μμ

### Model field option - "null"

- κΈ°λ³Έ κ° : False
- Database-related
- TrueμΈ κ²½μ° djangoλ λΉ κ°μ λν΄ DBμ NULLλ‘ μ μ₯
- μ£Όμ μ¬ν­
  - `CharField`, `TextField`μ κ°μ λ¬Έμμ΄ κΈ°λ° νλμλ μ¬μ©νλ κ²μ νΌν΄μΌ ν¨
  - λ¬Έμμ΄ κΈ°λ° νλμ `True`λ‘ μ€μ  μ 'λ°μ΄ν° μμ(no data)'μ ''λΉ λ¬Έμμ΄('')'κ³Ό 'NULL'μ 2κ°μ§ κ°λ₯ν κ°μ΄ μμμ μλ―Ένκ² λ¨
  - λλΆλΆμ κ²½μ° "λ°μ΄ν° μμ"μ λν΄ λ κ°μ κ°λ₯ν κ°μ κ°λ κ²μ μ€λ³΅λλ κ²μ΄λ©°, Djangoλ NULLμ΄ μλ λΉ λ¬Έμμ΄μ μ¬μ©νλ κ²μ΄ κ·μΉ



> λ€μ νλ² μ£Όμ!
>
> λ¬Έμμ΄ κΈ°λ° λ° λΉλ¬Έμμ΄ κΈ°λ° νλ λͺ¨λμ λν΄ null optionμ DBμλ§ μν₯μ λ―ΈμΉλ―λ‘, formμμ λΉ κ°μ νμ©νλ €λ©΄ blank =Trueμ μ€μ ν΄μΌ νλ€!

```python
# blank & null λΉκ΅
# models.py

class Person(models.Model):
    name = models.CharField(max_length=10)
    
    # null = True κΈμ§
    bio = models.TextField(max_length=50, blank = True)
    
    # null, blank λͺ¨λ μ€μ  κ°λ₯ -> λ¬Έμμ΄ κΈ°λ° νλκ° μλκΈ° λλ¬Έ
    birth_date = models.DateField(null=True, blank=True)
```



## ImageField (or FileField)λ₯Ό μ¬μ©νκΈ° μν λͺ κ°μ§ λ¨κ³

1. `settings.py`μ `MEDIA_ROOT`, `MEDIA_URL` μ€μ 
2. `upload_to` μμ±μ μ μνμ¬ μλ‘λ λ νμΌμ μ¬μ©ν  `MEDIA_ROOT`μ νμ κ²½λ‘λ₯Ό μ§μ 
3. μλ‘λ λ νμΌμ κ²½λ‘λ djangoκ° μ κ³΅νλ `url` μμ±μ ν΅ν΄ μ»μ μ μμ

![image-20220410202328407](Django%20-%20Media%20files.assets/image-20220410202328407.png)



#### MEDIA_ROOT

- μ¬μ©μκ° μλ‘λ ν νμΌ(λ―Έλμ΄ νμΌ)λ€μ λ³΄κ΄ν  λλ ν λ¦¬μ μ λ κ²½λ‘
- Djangoλ μ±λ₯μ μν΄ μλ‘λ νμΌμ λ°μ΄ν°λ² μ΄μ€μ μ μ₯νμ§ μμ
  - μ€μ  λ°μ΄ν°λ² μ΄μ€μ μ μ₯λλ κ²μ νμΌμ κ²½λ‘
- [μ£Όμ] `MEDIA_ROOT`λ `STATIC_ROOT`μ λ°λμ λ€λ₯Έ κ²½λ‘λ‘ μ§μ ν΄μΌ ν¨

```PYTHON
# settings.py μ μΌ λ°

MEDIA_ROOT = BASE_DIR / 'media'
```

#### MEDIA_URL

- `MEDIA_ROOT`μμ μ κ³΅λλ λ―Έλμ΄λ₯Ό μ²λ¦¬νλ URL
- μλ‘λ λ νμΌμ μ£Όμ(URL)λ₯Ό λ§λ€μ΄ μ£Όλ μ­ν 
  - μΉ μλ² μ¬μ©μκ° μ¬μ©νλ public URL
- λΉμ΄ μμ§ μμ κ°μΌλ‘ μ€μ  νλ€λ©΄ λ°λμ slash(/)λ‘ λλμΌ ν¨
- [μ£Όμ] `MEDIA_URL`μ `STATIC_URL`κ³Ό λ°λμ λ€λ₯Έ κ²½λ‘λ‘ μ§μ ν΄μΌ ν¨

```PYTHON
# settings.py MEDIA_ROOT λ°μ

MEDIA_URL = '/media/'
```



### κ°λ° λ¨κ³μμ μ¬μ©μκ° μλ‘λ ν νμΌ μ κ³΅νκΈ°

- `settings.MEDIA_URL`
  - μλ‘λ λ νμΌμ URL
- `settings.MEDIA_ROOT`
  - `MEDIA_URL`μ ν΅ν΄ μ°Έμ‘°νλ νμΌμ μ€μ  μμΉ

![image-20220410202539726](Django%20-%20Media%20files.assets/image-20220410202539726.png)

```PYTHON
# νλ‘μ νΈ/urls.py

from django.contrib import admin
from django.urls import path, include
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```



> μ¬κΈ°κΉμ§ μλ£νκ³  λ§μ΄κ·Έλ μ΄μμ μ€ννλ©΄ μ€λ₯κ° λ° μ μλ€. μ΄μ λ ImageFieldλ₯Ό μ¬μ©νκΈ° μν΄μλ Pillow λΌμ΄λΈλ¬λ¦¬ μ€μΉκ° νμνκΈ° λλ¬Έ!

```bash
# Pillow λΌμ΄λΈλ¬λ¦¬ μ€μΉ λ° λ§μ΄κ·Έλ μ΄μ μ§ν

$ pip install Pillow

$ python manage.py makemigrations
$ python manage.py migrate

$ pip freeze > requirements.txt
```



## μ΄λ―Έμ§ μλ‘λ (CREATE)

1. `create.html`μ `enctype` μμ± μΆκ°

```html
<--! articles/create.html -->
    
<form action="{% url 'articles:create' %}" method="POST" enctype="multipart/form-data">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
</form>    
```

- `enctype` μμ±

> multipart / form-data
>
> νμΌ / μ΄λ―Έμ§ μλ‘λ μμ λ°λμ μ¬μ©ν΄μΌ ν¨ (μ μ‘λλ λ°μ΄ν° νμμ μ§μ )
>
> <input type = "file">μ μ¬μ©ν  κ²½μ°μ μ¬μ©

- input μμ - `accept` μμ±

  - μλ ₯μ νμ©ν  νμΌ μ νμ λνλ΄λ λ¬Έμμ΄
  - μΌνλ‘ κ΅¬λΆλ "κ³ μ  νμΌ μ ν μ§μ μ"
  - νμΌμ κ²μ¦ νλ κ²μ μλ
    - accept μμ± κ°μ imageλΌκ³  νλλΌλ λΉλμ€λ μ€λμ€ λ° λ€λ₯Έ νμ νμΌμ μ μΆν  μ μμ
  - κ³ μ  νμΌ μ ν μ§μ μ
    - `<input type="file">` μμ μ νν  μ μλ νμΌμ μ’λ₯λ₯Ό μ€λͺνλ λ¬Έμμ΄

  ![image-20220410214834753](Django%20-%20Media%20files.assets/image-20220410214834753.png)



2. `views.py` μμ 

- μλ‘λν νμΌμ `request.FILES` κ°μ²΄λ‘ μ λ¬λλ€.

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

3. `DB` λ° νμΌ νΈλ¦¬ νμΈ

- μ€μ  νμΌ μμΉ

  - `MEDIA_ROOT/images/`

  ![image-20220410215307972](Django%20-%20Media%20files.assets/image-20220410215307972.png)

- `DB`μ μ μ₯λλ κ²μ μ΄λ―Έμ§ νμΌ μμ²΄κ° μλ νμΌμ κ²½λ‘

![image-20220410215329568](Django%20-%20Media%20files.assets/image-20220410215329568.png)

## μ΄λ―Έμ§ κ²½λ‘ λΆλ¬μ€κΈ°

- `article.image.url`
  - μλ‘λ νμΌμ κ²½λ‘
- `article.image`
  - μλ‘λ νμΌμ νμΌ μ΄λ¦

```html
<--! detail.html -->

{% extends 'base.html' %}

{% block content %}
  <h1>DETAIL</h1>
  <h3>{{ article.pk }}λ²μ§Έ κΈ</h3>
  <img src="{{ article.image.url }}" alt="{{ article.image }}">
  <hr>
  ...
{% endblock content %}
```

> static, media νμΌ λͺ¨λ μλ²μ μμ²­ν΄μ μ‘°ννλ κ²μ΄κΈ° λλ¬Έμ λλ€ urlμ΄ νμνλ€!

## μ΄λ―Έμ§ μλ‘λ (UPDATE)

- μ΄λ―Έμ§λ λ°μ΄λλ¦¬ λ°μ΄ν°(νλμ λ©μ΄λ¦¬)μ΄κΈ° λλ¬Έμ νμ€νΈμ²λΌ μΌλΆλ§ μμ νλ κ²μ λΆκ°λ₯
- λλ¬Έμ μλ‘μ΄ μ¬μ§μΌλ‘ λ?μ΄ μμ°λ λ°©μμ μ¬μ©νλ€.

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

> μ΄λ°μμΌλ‘ μ§νμ νλ©΄ μ΄λ―Έμ§κ° μμ΄ μμ±λ κ²μκΈμ `detail` νμ΄μ§κ° μΆλ ₯λμ§ μλ λ¬Έμ κ° λ°μνλ€. `image`κ° μλ κ²μκΈμ κ²½μ° μΆλ ₯ν  μ΄λ―Έμ§ κ²½λ‘κ° μκΈ° λλ¬ΈμΈλ°, κ·Έλ λ€λ©΄ `detail` νμ΄μ§μμ DTLμ ν΅ν΄ μ΄λ―Έμ§ κ²½λ‘κ° μμ κ²½μ°λ§ μΆλ ₯νλ κ΅¬λ¬ΈμΌλ‘ λ³κ²½ν΄μ£Όλ©΄ λλ€.

```HTML
<--! detail.html -->

{% extends 'base.html' %}

{% block content %}
  <h1>DETAIL</h1>
  <h3>{{ article.pk }}λ²μ§Έ κΈ</h3>
  {% if article.image %}
    <img src="{{ article.image.url }}" alt="{{ article.image }}">
  {% endif %}
  <hr>
  ...
{% endblock content %}
```

## μ΄λ―Έμ§ Resizing

- μ€μ  μλ³Έ μ΄λ―Έμ§λ₯Ό μλ²μ κ·Έλλ‘ μλ‘λ νλ κ²μ μλ²μ λΆλ΄μ΄ ν° μμ
- `<img>` νκ·Έμμ μ§μ  μ¬μ΄μ¦λ₯Ό μ‘°μ ν  μλ μμ§λ§ (widthμ height μμ± μ¬μ©), μλ‘λ λ  λ μ΄λ―Έμ§ μμ²΄λ₯Ό `resizing` νλ κ²μ κ³ λ €νκΈ°
- `django-imagekit` λΌμ΄λΈλ¬λ¦¬ νμ©

1. `django-imagekit` μ€μΉ

```bash
$ pip install django-imagekit
$ pip freeze > requirements.txt
```

2. `INSTALLED_APPS`μ μΆκ°

```PYTHON
# settings.py

INSTALLED_APP = [
    ...
    'imagekit',
    ...
]
```

### μλ³Έ μ΄λ―Έμ§λ₯Ό μ¬κ°κ³΅νμ¬ μ μ₯ (μλ³Έ X, μΈλ€μΌ O)

```python
# models.py
from distutils.command.upload import upload
from django.db import models
# μλ 2κ° import
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

- `ProcessedImageField()`μ νλΌλ―Έν°λ‘ μμ±λ κ°λ€μ λ³κ²½νλλΌλ λ€μ `makemigrations`λ₯Ό ν΄μ€ νμ μμ΄ μ¦μ λ°μ λλ€.

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```



### μλ³Έ μ΄λ―Έμ§λ₯Ό μ¬κ°κ³΅νμ¬ μ μ₯ (μλ³Έ O, μΈλ€μΌ O)

```PYTHON
# models.py

from distutils.command.upload import upload
from django.db import models
# μλ 2κ° import
from imagekit.models import ProcessedImageField, ImageSpecField
from imagekit.processors import Thumbnail

# Create your models here.
class Article(models.Model):
    title = models.CharField(max_length=10)
    content = models.TextField()
    image = models.ImageField(upload_to='images/', blank=True)
    image_thumbnail = ImageSpecField(
        source='image',		# μλ³Έ ImageField λͺ
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
  <h3>{{ article.pk }}λ²μ§Έ κΈ</h3>
  {% if article.image %}
    <img src="{{ article.image.url }}" alt="{{ article.image }}">
    <img src="{{ article.image_thumbnail.url }}" alt="{{ article.image_thumbnail }}">
  {% endif %}
  <hr>
  ...
{% endblock content %}
```

- μΆκ°λ `ImageSpecField()` νλ μ¬μ©