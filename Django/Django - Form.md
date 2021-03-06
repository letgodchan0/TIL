# π± Django - Form

> μ§κΈκΉμ§ HTML form, inputμ ν΅ν΄μ μ¬μ©μλ‘λΆν° λ°μ΄ν°λ₯Ό λ°μμλλ°, μ¬μ©μλ€μ΄ μλ ₯ν λ°μ΄ν°κ° κ°λ°μκ° μκ΅¬ν νμμ΄ μλ μ μμμ ν­μ μκ°ν΄μΌ νλ€. λ°λΌμ μ¬μ©μκ° μλ ₯ν λ°μ΄ν°λ₯Ό κ²μ¦νλ 'μ ν¨μ± κ²μ¦' κ³Όμ μ μ§ννκ³  νμμ μλ ₯λ λ°μ΄ν°λ₯Ό κ²μ¦ κ²°κ³Όμ ν¨κ» λ€μ νμν  μ μμ΄μΌ νλ€. 
>
> Djangoλ 'μ ν¨μ± κ²μ¦'μ κ΅¬ννλλ° κ³Όμ€ν μμκ³Ό λ°λ³΅ μ½λλ₯Ό μ€μ¬μ€μΌλ‘μ¨ μ΄ μμμ ν¨μ¬ μ½κ² λ§λ€μ΄ μ€λ€.

## Django`s forms

- `Form`μ `Django`μ μ ν¨μ± κ²μ¬ λκ΅¬ μ€ νλλ‘ μΈλΆμ μμμ  κ³΅κ²© λ° λ°μ΄ν° μμμ λν μ€μν λ°©μ΄ μλ¨

- `Django`λ `Form`κ³Ό κ΄λ ¨ν μ ν¨μ± κ²μ¬λ₯Ό λ¨μν νκ³  μλν ν  μ μλ κΈ°λ₯μ μ κ³΅νμ¬ κ°λ°μλ‘ νμ¬κΈ μ§μ  μμ±νλ μ½λλ³΄λ€ λ μμ νκ³  λΉ λ₯΄κ² μννλ μ½λλ₯Ό μμ±ν  μ μκ² νλ€.
- `Django`λ `form`μ κ΄λ ¨λ μμμ μλ μΈ λΆλΆμ μ²λ¦¬ν΄ μ€λ€.
  1. λ λλ§μ μν λ°μ΄ν° μ€λΉ λ° μ¬κ΅¬μ±
  2. λ°μ΄ν°μ λν HTML forms μμ±
  3. ν΄λΌμ΄μΈνΈλ‘λΆν° λ°μ λ°μ΄ν° μμ  λ° μ²λ¦¬

### `Form Class`

> Django Form κ΄λ¦¬ μμ€νμ ν΅μ¬μΌλ‘ Form λ΄ field, field λ°°μΉ, λμ€νλ μ΄, widget, label μ΄κΈ°κ°, μ ν¨νμ§ μλ fieldμ κ΄λ ¨λ μλ¬ λ©μΈμ§λ₯Ό κ²°μ 

```python
# app λ΄μ forms.py μμ±νμ¬ μ μΈν΄μ€λ€!
# articles/forms.py
from django import forms

class ArticleForm(form.Form):
    title = forms.CharField(max_length=10)
    content = forms.CharField()
```

- Modelμ μ μΈνλ κ²κ³Ό μ μ¬νλ©° κ°μ νλ νμμ μ¬μ© (λν, μΌλΆ λ§€κ°λ³μλ μ μ¬ν¨)
- `forms` λΌμ΄λΈλ¬μ΄μμ νμλ `Form` ν΄λμ€λ₯Ό μμλ°μ

### viewν¨μ μμ 

``` python
# κΈ°μ‘΄μ new ν¨μ
def new(request):
    return render(request, 'articles/new.html')
```

```python
# formμ μ μ©ν new ν¨μ
from boards.forms import ArticleForm # μΆκ°
def new(request):
    form = ArticleForm()
    context = {
        'form' : form
    }
    return render(request, 'articles/new.html', context)
```

### ννλ¦Ώμ μ μ©

```html
<!-- κΈ°μ‘΄ new.html -->

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
    <!-- λ°λ‘ μ λΆλΆκΉμ§ μλ‘μ΄ formμ ν΅ν΄ μλ ₯μ λ°λλ€! -->
    <input type="submit">
  </form>
  <a href="{% url 'articles:index' %}">back</a>
{% endblock content %}
```

```html
<!-- formμ μ μ©ν new.html -->

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

- λ¨μν  `{{ form.as_p}}`λ§ ν΄μ£Όμ΄λ μ₯κ³  λ΄λΆμ μΌλ‘ `forms.py`μ μμ±ν νλ νμλ€μ μλ ₯ λ°λλ‘ νκ³  μλ€. κΈ°μ‘΄μ μ°λ¦¬κ° μμ±ν΄ μ£Όμλ `label`μ `for`, `input`μ `id`, `name`λ±μ μμμ μ§μ μ ν΄μ£Όλ μ κΈ°ν μμ΄

## Form rendering options

> label & input μμ λν 3κ°μ§ μΆλ ₯ μ΅μ

1. `as_p()`
   - κ° νλκ° λ¨λ½ (`<p>` νκ·Έ)μΌλ‘ κ°μΈμ Έμ λ λλ§ λ¨
2. `as_ul()`
   - κ° νλκ° λͺ©λ‘ ν­λͺ©(`<li>` νκ·Έ)μΌλ‘ κ°μΈμ Έμ λ λλ§ λ¨
   - `<ul>` νκ·Έλ μ§μ  μμ±ν΄μΌ ν¨

3. `as_table()`
   - κ° νλκ° νμ΄λΈ(`<tr>` νκ·Έ)νμΌλ‘ κ°μΈμ Έμ λ λλ§ λ¨
   - `<table>` νκ·Έλ μ§μ  μμ±ν΄μΌ ν¨

## Djangoμ HTML input μμ νν λ°©λ² 2κ°μ§

1. `Form fields`

   - `input`μ λν μ ν¨μ± κ²μ¬ λ‘μ§μ μ²λ¦¬νλ©° ννλ¦Ώμμ μ§μ  μ¬μ© λ¨

2. `Widgets`

   > Form Fieldsμ νΌλλμ΄μλ μλλ€!! Form fieldsλ input μ ν¨μ± κ²μ¬λ₯Ό μ²λ¦¬νλ λ°λ©΄, widgetsμ μΉνμ΄μ μμ input elementμ λ¨μ rawν λ λλ§μ μ²λ¦¬νλ€.

   - μΉ νμ΄μ§μ HTML input μμ λ λλ§
   - GET/POST λμλλ¦¬μμ λ°μ΄ν° μΆμΆ
   - widgetsμ λ°λμ Form fieldsμ ν λΉ λ¨

```python
# articles/forms.py

from django import forms

class ArticleForm(form.Form):
    title = forms.CharField(max_length=10)
    # λͺ¨λΈμ TextFieldμ²λΌ λ΄μ©μ μλ ₯λ°μ λ λμ§ν μΉΈμ΄ νλ©΄μ λ λλ§ λλλ‘ ν¨
    content = forms.CharField(widget=forms.Textarea)
```

```python
# articles/forms.py

from django import forms

class ArticleForm(form.Form):
    # choiceFieldμ μ€νμΌ κ°μ΄λκ° λλ¬Έμμ, `sl` μ¬κΈ°λ valueκ°μΌλ‘ μ₯κ³ κ° λ°λ κ°
    REGION_A = 'sl'
    REGION_B = 'dj'
    REGION_C = 'gj'
    RESIGONS_CHOICES = [
        #(λ³μκ° (value κ°), μ€μ  μ¬μ©μνν μΆλ ₯λλ κ°)
        (REGION_A, 'μμΈ'),
        (REGION_B, 'λμ '),
        (REGION_C, 'κ΄μ£Ό')
    ]
    
    # ChoiceFieldλ htmlμμ selectμ ν΄λΉνλ κ²!
    region = forms.ChoiceField(widget=forms.Select, choices=RESIGONS_CHOICES)
    
    # ChoiceFiledμ κΈ°λ³Έκ°μ΄ selectμ¬μ widget μν΄μ€λ λλ€
    # region = forms.ChoiceField(choices=RESIGONS_CHOICES)
```

![image-20220408013228507](Django%20-%20Form.assets/image-20220408013228507.png)