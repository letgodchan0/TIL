## AOP

![image-20230208114010228](Spring%20AOP.assets/image-20230208114010228.png)

#### - AOP가 필요한 상황

- 위와 같이 모든 메소드의 호출 시간을 측정하고 싶다면?
- 공통 관심 사항(cross-cuting concern) vs 핵심 관심 사항 (core concern)
- 회원 가입 시간, 회원 조회 시간을 측정하고 싶다면 아래와 같이 짜면 된다.

```java
MemberService
    
    // 회원 가입
    public Long join(Member member){
    long start = System.currentTimeMillis();
    
    try {
        // 비즈니스 로직
    }finally{
        long finish = System.currentTimeMillis();
        long timeMs = finish - start;
        System.println("join " + timeMs + "ms");
    }
}
```

문제

- 회원가입, 회원 조회에 시간을 측정하는 기능은 핵심 관심 사항이 아니다. 
- 시간을 측정하는 로직은 공통 관심 사항이다. 
- 시간을 측정하는 로직과 핵심 비즈니스의 로직이 섞여서 유지보수가 어렵다. 
- 시간을 측정하는 로직을 별도의 공통 로직으로 만들기 매우 어렵다. 
- 시간을 측정하는 로직을 변경할 때 (갑자기 초 단위로 변경해달래...) 모든 로직을 찾아가면서 변경해야 한다

<br>

#### - AOP 적용

![image-20230208120918039](Spring%20AOP.assets/image-20230208120918039.png)

- AOP : Aspect Oriented Programming
- 공통 관심 사항(cross-cuting concern)과 핵심 관심 사항 (core concern) 분리

- 아래와 같이 해결

<br>

#### AOP 적용 클래스 생성

```java
newproject/aop/TimeTraceAop
    
@Aspect
//@Component 이렇게 컴포넌트 스캔을 통해 빈에 등록할 수 있지만, SpringConfig에서 직접 등록할꺼야
public class TimeTraceAop {
    @Around("execution(* com.study.newproject..*(..))")
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        try {
            return joinPoint.proceed();
        }finally {
            long finish = System.currentTimeMillis();
            long timeMs = finish - start;
            System.out.println("END: " + joinPoint.toString() + " " + timeMs + "ms");
        }
    }
}
```

- `@Aspect` : 해당 클래스가 AOP라고 알려주는 어노테이션
- `@Around` : 어디에 적용할지 설정해주는 어노테이션

<br>

#### AOP 적용 클래스 빈에 직접 등록

```java
SpringConfig
    
@Configuration
public class SpringConfig {
	...

    @Bean
    public TimeTraceAop timeTraceAop() {
        return new TimeTraceAop();
    }
}
```

<br>

#### 문제 해결

- 회원가입, 회원 조회등 핵심 관심사항과 시간을 측정하는 공통 관심 사항을 분리한다. 
- 시간을 측정하는 로직을 별도의 공통 로직으로 만들었다. 
- 핵심 관심 사항을 깔끔하게 유지할 수 있다. 
- 변경이 필요하면 이 로직만 변경하면 된다. 
- 원하는 적용 대상을 선택할 수 있다.

<br>

### 스프링의 AOP 동작 방식

<HR>


#### - AOP 적용 전 의존 관계

![image-20230208121636328](Spring%20AOP.assets/image-20230208121636328.png)

- 스프링 적용 전에는 컨트롤러와 서비스가 의존관계에 있어서 그냥 호출했었다.

<br>

#### - AOP 적용 후 의존관계

![image-20230208121733908](Spring%20AOP.assets/image-20230208121733908.png)

- `memberService`에 AOP를 적용한다고 지정했을때,
- 스프링은 프록시라는 가짜 `memberService`를 만들어서 얘를 통해 AOP가 실행이 되고, `joinPoint,proceed()`를 통해 진짜 `memberService`가 호출이 된다.

![image-20230208122124728](Spring%20AOP.assets/image-20230208122124728.png)