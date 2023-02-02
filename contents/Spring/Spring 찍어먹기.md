# Spring 찍어먹기



## - 프로젝트 생성

##### 1. [여기서 편하게 생성할 수 있다!](https://start.spring.io/) - spring 관련 프로젝트를 만들어 주는 사이트

![image-20230202200156157](C:/Users/livem/AppData/Roaming/Typora/typora-user-images/image-20230202200156157.png)

<br>

##### 2. 인텔리에서 직접 생성하기

(1) New Project

![image-20230203002600422](Spring%20%EC%B0%8D%EC%96%B4%EB%A8%B9%EA%B8%B0.assets/image-20230203002600422.png)

- Gradle, Maven : 필요한 라이브러리를 땡겨서 오고, 빌드하는 라이프사이클을 관리해주는 툴

- Group : 보통 기업명 같은거 적음

- Artifact : 프로젝트 명

<br>

(2) Dependencies 추가

![image-20230203002741826](Spring%20%EC%B0%8D%EC%96%B4%EB%A8%B9%EA%B8%B0.assets/image-20230203002741826.png)

- Dependencies : 스프링 부트 기반으로 프로젝트 시작할 때 어떤 라이브러리를 땡겨서 쓸꺼야?
  - Spring Web
  - Spring Web Services
  - Thymeleaf 
  - Lombok

<br>

(3) 성공

![image-20230203004451180](Spring%20%EC%B0%8D%EC%96%B4%EB%A8%B9%EA%B8%B0.assets/image-20230203004451180.png)

서버 실행도 해보면! 아래와 같이 잘 나오는 것을 확인할 수 있다!

![image-20230203004625394](Spring%20%EC%B0%8D%EC%96%B4%EB%A8%B9%EA%B8%B0.assets/image-20230203004625394.png)



<br>

(4) 참고

![image-20230203000847744](Spring%20%EC%B0%8D%EC%96%B4%EB%A8%B9%EA%B8%B0.assets/image-20230203000847744.png)

- Build and run using과 Run tests using을 인텔리로 바꿔주자

- Gradle 통해서 실행하면 느릴 때가 있다. 따라서 인텔리로 설정하면, Gradle 통하지 않고 인텔리에서 자바를 바로 돌려서 더 빨리 된다.