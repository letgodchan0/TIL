# Springboot 동작 원리

### 1. 정적페이지 vs 동적 페이지

정적 페이지란 서버에 미리 저장된 파일(HTML 파일, 이미지, JavaScript 파일 등)이 그대로 전달되는 웹 페이지를 말하며, 서버에 저장된 데이터가 수정되지 않는 한 항상 동일한 페이지를 반환한다.

반면, 동적 페이지는 서버에 있는 데이터들을 스크립트에 의해 가공처리한 후 생성되어 전달되는 웹 페이지를 말한다. 서버는 사용자의 요청을 해석하여 데이터를 가공한 후 생성된 웹 페이지를 반환하며, 사용자는 상황, 시간, 요청 등에 따라 달라지는 웹 페이지를 보게 된다.

<br>

### 2. 웹 서버

웹서버는 HTTP 프로토콜을 기반으로 하여 클라이언트(웹 브라우저 또는 웹 크롤러)의 요청을 서비스하는 기능을 담당한다.

웹서버는 정적인 컨텐츠를 제공하고, WAS를 거치지 않고 바로 자원을 제공한다. 또한 동적인 컨텐츠 제공을 위해 클라이언트의 요청을 WAS로 보내고, WAS가 처리한 결과를 클라이언트에게 전달(응답)하는 역할을 한다.

웹서버의 종류로는 Apache, Nginx 등이 있다.

<br>

### 3. WAS(Web Application Server)

> DB 조회나 다양한 로직 처리를 요구하는 동적인 컨텐츠를 제공하기 위해 만들어진 Application Server

웹 페이지는 정적 컨텐츠와 동적 컨텐츠가 모두 존재한다. 사용자의 요청에 맞게 적절한 동적 컨텐츠를 만들어서 제공해야 하지만, 웹서버만을 이용한다면 사용자가 원하는 요청에 대한 결과값을 모두 미리 만들어놓아야 한다. 그러나 이렇게 하기에는 자원이 절대적으로 부족하기에 WAS를 통해 요청에 맞는 데이터를 DB에서 가져와서 그때 그때 결과를 만들어 제공함으로써 자원을 효율적으로 사용할 수 있다.

톰캣(Apache Tomcat)은 대표적인 WAS 중 하나로, JSP 페이지의 실행환경을 제공하는 웹 어플리케이션 서버이다. 톰캣은 자바코드를 이용해 HTML 페이지를 동적으로 생성한다. 웹 컨테이너(Web Container) 또는 서블릿 컨테이너(Servlet Container)라고도 한다.

<br>

##### - 웹 서버와 WAS를 분리하는 이유

WAS는 기본적으로 동적 컨텐츠를 제공하기 위해 존재하는 서버이다. DB 조회나 다양한 로직을 처리하느라 바쁜 WAS가 정적 컨텐츠 요청까지 WAS가 처리한다면, 그로 인한 부하가 더 커지게 되고, 수행 속도가 느려져 페이지 노출 시간이 늘어나게 될 것이다. 따라서 단순 정적 컨텐츠는 웹서버에서 빠르게 클라이언트에게 제공하도록 기능을 분리하는 것이 좋다.

즉, **자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성을 위해 Web Server와 WAS를 분리**한다.

<br>

#### - Dispatcher Servlet

>  Dispatcher Servlet은 클라이언트의 모든 요청을 한 곳으로 받아서 처리합니다. 요청에 맞는 Handler로 요청을 전달하고 실행결과를 Http Response 형태로 만들어서 반환합니다.

![image-20221028044812871](https://github.com/letgodchan0/TIL/blob/main/contents/springboot/Springboot%20%EB%8F%99%EC%9E%91%20%EC%9B%90%EB%A6%AC.assets/image-20221028044812871.png?raw=true)

Spring MVC에서는 위와 같은 구조를 가진다. 클라이언트의 요청을 DispatcherServlet이 받아 Handler Mapping이나 Controller에 전달하고 처리된 결과 값을 Model 형태로 받는다. 최종적으로 클라이언트에게 보여주고자 하는 페이지 포맷에 따라 ViewResolver가 페이지(View)를 생성하고 페이지에 Model을 포함시켜 반환하게 된다.(ModelAndView).

<br>

![image-20221028044911343](https://github.com/letgodchan0/TIL/blob/main/contents/springboot/Springboot%20%EB%8F%99%EC%9E%91%20%EC%9B%90%EB%A6%AC.assets/image-20221028044859975.png?raw=true)

Spring Boot 기반의 RESTful Web Services의 경우, 클라이언트에게 보여지는 형태의 서비스가 아니라 클라이언트의 요청을 JSON 또는 XML 형태의 데이터 포맷으로 반환한다. 즉, Spring MVC의 View 형태의 페이지를 생성할 필요가 없다. 이렇게 클라이언트에게 보여지는 페이지를 가지지 않는 Controller를 REST Controller라고 한다.

<br>