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

