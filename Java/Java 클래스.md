# π§ Java ν΄λμ€

> μλ°μμ ν΄λμ€λ κ°μ²΄λ₯Ό μ μνλ ν λλ μ€κ³λμ κ°μ μλ―Έ! ν΄λμ€λ₯Ό κ°μ§κ³  μ¬λ¬ κ°μ²΄λ₯Ό μμ±νμ¬ μ¬μ©νκ² λλ€. ν΄λμ€λ κ°μ²΄μ μνλ₯Ό λνλ΄λ νλ(λ³μ)μ κ°μ²΄μ νλμ λνλ΄λ λ©μλλ‘ κ΅¬μ±λλ€.

κ°μ²΄ : νλμ μ­ν μ μννλ λ©μλμ λ³μ(λ°μ΄ν°)μ λ¬Άμ

κ°μ²΄μ§ν₯ νλ‘κ·Έλλ° : νλ‘κ·Έλ¨μ λ¨μν λ°μ΄ν°μ μ²λ¦¬ λ°©λ²μΌλ‘ λλλ κ²μ΄ μλλΌ, νλ‘κ·Έλ¨μ μλ§μ κ°μ²΄(object)λΌλ κΈ°λ³Έ λ¨μλ‘ λλκ³  μ΄λ€μ μνΈμμ©μΌλ‘ μμ νλ λ°©μ 

<br>

## βοΈ ν΄λμ€

### - ν΄λμ€ μ μΈ

```java
public class TV {
    // μμ± (Attribute) - λ©€λ² λ³μ
    int channel;
    int volumn;
    
 	// μμ±μ
    Tv(int channel, int volumn){
        this.channel = channel;
        this.volumn = volumn;
    }
    
    // λμ (Behavior) - λ©μλ, ν¨μ
    public void channelUp() {
        
    }
}
```

<br>



### - λ©μλ μ μ λ° νΈμΆ

```java
class Tv {
    int channel;
    // μμ±μ
    Tv(int channel){
    	this.channel = channel;
    }
    // λ©μλ μ μ void
    public void showchannel(){
        System.out.println(channel);
    }
    // λ©μλ μ μ int
    public int channelUp(int size) {
        return this.channel + size;
    }
}

public class Test {
	public static void main(String[] args) {
		Tv tv = new Tv(10);
		tv.showchannel();	// λ©μλ νΈμΆ
		System.out.println(tv.channelUp(10));
	}
}
```



<br>

### - static

> κ°μ²΄λ₯Ό μμ±νμ§ μμλ μ‘΄μ¬νλ λ³μλ ν¨μλ‘ `ν΄λμ€μ΄λ¦.λ©μλμ΄λ¦`μΌλ‘ νΈμΆν΄μΌ νλ€.

```java
class Test {
    // ν΄λμ€ λ©μλ (static λ©μλ)
    public static void print(){
        System.out.println("staticμ")
    }
    public void print2(){
        System.out.println("μΈμ€ν΄μ€ λ©μλμ")
    }
}
public class Test {
    public static void main(String[] args) {
        Test.print(); // κ°μ²΄λ₯Ό μμ±νμ§ μμλ νΈμΆ κ°λ₯
        
        Test t = new Test(); // κ°μ²΄ μμ±
        t.print2()  // κ°μ²΄λ₯Ό μμ±ν΄μΌλ§ νΈμΆ κ°λ₯
    }
}
```

<br>

## βοΈ μμ±μ

```java
public class Dog {
    Dog() {
       System.out.println("λλ μμ±μ, ν΄λμ€λ μ΄λ¦μ΄ κ°κ³ , λ°ννμμ΄ μμ") 
    }
}
```

- κ°μ²΄μ μμ±κ³Ό λμμ μΈμ€ν΄μ€ λ³μλ₯Ό μνλ κ°μΌλ‘ μ΄κΈ°νν  μ μλλ‘ νλ λ©μλ!
- κ°μ²΄λ₯Ό μμ±ν  λ μμ±μ μ΄κΈ°νλ₯Ό λ΄λΉνκ² νλ€. 
- ν΄λμ€λͺκ³Ό μ΄λ¦μ΄ λμΌ
- λ°ν νμμ΄ μμ, but λ°ν νμμ voidνμΌλ‘ μ μΈνμ§ μμ

<br>



### - λν΄νΈ μμ±μ

```java
class Dog {
   μμ±μκ° νλλ μλ μν, JVMμ΄ μλμΌλ‘ μ κ³΅νλ€!
}

// μμ

class Main {
    public static void main(String [] a){
        // κ°μ²΄ μμ±
        Dog d = new Dog();  // μ¬κΈ°μ new ν€μλλ₯Ό μ¬μ©ν κ°μ²΄λ₯Ό μμ±ν  λ μμ±μλ₯Ό μ¬μ©κ°λ₯νλλ‘ ν¨!
    }
}
```

<br>



### - μμ±μλ μ€λ²λ‘λ© μ§μ!

```java
public class Dog {
    Dog() {}
    Dog(String name) {}
	Dog(int age) {}
	Dog(String name, int age) {}
}

// μμ

class Main {
    public static void main(String [] a) {
        Dog d = new Dog();
        Dog d2 = new Dog("λͺ°νΈ");
        Dog d3 = new Dog(5);
        Dog d4 = new Dog("λ―Έλ₯΄", 1)
    }
}
```

- ν΄λμ€ λ΄μ λ©μλ μ΄λ¦μ΄ κ°κ³  λ§€κ°λ³μμ νμ λλ κ°μκ° λ€λ₯Έ κ²!

<br>

### - μμ±μ / this() λ©μλ

```java
public class Dog {
    private int age;
    private String name;
    
    // μ²« λ²μ§Έ μμ±μ
    Dog(String name, int, age){
        this.name = name;
        this.age = age;
    }
    
    // λ λ²μ§Έ μμ±μ - μ²« λ²μ§Έ μμ±μ νΈμΆ
    Dog() {
        this("λͺ°νΈ", 3);
    }
	
    public String getDog() {
        return this.name
    }
}

//
public class Practice {
    public static void main(String[] args) {
        Dog d = new Dog();
        System.out.println(d.getModel());	// λͺ°νΈκ° μΆλ ₯λλ€!
    }
}
```

- ν μμ±μμμ λ€λ₯Έ μμ±μλ₯Ό νΈμΆ ν  λμλ λ°λμ μμ±μμ μ²« μ€μμλ§ νΈμΆν  μ μλ€!!

