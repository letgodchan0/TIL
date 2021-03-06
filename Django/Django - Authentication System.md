# π± Django - Authentication System

## HTTP

- Hyper Text Transfer Protocol
- HTML λ¬Έμμ κ°μ λ¦¬μμ€(μμ, λ°μ΄ν°)λ€μ κ°μ Έμ¬ μ μλλ‘ ν΄μ£Όλ νλ‘ν μ½(κ·μΉ, κ·μ½)
- μΉμμ μ΄λ£¨μ΄μ§λ λͺ¨λ  λ°μ΄ν° κ΅νμ κΈ°μ΄
- ν΄λΌμ΄μΈνΈ - μλ² νλ‘ν μ½μ΄κΈ°λ ν¨

#### νΉμ§

- λΉμ°κ²°μ§ν₯ (connectionless)
  - μλ²λ μμ²­μ λν μλ΅μ λ³΄λΈ ν μ°κ²°μ λμ
- λ¬΄μν (stateless)
  - μ°κ²°μ λλ μκ° ν΄λΌμ΄μΈνΈμ μλ² κ°μ ν΅μ μ΄ λλλ©° μν μ λ³΄κ° μ μ§λμ§ μμ
  - ν΄λΌμ΄μΈνΈμ μλ²κ° μ£Όκ³  λ°λ λ©μμ§λ€μ μλ‘ μμ ν λλ¦½μ μ
- ν΄λΌμ΄μΈνΈμ μλ²μ μ§μμ μΈ κ΄κ³λ₯Ό μ μ§νκΈ° μν΄ μΏ ν€μ μΈμμ΄ μ‘΄μ¬



## μΏ ν€(Cookie)

- μλ²κ° μ¬μ©μμ μΉ λΈλΌμ°μ μ μ μ‘νλ μμ λ°μ΄ν° μ‘°κ°
- μ¬μ©μκ° μΉ μ¬μ΄νΈλ₯Ό λ°©λ¬Έν  κ²½μ° ν΄λΉ μΉ μ¬μ΄νΈμ μλ²λ₯Ό ν΅ν΄ μ¬μ©μμ μ»΄ν¨ν°μ λ°°μΉ(placed-on)νλ μμ κΈ°λ‘ μ λ³΄ νμΌ
  - λΈλΌμ°μ (ν΄λΌμ΄μΈνΈ)λ μΏ ν€λ₯Ό λ‘μ»¬μ KEY - VALUEμ λ°μ΄ν° νμμΌλ‘ μ μ₯
  - μ΄λ κ² μΏ ν€λ₯Ό μ μ₯ν΄ λμλ€κ°, λμΌν μλ²μ μ¬ μμ²­ μ μ μ₯λ μΏ ν€λ₯Ό ν¨κ» μ μ‘ 

>  [μ°Έκ³ ] μννΈμ¨μ΄κ° μλκΈ° λλ¬Έμ νλ‘κ·Έλ¨μ²λΌ μ€ν λ  μ μμΌλ©° μμ± μ½λλ₯Ό μ€μΉ ν  μ μμ§λ§, μ¬μ©μμ νλμ μΆμ νκ±°λ μΏ ν€λ₯Ό νμ³μ ν΄λΉ μ¬μ©μμ κ³μ  μ κ·Ό κΆνμ νλ ν  μλ μμ

- HTTP μΏ ν€λ μνκ° μλ μΈμμ λ§λ€μ΄ μ€
- μΏ ν€λ λ μμ²­μ΄ λμΌν λΈλΌμ°μ μμ λ€μ΄μλμ§ μλμ§λ₯Ό νλ¨ν  λ μ£Όλ‘ μ¬μ©
  - μ΄λ₯Ό μ΄μ©ν΄ μ¬μ©μμ λ‘κ·ΈμΈ μνλ₯Ό μ μ§ν  μ μμ
  - μνκ° μλ(stateless) HTTP νλ‘ν μ½μμ μν μ λ³΄λ₯Ό κΈ°μ΅ μμΌμ£ΌκΈ° λλ¬Έ

- μΉ νμ΄μ§μ μ μνλ©΄ μμ²­ν μΉ νμ΄μ§λ₯Ό λ°μΌλ©° μΏ ν€λ₯Ό μ μ₯νκ³ , ν΄λΌμ΄μΈνΈκ° κ°μ μλ²μ μ¬ μμ²­ μ μμ²­κ³Ό ν¨κ» μΏ ν€λ ν¨κ» μ μ‘

![image-20220411211920958](Django%20-%20Authentication%20System.assets/image-20220411211920958.png)



### μΏ ν€μ μ¬μ© λͺ©μ 

1. μΈμ κ΄λ¦¬ (Session management)
   - λ‘κ·ΈμΈ, μμ΄λ μλ μμ±, κ³΅μ§ νλ£¨ μλ³΄κΈ°, νμ μ²΄ν¬, μ₯λ°κ΅¬λ λ±μ μ λ³΄ κ΄λ¦¬
2. κ°μΈν (Personalization)
   - μ¬μ©μ μ νΈ, νλ§ λ±μ μ€μ 
3. νΈλνΉ (Tracking)
   - μ¬μ©μ νλμ κΈ°λ‘ λ° λΆμ



## μΈμ (Session)

- μ¬μ΄νΈμ νΉμ  λΈλΌμ°μ  μ¬μ΄μ "μν(state)"λ₯Ό μ μ§μν€λ κ²
- ν΄λΌμ΄μΈνΈκ° μλ²μ μ μνλ©΄ μλ²κ° νΉμ  `session id`λ₯Ό λ°κΈνκ³ , ν΄λΌμ΄μΈνΈλ λ°κΈ λ°μ `session id`λ₯Ό μΏ ν€μ μ μ₯
  - ν΄λΌμ΄μΈνΈκ° λ€μ μλ²μ μ μνλ©΄ μμ²­κ³Ό ν¨κ» μΏ ν€(`session id`κ° μ μ₯λ¨)λ₯Ό μλ²μ μ λ¬
  - μΏ ν€λ μμ²­ λλ§λ€ μλ²μ ν¨κ» μ μ‘λλ―λ‘ μλ²μμ `session id`λ₯Ό νμΈν΄ μλ§μ λ‘μ§μ μ²λ¦¬

- IDλ μΈμμ κ΅¬λ³νκΈ° μν΄ νμνλ©°, μΏ ν€μλ IDλ§ μ μ₯ν¨



### μΏ ν€ lifetime (μλͺ)

- μΏ ν€μ μλͺμ λ κ°μ§ λ°©λ²μΌλ‘ μ μν  μ μμ

1. Session cookies
   - νμ¬ μΈμμ΄ μ’λ£λλ©΄ μ­μ λ¨
   - λΈλΌμ°μ κ° "νμ¬ μΈμ(current session)"μ΄ μ’λ£λλ μκΈ°λ₯Ό μ μ
   - [μ°Έκ³ ] μΌλΆ λΈλΌμ°μ λ λ€μ μμν  λ μΈμ λ³΅μ(session restoring)μ μ¬μ©ν΄ μΈμ μΏ ν€κ° μ€λ μ§μ λ  μ μλλ‘ ν¨
2. Persistent cookies (or Permanent cookies)
   - Expires μμ±μ μ§μ λ λ μ§ νΉμ Max-Age μμ±μ μ§μ λ κΈ°κ°μ΄ μ§λλ©΄ μ­μ 

## Session in Django

- Djangoμ μΈμμ λ―Έλ€μ¨μ΄λ₯Ό ν΅ν΄ κ΅¬νλ¨
- Djangoλ database-backed sessions μ μ₯ λ°©μμ κΈ°λ³Έ κ°μΌλ‘ μ¬μ©
  - [μ°Έκ³ ] μ€μ μ ν΅ν΄ cached, file-base, cookie-based λ°©μμΌλ‘ λ³κ²½ κ°λ₯
- Djangoλ νΉμ  `session id`λ₯Ό ν¬ν¨νλ μΏ ν€λ₯Ό μ¬μ©ν΄μ κ°κ°μ λΈλΌμ°μ μ μ¬μ΄νΈκ° μ°κ²°λ μΈμμ μμλ
  - μΈμ μ λ³΄λ Django DBμ `django_session` νμ΄λΈμ μ μ₯λ¨
- λͺ¨λ  κ²μ μΈμμΌλ‘ μ¬μ©νλ €κ³  νλ©΄ μ¬μ©μκ° λ§μ λ μλ²μ λΆνκ° κ±Έλ¦΄ μ μμ 

### Authentication System in MIDDLEWARE

```python
# settings.py

MIDDLEWARE = [
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    ...
]
```

- SessionMiddleware
  - μμ²­ μ λ°μ κ±Έμ³ μΈμμ κ΄λ¦¬
- AuthenticationMiddleware
  - μΈμμ μ¬μ©νμ¬ μ¬μ©μλ₯Ό μμ²­κ³Ό μ°κ²°



#### [μ°Έκ³ ] MIDDLEWARE (λ―Έλ€μ¨μ΄)

- HTTP μμ²­κ³Ό μλ΅ μ²λ¦¬ μ€κ°μμ μλνλ μμ€ν(hooks)
- Djangoλ HTTP μμ²­μ΄ λ€μ΄μ€λ©΄ λ―Έλ€μ¨μ΄λ₯Ό κ±°μ³ ν΄λΉ URLμ λ±λ‘λμ΄ μλ viewλ‘ μ°κ²°ν΄μ£Όκ³ , HTTP μλ΅ μ­μ λ―Έλ€μ¨μ΄λ₯Ό κ±°μ³μ λ΄λ³΄λ
- μ£Όλ‘ λ°μ΄ν° κ΄λ¦¬, μ νλ¦¬μΌμ΄μ μλΉμ€, λ©μμ§, μΈμ¦ λ° API κ΄λ¦¬λ₯Ό λ΄λΉ





## The Django Authentication System 

- Django μΈμ¦ μμ€νμ `django.contrib.auth`μ `Django contrib module`λ‘ μ κ³΅
- νμ κ΅¬μ±μ settings.pyμ μ΄λ―Έ ν¬ν¨λμ΄ μμΌλ©° INSTALLED_APPS μ€μ μ λμ΄λ μλ λ ν­λͺ©μΌλ‘ κ΅¬μ± λ¨

1. django.contrib.auth
   - μΈμ¦ νλ μμν¬μ ν΅μ¬κ³Ό κΈ°λ³Έ λͺ¨λΈμ ν¬ν¨
2. django.contrib.contenttypes
   - μ¬μ©μκ° μμ±ν λͺ¨λΈκ³Ό κΆνμ μ°κ²°ν  μ μμ

> Django μΈμ¦ μμ€νμ μΈμ¦(Authentication)κ³Ό κΆν(Authorization) λΆμ¬λ₯Ό ν¨κ» μ κ³΅(μ²λ¦¬)νλ©°, μ΄λ¬ν κΈ°λ₯μ΄ μ΄λ μ λ κ²°ν©λμ΄ μΌλ°μ μΌλ‘ μΈμ¦ μμ€νμ΄λΌκ³  ν¨

- Authentication (μΈμ¦)
  - μ μ νμΈ
  - μ¬μ©μκ° μμ μ΄ λκ΅¬μΈμ§ νμΈνλ κ²
- Authorization (κΆν, νκ°)
  - κΆν λΆμ¬
  - μΈμ¦λ μ¬μ©μκ° μνν  μ μλ μμμ κ²°μ 



## λ‘κ·ΈμΈ

#### μ± μμ±, λ±λ‘ λ° url μ€μ 

```bash
$ python manage.py startapp accounts
```

```python
# settings.py

INSTALLED_APPS = [
    'accounts',
    ...
]
```

```python
# crud/urls.py

urlpatterns = [
    ...,
    path('accounts/', include('accounts.urls')),
]
```

```python
# accounts/urls.py

app_name = 'accounts'
urlpatterns = []
```

- app μ΄λ¦μ΄ λ°λμ accountsμΌ νμλ μμ
- λ¨, authμ κ΄λ ¨ν΄ Django λ΄λΆμ μΌλ‘ accountsλΌλ μ΄λ¦μΌλ‘ μ¬μ©λκ³  μκΈ° λλ¬Έμ λλλ‘ accountsλ‘ μ§μ νλ κ²μ κΆμ₯

#### λ‘κ·ΈμΈ url, view, templates

- λ‘κ·ΈμΈμ sessionμ Create νλ λ‘μ§κ³Ό κ°μ
- Djangoλ μ°λ¦¬κ° sessionμ λ©μ»€λμ¦μ κ΄ν΄ μκ°νμ§ μκ²λ μΈμ¦μ κ΄ν built-in formsλ₯Ό μ κ³΅νλ€.

```python
# accounts/urls.py
from django.urls import path
from . import views

app_name = 'accounts'
urlpatterns = [
    path('login/', views.login, name='login'),
]
```

```python
# accounts/views.py
from django.shortcuts import render, redirect
# login ν¨μ μ΄λ¦ λ°κΏμ νΈμΆ, μλ login ν¨μμ νΌλ λ°©μ§λ₯Ό μν¨
from django.contrib.auth import login as auth_login
from django.contrib.auth.forms import AuthenticationForm
from django.views.decorators.http import require_http_methods


@require_http_methods(['GET', 'POST'])
def login(request):
    if request.method == 'POST':
        # modelformμ΄ μλ formμ μμλ°κΈ° λλ¬Έμ requestκ° μ²«λ²μ§Έ μΈμ!
        form = AuthenticationForm(request, request.POST)
        if form.is_valid():
            # λ‘κ·ΈμΈ
            auth_login(request, form.get_user())
            return redirect(request.GET.get('next') or 'articles:index')
    else:
        form = AuthenticationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/login.html', context)

```

- `AuthenticationForm`
  - μ¬μ©μ λ‘κ·ΈμΈμ μν form
  - `request`λ₯Ό μ²«λ²μ§Έ μΈμλ‘ μ·¨ν¨
  - AuthenticationFormμ ν΅κ³Όν μ¬λμ΄ μΈμ¦ λ μ¬μ©μμ AuthenticationForm μλ dbμ μνΈνλ λΉλ°λ²νΈ ν¨ν΄μ΄ λ§λμ§λ νμΈν΄μ€
- loginν¨μ
  - login(request, user, backend=None)
  - νμ¬ μΈμμ μ°κ²°νλ €λ μΈμ¦ λ μ¬μ©μκ° μλ κ²½μ° login() ν¨μκ° νμ
  - μ¬μ©μλ₯Ό λ‘κ·ΈμΈνλ©° view ν¨μμμ μ¬μ©λ¨
  - HttpRequest κ°μ²΄μ User κ°μ²΄κ° νμ
  - Djangoμ session frameworkλ₯Ό μ¬μ©νμ¬ μΈμμ userμ IDλ₯Ό μ μ₯(== λ‘κ·ΈμΈ)

```html
<!-- accounts/login.html -->

{% extends 'base.html' %}

{% block content %}
  <h1>λ‘κ·ΈμΈ</h1>
  <hr>
  <form action="{% url 'accounts:login' %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
  <a href="{% url 'articles:index' %}">back</a>
{% endblock content %}
```

> μ¬κΈ°κΉμ§ μ§ν ν λ‘κ·ΈμΈμ νλ©΄ λΈλΌμ°μ μ Django DBμμ Djangoλ‘λΆν° λ°κΈ λ°μ session idλ₯Ό νμΈν  μ μλ€.

#### get_user()

- `AuthenticationForm`μ μΈμ€ν΄μ€ λ©μλ
- user_cacheλ μΈμ€ν΄μ€ μμ± μμ NoneμΌλ‘ ν λΉλλ©°, μ ν¨μ± κ²μ¬λ₯Ό ν΅κ³Όνμ κ²½μ° λ‘κ·ΈμΈ ν μ¬μ©μ κ°μ²΄λ‘ ν λΉ λ¨
- μΈμ€ν΄μ€ μ ν¨μ±μ λ¨Όμ  νμΈνκ³ , μΈμ€ν΄μ€κ° μ ν¨ν  λλ§ userλ₯Ό μ κ³΅νλ €λ κ΅¬μ‘° 

```python
# geu_user ν¨μ κ΅¬μ‘°

class AuthenticationForm(forms.Form):
    ...
    def get_user(self):
        return self.user_cache
```



## Authentication data in templates

- μ₯κ³ μμλ κ·Έλ₯ `user`λ₯Ό ν΅ν΄ νμ¬ λ‘κ·ΈμΈ λμ΄ μλ μ μ  μ λ³΄λ₯Ό μΆλ ₯ν  μ μλ€.

```html
<!-- base.html -->

<h3>Hello, {{ user }}</h3>
<form action="{% url 'accounts:logout' %}" method="POST">
```

![image-20220416003956773](Django%20-%20Authentication%20System.assets/image-20220416003956773.png)



### Context processors

- ννλ¦Ώμ΄ λ λλ§ λ  λ μλμΌλ‘ νΈμΆ κ°λ₯ν μ»¨νμ€νΈ λ°μ΄ν° λͺ©λ‘
- μμ±λ νλ‘μΈμλ `RequestContext`μμ μ¬μ© κ°λ₯ν λ³μλ‘ ν¬ν¨λ¨

> settings.pyμμ context_processorsμ contribμμ κΈ°λ³Έμ μΌλ‘ contextμ λ£μ΄μ£Όλ μ λ€μ΄ μμ request, λ auth κ°μ μ λ€! λμ€μ λ΄κ° customize ν  μ μμ λͺ¨λ  νμ΄μ§μμ λ£κ³  μΆμ κ°μ΄ μλ€λ©΄ μ κΈ°μ μΆκ°νκ³ , κ·Έλ¬λ©΄ viewμμ context λκΈΈ λ μμμ λμ΄κ°

```python
# settings.py

TEMPLATES = [
    {
			...
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

### Users

- ννλ¦Ώ `RequestContext`λ₯Ό λ λλ§ ν  λ, νμ¬ λ‘κ·ΈμΈ ν μ¬μ©μλ₯Ό λνλ΄λ `auth.User` μΈμ€ν΄μ€ (λλ ν΄λΌμ΄μΈνΈκ° λ‘κ·ΈμΈνμ§ μμ κ²½μ° AnonymousUser μΈμ€ν΄μ€)λ ννλ¦Ώ λ³μ  `{{ user }}`μ μ μ₯λ¨

```python
# settings.py

'django.contrib.auth.context_processors.auth',
```

## λ‘κ·Έμμ

> λ‘κ·Έμμμ sessionμ Deleteνλ λ‘μ§κ³Ό κ°μ

- logou ν¨μ
  - logout(request)
  - `HttpRequest` κ°μ²΄λ₯Ό μΈμλ‘ λ°κ³  λ°ν κ°μ΄ μμ
  - μ¬μ©μκ° λ‘κ·ΈμΈνμ§ μμ κ²½μ° μ€λ₯λ₯Ό λ°μμν€μ§ μμ
  - νμ¬ μμ²­μ λν `session data`λ₯Ό DBμμ μμ ν μ­μ νκ³ , ν΄λΌμ΄μΈνΈμ μΏ ν€μμλ `sessionid`κ° μ­μ λ¨
  - μ΄λ λ€λ₯Έ μ¬λμ΄ λμΌν μΉ λΈλΌμ°μ λ₯Ό μ¬μ©νμ¬ λ‘κ·ΈμΈνκ³ , μ΄μ  μ¬μ©μμ μΈμ λ°μ΄ν°μ μμΈμ€νλ κ²μ λ°©μ§νκΈ° μν¨

```python
# accounts/urls.py

path('logout/', views.logout, name='logout'),
```

```python
# accounts/views.py
from django.views.decorators.http import require_http_methods, require_POST
from django.contrib.auth import logout as auth_logout

@require_POST
def logout(request):
    auth_logout(request)
    return redirect('articles:index')
```

```html
<!-- base.html -->

<form action="{% url 'accounts:logout' %}" method="POST">
    {% csrf_token %}
    <input type="submit" value="Logout">
</form>
```

## λ‘κ·ΈμΈ μ¬μ©μμ λν μ κ·Ό μ ν

### `is_authenticated` attribute

- User model μμ±(attributes) μ€ νλ
- λͺ¨λ  User μΈμ€ν΄μ€μ λν΄ ν­μ TrueμΈ μ½κΈ° μ μ© μμ± (AnonymousUserμ λν΄μλ ν­μ False)
- μ¬μ©μκ° μΈμ¦ λμλμ§ μ¬λΆλ₯Ό μ μ μλ λ°©λ²
- μΌλ°μ μΌλ‘ `request.user`μμ μ΄ μμ±μ μ¬μ©νμ¬, λ―Έλ€μ¨μ΄μ `django.contrib.auth.middleware.AuthenticationMiddleware`λ₯Ό ν΅κ³Ό νλμ§ νμΈ
- λ¨, κΆν(permission)κ³Όλ κ΄λ ¨μ΄ μμΌλ©°, μ¬μ©μκ° νμ±ν μν(active)μ΄κ±°λ μ ν¨ν μΈμ(valid session)μ κ°μ§κ³  μλμ§λ νμΈνμ§ μμ

##### is_authenticated μ μ©

1. λ‘κ·ΈμΈκ³Ό λΉλ‘κ·ΈμΈ μνμμ μΆλ ₯λλ λ§ν¬λ₯Ό λ€λ₯΄κ² μ€μ 

```html
<!-- base.html -->

{% if request.user.is_authenticated %}
    <h3>Hello, {{ user }}</h3>
    <form action="{% url 'accounts:logout' %}" method="POST">
        {% csrf_token %}
        <input type="submit" value="Logout">
    </form>
    <a href="{% url 'accounts:update' %}">νμμ λ³΄μμ </a>
    <form action="{% url 'accounts:delete' %}" method="POST">
        {% csrf_token %}
        <input type="submit" value="νμνν΄">
    </form>
{% else %}
    <a href="{% url 'accounts:login' %}">Login</a>
    <a href="{% url 'accounts:signup' %}">Signup</a>
{% endif %}
```

2. μΈμ¦λ μ¬μ©μ(λ‘κ·ΈμΈ μν)λΌλ©΄ λ‘κ·ΈμΈ λ‘μ§μ μνν  μ μλλ‘ μ²λ¦¬

```python
# accounts/views.py

@require_http_methods(['GET', 'POST'])
def login(request):
    if request.user.is_authenticated:
        return redirect('articles:index')
```

3. μΈμ¦λ μ¬μ©μ(λ‘κ·ΈμΈ μν)λΌλ©΄ λ‘κ·Έμμ λ‘μ§μ μνν  μ μλλ‘ μ²λ¦¬

```python
# accounts/views.py

@require_POST
def logout(request):
    if request.user.is_authenticated:
        auth_logout(request)
    return redirect('articles:index')
```

4. μΈμ¦λ μ¬μ©μ(λ‘κ·ΈμΈ μν)λ§ κ²μκΈ μμ± λ§ν¬λ₯Ό λ³Ό μ μλλ‘ μ²λ¦¬

```html
<!-- articles/index.html -->

<h1>Articles</h1>
{% if request.user.is_authenticated %}
	<a href="{% url 'articles:create' %}">CREATE</a>
{% else %}
	<a href="{% url 'accounts:login' %}">[μ κΈμ μμ±νλ €λ©΄ λ‘κ·ΈμΈ νμΈμ]</a>
{% endif %}
```

### `login_required` decorator

- μ¬μ©μκ° λ‘κ·ΈμΈλμ΄ μμ§ μμΌλ©΄, `settings.LOGIN_URL`μ μ€μ λ λ¬Έμμ΄ κΈ°λ° μ λ κ²½λ‘λ‘ `redirect` ν¨
  - LOGIN_URLμ κΈ°λ³Έ κ°μ '/accounts/login/'
  - λλ²μ§Έ app μ΄λ¦μ accountsλ‘ νλ μ΄μ  μ€ νλ
- μ¬μ©μκ° λ‘κ·ΈμΈλμ΄ μμΌλ©΄ μ μμ μΌλ‘ view ν¨μλ₯Ό μ€ν
- μΈμ¦ μ±κ³΅ μ μ¬μ©μκ° `redirect` λμ΄μΌ νλ κ²½λ‘λ "next" λΌλ μΏΌλ¦¬ λ¬Έμμ΄ λ§€κ° λ³μμ μ μ₯λ
  - `/accounts/login/?next=/articles/create/`

##### login_required μ μ©

```python
# accounts/views.py
from django.contrib.auth.decorators import login_required

@login_required
def update(request):
    ...
```



### "next" query string parameter

- λ‘κ·ΈμΈμ΄ μ μμ μΌλ‘ μ§νλλ©΄ κΈ°μ‘΄μ μμ²­νλ μ£Όμλ‘ `redirect` νκΈ° μν΄ λ§μΉ μ£Όμλ₯Ό keep ν΄μ£Όλ κ²
- λ¨, λ³λλ‘ μ²λ¦¬ ν΄μ£Όμ§ μμΌλ©΄ μ°λ¦¬κ° viewμ μ€μ ν redirect κ²½λ‘λ‘ μ΄λνκ² λ¨

```python
# accounts/views.py

@require_http_methods(['GET', 'POST'])
def login(request):
    if request.user.is_authenticated:
        return redirect('articles:index')

    if request.method == 'POST':
        form = AuthenticationForm(request, request.POST)
        if form.is_valid():
            # λ‘κ·ΈμΈ
            auth_login(request, form.get_user())
            return redirect(request.GET.get('next') or 'articles:index')
    else:
        form = AuthenticationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/login.html', context)
```

- νμ¬ URL(next parameterκ° μλ) μμ²­μ λ³΄λ΄κΈ° μν΄ action κ° λΉμ°κΈ°

```html
<!-- accounts/login.html -->

{% extends 'base.html' %}

{% block content %}
  <h1>λ‘κ·ΈμΈ</h1>
  <hr>
  <form action="" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
  <a href="{% url 'articles:index' %}">back</a>
{% endblock content %}
```



> "λ λ°μ½λ μ΄ν°λ‘ μΈν΄ λ°μνλ κ΅¬μ‘°μ  λ¬Έμ μ ν΄κ²°"

- λΉλ‘κ·ΈμΈ μνμμ κ²μκΈ μ­μ  μλ

```python
# articles/views.py

@login_required
@require_POST
def delete(request):
    article = get_object_or_404(Article, pk=pk)
    article.delete()
    return redirect('articles:index')
```

> 1. redirectλ‘ μ΄λν λ‘κ·ΈμΈ νμ΄μ§μμ λ‘κ·ΈμΈ μλ
> 2. 405(Method Not Allowed) status code νμΈ

`@require_POST` κ° μμ±λ ν¨μμ `@login_required`λ₯Ό ν¨κ» μ¬μ©νλ κ²½μ° μλ¬κ° λ°μνλ€. μ΄μ λ λ‘κ·ΈμΈ μ΄ν "next" λ§€κ°λ³μλ₯Ό λ°λΌ ν΄λΉ ν¨μλ‘ λ€μ `redirect` λκ³ , `redirect`λ `GET`λ°©μμΌλ‘ μμ²­μ νκΈ° λλ¬Έμ @require_POST` λ°μ½λ μ΄ν°λ₯Ό μ¬μ©νλ€λ©΄ 405 μλ¬κ° λ°μνκ² λλ€.

- λ κ°μ§ λ¬Έμ 
  1. redirect κ³Όμ μμ POST λ°μ΄ν°μ μμ€
  2. redirect μμ²­μ POST λ°©μμ΄ λΆκ°λ₯νκΈ° λλ¬Έμ GET λ°©μμΌλ‘ μμ²­λ¨

![image-20220416010642362](Django%20-%20Authentication%20System.assets/image-20220416010642362.png)

- `login_required`λ GET method requestλ₯Ό μ²λ¦¬ν  μ μλ view ν¨μμμλ§ μ¬μ©ν΄μΌ ν¨

```python
# articles/views.py

@require_POST
def delete(request):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=pk)
        article.delete()
    return redirect('articles:index')
```

## νμκ°μ

### UserCreationForm

- μ£Όμ΄μ§ `username`κ³Ό `password`λ‘ κΆνμ΄ μλ μ userλ₯Ό μμ±νλ ModelForm
- 3κ°μ νλλ₯Ό κ°μ§
  1. username (from the user model)
  2. password1
  3. password2

```python
# accounts/urls.py

app_name = "accounts"

urlpatterns = [
    path('signup/', views.signup, name='signup'),
]
```

```python
# accounts/views.py
from django.contrib.auth.forms import AuthenticationForm, UserCreationForm

def signup(request):
	# νμ κ°μμ μλ£ λ²νΌμ λλ₯΄λ©΄ `POST`λ‘ μμ²­μ΄ κ°
    if request.method == 'POST': # POST νμΈ
        # λ΄κ° μμ±ν λ΄μ©μ UserCreationFormμ μ΄μ©ν΄μ formμ λ΄μ
        form = UserCreationForm(request.POST)
        # μ ν¨μ± κ²μ¬
        if form.is_valid():
            # λ΄κ° μμ±ν νμκ°μ λ΄μ© DBμ μ μ₯
            form.save()
            return redirect('articles:index')
    # νμκ°μ νλ¬ μ²μ λ€μ΄μ΄
    else:
        # μ₯κ³ κ° μ κ³΅νλ κΈ°λ³Έ νμκ°μ νΌμ `form`μ λ΄μ
        form = UserCreationForm()
    # contextλ₯Ό μ΄μ©ν΄μ formμ νλ©΄μΌλ‘ λ³΄λΌκΊΌμ
    context = {
        'form': form,
    }
    # else κ΅¬λ¬Έμμ μ₯κ³ κ° μ κ³΅νλ κΈ°λ³Έ νμκ°μ formμ λ΄μμ€¬κΈ° λλ¬Έμ νλ©΄μ νμκ°μ μ°½μ΄ μκΉ
    return render(request, 'accounts/signup.html', context)
```

```html
<--! accounts/singup.html -->

{% extends 'base.html' %}

{% block body %}
  <h1>νμκ°μ</h1>
  <hr>
  <form action="{% url 'accounts:signup' %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
  <a href="{% url 'articles:index' %}">back</a>
{% endblock body %}
```

### νμκ°μ ν μλμΌλ‘ λ‘κ·ΈμΈ μ§ννκΈ°

```python
# accounts/views.py

@require_http_methods(['GET', 'POST'])
def signup(request):
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            user = form.save()
            auth_login(request, user)
            return redirect('articles:index')
    else:
        form = UserCreationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/signup.html', context)
```

```python
<--! base.html -->

<a href="{% url 'accounts:signup' %}">Signup</a>
```



## νμνν΄

- νμνν΄λ DBμμ μ¬μ©μλ₯Ό μ­μ νλ κ²κ³Ό κ°μ
- `auth_logout(request)`λ₯Ό λ¨Όμ νλ©΄, λ‘κ·Έμμμ΄ λμ΄ λ²λ €μ νμμ λ³΄λ₯Ό κ°μ Έμ¬ μ μκ³  μ­μ λ₯Ό λͺ»νκΈ° λλ¬Έμ, νμνν΄λ λλΉμμ λ¨Όμ  μ κ±° ν μΈμμμ λ‘κ·Έμμ ν΄μΌνλ€.

```python
# accounts/urls.py

app_name = "accounts"

urlpatterns = [
    path('delete/', views.delete, name='delete'),
]
```

```python
# accounts/views.py
from django.views.decorators.http import require_POST

@require_POST
def delete(request):
    if request.user.is_authenticated:
        # λ°λμ νμνν΄ ν λ‘κ·Έμμ ν¨μ νΈμΆ
        request.user.delete()
        # νν΄ νλ©΄μ ν΄λΉ μ μ μ μΈμ λ°μ΄ν°λ ν¨κ» μ§μΈ κ²½μ°
        # λ¨, λ°λμ νν΄ ν λ‘κ·Έμμ μμΌλ‘ μ²λ¦¬ν΄μΌ ν¨
        auth_logout(request)
    return redirect('articles:index')
```

```html
<--! base.html -->

<form action="{% url 'accounts:delete' %}" method="POST">
    {% csrf_token %}
    <input type="submit" value="νμνν΄">
</form>
```



## νμμ λ³΄ μμ 

### UserChangeForm

> μ¬μ©μμ μ λ³΄ λ° κΆνμ λ³κ²½νκΈ° μν΄ admin μΈν°νμ΄μ€μμ μ¬μ©λλ ModelForm

```python
# accounts/urls.py

app_name = "accounts"

urlpatterns = [
    path('update/', views.update, name='update'),
]
```

```python
# accounts/views.py
from django.contrib.auth.forms import AuthenticationForm, UserCreationForm, 

@require_http_methods(['GET', 'POST'])
def update(request):
    if request.method == 'POST':
        pass
    else:
        form = CustomUserChangeForm(instance=request.user)
    context = {
        'form': form,
    }
    return render(request, 'accounts/update.html', context)
```

```html
<--! accounts/update.html -->
    
{% extends 'base.html' %}

{% block content %}
  <h1>νμμ λ³΄μμ </h1>
  <hr>
  <form action="{% url 'accounts:update' %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
  <a href="{% url 'articles:index' %}">back</a>
{% endblock content %}
```

- νμμ λ³΄ μμ  νμ΄μ§ λ§ν¬ μμ±

```html
<--! base.html -->
    
<a href="{% url 'accounts:update' %}">νμμ λ³΄μμ </a>
```

![image-20220416012353023](Django%20-%20Authentication%20System.assets/image-20220416012353023.png)

### UserChangeForm μ¬μ© μ λ¬Έμ μ 

- μΌλ° μ¬μ©μκ° μ κ·Όν΄μλ μλ  μ λ³΄λ€(fields)κΉμ§ λͺ¨λ μμ μ΄ κ°λ₯ν΄μ§
- λ°λΌμ UserChangeFormμ μμλ°μ CustomUserChangeFormμ΄λΌλ μλΈ ν΄λμ€λ₯Ό μμ±ν΄ μ κ·Ό κ°λ₯ν νλλ₯Ό μ‘°μ ν΄μΌ ν¨

1. get_user_model()
   - νμ¬ νλ‘μ νΈμμ νμ±ν λ μ¬μ©μ λͺ¨λΈμ λ°ν
   - Djangoλ User ν΄λμ€λ₯Ό μ§μ  μ°Έμ‘°νλ λμ  `django.contrib.auth.get_user_model()`μ μ¬μ©νμ¬ μ°Έμ‘°ν΄μΌ νλ€κ³  κ°μ‘°
2. User λͺ¨λΈμ fields

```python
# accounts/forms.py

from django.contrib.auth.forms import UserChangeForm
from django.contrib.auth import get_user_model

class CustomUserChangeForm(UserChangeForm):

    # password = None

    class Meta:
        model = get_user_model() # User
        # μμ  μ νμν νλλ§ μ ννμ¬ μμ±
        fields = ('email', 'first_name', 'last_name',)
```

> ```
> from .forms import CustomUserChangeForm
> ```
>
> κΈ°μ‘΄μ UserChangeFormμ CustomUserChangeFormμΌλ‘ λͺ¨λ λ³κ²½νλ©΄ νμ μ λ³΄ μμ  νμ΄μ§μμ μ°λ¦¬κ° μ»€μ€νν νλλ€λ§ μΆλ ₯λλ€.

- νμμ λ³΄ μμ  νμ΄μ§μμ λΉλ°λ²νΈ κ΄λ ¨ λ΄μ© μμ κ³  μΆμΌλ©΄, μλμ κ°μ΄ νλ©΄ λλ€. UserChangeFormμ μμμΌλ‘ λ°λλ° μ΄ ν΄λμ€μ λ΄μ©μ λ³΄λ©΄ passwordκ° κΈ°λ³Έμ μΌλ‘ νλ©΄μ λ³΄μ΄λ κ²μ²λΌ μμ±λμ΄ μκΈ° λλ¬Έ

```python
class CustomUserChangeForm(UserChangeForm):
    
    password = None
```

## λΉλ°λ²νΈ λ³κ²½

### PasswordChangeForm

- μ¬μ©μκ° λΉλ°λ²νΈλ₯Ό λ³κ²½ν  μ μλλ‘ νλ Form
- μ΄μ  λΉλ°λ²νΈλ₯Ό μλ ₯νμ¬ λΉλ°λ²νΈλ₯Ό λ³κ²½ν  μ μλλ‘ ν¨
- μ΄μ  λΉλ°λ²νΈλ₯Ό μλ ₯νμ§ μκ³  λΉλ°λ²νΈλ₯Ό μ€μ ν  μ μλ `SetPasswordForm`μ μμλ°λ μλΈ ν΄λμ€

> νμμ λ³΄ μμ  νμ΄μ§μ μμ±λ λΉλ°λ²νΈ λ³κ²½ form μ£Όμ νμΈ

![image-20220416013032296](Django%20-%20Authentication%20System.assets/image-20220416013032296.png)

```python
# accounts/urls.py

app_name = "accounts"

urlpatterns = [
    path('password/', views.password, name='change_password'),
]
```

```python
# accounts/views.py
from django.contrib.auth.forms import AuthenticationForm, UserCreationForm, PasswordChangeForm,

@login_required
@require_http_methods(['GET', 'POST'])
def change_password(request):
    if request.method == 'POST':
        # μ£Όλͺ© μ²«λ²μ§Έ μΈμκ° user! SetPasswordFormμ μμλ°μμ!
        form = PasswordChangeForm(request.user, request.POST)
        if form.is_valid():
            # user = form.save()
            # update_session_auth_hash(request, user), μλ μ€λͺ!!
            form.save()
            update_session_auth_hash(request, form.user)
            return redirect('articles:index')
    else:
        form = PasswordChangeForm(request.user)
    context = {
        'form': form,
    }
    return render(request, 'accounts/change_password.html', context)
```

```html
<--! accounts/change_password.html -->
    
{% extends 'base.html' %}

{% block content %}
  <h1>λΉλ°λ²νΈλ³κ²½</h1>
  <hr>
  <form action="{% url 'accounts:change_password' %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
  <a href="{% url 'articles:index' %}">back</a>
{% endblock content %}
```

### μνΈ λ³κ²½ μ μΈμ λ¬΄ν¨ν λ°©μ§

- `update_session_auth_hash(request, user)`
  - νμ¬ μμ²­(current request)κ³Ό μ session hashκ° νμ λ  μλ°μ΄νΈ λ μ¬μ©μ κ°μ²΄λ₯Ό κ°μ Έμ€κ³ , session hashλ₯Ό μ μ νκ² μλ°μ΄νΈ
  - λΉλ°λ²νΈκ° λ³κ²½λλ©΄ κΈ°μ‘΄ μΈμκ³Όμ νμ μΈμ¦ μ λ³΄κ° μΌμΉνμ§ μκ² λμ΄ λ‘κ·ΈμΈ μνλ₯Ό μ μ§ν  μ μκΈ° λλ¬Έ
  - μνΈκ° λ³κ²½λμ΄λ λ‘κ·Έμμλμ§ μλλ‘ μλ‘μ΄ password hashλ‘ sessionμ μλ°μ΄νΈ ν¨

