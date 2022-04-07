# 🌱 ModelForm

> Django Form을 사용하다 보면 Model에 정의한 필드를 유저로부터 입력받기 위해 Form에서 Model 필드를 재정의하는 행위가 중복될수 있다. 따라서 Django는 Model을 통해 Form Class를 만들 수 있는 ModelForm이라는 Helper를 제공한다.

- 우리가 `Form`을 통해 `html`파일에서 입력을 받는 것이 간단해지긴 했지만, 이 과정에서 DB에 데이터를 보내는 과정에서 불필요한 코드가 중복될 수 있다. 그렇기 때문에 `ModelForm`은 우리가 사용자로부터 입력받는 내용에 대한 정보들, 즉 데이터 베이스에 있는 필드(`Model`에 작성한 내용)를 기반으로 `Form`을 작성하는 느낌이라고 생각하면 될듯

### 1. ModelForm 선언

```PYTHON
# articles/forms.py

from django import forms
from .models import Article

# ModelForm은 모델의 필드로 HTML을 만들어줌
class ArticleForm(forms.ModelForm):
    # 내가 form으로 만들고 싶은 모델의 정보를 저장하는 클래스
    class Meta:
        model = Article # 내가 만든 모델 이름
        fields = ['title', 'content'] # 내가 만든 모델 필드 중 사용할 필드
        # fields = '__all__'  # 전체 필드 출력
        # exclude = ('title') # 필드로 가져오는 것 중에 title을 빼겠다는 뜻 튜플이나 리스트 둘다 가능
```

- `forms` 라이브러리에서 파생된 `ModelForm` 클래스를 상속 받음
- 정의한 클래스 안에 `Meta` 클래스를 선언하고, 어떤 모델을 기반으로 `Form`을 작성할 것인지에 대한 정보를 `Meta` 클래스에 지정
- 클래스 변수 `fields`와 `exclude`는 동시에 사용할 수 없음

#### `Meta class`

> Model의 정보를 저장하는 곳으로 ModelForm을 사용할 경우 사용할 모델이 있어야 하는데 Meta Class가 이를 구성한다. 즉 해당 Model에 정의한 field 정보를 Form에 적용하기 위함!

##### [참고] Inner Class(Nested Class)

- 클래스 내에 선언된 다른 클래스
- 관련 클래스를 함께 그룹화 하여 가독성 및 프로그램 유지 관리를 지원 (논리적으로 묶어서 표현)
- 외부에서 내부 클래스에 접근할 수 없으므로 코드의 복잡성을 줄일 수 있음

### 2. create view 수정

```python
# articles/views.py
from .forms import ArticleForm # 추가

def create(request):
    form = ArticleForm(request.POST)
    if form.is_valid():
        article = form.save()
        return redirect('articles:detail', article.pk)
   	return redirect('articles:new')
```

- `is_valid() method`는 유효성 검사를 실행하고, 데이터가 유효한지 여부를 boolean으로 반환 해준다. 데이터 유효성 검사를 보장하기 위해 많은 테스트에 대해 Django는 `is_valid()`를 제공한다.

- `save() method`

  - Form에 바인딩 된 데이터에서 데이터베이스 객체를 만들고 저장한다.
  - ModelForm의 하위 클래스는 기존 모델 인스턴스를 키워드 인자 `instance`로 받아 들일 수 있음
    - 이것이 제공되면 `save()`는 해당 인스턴스를 수정(Update)
    - 제공되지 않은 경우 `save()`는 지정된 모델의 새 인스턴스를 만듦 (Create)
  - Form의 유효성이 확인되지 않은 경우 `save()`를 호출하면 `form.errors`를 통해 에러 확인이 가능하다.

  ```python
  # Create a form instance from POST data.
  form = ArticleForm(request.POST)
  
  # Create
  # Save a new Article object from the form`s data
  new_article = form.save()
  
  # Update
  # Create a form to edit an existing Article, but use POST data to populate the form.
  article = Article.objects.get(pk=1)
  form = ArticleForm(request.POST, instance=article)
  form.save()
  ```

### 3. create 함수 대격변

> new path & new view 함수를 삭제하고 create view 함수로 한번에 구현

```python
def create(request):
    if request.method == 'POST':
        # 사용자가 입력한거 가지고 옴
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save()
            return redirect('articles:detail', article.pk)
    else:
        # 아무것도 입력 안받은 상태, 즉 처음에 작성하려고 들어오는 상태 기존의 new 부분
        form = ArticleForm()
    context = {
        'form' : form
    }
    return render(request, 'articles/create.html', context)
```

```html
<!-- create.html(기존의 new.html 이름 변경 모든 new(url등등)를 create로 변경) -->

{% extends 'base.html' %}

{% block content %}
  <h1>CREATE</h1>
  <hr>
  <form action="{% url 'articles:create' %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
{% endblock content %}
```

- `input` 태그에 공백 데이터를 넣어보고 작성하면 에러 메세지를 출력해준다.

![image-20220408021504067](Django%20-%20ModelForm.assets/image-20220408021504067.png)

### 4. Delete 수정

```python
def delete(request, pk):
    article = Article.objects.get(pk=pk)
    # 무조건 삭제버튼을 통해서만 삭제를 할 수 있도록
    if request.method == 'POST':
        article.delete()
        return redirect('articles:index')
    return redirect('articles:detail', article.pk)
```

- `delete` 함수에서 `request의 method`가 `POST`일때만 삭제를 한다는 의미는, 삭제 버튼을 눌렀을 때만 삭제가 진행된다는 것을 의미한다. 너무 당연하다고 생각할 수 있지만 기존의 `delete` 함수였다면 누군가 `url`에 `delete`를 요청하는 주소를 쳤을 때(요청이 `GET`) 삭제가 되버리는 어이없는 상황이 발생할 수 있다. 따라서 `db`를 조작하는 `POST`일 때만 삭제하도록 하기 위해 위와 같이 코드를 짜야 한다. 

```HTML
<!-- detail.html -->

<form action="{% url 'articles:delete' article.pk %}" method="POST">
    {% csrf_token %}
    <input type="submit" value="삭제">
</form>
```

### 5. Update 수정

> edit path & edit view 함수를 삭제하고 update view 함수로 한번에 구현

```python
def update(request, pk):
    article = Article.objects.get(pk=pk)
    if request.method == 'POST':
        # update
        form = ArticleForm(request.POST, instance=article)
        if form.is_valid():  # 유효성 검사
            article = form.save()
            return redirect('articles:detail', article.pk)
    else:
        # edit
        form = ArticleForm(instance=article)
    context = {
        'article': article,
        'form': form,
    }
    return render(request, 'articles/update.html', context)
```

```html
<!-- edit.html(기존의 edit.html 이름 변경 모든 edit(url등등)을 update로 변경) -->
{% extends 'base.html' %}

{% block content %}
  <h1>UPDATE</h1>
  <hr>
  <form action="{% url 'articles:update' article.pk %}" method="POST">
    {% csrf_token %}
    {{ form.as_p }}
    <input type="submit">
  </form>
  <a href="{% url 'articles:detail' article.pk %}">back</a>
{% endblock content %}
```

## Form & ModelForm 비교

- Form

  - 어떤 Model에 저장해야 하는지 알수 없으므로 유효성 검사 이후 cleaned_data 딕셔너리를 생성
  - cleaned_data 딕셔너리에서 데이터를 가져온 후 `.save()`를 호출해야 함

  ![image-20220408023740257](Django%20-%20ModelForm.assets/image-20220408023740257.png)

  - Model에 연관되지 않은 데이터를 받을 때 사용

- ModelForm

  - Django가 해당 model에서 양식에 필요한 대부분의 정보를 이미 정의
  - 어떤 레코드를 만들어야 할지 알고 있기 때문에 바로 `.save()` 호출이 가능하다.

## ModelForm - widgets

> Django의 HTML input element를 표현하고 HTML 렌더링을 처리하기 위해 사용

1. 첫번째 방식

```python
# articles/forms.py
from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):
    
    class Meta:
        model = Article
        fields = '__all__'
        widgets = {
            'title' : forms.TextInput(attrs = {
                'class' : 'my-title',
                'placeholder' : 'Enter the title',
                'maxlength' : 10,
            }
          )
        }
```

2. <u>**두번째 방식 (권장)**</u>

```python
from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):
    title = forms.CharField(
    	label = '제목',
    	widget = forms.TextInput(
        	attrs = {
                'class' : 'my-title',
                'placeholder' : 'Enter the title',
            }
        ),
    )
    
    class Meta:
        model = Article
        fields = '__all__'
```

##### `widgets 사용 예시`

```python
from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):
    title = forms.CharField(
        label = '제목',
        widget = forms.TextInput(
            attrs = {
                'class': 'my-title',
                'placeholder': 'Enter the title',
                'maxlength' : 10,
            }
        )
    )
    content = forms.CharField(
        widget = forms.Textarea(
            label = '내용',
            attrs = {
                'class': 'my-content',
                'placeholder': 'Enter the content',
                'rows' : 5,
                'cols' : 50,
            }
        ),
        error_messages={
            'required': 'Please enter your content!!!',
        }
    )

    class Meta:
        model = Article
        fields = '__all__'
```

![image-20220408024919478](Django%20-%20ModelForm.assets/image-20220408024919478.png)

## Rendering fields manually 방법

### 1. Rendering fields manually

```html
<!-- articles/create.html -->

<h2>1. Rendering fields manually</h2>
<form action="{% url 'articles:create' %}" method="POST">
  {% csrf_token %}
  <div>
    {{ form.title.errors }}
    {{ form.title.label_tag }}
    {{ form.title }}
  </div>
  <div>
    {{ form.content.errors }}
    {{ form.content.label_tag }}
    {{ form.content }}
  </div>
  <input type="submit">
</form>
```

### 2. Looping over the form`s fields

```html
<h2>2. Looping over the form`s fields</h2>
<form action="{% url 'articles:create' %}" method="POST">
<form action="{% url 'articles:create' %}" method="POST">
  {% csrf_token %}
  {% for field in form %}
    {{ field.errors }}
    {{ field.label_tag }}
    {{ field }}
  {% endfor %}
  <input type="submit">
</form>
```

### 3. Bootstrap Form class

- Bootstrap의 핵심 클래스인  `form-control` class를 widget에 작성한다.

```python
from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):
    title = forms.CharField(
        label = '제목',
        widget = forms.TextInput(
            attrs = {
                'class': 'my-title' 'form-control',
                'placeholder': 'Enter the title',
                'maxlength' : 10,
            }
        )
    )
    content = forms.CharField(
        widget = forms.Textarea(
            label = '내용',
            attrs = {
                'class': 'my-content' 'form-control',
                'placeholder': 'Enter the content',
                'rows' : 5,
                'cols' : 50,
            }
        ),
        error_messages={
            'required': 'Please enter your content!!!',
        }
    )

    class Meta:
        model = Article
        fields = '__all__'
```

![image-20220408030303194](Django%20-%20ModelForm.assets/image-20220408030303194.png)

#### 에러 메시지 with bootstrap alert 컴포넌트

```html
<!-- articles/create.html -->

<h2>2. Looping over the form`s fields</h2>
<form action="{% url 'articles:create' %}" method="POST">
<form action="{% url 'articles:create' %}" method="POST">
  {% csrf_token %}
  {% for field in form %}
   {% if field.errors %}
    {% for error in field.errors %}
      <div class="alert alert-warning">{{ error|escape }}</div>
    {% endfor %}
   {% endif %}
   {{ field.label_tag }}
   {{ field }}
  {% endfor %}
  <input type="submit">
</form>
```

![image-20220408030547843](Django%20-%20ModelForm.assets/image-20220408030547843.png)

### 4. Django Bootstrap Library

- form class에 bootstrap을 적용시켜주는 라이브러리

```bash
$ pip install django-bootstrap-v5
```

```python
# settings.py
INSTALLED_APPS = [
    ...
    'BOOTSTRAP5',
    ...
]
```

- 패키지 목록 업데이트

```BASH
$ pip frreze > requirements.txt
```

#### 설치한 라이브러리 적용해보기

```html
<!-- base.html -->
{% load bootstrap5 %}
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  {% bootstrap_css %}
  <title>Document</title>
</head>
<body>
  <div class="container">
    {% block content %}
    {% endblock content %}
  </div>
  {% bootstrap_javascript %}
</body>
</html>
```

- 기존에 부트스트랩 `CDN`을 넣어주던 곳을 수정해주면 되는데, `CDN` 사용하고 있으면 따로 안해줘도 되는듯!

#### 예시

```HTML
<!-- article/update -->
{% extends 'base.html' %}
{% load bootstrap5 %}

{% block content %}
  <h1>UPDATE</h1>
  <hr>
  <form action="{% url 'articles:update' article.pk %}" method="POST">
    {% csrf_token %}
    {% bootstrap_form form layout='horizontal' %}
    {% buttons submit="Submit" reset="Cancel" %} {% endbuttons %}
  </form>
  <a href="{% url 'articles:detail' article.pk %}">back</a>
{% endblock content %}
```

