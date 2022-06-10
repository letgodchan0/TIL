# 🧋 Java 클래스2

> 자바를 배우면서 새로 알게된 내용만 기술!

<br>

## ✔️ 패키지

- 클래스와 관련 있는 인터페이스들을 모아두기 위한 이름 공간.
- 시중에 나와 있는 패키지들과 구분되게 이름을 지어야 하고 일반적으로 소속이나 회사의 도메인을 사용한다.
- ```com.project_이름.module_이름```

<br>

### - 접근 제한자 

> 클래스, 멤버 변수, 멤버 메서드 등의 선언부에서 접근 허용 범위를 지정하는 역활의 키워드

|  수식어   | 클래스 내부 | 동일 패키지 | (다른 패키지 내의) 하위 클래스 | 다른 클래스 |
| :-------: | :---------: | :---------: | :----------------------------: | :---------: |
|  private  |      O      |             |                                |             |
| (default) |      O      |      O      |                                |             |
| protected |      O      |      O      |               O                |             |
|  public   |      O      |      O      |               O                |      O      |

- 그 외 제한자
  - static : 클래스 레벨의 요소 설정
  - final : 요소를 더 이상 수정할 수 없게 함
  - abstract : 추상 메서드 및 추상 클래스 작성

<br>

### - 접근자(getter) / 설정자(setter)

> 클래스에서 선언된 변수 중 접근제한에 의해 접근할 수 없는 변수의 경우 다른 클래스에서 접근할 수 없기 때문에, 접근하기 위한 메서드(setter/getter)를 public으로 선언하여 사용!

```java
public class Person {
    private String name;
    private int age;
    
    public String getName() {
        return name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        this.age = age;
    }
}
```

- private으로 변수를 선언하면 객체를 생성해도 접근할 수가 없다. 따라서 getter와 setter로 접근하도록 하는 것!
- Alt + Shift + S -> Generate Getters and Setters를 하면 자동 완성 시켜줌!

<br>

### - 싱글턴 패턴

> 소프트웨어 디자인 패턴에서 싱글턴 패턴을 따르는 클래스는, 생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고 최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴한다.

```java
public class Manager {
    
    private static Manager manager = new Manaver();
    
    private Manager() {}
    
    public static Manager getManager() {
        return manager;
    }
    
}
```

<br>



## ✔️ 상속

```java
public class Person {
    String name;
    
    public void eat() {
        System.out.println("음식을 먹는다.");
    }
}
// extends로 상속
public class Student extends Perosn {
    String major;
    
    public void study() {
        System.out.println("공부를 한다.");
    }
}
```

<br>

### - super

> super를 통해 부모 클래스의 생성자 호출

```java
public class Person {
    String name;
    
    public void eat() {
        System.out.println("음식을 먹는다.");
    }
}
// extends로 상속
public class Student extends Perosn {
    String major;
    
    public Student(String name, int age, String major){
        // 부모 클래스에 기본 생성자가 없는 경우 자식 클래스 생성자의 첫 줄에서, super 키워드를 이용해 명시적으로 부모 클래스의 생성자를 호출해줘야 됨
        // 자식 클래스의 생성자에서는 제일 처음 부모 클래스의 기본 생성자를 호출한다.
        super(name, age);	
        this.major = major
    }
    
    public void study() {
        super.eat();	// 부모 클래스 호출
        System.out.println("공부를 한다.");
    }
}

Student st = new Student("몰트", 4, "미술");
st.study();
// 아래와 같이 출력
음식을 먹는다.
공부를 한다.
```

<br>

### - 오버라이딩

> 자체로서의 동작은 없지만, 컴파일러가 아~ 너 오버라이드된 애구나 하면서 부모 클래스가서 한번 확인을 해주고 덮어씌워줌 즉, 안전하다ㅋㅋㅋㅋ 혹시 문제 있는지 컴파일러한테 체크하라고 하는 것

```java
public class Person {
    String name;
    
    public void eat() {
        System.out.println("음식을 먹는다.");
    }
}

public class Student extends Perosn {
    String major;
    
    public Student(String name, int age, String major){
        super(name, age);	
        this.major = major
    }
    
    @Override // 요기 오버라이드!
    public void study() {
        super.eat();
        System.out.println("공부를 한다.");
    }
}

Student st = new Student("몰트", 4, "미술");
```

- 상위 클래스에 선언된 메소드를 자식 클래스에서 재정의 하는 것.
- 메서드의 이름, 반환형, 매개변수 (타입, 개수, 순서)가 동일해야 한다.
- 하위 클래스의 접근제어자 범위가 상위 클래스보다 크거나 같아야 한다.

<br>

## ✔️ Object

> 가장 최상위 클래스로 모든 클래스의 조상 클래스



### - toStrig 메소드

> 객체를 문자열로 변경하는 메소드

```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}


public class Test{
    public int Age;
 
    @Override
    public String toString() {
        return "나이" + Age;
    }
}
```

- object의 메소드이기 때문에 오버라이딩(재정의)해서 내가 원하는 타입으로 반환 가능

<br>



### - equals 메소드

> 두 객체가 같은지를 비교하는 메소드, 참조 변수가 가리키는 값을 비교!

```java
Car car01 = new Car();

Car car02 = new Car();

 

System.out.println(car01.equals(car02));

car01 = car02; // 두 참조 변수가 같은 주소를 가리킴.

System.out.println(car01.equals(car02));

// 실행결과
false
true
```

<br>



## ✔️ 다형성

> 부모 클래스의 참조변수로 자식 클래스의 객체를 참조할 수 있다. but 실제 부모 영역만 접근이 가능하다.
>
> => 하나의 객체가 여러가지 타입을 가질 수 있는 것을 의미

```java
class Parent {}
class Child extends Parent {}

//
Parent p1 = new Parent();
Child c1 = new Child();
Parent p2 = new Child(); // 부모 클래스의 참조변수로 자식 클래스 객체 참조
Child c2 = new Parent(); // 에러
```

- 자식 클래스 타입의 참조 변수로는 부모 클래스 타입의 인스턴스를 참조할 수 없는데, 참조 변수가 사용할 수 있는 멤버의 개수가 실제 인스턴스의 멤버 개수보다 많기 때문이다. 
- 클래스는 상속을 통해 확장될 수는 있어도 축소될 수는 없으므로, 자식 클래스에서 사용할 수 있는 멤버의 개수가 언제나 부모 클래스와 같거나 많게 된다. 

<br>

### - 참조 변수의 타입 변환

```java
// 문법 => (변환 할 타입의 클래스 이름) 변환할 참조 변수

class Parent {}
class Child extends Parent {}
class Brother extends Parent {}

Parent p1 = null;
Child c = new Child();
Parent p2 = new Parent();
Brother b = null;

p1 = c;	// p = (Parent) c와 같고, 타입 변환을 생략할 수 있음
b = (Brother) p2; // 타입 변환을 생략할 수 없음
b = (Brother) c;  // 직접적인 상속관계가 아니기 때문에 에러
```

1. 서로 상속 관계에 있는 클래스 사이에만 타입 변환을 할 수 있음
2. 자식 클래스 타입에서 부모 클래스 타입으로의 타입 변환은 생략 가능
3. 하지만, 부모 클래스 타입에서 자식 클래스 타입으로의 타입 변환은 반드시 명시해야 함

<br>

### - instanceof 연산자

> 참조변수 `instanceof` 클래스 이름
>
> 왼쪽에 전달된 참조 변수가 실제로 참조하고 있는 인스턴스의 타입이 오른쪽에 전달된 클래스 타입이면 true 그렇지 않으면 false 반환한다. 참조 변수가 null을 가리키고 있으면 false 반환

```java
class Parent {}
class Child extends Parent {}
class Brother extends Parent {}

public class Test{
    public static void main(String[] args){
        Parent p = new Parent();
        
        System.out.println(p instanceof Object); // true
        System.out.println(p instanceof Parent); // true
        System.out.println(p instanceof Child);  // false
        
        Parent c = new Child();

        System.out.println(c instanceof Object); // true
        System.out.println(c instanceof Parent); // true
        System.out.println(c instanceof Child);  // true
    }
}
```

