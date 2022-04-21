# 📩 REST API

### DRF를 활용하여 게시글 관련 REST API 서버 구축하기



## 📋 REST API 서버 구축을 위한 기본세팅

### 1. 가상환경 설정 및 패키지 설치

```bash
$ python -m venv venv
$ python -m venv/Scripts/activate
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

### 2. 프로젝트 생성 및 서버 실행

```bash
$ django-admin startproject my_api .
$ python manage.py runserver
```

### 3. 앱 생성 및 `INSTALLED_APPS`에 등록

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

### 4. `models.py` 생성

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

### 6. `serializers.py` 생성

> Serializers - Queryset 및 Model Instance와 같은 복잡한 데이터를 JSON, XML 등의 유형으로 쉽게 변환할 수 있는 Python 데이터 타입으로 만들어 줌

```python
# articles/serializers.py

from rest_framework import serializers
from .models import Article, Comment

# 모든 게시글 정보를 반환하기 위한 ModelSerializer
class ArticleListSerializer(serializers.ModelSerializer):

    class Meta:
        model = Article
        fields = ('id', 'title',)

class CommentSerializer(serializers.ModelSerializer):
	# Comment 테이블의 모든 필드의 데이터를 넘기고 참조하는 데이터는 유효성 검사 x
    class Meta:
        model = Comment
        fields = '__all__'
        read_only_fields = ('article',)

# 게시글 상세 정보를 반환 및 생성하기 위한 ModelSerializer
class ArticleSerializer(serializers.ModelSerializer):
    # 게시글마다
    comment_set = CommentSerializer(many=True, read_only=True)
    comment_count = serializers.IntegerField(source='comment_set.count', read_only=True)
	# Article의 모든 필드의 데이터를 넘김
    class Meta:
        model = Article
        fields = '__all__'
```

- `ArticleListSerializer` - `Article` 테이블의 필드 중 `id`와 `title`  데이터만 넘겨주는 클래스
- `CommentSerializer` - `Comment` 테이블의 모든 필드의 데이터를 넘김, 
  - `read_only_fields`  - 참조하는 데이터는 유효성 검사하지 않기 작성
- `ArticleSerializer` - Article 테이블의 모든 필드와 직접 작성한 `comment_set`와 `comment_count` 데이터를 같이 넘김, `read_only` 옵션을 주었기 때문에 유효성 검사는 진행되지 않음

### 7. url 작성

```python
# my_api/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/', include('articles.urls')),
]
```

### 8. `django-seed` 라이브러리를 사용해서 모델 구조에 맞는 가상 데이터 생성

```bash
$ python manage.py seed articles --number=20
```



# 🌈 게시글 관련

<br>

<hr>



## 🔗 GET - api/v1/articles/ (모든 게시글 조회)

> 모든 게시글의 id와 title 컬럼을 JSON 데이터 타입으로 응답하기

### 1. serializer 작성

```python
# articles/serializers.py

from rest_framework import serializers
from .models import Article

# 모든 게시글 정보를 반환하기 위한 ModelSerializer
class ArticleListSerializer(serializers.ModelSerializer):

    class Meta:
        model = Article
        fields = ('id', 'title',)
```

### 2. url 작성

```python
# articles/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path('articles/', views.article_list),
]
```

### 3. view 작성

```python
# articles/views.py

from rest_framework.response import Response
from rest_framework.decorators import api_view
from django.shortcuts import get_list_or_404
from .models import Article
from .serializers import ArticleListSerializer

@api_view(['GET'])
def article_list(request):
    # 게시글 조회
    if request.method == 'GET':
        articles = get_list_or_404(Article)
        serializer = ArticleListSerializer(articles, many=True)
        return Response(serializer.data)
```

- `many=True` - 단일 인스턴스 대신 QuerySet 등을 직렬화 하기 위해서는 serializer를 인스턴스화 할 때 many=True를 키워드 인자로 전달해야 함

- `api_view` decorator 
  - 기본적으로 GET 메서드만 허용되며 다른 메서드 요청에 대해서는 405 Method Not Allowed로 응답
  - View 함수가 응답해야 하는 HTTP 메서드의 목록을 리스트의 인자로 받음
  - DRF에서는 선택이 아닌 필수적으로 작성해야 해당 view 함수가 정상적으로 동작함

## 🔗 POST- api/v1/articles/ (게시글 생성)

> - 검증에 성공하는 경우 새로운 게시글의 정보를 DB에 저장하고, 저장된 게시글의 정보를 응답한다.
>
> - 검증에 실패하는 경우 400 Bad Request 예외를 발생시킨다.
> - `article_list` 함수로 게시글을 조회하거나 생성하는 행위를 모두 처리 가능

### 1. view 작성

```python
# articles/views.py
from res_framework import status

@api_view(['GET', 'POST'])
def article_list(request):
    # 게시글 조회
    if request.method == 'GET':
        articles = get_list_or_404(Article)
        serializer = ArticleListSerializer(articles, many=True)
        return Response(serializer.data)
    # 게시글 생성
    elif request.method == 'POST':
        serializer = ArticleSerializer(data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
```

- `status=status.HTTP_201_CREATED` 
  - 201 Created 상태 코드 및 메시지 응답
  - DRF에는 status code를 보다 명확하고 읽기 쉽게 만드는데 사용할 수 있는 정의된 상수 집합을 제공
  - `status` 모듈에 HTTP status code 집합이 모두 포함되어 있음
- `raise_exception` 
  - ''Raising an exception on invalid data"
  - `is_valid()`는 유효성 검사 오류가 있는 경우 `serializers.ValidationError` 예외를 발생시키는 선택적 `raise_exception` 인자를 사용할 수 있음
  - DRF에서 제공하는 기본 예외 처리기에 의해 자동으로 처리되며, 기본적으로 HTTP status code 400을 응답으로 반환함



## 🔗 GET - api/v1/articles/article_pk/ (특정 게시글 조회)

> 특정 게시글의 모든 컬럼을 JSON 데이터 타입으로 응답한다.

### 1. serializer 작성

```python
# articles/serializers.py

class ArticleSerializer(serializers.ModelSerializer):

    class Meta:
        model = Article
        fields = '__all__'
```

### 2. url 작성

```python
# articles/urls.py

urlpatterns = [
    path('articles/<int:article_pk>/', views.article_detail),
]
```

### 3. view 작성

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



## 🔗 DELETE - api/v1/articles/article_pk/ (특정 게시글 삭제)

> - DELETE 요청인 경우 특정 게시글을 삭제하고 삭제가 완료되면, 삭제한 게시글의 id를 응답한다. 
>
> - 204 No Content 상태 코드 및 메시지 응답
> - `article_detail` 함수로 상세 게시글을 조회하거나 삭제하는 행위 모두 처리 가능

### view 작성

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
            'delete' : f'데이터 {article_pk}번이 삭제되었습니다.'
        }
        return Response(data, status=status.HTTP_204_NO_CONTENT)
```



## 🔗 PUT - api/v1/articles/article_pk/ (특정 게시글 수정)

> - PUT 요청인 경우 특정 게시글의 정보를 수정한다.
>   - 검증에 성공하는 경우 수정된 게시글의 정보를 DB에 저장한다.
>   - 검증에 실패할 경우 400 Bad Request 예외를 발생시킨다.
>   - 수정이 완료되면 수정한 게시글의 정보를 응답한다.
> - `article_detail` 함수로 상세 게시글을 조회하거나 삭제, 수정하는 행위 모두 처리 가능

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
            'delete' : f'데이터 {article_pk}번이 삭제되었습니다.'
        }
        return Response(data)
    elif request.method == 'PUT':
        serializer = ArticleSerializer(article, data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data)
```



# 🌈 댓글 관련

## 🔗 GET - api/v1/comments/ (모든 댓글 조회)

> 모든 댓글의 필드를 JSON 데이터 타입으로 응답하기

### 1. serializer 작성

```python
# articles/serializers.py
from .models import Article, Comment

class CommentSerializer(serializers.ModelSerializer):

    class Meta:
        model = Comment
        fields = '__all__'
```

### 2. url 작성

```python
# articles/urls.py

urlpatterns = [
    ...,
    path('comments/', views.comment_list),
]
```

### 3. view 작성

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



## 🔗 GET - api/v1/comments/comment_pk/ (특정 댓글 조회)

> GET 요청인 경우 특정 댓글의 모든 컬럼을 JSON으로 응답

### 1. url 작성

```python
# articles/urls.py

urlpatterns = [
    ...,
    path('comments/<int:comment_pk>/', views.comment_detail),
]
```

### 2. view 작성

```python
# articles/views.py

@api_view(['GET'])
def comment_detail(request, comment_pk):
    comment = get_object_or_404(Comment, pk=comment_pk)
    serializer = CommentSerializer(comment)
    return Response(serializer.data)
```

## 🔗 DELETE & PUT- api/v1/comments/comment_pk/ (특정 댓글 삭제 & 수정)

> DELETE 요청인 경우 특정 댓글의 정보를 삭제한다.
>
> - 삭제가 완료된 이후에 삭제한 댓글의 id와 204 No Content를 응답한다.
>
> PUT 요청인 경우 특정 댓글의 정보를 수정한다.
>
> - 검증에 성공하는 경우 수정된 댓글의 정보를 DB에 저장한다.
> - 검증에 실패하는 경우 400 Bad Request를 응답한다.
> - 수정이 완료된 이후에 수정된 음악의 정보를 응답한다.
>
> `Article` 생성 로직에서와 마찬가지로 `comment_detail` 함수가 모두 처리할 수 있도록 작성

### view 수정

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
            'delete' : f'댓글 {comment_pk}번이 삭제되었습니다.'
        }
        return Response(data)

    elif request.method == 'PUT':
        serializer = CommentSerializer(comment, data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save()
            return Response(serializer.data)
```





## 🔗 POST - api/v1/articles/article_pk/comments/ (댓글 생성)

> 특정 게시글의 댓글 생성

### 1. url 작성

```python
# articles/urls.py

urlpatterns = [
    ...,
    path('articles/<int:article_pk>/comments/', views.comment_create),
]
```

### 2. view 작성

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

- Article 생성과 달리 Comment 생성은 생성 시에 참조하는 모델의 객체 정보가 필요한다.
  - 1:N 관계에서 N은 어떤 1을 참조하는지에 대한 정보가 필요하기 때문 (외래 키)
- `serializer.save(article=article)`
  - `.save()` 메서드는 특정 Serializer 인스턴스를 저장하는 과정에서 추가적인 데이터를 받을 수 있음
  - 인스턴스를 저장하는 시점에 추가 데이터 삽입이 필요한 경우

### 3. serializer 수정

```python
# articles/serializers.py

class CommentSerializer(serializers.ModelSerializer):

    class Meta:
        model = Comment
        fields = '__all__'
        read_only_fields = ('article',)
```

- `Read Only Field`
  - 어떤 게시글에 작성하는 댓글인지에 대한 정보를 form-data로 넘겨주지 않았기 때문에 `serializer` 하는 과정에서 `article` 필드가 유효성 검사를 통과하지 못함
    - CommentSerializer에서 article 필드에 해당하는 데이터 또한 요청으로부터 받아서 `serializer`나는 것으로 설정되어 있기 때문!
  - 이때는 읽기 전용 필드(read_only_fields) 설정을 통해 `serializer`하지 않고 반환 값에만 해당 필드가 포함되도록 설정할 수 있음 



## 🌈1:N Serializer

### 🔗 GET - api/v1/articles/article_pk/ (특정 게시글에 작성된 댓글 목록 출력)

- `Serializer`는 기존 필드를 override 하거나 추가 필드를 구성할 수 있음
  1. PrimaryKeyField
  2. Nested relationships

### case1) PrimaryKeyRelatedField

```python
# articles/serializers.py

class ArticleSerializer(serializers.ModelSerializer):
    comment_set = serializers.PrimaryKeyRelatedField(many=True, read_only=True)

    class Meta:
        model = Article
        fields = '__all__'
```

- pk를 사용하여 관계된 대상을 나타내는 데 사용할 수 있음
- 필드가 `to many relationships(N)`를 나타내는데 사용되는 경우 `many=True` 속성 필요
- `comment_set` 필드 값을 form-data로 받지 않으므로 `read_only=True` 설정 필요

[참고] 역참조 시 생성되는 `comment_set`을 다른 매니저 이름으로 수정할 수 있음, 단 수정할 경우 이전 `serializers.py`에서의 클래스 변수명도 일치하도록 수정해야 함

![image-20220422013521246](Django%20-%20REST%20API%20%EC%84%9C%EB%B2%84%20%EA%B5%AC%ED%98%84.assets/image-20220422013521246.png)

`api/v1/articles/1/`로 GET 요청 후 응답 확인



### case2) Nested relationships

```python
# articles/serializers.py

class ArticleSerializer(serializers.ModelSerializer):
    comment_set = CommentSerializer(many=True, read_only=True)

    class Meta:
        model = Article
        fields = '__all__'
```

- 모델 관계상으로 참조된 대상은 참조하는 대상의 표현(응답)에 포함되거나 중첩(nested)될 수 있음
- 이러한 중첩된 관계를 `serializers`를 필드로 사용하여 표현할 수 있음
- 두 클래스의 상하 위치 변경!!!

### 🔗 GET - api/v1/articles/article_pk/ (특정 게시글에 작성된 댓글의 개수 구하기)

```python
# articles/serializers.py

class ArticleSerializer(serializers.ModelSerializer):
    comment_set = CommentSerializer(many=True, read_only=True)
    comment_count = serializers.IntegerField(source='comment_set.count', read_only=True)

    class Meta:
        model = Article
        fields = '__all__'
```

- `comment_set` 매니저는 모델 관계로 인해 자동으로 구성되기 때문에 커스텀 필드를 구성하지 않아도 `comment_set`이라는 필드명을 `fields` 옵션에 작성만 해도 사용할 수 있었음 
- 하지만 지금처럼 별도의 값을 위한 필드를 사용하려는 경우 자동으로 구성되는 매니저가 아니기 때문에 직접 필드를 작성해야 함

- `source='comment_set.count'`

  - 필드를 채우는 데 사용할 속성의 이름
  - 점 표기법(dot notation)을 사용하여 속성을 탐색할 수 있음
  - `comment_set`이라는 필드에 .(dot)을 통해 전체 댓글의 개수 확인 가능
  - `count()`는 built-in Queryset API 중 하나

- `read_only=True`

  - 특정 필드를 override 혹은 추가한 경우 `read_only_fields`를 사용할 수 없음

  ![image-20220422014549008](Django%20-%20REST%20API%20%EC%84%9C%EB%B2%84%20%EA%B5%AC%ED%98%84.assets/image-20220422014549008.png)

  