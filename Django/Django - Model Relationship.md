# ๐ฑDjango - Model Relationship 

## Foreign Key ๊ฐ๋

- ์ธ๋ ํค (์ธ๋ถ ํค)
- ๊ด๊ณํ ๋ฐ์ดํฐ๋ฒ ์ด์ค์์ ํ ํ์ด๋ธ์ ํ๋ ์ค ๋ค๋ฅธ ํ์ด๋ธ์ ํ์ ์๋ณํ  ์ ์๋ ํค
- ์ฐธ์กฐํ๋ ํ์ด๋ธ์์ ์์ฑ(ํ๋)์ ํด๋นํ๊ณ , ์ด๋ ์ฐธ์กฐ๋๋ ํ์ด๋ธ์ ๊ธฐ๋ณธ ํค(Primary Key)๋ฅผ ๊ฐ๋ฆฌํด
- ์ฐธ์กฐํ๋ ํ์ด๋ธ์ ์ธ๋ ํค๋ ์ฐธ์กฐ๋๋ ํ์ด๋ธ ํ 1๊ฐ์ ๋์๋จ
  - ์ด ๋๋ฌธ์ ์ฐธ์กฐํ๋ ํ์ด๋ธ์์ ์ฐธ์กฐ๋๋ ํ์ด๋ธ์ ์กด์ฌํ์ง ์๋ ํ์ ์ฐธ์กฐํ  ์ ์์
- ์ฐธ์กฐํ๋ ํ์ด๋ธ์ ํ ์ฌ๋ฌ ๊ฐ๊ฐ ์ฐธ์กฐ๋๋ ํ์ด๋ธ์ ๋์ผํ ํ์ ์ฐธ์กฐํ  ์ ์์ 

![image-20220413091841736](Django%20-%20Model%20Relationship.assets/image-20220413091841736.png)



## Foreign Key ํน์ง

- ํค๋ฅผ ์ฌ์ฉํ์ฌ ๋ถ๋ชจ ํ์ด๋ธ (์ฐธ์กฐ ๋๋ ํ์ด๋ธ)์ ์ ์ผํ ๊ฐ์ ์ฐธ์กฐ (์ฐธ์กฐ ๋ฌด๊ฒฐ์ฑ)
  - [์ฐธ๊ณ ] ์ฐธ์กฐ ๋ฌด๊ฒฐ์ฑ
  - ๋ฐ์ดํฐ๋ฒ ์ด์ค ๊ด๊ณ ๋ชจ๋ธ์์ ๊ด๋ จ๋ 2๊ฐ์ ํ์ด๋ธ ๊ฐ์ ์ผ๊ด์ฑ์ ๋งํจ
  - ์ธ๋ ํค๊ฐ ์ ์ธ๋ ํ์ด๋ธ์ ์ธ๋ ํค ์์ฑ(์ด)์ ๊ฐ์ ๊ทธ ํ์ด๋ธ์ ๋ถ๋ชจ๊ฐ ๋๋ ํ์ด๋ธ์ ๊ธฐ๋ณธ ํค ๊ฐ์ผ๋ก ์กด์ฌํด์ผ ํจ
- ์ธ๋ ํค์ ๊ฐ์ด ๋ฐ๋์ ๋ถ๋ชจ ํ์ด๋ธ์ ๊ธฐ๋ณธ ํค์ผ ํ์๋ ์์ง๋ง ์ ์ผํ ๊ฐ์ด์ด์ผ ํจ

## ForeignKey field

- A many-to-one relationship
- 2๊ฐ์ ์์น ์ธ์๊ฐ ๋ฐ๋์ ํ์
  1. ์ฐธ์กฐํ๋ model class
  2. on_delete ์ต์
- migrate ์์ ์ ํ๋ ์ด๋ฆ์ `_id`๋ฅผ ์ถ๊ฐํ์ฌ ๋ฐ์ดํฐ๋ฒ ์ด์ค ์ด ์ด๋ฆ์ ๋ง๋ฆ

- [์ฐธ๊ณ ] ์ฌ๊ท ๊ด๊ณ (์์ ๊ณผ 1:N)

```python
models.ForeignKey('self', on_delete=models.CASCADE)
```

- comment ๋ชจ๋ธ ์ ์ ํ๊ธฐ

```python
# articles/models.py

class Comment(models.Model):
    # ์ฐธ์กฐํ๋ ํ์ด๋ธ์ ์๋ฌธ์ ๋จ์ํ, db ์ปฌ๋ผ์ article_id๋ก ์์ฑ๋๋ค
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    def __str__(self):
        return self.content
```

### ForeignKey arguments - 'on_delete'

- ์ธ๋ ํค๊ฐ ์ฐธ์กฐํ๋ ๊ฐ์ฒด๊ฐ ์ฌ๋ผ์ก์ ๋ ์ธ๋ ํค๋ฅผ ๊ฐ์ง ๊ฐ์ฒด๋ฅผ ์ด๋ป๊ฒ ์ฒ๋ฆฌํ  ์ง๋ฅผ ์ ์
- Database Integrity(๋ฐ์ดํฐ ๋ฌด๊ฒฐ์ฑ)์ ์ํด์ ๋งค์ฐ ์ค์ํ ์ค์ 
- on_delete ์ต์์ ์ฌ์ฉ ๊ฐ๋ฅํ ๊ฐ๋ค
  - CASCADE : ๋ถ๋ชจ ๊ฐ์ฒด(์ฐธ์กฐ ๋ ๊ฐ์ฒด)๊ฐ ์ญ์  ๋์ ๋ ์ด๋ฅผ ์ฐธ์กฐํ๋ ๊ฐ์ฒด๋ ์ญ์  
  - PROTECT
  - SET_NULL
  - SET_DEFAULT
  - SET()
  - DO_NOTHING
  - RESTRICT

### [์ฐธ๊ณ ] ๋ฐ์ดํฐ ๋ฌด๊ฒฐ์ฑ

- ๋ฐ์ดํฐ์ ์ ํ์ฑ๊ณผ ์ผ๊ด์ฑ์ ์ ์งํ๊ณ  ๋ณด์ฆํ๋ ๊ฒ์ ๊ฐ๋ฆฌํค๋ฉฐ, ๋ฐ์ดํฐ๋ฒ ์ด์ค๋ RDBMS ์์คํ์ ์ค์ํ ๊ธฐ๋ฅ์
- ๋ฌด๊ฒฐ์ฑ ์ ํ์ ์ ํ
  1. ๊ฐ์ฒด ๋ฌด๊ฒฐ์ฑ (Entity integrity)
     - PK์ ๊ฐ๋๊ณผ ๊ด๋ จ
     - ๋ชจ๋  ํ์ด๋ธ์ด PK๋ฅผ ๊ฐ์ ธ์ผ ํ๋ฉฐ PK๋ก ์ ํ๋ ์ด์ ๊ณ ์ ํ ๊ฐ์ด์ด์ผ ํ๊ณ  ๋น ๊ฐ์ ํ์ฉ์น ์์์ ๊ท์ 
  2. ์ฐธ์กฐ ๋ฌด๊ฒฐ์ฑ (Referential integrity)
     - FK(์ธ๋ ํค) ๊ฐ๋๊ณผ ๊ด๋ จ
     - FK ๊ฐ์ด ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ํน์  ํ์ด๋ธ์ PK ๊ฐ์ ์ฐธ์กฐํ๋ ๊ฒ
  3. ๋ฒ์(๋๋ฉ์ธ) ๋ฌด๊ฒฐ์ฑ (Domain integrity)
     - ์ ์๋ ํ์(๋ฒ์)์์ ๊ด๊ณํ ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ๋ชจ๋  ์ปฌ๋ผ์ด ์ ์ธ๋๋๋ก ๊ท์ 

### Migration

1. ```bash
   $ python manage.py makemigrations
   ```

2. ![image-20220413093519088](Django%20-%20Model%20Relationship.assets/image-20220413093519088.png)

3. ```bash
   $ python manage.py migrate
   ```

4. `articles_comment` ํ์ด๋ธ์ ์ธ๋ ํค ์ปฌ๋ผ ํ์ธ (ํ๋ ์ด๋ฆ์ _id๊ฐ ์ถ๊ฐ๋จ)

![image-20220413093637878](Django%20-%20Model%20Relationship.assets/image-20220413093637878.png)



### ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ForeignKey ํํ

- ๋ง์ฝ ForeginKey ์ธ์คํด์ค๋ฅผ abcd๋ก ์์ฑํ๋ค๋ฉด abcd_id๋ก ๋ง๋ค์ด์ง

- ํ์ง๋ง ๋ช์์ ์ธ ๋ชจ๋ธ ๊ด๊ณ ํ์์ ์ํด ์ฐธ์กฐํ๋ ํด๋์ค ์ด๋ฆ์ ์๋ฌธ์(๋จ์ํ)๋ก ์์ฑํ๋ ๊ฒ์ด ๋ฐ๋์งํจ (1:N)

- | id   | content | article_id |
  | ---- | ------- | ---------- |
  | 1    | ๋๊ธ 1  | 3          |
  | 2    | ๋๊ธ 2  | 1          |
  | 3    | ๋๊ธ 3  | 1          |



## ๋๊ธ ์์ฑ ์ฐ์ตํ๊ธฐ

1. ```bash
   $python manage.py shell_plus
   ```

2. ```shell
   comment = Comment()
   comment.content = 'first comment'
   comment.save()
   ```

   ![image-20220413094134268](Django%20-%20Model%20Relationship.assets/image-20220413094134268.png)

   - articles_comment ํ์ด๋ธ์ ForeignKey, article_id ๊ฐ์ด ๋๋ฝ๋์๊ธฐ ๋๋ฌธ์ ์๋ฌ ๋ฐ์ 
   - ๋ฐ๋ผ์ ๊ฒ์๊ธ์ ์์ฑ ํ ๋๊ธ ์์ฑ ์ฌ์๋๋ฅผ ํด์ผ ํ๋ค.

3. ```shell
   # ๊ฒ์๊ธ ์์ฑ
   article = Article.objects.create(title='title', content='content')
   
   # 1๋ฒ ๊ฒ์๊ธ์ article๋ก ๋ด๊ธฐ
   article = Article.objects.get(pk=1)
   
   article
   <Article: title>
   
   comment.article = article
   
   comment.save()
   
   comment
   <Comment: first comment>
   
   comment.pk
   1
   ```

   - ๋๊ธ ์์ฑ ๊ฐ ํ์ธ

     - ์ค์ ๋ก ์์ฑ๋ ์ธ๋ ํค ์ปฌ๋ผ๋ช์ `article_id`์ด๊ธฐ ๋๋ฌธ์ `article_pk`๋ก๋ ๊ฐ์ ์ ๊ทผํ  ์ ์์

     ```shell
     comment.content
     'first comment'
     
     comment.article_id
     1
     
     comment.article
     <Article: title>
     ```

     - comment ์ธ์คํด์ค๋ฅผ ํตํ article ๊ฐ ์ ๊ทผ

     ```shell
     comment.article.pk
     1
     # ๊ฒ์๊ธ ๋ด์ฉ
     comment.article.content
     'content'
     ```

4. ๋๋ฒ์งธ ๋๊ธ ์์ฑ ํ๊ธฐ

```shell
# ํ์ฌ article์ pk๊ฐ 1, ์ฆ ์ฒซ๋ฒ์งธ ๊ฒ์๊ธ์ ๋ด๊ณ  ์์
comment = Comment(content='second comment', article=article)

comment.save()

# ๋๊ธ pk
comment.pk
2

# ๊ฒ์๊ธ pk
comment.article_id
1
```

#### admin site์ ์์ฑ๋ ๋๊ธ ํ์ธ

```python
# articles/admin.py

from .models import Article, Comment

admin.site.register(Comment)
```

```bash
# admin ๊ณ์  ์์ฑ
$ python manage.py createsuperuser
```



## 1:N ๊ด๊ณ related manager

- ์ญ์ฐธ์กฐ('comment_set')
  - Article(1) -> Comment(N)
  - article.comment ํํ๋ก๋ ์ฌ์ฉํ  ์ ์๊ณ , article.comment_set manager๊ฐ ์์ฑ๋จ, (Article ํ์ด๋ธ์๋ comment๊ฐ ์กด์ฌํ์ง ์๊ธฐ ๋๋ฌธ)
  - ๊ฒ์๊ธ์ ๋ช ๊ฐ์ ๋๊ธ์ด ์์ฑ ๋์๋์ง Django ORM์ด ๋ณด์ฅํ  ์ ์๊ธฐ ๋๋ฌธ
    - article์ comment๊ฐ ์์ ์๋ ์๊ณ , ์์ ์๋ ์์
    - ์ค์ ๋ก Article ํด๋์ค์๋ Comment์์ ์ด๋ ํ ๊ด๊ณ๋ ์์ฑ๋์ด ์์ง ์์
- ์ฐธ์กฐ('article')
  - Comment(N) -> Article(1)
  - ๋๊ธ์ ๊ฒฝ์ฐ ์ด๋ ํ ๋๊ธ์ด๋  ๋ฐ๋์ ์์ ์ด ์ฐธ์กฐํ๊ณ  ์๋ ๊ฒ์๊ธ์ด ์์ผ๋ฏ๋ก, comment.article๊ณผ ๊ฐ์ด ์ ๊ทผํ  ์ ์์
  - ์ค์  ForeignKey ๋ํ Comment ํด๋์ค์์ ์์ฑ๋จ

### 1:N related manager ์ฐ์ตํ๊ธฐ

```shell
article = Article.objects.get(pk=1)
dir(article)
```

- `dir()` ํจ์๋ฅผ ํตํด article ์ธ์คํด์ค๊ฐ ์ฌ์ฉํ  ์ ์๋ ๋ชจ๋  ์์ฑ, ๋ฉ์๋๋ฅผ ์ง์  ํ์ธํ๊ธฐ

- ๋ฉ์๋ ์ค `comment_set`์ด ์ญ์ฐธ์กฐ๋ฅผ ํด์ฃผ๋ ๋ฉ์๋

![image-20220413101318886](Django%20-%20Model%20Relationship.assets/image-20220413101318886.png)



- article์ ์์ฅ์์ ๋ชจ๋  ๋๊ธ ์กฐํํ๊ธฐ (์ญ์ฐธ์กฐ, 1 -> N)

```shell
article.comment_set.all()
<QuerySet [<Comment: first comment>, <Comment: second comment>]>
```

- ์กฐํํ ๋ชจ๋  ๋๊ธ ์ถ๋ ฅํ๊ธฐ

```shell
comments = article.comment_set.all()

for comment in comments:
	print(comment.content)
	
first comment
second comment
```

- comment์ ์์ฅ์์ ์ฐธ์กฐํ๋ ๊ฒ์๊ธ ์กฐํํ๊ธฐ (์ฐธ์กฐ, N -> 1)

```shell
comment = Comment.objects.get(pk=1)

comment.article
<Article: title>

comment.article.content
'content'

comment.article_id
1
```



### ForeignKey arguments - 'related_name'

![image-20220413102103663](Django%20-%20Model%20Relationship.assets/image-20220413102103663.png)

- ์ญ์ฐธ์กฐ ์ ์ฌ์ฉํ  ์ด๋ฆ('model_set' manager)์ ๋ณ๊ฒฝํ  ์ ์๋ ์ต์
- ์์ ๊ฐ์ด ๋ณ๊ฒฝํ๋ฉด `article.comment_set`์ ๋์ด์ ์ฌ์ฉํ  ์ ์๊ณ , `article.comments`๋ก ๋์ฒด ๋จ
- [์ฃผ์] ์ญ์ฐธ์กฐ ์ ์ฌ์ฉํ  ์ด๋ฆ ์์  ํ, migration ๊ณผ์  ํ์

- ๊ทธ๋ฐ๋ฐ 1:N ๊ด๊ณ์์๋ ์ด๋ฆ์ ๋ฐ๊พธ๋ ๊ฒ์ ๊ถ์ฅํ์ง ์์. M:N ๊ด๊ณ์์๋ ํ์์ ์ผ๋ก ์ด๋ฆ์ ๋ฐ๊ฟ์ผ ํ  ์ํฉ์ด ์๊ธฐ๊ธฐ ๋๋ฌธ์ ํท๊ฐ๋ฆฌ์ง ์๊ธฐ ์ํด!

## Comment CREATE

```python
# articles/forms.py

from .models import Article, Comment

class CommentForm(forms.ModelForm):
    
    class Meta:
        model = Comment
        fields = '__all__'
```

> detail ํ์ด์ง์์ CommentForm ์ถ๋ ฅ

```python
# articles/views.py
from .forms import ArticleForm, CommentForm

@require_safe
def detail(request, pk):
    article = get_object_or_404(Article, pk=pk)
    comment_form = CommentForm()
 
    context = {
        'article': article,
        'comment_form' : comment_form,
    }
    return render(request, 'articles/detail.html', context)
```

```html
<!-- articles/detail.html -->
{% extends 'base.html' %}

{% block content %}
...
  <a href="{% url 'articles:index' %}">back</a>
  <hr>
  <form action="" method="POST">
  	{% csrf_token %}
    {{ comment_form }}
    <input type="submit">
  </form>
{% endblock %}
```

![image-20220413195637461](Django%20-%20Model%20Relationship.assets/image-20220413195637461.png)

- ๋ฐ๋ผ์ `CommentForm`์์ ์ธ๋ ํค ํ๋ ์ถ๋ ฅ์ ์ ์ธํด์ผ ํ๋ค

```python
# articles/forms.py

class CommentForm(forms.ModelForm):
    
    class Meta:
        model = Comment
        exclude = ('article',)
```

- exclude๋ก article์ ํด์ค๋ค๋ ์๋ฏธ๋ ๋จ์ํ ํ๋ฉด์์ ํด๋น ํ๋๋ฅผ ๋ณด์ฌ์ฃผ์ง ์๋ ๊ฒ์ ์๋ฏธํ๋ค. ๊ทธ๋ฐ๋ฐ ์ฌ์ ํ ์๋ฌ๊ฐ ๋๋ ์ด์ ๋, CommentForm์ ModelForm์ ์์ํ๊ณ  Article์ ์ฐธ์กฐํ๊ธฐ ๋๋ฌธ์ ํด๋น ์ธ๋ํค์ ๋ํ ์๋ ฅ์ ํ์์ ์ผ๋ก ์๊ตฌํ๋๋ฐ, exclude๋ก ์๋ ฅํ๋ ๊ณณ์ ์ง์๋ฒ๋ ธ๊ธฐ ๋๋ฌธ์ ์๋ฌ๊ฐ ๋ฐ์ํ๋ค.  
- ๋ฐ๋ผ์ ModelForm์ save์ ๋ฉ์๋๋ฅผ ํ์ฉํ๋ค. 

### ๋๊ธ ๊ธฐ๋ฅ ๊ฐ๋ฐ ๋ก์ง

```python
# articles/urls.py

app_name = 'articles'
urlpatterns = [
    ...
    path('<int:pk>/comments/', views.comments_create, name='comments_create'),
]
```

```python
# articles/views.py

@require_POST
def comment_create(request, pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=pk)
        comment_form = CommentForm(request.POST)
        if comment_form.is_valid():
            comment = comment_form.save(commit=False)
            comment.article = article
            comment.user = request.user
            comment.save()
        return redirect('articles:detail', article.pk)
    return redirect('accounts:login')
```

1. `request.user.is_authenticated` : ํ์ฌ ์์ฒญ์ ๋ณด๋ธ ์ฌ์ฉ์๊ฐ ๊ถํ์ด ์๋ ์ฌ์ฉ์๋ผ๋ฉด,

2. `article = get_object_or_404(Article, pk=pk)`: ํ์ฌ pk์ ํด๋นํ๋ ๊ฐ์ฒด๋ฅผ article์ ๋ด์

3. `comment_form = CommentForm(request.POST)`: ๋๊ธ ๋ฐ์ดํฐ(?)๋ฅผ `comment_form`์ ๋ด์

4. `if comment_form.is_valid()` : ๋๊ธ ๋ฐ์ดํฐ๊ฐ ์ ํจ์ฑ ๊ฒ์ฌ๋ฅผ ํต๊ณผํ๋ฉด

5. `comment = comment_form.save(commit=False)` : ํ์ฌ ์๋ ฅ๋ฐ์ ๋๊ธ ๋ฐ์ดํฐ๋ฅผ `db`์ ์ ์ฅํ๊ธฐ ์ํด์๋ ์ธ๋ํค์ ์ ๋ณด๊ฐ ์ถ๊ฐ์ ์ผ๋ก ํ์ํ๋ค. ๋ฐ๋ผ์ `save` ๋ฉ์๋์ ์ต์์ธ `commit=False`๋ฅผ ํด์ค๋ค๋ฉด, ์์ง `db`์ ์ ์ฅ์ ํ์ง ์๊ณ  ๊ฐ์ฒด๋ง ๋ฐํํ๋ค. 

   "์ฆ, commit์ด false๋ผ๋ฉด db๊ฐ ์ฐ๋ฆฌ์๊ฒ ๊ธฐํ๋ฅผ ์ฃผ๋๊ฑฐ๋ผ๊ณ  ์๊ฐํ์! ๋ ์ง๊ธ ์ ๋ฌํ ๋ฐ์ดํฐ ๋ญ๊ฐ ๋ถ์กฑํด!! ์ถ๊ฐํด์ผํ  ๊ฒ ์๋ ๊ฒ ๊ฐ์๋ฐ, ์ผ๋จ ์ ์ฅ์ํ๊ณ  ์ธ์คํด์ค ๋ง๋ค์ด ์คํ๋๊น ๋นจ๋ฆฌ ์ถ๊ฐ ์์ํด ๋ผ๊ณ  ๋งํด์ฃผ๋ ๊ฑฐ๋ผ๊ณ  ์๊ฐํ์!"

6. `comment.article = article` : `comment_article_id` ํ๋์ ๊ฐ์ `article` ์ pk๋ก ํ ๋นํ๋ค.

7. `comment.user = request.user` : `comment` ํ์ด๋ธ์ user๋ฅผ ์์ฒญ์ ๋ณด๋ธ user๋ก ํ ๋น, ์์ฑ์ ์ ๋ณด๊ฐ ๋ฐ๋ก ์๋ ฅ ๋ฐ์ ์ ์๋ ๊ฒฝ์ฐ๊ฐ ์กด์ฌํ๊ธฐ ๋๋ฌธ์ ์ฌ๊ธฐ์ ์์ฑ์๊ฐ ๋๊ตฌ์ธ์ง ์๋ ฅํด์ฃผ์ด์ผ ํจ

8.  `comment.save()` : DB์ ์ ์ฅ

```HTML
<!-- articles/detail.html -->
  <form action="{% url 'articles:comments_create' article.pk %}" method="POST">
  	{% csrf_token %}
    {{ comment_form }}
    <input type="submit">
  </form>
```

> The 'save()' method
>
> - save(commit=False)
>   - Create, but don`t save the new instance.
>   - ์์ง ๋ฐ์ดํฐ๋ฒ ์ด์ค์ ์ ์ฅ๋์ง ์์ ์ธ์คํด์ค๋ฅผ ๋ฐํ
>   - ์ ์ฅํ๊ธฐ ์ ์ ๊ฐ์ฒด์ ๋ํ ์ฌ์ฉ์ ์ง์  ์ฒ๋ฆฌ๋ฅผ ์ํํ  ๋ ์ ์ฉํ๊ฒ ์ฌ์ฉ

## Commnet READ

### ๋๊ธ ์ถ๋ ฅ ๊ฐ๋ฐ ๋ก์ง

- ํน์  article์ ์๋ ๋ชจ๋  ๋๊ธ์ ๊ฐ์ ธ์จ ํ context์ ์ถ๊ฐ

```python
# articles/views.py
from .models import Article, Comment

def detail(request, pk):
    article = get_object_or_404(Article, pk=pk)
    comment_form = CommentForm()
    comments =article.comment_set.all()
    context = {
        'article' : article,
        'comment_form' : comment_form,
        'comments' : comments,
    }
    return render(request, 'articles/detail.html', context)
```

1. `article = get_object_or_404(Article, pk=pk)` : pk์ ํด๋นํ๋ `Article` form์ ๋ํ ๋ฐ์ดํฐ๋ฅผ `article`์ด ์ธ์คํด์ค ํํ๋ก ๊ฐ์ง๊ณ  ์์

2. `comment_form = CommentForm()` : `Article`์ pk์ ํด๋นํ๋ `CommentForm()`์ ๋ํ ๋ฐ์ดํฐ๋ฅผ `comment_form`์ด ์ธ์คํด์ค ํํ๋ก ๊ฐ์ง๊ณ  ์์()

3. `comments = article.comment_set.all()` :  (1)๋ฒ์ `article` ๊ฒ์๋ฌผ์ ์์ฑํ ๋๊ธ์ ๋ชจ๋ ๊ฐ์ ธ์ `comments`์ ๋ด์, ์ญ์ฐธ์กฐ์ ๊ณผ์ 

   "comment_set์ ๋ชจ๋ธ ์ด๋ฆ์ด Comment์ฌ์ ๊ทธ๋ ๋ค! ๋ชจ๋ธ ์ด๋ฆ์ abc๋ก ๋ณ๊ฒฝํ๋ฉด abc_set์ผ๋ก ๋ฉ์๋ ์ด๋ฆ์ด ๋ฐ๋๋ค. "

```html
- detail ํ์ด์ง์์ ๋๊ธ ์ถ๋ ฅ

<!-- articles/detail.html -->
{% extends 'base.html' %}

{% block content %}
...
    <a href="{% url 'articles:index' %}">back</a>
    <hr>
    <h4>๋๊ธ ๋ชฉ๋ก</h4>
    <ul>
      {% for comment in comments %}
        <li>{{ comment.content }}</li>
      {% endfor %}
    </ul>
...
{% endblock %}
```

## Comment DELETE

### ๋๊ธ ์ญ์  ๊ฐ๋ฐ ๋ก์ง

```python
# articles/urls.py

urlpatterns = [
    ...,
    path('<int:article_pk>/comments/<int:comment_pk>/delete/', views.comment_delete, name='comment_delete'),
]
```

```python
# articles/views.py

@require_POST
def comment_delete(request, article_pk ,comment_pk):
    if request.user.is_authenticated:
        comment = get_object_or_404(Comment, pk=comment_pk)
        if request.user == comment.user:
            comment.delete()
    return redirect('articles:detail', article_pk)
```

1. `comment = get_object_or_404(Comment, pk=comment_pk)` : ๋๊ธ ํ์ด๋ธ์์ pk๊ฐ `comment_pk`์ธ ๋ฐ์ดํฐ๋ฅผ `comment`์ ๋ด๋๋ค.
2. `comment.delete()` : ํ์ด๋ธ์์ ํด๋น ๋ฐ์ดํฐ ์ญ์ , ์ญ์ ๋ ๋ฐ๋ก ์ ์ฅํ์ง ์๊ณ  ์ญ์ ๋๋ค.

```html
<!-- articles/detail.html -->
{% extends 'base.html' %}

{% block content %}
...
    <a href="{% url 'articles:index' %}">back</a>
    <hr>
    <h4>๋๊ธ ๋ชฉ๋ก</h4>
    <ul>
      {% for comment in comments %}
        <li>
          {{ comment.content }}
          <form action = "{% url 'articles:comments_delete' article.pk comment.pk%}" method="POST" class="d-inline">
              {% csrf_token %}
              <input type="submit" value="DELETE">
            </form>
        </li>
      {% endfor %}
    </ul>
...
{% endblock %}
```



## Comment ์ถ๊ฐ์ฌํญ

#### - ๋๊ธ ๊ฐ์ ์ถ๋ ฅํ๊ธฐ

```html
<!-- articles/detail.html -->

<h4>๋๊ธ ๋ชฉ๋ก</h4>
{% if comments %}
	<p><b>{{ comments|length }}๊ฐ์ ๋๊ธ์ด ์์ต๋๋ค.</b></p>
{% endif %}
```

#### - ๋๊ธ์ด ์๋ ๊ฒฝ์ฐ ๋์ฒด ์ปจํ์ธ  ์ถ๋ ฅ (DTL์ for-empty ํ๊ทธ ํ์ฉ)

```html
<ul>
  {% for comment in comments %}
    <li>
        {{ comment.content }}
        <form action = "{% url 'articles:comments_delete' article.pk comment.pk%}" method="POST" class="d-inline">
            {% csrf_token %}
            <input type="submit" value="DELETE">
        </form>
    </li>
  {% empty %}
    <p>๋๊ธ์ด ์์ด์..</p>
  {% endfor %}
</ul>
```



## Customizing authentication in Django

### Substituing a custom User model (User ๋ชจ๋ธ ๋์ฒดํ๊ธฐ)

- ์ผ๋ถ ํ๋ก์ ํธ์์๋ Django์  ๋ด์ฅ User ๋ชจ๋ธ์ด ์ ๊ณตํ๋ ์ธ์ฆ ์๊ตฌ์ฌํญ์ด ์ ์ ํ์ง ์์ ์ ์์

  - username ๋์  email์ ์๋ณ ํ ํฐ์ผ๋ก ์ฌ์ฉํ๋ ๊ฒ์ด ๋ ์ ํฉํ ์ฌ์ดํธ

- Django๋ User๋ฅผ ์ฐธ์กฐํ๋๋ฐ ์ฌ์ฉํ๋ `AUTH_USER_MODEL` ๊ฐ์ ์ ๊ณตํ์ฌ, default user model์ ์ฌ์ ์ ํ  ์ ์๋๋ก ํจ

- Django๋ ์ ํ๋ก์ ํธ๋ฅผ ์์ํ๋ ๊ฒฝ์ฐ ๊ธฐ๋ณธ ์ฌ์ฉ์ ๋ชจ๋ธ์ด ์ถฉ๋ถํ๋๋ผ๋, ์ปค์คํ ์ ์  ๋ชจ๋ธ์ ์ค์ ํ๋ ๊ฒ์ ๊ฐ๋ ฅํ๊ฒ ๊ถ์ฅ (highly recommended)

  - ๋จ, ํ๋ก์ ํธ์ ๋ชจ๋  migrations ํน์ ์ฒซ migrate๋ฅผ ์คํํ๊ธฐ ์ ์ ์ด ์์์ ๋ง์ณ์ผ ํจ

  

>  "ํ๋ก์ ํธ ์ด๊ธฐ, `migrations` ํ๊ธฐ ์ ์ ์ปค์คํ ์ ์  ๋ชจ๋ธ ์ค์ ํ๋ ๊ฒ์ด ๊ตญ๋ฃฐ"



### AUTH_USER_MODEL

- User๋ฅผ ๋ํ๋ด๋๋ฐ ์ฌ์ฉํ๋ ๋ชจ๋ธ
- ํ๋ก์ ํธ๊ฐ ์งํ๋๋ ๋์ ๋ณ๊ฒฝํ  ์ ์์
- ํ๋ก์ ํธ ์์ ์ ์ค์ ํ๊ธฐ ์ํ ๊ฒ์ด๋ฉฐ, ์ฐธ์กฐํ๋ ๋ชจ๋ธ์ ์ฒซ๋ฒ์งธ ๋ง์ด๊ทธ๋ ์ด์์์ ์ฌ์ฉํ  ์ ์์ด์ผ ํจ.
- ๊ธฐ๋ณธ ๊ฐ : `'auth.User'` (auth ์ฑ์ User ๋ชจ๋ธ)
- [์ฐธ๊ณ ] ํ๋ก์ ํธ ์ค๊ฐ(mid-project)์ AUTH_USER_MODEL ๋ณ๊ฒฝํ๊ธฐ
  - ๋ชจ๋ธ ๊ด๊ณ์ ์ํฅ์ ๋ฏธ์น๊ธฐ ๋๋ฌธ์ ํจ์ฌ ๋ ์ด๋ ค์ด ์์์ด ํ์
  - ์ฆ, ์ค๊ฐ ๋ณ๊ฒฝ์ ๊ถ์ฅํ์ง ์์ผ๋ฏ๋ก ์ด๊ธฐ์ ์ค์ ํ๋ ๊ฒ์ ๊ถ์ฅ

### Custom User ๋ชจ๋ธ ์ ์ํ๊ธฐ (4 ๋จ๊ณ)

1. ๊ด๋ฆฌ์ ๊ถํ๊ณผ ํจ๊ป ์์ ํ ๊ธฐ๋ฅ์ ๊ฐ์ถ User ๋ชจ๋ธ์ ๊ตฌํํ๋ ๊ธฐ๋ณธ ํด๋์ค์ธ `AbstractUser`๋ฅผ ์์๋ฐ์ ์๋ก์ด User ๋ชจ๋ธ ์์ฑ

```python
# accounts/models.py
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass
```

2. ๊ธฐ์กด์ Django๊ฐ ์ฌ์ฉํ๋ User ๋ชจ๋ธ์ด์๋ auth ์ฑ์ User ๋ชจ๋ธ์ accounts ์ฑ์ User ๋ชจ๋ธ์ ์ฌ์ฉํ๋๋ก ๋ณ๊ฒฝ

```python
# settings.py ๋งจ ๋ฐ์

AUTH_USER_MODEL = 'accounts.User'
```

3. admin site์ Custom User ๋ชจ๋ธ ๋ฑ๋ก

```python
# accounts/admin.py
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin
from .models import User

admin.site.register(User, UserAdmin)
```

4. ํ๋ก์ ํธ ์ค๊ฐ์ ์งํํ๋ค๋ฉด ๋ฐ์ดํฐ๋ฒ ์ด์ค๋ฅผ ์ด๊ธฐํ ํ ํ ๋ง์ด๊ทธ๋ ์ด์ ์งํ

   - ์ด๊ธฐํ ๋ฐฉ๋ฒ
     1. `db.sqlite3` ํ์ผ ์ญ์ 
     2. `migrations` ํ์ผ ๋ชจ๋ ์ญ์  (ํ์ผ๋ช์ ์ซ์๊ฐ ๋ถ์ ํ์ผ๋ง ์ญ์ )
   - ์ดํ ๋ง์ด๊ทธ๋ ์ด์ ์งํ

   ```bash
    $ python manage.py makemigrations
    $ python manage.py migrate
   ```



## Custom user & Built-in auth forms

>  User ๋ชจ๋ธ์ ์ปค์คํฐ๋ง์ด์งํ ์ดํ ํ์๊ฐ์์ ์๋ํ๋ฉด ๋ค์๊ณผ ๊ฐ์ ์๋ฌ๊ฐ ๋ฐ์ํ๋ค

![image-20220413211037431](Django%20-%20Model%20Relationship.assets/image-20220413211037431.png)

- `UserCreationForm`๊ณผ `UserChangeForm`์ ๊ธฐ์กด ๋ด์ฅ User ๋ชจ๋ธ์ ์ฌ์ฉํ `ModelForm`์ด๊ธฐ ๋๋ฌธ์ ์ปค์คํ User ๋ชจ๋ธ๋ก ๋์ฒดํด์ผ ํ๋ค. 

- ์ปค์คํ User ๋ชจ๋ธ์ด AbstractUser์ ํ์ ํด๋์ค์ธ ๊ฒฝ์ฐ ๋ค์๊ณผ ๊ฐ์ ๋ฐฉ์์ผ๋ก form์ ํ์ฅํ๋ค.

``` python
# accounts/forms.py
from django.contrib.auth.forms import UserChangeForm, UserCreationForm
from django.contrib.auth import get_user_model

class CustomUserChangeForm(UserChangeForm):

    # password = None

    class Meta:
        model = get_user_model() # User
        fields = ('email', 'first_name', 'last_name',)

class CustomUserCreationForm(UserCreationForm):

    class Meta(UserCreationForm.Meta):
        model = get_user_model()
        fields = UserCreationForm.Meta.fields + ('email',)
```

> `get_user_model()`

- ํ์ฌ ํ๋ก์ ํธ์์ ๋ฉ์ธ์ผ๋ก ํ์ฑํ๋ ์ฌ์ฉ์ ๋ชจ๋ธ(active user model)์ ๋ฐํ
  - User ๋ชจ๋ธ์ ์ปค์คํฐ๋ง์ด์งํ ์ํฉ์์๋ Custom User ๋ชจ๋ธ์ ๋ฐํ
- ์ด ๋๋ฌธ์ Django๋ User ํด๋์ค๋ฅผ ์ง์  ์ฐธ์กฐํ๋ ๋์  `django.contrib.auth.get_user_model()`์ ์ฌ์ฉํ์ฌ ์ฐธ์กฐํด์ผ ํ๋ค๊ณ  ๊ฐ์กฐ

> `signup view `ํจ์ ์ฝ๋ ์์ 
>
> - ์์  ํ ํ์๊ฐ์ ์ฌ์๋

```python
# accounts/views.py

from .forms import CustomUserChangeForm, CustomUserCreationForm

def signup(request):
    if request.user.is_authenticated:
        return redirect('articles:index')

    if request.method == 'POST':
        form = CustomUserCreationForm(request.POST)
        if form.is_valid():
            user = form.save()
            auth_login(request, user)
            return redirect('articles:index')
    else:
        form = CustomUserCreationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/signup.html', context)
```

1. `form = CustomUserCreationForm(request.POST)` : Custom User๋ฅผ ์์ํ ํผ์ ๋ง์ถฐ ์์ฑํ ๋ฐ์ดํฐ๋ฅผ `form`์ ๋ด์
2. `form = CustomUserCreationForm()` : Custom User๋ฅผ ์์ํ ํผ์ ๋ง์ถฐ ํ๋ฉด์ ๋ ๋๋ง ํ  ์ ์๋๋ก ํจ



## User - Article (1:N)

### User ๋ชจ๋ธ ์ฐธ์กฐํ๊ธฐ

1. `settings.AUTH_USER_MODEL`
   - User ๋ชจ๋ธ์ ๋ํ ์ธ๋ ํค ๋๋ ๋ค๋๋ค ๊ด๊ณ๋ฅผ ์ ์ ํ  ๋ ์ฌ์ฉํด์ผ ํจ
   - `models.py`์์ User ๋ชจ๋ธ์ ์ฐธ์กฐํ  ๋ ์ฌ์ฉ
2. `get_user_model()`
   - ํ์ฌ ํ์ฑํ(active) ๋ User ๋ชจ๋ธ์ ๋ฐํ
     - ์ปค์คํฐ๋ง์ด์งํ User ๋ชจ๋ธ์ด ์๋ ๊ฒฝ์ฐ๋ Custom User ๋ชจ๋ธ, ๊ทธ๋ ์ง ์์ผ๋ฉด User๋ฅผ ๋ฐํ
     - User๋ฅผ ์ง์  ์ฐธ์กฐํ์ง ์๋ ์ด์ 
   - `models.py`๊ฐ ์๋ ๋ค๋ฅธ ๋ชจ๋  ๊ณณ์์ ์ ์  ๋ชจ๋ธ์ ์ฐธ์กฐํ  ๋ ์ฌ์ฉ

>์ get_user_modelํจ์๋ฅผ ์์ฐ๊ณ   settings.AUTH_USER_MODEL ์ ์ฌ์ฉํ๋์ง?
>
>์ฅ๊ณ ์์ APP์ด ์คํ๋๋ ์์์ ๋ฐ๋ผ  User ๋ชจ๋ธ์ด ํ์ฑํ ๋์ง ์๊ณ  ํธ์ถ๋  ๊ฒฝ์ฐ๊ฐ ์์ ์ ์๊ธฐ ๋๋ฌธ์ `settings.AUTH_USER_MODEL`์ ์ฌ์ฉํ๋ค.
>
>(get_user_model()์ ๋ฐํ์ด object(๊ฐ์ฒด)์ด๊ณ  settings.AUTH_USER_MODEL์ ๋ฐํ์ด str์)



```python
# articles/models.py

from django.conf import settings

class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
```

```bash
$ python manage.py makemigrations
```

![image-20220413220648759](Django%20-%20Model%20Relationship.assets/image-20220413220648759.png)

- null ๊ฐ์ด ํ์ฉ๋์ง ์๋ `user_id` ํ๋๊ฐ ๋ณ๋์ ๊ฐ ์์ด `article`์ ์ถ๊ฐ๋๋ ค ํ๊ธฐ ๋๋ฌธ
- 1์ ์๋ ฅ ํ enter - ''ํ์ฌ ํ๋ฉด์์ ๊ธฐ๋ณธ ๊ฐ์ ์ค์ ํ๊ฒ ๋ค''๋ผ๋ ์๋ฏธ
- ๋ ๋ค์ 1์ ์๋ ฅ ํ enter - ''๊ธฐ์กด ํ์ด๋ธ์ ์ถ๊ฐ๋๋ `user_id` ํ๋์ ๊ฐ์ 1๋ก ์ค์ ํ๊ฒ ๋ค''๋ผ๋ ์๋ฏธ

```bash
$ python manage.py migrate
```

### ๊ฒ์๊ธ ์ถ๋ ฅ ํ๋ ์์ 

![image-20220413220852537](Django%20-%20Model%20Relationship.assets/image-20220413220852537.png)

>  ๊ฒ์๊ธ ์์ฑ ํ์ด์ง์์ ๋ถํ์ํ ํ๋๊ฐ ์ถ๋ ฅ๋๋ ๊ฒ์ ํ์ธ

1. `ArticleForm`์ ์ถ๋ ฅ ํ๋ ์์  ํ ๊ฒ์๊ธ ์์ฑ ์ฌ์๋ - ๋จ์ํ ํ๋ฉด์์๋ง ๋ถํ์ํ ํ๋ ์ญ์ 

```python
# articles/forms.py

class ArticleForm(forms.ModelForm):
    
    class Meta:
        model = Article
        fields = ('title', 'content',)
```

" ๋ค์ ๊ฒ์๊ธ์ ์์ฑํด ๋ณด๋ฉด ๋ค์๊ณผ ๊ฐ์ ์๋ฌ๊ฐ ๋ฐ์ํ๋ค"

![image-20220413221202250](Django%20-%20Model%20Relationship.assets/image-20220413221202250.png)

- ๊ฒ์๊ธ ์์ฑ ์ NOT NULL constraint failed : articles_article_user_id ์๋ฌ ๋ฐ์
- ๊ฒ์๊ธ ์์ฑ ์ ์์ฑ์ ์ ๋ณด(article.user)๊ฐ ๋๋ฝ๋์๊ธฐ ๋๋ฌธ
- ํ๋ฉด์์ ์์ฑ์ ์ ๋ณด ํ๋๋ฅผ ์ ๊ฑฐํด๋ฒ๋ ค์ ์์ฑ์ ์ ๋ณด๋ฅผ ๋ฐ๋ ๋ถ๋ถ์ด ๋ฐ๋ก ์์..

2. `article.user = request.user` : ๊ฒ์๊ธ ์์ฑ ์ ์์ ์ฌ ์ ๋ณด(article.user) ์ถ๊ฐ ํ ๊ฒ์๊ธ ์์ฑ ์ฌ์๋

#### CREATE

- ๊ฒ์๊ธ ์์ฑ ์ ์์ฑ์ ์ ๋ณด(article.user) ์ถ๊ฐ ํ ๊ฒ์๊ธ ์์ฑ ์ฌ์๋

```python
# articles/views.py

@login_required
@require_http_methods(['GET', 'POST'])
def create(request):
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        if form.is_valid():
            article = form.save(commit=False)
            article.user = request.user
            article.save()
            return redirect('articles:detail', article.pk)
    else:
        form = ArticleForm()
    context = {
        'form': form,
    }
    return render(request, 'articles/create.html', context)
```

#### DELETE

- ์์ ์ด ์์ฑํ ๊ฒ์๊ธ๋ง ์ญ์  ๊ฐ๋ฅํ๋๋ก ์ค์ 

```PYTHON
# articles/views.py

@require_POST
def delete(request, pk):
    article = get_object_or_404(Article, pk=pk)
    if request.user.is_authenticated:
        if request.user == article.user:
            article.delete()
    		return redirect('articles:index')
    return redirect('articles:detail', article.pk)
```

#### UPDATE

- ์์ ์ด ์์ฑํ ๊ฒ์๊ธ๋ง ์์  ๊ฐ๋ฅํ๋๋ก ์ค์ 

```PYTHON
# articles/views.py

@login_required
@require_http_methods(['GET', 'POST'])
def update(request, pk):
    article = get_object_or_404(Article, pk=pk)
    if request.user == article.user:
        if request.method == 'POST':
            form = ArticleForm(request.POST, instance=article)
            if form.is_valid():
                article = form.save()
                return redirect('articles:detail', article.pk)
        else:
            form = ArticleForm(instance=article)
    else:
        return redirect('articles:index')
    context = {
        'article': article,
        'form': form,
    }
    return render(request, 'articles/update.html', context)
```

1. `article = get_object_or_404(Article, pk=pk)` : `Article` ํ์ด๋ธ์์ ํด๋น pk์ธ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์์ `article`์ ๋ด์
2. `if request.user == article.user` : ๊ฒ์๊ธ์ ์์ฑํ ์ฌ์ฉ์๊ฐ ํ์ฌ ์์ ์ ์์ฒญํ ์ฌ์ฉ์์ ๊ฐ์์ง ํ์ธ
3. `if request.method == 'POST'` : ์์ฒญ์ `POST` ๋ฐฉ์์ผ๋ก ํ๋์ง ํ์ธ
4. `form = ArticleForm(request.POST, instance=article)` : ๋ด๊ฐ ์์ฑํ ๊ฒ์๋ฌผ
5. `if form.is_valid()` : ์ ํจ์ฑ ๊ฒ์ฌ
6. `article = form.save()` : ์์ฑํ ๊ฒ์๊ธ ์ ์ฅ
7. `form = ArticleForm(instance=article)` : ๊ฒ์๊ธ ์์ ํ๋ฌ ๋ค์ด์์ ๋ ๋ด๊ฐ ์ด์ ์ ์์ฑํ ๋ด์ฉ ๋ณด์ฌ์ฃผ๊ธฐ

#### READ

- ๊ฒ์๊ธ ์์ฑ user๊ฐ ๋๊ตฌ์ธ์ง `index.html`์์ ์ถ๋ ฅํ๊ธฐ

```html
<!-- articles/index.html -->

{% extends 'base.html' %}

{% block content %}
  {% for article in articles %}
    <p>์์ฑ์: {{ article.user }}</p>
    <p>๊ธ ๋ฒํธ: {{ article.pk }}</p>  
    <p>๊ธ ์ ๋ชฉ: {{ article.title }}</p>
    <p>๊ธ ๋ด์ฉ: {{ article.content }}</p>
    <a href="{% url 'articles:detail' article.pk %}">DETAIL</a>
    <hr>
  {% endfor %}
{% endblock content %}
```

- ํด๋น ๊ฒ์๊ธ์ ์์ฑ์๊ฐ ์๋๋ผ๋ฉด, ์์ /์ญ์  ๋ฒํผ์ ์ถ๋ ฅํ์ง ์๋๋ก ์ฒ๋ฆฌ

```html
<!-- articles/detail.html -->

{% if user == article.user %}
    <a href="{% url 'articles:update' article.pk %}">์์ </a>
    <form action="{% url 'articles:delete' article.pk %}" method="POST">
        {% csrf_token %}
        <input type="submit" value="์ญ์ ">
    </form>
{% endif %}
```





## User - Article (1:N)

### User์ Comment ๊ฐ ๋ชจ๋ธ ๊ด๊ณ ์ ์ ํ migration

```python
# articles/models.py

class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
	...
```

```bash
$ python manage.py makemigrations
$ python manage.py migrate
```

![image-20220415234028371](Django%20-%20Model%20Relationship.assets/image-20220415234028371.png)

- null ๊ฐ์ด ํ์ฉ๋์ง ์๋ `user_id` ํ๋๊ฐ ๋ณ๋์ ๊ฐ ์์ด `comment`์ ์ถ๊ฐ๋๋ ค ํ๊ธฐ ๋๋ฌธ

- 1์ ์๋ ฅ ํ enter

  - ''ํ์ฌ ํ๋ฉด์์ ๊ธฐ๋ณธ ๊ฐ์ ์ค์ ํ๊ฒ ๋ค'' ๋ผ๋ ์๋ฏธ

- ๋ค์ ํ๋ฒ 1์ ์๋ ฅ ํ enter

  - '๊ธฐ์กด ํ์ด๋ธ์ ์ถ๊ฐ๋๋ `user_id` ํ๋์ ๊ฐ์ 1๋ก ์ค์ ํ๊ฒ ๋ค' ๋ผ๋ ์๋ฏธ
  - ๊ธฐ์กด ๋๊ธ์ ์์ฑ์๊ฐ ๋ชจ๋ 1๋ฒ user๊ฐ ๋จ

- ```bash
  # migrate ๋ง๋ฌด๋ฆฌ
  $ python manage.py migrate
  ```

### ๋๊ธ ์ถ๋ ฅ ํ๋ ์์ 

- ๊ฒ์๊ธ ์์ฑ ํ์ด์ง์์ ๋ถํ์ํ ํ๋๊ฐ ์ถ๋ ฅ๋๋ ๊ฒ์ ํธ๊ฐ์ธ

![image-20220415234240982](Django%20-%20Model%20Relationship.assets/image-20220415234240982.png)

- ๋๊ธ ์์ฑ ์ user ForeignKey๋ฅผ ์ถ๋ ฅํ์ง ์๋๋ก ์ค์ 

```python
# articles/forms.py

class CommentForm(forms.ModelForm):

    class Meta:
        model = Comment
        fields = ('content',)
      	# exclude = ('article', 'user')
```

- ๋ค์ ๋๊ธ์ ์๋ ฅํ๋ฉด `NOT NULL constraint faield` ์๋ฌ๊ฐ ๋ฐ์ํ๋ค. ๋๊ธ ์์ฑ์ ์๋ ฅ ๋ฐ์๋ ์์ฑ์ ์ ๋ณด(comment.user)๊ฐ ๋๋ฝ๋์๊ธฐ ๋๋ฌธ

![image-20220415235411581](Django%20-%20Model%20Relationship.assets/image-20220415235411581.png)

### CREATE

- ๋๊ธ ์์ฑ ์ ์์ฑ์ ์ ๋ณด(request.user) ์ถ๊ฐ ํ ๋๊ธ ์์ฑ ์ฌ์๋

```python
# articles/views.py

@require_POST
def comment_create(request, pk):
    if request.user.is_authenticated:
        article = get_object_or_404(Article, pk=pk)
        comment_form = CommentForm(request.POST)
        if comment_form.is_valid():
            comment = comment_form.save(commit=False)
            comment.article = article
            comment.user = request.user
            comment.save()
        return redirect('articles:detail', article.pk)
    return redirect('accounts:login')
```

### READ

- ๋น๋ก๊ทธ์ธ ์ ์ ์๊ฒ๋ ๋๊ธ form ์ถ๋ ฅ ์จ๊ธฐ๊ธฐ

```html
<!-- articles/detail.html -->

{% if request.user.is_authenticated %}
    <form action="{% url 'articles:comment_create' article.pk %}" method="POST">
          {% csrf_token %}
          {{ comment_form }}
      <input type="submit">
    </form>
{% else %}
	<a href="{% url 'accounts:login' %}">[๋๊ธ์ ์์ฑํ๋ ค๋ฉด ๋ก๊ทธ์ธํ์ธ์.]</a>
{% endif %}
```

- ๋๊ธ ์์ฑ์ ์ถ๋ ฅํ๊ธฐ

```html
<!-- articles/detail.html -->

<h4>๋๊ธ ๋ชฉ๋ก</h4>

{% for comment in comments %}
<li>
    {{ comment.user }} - {{ comment.content }}
    <form action="{% url 'articles:comment_delete' article.pk comment.pk %}" method="POST">
        {% csrf_token %}
        <input type="submit" value="์ญ์ ">
    </form>
</li>
{% empty %}
	<p>๋๊ธ์ด ์์ด์..</p>
{% endfor %}
```

### DELETE

- ์์ ์ด ์์ฑํ ๋๊ธ๋ง ์ญ์  ๋ฒํผ์ ๋ณผ ์ ์๋๋ก ์์ 

```HTML
<!-- articles/detail.html -->

{% for comment in comments %}
<li>
    {{ comment.user }} - {{ comment.content }}
    {% if request.user == comment.user %}
    <form action="{% url 'articles:comment_delete' article.pk comment.pk %}" method="POST">
        {% csrf_token %}
        <input type="submit" value="์ญ์ ">
    </form>
    {% endif %}
</li>
{% empty %}
	<p>๋๊ธ์ด ์์ด์..</p>
{% endfor %}
```

- ์์ ์ด ์์ฑํ ๋๊ธ๋ง ์ญ์  ํ  ์ ์๋๋ก ์์ 

```python
# articles/views.py

@require_POST
def comment_delete(request, article_pk ,comment_pk):
    if request.user.is_authenticated:
        comment = get_object_or_404(Comment, pk=comment_pk)
        if request.user == comment.user:
            comment.delete()
    return redirect('articles:detail', article_pk)
```