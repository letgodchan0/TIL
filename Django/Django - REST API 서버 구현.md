# ๐ฉ REST API

### DRF๋ฅผ ํ์ฉํ์ฌ ๊ฒ์๊ธ ๊ด๋ จ REST API ์๋ฒ ๊ตฌ์ถํ๊ธฐ

<hr>

<br>

## ๐ REST API ์๋ฒ ๊ตฌ์ถ์ ์ํ ๊ธฐ๋ณธ์ธํ

### 1. ๊ฐ์ํ๊ฒฝ ์ค์  ๋ฐ ํจํค์ง ์ค์น

```bash
$ python -m venv venv
$ source venv/Scripts/activate
```

```bash
$ pip install -r requirements.txt

'''
asgiref==3.5.0
Django==3.2.12
django-extensions==3.1.5
django-seed==0.3.1
djangorestframework==3.13.1
Faker==13.3.4
psycopg2==2.9.3
python-dateutil==2.8.2
pytz==2022.1
six==1.16.0
sqlparse==0.4.2
toposort==1.7
'''
```

### 2. ํ๋ก์ ํธ ์์ฑ ๋ฐ ์๋ฒ ์คํ

```bash
$ django-admin startproject my_api .
$ python manage.py runserver
```

### 3. ์ฑ ์์ฑ ๋ฐ `INSTALLED_APPS`์ ๋ฑ๋ก

```BASH
$ python manage.py startapp articles
```

```python
# settings.py

INSTALLED_APPS = [
    'articles',
    'django_seed', 
    'django_extensions',
    'rest_framework',
    ...
]
```

### 4. `models.py` ์์ฑ

```python
# articles/models.py

from django.db import models

# Create your models here.
class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

### 5. migration

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

### 6. `serializers.py` ์์ฑ

> Serializers - Queryset ๋ฐ Model Instance์ ๊ฐ์ ๋ณต์กํ ๋ฐ์ดํฐ๋ฅผ JSON, XML ๋ฑ์ ์ ํ์ผ๋ก ์ฝ๊ฒ ๋ณํํ  ์ ์๋ Python ๋ฐ์ดํฐ ํ์์ผ๋ก ๋ง๋ค์ด ์ค

```python
# articles/serializers.py

from rest_framework import serializers
from .models import Article, Comment

# ๋ชจ๋  ๊ฒ์๊ธ ์ ๋ณด๋ฅผ ๋ฐํํ๊ธฐ ์ํ ModelSerializer
class ArticleListSerializer(serializers.ModelSerializer):

    class Meta:
        model = Article
        fields = ('id', 'title',)

class CommentSerializer(serializers.ModelSerializer):
	# Comment ํ์ด๋ธ์ ๋ชจ๋  ํ๋์ ๋ฐ์ดํฐ๋ฅผ ๋๊ธฐ๊ณ  ์ฐธ์กฐํ๋ ๋ฐ์ดํฐ๋ ์ ํจ์ฑ ๊ฒ์ฌ x
    class Meta:
        model = Comment
        fields = '__all__'
        read_only_fields = ('article',)

# ๊ฒ์๊ธ ์์ธ ์ ๋ณด๋ฅผ ๋ฐํ ๋ฐ ์์ฑํ๊ธฐ ์ํ ModelSerializer
class ArticleSerializer(serializers.ModelSerializer):
    # ๊ฒ์๊ธ๋ง๋ค
    comment_set = CommentSerializer(many=True, read_only=True)
    comment_count = serializers.IntegerField(source='comment_set.count', read_only=True)
	# Article์ ๋ชจ๋  ํ๋์ ๋ฐ์ดํฐ๋ฅผ ๋๊น
    class Meta:
        model = Article
        fields = '__all__'
```

- `ArticleListSerializer` - `Article` ํ์ด๋ธ์ ํ๋ ์ค `id`์ `title`  ๋ฐ์ดํฐ๋ง ๋๊ฒจ์ฃผ๋ ํด๋์ค
- `CommentSerializer` - `Comment` ํ์ด๋ธ์ ๋ชจ๋  ํ๋์ ๋ฐ์ดํฐ๋ฅผ ๋๊น, 
  - `read_only_fields`  - ์ฐธ์กฐํ๋ ๋ฐ์ดํฐ๋ ์ ํจ์ฑ ๊ฒ์ฌํ์ง ์๊ธฐ ์์ฑ
- `ArticleSerializer` - Article ํ์ด๋ธ์ ๋ชจ๋  ํ๋์ ์ง์  ์์ฑํ `comment_set`์ `comment_count` ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ด ๋๊น, `read_only` ์ต์์ ์ฃผ์๊ธฐ ๋๋ฌธ์ ์ ํจ์ฑ ๊ฒ์ฌ๋ ์งํ๋์ง ์์

### 7. url ์์ฑ

```python
# my_api/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/', include('articles.urls')),
]
```

### 8. `django-seed` ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ์ฌ์ฉํด์ ๋ชจ๋ธ ๊ตฌ์กฐ์ ๋ง๋ ๊ฐ์ ๋ฐ์ดํฐ ์์ฑ

```bash
$ python manage.py seed articles --number=20
```

<br>



# ๐ ๊ฒ์๊ธ ๊ด๋ จ

<hr>



## ๐ GET - api/v1/articles/ (๋ชจ๋  ๊ฒ์๊ธ ์กฐํ)

> ๋ชจ๋  ๊ฒ์๊ธ์ id์ title ์ปฌ๋ผ์ JSON ๋ฐ์ดํฐ ํ์์ผ๋ก ์๋ตํ๊ธฐ

### 1. serializer ์์ฑ

```python
# articles/serializers.py

from rest_framework import serializers
from .models import Article

# ๋ชจ๋  ๊ฒ์๊ธ ์ ๋ณด๋ฅผ ๋ฐํํ๊ธฐ ์ํ ModelSerializer
class ArticleListSerializer(serializers.ModelSerializer):

    class Meta:
        model = Article
        fields = ('id', 'title',)
```

### 2. url ์์ฑ

```python
# articles/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path('articles/', views.article_list),
]
```

### 3. view ์์ฑ

```python
# articles/views.py

from rest_framework.response import Response
from rest_framework.decorators import api_view
from django.shortcuts import get_list_or_404
from .models import Article
from .serializers import ArticleListSerializer

@api_view(['GET'])
def article_list(request):
    # ๊ฒ์๊ธ ์กฐํ
    if request.method == 'GET':
        articles = get_list_or_404(Article)
        serializer = ArticleListSerializer(articles, many=True)
        return Response(serializer.data)
```

- `many=True` - ๋จ์ผ ์ธ์คํด์ค ๋์  QuerySet ๋ฑ์ ์ง๋ ฌํ ํ๊ธฐ ์ํด์๋ serializer๋ฅผ ์ธ์คํด์คํ ํ  ๋ many=True๋ฅผ ํค์๋ ์ธ์๋ก ์ ๋ฌํด์ผ ํจ

- `api_view` decorator 
  - ๊ธฐ๋ณธ์ ์ผ๋ก GET ๋ฉ์๋๋ง ํ์ฉ๋๋ฉฐ ๋ค๋ฅธ ๋ฉ์๋ ์์ฒญ์ ๋ํด์๋ 405 Method Not Allowed๋ก ์๋ต
  - View ํจ์๊ฐ ์๋ตํด์ผ ํ๋ HTTP ๋ฉ์๋์ ๋ชฉ๋ก์ ๋ฆฌ์คํธ์ ์ธ์๋ก ๋ฐ์
  - DRF์์๋ ์ ํ์ด ์๋ ํ์์ ์ผ๋ก ์์ฑํด์ผ ํด๋น view ํจ์๊ฐ ์ ์์ ์ผ๋ก ๋์ํจ

## ๐ POST- api/v1/articles/ (๊ฒ์๊ธ ์์ฑ)

> - ๊ฒ์ฆ์ ์ฑ๊ณตํ๋ ๊ฒฝ์ฐ ์๋ก์ด ๊ฒ์๊ธ์ ์ ๋ณด๋ฅผ DB์ ์ ์ฅํ๊ณ , ์ ์ฅ๋ ๊ฒ์๊ธ์ ์ ๋ณด๋ฅผ ์๋ตํ๋ค.
>
> - ๊ฒ์ฆ์ ์คํจํ๋ ๊ฒฝ์ฐ 400 Bad Request ์์ธ๋ฅผ ๋ฐ์์ํจ๋ค.
> - `article_list` ํจ์๋ก ๊ฒ์๊ธ์ ์กฐํํ๊ฑฐ๋ ์์ฑํ๋ ํ์๋ฅผ ๋ชจ๋ ์ฒ๋ฆฌ ๊ฐ๋ฅ

### 1. view ์์ฑ

```python
# articles/views.py
from res_framework import status

@api_view(['GET', 'POST'])
def article_list(request):
    # ๊ฒ์๊ธ ์กฐํ
    if request.method == 'GET':
        articles = get_list_or_404(Article)
        serializer = ArticleListSerializer(articles, many=True)
        return Response(serializer.data)
    # ๊ฒ์๊ธ ์์ฑ
    elif request.method == 'POST':
        serializer = ArticleSerializer(data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
```

- `status=status.HTTP_201_CREATED` 

  - 201 Created ์ํ ์ฝ๋ ๋ฐ ๋ฉ์์ง ์๋ต
  - DRF์๋ status code๋ฅผ ๋ณด๋ค ๋ชํํ๊ณ  ์ฝ๊ธฐ ์ฝ๊ฒ ๋ง๋๋๋ฐ ์ฌ์ฉํ  ์ ์๋ ์ ์๋ ์์ ์งํฉ์ ์ ๊ณต
  - `status` ๋ชจ๋์ HTTP status code ์งํฉ์ด ๋ชจ๋ ํฌํจ๋์ด ์์

- `raise_exception` 

  - ''Raising an exception on invalid data"
  - `is_valid()`๋ ์ ํจ์ฑ ๊ฒ์ฌ ์ค๋ฅ๊ฐ ์๋ ๊ฒฝ์ฐ `serializers.ValidationError` ์์ธ๋ฅผ ๋ฐ์์ํค๋ ์ ํ์  `raise_exception` ์ธ์๋ฅผ ์ฌ์ฉํ  ์ ์์
  - DRF์์ ์ ๊ณตํ๋ ๊ธฐ๋ณธ ์์ธ ์ฒ๋ฆฌ๊ธฐ์ ์ํด ์๋์ผ๋ก ์ฒ๋ฆฌ๋๋ฉฐ, ๊ธฐ๋ณธ์ ์ผ๋ก HTTP status code 400์ ์๋ต์ผ๋ก ๋ฐํํจ

- `serializer = ArticleSerializer(data=request.data)` 

  - `ArticleListSerializer`์ ๊ธฐ๋ณธ์ ์ผ๋ก `ModelSerializer`์ ์์๋ฐ๊ณ  ์๊ณ  ์๋ ๋ค์ `Serializer`์ ์์๋ฐ๊ณ  ์๋ค. DB ์ธ์คํด์ค๋ฅผ ์์ฑํ๊ณ ์ ํ  ๋ ์ฌ์ฉํ๋ ๋ฉ์๋๊ฐ  `Serializer`์  `create` ๋ฉ์๋์ด์ด๊ณ , ๋ค์๊ณผ ๊ฐ์ด ์ธ์์ ๋ค์ด๊ฐ๋๊ฒ `request.data` ํ๋๋ฟ์ด๋ค!

    ```python
    def create(self, validated_data):
        ...
    ```

    



## ๐ GET - api/v1/articles/article_pk/ (ํน์  ๊ฒ์๊ธ ์กฐํ)

> ํน์  ๊ฒ์๊ธ์ ๋ชจ๋  ์ปฌ๋ผ์ JSON ๋ฐ์ดํฐ ํ์์ผ๋ก ์๋ตํ๋ค.

### 1. serializer ์์ฑ

```python
# articles/serializers.py

class ArticleSerializer(serializers.ModelSerializer):

    class Meta:
        model = Article
        fields = '__all__'
```

### 2. url ์์ฑ

```python
# articles/urls.py

urlpatterns = [
    path('articles/<int:article_pk>/', views.article_detail),
]
```

### 3. view ์์ฑ

```python
# articles/views.py

from django.shortcuts import get_object_or_404
from .serializers import ArticleListSerializer, ArticleSerializer

@api_view(['GET'])
def article_detail(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    serializer = ArticleSerializer(article)
    return Response(serializer.data)
```



## ๐ DELETE - api/v1/articles/article_pk/ (ํน์  ๊ฒ์๊ธ ์ญ์ )

> - DELETE ์์ฒญ์ธ ๊ฒฝ์ฐ ํน์  ๊ฒ์๊ธ์ ์ญ์ ํ๊ณ  ์ญ์ ๊ฐ ์๋ฃ๋๋ฉด, ์ญ์ ํ ๊ฒ์๊ธ์ id๋ฅผ ์๋ตํ๋ค. 
>
> - 204 No Content ์ํ ์ฝ๋ ๋ฐ ๋ฉ์์ง ์๋ต
> - `article_detail` ํจ์๋ก ์์ธ ๊ฒ์๊ธ์ ์กฐํํ๊ฑฐ๋ ์ญ์ ํ๋ ํ์ ๋ชจ๋ ์ฒ๋ฆฌ ๊ฐ๋ฅ

### view ์์ฑ

```python
# articles/views.py

@api_view(['GET', 'DELETE', 'PUT'])
def article_detail(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    if request.method == 'GET':
        serializer = ArticleSerializer(article)
        return Response(serializer.data)

    elif request.method == 'DELETE':
        article.delete()
        data = {
            'delete' : f'๋ฐ์ดํฐ {article_pk}๋ฒ์ด ์ญ์ ๋์์ต๋๋ค.'
        }
        return Response(data, status=status.HTTP_204_NO_CONTENT)
```



## ๐ PUT - api/v1/articles/article_pk/ (ํน์  ๊ฒ์๊ธ ์์ )

> - PUT ์์ฒญ์ธ ๊ฒฝ์ฐ ํน์  ๊ฒ์๊ธ์ ์ ๋ณด๋ฅผ ์์ ํ๋ค.
>   - ๊ฒ์ฆ์ ์ฑ๊ณตํ๋ ๊ฒฝ์ฐ ์์ ๋ ๊ฒ์๊ธ์ ์ ๋ณด๋ฅผ DB์ ์ ์ฅํ๋ค.
>   - ๊ฒ์ฆ์ ์คํจํ  ๊ฒฝ์ฐ 400 Bad Request ์์ธ๋ฅผ ๋ฐ์์ํจ๋ค.
>   - ์์ ์ด ์๋ฃ๋๋ฉด ์์ ํ ๊ฒ์๊ธ์ ์ ๋ณด๋ฅผ ์๋ตํ๋ค.
> - `article_detail` ํจ์๋ก ์์ธ ๊ฒ์๊ธ์ ์กฐํํ๊ฑฐ๋ ์ญ์ , ์์ ํ๋ ํ์ ๋ชจ๋ ์ฒ๋ฆฌ ๊ฐ๋ฅ

```python
# articles/views.py

@api_view(['GET', 'DELETE', 'PUT'])
def article_detail(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    if request.method == 'GET':
        serializer = ArticleSerializer(article)
        return Response(serializer.data)

    elif request.method == 'DELETE':
        article.delete()
        data = {
            'delete' : f'๋ฐ์ดํฐ {article_pk}๋ฒ์ด ์ญ์ ๋์์ต๋๋ค.'
        }
        return Response(data)
    elif request.method == 'PUT':
        serializer = ArticleSerializer(article, data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data)
```

- `serializer = ArticleSerializer(article, data=request.data)`

  - `ArticleListSerializer`์ ๊ธฐ๋ณธ์ ์ผ๋ก `ModelSerializer`์ ์์๋ฐ๊ณ  ์๊ณ  ์๋ ๋ค์ `Serializer`์ ์์๋ฐ๊ณ  ์๋ค. DB ์ธ์คํด์ค๋ฅผ ์์ ํ๊ณ ์ ํ  ๋ ์ฌ์ฉํ๋ ๋ฉ์๋๊ฐ  `Serializer`์  `update` ๋ฉ์๋์ด๊ณ , ๋ค์๊ณผ ๊ฐ์ด ์ธ์์ ๋ค์ด๊ฐ๋๊ฒ ์ธ์คํด์ค์ธ  `article`๊ณผ ์ฌ์ฉ์๊ฐ ์์ฑํ  `request.data` 2๊ฐ์ด๊ธฐ ๋๋ฌธ์ ์ธ์์ 2๊ฐ๊ฐ ๋ค์ด๊ฐ๋ค.

    ```python
    def update(self, instance, validated_data):
        ...
    ```

    



<br>



# ๐ ๋๊ธ ๊ด๋ จ

<hr>



## ๐ GET - api/v1/comments/ (๋ชจ๋  ๋๊ธ ์กฐํ)

> ๋ชจ๋  ๋๊ธ์ ํ๋๋ฅผ JSON ๋ฐ์ดํฐ ํ์์ผ๋ก ์๋ตํ๊ธฐ

### 1. serializer ์์ฑ

```python
# articles/serializers.py
from .models import Article, Comment

class CommentSerializer(serializers.ModelSerializer):

    class Meta:
        model = Comment
        fields = '__all__'
```

### 2. url ์์ฑ

```python
# articles/urls.py

urlpatterns = [
    ...,
    path('comments/', views.comment_list),
]
```

### 3. view ์์ฑ

```python
# articles/views.py

@api_view(['GET'])
from .models import Article, Comment
from .serializers import ArticleListSerializer, ArticleSerializer, CommentSerializer

def comment_list(request):
    comments = get_list_or_404(Comment)
    serializer = CommentSerializer(comments, many=True)
    return Response(serializer.data)
```



## ๐ GET - api/v1/comments/comment_pk/ (ํน์  ๋๊ธ ์กฐํ)

> GET ์์ฒญ์ธ ๊ฒฝ์ฐ ํน์  ๋๊ธ์ ๋ชจ๋  ์ปฌ๋ผ์ JSON์ผ๋ก ์๋ต

### 1. url ์์ฑ

```python
# articles/urls.py

urlpatterns = [
    ...,
    path('comments/<int:comment_pk>/', views.comment_detail),
]
```

### 2. view ์์ฑ

```python
# articles/views.py

@api_view(['GET'])
def comment_detail(request, comment_pk):
    comment = get_object_or_404(Comment, pk=comment_pk)
    serializer = CommentSerializer(comment)
    return Response(serializer.data)
```

## ๐ DELETE & PUT- api/v1/comments/comment_pk/ (ํน์  ๋๊ธ ์ญ์  & ์์ )

> DELETE ์์ฒญ์ธ ๊ฒฝ์ฐ ํน์  ๋๊ธ์ ์ ๋ณด๋ฅผ ์ญ์ ํ๋ค.
>
> - ์ญ์ ๊ฐ ์๋ฃ๋ ์ดํ์ ์ญ์ ํ ๋๊ธ์ id์ 204 No Content๋ฅผ ์๋ตํ๋ค.
>
> PUT ์์ฒญ์ธ ๊ฒฝ์ฐ ํน์  ๋๊ธ์ ์ ๋ณด๋ฅผ ์์ ํ๋ค.
>
> - ๊ฒ์ฆ์ ์ฑ๊ณตํ๋ ๊ฒฝ์ฐ ์์ ๋ ๋๊ธ์ ์ ๋ณด๋ฅผ DB์ ์ ์ฅํ๋ค.
> - ๊ฒ์ฆ์ ์คํจํ๋ ๊ฒฝ์ฐ 400 Bad Request๋ฅผ ์๋ตํ๋ค.
> - ์์ ์ด ์๋ฃ๋ ์ดํ์ ์์ ๋ ์์์ ์ ๋ณด๋ฅผ ์๋ตํ๋ค.
>
> `Article` ์์ฑ ๋ก์ง์์์ ๋ง์ฐฌ๊ฐ์ง๋ก `comment_detail` ํจ์๊ฐ ๋ชจ๋ ์ฒ๋ฆฌํ  ์ ์๋๋ก ์์ฑ

### view ์์ 

```python
# articles/views.py

@api_view(['GET', 'PUT', 'DELETE'])
def comment_detail(request, comment_pk):
    comment = get_object_or_404(Comment, pk=comment_pk)
    if request.method == 'GET':
        serializer = CommentSerializer(comment)
        return Response(serializer.data)

    elif request.method == 'DELETE':
        comment.delete()
        data = {
            'delete' : f'๋๊ธ {comment_pk}๋ฒ์ด ์ญ์ ๋์์ต๋๋ค.'
        }
        return Response(data)

    elif request.method == 'PUT':
        serializer = CommentSerializer(comment, data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data)
```





## ๐ POST - api/v1/articles/article_pk/comments/ (๋๊ธ ์์ฑ)

> ํน์  ๊ฒ์๊ธ์ ๋๊ธ ์์ฑ

### 1. url ์์ฑ

```python
# articles/urls.py

urlpatterns = [
    ...,
    path('articles/<int:article_pk>/comments/', views.comment_create),
]
```

### 2. view ์์ฑ

```python
# articles/views.py

@api_view(['POST'])
def comment_create(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    serializer = CommentSerializer(data=request.data)
    if serializer.is_valid(raise_exception=True):
        # serializer.save()
        serializer.save(article=article)
        return Response(serializer.data, status=status.HTTP_201_CREATED)
```

- Article ์์ฑ๊ณผ ๋ฌ๋ฆฌ Comment ์์ฑ์ ์์ฑ ์์ ์ฐธ์กฐํ๋ ๋ชจ๋ธ์ ๊ฐ์ฒด ์ ๋ณด๊ฐ ํ์ํ๋ค.
  - 1:N ๊ด๊ณ์์ N์ ์ด๋ค 1์ ์ฐธ์กฐํ๋์ง์ ๋ํ ์ ๋ณด๊ฐ ํ์ํ๊ธฐ ๋๋ฌธ (์ธ๋ ํค)
- `serializer.save(article=article)`
  - `.save()` ๋ฉ์๋๋ ํน์  Serializer ์ธ์คํด์ค๋ฅผ ์ ์ฅํ๋ ๊ณผ์ ์์ ์ถ๊ฐ์ ์ธ ๋ฐ์ดํฐ๋ฅผ ๋ฐ์ ์ ์์
  - ์ธ์คํด์ค๋ฅผ ์ ์ฅํ๋ ์์ ์ ์ถ๊ฐ ๋ฐ์ดํฐ ์ฝ์์ด ํ์ํ ๊ฒฝ์ฐ

### 3. serializer ์์ 

```python
# articles/serializers.py

class CommentSerializer(serializers.ModelSerializer):

    class Meta:
        model = Comment
        fields = '__all__'
        read_only_fields = ('article',)
```

- `Read Only Field`
  - ์ด๋ค ๊ฒ์๊ธ์ ์์ฑํ๋ ๋๊ธ์ธ์ง์ ๋ํ ์ ๋ณด๋ฅผ form-data๋ก ๋๊ฒจ์ฃผ์ง ์์๊ธฐ ๋๋ฌธ์ `serializer` ํ๋ ๊ณผ์ ์์ `article` ํ๋๊ฐ ์ ํจ์ฑ ๊ฒ์ฌ๋ฅผ ํต๊ณผํ์ง ๋ชปํจ
    - CommentSerializer์์ article ํ๋์ ํด๋นํ๋ ๋ฐ์ดํฐ ๋ํ ์์ฒญ์ผ๋ก๋ถํฐ ๋ฐ์์ `serializer`ํ๋ ๊ฒ์ผ๋ก ์ค์ ๋์ด ์๊ธฐ ๋๋ฌธ!
  - ์ด๋๋ ์ฝ๊ธฐ ์ ์ฉ ํ๋(read_only_fields) ์ค์ ์ ํตํด `serializer`ํ์ง ์๊ณ  ๋ฐํ ๊ฐ์๋ง ํด๋น ํ๋๊ฐ ํฌํจ๋๋๋ก ์ค์ ํ  ์ ์์ 

<br>

# ๐1:N Serializer

<hr>

### ๐ GET - api/v1/articles/article_pk/ (ํน์  ๊ฒ์๊ธ์ ์์ฑ๋ ๋๊ธ ๋ชฉ๋ก ์ถ๋ ฅ)

>ํน์  ๊ฒ์๊ธ์ ๋ชจ๋  ํ๋์ ๋๊ธ ๋ชฉ๋ก์ JSON ํ์์ผ๋ก ์๋ต
>
>`Serializer`๋ ๊ธฐ์กด ํ๋๋ฅผ override ํ๊ฑฐ๋ ์ถ๊ฐ ํ๋๋ฅผ ๊ตฌ์ฑํ  ์ ์์
>
>1. PrimaryKeyField
>2. Nested relationships

### case1) PrimaryKeyRelatedField

```python
# articles/serializers.py

class ArticleSerializer(serializers.ModelSerializer):
    comment_set = serializers.PrimaryKeyRelatedField(many=True, read_only=True)

    class Meta:
        model = Article
        fields = '__all__'
```

- pk๋ฅผ ์ฌ์ฉํ์ฌ ๊ด๊ณ๋ ๋์์ ๋ํ๋ด๋ ๋ฐ ์ฌ์ฉํ  ์ ์์
- ํ๋๊ฐ `to many relationships(N)`๋ฅผ ๋ํ๋ด๋๋ฐ ์ฌ์ฉ๋๋ ๊ฒฝ์ฐ `many=True` ์์ฑ ํ์
- `comment_set` ํ๋ ๊ฐ์ form-data๋ก ๋ฐ์ง ์์ผ๋ฏ๋ก `read_only=True` ์ค์  ํ์

[์ฐธ๊ณ ] ์ญ์ฐธ์กฐ ์ ์์ฑ๋๋ `comment_set`์ ๋ค๋ฅธ ๋งค๋์  ์ด๋ฆ์ผ๋ก ์์ ํ  ์ ์์, ๋จ ์์ ํ  ๊ฒฝ์ฐ ์ด์  `serializers.py`์์์ ํด๋์ค ๋ณ์๋ช๋ ์ผ์นํ๋๋ก ์์ ํด์ผ ํจ

![image-20220422013521246](Django%20-%20REST%20API%20%EC%84%9C%EB%B2%84%20%EA%B5%AC%ED%98%84.assets/image-20220422013521246.png)

`api/v1/articles/1/`๋ก GET ์์ฒญ ํ ์๋ต ํ์ธ



### case2) Nested relationships

```python
# articles/serializers.py

class ArticleSerializer(serializers.ModelSerializer):
    comment_set = CommentSerializer(many=True, read_only=True)

    class Meta:
        model = Article
        fields = '__all__'
```

- ๋ชจ๋ธ ๊ด๊ณ์์ผ๋ก ์ฐธ์กฐ๋ ๋์์ ์ฐธ์กฐํ๋ ๋์์ ํํ(์๋ต)์ ํฌํจ๋๊ฑฐ๋ ์ค์ฒฉ(nested)๋  ์ ์์
- ์ด๋ฌํ ์ค์ฒฉ๋ ๊ด๊ณ๋ฅผ `serializers`๋ฅผ ํ๋๋ก ์ฌ์ฉํ์ฌ ํํํ  ์ ์์
- ๋ ํด๋์ค์ ์ํ ์์น ๋ณ๊ฒฝ!!!

### ๐ GET - api/v1/articles/article_pk/ (ํน์  ๊ฒ์๊ธ์ ์์ฑ๋ ๋๊ธ์ ๊ฐ์ ๊ตฌํ๊ธฐ)

> ํน์  ๊ฒ์๊ธ์ ๋ชจ๋  ํ๋์ ๋๊ธ, ๋๊ธ ๊ฐ์ ๋ฐ์ดํฐ๋ฅผ JSON ํ์์ผ๋ก ์๋ต

```python
# articles/serializers.py

class ArticleSerializer(serializers.ModelSerializer):
    comment_set = CommentSerializer(many=True, read_only=True)
    comment_count = serializers.IntegerField(source='comment_set.count', read_only=True)

    class Meta:
        model = Article
        fields = '__all__'
```

- `comment_set` ๋งค๋์ ๋ ๋ชจ๋ธ ๊ด๊ณ๋ก ์ธํด ์๋์ผ๋ก ๊ตฌ์ฑ๋๊ธฐ ๋๋ฌธ์ ์ปค์คํ ํ๋๋ฅผ ๊ตฌ์ฑํ์ง ์์๋ `comment_set`์ด๋ผ๋ ํ๋๋ช์ `fields` ์ต์์ ์์ฑ๋ง ํด๋ ์ฌ์ฉํ  ์ ์์์ 
- ํ์ง๋ง ์ง๊ธ์ฒ๋ผ ๋ณ๋์ ๊ฐ์ ์ํ ํ๋๋ฅผ ์ฌ์ฉํ๋ ค๋ ๊ฒฝ์ฐ ์๋์ผ๋ก ๊ตฌ์ฑ๋๋ ๋งค๋์ ๊ฐ ์๋๊ธฐ ๋๋ฌธ์ ์ง์  ํ๋๋ฅผ ์์ฑํด์ผ ํจ

- `source='comment_set.count'`

  - ํ๋๋ฅผ ์ฑ์ฐ๋ ๋ฐ ์ฌ์ฉํ  ์์ฑ์ ์ด๋ฆ
  - ์  ํ๊ธฐ๋ฒ(dot notation)์ ์ฌ์ฉํ์ฌ ์์ฑ์ ํ์ํ  ์ ์์
  - `comment_set`์ด๋ผ๋ ํ๋์ .(dot)์ ํตํด ์ ์ฒด ๋๊ธ์ ๊ฐ์ ํ์ธ ๊ฐ๋ฅ
  - `count()`๋ built-in Queryset API ์ค ํ๋

- `read_only=True`

  - ํน์  ํ๋๋ฅผ override ํน์ ์ถ๊ฐํ ๊ฒฝ์ฐ `read_only_fields`๋ฅผ ์ฌ์ฉํ  ์ ์์

  ![image-20220422014549008](Django%20-%20REST%20API%20%EC%84%9C%EB%B2%84%20%EA%B5%AC%ED%98%84.assets/image-20220422014549008.png)

<br>

## ๐M:N Serializer

<hr>

> Card๋ผ๋ ์๋ก์ด ๋ชจ๋ธ ํด๋์ค ์ ์, ์ด ํด๋์ค๋ Article์ M:N์ ๊ด๊ณ๋ฅผ ๊ฐ์ง๋ค.

### 1. Model ์์  ๋ฐ ๋ง์ด๊ทธ๋ ์ด์

```python
# articles/models.py
	...,
    
    class Card(models.Model):
        articles = models.ManyToManyField(Article, related_name='cards')
        name = models.CharField(max_length=100)
```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

## 2. Serializer ์์ฑ

```python
# articles/serializers.py
from .models import Card

class CardSerializer(serializers.ModelSerializer):

    class Meta:
        model = Card
        fields = '__all__'
        
class ArticleSerializer(serializers.ModelSerializer):
    comment_set = CommentSerializer(many=True, read_only=True)
    comment_count = serializers.IntegerField(source='comment_set.count', read_only=True)
    cards = CardSerializer(many=True, read_only=True)  # ์๋ก์ถ๊ฐ

    class Meta:
        model = Article
        fields = '__all__'
```

- `CardSerializer` ํด๋์ค ์์ฑ
- `ArticleSrializer` ํด๋์ค์์ `cards` ํ๋ ์์ฑ



## ๐GET - api/v1/cards/ (๋ชจ๋  ์นด๋์ ์์ฑ๋ ๋ฐ์ดํฐ ์กฐํ)

### 1. url ์์ฑ

```python
# articles/urls.py

urlpatterns = [
    ...,
    path('cards/', views.card_list),
]
```

### 2. view ์์ฑ

```python
# articles/views.py
from .serializers import CardSerializer

@api_view(['GET'])
def card_list(request):
    cards = get_list_or_404(Card)
    serializer = CardSerializer(cards, many=True)
    return Response(serializer.data)
```





## ๐GET - api/v1/cards/card_pk/ (ํน์  ์นด๋์ ์์ฑ๋ ๋ฐ์ดํฐ ์กฐํ, ์์ , ์ญ์ )

### 1. url ์์ฑ

```python
# articles/urls.py

urlpatterns = [
    ...,
    path('cards/<int:card_pk>/', views.card_detail),
]
```

### 2. view ์์ฑ

```python
# articles/views.py

@api_view(['GET', 'PUT', 'DELETE'])
def card_detail(request, card_pk):
    card = get_object_or_404(Card)
    # ์กฐํ
    if request.method == 'GET':
        serializer = CardSerializer(card)
        return Response(serializer.data)
    
    elif request.method == 'PUT':
        serializer = CardSerializer(card, data=request.data)
        if serializer.is_valid(raise_exception=True):
            serailizer.save()
            return Response(serailizer.data)
        
    elif request.method == 'DELETE':
        card.delete()
        data = {
            'delete': f'๋ฐ์ดํฐ {card_pk}๋ฒ์ด ์ญ์ ๋์์ต๋๋ค.',
        }
        return Response(data, status=status.HTTP_204_NO_CONTENT)
```



## ๐GET - api/v1/cards/card_pk/ (์นด๋ ์์ฑ)

### 1. url ์์ฑ

```PYTHON
# articles/urls.py

urlpatterns = [
    ...,
    path('cards/<int:card_pk>/<int:article_pk>/', views.card_register),
]
```

### 2. view ์์ฑ

```python
# articles/views.py

@api_view(['POST'])
def card_register(request, card_pk, article_pk):
    card = get_object_or_404(Card)
    article = get_object_or_404(Article)
    # ํด๋น ๊ฒ์๊ธ์ ์นด๋๊ฐ ์ด๋ฏธ ์กด์ฌํ๋ฉด ์ ๊ฑฐ
    if card.articles.filter(pk=article_pk).exists():
        card.articles.remove(article)
    # ์กด์ฌํ์ง ์์ผ๋ฉด ์์ฑ
    else:
        card.articles.add(article)
    serializer = CardSerializer(card)
    return Response(serializer.data)
```

