# π± Django - μ’μμ, νλ‘μ° κΈ°λ₯ κ°λ°

## LIKE κΈ°λ₯ κ΅¬ν

### 1. ManyToManyField μμ± ν λ§μ΄κ·Έλ μ΄μ

```python
# articles/models.py

class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL)
```

```bash
$ python manage.py makemigrations
```

> μλ¬ λ°μ μμΈ
>
> - like_users νλ μμ± μ μλμΌλ‘ μ­μ°Έμ‘°λ `.article_set` λ§€λμ λ₯Ό μμ±
> - κ·Έλ¬λ μ΄μ  1:N(User:Article) κ΄κ³μμ μ΄λ―Έ ν΄λΉ λ§€λμ  μ΄λ¦μ μ¬μ© μ€μ΄κΈ° λλ¬Έ
> - Userμ κ΄κ³λ ForeignKey λλ ManyToManyField μ€ νλμ related_name μΆκ° νμ
>
> ![image-20220419002750992](Django%20-%20%EC%A2%8B%EC%95%84%EC%9A%94,%20%ED%8C%94%EB%A1%9C%EC%9A%B0%20%EA%B8%B0%EB%8A%A5%20%EA%B0%9C%EB%B0%9C.assets/image-20220419002750992.png)

### 2. related_name μ€μ  ν λ§μ΄κ·Έλ μ΄μ λ€μ μ§ν

```python
class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')

```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

- μμ±λ μ€κ° νμ΄λΈμ νμΈ ν΄λ³΄λ©΄ `articles_article_like_users`λΌλ νμ΄λΈ μ΄λ¦κ³Ό `id`, `article_id`, `user_id` νλλ₯Ό κ°μ§κ³  μλ€!

#### νμ¬ User - Article κ° μ¬μ© κ°λ₯ν DB API

- `article.user` - κ²μκΈμ μμ±ν μ μ  (1:N)
- `article.like_users` - κ²μκΈμ μ’μμ ν μ μ  (M:N)
- `user.article_set` - μ μ κ° μμ±ν κ²μκΈ(μ­μ°Έμ‘°) (1:N)
- `user.like_articles` - μ μ κ° μ’μμ ν κ²μκΈ(μ­μ°Έμ‘°) (M:N)

### 3. url μμ±

```python
# articles/urls.py

urlpatterns = [
    ...,
    path('<int:article_pk>/likes/', views.likes, name='likes'),
]
```

### 4. like view ν¨μ μμ±

```python
# articles/views.py

@require_POST
def likes(request, article_pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=article_pk)

        # μ΄ κ²μκΈμ μ’μμλ₯Ό λλ₯Έ μ μ  λͺ©λ‘μ νμ¬ μμ²­νλ μ μ κ° μλ€λ©΄.. μ’μμ μ·¨μ
        # if request.user in article.like_users.all(): 
        if article.like_users.filter(pk=request.user.pk).exists():
            article.like_users.remove(request.user)
        else:
            article.like_users.add(request.user)
        return redirect('articles:index')
    return redirect('accounts:login')
```

1. `article = get_object_or_404(Article, pk=article_pk)` : article_pkμ ν΄λΉνλ λ°μ΄ν°λ₯Ό article κ°μ²΄μ λ΄μ
2. ` if article.like_users.filter(pk=request.user.pk).exists():` : ν΄λΉ κ²μκΈμ μ’μμλ₯Ό λλ₯Έ μ¬λλ€ μ€ pkκ° μμ²­ν μ¬λμ pkμΈμ§ νμΈ
   - `article.like_users.remove(request.user)` : μ΄λ―Έ μ’μμλ₯Ό λλ₯Έ μ¬λμ΄ λ λ€μ μ’μμλ₯Ό μμ²­νλ©΄ μ’μμ μ·¨μ
   - `article.like_users.add(request.user)` : νμ¬ κ²μκΈμ μ’μμ μΆκ°

#### QuerySet API - 'exists()'

- QuerySetμ κ²°κ³Όκ° ν¬ν¨λμ΄ μμΌλ©΄ Trueλ₯Ό λ°ννκ³  κ·Έλ μ§ μμΌλ©΄ Falseλ₯Ό λ°ν
- νΉν κ·λͺ¨κ° ν° QuerySetμ μ»¨νμ€νΈμμ νΉμ  κ°μ²΄ μ‘΄μ¬ μ¬λΆμ κ΄λ ¨λ κ²μμ μ μ©
- κ³ μ ν νλ(PK)κ° μλ λͺ¨λΈμ΄ QuerySetμ κ΅¬μ±μμΈμ§ μ¬λΆλ₯Ό μ°Ύλ κ°μ₯ ν¨μ¨μ μΈ λ°©λ²

### 5. index νμ΄μ§μ like μΆλ ₯ λΆλΆ μμ±

```html
<--! articles/index.html -->
{% extends 'base.html' %}
	...
    <div>
      <form action="{% url 'articles:likes' article.pk %}" method="POST">  
        {% csrf_token %}
        {% if user in article.like_users.all %}
          <input type="submit" value="μ’μμ μ·¨μ">
        {% else %}
          <input type="submit" value="μ’μμ">
        {% endif %}
      </form>
    </div>
```



## Profile Page

> μμ°μ€λ¬μ΄ follow νλ¦μ μν νλ‘ν νμ΄μ§ μμ±

### 1. url μμ±

```python
# accounts/urls.py

urlpatterns = [
    ...,
    # μ¬κΈ°μ μ£Όμ <username>λ λ¬Έμμ΄, λ§μ½ μ μΌ μμ μμ±νλ©΄ λ°μ ν¨μλ€μ λ¬Έμμ΄ μ·¨κΈν  μ μμ!
    path('<username>/', views.profile, name='profile'),
]
```

### 2. profile view ν¨μ μμ±

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

1. `User = get_user_model()` : νμ¬ νμ±ν λκ³  μλ User λͺ¨λΈ
2. `person = get_object_or_404(User, username=username)` : User λͺ¨λΈ μ€ μμ²­ν μ¬λμ λ°μ΄ν°λ₯Ό person κ°μ²΄μ λ΄μ

### 3. profile νμ΄μ§ μμ±

```html
<--! articles/profile.html -->
{% extends 'base.html' %}

{% block content %}
<h1>{{ person.username }}λμ νλ‘ν</h1>
    
<hr>
    
{% comment %} μ΄ μ¬λμ΄ μμ±ν κ²μκΈ λͺ©λ‘ {% endcomment %}
<h2>{{ person.username }}μ΄ μμ±ν κ²μκΈ</h2>
{% for article in person.article_set.all %}
  <p>{{ article.title }}</p>
{% endfor %}

<hr>

{% comment %} μ΄ μ¬λμ΄ μμ±ν λκΈ λͺ©λ‘ {% endcomment %}
<h2>{{ person.username }}μ΄ μμ±ν λκΈ</h2>
{% for comment in person.comment_set.all %}
  <p>{{ comment.content }}</p>
{% endfor %}

<hr>

{% comment %} μ΄ μ¬λμ΄ μ’μμλ₯Ό λλ₯Έ κ²μκΈ λͺ©λ‘ {% endcomment %}
<h2>{{ person.username }}μ΄ μ’μμλ₯Ό λλ₯Έ κ²μκΈ</h2>
{% for article in person.like_articles.all %}
  <p>{{ article.title }}</p>
{% endfor %}

<a href="{% url 'articles:index' %}">[back]</a>

{% endblock content %}

```

### 4. base νμ΄μ§μ νλ‘ν λ§ν¬ μμ±

```html
<--! base.html -->
<body>
  <div class="container">
    {% if request.user.is_authenticated %}
      <h3>Hello, {{ user }}</h3>
      <a href="{% url 'accounts:profile' request.user.username %}">λ΄ νλ‘ν</a>
	  ...
```

### 5. index νμ΄μ§μ νλ‘ν λ§ν¬ μμ±

```html
<--! articles/index.html -->
     <p>μμ±μ:  <a href="{% url 'accounts:profile' article.user.username %}">{{ article.user }}</a></p>
```



## Follow κΈ°λ₯ κ΅¬ν

### 1. ManyToManyField μμ± ν λ§μ΄κ·Έλ μ΄μ

```python
# accounts/models.py

class User(AbstractUser):
    followings = models.ManyToManyField('self', symmetrical=False, related_name='followers')
```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

### 2. url μμ±

```python
# accounts/urls.py

urlpatterns = [
    ...,
    path('<int:user_pk>/follow/', views.follow, name='follow'),
]
```

### 3. follow view ν¨μ μμ±

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
                # μΈνλ‘μ°
                you.followers.remove(me)
            else:
                # νλ‘μ°
                you.followers.add(me)
        return redirect('accounts:profile', you.username)
    return redirect('accounts:login')
```

1. `you = get_object_or_404(get_user_model(), pk=user_pk)` : νμ¬ νλ‘μ° ν΄λ¦­μ λΉν(?) νλ‘μ°μ μ£ΌμΈ(?) μ μ μ λν λ°μ΄ν°κ° `you` κ°μ²΄μ λ΄κΉ
2. `me = request.user` : νλ‘μ° μμ²­μ μλν μ μ μ λ°μ΄ν°λ₯Ό λ΄μ
3. `if me != you:` : νλ‘μ° μμ²­μ μλν μ μ (me) κ° νλ‘μ°μ μ£ΌμΈ(?) μ μ  (you)μ λ€λ₯Έμ§ νμΈ, μκΈ°μμ μ νλ‘μ° νμ§ λͺ»νκ² νκΈ° μν΄μ

### 4. profile νμ΄μ§μ νλ‘μ°μ μΈνλ‘μ° λ²νΌ μΆκ°

> 1. νλ‘μ μ / νλ‘μ μΆλ ₯
> 2. μκΈ° μμ μ νλ‘μ° ν  μ μμ

```html
<--! accounts/profile.html -->
{% extends 'base.html' %}

{% block content %}
<h1>{{ person.username }}λμ νλ‘ν</h1>

{% with followers=person.followers.all followings=person.followings.all %}
  <div>
    νλ‘μ : {{ followers|length }} / νλ‘μ° : {{ followings|length }}
  </div>

  <div>
    {% if user != person %}
      <form action="{% url 'accounts:follow' person.pk %}" method="POST">
        {% csrf_token %}
        {% if user in followers %}
          <input type="submit" value="μΈνλ‘μ°">
        {% else %}
          <input type="submit" value="νλ‘μ°">
        {% endif %}
      </form>
    {% endif %}
  </div>
{% endwith %}

<hr>
...
```



















