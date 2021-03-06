# ๐ฉ REST API

### DRF๋ฅผ ํ์ฉํ์ฌ Authentication ๊ธฐ๋ฅ ๊ตฌํ

<hr>

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

<br>

### 2. ํ๋ก์ ํธ ์์ฑ ๋ฐ ์๋ฒ ์คํ

```
$ django-admin startproject my_api .
$ python manage.py runserver
```

<br>

### 3. ์ฑ ์์ฑ ๋ฐ `INSTALLED_APPS`์ ๋ฑ๋ก

```bash
$ python manage.py startapp articles	# ๊ฒ์ํ
$ python manage.py startapp accounts	# ํ์๊ฐ์ ๋ก๊ทธ์ธ ๊ด๋ จ
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
    
    # native apps
    ...
]
```

<br>

### 4. Custom User ๋ฑ๋ก

```python
# accounts/models.py
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass
```

```python
# settings.py ๋งจ ๋ฐ์

AUTH_USER_MODEL = 'accounts.User'
```

```python
# accounts/admin.py
# admin site์ Custom User ๋ชจ๋ธ ๋ฑ๋ก

from django.contrib import admin
from django.contrib.auth.admin import UserAdmin
from .models import User

admin.site.register(User, UserAdmin)
```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

<br>

### 5. `models.py` ์์ฑ

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

<br>

### 6. migration

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

<br>

### 7. `serializers.py` ์์ฑ

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
    # queryset annotate (views์์ ์ฑ์์ค๊ฒ!)
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

<br>

### 8. url ์์ฑ

```python
# api/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/', include('articles.urls')),
]
```

```python
# accounts/urls.py

from django.urls import path
from . import views

app_name = 'accounts'

urlpatterns = [
    path('profile/<username>/', views.profile),
]
```

```python
# articles/urls.py

from django.urls import path
from . import views

app_name = 'articles'

urlpatterns = [
    # articles
    path('', views.article_list_or_create),
    path('<int:article_pk>/', views.article_detail_or_update_or_delete),
    path('<int:article_pk>/like/', views.like_article),
    # comments
    path('<int:article_pk>/comments/', views.create_comment),
    path('<int:article_pk>/comments/<int:comment_pk>/', views.comment_update_or_delete)
]
```

<br>

### 9. View ์์ฑ

```python
# articles/views.py

from django.shortcuts import get_object_or_404
from django.db.models import Count

from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response
from .models import Article, Comment
from .serializers.article import ArticleListSerializer, ArticleSerializer
from .serializers.comment import CommentSerializer


@api_view(['GET', 'POST'])
def article_list_or_create(request):

    def article_list():
        # comment ๊ฐ์ ์ถ๊ฐ
        articles = Article.objects.annotate(
            comment_count=Count('comments', distinct=True),
            like_count=Count('like_users', distinct=True)
        ).order_by('-pk')
        serializer = ArticleListSerializer(articles, many=True)
        return Response(serializer.data)
    
    def create_article():
        serializer = ArticleSerializer(data=request.data)
        if serializer.is_valid(raise_exception=True):
            serializer.save(user=request.user)
            return Response(serializer.data, status=status.HTTP_201_CREATED)

    if request.method == 'GET':
        return article_list()
    elif request.method == 'POST':
        return create_article()


@api_view(['GET', 'PUT', 'DELETE'])
def article_detail_or_update_or_delete(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)

    def article_detail():
        serializer = ArticleSerializer(article)
        return Response(serializer.data)

    def update_article():
        if request.user == article.user:
            serializer = ArticleSerializer(instance=article, data=request.data)
            if serializer.is_valid(raise_exception=True):
                serializer.save()
                return Response(serializer.data)

    def delete_article():
        if request.user == article.user:
            article.delete()
            return Response(status=status.HTTP_204_NO_CONTENT)

    if request.method == 'GET':
        return article_detail()
    elif request.method == 'PUT':
        if request.user == article.user:
            return update_article()
    elif request.method == 'DELETE':
        if request.user == article.user:
            return delete_article()

@api_view(['POST'])
def like_article(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    user = request.user
    if article.like_users.filter(pk=user.pk).exists():
        article.like_users.remove(user)
        serializer = ArticleSerializer(article)
        return Response(serializer.data)
    else:
        article.like_users.add(user)
        serializer = ArticleSerializer(article)
        return Response(serializer.data)

@api_view(['POST'])
def create_comment(request, article_pk):
    user = request.user
    article = get_object_or_404(Article, pk=article_pk)
    
    serializer = CommentSerializer(data=request.data)
    if serializer.is_valid(raise_exception=True):
        serializer.save(article=article, user=user)

        # ๊ธฐ์กด serializer ๊ฐ return ๋๋ฉด, ๋จ์ผ comment ๋ง ์๋ต์ผ๋ก ๋ฐ๊ฒ๋จ.
        # ์ฌ์ฉ์๊ฐ ๋๊ธ์ ์๋ ฅํ๋ ์ฌ์ด์ ์๋ฐ์ดํธ๋ comment ํ์ธ ๋ถ๊ฐ => ์๋ฐ์ดํธ๋ ์ ์ฒด ๋ชฉ๋ก return 
        comments = article.comments.all()
        serializer = CommentSerializer(comments, many=True)
        return Response(serializer.data, status=status.HTTP_201_CREATED)


@api_view(['PUT', 'DELETE'])
def comment_update_or_delete(request, article_pk, comment_pk):
    article = get_object_or_404(Article, pk=article_pk)
    comment = get_object_or_404(Comment, pk=comment_pk)

    def update_comment():
        if request.user == comment.user:
            serializer = CommentSerializer(instance=comment, data=request.data)
            if serializer.is_valid(raise_exception=True):
                serializer.save()
                comments = article.comments.all()
                serializer = CommentSerializer(comments, many=True)
                return Response(serializer.data)

    def delete_comment():
        if request.user == comment.user:
            comment.delete()
            comments = article.comments.all()
            serializer = CommentSerializer(comments, many=True)
            return Response(serializer.data)
    
    if request.method == 'PUT':
        return update_comment()
    elif request.method == 'DELETE':
        return delete_comment()
```

```python
# accounts/views.py

from django.shortcuts import get_object_or_404
from django.contrib.auth import get_user_model

from rest_framework.decorators import api_view
from rest_framework.response import Response

from .serializers import ProfileSerializer

User = get_user_model()

@api_view(['GET'])
def profile(request, username):
    user = get_object_or_404(User, username=username)
    serializer = ProfileSerializer(user)
    return Response(serializer.data)
```

<hr>

<br>

### 10. ์ธ์ฆ ๊ด๋ จ ๋ผ์ด๋ธ๋ฌ๋ฆฌ ์ค์น ๋ฐ ์ธํ ๐ [๊ณต์๋ฌธ์](https://django-rest-auth.readthedocs.io/en/latest/installation.html) ์ฐธ๊ณ 

```bash
$ pip install django-allauth
$ pip install dj-rest-auth
$ pip freeze > requirements.txt # requirements.txt์ ์์ 2๊ฐ ์ ์ฅ
```

```python
# settings.p
INSTALLED_APPS = [
    
    # local apps
    'accounts',
    'articles',
    
    # 3rd party apps
    'django_extensions'
    'rest_framework',
    'rest_framework.authtoken', # token ๊ธฐ๋ฐ auth

    # DRF auth ๋ด๋น
    'dj_rest_auth',  # sinup ์ ์ธ ๋ชจ๋  auth ๊ด๋ จ ๋ด๋น

    # native apps
    ...
]

...

# DRF ์ธ์ฆ ๊ด๋ จ ์ค์ 
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        # ์ฐ๋ฆฌ ์ฌ์ดํธ๋ ์ธ์ฆ์ ํ ํฐ์ผ๋ก ํ ๊บผ์
        'rest_framework.authentication.TokenAuthentication',
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        # ๋ชจ๋์๊ฒ ํ์ฉ - ์ธ์ฆ ํ๋ ์ํ๋ ์ฐ๋ฆฌ ์ฌ์ดํธ ์ด์ฉ ๊ฐ๋ฅ
        # 'rest_framework.permissions.AllowAny', 

        # ์ธ์ฆ๋ ์ฌ์ฉ์๋ง ๋ชจ๋ ์ผ์ด ๊ฐ๋ฅ / ๋น์ธ์ฆ ์ฌ์ฉ์๋ ๋ชจ๋ 401 Unauthorized
        'rest_framework.permissions.IsAuthenticated'
    ]
}
```

```python
# urls.py

urlpatterns = [
    ...,
    path('dj-rest-auth/', include('dj_rest_auth.urls')),
]
```

```bash
$ python manage.py migrate
```



<br>

## ๐ก dj-rest-auth

![image-20220519023543906](Django%20-%20DRF%20Authentication%20%EA%B5%AC%ED%98%84.assets/image-20220519023543906.png)

- ํ ์ํ๋ก ์๋ฒ๋ฅผ ์ฐ์ด๋ณด๋ฉด `dj-rest-auth` ๊ฐ ์ ๊ณตํ๋ ๊ฒ๋ค์ ํ์ธํ  ์ ์๋ค. 

- ์ฆ, ์  url๋ก ์์ฒญ๋ง ์๋ณด๋ด๋ฉด ๋ก๊ทธ์ธ, ๋ก๊ทธ์์๋ฑ์ ๋ค ํด์ค๋ค!

- url ๊ตฌ๋ณํ๊ธฐ๊ฐ ์ด๋ ต๊ธฐ ๋๋ฌธ์ ์์ ํด์ฃผ์!

  ```python
  # urls.py
  
  urlpatterns = [
      ...
      path('api/v1/accounts/', include('dj_rest_auth.urls')),
  ]
  ```

  

<br>

## ๐ก Login

![image-20220519024532617](Django%20-%20DRF%20Authentication%20%EA%B5%AC%ED%98%84.assets/image-20220519024532617.png)

- ์ธ์ฆ๋ ์ฌ์ฉ์๋ก ๋ก๊ทธ์ธ์ ํ๋ฉด token์ ๋ฐํํด์ฃผ๋ ๊ฒ์ ํ์ธํ  ์ ์๊ณ , ์ด token์ด ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์ ์ฅ๋์ด ์๋ค. ์ฆ ์ด ์๊ฐ๋ถํฐ ๋ก๊ทธ์ธ์ด ๋ ๊ฒ!

<br>

## ๐ก Logout

![image-20220519024910065](Django%20-%20DRF%20Authentication%20%EA%B5%AC%ED%98%84.assets/image-20220519024910065.png)

- ์ธ๋ป ๋ณด๋ฉด ๋ก๊ทธ์์์ด ์๋ ๊ฒ ๊ฐ์ง๋ง ์ง๊ธ ์ํฉ์ ๋ก๊ทธ์์ ํ  ๋ ํ ํฐ๊ฐ์ ๋ณด์ฌ์ฃผ์ง ์๊ณ  ๊ทธ๋ฅ ๋ฌด์ง์ฑ์ผ๋ก ๋ก๊ทธ์์ ํด๋ฒ๋ฆฐ ๊ฒ์ด๋ค. 
- ์ฆ, ์ธ์ฆ๋ ์ฌ์ฉ์๊ฐ ์๋ฒ์ ์์ฒญ์ ํ ๋ ํ ํฐ ๊ฐ์ ๋๊ธฐ์ง ์์๋๋ฐ๋ ๋ก๊ทธ์์์ ํด๋ฒ๋ฆฐ ๊ฒ์ด๋ค!

<hr>

![image-20220519025556227](Django%20-%20DRF%20Authentication%20%EA%B5%AC%ED%98%84.assets/image-20220519025556227.png)

- Headers์ Authorization ๊ฐ์ ํ ํฐ์ ๋ฃ์ด์ฃผ๋ฉด ์ฑ๊ณต์ ์ผ๋ก ๋ก๊ทธ์์ํ ๊ฒ์ ํ์ธํ  ์ ์๋ค!
- DB์ ํ ํฐ์ ๊ด๋ฆฌํ๋ ํ์ด๋ธ์๋ ํด๋น ํ ํฐ ๊ฐ์ด ์ญ์ ๋๋ ๊ฒ์ ํ์ธํ  ์ ์๋ค. 
- ๋ง์ฝ ํ ํฐ ๊ฐ์ด ์๋ชป๋๋ฉด `Invalid token`์ด๋ผ๊ณ  ์๋ต์ ํ๋ค.

<br>

## Registration (sinup ๊ธฐ๋ฅ ์ ๊ณต) ๐ [๊ณต์๋ฌธ์](https://django-rest-auth.readthedocs.io/en/latest/installation.html) 

```python
# settings.py

INSTALLED_APPS = [
    ...
    # DRF auth ๋ด๋น
    'dj_rest_auth',  # sinup ์ ์ธ ๋ชจ๋  auth ๊ด๋ จ ๋ด๋น
    'dj_rest_auth.registration',

    # sinup์ ์ํด ํ์
    'allauth',
    'allauth.account',
    'allauth.socialaccount',
    
    # native apps
    ...
    'django.contrib.sites',
]

SITE_ID = 1
```

```PYTHON
# urls.py

urlpatterns = [
	...,
    path('api/v1/accounts/signup/', include('dj_rest_auth.registration.urls')),
]
```

```bash
$ python manage.py migrate
```

- ์ด ์ํ๋ก ์๋ฒ๋ฅผ ํค๊ธฐ ์ ์ migrate๋ฅผ ํด์ฃผ์ด์ผ ํ๋ค. ๋๋  settings.py์์ ๋ฑ๋ก ํด์ฃผ๋ `allauth.socialaccount,` ์ ๋๋ฌธ์ migrate๊ฐ ์๋๋ ์๋ฌ๊ฐ ๋ฐ์ํ๋๋ฐ, ์ด๊ฑด ์ฌ์ฉํ์ง ์์ผ๋๊น ์ฃผ์์ฒ๋ฆฌ ํ๋ฉด์ ํด๊ฒฐํ๋ค.

<br>

## ๐ก signup

![image-20220519031756464](Django%20-%20DRF%20Authentication%20%EA%B5%AC%ED%98%84.assets/image-20220519031756464.png)

- Body์ form-data์ ์๋ ฅํด๋ ๋๊ณ , raw์ JSON์ผ๋ก ์๋ ฅํด๋ ๋๋ค. ์ฌํผ ์๋ ฅ๋ง ํ๊ณ  ์์ฒญ์ ํ๋ฉด ํ์๊ฐ์์ด ์๋ฃ๋๊ณ  DB์ ์๋ก์ด user๊ฐ ์์ฑ๋๊ณ  ํ ํฐ๋ ๋ฐ๊ธํด์ค๋ค.
- ๊ธฐ์กด์๋ ํ์๊ฐ์ ํ๋ฉด user ๊ฐ์ฒด๋ฅผ ์์ฑํ๊ณ  ์ง์  `auth_login` ํตํด ๋ก๊ทธ์ธ ์์ผ๋ฌ๋ผ๊ณ  ํ๋๋ฐ...ใทใท

<br>

## ๐ก profile

```python
# urls.py

from django.urls import path
from . import views

app_name = 'accounts'

urlpatterns = [
    path('profile/<username>/', views.profile),
]
```

```python
# accounts/views.py

from django.shortcuts import get_object_or_404
from django.contrib.auth import get_user_model

from rest_framework.decorators import api_view
from rest_framework.response import Response

from .serializers import ProfileSerializer

User = get_user_model()

@api_view(['GET'])
def profile(request, username):
    user = get_object_or_404(User, username=username)
    serializer = ProfileSerializer(user)
    return Response(serializer.data)
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

![image-20220519033609531](Django%20-%20DRF%20Authentication%20%EA%B5%AC%ED%98%84.assets/image-20220519033609531.png)

<br>

## 11. CORS header ์ถ๊ฐ

```bash
$ pip install django-cors-headers
$ pip freeze > requirements.txt
```

```python
# settings.py

INSTALLED_APPS = [
    ...
	'corsheaders',
    ...
]

...

MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware', # CommonMiddleware๋ณด๋ค ์์ ์์ด์ผ ํจ
	...
    'django.middleware.common.CommonMiddleware',
	...
]

...

# ํน์  origin ์๊ฒ๋ง ๊ต์ฐจ ์ถ์ฒ ํ์ฉ
CORS_ALLOWED_ORIGINS = [
    # Vue LocalHost
    'http://localhost:8080',
    'http://127.0.0.1:8001',
]

# or ๋ชจ๋์๊ฒ ๊ต์ฐจ์ถ์ฒ ํ์ฉ
# CORS_ALLOW_ALL_ORIGINS = True
```

