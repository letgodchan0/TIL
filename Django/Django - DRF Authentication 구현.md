# 📩 REST API

### DRF를 활용하여 Authentication

<hr>

### 1. 가상환경 설정 및 패키지 설치

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

<br>

### 2. 프로젝트 생성 및 서버 실행

```
$ django-admin startproject my_api .
$ python manage.py runserver
```

<br>

### 3. 앱 생성 및 `INSTALLED_APPS`에 등록

```bash
$ python manage.py startapp articles	# 게시판
$ python manage.py startapp accounts	# 회원가입 로그인 관련
```

```python
# settings.py

INSTALLED_APPS = [
    # Local apps
    'articles',
    'accounts',
    
    # 3rd party apps
    'django_extensions',
    'rest_framework'
    'django_seed', 
    
    # native apps
    ...
]
```

### 4. `models.py` 생성



```python
# accounts/models.py
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass
```

```python
# settings.py 맨 밑에

AUTH_USER_MODEL = 'accounts.User'
```

```python
# accounts/admin.py
# admin site에 Custom User 모델 등록

from django.contrib import admin
from django.contrib.auth.admin import UserAdmin
from .models import User

admin.site.register(User, UserAdmin)
```







```python
# articles/models.py

from django.db import models
from django.conf import settings

class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE, related_name='articles')
    title = models.CharField(max_length=100)
    content = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')


class Comment(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE, related_name='comments')
    article = models.ForeignKey(Article, on_delete=models.CASCADE, related_name='comments')
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

```python
# articles/serializers/article.py

from rest_framework import serializers
from django.contrib.auth import get_user_model

from ..models import Article
from .comment import CommentSerializer

User = get_user_model()


class ArticleSerializer(serializers.ModelSerializer):
    
    class UserSerializer(serializers.ModelSerializer):
        class Meta:
            model = User
            fields = ('pk', 'username')

    comments = CommentSerializer(many=True, read_only=True)
    user = UserSerializer(read_only=True)
    like_users = UserSerializer(read_only=True, many=True)

    class Meta:
        model = Article
        fields = ('pk', 'user', 'title', 'content', 'comments', 'like_users')


# Article List Read
class ArticleListSerializer(serializers.ModelSerializer):
    class UserSerializer(serializers.ModelSerializer):
        class Meta:
            model = User
            fields = ('pk', 'username')

    user = UserSerializer(read_only=True)
    # queryset annotate (views에서 채워줄것!)
    comment_count = serializers.IntegerField()
    like_count = serializers.IntegerField()

    class Meta:
        model = Article
        fields = ('pk', 'user', 'title', 'comment_count', 'like_count')
```

```python
# articles/serializers/comment.py

from rest_framework import serializers
from django.contrib.auth import get_user_model
from ..models import Comment

User = get_user_model()

class CommentSerializer(serializers.ModelSerializer):
    
    class UserSerializer(serializers.ModelSerializer):
        class Meta:
            model = User
            fields = ('pk', 'username')

    user = UserSerializer(read_only=True)

    class Meta:
        model = Comment
        fields = ('pk', 'user', 'content', 'article',)
        read_only_fields = ('article', )
```

```python
# accounts/serializers.py

from rest_framework import serializers
from django.contrib.auth import get_user_model
from articles.models import Article

class ProfileSerializer(serializers.ModelSerializer):

    class ArticleSerializer(serializers.ModelSerializer):
        
        class Meta:
            model = Article
            fields = ('pk', 'title', 'content')

    like_articles = ArticleSerializer(many=True)
    articles = ArticleSerializer(many=True)

    class Meta:
        model = get_user_model()
        fields = ('pk', 'username', 'email', 'like_articles', 'articles',)
```

