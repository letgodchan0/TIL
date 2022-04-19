# 🌱 Django - 좋아요, 팔로우 기능 개발

## LIKE 기능 구현

### 1. ManyToManyField 작성 후 마이그레이션

```python
# articles/models.py

class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL)
```

```bash
$ python manage.py makemigrations
```

> 에러 발생 원인
>
> - like_users 필드 생성 시 자동으로 역참조는 `.article_set` 매니저를 생성
> - 그러나 이전 1:N(User:Article) 관계에서 이미 해당 매니저 이름을 사용 중이기 때문
> - User와 관계된 ForeignKey 또는 ManyToManyField 중 하나에 related_name 추가 필요
>
> ![image-20220419002750992](Django%20-%20%EC%A2%8B%EC%95%84%EC%9A%94,%20%ED%8C%94%EB%A1%9C%EC%9A%B0%20%EA%B8%B0%EB%8A%A5%20%EA%B0%9C%EB%B0%9C.assets/image-20220419002750992.png)

### 2. related_name 설정 후 마이그레이션 다시 진행

```python
class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')

```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

- 생성된 중개 테이블을 확인 해보면 `articles_article_like_users`라는 테이블 이름과 `id`, `article_id`, `user_id` 필드를 가지고 있다!

#### 현재 User - Article 간 사용 가능한 DB API

- `article.user` - 게시글을 작성한 유저 (1:N)
- `article.like_users` - 게시글을 좋아요 한 유저 (M:N)
- `user.article_set` - 유저가 작성한 게시글(역참조) (1:N)
- `user.like_articles` - 유저가 좋아요 한 게시글(역참조) (M:N)

### 3. url 작성

```python
# articles/urls.py

urlpatterns = [
    ...,
    path('<int:article_pk>/likes/', views.likes, name='likes'),
]
```

### 4. like view 함수 작성

```python
# articles/views.py

@require_POST
def likes(request, article_pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=article_pk)

        # 이 게시글에 좋아요를 누른 유저 목록에 현재 요청하는 유저가 있다면.. 좋아요 취소
        # if request.user in article.like_users.all(): 
        if article.like_users.filter(pk=request.user.pk).exists():
            article.like_users.remove(request.user)
        else:
            article.like_users.add(request.user)
        return redirect('articles:index')
    return redirect('accounts:login')
```

1. `article = get_object_or_404(Article, pk=article_pk)` : article_pk에 해당하는 데이터를 article 객체에 담음
2. ` if article.like_users.filter(pk=request.user.pk).exists():` : 해당 게시글의 좋아요를 누른 사람들 중 pk가 요청한 사람의 pk인지 확인
   - `article.like_users.remove(request.user)` : 이미 좋아요를 누른 사람이 또 다시 좋아요를 요청하면 좋아요 취소
   - `article.like_users.add(request.user)` : 현재 게시글의 좋아요 추가

#### QuerySet API - 'exists()'

- QuerySet에 결과가 포함되어 있으면 True를 반환하고 그렇지 않으면 False를 반환
- 특히 규모가 큰 QuerySet의 컨텍스트에서 특정 개체 존재 여부와 관련된 검색에 유용
- 고유한 필드(PK)가 있는 모델이 QuerySet의 구성원인지 여부를 찾는 가장 효율적인 방법

### 5. index 페이지에 like 출력 부분 작성

```html
<--! articles/index.html -->
{% extends 'base.html' %}
	...
    <div>
      <form action="{% url 'articles:likes' article.pk %}" method="POST">  
        {% csrf_token %}
        {% if user in article.like_users.all %}
          <input type="submit" value="좋아요 취소">
        {% else %}
          <input type="submit" value="좋아요">
        {% endif %}
      </form>
    </div>
```



## Profile Page

> 자연스러운 follow 흐름을 위한 프로필 페이지 작성

### 1. url 작성

```python
# accounts/urls.py

urlpatterns = [
    ...,
    # 여기서 주의 <username>는 문자열, 만약 제일 위에 작성하면 밑에 함수들을 문자열 취급할 수 있음!
    path('<username>/', views.profile, name='profile'),
]
```

### 2. profile view 함수 작성

```python
# accounts/views.py
from django.shortcuts import render, redirect, get_object_or_404
from django.contrib.auth import get_user_model

def profile(request, username):
    User = get_user_model()
    person = get_object_or_404(User, username=username)
    context = {
        'person': person,
    }
    return render(request, 'accounts/profile.html', context)
```

1. `User = get_user_model()` : 현재 활성화 되고 있는 User 모델
2. `person = get_object_or_404(User, username=username)` : User 모델 중 요청한 사람의 데이터를 person 객체에 담음

### 3. profile 페이지 작성

```html
<--! articles/profile.html -->
{% extends 'base.html' %}

{% block content %}
<h1>{{ person.username }}님의 프로필</h1>
    
<hr>
    
{% comment %} 이 사람이 작성한 게시글 목록 {% endcomment %}
<h2>{{ person.username }}이 작성한 게시글</h2>
{% for article in person.article_set.all %}
  <p>{{ article.title }}</p>
{% endfor %}

<hr>

{% comment %} 이 사람이 작성한 댓글 목록 {% endcomment %}
<h2>{{ person.username }}이 작성한 댓글</h2>
{% for comment in person.comment_set.all %}
  <p>{{ comment.content }}</p>
{% endfor %}

<hr>

{% comment %} 이 사람이 좋아요를 누른 게시글 목록 {% endcomment %}
<h2>{{ person.username }}이 좋아요를 누른 게시글</h2>
{% for article in person.like_articles.all %}
  <p>{{ article.title }}</p>
{% endfor %}

<a href="{% url 'articles:index' %}">[back]</a>

{% endblock content %}

```

### 4. base 페이지에 프로필 링크 작성

```html
<--! base.html -->
<body>
  <div class="container">
    {% if request.user.is_authenticated %}
      <h3>Hello, {{ user }}</h3>
      <a href="{% url 'accounts:profile' request.user.username %}">내 프로필</a>
	  ...
```

### 5. index 페이지에 프로필 링크 작성

```html
<--! articles/index.html -->
     <p>작성자:  <a href="{% url 'accounts:profile' article.user.username %}">{{ article.user }}</a></p>
```



## Follow 기능 구현

### 1. ManyToManyField 작성 후 마이그레이션

```python
# accounts/models.py

class User(AbstractUser):
    followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

### 2. url 작성

```python
# accounts/urls.py

urlpatterns = [
    ...,
    path('<int:user_pk>/follow/', views.follow, name='follow'),
]
```

### 3. follow view 함수 작성

```python
# accounts/views.py

@require_POST
def follow(request, user_pk):
    if request.user.is_authenticated:
        you = get_object_or_404(get_user_model(), pk=user_pk)
        me = request.user

        if me != you:
            if you.followers.filter(pk=me.pk).exists():
            # if me in you.followers.all():
                # 언팔로우
                you.followers.remove(me)
            else:
                # 팔로우
                you.followers.add(me)
        return redirect('accounts:profile', you.username)
    return redirect('accounts:login')
```

1. `you = get_object_or_404(get_user_model(), pk=user_pk)` : 현재 팔로우 클릭을 당한(?) 팔로우의 주인(?) 유저에 대한 데이터가 `you` 객체에 담김
2. `me = request.user` : 팔로우 요청을 시도한 유저의 데이터를 담음
3. `if me != you:` : 팔로우 요청을 시도한 유저(me) 가 팔로우의 주인(?) 유저 (you)와 다른지 확인, 자기자신을 팔로우 하지 못하게 하기 위해서

### 4. profile 페이지에 팔로우와 언팔로우 버튼 추가

> 1. 팔로잉 수 / 팔로워 출력
> 2. 자기 자신을 팔로우 할 수 없음

```html
<--! accounts/profile.html -->
{% extends 'base.html' %}

{% block content %}
<h1>{{ person.username }}님의 프로필</h1>

{% with followers=person.followers.all followings=person.followings.all %}
  <div>
    팔로워 : {{ followers|length }} / 팔로우 : {{ followings|length }}
  </div>

  <div>
    {% if user != person %}
      <form action="{% url 'accounts:follow' person.pk %}" method="POST">
        {% csrf_token %}
        {% if user in followers %}
          <input type="submit" value="언팔로우">
        {% else %}
          <input type="submit" value="팔로우">
        {% endif %}
      </form>
    {% endif %}
  </div>
{% endwith %}

<hr>
...
```



















