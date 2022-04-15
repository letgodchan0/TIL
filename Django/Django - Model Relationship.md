# 🌱Django - Model Relationship 

## Foreign Key 개념

- 외래 키 (외부 키)
- 관계형 데이터베이스에서 한 테이블의 필드 중 다른 테이블의 행을 식별할 수 있는 키
- 참조하는 테이블에서 속성(필드)에 해당하고, 이는 참조되는 테이블의 기본 키(Primary Key)를 가리킴
- 참조하는 테이블의 외래 키는 참조되는 테이블 행 1개에 대응됨
  - 이 때문에 참조하는 테이블에서 참조되는 테이블의 존재하지 않는 행을 참조할 수 없음
- 참조하는 테이블의 행 여러 개가 참조되는 테이블의 동일한 행을 참조할 수 있음 

![image-20220413091841736](Django%20-%20Model%20Relationship.assets/image-20220413091841736.png)



## Foreign Key 특징

- 키를 사용하여 부모 테이블 (참조 되는 테이블)의 유일한 값을 참조 (참조 무결성)
  - [참고] 참조 무결성
  - 데이터베이스 관계 모델에서 관련된 2개의 테이블 간의 일관성을 말함
  - 외래 키가 선언된 테이블의 외래 키 속성(열)의 값은 그 테이블의 부모가 되는 테이블의 기본 키 값으로 존재해야 함
- 외래 키의 값이 반드시 부모 테이블의 기본 키일 필요는 없지만 유일한 값이어야 함

## ForeignKey field

- A many-to-one relationship
- 2개의 위치 인자가 반드시 필요
  1. 참조하는 model class
  2. on_delete 옵션
- migrate 작업 시 필드 이름에 `_id`를 추가하여 데이터베이스 열 이름을 만듦

- [참고] 재귀 관계 (자신과 1:N)

```python
models.ForeignKey('self', on_delete=models.CASCADE)
```

- comment 모델 정의 하기

```python
# articles/models.py

class Comment(models.Model):
    # 참조하는 테이블의 소문자 단수형, db 컬럼에 article_id로 생성된다
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    def __str__(self):
        return self.content
```

### ForeignKey arguments - 'on_delete'

- 외래 키가 참조하는 객체가 사라졌을 때 외래 키를 가진 객체를 어떻게 처리할 지를 정의
- Database Integrity(데이터 무결성)을 위해서 매우 중요한 설정
- on_delete 옵션에 사용 가능한 값들
  - CASCADE : 부모 객체(참조 된 객체)가 삭제 됐을 때 이를 참조하는 객체도 삭제 
  - PROTECT
  - SET_NULL
  - SET_DEFAULT
  - SET()
  - DO_NOTHING
  - RESTRICT

### [참고] 데이터 무결성

- 데이터의 정확성과 일관성을 유지하고 보증하는 것을 가리키며, 데이터베이스나 RDBMS 시스템의 중요한 기능임
- 무결성 제한의 유형
  1. 개체 무결성 (Entity integrity)
     - PK의 개념과 관련
     - 모든 테이블이 PK를 가져야 하며 PK로 선택된 열은 고유한 값이어야 하고 빈 값은 허용치 않음을 규정
  2. 참조 무결성 (Referential integrity)
     - FK(외래 키) 개념과 관련
     - FK 값이 데이터베이스의 특정 테이블의 PK 값을 참조하는 것
  3. 범위(도메인) 무결성 (Domain integrity)
     - 정의된 형식(범위)에서 관계형 데이터베이스의 모든 컬럼이 선언되도록 규정

### Migration

1. ```bash
   $ python manage.py makemigrations
   ```

2. ![image-20220413093519088](Django%20-%20Model%20Relationship.assets/image-20220413093519088.png)

3. ```bash
   $ python manage.py migrate
   ```

4. `articles_comment` 테이블의 외래 키 컬럼 확인 (필드 이름에 _id가 추가됨)

![image-20220413093637878](Django%20-%20Model%20Relationship.assets/image-20220413093637878.png)



### 데이터베이스의 ForeignKey 표현

- 만약 ForeginKey 인스턴스를 abcd로 생성했다면 abcd_id로 만들어짐

- 하지만 명시적인 모델 관계 파악을 위해 참조하는 클래스 이름의 소문자(단수형)로 작성하는 것이 바람직함 (1:N)

- | id   | content | article_id |
  | ---- | ------- | ---------- |
  | 1    | 댓글 1  | 3          |
  | 2    | 댓글 2  | 1          |
  | 3    | 댓글 3  | 1          |



## 댓글 생성 연습하기

1. ```bash
   $python manage.py shell_plus
   ```

2. ```shell
   comment = Comment()
   comment.content = 'first comment'
   comment.save()
   ```

   ![image-20220413094134268](Django%20-%20Model%20Relationship.assets/image-20220413094134268.png)

   - articles_comment 테이블의 ForeignKey, article_id 값이 누락되었기 때문에 에러 발생 
   - 따라서 게시글을 생성 후 댓글 생성 재시도를 해야 한다.

3. ```shell
   # 게시글 생성
   article = Article.objects.create(title='title', content='content')
   
   # 1번 게시글을 article로 담기
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

   - 댓글 속성 값 확인

     - 실제로 작성된 외래 키 컬럼명은 `article_id`이기 때문에 `article_pk`로는 값에 접근할 수 없음

     ```shell
     comment.content
     'first comment'
     
     comment.article_id
     1
     
     comment.article
     <Article: title>
     ```

     - comment 인스턴스를 통한 article 값 접근

     ```shell
     comment.article.pk
     1
     # 게시글 내용
     comment.article.content
     'content'
     ```

4. 두번째 댓글 작성 하기

```shell
# 현재 article의 pk가 1, 즉 첫번째 게시글을 담고 있음
comment = Comment(content='second comment', article=article)

comment.save()

# 댓글 pk
comment.pk
2

# 게시글 pk
comment.article_id
1
```

#### admin site에 작성된 댓글 확인

```python
# articles/admin.py

from .models import Article, Comment

admin.site.register(Comment)
```

```bash
# admin 계정 생성
$ python manage.py createsuperuser
```



## 1:N 관계 related manager

- 역참조('comment_set')
  - Article(1) -> Comment(N)
  - article.comment 형태로는 사용할 수 없고, article.comment_set manager가 생성됨, (Article 테이블에는 comment가 존재하지 않기 때문)
  - 게시글에 몇 개의 댓글이 작성 되었는지 Django ORM이 보장할 수 없기 때문
    - article은 comment가 있을 수도 있고, 없을 수도 있음
    - 실제로 Article 클래스에는 Comment와의 어떠한 관계도 작성되어 있지 않음
- 참조('article')
  - Comment(N) -> Article(1)
  - 댓글의 경우 어떠한 댓글이든 반드시 자신이 참조하고 있는 게시글이 있으므로, comment.article과 같이 접근할 수 있음
  - 실제 ForeignKey 또한 Comment 클래스에서 작성됨

### 1:N related manager 연습하기

```shell
article = Article.objects.get(pk=1)
dir(article)
```

- `dir()` 함수를 통해 article 인스턴스가 사용할 수 있는 모든 속성, 메서드를 직접 확인하기

- 메서드 중 `comment_set`이 역참조를 해주는 메서드

![image-20220413101318886](Django%20-%20Model%20Relationship.assets/image-20220413101318886.png)



- article의 입장에서 모든 댓글 조회하기 (역참조, 1 -> N)

```shell
article.comment_set.all()
<QuerySet [<Comment: first comment>, <Comment: second comment>]>
```

- 조회한 모든 댓글 출력하기

```shell
comments = article.comment_set.all()

for comment in comments:
	print(comment.content)
	
first comment
second comment
```

- comment의 입장에서 참조하는 게시글 조회하기 (참조, N -> 1)

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

- 역참조 시 사용할 이름('model_set' manager)을 변경할 수 있는 옵션
- 위와 같이 변경하면 `article.comment_set`은 더이상 사용할 수 없고, `article.comments`로 대체 됨
- [주의] 역참조 시 사용할 이름 수정 후, migration 과정 필요

- 그런데 1:N 관계에서는 이름을 바꾸는 것을 권장하지 않음. M:N 관계에서는 필수적으로 이름을 바꿔야 할 상황이 생기기 때문에 헷갈리지 않기 위해!

## Comment CREATE

```python
# articles/forms.py

from .models import Article, Comment

class CommentForm(forms.ModelForm):
    
    class Meta:
        model = Comment
        fields = '__all__'
```

> detail 페이지에서 CommentForm 출력

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

- 따라서 `CommentForm`에서 외래 키 필드 출력을 제외해야 한다

```python
# articles/forms.py

class CommentForm(forms.ModelForm):
    
    class Meta:
        model = Comment
        exclude = ('article',)
```

- exclude로 article을 해준다는 의미는 단순히 화면에서 해당 필드를 보여주지 않는 것을 의미한다. 그런데 여전히 에러가 나는 이유는, CommentForm은 ModelForm을 상속하고 Article을 참조하기 때문에 해당 외래키에 대한 입력을 필수적으로 요구하는데, exclude로 입력하는 곳을 지워버렸기 때문에 에러가 발생한다.  
- 따라서 ModelForm의 save의 메서드를 활용한다. 

### 댓글 기능 개발 로직

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

1. `request.user.is_authenticated` : 현재 요청을 보낸 사용자가 권한이 있는 사용자라면,

2. `article = get_object_or_404(Article, pk=pk)`: 현재 pk의 해당하는 객체를 article에 담음

3. `comment_form = CommentForm(request.POST)`: 댓글 데이터(?)를 `comment_form`에 담음

4. `if comment_form.is_valid()` : 댓글 데이터가 유효성 검사를 통과하면

5. `comment = comment_form.save(commit=False)` : 현재 입력받은 댓글 데이터를 `db`에 저장하기 위해서는 외래키의 정보가 추가적으로 필요하다. 따라서 `save` 메서드의 옵션인 `commit=False`를 해준다면, 아직 `db`에 저장을 하지 않고 객체만 반환한다. 

   "즉, commit이 false라면 db가 우리에게 기회를 주는거라고 생각하자! 너 지금 전달한 데이터 뭔가 부족해!! 추가해야할 것 있는 것 같은데, 일단 저장안하고 인스턴스 만들어 줄테니까 빨리 추가 작업해 라고 말해주는 거라고 생각하자!"

6. `comment.article = article` : `comment_article_id` 필드의 값을 `article` 의 pk로 할당한다.

7. `comment.user = request.user` : `comment` 테이블의 user를 요청을 보낸 user로 할당, 작성자 정보가 따로 입력 받을 수 없는 경우가 존재하기 때문에 여기서 작성자가 누구인지 입력해주어야 함

8.  `comment.save()` : DB에 저장

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
>   - 아직 데이터베이스에 저장되지 않은 인스턴스를 반환
>   - 저장하기 전에 객체에 대한 사용자 지정 처리를 수행할 때 유용하게 사용

## Commnet READ

### 댓글 출력 개발 로직

- 특정 article에 있는 모든 댓글을 가져온 후 context에 추가

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

1. `article = get_object_or_404(Article, pk=pk)` : pk에 해당하는 `Article` form에 대한 데이터를 `article`이 인스턴스 형태로 가지고 있음

2. `comment_form = CommentForm()` : `Article`의 pk에 해당하는 `CommentForm()`의 대한 데이터를 `comment_form`이 인스턴스 형태로 가지고 있음()

3. `comments = article.comment_set.all()` :  (1)번의 `article` 게시물에 작성한 댓글을 모두 가져와 `comments`에 담음, 역참조의 과정

   "comment_set은 모델 이름이 Comment여서 그렇다! 모델 이름을 abc로 변경하면 abc_set으로 메서드 이름이 바뀐다. "

```html
- detail 페이지에서 댓글 출력

<!-- articles/detail.html -->
{% extends 'base.html' %}

{% block content %}
...
    <a href="{% url 'articles:index' %}">back</a>
    <hr>
    <h4>댓글 목록</h4>
    <ul>
      {% for comment in comments %}
        <li>{{ comment.content }}</li>
      {% endfor %}
    </ul>
...
{% endblock %}
```

## Comment DELETE

### 댓글 삭제 개발 로직

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

1. `comment = get_object_or_404(Comment, pk=comment_pk)` : 댓글 테이블에서 pk가 `comment_pk`인 데이터를 `comment`에 담는다.
2. `comment.delete()` : 테이블에서 해당 데이터 삭제, 삭제는 따로 저장하지 않고 삭제된다.

```html
<!-- articles/detail.html -->
{% extends 'base.html' %}

{% block content %}
...
    <a href="{% url 'articles:index' %}">back</a>
    <hr>
    <h4>댓글 목록</h4>
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



## Comment 추가사항

#### - 댓글 개수 출력하기

```html
<!-- articles/detail.html -->

<h4>댓글 목록</h4>
{% if comments %}
	<p><b>{{ comments|length }}개의 댓글이 있습니다.</b></p>
{% endif %}
```

#### - 댓글이 없는 경우 대체 컨텐츠 출력 (DTL의 for-empty 태그 활용)

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
    <p>댓글이 없어요..</p>
  {% endfor %}
</ul>
```



## Customizing authentication in Django

### Substituing a custom User model (User 모델 대체하기)

- 일부 프로젝트에서는 Django의  내장 User 모델이 제공하는 인증 요구사항이 적절하지 않을 수 있음

  - username 대신 email을 식별 토큰으로 사용하는 것이 더 적합한 사이트

- Django는 User를 참조하는데 사용하는 `AUTH_USER_MODEL` 값을 제공하여, default user model을 재정의 할 수 있도록 함

- Django는 새 프로젝트를 시작하는 경우 기본 사용자 모델이 충분하더라도, 커스텀 유저 모델을 설정하는 것을 강력하게 권장 (highly recommended)

  - 단, 프로젝트의 모든 migrations 혹은 첫 migrate를 실행하기 전에 이 작업을 마쳐야 함

  

>  "프로젝트 초기, `migrations` 하기 전에 커스텀 유저 모델 설정하는 것이 국룰"



### AUTH_USER_MODEL

- User를 나타내는데 사용하는 모델
- 프로젝트가 진행되는 동안 변경할 수 없음
- 프로젝트 시작 시 설정하기 위한 것이며, 참조하는 모델은 첫번째 마이그레이션에서 사용할 수 있어야 함.
- 기본 값 : `'auth.User'` (auth 앱의 User 모델)
- [참고] 프로젝트 중간(mid-project)에 AUTH_USER_MODEL 변경하기
  - 모델 관계에 영향을 미치기 때문에 훨씬 더 어려운 작업이 필요
  - 즉, 중간 변경은 권장하지 않으므로 초기에 설정하는 것을 권장

### Custom User 모델 정의하기 (4 단계)

1. 관리자 권한과 함께 완전한 기능을 갖춘 User 모델을 구현하는 기본 클래스인 `AbstractUser`를 상속받아 새로운 User 모델 작성

```python
# accounts/models.py
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass
```

2. 기존에 Django가 사용하는 User 모델이었던 auth 앱의 User 모델을 accounts 앱의 User 모델을 사용하도록 변경

```python
# settings.py 맨 밑에

AUTH_USER_MODEL = 'accounts.User'
```

3. admin site에 Custom User 모델 등록

```python
# accounts/admin.py
from django.contrib import admin
from django.contrib.auth.admin import UserAdmin
from .models import User

admin.site.register(User, UserAdmin)
```

4. 프로젝트 중간에 진행했다면 데이터베이스를 초기화 한 후 마이그레이션 진행

   - 초기화 방법
     1. `db.sqlite3` 파일 삭제
     2. `migrations` 파일 모두 삭제 (파일명에 숫자가 붙은 파일만 삭제)
   - 이후 마이그레이션 진행

   ```bash
    $ python manage.py makemigrations
    $ python manage.py migrate
   ```



## Custom user & Built-in auth forms

>  User 모델을 커스터마이징한 이후 회원가입을 시도하면 다음과 같은 에러가 발생한다

![image-20220413211037431](Django%20-%20Model%20Relationship.assets/image-20220413211037431.png)

- `UserCreationForm`과 `UserChangeForm`은 기존 내장 User 모델을 사용한 `ModelForm`이기 때문에 커스텀 User 모델로 대체해야 한다. 

- 커스텀 User 모델이 AbstractUser의 하위 클래스인 경우 다음과 같은 방식으로 form을 확장한다.

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

- 현재 프로젝트에서 메인으로 활성화된 사용자 모델(active user model)을 반환
  - User 모델을 커스터마이징한 상황에서는 Custom User 모델을 반환
- 이 때문에 Django는 User 클래스를 직접 참조하는 대신 `django.contrib.auth.get_user_model()`을 사용하여 참조해야 한다고 강조

> `signup view `함수 코드 수정
>
> - 수정 후 회원가입 재시도

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

1. `form = CustomUserCreationForm(request.POST)` : Custom User를 상속한 폼에 맞춰 작성한 데이터를 `form`에 담음
2. `form = CustomUserCreationForm()` : Custom User를 상속한 폼에 맞춰 화면에 렌더링 할 수 있도록 함



## User - Article (1:N)

### User 모델 참조하기

1. `settings.AUTH_USER_MODEL`
   - User 모델에 대한 외래 키 또는 다대다 관계를 정의 할 때 사용해야 함
   - `models.py`에서 User 모델을 참조할 때 사용
2. `get_user_model()`
   - 현재 활성화(active) 된 User 모델을 반환
     - 커스터마이징한 User 모델이 있는 경우는 Custom User 모델, 그렇지 않으면 User를 반환
     - User를 직접 참조하지 않는 이유
   - `models.py`가 아닌 다른 모든 곳에서 유저 모델을 참조할 때 사용

>왜 get_user_model함수를 안쓰고  settings.AUTH_USER_MODEL 을 사용하는지?
>
>장고에서 APP이 실행되는 순서에 따라  User 모델이 활성화 되지 않고 호출될 경우가 있을 수 있기 때문에 `settings.AUTH_USER_MODEL`을 사용한다.
>
>(get_user_model()은 반환이 object(객체)이고 settings.AUTH_USER_MODEL은 반환이 str임)



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

- null 값이 허용되지 않는 `user_id` 필드가 별도의 값 없이 `article`에 추가되려 하기 때문
- 1을 입력 후 enter - ''현재 화면에서 기본 값을 설정하겠다''라는 의미
- 또 다시 1을 입력 후 enter - ''기존 테이블에 추가되는 `user_id` 필드의 값을 1로 설정하겠다''라는 의미

```bash
$ python manage.py migrate
```

### 게시글 출력 필드 수정

![image-20220413220852537](Django%20-%20Model%20Relationship.assets/image-20220413220852537.png)

>  게시글 작성 페이지에서 불필요한 필드가 출력되는 것을 확인

1. `ArticleForm`의 출력 필드 수정 후 게시글 작성 재시도 - 단순히 화면에서만 불필요한 필드 삭제

```python
# articles/forms.py

class ArticleForm(forms.ModelForm):
    
    class Meta:
        model = Article
        fields = ('title', 'content',)
```

" 다시 게시글을 작성해 보면 다음과 같은 에러가 발생한다"

![image-20220413221202250](Django%20-%20Model%20Relationship.assets/image-20220413221202250.png)

- 게시글 작성 시 NOT NULL constraint failed : articles_article_user_id 에러 발생
- 게시글 작성 시 작성자 정보(article.user)가 누락되었기 때문
- 화면에서 작성자 정보 필드를 제거해버려서 작성자 정보를 받는 부분이 따로 없음..

2. `article.user = request.user` : 게시글 작성 시 작정사 정보(article.user) 추가 후 게시글 작성 재시도

#### CREATE

- 게시글 작성 시 작성자 정보(article.user) 추가 후 게시글 작성 재시도

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

- 자신이 작성한 게시글만 삭제 가능하도록 설정

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

- 자신이 작성한 게시글만 수정 가능하도록 설정

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

1. `article = get_object_or_404(Article, pk=pk)` : `Article` 테이블에서 해당 pk인 데이터를 가져와서 `article`에 담음
2. `if request.user == article.user` : 게시글을 작성한 사용자가 현재 수정을 요청한 사용자와 같은지 확인
3. `if request.method == 'POST'` : 요청을 `POST` 방식으로 했는지 확인
4. `form = ArticleForm(request.POST, instance=article)` : 내가 작성한 게시물
5. `if form.is_valid()` : 유효성 검사
6. `article = form.save()` : 작성한 게시글 저장
7. `form = ArticleForm(instance=article)` : 게시글 수정하러 들어왔을 때 내가 이전에 작성한 내용 보여주기

#### READ

- 게시글 작성 user가 누구인지 `index.html`에서 출력하기

```html
<!-- articles/index.html -->

{% extends 'base.html' %}

{% block content %}
  {% for article in articles %}
    <p>작성자: {{ article.user }}</p>
    <p>글 번호: {{ article.pk }}</p>  
    <p>글 제목: {{ article.title }}</p>
    <p>글 내용: {{ article.content }}</p>
    <a href="{% url 'articles:detail' article.pk %}">DETAIL</a>
    <hr>
  {% endfor %}
{% endblock content %}
```

- 해당 게시글의 작성자가 아니라면, 수정/삭제 버튼을 출력하지 않도록 처리

```html
<!-- articles/detail.html -->

{% if user == article.user %}
    <a href="{% url 'articles:update' article.pk %}">수정</a>
    <form action="{% url 'articles:delete' article.pk %}" method="POST">
        {% csrf_token %}
        <input type="submit" value="삭제">
    </form>
{% endif %}
```





## User - Article (1:N)

### User와 Comment 간 모델 관계 정의 후 migration

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

- null 값이 허용되지 않는 `user_id` 필드가 별도의 값 없이 `comment`에 추가되려 하기 때문

- 1을 입력 후 enter

  - ''현재 화면에서 기본 값을 설정하겠다'' 라는 의미

- 다시 한번 1을 입력 후 enter

  - '기존 테이블에 추가되는 `user_id` 필드의 값을 1로 설정하겠다' 라는 의미
  - 기존 댓글의 작성자가 모두 1번 user가 됨

- ```bash
  # migrate 마무리
  $ python manage.py migrate
  ```

### 댓글 출력 필드 수정

- 게시글 작성 페이지에서 불필요한 필드가 출력되는 것을 호가인

![image-20220415234240982](Django%20-%20Model%20Relationship.assets/image-20220415234240982.png)

- 댓글 작성 시 user ForeignKey를 출력하지 않도록 설정

```python
# articles/forms.py

class CommentForm(forms.ModelForm):

    class Meta:
        model = Comment
        fields = ('content',)
      	# exclude = ('article', 'user')
```

- 다시 댓글을 입력하면 `NOT NULL constraint faield` 에러가 발생한다. 댓글 작성시 입력 받았던 작성자 정보(comment.user)가 누락되었기 때문

![image-20220415235411581](Django%20-%20Model%20Relationship.assets/image-20220415235411581.png)

### CREATE

- 댓글 작성 시 작성자 정보(request.user) 추가 후 댓글 작성 재시도

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

- 비로그인 유저에게는 댓글 form 출력 숨기기

```html
<!-- articles/detail.html -->

{% if request.user.is_authenticated %}
    <form action="{% url 'articles:comment_create' article.pk %}" method="POST">
          {% csrf_token %}
          {{ comment_form }}
      <input type="submit">
    </form>
{% else %}
	<a href="{% url 'accounts:login' %}">[댓글을 작성하려면 로그인하세요.]</a>
{% endif %}
```

- 댓글 작성자 출력하기

```html
<!-- articles/detail.html -->

<h4>댓글 목록</h4>

{% for comment in comments %}
<li>
    {{ comment.user }} - {{ comment.content }}
    <form action="{% url 'articles:comment_delete' article.pk comment.pk %}" method="POST">
        {% csrf_token %}
        <input type="submit" value="삭제">
    </form>
</li>
{% empty %}
	<p>댓글이 없어요..</p>
{% endfor %}
```

### DELETE

- 자신이 작성한 댓글만 삭제 버튼을 볼 수 있도록 수정

```HTML
<!-- articles/detail.html -->

{% for comment in comments %}
<li>
    {{ comment.user }} - {{ comment.content }}
    {% if request.user == comment.user %}
    <form action="{% url 'articles:comment_delete' article.pk comment.pk %}" method="POST">
        {% csrf_token %}
        <input type="submit" value="삭제">
    </form>
    {% endif %}
</li>
{% empty %}
	<p>댓글이 없어요..</p>
{% endfor %}
```

- 자신이 작성한 댓글만 삭제 할 수 있도록 수정

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