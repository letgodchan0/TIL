# π§ Java ν΄λμ€2

> μλ°λ₯Ό λ°°μ°λ©΄μ μλ‘ μκ²λ λ΄μ©λ§ κΈ°μ !

<br>

## βοΈ ν¨ν€μ§

- ν΄λμ€μ κ΄λ ¨ μλ μΈν°νμ΄μ€λ€μ λͺ¨μλκΈ° μν μ΄λ¦ κ³΅κ°.
- μμ€μ λμ μλ ν¨ν€μ§λ€κ³Ό κ΅¬λΆλκ² μ΄λ¦μ μ§μ΄μΌ νκ³  μΌλ°μ μΌλ‘ μμμ΄λ νμ¬μ λλ©μΈμ μ¬μ©νλ€.
- ```com.project_μ΄λ¦.module_μ΄λ¦```

<br>

### - μ κ·Ό μ νμ 

> ν΄λμ€, λ©€λ² λ³μ, λ©€λ² λ©μλ λ±μ μ μΈλΆμμ μ κ·Ό νμ© λ²μλ₯Ό μ§μ νλ μ­νμ ν€μλ

|  μμμ΄   | ν΄λμ€ λ΄λΆ | λμΌ ν¨ν€μ§ | (λ€λ₯Έ ν¨ν€μ§ λ΄μ) νμ ν΄λμ€ | λ€λ₯Έ ν΄λμ€ |
| :-------: | :---------: | :---------: | :----------------------------: | :---------: |
|  private  |      O      |             |                                |             |
| (default) |      O      |      O      |                                |             |
| protected |      O      |      O      |               O                |             |
|  public   |      O      |      O      |               O                |      O      |

- κ·Έ μΈ μ νμ
  - static : ν΄λμ€ λ λ²¨μ μμ μ€μ 
  - final : μμλ₯Ό λ μ΄μ μμ ν  μ μκ² ν¨
  - abstract : μΆμ λ©μλ λ° μΆμ ν΄λμ€ μμ±

<br>

### - μ κ·Όμ(getter) / μ€μ μ(setter)

> ν΄λμ€μμ μ μΈλ λ³μ μ€ μ κ·Όμ νμ μν΄ μ κ·Όν  μ μλ λ³μμ κ²½μ° λ€λ₯Έ ν΄λμ€μμ μ κ·Όν  μ μκΈ° λλ¬Έμ, μ κ·ΌνκΈ° μν λ©μλ(setter/getter)λ₯Ό publicμΌλ‘ μ μΈνμ¬ μ¬μ©!

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

- privateμΌλ‘ λ³μλ₯Ό μ μΈνλ©΄ κ°μ²΄λ₯Ό μμ±ν΄λ μ κ·Όν  μκ° μλ€. λ°λΌμ getterμ setterλ‘ μ κ·Όνλλ‘ νλ κ²!
- Alt + Shift + S -> Generate Getters and Settersλ₯Ό νλ©΄ μλ μμ± μμΌμ€!

<br>

### - μ±κΈν΄ ν¨ν΄

> μννΈμ¨μ΄ λμμΈ ν¨ν΄μμ μ±κΈν΄ ν¨ν΄μ λ°λ₯΄λ ν΄λμ€λ, μμ±μκ° μ¬λ¬ μ°¨λ‘ νΈμΆλλλΌλ μ€μ λ‘ μμ±λλ κ°μ²΄λ νλμ΄κ³  μ΅μ΄ μμ± μ΄νμ νΈμΆλ μμ±μλ μ΅μ΄μ μμ±μκ° μμ±ν κ°μ²΄λ₯Ό λ¦¬ν΄νλ€.

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



## βοΈ μμ

```java
public class Person {
    String name;
    
    public void eat() {
        System.out.println("μμμ λ¨Ήλλ€.");
    }
}
// extendsλ‘ μμ
public class Student extends Perosn {
    String major;
    
    public void study() {
        System.out.println("κ³΅λΆλ₯Ό νλ€.");
    }
}
```

<br>

### - super

> superλ₯Ό ν΅ν΄ λΆλͺ¨ ν΄λμ€μ μμ±μ νΈμΆ

```java
public class Person {
    String name;
    
    public void eat() {
        System.out.println("μμμ λ¨Ήλλ€.");
    }
}
// extendsλ‘ μμ
public class Student extends Perosn {
    String major;
    
    public Student(String name, int age, String major){
        // λΆλͺ¨ ν΄λμ€μ κΈ°λ³Έ μμ±μκ° μλ κ²½μ° μμ ν΄λμ€ μμ±μμ μ²« μ€μμ, super ν€μλλ₯Ό μ΄μ©ν΄ λͺμμ μΌλ‘ λΆλͺ¨ ν΄λμ€μ μμ±μλ₯Ό νΈμΆν΄μ€μΌ λ¨
        // μμ ν΄λμ€μ μμ±μμμλ μ μΌ μ²μ λΆλͺ¨ ν΄λμ€μ κΈ°λ³Έ μμ±μλ₯Ό νΈμΆνλ€.
        super(name, age);	
        this.major = major
    }
    
    public void study() {
        super.eat();	// λΆλͺ¨ ν΄λμ€ νΈμΆ
        System.out.println("κ³΅λΆλ₯Ό νλ€.");
    }
}

Student st = new Student("λͺ°νΈ", 4, "λ―Έμ ");
st.study();
// μλμ κ°μ΄ μΆλ ₯
μμμ λ¨Ήλλ€.
κ³΅λΆλ₯Ό νλ€.
```

<br>

### - μ€λ²λΌμ΄λ©

> μμ²΄λ‘μμ λμμ μμ§λ§, μ»΄νμΌλ¬κ° μ~ λ μ€λ²λΌμ΄λλ μ κ΅¬λ νλ©΄μ λΆλͺ¨ ν΄λμ€κ°μ νλ² νμΈμ ν΄μ£Όκ³  λ?μ΄μμμ€ μ¦, μμ νλ€γγγγ νΉμ λ¬Έμ  μλμ§ μ»΄νμΌλ¬νν μ²΄ν¬νλΌκ³  νλ κ²

```java
public class Person {
    String name;
    
    public void eat() {
        System.out.println("μμμ λ¨Ήλλ€.");
    }
}

public class Student extends Perosn {
    String major;
    
    public Student(String name, int age, String major){
        super(name, age);	
        this.major = major
    }
    
    @Override // μκΈ° μ€λ²λΌμ΄λ!
    public void study() {
        super.eat();
        System.out.println("κ³΅λΆλ₯Ό νλ€.");
    }
}

Student st = new Student("λͺ°νΈ", 4, "λ―Έμ ");
```

- μμ ν΄λμ€μ μ μΈλ λ©μλλ₯Ό μμ ν΄λμ€μμ μ¬μ μ νλ κ².
- λ©μλμ μ΄λ¦, λ°νν, λ§€κ°λ³μ (νμ, κ°μ, μμ)κ° λμΌν΄μΌ νλ€.
- νμ ν΄λμ€μ μ κ·Όμ μ΄μ λ²μκ° μμ ν΄λμ€λ³΄λ€ ν¬κ±°λ κ°μμΌ νλ€.

<br>

## βοΈ Object

> κ°μ₯ μ΅μμ ν΄λμ€λ‘ λͺ¨λ  ν΄λμ€μ μ‘°μ ν΄λμ€



### - toStrig λ©μλ

> κ°μ²΄λ₯Ό λ¬Έμμ΄λ‘ λ³κ²½νλ λ©μλ

```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}


public class Test{
    public int Age;
 
    @Override
    public String toString() {
        return "λμ΄" + Age;
    }
}
```

- objectμ λ©μλμ΄κΈ° λλ¬Έμ μ€λ²λΌμ΄λ©(μ¬μ μ)ν΄μ λ΄κ° μνλ νμμΌλ‘ λ°ν κ°λ₯

<br>



### - equals λ©μλ

> λ κ°μ²΄κ° κ°μμ§λ₯Ό λΉκ΅νλ λ©μλ, μ°Έμ‘° λ³μκ° κ°λ¦¬ν€λ κ°μ λΉκ΅!

```java
Car car01 = new Car();

Car car02 = new Car();

 

System.out.println(car01.equals(car02));

car01 = car02; // λ μ°Έμ‘° λ³μκ° κ°μ μ£Όμλ₯Ό κ°λ¦¬ν΄.

System.out.println(car01.equals(car02));

// μ€νκ²°κ³Ό
false
true
```

<br>



## βοΈ λ€νμ±

> λΆλͺ¨ ν΄λμ€μ μ°Έμ‘°λ³μλ‘ μμ ν΄λμ€μ κ°μ²΄λ₯Ό μ°Έμ‘°ν  μ μλ€. but μ€μ  λΆλͺ¨ μμ­λ§ μ κ·Όμ΄ κ°λ₯νλ€.
>
> => νλμ κ°μ²΄κ° μ¬λ¬κ°μ§ νμμ κ°μ§ μ μλ κ²μ μλ―Έ

```java
class Parent {}
class Child extends Parent {}

//
Parent p1 = new Parent();
Child c1 = new Child();
Parent p2 = new Child(); // λΆλͺ¨ ν΄λμ€μ μ°Έμ‘°λ³μλ‘ μμ ν΄λμ€ κ°μ²΄ μ°Έμ‘°
Child c2 = new Parent(); // μλ¬
```

- μμ ν΄λμ€ νμμ μ°Έμ‘° λ³μλ‘λ λΆλͺ¨ ν΄λμ€ νμμ μΈμ€ν΄μ€λ₯Ό μ°Έμ‘°ν  μ μλλ°, μ°Έμ‘° λ³μκ° μ¬μ©ν  μ μλ λ©€λ²μ κ°μκ° μ€μ  μΈμ€ν΄μ€μ λ©€λ² κ°μλ³΄λ€ λ§κΈ° λλ¬Έμ΄λ€. 
- ν΄λμ€λ μμμ ν΅ν΄ νμ₯λ  μλ μμ΄λ μΆμλ  μλ μμΌλ―λ‘, μμ ν΄λμ€μμ μ¬μ©ν  μ μλ λ©€λ²μ κ°μκ° μΈμ λ λΆλͺ¨ ν΄λμ€μ κ°κ±°λ λ§κ² λλ€. 

<br>

### - μ°Έμ‘° λ³μμ νμ λ³ν

```java
// λ¬Έλ² => (λ³ν ν  νμμ ν΄λμ€ μ΄λ¦) λ³νν  μ°Έμ‘° λ³μ

class Parent {}
class Child extends Parent {}
class Brother extends Parent {}

Parent p1 = null;
Child c = new Child();
Parent p2 = new Parent();
Brother b = null;

p1 = c;	// p = (Parent) cμ κ°κ³ , νμ λ³νμ μλ΅ν  μ μμ
b = (Brother) p2; // νμ λ³νμ μλ΅ν  μ μμ
b = (Brother) c;  // μ§μ μ μΈ μμκ΄κ³κ° μλκΈ° λλ¬Έμ μλ¬
```

1. μλ‘ μμ κ΄κ³μ μλ ν΄λμ€ μ¬μ΄μλ§ νμ λ³νμ ν  μ μμ
2. μμ ν΄λμ€ νμμμ λΆλͺ¨ ν΄λμ€ νμμΌλ‘μ νμ λ³νμ μλ΅ κ°λ₯
3. νμ§λ§, λΆλͺ¨ ν΄λμ€ νμμμ μμ ν΄λμ€ νμμΌλ‘μ νμ λ³νμ λ°λμ λͺμν΄μΌ ν¨

<br>

### - instanceof μ°μ°μ

> μ°Έμ‘°λ³μ `instanceof` ν΄λμ€ μ΄λ¦
>
> μΌμͺ½μ μ λ¬λ μ°Έμ‘° λ³μκ° μ€μ λ‘ μ°Έμ‘°νκ³  μλ μΈμ€ν΄μ€μ νμμ΄ μ€λ₯Έμͺ½μ μ λ¬λ ν΄λμ€ νμμ΄λ©΄ true κ·Έλ μ§ μμΌλ©΄ false λ°ννλ€. μ°Έμ‘° λ³μκ° nullμ κ°λ¦¬ν€κ³  μμΌλ©΄ false λ°ν

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

