# 🌱 REST API

### DRF를 활용하여 게시글 관련 REST API 서버 구축하기

</div>

프로젝트는 `my_api` , 앱은 `articles`인 REST API 서버를 통해 게시판과 댓글 관련 데이터를 넘겨주는 방법을 다루었습니다.



## REST API 서버 구축을 위한 기본세팅

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

### 4.  `models.py` 생성

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

### 7. `django-seed` 라이브러리를 사용해서 모델 구조에 맞는 가상 데이터 생성

```bash
$ python manage.py seed articles --number=20
```

### 8. url 작성

```python
# my_api/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/', include('articles.urls')),
]
```



## 🔗 GET - api/v1/articles/

> 모든 게시글의 id와 title 컬럼을 JSON 데이터 타입으로 응답하기

### 1. url 작성

```python
# articles/urls.py

from django.urls import path
from . import views

urlpatterns = [
    path('articles/', views.article_list),
]
```





