# ๐ฑ Django - CRUD

## ORM

>๊ฐ์ฒด ์งํฅ ํ๋ก๊ทธ๋๋ฐ ์ธ์ด๋ฅผ ์ฌ์ฉํ์ฌ ํธํ๋์ง ์๋ ์ ํ์ ์์คํ ๊ฐ(Django - SQL) ๋ฐ์ดํฐ๋ฅผ ๋ณํํ๋ ํ๋ก๊ทธ๋๋ฐ ๊ธฐ์ 
>
>OOP ํ๋ก๊ทธ๋๋ฐ์์ RDBMS๋ฅผ ์ฐ๋ํ  ๋, DB์ ๊ฐ์ฒด ์งํฅ ํ๋ก๊ทธ๋๋ฐ ์ธ์ด ๊ฐ์ ํธํ๋์ง ์๋ ๋ฐ์ดํฐ๋ฅผ ๋ณํํ๋ ํ๋ก๊ทธ๋๋ฐ ๊ธฐ๋ฒ์ผ๋ก django๋ ๋ด์ฅ orm์ ์ฌ์ฉํ๋ค!
>
>ํ๋ง๋๋ก ํ์ด์ฌ์ผ๋ก DB ์กฐ์์ ์ด๋์ ๋ ๊ฐ๋ฅํ๊ฒ ํด์ค๋ค!

## DB API

> "DB๋ฅผ ์กฐ์ํ๊ธฐ ์ํ ๋๊ตฌ"
>
> Django๊ฐ ๊ธฐ๋ณธ์ ์ผ๋ก ORM์ ์ ๊ณตํจ์ ๋ฐ๋ฅธ ๊ฒ์ผ๋ก DB๋ฅผ ํธํ๊ฒ ์กฐ์ํ  ์ ์๋๋ก ๋์
>
> Model์ ๋ง๋ค๋ฉด Django๋ ๊ฐ์ฒด๋ค์ ๋ง๋ค๊ณ  ์ฝ๊ณ  ์์ ํ๊ณ  ์ง์ธ ์ ์๋ db-abstract API๋ฅผ ์๋์ผ๋ก ๋ง๋ ๋ค!

```python
Article.objects.all()
# ํด๋์ค ์ด๋ฆ, Manager, QuerySet API 
```

- Manager
  - Django ๋ชจ๋ธ์ ๋ฐ์ดํฐ๋ฒ ์ด์ค query ์์์ด ์ ๊ณต๋๋ ์ธํฐํ์ด์ค
  - ๊ธฐ๋ณธ์ ์ผ๋ก ๋ชจ๋  Django ๋ชจ๋ธ ํด๋์ค์ objects๋ผ๋ Manager๋ฅผ ์ถ๊ฐ
- QuerySet
  - DB๋ก๋ถํฐ ์ ๋ฌ๋ฐ์ ๊ฐ์ฒด ๋ชฉ๋ก
  - queryset ์์ ๊ฐ์ฒด๋ 0๊ฐ, 1๊ฐ ํน์ ์ฌ๋ฌ ๊ฐ์ผ ์ ์์
  - DB๋ก๋ถํฐ ์กฐํ, ํํฐ, ์ ๋ ฌ ๋ฑ์ ์ํํ  ์ ์์

## Django shell

> ์ผ๋ฐ Python shell์ ํตํด์๋ ์ฅ๊ณ  ํ๋ก์ ํธ ํ๊ฒฝ์ ์ ๊ทผํ  ์ ์์!!!
>
> ๊ทธ๋์ ์ฅ๊ณ  ํ๋ก์ ํธ ์ค์ ์ด load ๋ Python shell์ ํ์ฉํด DB API ๊ตฌ๋ฌธ ํ์คํธ๋ฅผ ์งํํ๋ค
>
> ๊ธฐ๋ณธ Django shell ๋ณด๋ค ๋ ๋ง์ ๊ธฐ๋ฅ์ ์ ๊ณตํ๋ shell_plus๋ฅผ ์ฌ์ฉํด์ ์งํํ๋ค!
>
> - Django-extensions ๋ผ์ด๋ธ๋ฌ๋ฆฌ์ ๊ธฐ๋ฅ ์ค ํ๋

```bash
# ๋ผ์ด๋ธ๋ฌ๋ฆฌ ์ค์น
$ pip install ipython
$ pip install django-extensions
```

```python
# settings.py์ ๋ฑ๋ก
INSTALLED_APPS = [
    ....,
    'django_extensions',		# ์ธ๋๋ฐ _ ์ฃผ์!!
    .....,
]
```

```bash
# shell_plus ์คํ!
$ python manage.py shell_plus

# DB์ ์ธ์คํด์ค ๊ฐ์ฒด๋ฅผ ์ป๊ธฐ ์ํ ํด๋ฆฌ๋ฌธ ๋ ๋ฆฌ๊ธฐ 
# ๋ ์ฝ๋๊ฐ ํ๋๋ง ์์ผ๋ฉด ์ธ์คํด์ค ๊ฐ์ฒด๋ก, ๋ ๊ฐ ์ด์์ด๋ฉด ์ฟผ๋ฆฌ์์ผ๋ก ๋ฆฌํด!
# ์ ์ฒด ๊ฐ์ฒด ์กฐํ
>>> Article.objects.all()
<QuerySet []>
```

## CRUD - Create (์์ฑ)

```shell
# Article(class)๋ก๋ถํฐ article(instance)
>>> article = Article()

# ์ธ์คํด์ค ๋ณ์(title, content)์ ๊ฐ์ ํ ๋น
>>> article.title = '์ ๋ชฉ'
>>> article.content = '๋ด์ฉ์๋๋ค'

# save๋ฅผ ํ์ง ์์ผ๋ฉด ์์ง DB์ ๊ฐ์ด ์ ์ฅ๋์ง ์์
>>> article
>>> <Article: Article object (None)>

# save๋ฅผ ํ๋ฉด ์ ์ฅ๋ ๊ฐ์ ํ์ธ ํ  ์ ์์
>>> article.save()
>>> article
<Article: Article object (1)>
>> Article.objects.all()
<QuerySet [Article: Article object (1)]>

# ์ธ์คํด์ค๋ฅผ ํ์ฉํ์ฌ ๋ณ์์ ์ ๊ทผํด๋ณด๊ธฐ
>>> article.title
'์ ๋ชฉ'
>>> article.content
'๋ด์ฉ์๋๋ค'
>>> article.created_at
datetime.datetime(2022, 3, 10, 2, 43, 56, 49345, tzinfo=<UTC>)
```

```bash
# ๋ค๋ฅธ ๋ฐฉ๋ฒ์ผ๋ก ์์ฑ ํ ์ ์ฅ

>>> a2 = Article(title='2๋ฒ๊ธ', content='2๋ฒ ๋ด์ฉ')
>>> a2.save()

>>> Article.objects.all()
>>> <QuerySet [<Article: Article object (1)>, <Article: Article object (2)>]>
```

```bash
# ์ ์ฅ๋ฐ๋ก ํ๋๊ฑฐ ๊ท์ฐฎ๋ค ํ๋ฒ์ ํด์ค
# ์๋ ๋ณ๋๋ก save ์ํด๋ ๋๋ค. ํด๋์ค์ ์ง์  ์์ฑํ๋ ๊ฑฐ๋๊น!

Article.objects.create(title='3๋ฒ๊ธ', content='3๋ฒ ๋ด์ฉ')
```



## CRUD - Read (์ฝ๊ธฐ)

### `all()` - ์ ์ฒด ๋ฐ์ดํฐ ์กฐํ, 

- ํ์ฌ QuerySet์ ๋ณต์ฌ๋ณธ์ ๋ฐํ

- ๋น์ด ์์ผ๋ฉด ๋น์ด ์๋ ๊ทธ๋๋ก ๋ฐํ

```bash
>>> Article.objects.all()
<QuerySet [<Article: Article object (1)>, <Article: Article object (2)>, <Article: Article object (3)>]>
```

### `get` - ๋จ์ผ ๋ฐ์ดํฐ ์กฐํ

- ์ผ๋ฐ์ ์ผ๋ก ๊ณ ์ ํ ๊ฐ(unique)์ธ ๊ฒฝ์ฐ ์ฌ์ฉํ๋ ๋ฉ์๋

  - ์๋ ๋ฐ์ดํฐ์ผ ๋ ์๋ฌ ๋ฐ์

  - ์ฌ๋ฌ๊ฐ ์๋ ๊ฒฝ์ฐ ์๋ฌ ๋ฐ์

- `pk`๋ ๋ฐ์ดํฐ๋ฒ ์ด์ค์์ ์ ์ผํ ๊ฐ์ด๋ฏ๋ก `pk`๋ฅผ ๊ธฐ์ค์ผ๋ก ์กฐํํ  ๋ ์ฌ์ฉ

```bash
# pk๋ฅผ ๊ธฐ์ค์ผ๋ก ์กฐํ
>>> Article.objects.get(pk=1)
<Article: Article object (1)>
```

```bash
# ์๋ ๋ฐ์ดํฐ => ์๋ฌ
>>> Article.objects.get(pk=100)
DoesNotExist: Article matching query does not exist.
    
# ์ฌ๋ฌ๊ฐ ์๋ ๊ฒฝ์ฐ => ์๋ฌ
>>> Article.objects.get(title='์ ๋ชฉ')
```

### `filter` - ์ฌ๋ฌ ๋ฐ์ดํฐ ์กฐํ

```bash
# title์ด ์ ๋ชฉ์ธ ์ ๋ค ๋ค ๋ฐํ
>>> Article.object.filter(title='์ ๋ชฉ')

Article.object.filter(title='์ฐพ์๋ด')[0].content
'๋ด์ฉ์๋๋ค.'
```



## CRUD - Update

```bash
>>> a2 = Article.objects.get(pk=2)
>>> a2.title = '๋ณ๊ฒฝ๋ ์ ๋ชฉ'
>>> a2.save()
```

## CRUD - Delete

- QuerySet์ ๋ชจ๋  ํ์ ๋ํด SQL ์ญ์  ์ฟผ๋ฆฌ๋ฅผ ์ํํ๊ณ  ์ญ์ ๋ ๊ฐ์ฒด ์์ ๊ฐ์ฒด ์ ํ๋น ์ญ์  ์๊ฐ ํฌํจ๋ ๋์๋๋ฆฌ ๋ฐํ

```bash
# ์ญ์ ๋ ์ ์ฅ์ ์ํด๋ ๋๋ค!!
>>> a3 = Article.objects.get(pk=3)
>>> a2.delete()
```

## HTTP method

### `GET`

- ํน์  ๋ฆฌ์์ค๋ฅผ ๊ฐ์ ธ์ค๋๋ก ์์ฒญํ  ๋ ์ฌ์ฉ
- ๋ฐ๋์ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์ฌ ๋๋ง ์ฌ์ฉํด์ผ ํ๋ค!!!
- DB์ ๋ณํ๋ฅผ ์ฃผ์ง ์๊ณ , `CRUD`์์ `R` ์ญํ ์ ๋ด๋นํ๋ค

### `POST`

- ์๋ฒ๋ก ๋ฐ์ดํฐ๋ฅผ ์ ์กํ  ๋ ์ฌ์ฉํ๊ณ  ๋ฆฌ์์ค๋ฅผ ์์ฑ/๋ณ๊ฒฝํ๊ธฐ ์ํด ๋ฐ์ดํฐ๋ฅผ HTTP body์ ๋ด์ ์ ์กํ๋ค
- ์๋ฒ์ ๋ณ๊ฒฝ์ฌํญ์ ๋ง๋ค๊ณ , `CRUD`์์ `C/U/D` ์ญํ ์ ๋ด๋นํ๋ค
- `GET`์ ์ฌ์ฉํ๋ฉด `/article/create/?title=์ ๋ชฉ&content=๋ด์ฉ` ์ ํํ๋ก ์๋ฒ ๋ก๊ทธ์ ๋ชจ๋  ์ ๋ณด๊ฐ ์ฃผ์๋ก์ ๋ํ๋๊ฒ ๋๋ค. ๋ฐ๋ผ์ ์ ๋ณด๋ฅผ ์จ๊ฒจ ๋ณด๋ด๊ธฐ ์ํด `form` ํ๊ทธ์ `method` ์์ฑ์ ๊ฐ์ `GET`์ด ์๋ `POST`๋ก ๋ณ๊ฒฝํด์ผ ํ๋ค. ์ผ๋ฐ์ ์ผ๋ก ๋ณด์์ด ์๊ตฌ๋๋ ๊ฒ์ ์์ฒญํ  ๋๋ `POST` ํ์์ผ๋ก ์์ฒญ์ ๋ฐ์์ผ ํ๋ค!

```html
# edit.html

<form action="{% url 'articles:update' article.pk %}" method="POST">
    {% csrf_token %}
    ....
</form>
```

```python
# articles/views.py

def update(request, pk):
    # 1. ๊ฐ ๊ฐ์ ธ์ค๊ธฐ
    title = request.POST.get('title')
    content = request.POST.get('content')
   	....
    return redirect('articles:detail', article.pk)
```

### `CSRF`

- '์ฌ์ดํธ ๊ฐ ์์ฒญ ์์กฐ' ๋ผ๋ ๊ณต๊ฒฉ ๋ฐฉ๋ฒ ์ค ํ๋๋ก Django๋ ๊ธฐ๋ณธ์ ์ผ๋ก ์ด ๊ณต๊ฒฉ์ ๋ฐฉ์งํ  ์ ์๋ `CSRF token` ํฌํ๋ฆฟ์ ์ ๊ณตํ๋ค.
- ์ฌ์ฉ์์ ๋ฐ์ดํฐ์ ์์์ ๋์ ๊ฐ์ ๋ถ์ฌํด, ๋งค ์์ฒญ๋ง๋ค ํด๋น ๋์ ๊ฐ์ ํฌํจ์์ผ ์ ์ก์ํค๋๋ก ํ๊ณ  ์ดํ ์๋ฒ์์ ์์ฒญ์ ๋ฐ์ ๋๋ง๋ค ์ ๋ฌ๋ token ๊ฐ์ด ์ ํจํ์ง ๊ฒ์ฆํ๋ค.
- ์ผ๋ฐ์ ์ผ๋ก ๋ฐ์ดํฐ ๋ณ๊ฒฝ์ด ๊ฐ๋ฅํ POST, PATCH, DELETE Method ๋ฑ์ ์ ์ฉ ํ๋ค (GET ์ ์ธ)

```html
<form action="#" method="POST">
    {% csrf_token %}
    ....
</form>{}
```

- input type์ด hidden์ผ๋ก ์์ฑ๋๋ฉฐ value๋ Django์์ ์์ฑํ hash ๊ฐ์ผ๋ก ์ค์ ๋๋ค.
- ํด๋น ํ๊ทธ ์์ด ์์ฒญ์ ๋ณด๋ด๋ฉด Django ์๋ฒ๋ 403 forbidden์ ์๋ตํ๋ค.





