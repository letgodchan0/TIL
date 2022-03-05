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
    path('index/', views.index, name='index'),
    path('bts/', views.bts, name='bts'),
    path('lotto/', views.lotto, name='lotto'),
    path('mbti/', views.mbti, name='mbti'),
    path('lunch/', views.lunch, name='lunch'),
    path('throw/', views.throw, name='throw'),
    path('catch/', views.catch, name='catch'),
    path('blog/<int:id>/', views.blog, name='blog'),
    path('hello/<name>/', views.hello, name='hello'),
]
```

> 마지막으로 app이 많아지다 보면 비슷한 app이름 이 생기게 된다. 이를 위해 네임스페이스 (app_name)을 지정해서 app_name에 해당하는 별칭인 것을 알려주어야 한다!

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

namespace

templates namespace

이름공간 또는 네임스페이스는 객체를 구분할 수 있는 범위를 나타내는 말로 일반적으로 하나의 이름 공간에서는 하나의 이름이 단 하나의 객체만을 가리키게된다.

프로그래밍을 하다 보면 모든 변수명과 

그래서 장고에서는

1. 서로 다른 app의 같은 이름을 가진 url name은 이름 공간을 설정해서 구분
2. templates, static 등 장고에서는 정해진 경로 하나로 모아서 보기 때문에 **중간에 폴더를 임의로 만들어 줌**으로써 이름공간을 설정

**<u>장고는 기본적으로 각 app/templates까지는 알아서 읽어준다. base/templates도 마찬가지</u>**

articles도 index.html이 있고 pages도 index.html이 있다면, 장고는 기본적으로 모아서 보기 때문에 제일 처음 찾은 index.html을 그냥 화면에 뿌려짐(settings에서 앱을 등록한 순서대로)

따라서 app/templates/여기에 폴더를 하나 만들꺼임/index.html 이런식으로 물리적으로 폴더를 끼워넣는 네임스페이스 방식을 활용한다.  즉 어떤 앱의 텝플

그래서 articles/templates/index.html이 였다면, articles/templates/articles/index.html로 바꾸어 주어야함

그래서 view함수 리턴 값으로 index.html로 되어 있던 것을 articles/index.html로 변경해주어야 한다

---

**url namespace**

url도 마찬가지, 어떤 앱의 url인지 별칭 앞에 하나 더 붙여주어야 한다. 따로 폴더를 생성하는 건 아니고 app_name이라는 값을 설정하는 것! (app_name은 정해진 변수명!!!) 

그래서 `<a href="{% url 'articles:throw' %}">클릭</a>` 이렇게 되는거였음

## static files

서버측에서 미리 준비해 놓고 있는 파일

사용자의 요청에 따라 내용이 바뀌는 것이 아니라 요청한 것을 그대로 보여주는 파일

예를 들어, 웹 서버는 일반적으로 이미지, 자바 스크립트 또는 CSS와 같은 미리 준비된 추가 파일(움직이지 않는)을 제공해야 함

파일 자체가 고정되어 있고, 서비스 중에도 추가되거나 변경되지 않고 고정되어 있음

Django에서는 이러한 파일들을 static file이라 하고 staticfiles 앱을 통해 정적 파일 관련 기능을 제공한다.

INSTALLED_APPS에 이미 있음

templates와 비슷하게 기본적으로 app/static/이 밑에 정적 파일들을 넣어주어야 함

당연하게 이름공간 이슈가 생김 그래서, templates와 똑같이 

app/static/app/파일명 경로를 작성해준다.

---

settings.py에서 DEBUG = True가 기본 값인데, 배포 단계에서는 False로 바꾸어 주어야함 , 우리의 내부 코드를 노출시키기지 않기 위해

STATIC_ROOT : 개발과정에서 쓰지 않고 배포 단계에서 쓰이는 애

배포 모드라면 아마존이 장고 내의 모든 경로를 추적할 수 없기 때문에 STATIC_ROOT가 모든 STATIC 파일을 물리적으로 모아서 생성해주고 아마존은 STATIC_ROOT를 보고 경로를 추적함

settings.py에 STATIC_ROOT = BASE_DIR / 'staticfiles' 로 두고 python manage.py collectstatic 명령을 하면 장고 내부에서 구동하는데 필요한 모든 정적 파일들이 생김

static 관련해서는 서버가 실행되는 동안 추적 못할 수 있기 때문에 서버 종료후 다시 실행해보기

---

베이스 템플릿 줬던 것 처럼

STATICFILES_DIRS = [

​		BASE_DIR / 'static',

]
