### IOC(Inversion of Control)

한글로 직역하면 제어의 역전이라는 뜻이다. Spring 에서는 Container라는 개념이 있는데, 이 컨테이너는 객체를 담는 용기이다. bean의 생성부터 소멸까지 생명주기를 관리하게 된다.

> Container가 bean을 관리해주기에 제어의 역전이라고 한다.

쉽게 말해서 스프링컨테이너가 필요에 따라 개발자 대신에 bean을 관리해주는 행위라고 생각하면 된다.

 <br>



### AOP(Aspect Oriented Programming)

관점 지향 프로그래밍이라는 뜻으로 객체지향(OOP)과는 또다른 의미로 사용이된다.

여러 메서드에서 공통적으로 해야하는 일의 코드가 중복이 된다면 따로 모아서 관리를 하며 재활용을 하는것을 뜻한다. 여기서 상속의 개념이 좀 들어가게 되는데 여러 메서드에서 사용되는것을 인터페이스로 구현을 하고 implements로 구현을 받게 된다.

 <br>

 

### DI(Dependency Injection)

의존성주입을 뜻하는 것으로, 어떤 객체에 스프링 컨테이너가 또 다른 객체와의 의존성을 맺어주는것을 주입이라고 하면서 설명이 된다. IoC 컨테이너에서 빈 객체를 생성하는것이다.

대부분의 프레임워크에서는  IoC가 적용이 되긴 하지만 스프링에서는 di가 차별성이다.

