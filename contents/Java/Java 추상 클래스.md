# 🧋 Java 추상 클래스

> 하나 이상의 추상 메소드를 포함하는 클래스를 추상 클래스라고 하며 객체 지향 프로그래밍에서 중요한 특징인 다형성을 가지는 메소드의 집합을 정의할 수 있도록 한다!

<br>

### ✔️ 추상 클래스 정의!

```java
public abstract class Chef {
    private String name;

    public void eat() {
        System.out.println("음식을 먹는다!")
    }
    public abstract void cook();  // 추상 메소드, 자식들아 오버라이딩해라~
}
```

- cook() 메소드는 어차피 자식 메소드에서 `override` 할 꺼라서 조상 클래스에서 구현하는 것이 무의미!
- 그래서 메소드의 선언부만 남기고 구현부는 ;(세미콜론)으로 대체 할 수 있다!
- 구현부가 없기 때문에 `abstract` 키워드를 메소드 선언부에 추가한다!

- 추상 메소드를 하나라도 가지고 있는 클래스는 의무적으로 추상 클래스가 되어서 `abstract`를 붙여야만 한다!

<br>

### ✔️ 추상 클래스 특징

- `abstract` 클래스는 상속 전용의 클래스
- 추상 클래스는 타입으로는 존재할 수 있지만, 구현부가 없는 메소드가 있으므로 객체를 생성할 수 없음!!!
- 추상 클래스를 상속받는 자식 클래스는 추상 메소드를 오버라이딩해야만 비로소 객체를 생성할 수 있다!

<br>

### ✔️ 추상 클래스 사용 이유

> abstract 클래스는 구현의 강제를 통해 프로그램의 안정성을 향상 시킨다. 

##### + 익명 클래스로 추상 클래스 객체화 시키는 법!

```java
chef c2 = new Chef(); // 에러

// but 선언과 동시에 미흡한 부분을 1회용 구현을 하면 가능
// 이것을 익명 클래스 문법이라 함 - 1회용 구현과 함께 객체화 가능
chef c2 = new Chef() {
    @Override
    public void cook() {
        System.out.println("추상 메소드의 1회용 구현!")
    }
}
```

<br>



### ✔️ 인터페이스 

> 다른 클래스를 작성할 때 기본이 되는 틀을 제공하면서, 다른 클래스 사이의 중간 매개 역할까지 담당하는 일종의 추상 클래스를 의미하며 클래스 내 모든 메소드가 추상 메소드임!

```java
public interface MyInterfACE {
    public static final int MEMBER1 = 10;
    int MEMBER2 = 10;
    
    public abstract void method1(int param);
    void method2(int param);
}
```

- 구현의 강제로 표준화 처리 (abstract 메소드 사용)
- 인터페이스를 통한 간접적인 클래스 사용으로 손쉬운 모듈 교체 지원
- 서로 상속의 관계가 없는 클래스들에게 인터페이스를 통한 관계 부여로 다형성 확장
- 모듈 간 독립적 프로그래밍 가능 -> 개발 기간 단축

<br>

#### 1. interface 키워드를 이용한여 선언

```java
public interface MyInterface {}
```

<br>

#### 2. 선언되는 변수는 모두 상수로 적용

```java
public static final int MEMBER1 = 10;
int MEMBER2 = 10;	// public static final 생략 가능
```

<br>

#### 3.선언되는 메소드는 모두 추상 메소드로 적용

```java
public abstract void method1(int param);
void method2(int param);	// interface로 선언했기 때문에 추상메소드로 적용!
```

<br>

#### 4. 객체 생성이 불가능!

```java
interface MyInterface {}

public class Main {
    MyInterface m = new MyInterface();	// 에러
}
```

<br>

#### 5. 클래스가 인터페이스를 상속할 경우, implements 키워드를 이용!

```java
interface Shape {}

class Circle implements Shape {}	// 여러개의 interface implements 가능
```

<br>

#### 6. 인터페이스를 상속받는 하위클래스는 추상 메소드를 반드시 오버라이딩(재정의) 해야 한다.  (구현하지 않을 경우 abstract 클래스로 표시해야함!)

```java
interface Chef {
    void cook();
}

class KFoodChef implements Chef {
    @Override
    public void cook() {
        System.out.println("한식 요리")
    }
}

// 또는
abstract class JFoodChef implements Chef{}
```

<br>

#### 7. 인터페이스 다형성 적용.

```java
public static void main(String[] args) {
    Chef chef = new KFoodChef();
}
```

