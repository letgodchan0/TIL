# 🌱 Django - CRUD

## ORM

>객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템 간(Django - SQL) 데이터를 변환하는 프로그래밍 기술
>
>OOP 프로그래밍에서 RDBMS를 연동할 때, DB와 객체 지향 프로그래밍 언어 간의 호환되지 않는 데이터를 변환하는 프로그래밍 기법으로 django는 내장 orm을 사용한다!
>
>한마디로 파이썬으로 DB 조작을 어느정도 가능하게 해준다!

## DB API

> "DB를 조작하기 위한 도구"
>
> Django가 기본적으로 ORM을 제공함에 따른 것으로 DB를 편하게 조작할 수 있도록 도움
>
> Model을 만들면 Django는 객체들을 만들고 읽고 수정하고 지울 수 있는 db-abstract API를 자동으로 만든다!

```python
Article.objects.all()
# 클래스 이름, Manager, QuerySet API 
```

- Manager
  - Django 모델에 데이터베이스 query 작업이 제공되는 인터페이스
  - 기본적으로 모든 Django 모델 클래스에 objects라는 Manager를 추가
- QuerySet
  - DB로부터 전달받은 객체 목록
  - queryset 안의 객체는 0개, 1개 혹은 여러 개일 수 있음
  - DB로부터 조회, 필터, 정렬 등을 수행할 수 있음

## Django shell

> 일반 Python shell을 통해서는 장고 프로젝트 환경에 접근할 수 없음!!!
>
> 그래서 장고 프로젝트 설정이 load 된 Python shell을 활용해 DB API 구문 테스트를 진행한다
>
> 기본 Django shell 보다 더 많은 기능을 제공하는 shell_plus를 사용해서 진행한다!
>
> - Django-extensions 라이브러리의 기능 중 하나

```bash
# 라이브러리 설치
$ pip install ipython
$ pip install django-extensions
```

```python
# settings.py에 등록
INSTALLED_APPS = [
    ....,
    'django_extensions',		# 언더바 _ 주의!!
    .....,
]
```

```bash
# shell_plus 실행!
$ python manage.py shell_plus

# DB에 인스턴스 객체를 얻기 위한 퀴리문 날리기 
# 레코드가 하나만 있으면 인스턴스 객체로, 두 개 이상이면 쿼리셋으로 리턴!
# 전체 객체 조회
>>> Article.objects.all()
<QuerySet []>
```

## CRUD - Create (생성)

```shell
# Article(class)로부터 article(instance)
>>> article = Article()

# 인스턴스 변수(title, content)에 값을 할당
>>> article.title = '제목'
>>> article.content = '내용입니다'

# save를 하지 않으면 아직 DB에 값이 저장되지 않음
>>> article
>>> <Article: Article object (None)>

# save를 하면 저장된 값을 확인 할 수 있음
>>> article.save()
>>> article
<Article: Article object (1)>
>> Article.objects.all()
<QuerySet [Article: Article object (1)]>

# 인스턴스를 활용하여 변수에 접근해보기
>>> article.title
'제목'
>>> article.content
'내용입니다'
>>> article.created_at
datetime.datetime(2022, 3, 10, 2, 43, 56, 49345, tzinfo=<UTC>)
```

```bash
# 다른 방법으로 생성 후 저장

>>> a2 = Article(title='2번글', content='2번 내용')
>>> a2.save()

>>> Article.objects.all()
>>> <QuerySet [<Article: Article object (1)>, <Article: Article object (2)>]>
```

```bash
# 저장따로 하는거 귀찮다 한번에 해줘
# 얘는 별도로 save 안해도 된다. 클래스에 직접 생성하는 거니까!

Article.objects.create(title='3번글', content='3번 내용')
```



## CRUD - Read (읽기)

### `all()` - 전체 데이터 조회, 

- 현재 QuerySet의 복사본을 반환

- 비어 있으면 비어 있는 그대로 반환

```bash
>>> Article.objects.all()
<QuerySet [<Article: Article object (1)>, <Article: Article object (2)>, <Article: Article object (3)>]>
```

### `get` - 단일 데이터 조회

- 일반적으로 고유한 값(unique)인 경우 사용하는 메서드

  - 없는 데이터일 때 에러 발생

  - 여러개 있는 경우 에러 발생

- `pk`는 데이터베이스에서 유일한 값이므로 `pk`를 기준으로 조회할 때 사용

```bash
# pk를 기준으로 조회
>>> Article.objects.get(pk=1)
<Article: Article object (1)>
```

```bash
# 없는 데이터 => 에러
>>> Article.objects.get(pk=100)
DoesNotExist: Article matching query does not exist.
    
# 여러개 있는 경우 => 에러
>>> Article.objects.get(title='제목')
```

### `filter` - 여러 데이터 조회

```bash
# title이 제목인 애들 다 반환
>>> Article.object.filter(title='제목')

Article.object.filter(title='찾아봐')[0].content
'내용입니다.'
```



## CRUD - Update

```bash
>>> a2 = Article.objects.get(pk=2)
>>> a2.title = '변경된 제목'
>>> a2.save()
```

## CRUD - Delete

- QuerySet의 모든 행에 대해 SQL 삭제 쿼리를 수행하고 삭제된 객체 수와 객체 유형당 삭제 수가 포함된 딕셔너리 반환

```bash
# 삭제는 저장을 안해도 된다!!
>>> a3 = Article.objects.get(pk=3)
>>> a2.delete()
```

## HTTP method

### `GET`

- 특정 리소스를 가져오도록 요청할 때 사용
- 반드시 데이터를 가져올 때만 사용해야 한다!!!
- DB에 변화를 주지 않고, `CRUD`에서 `R` 역할을 담당한다

### `POST`

- 서버로 데이터를 전송할 때 사용하고 리소스를 생성/변경하기 위해 데이터를 HTTP body에 담아 전송한다
- 서버에 변경사항을 만들고, `CRUD`에서 `C/U/D` 역할을 담당한다
- `GET`을 사용하면 `/article/create/?title=제목&content=내용` 의 형태로 서버 로그에 모든 정보가 주소록에 나타나게 된다. 따라서 정보를 숨겨 보내기 위해 `form` 태그의 `method` 속성의 값을 `GET`이 아닌 `POST`로 변경해야 한다. 일반적으로 보안이 요구되는 것을 요청할 때는 `POST` 형식으로 요청을 받아야 한다!

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
    # 1. 값 가져오기
    title = request.POST.get('title')
    content = request.POST.get('content')
   	....
    return redirect('articles:detail', article.pk)
```

### `CSRF`

- '사이트 간 요청 위조' 라는 공격 방법 중 하나로 Django는 기본적으로 이 공격을 방지할 수 있는 `CSRF token` 탬플릿을 제공한다.
- 사용자의 데이터에 임의의 난수 값을 부여해, 매 요청마다 해당 난수 값을 포함시켜 전송시키도록 하고 이후 서버에서 요청을 받을 때마다 전달된 token 값이 유효한지 검증한다.
- 일반적으로 데이터 변경이 가능한 POST, PATCH, DELETE Method 등에 적용 한다 (GET 제외)

```html
<form action="#" method="POST">
    {% csrf_token %}
    ....
</form>{}
```

- input type이 hidden으로 작성되며 value는 Django에서 생성한 hash 값으로 설정된다.
- 해당 태그 없이 요청을 보내면 Django 서버는 403 forbidden을 응답한다.





