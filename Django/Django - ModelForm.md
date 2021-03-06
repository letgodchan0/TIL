# ๐ฑ ModelForm

> Django Form์ ์ฌ์ฉํ๋ค ๋ณด๋ฉด Model์ ์ ์ํ ํ๋๋ฅผ ์ ์ ๋ก๋ถํฐ ์๋ ฅ๋ฐ๊ธฐ ์ํด Form์์ Model ํ๋๋ฅผ ์ฌ์ ์ํ๋ ํ์๊ฐ ์ค๋ณต๋ ์ ์๋ค. ๋ฐ๋ผ์ Django๋ Model์ ํตํด Form Class๋ฅผ ๋ง๋ค ์ ์๋ ModelForm์ด๋ผ๋ Helper๋ฅผ ์ ๊ณตํ๋ค.

- ์ฐ๋ฆฌ๊ฐ `Form`์ ํตํด `html`ํ์ผ์์ ์๋ ฅ์ ๋ฐ๋ ๊ฒ์ด ๊ฐ๋จํด์ง๊ธด ํ์ง๋ง, ์ด ๊ณผ์ ์์ DB์ ๋ฐ์ดํฐ๋ฅผ ๋ณด๋ด๋ ๊ณผ์ ์์ ๋ถํ์ํ ์ฝ๋๊ฐ ์ค๋ณต๋  ์ ์๋ค. ๊ทธ๋ ๊ธฐ ๋๋ฌธ์ `ModelForm`์ ์ฐ๋ฆฌ๊ฐ ์ฌ์ฉ์๋ก๋ถํฐ ์๋ ฅ๋ฐ๋ ๋ด์ฉ์ ๋ํ ์ ๋ณด๋ค, ์ฆ ๋ฐ์ดํฐ ๋ฒ ์ด์ค์ ์๋ ํ๋(`Model`์ ์์ฑํ ๋ด์ฉ)๋ฅผ ๊ธฐ๋ฐ์ผ๋ก `Form`์ ์์ฑํ๋ ๋๋์ด๋ผ๊ณ  ์๊ฐํ๋ฉด ๋ ๋ฏ

### 1. ModelForm ์ ์ธ

```PYTHON
# articles/forms.py

from django import forms
from .models import Article

# ModelForm์ ๋ชจ๋ธ์ ํ๋๋ก HTML์ ๋ง๋ค์ด์ค
class ArticleForm(forms.ModelForm):
    # ๋ด๊ฐ form์ผ๋ก ๋ง๋ค๊ณ  ์ถ์ ๋ชจ๋ธ์ ์ ๋ณด๋ฅผ ์ ์ฅํ๋ ํด๋์ค
    class Meta:
        model = Article # ๋ด๊ฐ ๋ง๋  ๋ชจ๋ธ ์ด๋ฆ
        fields = ['title', 'content'] # ๋ด๊ฐ ๋ง๋  ๋ชจ๋ธ ํ๋ ์ค ์ฌ์ฉํ  ํ๋
        # fields = '__all__'  # ์ ์ฒด ํ๋ ์ถ๋ ฅ
        # exclude = ('title') # ํ๋๋ก ๊ฐ์ ธ์ค๋ ๊ฒ ์ค์ title์ ๋นผ๊ฒ ๋ค๋ ๋ป ํํ์ด๋ ๋ฆฌ์คํธ ๋๋ค ๊ฐ๋ฅ
```

- `forms` ๋ผ์ด๋ธ๋ฌ๋ฆฌ์์ ํ์๋ `ModelForm` ํด๋์ค๋ฅผ ์์ ๋ฐ์
- ์ ์ํ ํด๋์ค ์์ `Meta` ํด๋์ค๋ฅผ ์ ์ธํ๊ณ , ์ด๋ค ๋ชจ๋ธ์ ๊ธฐ๋ฐ์ผ๋ก `Form`์ ์์ฑํ  ๊ฒ์ธ์ง์ ๋ํ ์ ๋ณด๋ฅผ `Meta` ํด๋์ค์ ์ง์ 
- ํด๋์ค ๋ณ์ `fields`์ `exclude`๋ ๋์์ ์ฌ์ฉํ  ์ ์์

#### `Meta class`

> Model์ ์ ๋ณด๋ฅผ ์ ์ฅํ๋ ๊ณณ์ผ๋ก ModelForm์ ์ฌ์ฉํ  ๊ฒฝ์ฐ ์ฌ์ฉํ  ๋ชจ๋ธ์ด ์์ด์ผ ํ๋๋ฐ Meta Class๊ฐ ์ด๋ฅผ ๊ตฌ์ฑํ๋ค. ์ฆ ํด๋น Model์ ์ ์ํ field ์ ๋ณด๋ฅผ Form์ ์ ์ฉํ๊ธฐ ์ํจ!

##### [์ฐธ๊ณ ] Inner Class(Nested Class)

- ํด๋์ค ๋ด์ ์ ์ธ๋ ๋ค๋ฅธ ํด๋์ค
- ๊ด๋ จ ํด๋์ค๋ฅผ ํจ๊ป ๊ทธ๋ฃนํ ํ์ฌ ๊ฐ๋์ฑ ๋ฐ ํ๋ก๊ทธ๋จ ์ ์ง ๊ด๋ฆฌ๋ฅผ ์ง์ (๋ผ๋ฆฌ์ ์ผ๋ก ๋ฌถ์ด์ ํํ)
- ์ธ๋ถ์์ ๋ด๋ถ ํด๋์ค์ ์ ๊ทผํ  ์ ์์ผ๋ฏ๋ก ์ฝ๋์ ๋ณต์ก์ฑ์ ์ค์ผ ์ ์์

### 2. create view ์์ 

```python
# articles/views.py
from .forms import ArticleForm # ์ถ๊ฐ

def create(request):
    form = ArticleForm(request.POST)
    if form.is_valid():
        article = form.save()
        return redirect('articles:detail', article.pk)
   	return redirect('articles:new')
```

- `is_valid() method`๋ ์ ํจ์ฑ ๊ฒ์ฌ๋ฅผ ์คํํ๊ณ , ๋ฐ์ดํฐ๊ฐ ์ ํจํ์ง ์ฌ๋ถ๋ฅผ boolean์ผ๋ก ๋ฐํ ํด์ค๋ค. ๋ฐ์ดํฐ ์ ํจ์ฑ ๊ฒ์ฌ๋ฅผ ๋ณด์ฅํ๊ธฐ ์ํด ๋ง์ ํ์คํธ์ ๋ํด Django๋ `is_valid()`๋ฅผ ์ ๊ณตํ๋ค.

- `save() method`

  - Form์ ๋ฐ์ธ๋ฉ ๋ ๋ฐ์ดํฐ์์ ๋ฐ์ดํฐ๋ฒ ์ด์ค ๊ฐ์ฒด๋ฅผ ๋ง๋ค๊ณ  ์ ์ฅํ๋ค.
  - ModelForm์ ํ์ ํด๋์ค๋ ๊ธฐ์กด ๋ชจ๋ธ ์ธ์คํด์ค๋ฅผ ํค์๋ ์ธ์ `instance`๋ก ๋ฐ์ ๋ค์ผ ์ ์์
    - ์ด๊ฒ์ด ์ ๊ณต๋๋ฉด `save()`๋ ํด๋น ์ธ์คํด์ค๋ฅผ ์์ (Update)
    - ์ ๊ณต๋์ง ์์ ๊ฒฝ์ฐ `save()`๋ ์ง์ ๋ ๋ชจ๋ธ์ ์ ์ธ์คํด์ค๋ฅผ ๋ง๋ฆ (Create)
  - Form์ ์ ํจ์ฑ์ด ํ์ธ๋์ง ์์ ๊ฒฝ์ฐ `save()`๋ฅผ ํธ์ถํ๋ฉด `form.errors`๋ฅผ ํตํด ์๋ฌ ํ์ธ์ด ๊ฐ๋ฅํ๋ค.

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

### 3. create ํจ์ ๋๊ฒฉ๋ณ

> new path & new view ํจ์๋ฅผ ์ญ์ ํ๊ณ  create view ํจ์๋ก ํ๋ฒ์ ๊ตฌํ

```python
def create(request):
    if request.method == 'POST':
        # ์ฌ์ฉ์๊ฐ ์๋ ฅํ๊ฑฐ ๊ฐ์ง๊ณ  ์ด
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save()
            return redirect('articles:detail', article.pk)
    else:
        # ์๋ฌด๊ฒ๋ ์๋ ฅ ์๋ฐ์ ์ํ, ์ฆ ์ฒ์์ ์์ฑํ๋ ค๊ณ  ๋ค์ด์ค๋ ์ํ ๊ธฐ์กด์ new ๋ถ๋ถ
        form = ArticleForm()
    context = {
        'form' : form
    }
    return render(request, 'articles/create.html', context)
```

```html
<!-- create.html(๊ธฐ์กด์ new.html ์ด๋ฆ ๋ณ๊ฒฝ ๋ชจ๋  new(url๋ฑ๋ฑ)๋ฅผ create๋ก ๋ณ๊ฒฝ) -->

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

- `input` ํ๊ทธ์ ๊ณต๋ฐฑ ๋ฐ์ดํฐ๋ฅผ ๋ฃ์ด๋ณด๊ณ  ์์ฑํ๋ฉด ์๋ฌ ๋ฉ์ธ์ง๋ฅผ ์ถ๋ ฅํด์ค๋ค.

![image-20220408021504067](Django%20-%20ModelForm.assets/image-20220408021504067.png)

### 4. Delete ์์ 

```python
def delete(request, pk):
    article = Article.objects.get(pk=pk)
    # ๋ฌด์กฐ๊ฑด ์ญ์ ๋ฒํผ์ ํตํด์๋ง ์ญ์ ๋ฅผ ํ  ์ ์๋๋ก
    if request.method == 'POST':
        article.delete()
        return redirect('articles:index')
    return redirect('articles:detail', article.pk)
```

- `delete` ํจ์์์ `request์ method`๊ฐ `POST`์ผ๋๋ง ์ญ์ ๋ฅผ ํ๋ค๋ ์๋ฏธ๋, ์ญ์  ๋ฒํผ์ ๋๋ ์ ๋๋ง ์ญ์ ๊ฐ ์งํ๋๋ค๋ ๊ฒ์ ์๋ฏธํ๋ค. ๋๋ฌด ๋น์ฐํ๋ค๊ณ  ์๊ฐํ  ์ ์์ง๋ง ๊ธฐ์กด์ `delete` ํจ์์๋ค๋ฉด ๋๊ตฐ๊ฐ `url`์ `delete`๋ฅผ ์์ฒญํ๋ ์ฃผ์๋ฅผ ์ณค์ ๋(์์ฒญ์ด `GET`) ์ญ์ ๊ฐ ๋๋ฒ๋ฆฌ๋ ์ด์ด์๋ ์ํฉ์ด ๋ฐ์ํ  ์ ์๋ค. ๋ฐ๋ผ์ `db`๋ฅผ ์กฐ์ํ๋ `POST`์ผ ๋๋ง ์ญ์ ํ๋๋ก ํ๊ธฐ ์ํด ์์ ๊ฐ์ด ์ฝ๋๋ฅผ ์ง์ผ ํ๋ค. 

```HTML
<!-- detail.html -->

<form action="{% url 'articles:delete' article.pk %}" method="POST">
    {% csrf_token %}
    <input type="submit" value="์ญ์ ">
</form>
```

### 5. Update ์์ 

> edit path & edit view ํจ์๋ฅผ ์ญ์ ํ๊ณ  update view ํจ์๋ก ํ๋ฒ์ ๊ตฌํ

```python
def update(request, pk):
    article = Article.objects.get(pk=pk)
    if request.method == 'POST':
        # update
        form = ArticleForm(request.POST, instance=article)
        if form.is_valid():  # ์ ํจ์ฑ ๊ฒ์ฌ
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

##### + ์ฐธ๊ณ  get_object_or_404

```python
from django.shortcuts import render, redirect, get_object_or_404

def update(request, pk):
    # ๋ฐ์ดํฐ๊ฐ ์๊ฑฐ๋ ๋ง์ด ์์ผ๋ฉด ํ์ด์ฌ ์๋ฌ(์๋ฒ์ 500์ ์ํ์ฝ๋ ๋ฉ์ธ์ง๊ฐ ๋๊ฐ)
    article = Article.objects.get(pk=pk)
    # ์ค์ ๋ก๋ ๊ฒ์๊ธ์ด ์๋๋ฐ ์ฌ์ฉ์๊ฐ ์์ฒญ์ ํ๊ฑฐ๊ธฐ ๋๋ฌธ์ object๊ฐ ์์ผ๋ฉด 404 ์๋ฌ๋ฉ์ธ์ง ๋ณด์ฌ์ฃผ๋๋ก
    article = get_object_or_404(Article, pk = pk)
```

```html
<!-- edit.html(๊ธฐ์กด์ edit.html ์ด๋ฆ ๋ณ๊ฒฝ ๋ชจ๋  edit(url๋ฑ๋ฑ)์ update๋ก ๋ณ๊ฒฝ) -->
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

## Form & ModelForm ๋น๊ต

- Form

  - ์ด๋ค Model์ ์ ์ฅํด์ผ ํ๋์ง ์์ ์์ผ๋ฏ๋ก ์ ํจ์ฑ ๊ฒ์ฌ ์ดํ cleaned_data ๋์๋๋ฆฌ๋ฅผ ์์ฑ
  - cleaned_data ๋์๋๋ฆฌ์์ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์จ ํ `.save()`๋ฅผ ํธ์ถํด์ผ ํจ

  ![image-20220408023740257](Django%20-%20ModelForm.assets/image-20220408023740257.png)

  - Model์ ์ฐ๊ด๋์ง ์์ ๋ฐ์ดํฐ๋ฅผ ๋ฐ์ ๋ ์ฌ์ฉ

- ModelForm

  - Django๊ฐ ํด๋น model์์ ์์์ ํ์ํ ๋๋ถ๋ถ์ ์ ๋ณด๋ฅผ ์ด๋ฏธ ์ ์
  - ์ด๋ค ๋ ์ฝ๋๋ฅผ ๋ง๋ค์ด์ผ ํ ์ง ์๊ณ  ์๊ธฐ ๋๋ฌธ์ ๋ฐ๋ก `.save()` ํธ์ถ์ด ๊ฐ๋ฅํ๋ค.

## ModelForm - widgets

> Django์ HTML input element๋ฅผ ํํํ๊ณ  HTML ๋ ๋๋ง์ ์ฒ๋ฆฌํ๊ธฐ ์ํด ์ฌ์ฉ

1. ์ฒซ๋ฒ์งธ ๋ฐฉ์

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

2. <u>**๋๋ฒ์งธ ๋ฐฉ์ (๊ถ์ฅ)**</u>

```python
from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):
    title = forms.CharField(
    	label = '์ ๋ชฉ',
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

##### `widgets ์ฌ์ฉ ์์`

```python
from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):
    title = forms.CharField(
        label = '์ ๋ชฉ',
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
            label = '๋ด์ฉ',
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

## Rendering fields manually ๋ฐฉ๋ฒ

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

- Bootstrap์ ํต์ฌ ํด๋์ค์ธ  `form-control` class๋ฅผ widget์ ์์ฑํ๋ค.

```python
from django import forms
from .models import Article

class ArticleForm(forms.ModelForm):
    title = forms.CharField(
        label = '์ ๋ชฉ',
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
            label = '๋ด์ฉ',
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

#### ์๋ฌ ๋ฉ์์ง with bootstrap alert ์ปดํฌ๋ํธ

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

- form class์ bootstrap์ ์ ์ฉ์์ผ์ฃผ๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ

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

- ํจํค์ง ๋ชฉ๋ก ์๋ฐ์ดํธ

```BASH
$ pip frreze > requirements.txt
```

#### ์ค์นํ ๋ผ์ด๋ธ๋ฌ๋ฆฌ ์ ์ฉํด๋ณด๊ธฐ

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

- ๊ธฐ์กด์ ๋ถํธ์คํธ๋ฉ `CDN`์ ๋ฃ์ด์ฃผ๋ ๊ณณ์ ์์ ํด์ฃผ๋ฉด ๋๋๋ฐ, `CDN` ์ฌ์ฉํ๊ณ  ์์ผ๋ฉด ๋ฐ๋ก ์ํด์ค๋ ๋๋๋ฏ!

#### ์์

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

