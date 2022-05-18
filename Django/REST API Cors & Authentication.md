#  🪴 DRF Authentication

<br>

## 다양한 인증 방식

1. Session Based
2. Token Based
   - Basic Token
   - JWT
3. Oauth
   - google
   - facebook
   - github
   - ...

<br>

## Basic Token Authentication

![image-20220519005802345](REST%20API%20Cors%20&%20Authentication.assets/image-20220519005802345.png)

1. 클라이언트에서 아이디와 비밀번호를 입력해서 서버로 요청을 보낸다.
2. 서버는 클라이언트에서 넘어온 데이터와 데이터 베이스에 있는 데이터를 확인하고 동일하면 인증을 해줌
3. 2번에서 요청한 사용자에게 인증 토큰을 부여 (authtoken_token이라는 새로운 테이블에 생성)

4. 서버는 토큰을 클라이언트에 넘겨준다. 

5. 클라이언트는 이 토큰을 로컬 스토리지 같은데 잘 저장을 한다.

6. 이후 서버에 요청을 보낼 때 `Request Header`에 토큰을 같이 보낸다.

   ![image-20220519005648765](REST%20API%20Cors%20&%20Authentication.assets/image-20220519005648765-16528894112831.png)



7. 서버는 토큰을 보고 아 이사람이구나 확인을 하는 식..ㅋㅋㅋ

<br>

## JWT

- JSON 포맷을 활용하여 요소 간 안전하게 정보를 교환하기 위한 표준 포맷
- 암호화 알고리즘에 의한 디지털 서명이 되어 있기 때문에 JWT 자체로 검증 가능

- JWT 자체가 인증에 필요한 정보를 모두 갖기 때문에 DB를 사용하여 토큰의 유효성을 검사할 필요가 없음
- 하지만,,,!!
- 토큰 탈취시 서버 측에서 토큰 무효화가 불가능 (블랙리스팅 테이블 활용)
- 매우 짧은 유효기간(5min)과 Refresh 토큰을 활용하여 구현
- MSA(Micro Server Architecture) 구조에서 서버간 인증에 활용
- One Source(JWT) Multi Use 가능