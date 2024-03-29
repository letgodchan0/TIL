# Bean Scope

Bean은 스프링에서 사용하는 POJO 기반 객체다.

상황과 필요에 따라 Bean을 사용할 때 하나만 만들어야 할 수도 있고, 여러개가 필요할 때도 있고, 어떤 한 시점에서만 사용해야할 때가 있을 수 있다. 이를 위해 Scope를 설정해서 Bean의 사용 범위를 개발자가 설정할 수 있다.

우선 따로 설정을 해주지 않으면, Spring에서 Bean은 `Singleton`으로 생성된다. 싱글톤 패턴처럼 특정 타입의 Bean을 딱 하나만 만들고 모두 공유해서 사용하기 위함이다. 보통은 Bean을 이렇게 하나만 만들어 사용하는 경우가 대부분이지만, 요구사항이나 구현에 따라 아닐 수도 있을 것이다. 따라서 Bean Scope는 싱글톤 말고도 여러가지를 지원해준다.

<br>

### Scope 종류

- #### singleton

  해당 Bean에 대해 IoC 컨테이너에서 단 하나의 객체로만 존재한다.

- #### prototype

  해당 Bean에 대해 다수의 객체가 존재할 수 있다.

- #### request

  해당 Bean에 대해 하나의 HTTP Request의 라이프사이클에서 단 하나의 객체로만 존재한다.

- #### session

  해당 Bean에 대해 하나의 HTTP Session의 라이프사이클에서 단 하나의 객체로만 존재한다.

- #### global session

  해당 Bean에 대해 하나의 Global HTTP Session의 라이프사이클에서 단 하나의 객체로만 존재한다.