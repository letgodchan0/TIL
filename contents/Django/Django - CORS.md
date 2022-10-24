# 🌱 Django - CORS



## Same-origin policy (SOP)

![image-20220519000739788](Django%20-%20CORS.assets/image-20220519000739788.png)

- 특정 출처(origin)에서 불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호작용 하는 것을 제한하는 보안 방식
- 잠재적으로 해로울 수 있는 문서를 분리함으로써 공격받을 수 있는 경로를 줄임
- 클라이언트와 서버 두 URL의 Protocol, Port, Host가 모두 같아야 동일한 출처라할 수 있음

<BR>

## Cross-Origin Resource Sharing (CORS)

- "교차 출처 리소스(자원) 공유"
- <span style="color:indianred">추가 HTTP header를 사용</span>하여, 특정 출처에서 실행중인 웹 애플리케이션이 <span style="color:indianred">다른 출처의 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제</span>

- 리소스가 자신의 출처 (Domain, Protocol, Port)와 다를 때 교차 출처 HTTP 요청을 실행
- 보안 상의 이유로 브라우저는 교차 출처 HTTP 요청을 제한 (SOP)
- 다른 출처의 리소스를 불러오려면 그 출처에서 올바른 CORS header를 포함한 응답을 반환해야 함

<br>

## How CORS ?

- CORS 표준에 의해 추가된 HTTP Header를 통해 이를 통제
- Access-Control-Allow-Origin 응답 헤더
  - 이 응답이 주어진 출처(origin)로 부터 요청 코드와 공유 될 수 있는지를 나타냄
  - `*`는 모든 도메인에서 접근할 수 있음을 의미
  - `*` 외에 특정 origin 하나를 명시할 수 있음

<br>

## CORS 시나리오 예시

1. https://localhost:8080 (Vue.js)의 웹컨텐츠가 https://api.vi.com(Django) 도메인 컨텐츠의 호출하기를 원하는 상황

2. 요청 헤더의 Origin을 보면 localhost:8080으로부터 요청이 왔다는 것을 알 수 있음

   - 이에 대한 응답으로 `Access-Control-Allow-Origin` 헤더를 다시 전송
   - 만약 서버 리소스 소유자가 오직 localhost:8080의 요청에 대해서만 리소스에 대한 접근을 허용하려는 경우 `*` 가 아닌 `Access-Control-Allow-Origin: localhost:8080`을 전송해야 함

   ![image-20220519001732021](Django%20-%20CORS.assets/image-20220519001732021.png)

<hr>

### 정리

1. Vue.js에서 서버로 요청
2. 서버는 `Access-Control-Allow-Origin` 에 특정한 origin을 포함시켜 응답
   - 서버는 CORS Policy와 직접적인 연관이 없고 그저 요청에 응답만 하는 응답무새
3. 브라우저는 응답에서 `Access-Control-Allow-Origin`을 확인한 후 허용 여부를 결정
4. 프레임워크 별로 이를 지원하는 라이브러리가 존재
   - django는 django-cors-headers 라이브러리를 통해 응답 헤더 및 추가 설정 가능

<br>

## `django-cors-headers` 라이브러리

- 응답에 CORS header를 추가해주는 라이브러리
- 다른 출처에서 보내는 Django 애플리케이션에 대한 브라우저 내 요청을 허용함
- Django App이 header 정보에 CORS를 설정한 상태로 응답을 줄 수 있게 도와주며, 이 설정을 통해 브라우저는 다른 origin에서 요청을 보내는 것이 가능해짐



1. ```bash
   $ pip install django-cors-headers
   ```

2. ```python
   # settings.py
   
   INSTALLED_APPS = [
       ...
       'corsheaders',
       ...
   ]
   
   MIDDLEWARE = [
       ...
       # CommonMiddleware보다 위에 위치
       'corsheaders.middleware.CorsMiddleware',
       ...
       'django.middleware.common.CommonMiddleware',
   ]
   
   # CORS_ALLOWED_ORIGINS에 교차 출처 자원 공유를 허용한 Domain 등록
   CORS_ALLOWED_ORIGINS = [
       'http://localhost:8080',
   ]
   ```

