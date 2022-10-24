# Scala



## 스칼라란?

- Martin Odersky 교수
  - 자바 Generics를 만듬
  - 자바의 한계를 느끼고 함수형 언어의 장점을 반영한 스칼라를 만들었음
- Scala는 확장 가능한 언어
- JVM 위에서 실행
  - JVM bytecode로 컴파일되어 실행
  - 자바와 혼용 가능
  - 자바와 동일한 성능

<br>

## 스칼라의 특징

- 객체 지향과 함수형 언어의 특징을 모두 가지고 있음
- 객체지향 프로그래밍
  - 컴퓨터 프로그래밍 패러다임의 하나로 프로그램을 명령어 리스트로 보는 것이 아니라 여러 개의 독립된 단위(객체)로 보는 것
  - 객체는 데이터와 연산을 하나의 단위로 묶은 것이며 각 객체를 포함시키거나 넘기는 등이 자유로움
  - 프로그램을 유연하고 변경이 유용한 구조
  - 대규모 소프트웨어 개발에 유리함

- 스칼라에서의 객체지향 프로그래밍
  - 스칼라는 객체지향의 순수한 형태를 지니고 있음
  - 모든 값은 객체고 모든 오퍼레이션은 메서드 호출
  - 숫자, 함수 등도 모두 객체

- 함수형 프로그래밍 정의

  - 자료 처리를 수학적 함수의 계산으로 취급하고 상태와 가변 데이터를 멀리하는 프로그래밍 패러다임

- 함수형 프로그래밍의 특징

  - 메소드 수행 시 input을 받고 output만 리턴하며, 값의 변경을 내부에서 하지 않음 => 갑 변경 최소화는 버그의 가능성을 낮춰줌

  - 함수는 1급 계층(First Class) 값

- 정적타입 시스템의 이점
  - 검증할 수 있는 프로퍼티 : 런타임오류의 부재를 검증할 수 있다.
  - 안전한 리팩토링
  - 문서화

- 풍부한 표 현력
  - 1급 함수
  - 클로저

- 짧은 코드 지향

  - 간결한 코드는 버그 가능성을 줄여주고
  - 타입 추론
  - 강력한 리터럴
  - 고수준 언어

- `JAVA` vs `Scala` 1

  ```java
  //Java
  class SampleClass{
      private int first;
      private String second;
      
      public SampleClass(int first, String second) {
          this.first = first;
          this.second = second;
      }
  }
  ```

  ```scala
  //Scala
  // 주 생성자 (Primary Constructor)
  class Sample(first: int, second: String)
  ```

- `JAVA` vs `Scala` 2

  ```java
  //Java
  boolean nameHasUpperCase = false;
  for (int loop = 0; loop < name.length(); loop++){
      if (Character.isUpperCase(name.charAt(loop))){
          nameHasUpperCase = true;
          break;
      }
  }
  ```

  ```scala
  //Scala
  val nameHasUpperCase = name.exists(_.isUpper)
  ```

  

