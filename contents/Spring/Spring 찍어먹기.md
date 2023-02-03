# Spring 찍어먹기





### - 프로젝트 환경 설정

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

<br>

### - View 환경 설정

![image-20230203095421678](Spring%20%EC%B0%8D%EC%96%B4%EB%A8%B9%EA%B8%B0.assets/image-20230203095421678.png)

- controller 패키지 생성 후 `HelloController` 파일에 왼쪽과 같이 작성
- resources -> templates 하단에 `hello.html` 파일 생성후 오른쪽과 같이 작성
- 컨트롤러에서 리턴 값으로 문자를 반환하여 뷰 리졸버(viewResolver)가 화면을 찾아서 랜더링 해준다.
  - 스프링 부트 템플릿엔진 기본 viewName 매핑
  - `resources:templates/` + {ViewName}+`.html`

<br>

### - 빌드하고 실행하기

1. IDE 끄고 실행
2. cme에서 실행

```bash
gradlew // build하기

cd build
cd libs

java -jar NewProject-0.0.1-SNAPSHOT.jar // build 한거 실행
```

<br>

### - MVC와 템플릿 엔진

![image-20230203112709149](Spring%20%EC%B0%8D%EC%96%B4%EB%A8%B9%EA%B8%B0.assets/image-20230203112709149.png)

- `localhost:8080/hello-mcv`를 요청하면, 먼저 컨트롤러에서 `GetMapping` 어노테이션을 통해 이를 수행한다.
- `hello-template`를 리턴하게 되면, viewResolver `hello-template`라는 html파일을 templates 폴더에서 찾게 되고, 마지막으로 Thymeleaf 템플릿 엔진이 html 파일을 변환해서 렌더링한다.

<br>

### - API 방식

![image-20230203114644677](Spring%20%EC%B0%8D%EC%96%B4%EB%A8%B9%EA%B8%B0.assets/image-20230203114644677.png)

웹 브라우저에서 `hello-api`로 요청 보내면, 톰켓 내장 서버에서 hello-api가 왔다고 스프링에 던짐 -> 스프링은 hello-api가 있네? 근데 responsebody라는 어노테이션이 있네? http 응답에 그대로 데이터를 넣을꺼야, 근데 객체가 오면 디폴트로 json으로 반환하겠다는게 정책임

hello 객체를 넘기면, `HttpMessageConverter`가 동작을 함, 얘가 단순 문자면 StringConverter가 동작하고, 객체면 JsonConverter이 동작해서, 객체를 json 스타일로 바꿔서, 요청한 애한테 반환해줌

`@ResponseBody 를 사용` 

- HTTP의 BODY에 문자 내용을 직접 반환 
- viewResolver 대신에 HttpMessageConverter 가 동작 
- 기본 문자처리: StringHttpMessageConverter 
- 기본 객체처리: MappingJackson2HttpMessageConverter 
- byte 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있음