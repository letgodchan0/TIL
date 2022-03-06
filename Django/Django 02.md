# 🌱 Django 02

## HTML "form" element

> 웹에서 사용자 정보를 입력하는 여러 방식 (text, button, checkbox, file, hidden, image, password, radio, reset, submit)을 제공하고, 사용자로부터 할당된 데이터를 서버로 전송하는 역할을 담당
>
> **핵심 속성**
>
> - **action : 입력 데이터가 전송될 URL 지정**
> - **method : 입력 데이터 전달 방식을 지정**



## HTML "input" element

> 사용자로부터 데이터를 **입력** 받기 위해 사용하며 type 속성에 따라 동작 방식이 달라진다.
>
> 핵심 속성
>
> - **name**
> - 중복 가능, 양식을 제출했을 때 name에 설정된 값을 넘겨서 값을 가져올 수 있음
> - 주요 용도는 GET/POST 방식으로 서버에 전달하는 파라미터(name은 key, value는 value)로 매핑하는 것
> - GET 방식에서는 URL에서 ?key=value&key=value 형식으로 데이터를 전달함

```html
<form action="/catch/" method="GET">
    <label for="message">메시지</label>
    <input type="text" id="message" name="info">
</form>
```

#### <u>우리가 입력한 정보들이 `name="info"` 의 info에 담겨 action에 지정해준 곳으로 넘어가서 `requests` 모듈로 받아올 때 키값을 `info` 로 주어야 우리가 입력한 값들을 가져올 수 있다.</u>

```python
def catch(request):
	response = request.GET.get('info')
    context = {
        'message' = response
    }
    return render(request, 'catch.html', context)
```

## HTML "label" element

> 사용자 인터페이스 항목에 대한 설명을 나타내며 `input` 요소와 연결할 때 **<u>`input`의 `id` 속성과 `label`의 `for` 속성에 동일한 값을 지정해야 한다.</u>**
>
> label과 input 요소를 연결하면 시각적인 기능 뿐만 아니라 화면 리더기에서 label을 읽어 사용자가 입력해야 하는 텍스트가 무엇인지 더 쉽게 이해할 수 있도록 돕는 프로그래밍적 이점도 있음



## HTTP request method - "GET"

> 서버로부터 정보를 조회하는데 사용하며 데이터를 가져올 때만 사용해야 한다. 데이터를 서버로 전송할 때 body가 아닌 Query String Parameters를 통해 전송한다. 우리가 서버에 요청을 하면 일반적으로 HTML 문서 파일 한 장을 받는데, 이때 사용하는 요청 방식이 GET이다.!

## 예시

```python
# urls.py
urlpatterns = [
   path('admin/', admin.site.urls),
   # form을 통해 사용자로부터 정보를 받아서 처리
   path('throw/', views.throw),
   # form에 담긴 내용을 전달받음
   path('catch/', views.catch), 
]

# views.py
def throw(request):
    return render(request, 'throw.html')

def catch(request):
    # throw.html input의 name이 'message'여서 키 값이 message다!
    # request.GET['message']해도 되지만 키가 없을 경우를 고려해 get 메서드 사용
    message = request.GET.get('message')
    print(request.GET, type(request.GET))
    context = {
        'message' : message
    }
    return render(request, 'catch.html', context)

# throw.html
<form action="/catch/" method="GET">
  <label for="message">메시지</label>
  {% comment %} 여기 작성한 name 값으로 받을 꺼임!! {% endcomment %}
  <input type="text" id="message" name="message">
  <input type="submit" value="아무거니 입력">
</form>

# catch.html
<h1>{{ message }}</h1>
<a href="/throw/">다시 던지기</a>
```



## Variable Routing

- URL 주소를 변수로 지정하여 view 함수의 인자로 넘길 수 있음
- 즉, 변수 값에 따라 하나의 path()에 여러 페이지를 연결 시킬 수 있음

```python
# urls.py
urlpatterns = [
    .....,
    # url 주소를 변수로 사용하는 방법으로 id가 views.blog의 2번째 인자로 들어감
    path('blog/<int:id>/', views.blog),
    path('hello/<name>/', views.hello),
]

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

**<u>url 주소를 변수로 사용하여 정수를 입력하면 해당하는 페이지 연결</u>**

![image-20220303204517072](Django%2002.assets/image-20220303204517072.png)

![image-20220303204534880](Django%2002.assets/image-20220303204534880.png)

**<u>hello/뒤에 원하는 글자를 입력하면 해당하는 페이지 연결</u>**

![image-20220303204950326](Django%2002.assets/image-20220303204950326.png)

![image-20220303205005751](Django%2002.assets/image-20220303205005751.png)



## App URL mapping

> app의 view 함수가 많아지면서 사용하는 path() 또한 많아지고, 한 프로젝트는 여러 앱을 관리할 수 있기 때문에, app 또한 더 많이 작성되기 때문에 프로젝트의 `urls.py`에서 모두 관리하는 것은 프로젝트 유지 보수에 좋지 않다.
>
> 따라서, 이제는 각 app에 `urls.py`를 생성하고 여기에 작성한다!

### 1. 각각의 app 하위에 `urls.py` 파일을 생성한다

```python
# pages/urls.py
from django.urls import path
from . import views

urlpatterns = [

]

# articles/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('index/', views.index, name='index'),
]
```

 ### 2. 프로젝트 파일 하위의 `urls.py`에서 각 app의 `urls.py` 파일로 URL 매핑을 위탁

```python
# firstpjt/urls.py

from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('articles/', include('articles.urls')),
    path('pages/', include('pages.urls')),
]

```

### <u>include()</u>

- 다른 URLconf(app1/urls.py)들을 참조할 수 있도록 도움
- 함수 include()를 만나게 되면, URL의 그 시점까지 일치하는 부분을 잘라내고, 남은 문자열 부분을 후속 처리를 위해 include된 URLconf로 전달

- django는 명시적 상대경로 (from . module import ...)를 권장한다

![Image Pasted at 2022-3-3 12-54](Django%2002.assets/Image%20Pasted%20at%202022-3-3%2012-54.png)

## Naming URL patterns

> 더 나아가서 이제는 링크에 url을 직접 작성하는 것이 아니라 path() 함수의 name 인자를 정의해서 사용한다. 

- Django Template Tag 중 하나이 url 태그를 사용해서 path() 함수에 작성한 name을 사용할 수 있음
- url 설정에 정의된 특정한 경로들의 의존성을 제거할 수 있음

>  여기까지만 보면 무슨 소리인가 싶겠지만, 천천히 비교해보면 된다!

```html
# 기존의 a 태그를 보면 앞에 /라는 루트 + url 주소를 입력해주어야 했다.

<a href="/throw/">클릭</a>
```

```html
# 이제는 url patterns와 url template tag를 사용하면 다음과 같이 변경할 수 있다.

<a href="{% url 'throw' %}">클릭</a>
```

> 위와 같은 방식을 사용하기 위해서는 urls.py에 url patterns를 적용해야 한다.
>
> **path의 3번째 인자로 name="이 주소를 표현하는 값" 을 작성하고, template tag로 {% url '여기' %}를 대신 사용하면 된다.**
>
> 주어진 URL 패턴 이름 및 선택적 매개 변수와 일치하는 절대 경로주소를 반환하는 것으로 템플릿에 URL을 하드 코딩하지 않고도 DRY 원칙을 위반하지 않으면서 링크를 출력하는 방법이다. 

```python
# articles/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path('blog/<int:id>/', views.blog, name='blog'),
    path('hello/<name>/', views.hello, name='hello'),
]
```

- 마지막으로 app이 많아지다 보면 동일한 app 이름 이 생기게 된다. **동일한 app 이름이 생기게 되면 `settings`의 ``INSTALLED_APPS`의 적힌 순서대로 찾게 되기 때문에** 제일 위에 APP에서만 링크를 찾게 될 수 있다. 

- 이를 위해 네임스페이스 (app_name)을 설정해서 구분해주어야 한다. 단순히 app이 여러개 이상일 때 사용하는 것이 아닌 장고 권장사항이기 때문에 각 app에 url을 파일을 만들고 app_name을 설정 해준 뒤 include 하도록 한 뒤 네임스페이스를 사용하도록 하자!!!

- 여기서 app_name은 정해진 변수명!

```python
# articles/urls.py
from django.urls import path
from . import views

app_name = 'articles'

urlpatterns = [
    path('catch/', views.catch, name='catch'),
]

# catch.html
<a href="{% url 'articles:throw' %}">클릭</a>
```

## Namespace

> 이름공간 또는 네임스페이스는 객체를 구분할 수 있는 범위를 나타내는 말로 일반적으로 하나의 이름 공간에서는 하나의 이름이 단 하나의 객체만을 가리키게된다.

## Templates namespace

- 장고는 기본적으로 templates를 찾을 때 각 app/template까지는 알아서 찾아준다. 하지만 서로 다른 app에 동일한 이름(index.html)의 템플릿 파일이 있다면, 제일 처음 찾은(settings에서 앱을 등록한 순서대로) index.html을 화면에 뿌려준다. 이는 장고가 기본적으로 모든 경로를 모아서 보기 때문이다.

- 따라서 app/templates(여기까지는 장고가 기본으로 찯음) 뒤에 폴더를 하나 만들어야 한다. 
  - <u>**app/templates/app/index.html 이런식으로 물리적으로 폴더를 끼워넣는 네임스페이스 방식을 활용 해야 한다.**</u>
  - 그렇기 때문에 모든 경로도 빠짐없이 물리적으로 만들어준 폴더 이름을 붙여 주어야 하고, view 함수도 마찬가지로 리턴 값이 index.html로 되어 있던 것을 articles/index.html로 변경해주어야 한다!

```python
# articles/views.py

return render(request, 'articles/index.html')
```

```python
# pages/views.py

return render(request, 'pages/index.html')
```



## Static files

- 서버측에서 미리 준비해 놓고 있는 파일

- 사용자의 요청에 따라 내용이 바뀌는 것이 아니라 요청한 것을 그대로 보여주는 파일

- 예를 들어, 웹 서버는 일반적으로 이미지, 자바 스크립트 또는 CSS와 같은 미리 준비된 추가 파일(움직이지 않는)을 제공해야 함

- 파일 자체가 고정되어 있고, 서비스 중에도 추가되거나 변경되지 않고 고정되어 있음

- Django에서는 이러한 파일들을 static file이라 하고 staticfiles 앱을 통해 정적 파일 관련 기능을 제공한다.

## Static files 구성

1. django.contrib.staticfiles 앱이 `INSTALLED_APPS`에 있는지 확인INSTALLED_APPS에 이미 있음
2. setting.py에 `STATIC_URL` 정의
3. 템플릿에서 static 템플릿 태그를 사용하여 static file이 있는 상대경로를 빌드
4. 앱에 static file 저장하기 (`my_app/static/my_app/sample.jpg`)

## Django template tag

- load
  - 사용자 정의 템플릿 태그 세트를 로드
  - 로드하는 라이브러리, 패키지에 등록된 모든 태그와 필터를 로드
- static
  - STATIC_ROOT에 저장된 정적 파일에 연결



- templates와 서로 다른 app에 동일한 이름의 파일이 존재할 수 있기 때문에 네임스페이스를 생성해야 한다.

- 이미지 파일 위치 - `articles/static/articles/images/`
- static file 기본 경로
  - `app_name/static/`



## The staticfiles app

**`STATICFILES_DIRS`**

```python
STATICFILES_DIRS = [
    BASE_DIR / 'static',
]
```

- app/static/ 디렉토리 경로를 사용하는 것(기본 경로) 외에 추가적인 정적 파일 경로 목록을 정의하는 리스트
- 추가 파일 디렉토리에 대한 전체 경로를 포함하는 문자열 목록으로 작성되어야 함

**`STATIC_URL`**

```python
STATIC_URL = '/static/'
```

- STATIC_ROOT에 있는 정적 파일을 참조 할 때 사용할 URL
- 개발 단계에서는 실제 정적 파일들이 저장되어 있는 app/static/ 경로 (기본 경로) 및STATICFILES_DIRS에 정의된 추가 경로들을 탐색함
- 실제 파일이나 디렉토리가 아니며, URL로만 존재 비어 있지 않은 값으로 설정 한다면 반드시 slash(/)로 끝나야 함



**`STATIC_ROOT`**

- collectstatic이 배포를 위해 정적 파일을 수집하는 디렉토리의 절대 경로
- django 프로젝트에서 사용하는 모든 정적 파일을 한 곳에 모아 넣는 경로
- 개발 과정에서 setting.py의 DEBUG 값이 True로 설정되어 있으면 해당 값은 작용되지 않음
- 직접 작성하지 않으면 django 프로젝트에서는 setting.py에 작성되어 있지 않음
- 실 서비스 환경(배포 환경)에서 django의 모든 정적 파일을 다른 웹 서버가 직접 제공하기 위함
- 개발과정에서 쓰지 않고 배포 단계에서 쓰이는 애
- 배포 모드라면 아마존이 장고 내의 모든 경로를 추적할 수 없기 때문에 STATIC_ROOT가 모든 STATIC 파일을 물리적으로 모아서 생성해주고 아마존은 STATIC_ROOT를 보고 경로를 추적함
- static이 반영 안되어 있다면, 서버가 실행되는 동안 추적 못할 수 있기 때문에 서버 종료후 다시 실행해보기

> [참고] **collectstatic**
>
> - 프로젝트 배포 시 흩어져있는 정적 파일들을 모아 특정 디렉토리로 옮기는 작업
> - settings.py에 STATIC_ROOT = BASE_DIR / 'staticfiles' 로 두고 python manage.py collectstatic 명령을 하면 장고 내부에서 구동하는데 필요한 모든 정적 파일들이 생김
>
> ```python
> # settings.py 예시
> 
> STATIC_ROOT = BASE_DIR / 'staticfiles'
> $ python manage.py collectstatic
> ```



## Static file 사용하기

1. 기본경로

   - `article/static/articles/` 경로에 이미지 파일 위치

     ```jinja
     <!-- articles/index.html -->
     
     {% extends 'base.html' %}
     {% load static %}
     
     {% block content %}
       <img src="{% static 'articles/sample.png' %}" alt="sample">
       ...
     {% endblock %}
     ```

   - 이미지 파일 위치 - `articles/static/articles/`

   - static file 기본 경로

     - `app_name/static/`

2. 추가 경로

   - `static/` 경로에 CSS 파일 위치

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



**STATIC_URL 확인해보기**

![Snipaste_2022-03-04_16-17-39](Django%2002.assets/Snipaste_2022-03-04_16-17-39-16465760470623.png)

