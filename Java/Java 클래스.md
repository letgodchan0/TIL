# 🧋 Java 클래스

> 자바에서 클래스란 객체를 정의하는 틀 또는 설계도와 같은 의미! 클래스를 가지고 여러 객체를 생성하여 사용하게 된다. 클래스는 객체의 상태를 나타내는 필드(변수)와 객체의 행동을 나타내는 메소드로 구성된다.

객체 : 하나의 역할을 수행하는 메소드와 변수(데이터)의 묶음

객체지향 프로그래밍 : 프로그램을 단순히 데이터와 처리 방법으로 나누는 것이 아니라, 프로그램을 수많은 객체(object)라는 기본 단위로 나누고 이들의 상호작용으로 서술하는 방식 

<br>

## ✔️ 클래스

### - 클래스 선언

```java
public class TV {
    // 속성 (Attribute) - 멤버 변수
    int channel;
    int volumn;
    
 	// 생성자
    Tv(int channel, int volumn){
        this.channel = channel;
        this.volumn = volumn;
    }
    
    // 동작 (Behavior) - 메소드, 함수
    public void channelUp() {
        
    }
}
```

<br>



### - 메소드 정의 및 호출

```java
class Tv {
    int channel;
    // 생성자
    Tv(int channel){
    	this.channel = channel;
    }
    // 메소드 정의 void
    public void showchannel(){
        System.out.println(channel);
    }
    // 메소드 정의 int
    public int channelUp(int size) {
        return this.channel + size;
    }
}

public class Test {
	public static void main(String[] args) {
		Tv tv = new Tv(10);
		tv.showchannel();	// 메소드 호출
		System.out.println(tv.channelUp(10));
	}
}
```



<br>

### - static

> 객체를 생성하지 않아도 존재하는 변수나 함수로 `클래스이름.메소드이름`으로 호출해야 한다.

```java
class Test {
    // 클래스 메소드 (static 메소드)
    public static void print(){
        System.out.println("static임")
    }
    public void print2(){
        System.out.println("인스턴스 메소드임")
    }
}
public class Test {
    public static void main(String[] args) {
        Test.print(); // 객체를 생성하지 않아도 호출 가능
        
        Test t = new Test(); // 객체 생성
        t.print2()  // 객체를 생성해야만 호출 가능
    }
}
```

<br>

## ✔️ 생성자

```java
public class Dog {
    Dog() {
       System.out.println("나는 생성자, 클래스랑 이름이 같고, 반환타입이 없음") 
    }
}
```

- 객체의 생성과 동시에 인스턴스 변수를 원하는 값으로 초기화할 수 있도록 하는 메소드!
- 객체를 생성할 때 속성의 초기화를 담당하게 한다. 
- 클래스명과 이름이 동일
- 반환 타입이 없음, but 반환 타입을 void형으로 선언하지 않음

<br>



### - 디폴트 생성자

```java
class Dog {
   생성자가 하나도 없는 상태, JVM이 자동으로 제공한다!
}

// 예시

class Main {
    public static void main(String [] a){
        // 객체 생성
        Dog d = new Dog();  // 여기서 new 키워드를 사용하 객체를 생성할 때 생성자를 사용가능하도록 함!
    }
}
```

<br>



### - 생성자는 오버로딩 지원!

```java
public class Dog {
    Dog() {}
    Dog(String name) {}
	Dog(int age) {}
	Dog(String name, int age) {}
}

// 예시

class Main {
    public static void main(String [] a) {
        Dog d = new Dog();
        Dog d2 = new Dog("몰트");
        Dog d3 = new Dog(5);
        Dog d4 = new Dog("미르", 1)
    }
}
```

- 클래스 내에 메소드 이름이 같고 매개변수의 타입 또는 개수가 다른 것!

<br>

### - 생성자 / this() 메소드

```java
public class Dog {
    private int age;
    private String name;
    
    // 첫 번째 생성자
    Dog(String name, int, age){
        this.name = name;
        this.age = age;
    }
    
    // 두 번째 생성자 - 첫 번째 생성자 호출
    Dog() {
        this("몰트", 3);
    }
	
    public String getDog() {
        return this.name
    }
}

//
public class Practice {
    public static void main(String[] args) {
        Dog d = new Dog();
        System.out.println(d.getModel());	// 몰트가 출력된다!
    }
}
```

- 한 생성자에서 다른 생성자를 호출 할 때에는 반드시 생성자의 첫 줄에서만 호출할 수 있다!!

