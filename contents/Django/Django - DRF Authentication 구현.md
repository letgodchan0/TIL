# 📩 REST API

### DRF를 활용하여 Authentication 기능 구현

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
    
    # native apps
    ...
]
```

<br>

### 4. Custom User 등록

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

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

<br>

### 5. `models.py` 생성

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

### 7. `serializers.py` 생성

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

<br>

### 8. url 작성

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

### 9. View 작성

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
        # comment 개수 추가
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

        # 기존 serializer 가 return 되면, 단일 comment 만 응답으로 받게됨.
        # 사용자가 댓글을 입력하는 사이에 업데이트된 comment 확인 불가 => 업데이트된 전체 목록 return 
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

### 10. 인증 관련 라이브러리 설치 및 세팅 👉 [공식문서](https://django-rest-auth.readthedocs.io/en/latest/installation.html) 참고

```bash
$ pip install django-allauth
$ pip install dj-rest-auth
$ pip freeze > requirements.txt # requirements.txt에 위에 2개 저장
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
    'rest_framework.authtoken', # token 기반 auth

    # DRF auth 담당
    'dj_rest_auth',  # sinup 제외 모든 auth 관련 담당

    # native apps
    ...
]

...

# DRF 인증 관련 설정
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        # 우리 사이트는 인증을 토큰으로 할꺼임
        'rest_framework.authentication.TokenAuthentication',
    ],
    'DEFAULT_PERMISSION_CLASSES': [
        # 모두에게 허용 - 인증 하던 안하던 우리 사이트 이용 가능
        # 'rest_framework.permissions.AllowAny', 

        # 인증된 사용자만 모든일이 가능 / 비인증 사용자는 모두 401 Unauthorized
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

## 💡 dj-rest-auth

![image-20220519023543906](Django%20-%20DRF%20Authentication%20%EA%B5%AC%ED%98%84.assets/image-20220519023543906.png)

- 현 상태로 서버를 찍어보면 `dj-rest-auth` 가 제공하는 것들을 확인할 수 있다. 

- 즉, 저 url로 요청만 잘보내면 로그인, 로그아웃등을 다 해준다!

- url 구별하기가 어렵기 때문에 수정해주자!

  ```python
  # urls.py
  
  urlpatterns = [
      ...
      path('api/v1/accounts/', include('dj_rest_auth.urls')),
  ]
  ```

  

<br>

## 💡 Login

![image-20220519024532617](Django%20-%20DRF%20Authentication%20%EA%B5%AC%ED%98%84.assets/image-20220519024532617.png)

- 인증된 사용자로 로그인을 하면 token을 반환해주는 것을 확인할 수 있고, 이 token이 데이터베이스에 저장되어 있다. 즉 이 순간부터 로그인이 된 것!

<br>

## 💡 Logout

![image-20220519024910065](Django%20-%20DRF%20Authentication%20%EA%B5%AC%ED%98%84.assets/image-20220519024910065.png)

- 언뜻 보면 로그아웃이 잘된 것 같지만 지금 상황은 로그아웃 할 때 토큰값을 보여주지 않고 그냥 무지성으로 로그아웃 해버린 것이다. 
- 즉, 인증된 사용자가 서버에 요청을 할때 토큰 값을 넘기지 않았는데도 로그아웃을 해버린 것이다!

<hr>

![image-20220519025556227](Django%20-%20DRF%20Authentication%20%EA%B5%AC%ED%98%84.assets/image-20220519025556227.png)

- Headers의 Authorization 값에 토큰을 넣어주면 성공적으로 로그아웃한 것을 확인할 수 있다!
- DB의 토큰을 관리하는 테이블에도 해당 토큰 값이 삭제되는 것을 확인할 수 있다. 
- 만약 토큰 값이 잘못되면 `Invalid token`이라고 응답을 한다.

<br>

## Registration (sinup 기능 제공) 👉 [공식문서](https://django-rest-auth.readthedocs.io/en/latest/installation.html) 

```python
# settings.py

INSTALLED_APPS = [
    ...
    # DRF auth 담당
    'dj_rest_auth',  # sinup 제외 모든 auth 관련 담당
    'dj_rest_auth.registration',

    # sinup을 위해 필요
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

- 이 상태로 서버를 키기 전에 migrate를 해주어야 한다. 나는  settings.py에서 등록 해주는 `allauth.socialaccount,` 얘 때문에 migrate가 안되는 에러가 발생했는데, 이건 사용하지 않으니까 주석처리 하면서 해결했다.

<br>

## 💡 signup

![image-20220519031756464](Django%20-%20DRF%20Authentication%20%EA%B5%AC%ED%98%84.assets/image-20220519031756464.png)

- Body의 form-data에 입력해도 되고, raw의 JSON으로 입력해도 된다. 여튼 입력만 하고 요청을 하면 회원가입이 완료되고 DB에 새로운 user가 생성되고 토큰도 발급해준다.
- 기존에는 회원가입 하면 user 객체를 생성하고 직접 `auth_login` 통해 로그인 시켜달라고 했는데...ㄷㄷ

<br>

## 💡 profile

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

## 11. CORS header 추가

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
    'corsheaders.middleware.CorsMiddleware', # CommonMiddleware보다 위에 있어야 함
	...
    'django.middleware.common.CommonMiddleware',
	...
]

...

# 특정 origin 에게만 교차 출처 허용
CORS_ALLOWED_ORIGINS = [
    # Vue LocalHost
    'http://localhost:8080',
    'http://127.0.0.1:8001',
]

# or 모두에게 교차출처 허용
# CORS_ALLOW_ALL_ORIGINS = True
```

