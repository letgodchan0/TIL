# π± Django 02

## HTML "form" element

> μΉμμ μ¬μ©μ μ λ³΄λ₯Ό μλ ₯νλ μ¬λ¬ λ°©μ (text, button, checkbox, file, hidden, image, password, radio, reset, submit)μ μ κ³΅νκ³ , μ¬μ©μλ‘λΆν° ν λΉλ λ°μ΄ν°λ₯Ό μλ²λ‘ μ μ‘νλ μ­ν μ λ΄λΉ
>
> **ν΅μ¬ μμ±**
>
> - **action : μλ ₯ λ°μ΄ν°κ° μ μ‘λ  URL μ§μ **
> - **method : μλ ₯ λ°μ΄ν° μ λ¬ λ°©μμ μ§μ **



## HTML "input" element

> μ¬μ©μλ‘λΆν° λ°μ΄ν°λ₯Ό **μλ ₯** λ°κΈ° μν΄ μ¬μ©νλ©° type μμ±μ λ°λΌ λμ λ°©μμ΄ λ¬λΌμ§λ€.
>
> ν΅μ¬ μμ±
>
> - **name**
> - μ€λ³΅ κ°λ₯, μμμ μ μΆνμ λ nameμ μ€μ λ κ°μ λκ²¨μ κ°μ κ°μ Έμ¬ μ μμ
> - μ£Όμ μ©λλ GET/POST λ°©μμΌλ‘ μλ²μ μ λ¬νλ νλΌλ―Έν°(nameμ key, valueλ value)λ‘ λ§€ννλ κ²
> - GET λ°©μμμλ URLμμ ?key=value&key=value νμμΌλ‘ λ°μ΄ν°λ₯Ό μ λ¬ν¨

```html
<form action="/catch/" method="GET">
    <label for="message">λ©μμ§</label>
    <input type="text" id="message" name="info">
</form>
```

#### <u>μ°λ¦¬κ° μλ ₯ν μ λ³΄λ€μ΄ `name="info"` μ infoμ λ΄κ²¨ actionμ μ§μ ν΄μ€ κ³³μΌλ‘ λμ΄κ°μ `requests` λͺ¨λλ‘ λ°μμ¬ λ ν€κ°μ `info` λ‘ μ£Όμ΄μΌ μ°λ¦¬κ° μλ ₯ν κ°λ€μ κ°μ Έμ¬ μ μλ€.</u>

```python
def catch(request):
	response = request.GET.get('info')
    context = {
        'message' = response
    }
    return render(request, 'catch.html', context)
```

## HTML "label" element

> μ¬μ©μ μΈν°νμ΄μ€ ν­λͺ©μ λν μ€λͺμ λνλ΄λ©° `input` μμμ μ°κ²°ν  λ **<u>`input`μ `id` μμ±κ³Ό `label`μ `for` μμ±μ λμΌν κ°μ μ§μ ν΄μΌ νλ€.</u>**
>
> labelκ³Ό input μμλ₯Ό μ°κ²°νλ©΄ μκ°μ μΈ κΈ°λ₯ λΏλ§ μλλΌ νλ©΄ λ¦¬λκΈ°μμ labelμ μ½μ΄ μ¬μ©μκ° μλ ₯ν΄μΌ νλ νμ€νΈκ° λ¬΄μμΈμ§ λ μ½κ² μ΄ν΄ν  μ μλλ‘ λλ νλ‘κ·Έλλ°μ  μ΄μ λ μμ



## HTTP request method - "GET"

> μλ²λ‘λΆν° μ λ³΄λ₯Ό μ‘°ννλλ° μ¬μ©νλ©° λ°μ΄ν°λ₯Ό κ°μ Έμ¬ λλ§ μ¬μ©ν΄μΌ νλ€. λ°μ΄ν°λ₯Ό μλ²λ‘ μ μ‘ν  λ bodyκ° μλ Query String Parametersλ₯Ό ν΅ν΄ μ μ‘νλ€. μ°λ¦¬κ° μλ²μ μμ²­μ νλ©΄ μΌλ°μ μΌλ‘ HTML λ¬Έμ νμΌ ν μ₯μ λ°λλ°, μ΄λ μ¬μ©νλ μμ²­ λ°©μμ΄ GETμ΄λ€.!

## μμ

```python
# urls.py
urlpatterns = [
   path('admin/', admin.site.urls),
   # formμ ν΅ν΄ μ¬μ©μλ‘λΆν° μ λ³΄λ₯Ό λ°μμ μ²λ¦¬
   path('throw/', views.throw),
   # formμ λ΄κΈ΄ λ΄μ©μ μ λ¬λ°μ
   path('catch/', views.catch), 
]
```

```python
# views.py
def throw(request):
    return render(request, 'throw.html')

def catch(request):
    # throw.html inputμ nameμ΄ 'message'μ¬μ ν€ κ°μ΄ messageλ€!
    # request.GET['message']ν΄λ λμ§λ§ ν€κ° μμ κ²½μ°λ₯Ό κ³ λ €ν΄ get λ©μλ μ¬μ©
    message = request.GET.get('message')
    print(request.GET, type(request.GET))
    context = {
        'message' : message
    }
    return render(request, 'catch.html', context)
```

```html
# throw.html
<form action="/catch/" method="GET">
  <label for="message">λ©μμ§</label>
  {% comment %} μ¬κΈ° μμ±ν name κ°μΌλ‘ λ°μ κΊΌμ!! {% endcomment %}
  <input type="text" id="message" name="message">
  <input type="submit" value="μλ¬΄κ±°λ μλ ₯">
</form>

# catch.html
<h1>{{ message }}</h1>
<a href="/throw/">λ€μ λμ§κΈ°</a>
```



## Variable Routing

- URL μ£Όμλ₯Ό λ³μλ‘ μ§μ νμ¬ view ν¨μμ μΈμλ‘ λκΈΈ μ μμ
- μ¦, λ³μ κ°μ λ°λΌ νλμ path()μ μ¬λ¬ νμ΄μ§λ₯Ό μ°κ²° μν¬ μ μμ

```python
# urls.py
urlpatterns = [
    .....,
    # url μ£Όμλ₯Ό λ³μλ‘ μ¬μ©νλ λ°©λ²μΌλ‘ idκ° views.blogμ 2λ²μ§Έ μΈμλ‘ λ€μ΄κ°
    path('blog/<int:id>/', views.blog),
    path('hello/<name>/', views.hello),
]
```

```python
# veiws.py
def blog(request, id):
    context = {
        'id' : id
    }
    return render(request, 'blog.html', context)

def hello(request, name):
    context = {
        'name' : name
    }
    return render(request, 'hello.html', context)
```

**<u>url μ£Όμλ₯Ό λ³μλ‘ μ¬μ©νμ¬ μ μλ₯Ό μλ ₯νλ©΄ ν΄λΉνλ νμ΄μ§ μ°κ²°</u>**

![image-20220303204517072](Django%2002.assets/image-20220303204517072.png)

![image-20220303204534880](Django%2002.assets/image-20220303204534880.png)

**<u>hello/λ€μ μνλ κΈμλ₯Ό μλ ₯νλ©΄ ν΄λΉνλ νμ΄μ§ μ°κ²°</u>**

![image-20220303204950326](Django%2002.assets/image-20220303204950326.png)

![image-20220303205005751](Django%2002.assets/image-20220303205005751.png)



## App URL mapping

> appμ view ν¨μκ° λ§μμ§λ©΄μ μ¬μ©νλ path() λν λ§μμ§κ³ , ν νλ‘μ νΈλ μ¬λ¬ μ±μ κ΄λ¦¬ν  μ μκΈ° λλ¬Έμ app λν λ λ§μ΄ μμ±λλ€. λ°λΌμ νλ‘μ νΈμ `urls.py`μμ λͺ¨λ κ΄λ¦¬νλ κ²μ νλ‘μ νΈ μ μ§ λ³΄μμ μ’μ§ μλ€.
>
> κ·Έλμ, μ΄μ λ κ° appμ `urls.py`λ₯Ό μμ±νκ³  μ¬κΈ°μ μμ±νλ€!

### 1. κ°κ°μ app νμμ `urls.py` νμΌμ μμ±νλ€

```python
# pages/urls.py
from django.urls import path
from . import views

urlpatterns = [
	path('index/', views.index. name='index'),
]
```

```python
# articles/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('index/', views.index, name='index'),
]
```

 ### 2. νλ‘μ νΈ νμΌ νμμ `urls.py`μμ κ° appμ `urls.py` νμΌλ‘ URL λ§€νμ μν

```python
# firstpjt/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls')),
    path('pages/', include('pages.urls')),
]

# http://127.0.0.1:8000/articles/index/ λΌκ³  μΉλ©΄,
# articles μ±μ νμμ viewsμ index ν¨μκ° μ€νλλ€!
```

### <u>include()</u>

- λ€λ₯Έ URLconf(app1/urls.py)λ€μ μ°Έμ‘°ν  μ μλλ‘ λμ
- ν¨μ include()λ₯Ό λ§λκ² λλ©΄, URLμ κ·Έ μμ κΉμ§ μΌμΉνλ λΆλΆμ μλΌλ΄κ³ , λ¨μ λ¬Έμμ΄ λΆλΆμ νμ μ²λ¦¬λ₯Ό μν΄ includeλ URLconfλ‘ μ λ¬

- djangoλ λͺμμ  μλκ²½λ‘ (from . module import ...)λ₯Ό κΆμ₯νλ€

![Image Pasted at 2022-3-3 12-54](Django%2002.assets/Image%20Pasted%20at%202022-3-3%2012-54.png)

## Naming URL patterns

> λ λμκ°μ μ΄μ λ λ§ν¬μ urlμ μ§μ  μμ±νλ κ²μ΄ μλλΌ path() ν¨μμ name μΈμλ₯Ό μ μν΄μ μ¬μ©νλ€. 

- Django Template Tag μ€ νλμ΄ url νκ·Έλ₯Ό μ¬μ©ν΄μ path() ν¨μμ μμ±ν nameμ μ¬μ©ν  μ μμ
- url μ€μ μ μ μλ νΉμ ν κ²½λ‘λ€μ μμ‘΄μ±μ μ κ±°ν  μ μμ

>  μ¬κΈ°κΉμ§λ§ λ³΄λ©΄ λ¬΄μ¨ μλ¦¬μΈκ° μΆκ² μ§λ§, μ²μ²ν λΉκ΅ν΄λ³΄λ©΄ λλ€!

```html
# κΈ°μ‘΄μ a νκ·Έλ₯Ό λ³΄λ©΄ μμ /λΌλ λ£¨νΈ + url μ£Όμλ₯Ό μλ ₯ν΄μ£Όμ΄μΌ νλ€.

<a href="/throw/">ν΄λ¦­</a>
```

```html
# μ΄μ λ url patternsμ url template tagλ₯Ό μ¬μ©νλ©΄ λ€μκ³Ό κ°μ΄ λ³κ²½ν  μ μλ€.

<a href="{% url 'throw' %}">ν΄λ¦­</a>
```

> μμ κ°μ λ°©μμ μ¬μ©νκΈ° μν΄μλ urls.pyμ url patternsλ₯Ό μ μ©ν΄μΌ νλ€.
>
> **pathμ 3λ²μ§Έ μΈμλ‘ name="μ΄ μ£Όμλ₯Ό νννλ κ°" μ μμ±νκ³ , template tagλ‘ {% url 'μ¬κΈ°' %}λ₯Ό λμ  μ¬μ©νλ©΄ λλ€.**
>
> μ£Όμ΄μ§ URL ν¨ν΄ μ΄λ¦ λ° μ νμ  λ§€κ° λ³μμ μΌμΉνλ μ λ κ²½λ‘μ£Όμλ₯Ό λ°ννλ κ²μΌλ‘ ννλ¦Ώμ URLμ νλ μ½λ©νμ§ μκ³ λ DRY μμΉμ μλ°νμ§ μμΌλ©΄μ λ§ν¬λ₯Ό μΆλ ₯νλ λ°©λ²μ΄λ€. 

```python
# articles/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('blog/<int:id>/', views.blog, name='blog'),
    path('hello/<name>/', views.hello, name='hello'),
]
```

- λ§μ§λ§μΌλ‘ appμ΄ λ§μμ§λ€ λ³΄λ©΄ λμΌν app μ΄λ¦ μ΄ μκΈ°κ² λλ€. **λμΌν app μ΄λ¦μ΄ μκΈ°κ² λλ©΄ `settings`μ ``INSTALLED_APPS`μ μ ν μμλλ‘ μ°Ύκ² λκΈ° λλ¬Έμ** μ μΌ μμ APPμμλ§ λ§ν¬λ₯Ό μ°Ύκ² λ  μ μλ€. 

- μ΄λ₯Ό μν΄ λ€μμ€νμ΄μ€ (app_name)μ μ€μ ν΄μ κ΅¬λΆν΄μ£Όμ΄μΌ νλ€. λ¨μν appμ΄ μ¬λ¬κ° μ΄μμΌ λ μ¬μ©νλ κ²μ΄ μλ μ₯κ³  κΆμ₯μ¬ν­μ΄κΈ° λλ¬Έμ κ° appμ urlμ νμΌμ λ§λ€κ³  app_nameμ μ€μ  ν΄μ€ λ€ include νλλ‘ ν λ€ λ€μμ€νμ΄μ€λ₯Ό μ¬μ©νλλ‘ νμ!!!

- μ¬κΈ°μ app_nameμ μ ν΄μ§ λ³μλͺ!

```python
# articles/urls.py
from django.urls import path
from . import views

app_name = 'articles'

urlpatterns = [
    path('catch/', views.catch, name='catch'),
]
```

```html
# catch.html
<a href="{% url 'articles:throw' %}">ν΄λ¦­</a>
```

## Namespace

> μ΄λ¦κ³΅κ° λλ λ€μμ€νμ΄μ€λ κ°μ²΄λ₯Ό κ΅¬λΆν  μ μλ λ²μλ₯Ό λνλ΄λ λ§λ‘ μΌλ°μ μΌλ‘ νλμ μ΄λ¦ κ³΅κ°μμλ νλμ μ΄λ¦μ΄ λ¨ νλμ κ°μ²΄λ§μ κ°λ¦¬ν€κ²λλ€.

## Templates namespace

- μ₯κ³ λ κΈ°λ³Έμ μΌλ‘ templatesλ₯Ό μ°Ύμ λ κ° app/templateκΉμ§λ μμμ μ°Ύμμ€λ€. νμ§λ§ μλ‘ λ€λ₯Έ appμ λμΌν μ΄λ¦(index.html)μ ννλ¦Ώ νμΌμ΄ μλ€λ©΄, μ μΌ μ²μ μ°Ύμ(settingsμμ μ±μ λ±λ‘ν μμλλ‘) index.htmlμ νλ©΄μ λΏλ €μ€λ€. μ΄λ μ₯κ³ κ° κΈ°λ³Έμ μΌλ‘ λͺ¨λ  κ²½λ‘λ₯Ό λͺ¨μμ λ³΄κΈ° λλ¬Έμ΄λ€.

- λ°λΌμ app/templates(μ¬κΈ°κΉμ§λ μ₯κ³ κ° κΈ°λ³ΈμΌλ‘ μ°Ύμ) λ€μ ν΄λλ₯Ό νλ λ§λ€μ΄μΌ νλ€. 
  - <u>**app/templates/app/index.html μ΄λ°μμΌλ‘ λ¬Όλ¦¬μ μΌλ‘ ν΄λλ₯Ό λΌμλ£λ λ€μμ€νμ΄μ€ λ°©μμ νμ© ν΄μΌ νλ€.**</u>
  - κ·Έλ κΈ° λλ¬Έμ λͺ¨λ  κ²½λ‘μλ λΉ μ§μμ΄ λ¬Όλ¦¬μ μΌλ‘ λ§λ€μ΄μ€ ν΄λ μ΄λ¦μ λΆμ¬ μ£Όμ΄μΌ νκ³ , view ν¨μλ λ§μ°¬κ°μ§λ‘ λ¦¬ν΄ κ°μ΄ index.htmlλ‘ λμ΄ μλ κ²μ articles/index.htmlλ‘ λ³κ²½ν΄μ£Όμ΄μΌ νλ€!

```python
# articles/views.py

return render(request, 'articles/index.html')
```

```python
# pages/views.py

return render(request, 'pages/index.html')
```



## Static files

- μλ²μΈ‘μμ λ―Έλ¦¬ μ€λΉν΄ λκ³  μλ νμΌ

- μ¬μ©μμ μμ²­μ λ°λΌ λ΄μ©μ΄ λ°λλ κ²μ΄ μλλΌ μμ²­ν κ²μ κ·Έλλ‘ λ³΄μ¬μ£Όλ νμΌ

- μλ₯Ό λ€μ΄, μΉ μλ²λ μΌλ°μ μΌλ‘ μ΄λ―Έμ§, μλ° μ€ν¬λ¦½νΈ λλ CSSμ κ°μ λ―Έλ¦¬ μ€λΉλ μΆκ° νμΌ(μμ§μ΄μ§ μλ)μ μ κ³΅ν΄μΌ ν¨

- νμΌ μμ²΄κ° κ³ μ λμ΄ μκ³ , μλΉμ€ μ€μλ μΆκ°λκ±°λ λ³κ²½λμ§ μκ³  κ³ μ λμ΄ μμ

- Djangoμμλ μ΄λ¬ν νμΌλ€μ static fileμ΄λΌ νκ³  staticfiles μ±μ ν΅ν΄ μ μ  νμΌ κ΄λ ¨ κΈ°λ₯μ μ κ³΅νλ€.

## Static files κ΅¬μ±

1. django.contrib.staticfiles μ±μ΄ `INSTALLED_APPS`μ μλμ§ νμΈ (INSTALLED_APPSμ μ΄λ―Έ μμ)
2. setting.pyμ `STATIC_URL` μ μ
3. ννλ¦Ώμμ static ννλ¦Ώ νκ·Έλ₯Ό μ¬μ©νμ¬ static fileμ΄ μλ μλκ²½λ‘λ₯Ό λΉλ
4. μ±μ static file μ μ₯νκΈ° (`my_app/static/my_app/sample.jpg`)

## Django template tag

- load
  - μ¬μ©μ μ μ ννλ¦Ώ νκ·Έ μΈνΈλ₯Ό λ‘λ
  - λ‘λνλ λΌμ΄λΈλ¬λ¦¬, ν¨ν€μ§μ λ±λ‘λ λͺ¨λ  νκ·Έμ νν°λ₯Ό λ‘λ
- static
  - STATIC_ROOTμ μ μ₯λ μ μ  νμΌμ μ°κ²°

- templatesμ μλ‘ λ€λ₯Έ appμ λμΌν μ΄λ¦μ νμΌμ΄ μ‘΄μ¬ν  μ μκΈ° λλ¬Έμ λ€μμ€νμ΄μ€λ₯Ό μμ±ν΄μΌ νλ€.

- μ΄λ―Έμ§ νμΌ μμΉ - `articles/static/articles/images/`
- static file κΈ°λ³Έ κ²½λ‘
  - `app_name/static/`



## The staticfiles app

**`STATICFILES_DIRS`**

```python
STATICFILES_DIRS = [
    BASE_DIR / 'static',
]
```

- app/static/ λλ ν λ¦¬ κ²½λ‘λ₯Ό μ¬μ©νλ κ²(κΈ°λ³Έ κ²½λ‘) μΈμ μΆκ°μ μΈ μ μ  νμΌ κ²½λ‘ λͺ©λ‘μ μ μνλ λ¦¬μ€νΈ
- μΆκ° νμΌ λλ ν λ¦¬μ λν μ μ²΄ κ²½λ‘λ₯Ό ν¬ν¨νλ λ¬Έμμ΄ λͺ©λ‘μΌλ‘ μμ±λμ΄μΌ ν¨

**`STATIC_URL`**

```python
STATIC_URL = '/static/'
```

- STATIC_ROOTμ μλ μ μ  νμΌμ μ°Έμ‘° ν  λ μ¬μ©ν  URL
- κ°λ° λ¨κ³μμλ μ€μ  μ μ  νμΌλ€μ΄ μ μ₯λμ΄ μλ app/static/ κ²½λ‘ (κΈ°λ³Έ κ²½λ‘) λ°STATICFILES_DIRSμ μ μλ μΆκ° κ²½λ‘λ€μ νμν¨
- μ€μ  νμΌμ΄λ λλ ν λ¦¬κ° μλλ©°, URLλ‘λ§ μ‘΄μ¬ λΉμ΄ μμ§ μμ κ°μΌλ‘ μ€μ  νλ€λ©΄ λ°λμ slash(/)λ‘ λλμΌ ν¨



**`STATIC_ROOT`**

- collectstaticμ΄ λ°°ν¬λ₯Ό μν΄ μ μ  νμΌμ μμ§νλ λλ ν λ¦¬μ μ λ κ²½λ‘
- django νλ‘μ νΈμμ μ¬μ©νλ λͺ¨λ  μ μ  νμΌμ ν κ³³μ λͺ¨μ λ£λ κ²½λ‘
- κ°λ° κ³Όμ μμ setting.pyμ DEBUG κ°μ΄ Trueλ‘ μ€μ λμ΄ μμΌλ©΄ ν΄λΉ κ°μ μμ©λμ§ μμ
- μ§μ  μμ±νμ§ μμΌλ©΄ django νλ‘μ νΈμμλ setting.pyμ μμ±λμ΄ μμ§ μμ
- μ€ μλΉμ€ νκ²½(λ°°ν¬ νκ²½)μμ djangoμ λͺ¨λ  μ μ  νμΌμ λ€λ₯Έ μΉ μλ²κ° μ§μ  μ κ³΅νκΈ° μν¨
- κ°λ°κ³Όμ μμ μ°μ§ μκ³  λ°°ν¬ λ¨κ³μμ μ°μ΄λ μ 
- λ°°ν¬ λͺ¨λλΌλ©΄ μλ§μ‘΄μ΄ μ₯κ³  λ΄μ λͺ¨λ  κ²½λ‘λ₯Ό μΆμ ν  μ μκΈ° λλ¬Έμ STATIC_ROOTκ° λͺ¨λ  STATIC νμΌμ λ¬Όλ¦¬μ μΌλ‘ λͺ¨μμ μμ±ν΄μ£Όκ³  μλ§μ‘΄μ STATIC_ROOTλ₯Ό λ³΄κ³  κ²½λ‘λ₯Ό μΆμ ν¨
- staticμ΄ λ°μ μλμ΄ μλ€λ©΄, μλ²κ° μ€νλλ λμ μΆμ  λͺ»ν  μ μκΈ° λλ¬Έμ μλ² μ’λ£ν λ€μ μ€νν΄λ³΄κΈ°

> [μ°Έκ³ ] **collectstatic**
>
> - νλ‘μ νΈ λ°°ν¬ μ ν©μ΄μ Έμλ μ μ  νμΌλ€μ λͺ¨μ νΉμ  λλ ν λ¦¬λ‘ μ?κΈ°λ μμ
> - settings.pyμ STATIC_ROOT = BASE_DIR / 'staticfiles' λ‘ λκ³  python manage.py collectstatic λͺλ Ήμ νλ©΄ μ₯κ³  λ΄λΆμμ κ΅¬λνλλ° νμν λͺ¨λ  μ μ  νμΌλ€μ΄ μκΉ
>
> ```python
> # settings.py μμ
> 
> STATIC_ROOT = BASE_DIR / 'staticfiles'
> $ python manage.py collectstatic
> ```



## Static file μ¬μ©νκΈ°

1. κΈ°λ³Έκ²½λ‘

   - `article/static/articles/` κ²½λ‘μ μ΄λ―Έμ§ νμΌ μμΉ

     ```jinja
     <!-- articles/index.html -->
     
     {% extends 'base.html' %}
     {% load static %}
     
     {% block content %}
       <img src="{% static 'articles/sample.png' %}" alt="sample">
       ...
     {% endblock %}
     ```

   - μ΄λ―Έμ§ νμΌ μμΉ - `articles/static/articles/`

   - static file κΈ°λ³Έ κ²½λ‘

     - `app_name/static/`

2. μΆκ° κ²½λ‘

   - `static/` κ²½λ‘μ CSS νμΌ μμΉ

```jinja
<!-- base.html -->

<head>
  {% block css %}{% endblock %}
</head>
```

```python
# settings.py

STATICFILES_DIRS = [
    BASE_DIR / 'static',
]
```

```html
<!-- articles/index.html -->

{% extends 'base.html' %}
{% load static %}

{% block css %}
  <link rel="stylesheet" href="{% static 'style.css' %}">
{% endblock %}
```

```css
/* static/style.css */

h1 {
    color: crimson;
}
```



**STATIC_URL νμΈν΄λ³΄κΈ°**

![Snipaste_2022-03-04_16-17-39](Django%2002.assets/Snipaste_2022-03-04_16-17-39-16465760470623.png)

