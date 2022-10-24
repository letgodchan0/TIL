# 🌱 Django - Model

## Model

> 사용자가 저장하는 데이터들의 필수적인 필드들과 동작들을 포함하는 저장된 데이터베이스의 구조
>
> Django는 model을 통해 데이터에 접속하고 관리하며 일반적으로 각각의 model은 하나의 데이터베이스 테이블에 매핑 된다!

## Database - 체계화된 데이터의 모임

- 쿼리 - 데이터를 조회하기 위한 명령어, 조건에 맞는 데이터를 추출하거나 조작하는 명령어
- 스키마 - 데이터베이스의 구조와 제약 조건(자료의 구조, 표현의 방법, 관계)에 관련한 전반적인 명세를 기술한 것
- 테이블 
  - 열(column) : 필드 or 속성
  - 행(row) : 레코드 or 튜플

## Models.py

```python
# 각 모델은 장고 내장 모듈인 models를 가져와 Model 클래스의 서브 클래스로 표현된다
from django.db import models

class Article(models.Model):
    # title과 content은 DB의 컬럼
    title = models.CharField(max_length=30)
    content = models.TextField()
```

- `CharField(max_length=None, **options)`
  - 길이의 제한이 있는 문자열을 넣을 때 사용
  - CharField의 max_length는 필수 인자
  - **필드의 최대 길이(문자),** DB 레벨과 Django의 유효성 검사(값을 검증하는 것)에서 활용
- `TextField(**options)`
  - 글자의 수가 많을 때 사용
  - max_length 옵션 작성시 자동 필드인 testarea 위젯에 반영은 되지만 모델과 DB 수준에는 적용 X

## Models.py 수정

```python
# 다음과 같이 Article 클래스에 수정한다면,
from django.db import models

class Article(models.Model):
    title = models.CharField(max_length=30)
    content = models.TextField()
    # 속성 추가
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```

- 아래와 같은 구문이 나오게 되는데 기존에 DB에는 `title`과 `content` 속성의 값들로만 튜플이 구성되어 있는데, 새로운 속성들이 추가 된다면 이전에 있던 튜플에도 새로운 속성에 해당하는 값을 부여해주어야 하는 DB의 무결성 원칙을 따라야 한다!

![image-20220311013544512](Django%20-%20Model.assets/image-20220311013544512.png)

- `created_at` 필드에 대한 default 값 설정으로 1을 입력후 enter
  - 옵션 1) 내가 지금 직접 디폴트 값을 줄게 => 여기서는 1번 선택함
  - 옵션 2) 미안. 나 종료하고 models.py에서 내가 직접 설정할게

![image-20220311013555485](Django%20-%20Model.assets/image-20220311013555485.png)

- `timezone.now` 함수값 자동설정 => 빈 값 상태에서 enter 클릭 => migrate를 통해 models.py 수정사항 반영
- `auto_now_add`
  - 최초 생성 일자
  - Django ORM이 최초 insert(테이블에 데이터 입력)시에만 현재 날짜와 시간으로 갱신 (테이블에 어떤 값을 최초로 넣을 때)
- `auto_now`
  - 최종 수정 일자
  - Django ORM이 save를 할 때마다 현재 날짜와 시간으로 갱신

## Migrations

>"Django가 model에 생긴 변화를 반영하는 방법"

1. ```bash
   $ python manage.py makemigrations
   ```

  - model을 변경한 것에 기반한 새로운 마이그레이션(like 설계도)을 만들 때 사용
  - 'migrations/0001_initial.py' 생성 확인

2. ```bash
   $ python manage.py migrate
   ```

- 마이그레이션을 DB에 반영하기 위해 사용
- 설계도를 실제 DB에 반영하는 과정
- 모델에서의 변경 사항들과 DB의 스키마가 동기화를 이룸
- '0001_initial.py' 설계도를 실제 DB에 반영

3. ```bash
   $ python manage.py sqlmigrate app_name 0001
   ```

- 마이그레이션에 대한 SQL 구문을 보기 위해 사용
- 해당 마이그레이션이 SQL 문으로 어떻게 해석되어서 동작할지 미리 확인 할 수 있음

4. ```bash
   $ python manage.py showmigrations
   ```

- 프로젝트 전체의 마이그레이션 상태를 확인하기 위해 사용
- 마이그레이션 파일들이 migrate 됐는지 안됐는지 여부를 확인 할 수 있음

- migrations 설계도들이 migrate 됐는지 안됐는지 여부를 확인할 수 있음 [X] => 마이그레이션 됐음

## Admin Site

- 사용자가 아닌 서버의 관리자가 활용하기 위한 페이지로 `django.contrib.auth` 모듈에서 제공됨
- Model class를 admin.py에 등록하고 관리하고 record 생성 여부 확인에 매우 유용하며, 직접 record 삽입 가능

## admin 생성

```bash
$ python manage.py createsuperuser
```

- 관리자 계정 생성 후 서버를 실행한 다음 `'/admin'` 으로 가서 관리자 페이지로 로그인
  - 계정만 만든 경우 Django 관리자 화면에서 아무것도 보이지 않음
- 내가 만든 Model을 보기 위해서는 admin.py에 작성하여 Django 서버에 등록
- [주의] auth에 관련된 기본 테이블이 생성되지 않으면 관리자 계정을 생성할 수 없음

![image-20220311003026402](Django%20-%20Model.assets/image-20220311003026402.png)

## admin 등록

```python
# app폴더/admin.py
from django.contrib import admin
# 내가 만든 모델 추가
from .models import Article

# admin site에 register
admin.site.register(Article)
```

- `admin.py`는 관리자 사이트에 Article 객체가 관리자 인터페이스를 가지고 있다는 것을 알려주는 것
- `models.py` 에 정의한 `__str__`의 형태로 객체가 표현됨

## admin 옵션

```python
# app폴더/admin.py
from django.contrib import admin
from .models import Article

class ArticleAdmin(admin.ModelAdmin):
    list_display = ('pk', 'title', 'content', 'created_at', 'updated_at',)

# admin site에 register
admin.site.register(Article)
```

- `list_display` - models.py에 정의한 각각의 속성 들의 값(레코드)을 admin 페이지에 출력하도록 설정

- `list_filter`, `list_display_links` 등 다양한 [ModelAdmin options](https://docs.djangoproject.com/en/3.2/ref/contrib/admin/#modeladmin-options) 참고