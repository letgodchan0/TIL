# 🌱 Django - Form

> 지금까지 HTML form, input을 통해서 사용자로부터 데이터를 받아왔는데, 사용자들이 입력한 데이터가 개발자가 요구한 형식이 아닐 수 있음을 항상 생각해야 한다. 따라서 사용자가 입력한 데이터를 검증하는 '유효성 검증' 과정을 진행하고 필요시 입력된 데이터를 검증 결과와 함께 다시 표시할 수 있어야 한다. 
>
> Django는 '유효성 검증'을 구현하는데 과중한 작업과 반복 코드를 줄여줌으로써 이 작업을 훨씬 쉽게 만들어 준다.

## Django`s forms

- `Form`은 `Django`의 유효성 검사 도구 중 하나로 외부의 악의적 공격 및 데이터 손상에 대한 중요한 방어 수단

- `Django`는 `Form`과 관련한 유효성 검사를 단순화 하고 자동화 할 수 있는 기능을 제공하여 개발자로 하여금 직접 작성하는 코드보다 더 안전하고 빠르게 수행하는 코드를 작성할 수 있게 한다.
- `Django`는 `form`에 관련된 작업의 아래 세 부분을 처리해 준다.
  1. 렌더링을 위한 데이터 준비 및 재구성
  2. 데이터에 대한 HTML forms 생성
  3. 클라이언트로부터 받은 데이터 수신 및 처리

### `Form Class`

> Django Form 관리 시스템의 핵심으로 Form 내 field, field 배치, 디스플레이, widget, label 초기값, 유효하지 않는 field에 관련된 에러 메세지를 결정

```python
# app 내의 forms.py 생성하여 선언해준다!
# articles/forms.py
from django import forms

class ArticleForm(form.Form):
    title = forms.CharField(max_length=10)
    content = forms.CharField()
```

- Model을 선언하는 것과 유사하며 같은 필드 타입을 사용 (또한, 일부 매개변수도 유사함)
- `forms` 라이브러이에서 파생된 `Form` 클래스를 상속받음

### view함수 수정

``` python
# 기존의 new 함수
def new(request):
    return render(request, 'articles/new.html')
```

```python
# form을 적용한 new 함수
from boards.forms import ArticleForm # 추가
def new(request):
    form = ArticleForm()
    context = {
        'form' : form
    }
    return render(request, 'articles/new.html', context)
```

### 템플릿에 적용

```html
<!-- 기존 new.html -->

{% extends 'base.html' %}
{% block content %}
  <h1>NEW</h1>
  <hr>
  <form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %}
	<label for="title">Title: </label>
    <input type="text" id="title" name="title"><br>
    <label for="content">Content: </label>
    <textarea name="content" id="content" cols="30" rows="10"></textarea> 
    <!-- 바로 위 부분까지 새로운 form을 통해 입력을 받는다! -->
    <input type="submit">
  </form>
  <a href="{% url 'articles:index' %}">back</a>
{% endblock content %}
```

```html
<!-- form을 적용한 new.html -->

{% extends 'base.html' %}
{% block content %}
  <h1>NEW</h1>
  <hr>
  <form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
  <a href="{% url 'articles:index' %}">back</a>
{% endblock content %}
```

- 단순히  `{{ form.as_p}}`만 해주어도 장고 내부적으로 `forms.py`에 작성한 필드 타입들을 입력 받도록 하고 있다. 기존에 우리가 작성해 주었던 `label`의 `for`, `input`의 `id`, `name`등을 알아서 지정을 해주는 신기한 아이

## Form rendering options

> label & input 쌍에 대한 3가지 출력 옵션

1. `as_p()`
   - 각 필드가 단락 (`<p>` 태그)으로 감싸져서 렌더링 됨
2. `as_ul()`
   - 각 필드가 목록 항목(`<li>` 태그)으로 감싸져서 렌더링 됨
   - `<ul>` 태그는 직접 작성해야 함

3. `as_table()`
   - 각 필드가 테이블(`<tr>` 태그)행으로 감싸져서 렌더링 됨
   - `<table>` 태그는 직접 작성해야 함

## Django의 HTML input 요소 표현 방법 2가지

1. `Form fields`

   - `input`에 대한 유효성 검사 로직을 처리하며 템플릿에서 직접 사용 됨

2. `Widgets`

   > Form Fields와 혼동되어서는 안된다!! Form fields는 input 유효성 검사를 처리하는 반면, widgets은 웹페이제에서 input element의 단순 raw한 렌더링을 처리한다.

   - 웹 페이지의 HTML input 요소 렌더링
   - GET/POST 딕셔너리에서 데이터 추출
   - widgets은 반드시 Form fields에 할당 됨

```python
# articles/forms.py

from django import forms

class ArticleForm(form.Form):
    title = forms.CharField(max_length=10)
    # 모델의 TextField처럼 내용을 입력받을 때 넓직한 칸이 화면에 렌더링 되도록 함
    content = forms.CharField(widget=forms.Textarea)
```

```python
# articles/forms.py

from django import forms

class ArticleForm(form.Form):
    # choiceField의 스타일 가이드가 대문자임, `sl` 여기는 value값으로 장고가 받는 값
    REGION_A = 'sl'
    REGION_B = 'dj'
    REGION_C = 'gj'
    RESIGONS_CHOICES = [
        #(변수값 (value 값), 실제 사용자한테 출력되는 값)
        (REGION_A, '서울'),
        (REGION_B, '대전'),
        (REGION_C, '광주')
    ]
    
    # ChoiceField는 html에서 select에 해당하는 것!
    region = forms.ChoiceField(widget=forms.Select, choices=RESIGONS_CHOICES)
    
    # ChoiceFiled의 기본값이 select여서 widget 안해줘도 된다
    # region = forms.ChoiceField(choices=RESIGONS_CHOICES)
```

![image-20220408013228507](Django%20-%20Form.assets/image-20220408013228507.png)



















1.  delete  view에서 작성할 때`request.method == POST`를 체크해주는 이유?

   ```python
   def delete(request, pk):
       article = Article.objects.get(pk=pk)
       if request.method == 'POST':
           article.delete()
           return redirect('articles:index')
       return redirect('articles:detail', article.pk)
   ```

   2. get_object_or_404 이거 왜쓰는지? 

   ```python
   def delete(request, pk):
       article = get_object_or_404(Article, pk=pk)
       article.delete()
       return redirect('articles:index')
   ```

   