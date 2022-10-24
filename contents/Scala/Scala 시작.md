# Scala



## 스칼라 설치

- [설치파일](https://www.scala-lang.org/download/)

```scala
object MyFirstScala {
    
    def main(args: Array[String]){
        println("Hello Scala")
    }
}
```

<br>

## 스칼라를 위한 프로그래밍 도구

- Scala Interpreter
  - 파이썬 등과 같이 인터프리터를 제공함
  - 즉각적인 피드백을 주기 때문에 작은 모듈 작성이나 분석용으로 활용하기 좋음
  - RELP (read-eval-print loop)
  - 엄밀히 말하면 인터프리터가 아니라 내부적으로 코드를 빠르게 컴파일 한 후 바이트 코드를 JVM(자바 가상 머신)에서 실행
- 외부 도구
  - IntelliJ
  - TypeSafe Activator
  - Scala IDE
  - SBT
- [IntelliJ 설치](https://www.jetbrains.com/ko-kr/idea/)

<br>

### 실습

```scala
object MainClass {
    def main(args: Array[String]){
        println("Hello IntelliJ IDEA")
    }
}
```

- 입력하면 바로 결과가 출력됨
  - scala> 1+1
  - res0: Int = 2
- 연산된 결과를 가르키는 이름
  - 자동으로 생성되는 resX이거나 사용자가 직접 작성한 이름

- 결과 타입
  - :과 표현식의 타입이 같이 출력
  - 자바와 달리 타입이 뒤에 표시됨
- 등호 (=)
  - 표현식을 통해 evaluate된 결과