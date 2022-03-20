# 🌱 Django 01

## Framework Architecture

- MVC Design Pattern (model - view - controller)
- 소프트웨어 공학에서 사용되는 디자인 패턴 중 하나
- 사용자 인터페이스로부터 프로그램 로직을 분리하여 애플리케이션의 시각적 요소나 이면에서 실행되는 부분을 서로 영향 없이 쉽게 고칠 수 있는 애플리케이션을 만들 수 있음
- Django는 **<u>MTV Pattern</u>**이라고 함

### MTV Pattern

- Model
  - 응용프로그램의 데이터 구조를 정의하고 데이터베이스의 기록을 관리 (추가, 수정, 삭제)
- Template (View)
  - 파일의 구조나 레이아웃을 정의
  - 실제 내용을 보여주는 데 사용 (presentation)

- View (Controller)
  - HTTP 요청을 수신하고 HTTP 응답을 반환
  - Model을 통해 요청을 충족시키는데 필요한 데이터에 접근
  - template에게 응답의 서식 설정을 맡김

> 클라이언트가 주소를 검색하는 등 HTTP를 통해 요청을 보내면 제일 먼저 URLS에서 요청을 확인하고, 적절한 VIEW에게 요청을 전달해서 ,보여줄 수 있는 Template를 가져오거나 Model을 통해 적절한 활동을 한 후 HTML 뿌리면서 요청에 응답한다

![image-20220302092210296](Django%2001.assets/image-20220302092210296.png)

## Django 환경설정

### 1. 가상환경 생성 및 활성화

> 프로젝트별로 pip로 설치되는 패키지를 독립적으로 관리하기 위해 설정해주는 것으로 장고 프로젝트를 가상환경 내에서 설치하고 실행한다.
>
> $ python -m venv 가상환경이름

```bash
#가상환경 생성
$ python -m venv venv
```

> `venv` 폴더 내의 스크립트를 실행 시키는 것

```bash
# 가상환경 실행
# source는 어떤 형태의 파일을 실행시킬 때 사용하는 명령어
$ source venv/Scripts/activate

# 가상환경 종료
$ deactivate
```

### 2. Django 설치 및 실행

### <u>설치</u>

```bash
# 3.2.12 버전 설치
$ pip install django==3.2.12
```

**프로젝트 구조**

- Project는 여러 개의 Application의 집합
- 프로젝트에는 여러 앱이 포함될 수 있음
- 앱은 여러 프로젝트에 있을 수 있음
- 구조
  - `__init__.py` : Python에게 이 디렉토리를 하나의 Python 패키지로 다루도록 지시
  - `asgi.py` : Django 애플리케이션이 비동기식 웹 서버와 연결 및 소통하는 것을 도움
  - `settings.py` : 애플리케이션의 모든 설정을 포함
  - `urls.py` : 사이트 url과 적절한 views의 연결을 지정
  - `wsgi.py` :Django 애플리케이션이 웹 서버와 연결 및 소통하는 것을 도움
  - `manage.py` : Django 프로젝트와 다양한 방법으로 상호작용 하는 커맨드라인 유틸리티

### <u>프로젝트 생성</u>

```bash
# 프로젝트 이름에는 python이나 django에서 사용중인 키워드 X, '-'(하이픈 X)
$ django-admin startproject 프로젝트 명 .
```

### <u>서버 활성화</u>

```bash
# 서버 실행
$ python manage.py runserver
```

## 3. Django 개발하기

**Application의 구조**

> Application은 실제 요청을 처리하고 페이지를 보여주는 등의 역할을 담당한다. 하나의 프로젝트 안에 여러 앱을 가질 수 있고 일반적으로 앱은 하나의 역할 및 기능 단위로 작성한다.
>
> Application 에서는 MTV에 해당하는 기능을 구현한다.

- `admin.py` : 관리자용 페이지를 설정 하는 곳

- `apps.py` : 앱의 정보가 작성된 곳
- `models.py` : 앱에서 사용하는 Model을 정의하는 곳
- `test.py` : 프로젝트의 테스트 코드를 작성하는 곳
- `views.py` : view 함수들이 정의 되는 곳

### <u>Application 생성</u>

```bash
# 일반적으로 Application명은 복수형으로 하는 것을 권장
$ python manage.py startapp 하고싶은앱이름s
```

### <u>Application 등록</u>

> app을 생성하고 나면 프로젝트 디렉토리와 앱 디렉토리가 같은 레벨에 생성되기 때문에, 앱이 프로젝트에 포함되어있게 하려면 따로 등록을 해주어야 한다. 주의해야 할 점은 먼저 등록하고 생성하면 앱이 생성되지 않기 때문에 반드시 먼저 생성하고 등록을 진행해야 한다.

```bash
# project/settings.py
INSTALLED_APPS = [
    # Local apps, 여기서 생성한 앱을 등록해주어야 한다.
    '앱이름',
    
    # Third party apps
    'djangorestframework',
    
    # Django apps
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```



## 요청과 응답

>  코드 작성 순서 : 데이터의 흐름대로 **<u>URL -> VIEW -> TEMPLATE</u>**

### URLs

> URLs - HTTP 요청(request)을 알맞은 view로 전달
>
> 사용자가 크롬에 주소를 입력하거나 엔터를 치는 행동을 통해 서버에 요청을 하면 `urls.py`가 사용자로부터 가장 먼저 요청을 받게 되고, 예를들어 `admin` 이라는 주소를 요청한다면, `urls.py`가 아! 관리자 페이지 보여주면 되겠구나 하면서, 관리자페이지를 응답해서 화면에 뿌려주는 느낌

![image-20220302104143068](Django%2001.assets/image-20220302104143068.png)

- 로컬 주소 `http://127.0.0.1:8000/` 뒤에 `admin`이나 `index`를 입력했을 때 그에 해당하는 화면이나 활동이 이루어지는 것을  `view` 함수 내부의 함수를 통해 이루어 지는데, `index`와 그에 해당하는 `views.???` 를 매핑해주는 것을 <u>**urls.py**</u> 에 제일먼저 작성을 한다. 왜냐면 클라이언트의 요청을 가장 먼저 받기 때문에!
- 즉, 내가 `index` 주소를 요청하면, `urls`한테 야! `view` 함수 메인페이지를 리턴하는 애 실행시켜, 실행시키면 `index.html`을 준비시키고 사용자에게 응답함는 느낌
- index/  - url을 설정할 때 뒤에 엔드 슬래쉬를 반드시 써주어야함, but 실제 주소에 입력할 때는 안써도 된다!
- **<u>하나의 views 함수만 매핑 가능하다!!</u>**

### View

> HTTP 요청을 수신하고 HTTP 응답을 반환하는 함수를 작성하는 곳으로, 
>
> 1) Model을 통해 요청에 맞는 필요 데이터에 접근하거나 
> 2) Template에게 HTTP 응답 서식을 맡긴다. 

`index`  주소 요청이 왔을 때 해당 view 함수를 실행할 꺼야!!

![image-20220302223824284](Django%2001.assets/image-20220302223824284.png)



### Templates

> 실제 내용을 보여주는데 사용되는 파일로, 파일의 구조나 레이아웃을 정의(HTML) 한다. Template 파일 경로의 기본 값은 app 폴더 안의 templates 폴더로 지정되어 있다!! 즉, 기본적으로 app/templates로 경로를 읽는다.
>
> 한번 더 강조!! 앱 폴더 안에 **templates** 라고 s까지 확실하게 붙여서 만들어야 하는 것이 장고와의 약속, 장고 내부 경로안에 이렇게 설정되어 있음!



### 추가 설정

![image-20220302224412918](Django%2001.assets/image-20220302224412918.png)



## Template

> 데이터 표현을 제어하는 도구이자 표현에 관련된 로직, 사용하는 built-in-system (DTL)

### Django Template Language (DTL)

- Django template에서 사용하는 built-in template system
- **조건, 반복, 변수, 치환, 필터** 등의 기능을 제공
- 단순히 Python이 HTML에 포함된 것이 아니며, 프로그래밍적 로직이 아니라 <u>프레젠테이션을 표현하기 위한 것</u>
- Python처럼 일부 프로그래밍 구조(if, for 등)를 사용할 수 있지만, 이것은 해당 Python 코드로 실행되는 것이 아님

### DTL Syntax (1/4) - Varibale

```python
{{ variable }}
```

- render()를 사용하여 views.py에서 정의한 변수를 template 파일로 넘겨 사용하는 것
- 변수명은 영어, 숫자와 밑줄(_)의 조합으로 구성될 수 있으나 밑줄로는 시작할 수 없음
  - 공백이나 구두점 문자 또한 사용할 수 없음
- dot(.)를 사용하여 변수 속성에 접근할 수 있음
- render()의 세번째 인자로 {'key' : value} 와 같이 딕셔너리 형태로 넘겨주며, 여기서 정의한 key에 해당하는 문자열이 template에서 사용 가능한 변수명이 됨

### 실습

```python
# urls.py
urlpatterns = [
    path('admin/', admin.site.urls),
    path('index/', views.index),
    path('greeting/', views.greeting),
    path('dinner/', views.dinner),
]
```

```python
# views.py
def greeting(request):
    foods = ['apple', 'banana', 'coconut']
    info = {
        'name' : 'Alice'
    }
    # greeting.html 이 사용할 변수를 넘겨주는 것
    # render의 3번째 인자로 들어가는게 context니까 변수명을 context로 함 다른것도 가능, 딕셔너리니까 key로 접근함
    context = {
        'foods' : foods,
        'info' : info,
    }
    return render(request, 'greeting.html', context)
```

```html
# greeting.html
<p>안녕하세요 저는 {{ info.name }} 입니다.</p>
<p>안녕하세요 저는 {{ info.name|lower }} 입니다.</p>
<p>제가 가장 좋아하는 음식은 {{ foods }} 입니다.</p>
<p>저는 사실  {{ foods.0 }}를 가장 좋아합니다. </p>
```



### DTL Syntax (2/4) - Filters

```python
{{ variable|filter }}
```

- 표시할 변수를 수정할 때 사용

- ex) name 변수를 모두 소문자로 출력

```python
{{ name|lower }}
```

- [60개의 built-in template filters를 제공](https://docs.djangoproject.com/en/4.0/ref/templates/builtins/#built-in-filter-reference)

- chained가 가능하며 일부 필터는 인자를 받기도 한다!

```python
{{ variable|truncatewords:30 }}
```

### 실습

```python
# views.py
def dinner(request):
    foods = ['족발', '햄버거', '치킨', '초밥']
    pick = random.choice(foods)
    context = {
        'pick' : pick,
        'foods' : foods,
    }
    return render(request, 'dinner.html', context )
```

```html
# dinner.html
<h1>오늘 저녁은 {{ pick }}!</h1>
<p>{{ pick }}은 {{ pick|length }}글자 </p>
<p>{{ foods|join:', '}}</p>
<a href="/index/">뒤로</a>
```

### DTL Syntax (3/4) - Tags

```python
{% tag %}
```

- 출력 텍스트를 만들거나, 반복 또는 논리를 수행하여 제어 흐름을 만드는 등 변수보다 복잡한 일들을 수행

- 일부 태그는 시작과 종료 태그가 필요

  ```python
  {% if %}{% endif %}
  ```

- 약 24개의 built-in template tags를 제공한다.

- 실습

```python
# dinner.html

<h1>오늘 저녁은 {{ pick }}!</h1>
<p>{{ pick }}은 {{ pick|length }}글자 </p>
<p>{{ foods|join:', '}}</p>
<a href="/index/">뒤로</a>

<p>메뉴판</p>
<ul>
{% for food in foods%}
<li>{{ food }}</li>
{% endfor %}
</ul>
```



### DTL Syntax (4/4) - Comments

```python
{# #}
```

- Django template에서 라인의 주석을 표현하기 위해 사용

- 아래처럼 유효하지 않은 템플릿 코드가 포함될 수 있음

  ```python
  {# {% if ...%} text {% else %} #}
  ```

- 한 줄 수적에만 사용할 수 있음 (줄 바꿈이 허용되지 않음)

- 여러 줄 주석은 `{% comment %}`와 `{% endcomment %}`사이에 입력

  ```python
  {% comment %}
  	주석
      주석
  {& endcomment %}
  ```

  

### Template inheritance (템플릿 상속)

> app 폴더 내의 templates에 부모 템플림을 만들어 여러 요소들을 한번에 적용하기 위해서는 apps와 같은 레벨에 `templates` 라는 폴더를 생성한 후 project의 `settings.py`의 `TEMPLATES` 에서 경로 설정을 해야한다 `'DIRS' : [BASE_DIR / 'templates',],`

![image-20220302232537905](Django%2001.assets/image-20220302232537905.png)

- 템플릿 상속은 기본적으로 코드의 재사용성에 초점을 맞춤
- 템플릿 상속을 사용하면 사이트의 모든 공통 요소를 포함하고, 하위 템플릿이 재정의 할 수 있는 **블록**을 정의하는 기본 `skeleton` 템플릿을 만들 수 있음
- `base.html`에 부트스트랩 CDN을 적용시키고 다른 템플릿으로 `base.html`을 상속받음

**<u>템플릿 상속 - tags</u>**

```python
{% extends '부모 이름' %}
```

- 자식(하위) 템플릿이 부모 템플릿을 확장한다는 것을 알림
- 반드시 템플릿 최상단에 작성되어야 함

```python
{% block content %} {% endblock %}
```

- 부모 탬플릿에 하위 템플릿에서 재지정할 수 있는 블록을 정의
- 즉, 하위 템플릿이 채울 수 있는 공간 

### 실습

```html
# project/templates/base.html

{% include '_nav.html' %}
# 블럭의 이름을 맞춰야함 여기선 content 
{% block content %}
{% endblock content %}
```

```html
# index.html
# extends할 때는 무조건 최상단에 적어주어야 한다!
{% block extends 'base.html' %}

{% block content %}
  <p>안녕하세요 저는 {{ info.name }} 입니다.</p>
  <p>안녕하세요 저는 {{ info.name|lower }} 입니다.</p>
  <p>제가 가장 좋아하는 음식은 {{ foods }} 입니다.</p>
  <p>저는 사실  {{ foods.0 }}를 가장 좋아합니다. </p>

{% endblock extends 'base.html' %}
```

**<u>템플릿 태그 - include</u>**

```python
{% include '' %}
```

- 템플릿을 로드하고 현재 페이지로 렌더링
- 템플릿 내에 다른 템플릿을 포함 하는 방법

```html
# project/templates/_nav.html

<nav class ="navbar navbar-light bg-light">
	<div class="container-fluid">
    	<a class="navbar-brand" href="#">Navbar</a>
    </div>
</nav>
```

```html
# include tag를 통해 base.html에 _nav.html 포함시키기
# project/templates/base.html

<body>
	{% include '_nav.html' %}
	...
</body>
```

### Django template system (Django 설계 철학)

- 표현과 로직(view)을 분리
  - 템플릿 시스템은 표현을 제어하는 도구이자 표현에 관련된 로직일 뿐.
  - 템플릿 시스템은 이러한 기본 목표를 넘어서는 기능을 지원하지 말아야 한다
- "중복을 배제"
  - 대다수의 동적 웹사이트는 공통 header, footer, navbar 같은 사이트 공통 디자인을 갖는다.
  - Django 템플릿 시스템은 이러한 요소를 한 곳에 저장하기 쉽게 하여 중복 코드를 없애야 한다.
  - 이것이 템플릿 상속의 기초가 되는 철학!
