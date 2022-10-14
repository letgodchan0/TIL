# Kotlin 클래스

> 파일 이름이랑 클래스 이름이랑 달라도 되고, 여러 클래스도 한 파일에 넣을 수 있다!

<br>

### 1. 클래스 생성

```kotlin
class Human {
    fun eatingCake(){
        println("This is so YUMMY~~")	// 메소드
    }
}

fun main(){
    // 클래스 생성
    val human = Human()
    human.eatingCake()
}
```

- 자바랑 달리 `new`를 안써줌!

<br>

### 2. 프로퍼티

```kotlin
class Human {
    val name = "letgodchan0"	// 프로퍼티
}

fun main(){
    println("this human`s name is ${human.name}")
}
```

<br>

### 3. 생성자

```kotlin
class Human constructor(name : String) {
    val name = name	// 프로퍼티
}

fun main(){
    val human = Human("minsu")
    println("this human`s name is ${human.name}")
}
```

- 생성자를 만들려면 클래스를 생성할 때 파라미터를 넣어주어야 한다!

<br>

```kotlin
class Human (val name : String) {
}

fun main(){
    val human = Human("minsu")
    println("this human`s name is ${human.name}")
}
```

- 근데 위와 같이 `constructor`을 생략하고 파라미터 안에 선언하면서 생성할 수 있다!!

<br>

```kotlin
class Human (val name : String = "익명") {
}

fun main(){
    val stranger = Human()	// Human의 인자로 아무것도 안 넣어도 된다!
    println("this human`s name is ${stranger.name}")
}
```

- `class Human (val name : String = "익명")` : 이렇게 생성자의 디폴트 값을 선언할 수 있음

<br>

#### - 주 생성자

```kotlin
class Human (val name : String = "익명") {
    init {
        println("New human has been born!!")
    }
}

fun main(){
    val stranger = Human()	// Human의 인자로 아무것도 안 넣어도 된다!
    println("this human`s name is ${stranger.name}")
}
```

- `init` 은 주 생성자의 일부라서, 클래스가 생성됨가 동시에 `init`의 코드 블럭도 실행이 된다.

<br>

#### - 부 생성자

```kotlin
class Human (val name : String = "익명") {
    
    constructor(name : String, age : Int) : this(name){
        println("my name is ${name}, ${age}years old")
    }
    
    init {
        println("New human has been born!!")
    }
}

fun main(){
    val mom = Human("Kuri", 52)
}
```

- `constructor(name : String, age : Int) : this(name)` : 부 생성자를 만들 때는 항상 주 생성자의 위임을 받아야한다. this를 통해서 name을 받아야 한다. 
- main에서 클래스를 생성할 때 `init` 부분이 먼저 나오고 그 다음에 부 생성자인 `constructor` 부분이 나온다!

<br>

### 4. 클래스 상속

```kotlin
open class Human (val name : String = "익명"){
    init {
        println("화이팅")
    }

    open fun singASong(){
        println("lalala")
    }
}

// 이게 상속하는 법!
class Korean : Human(){
    // 이게 상속하는 법!
    override fun singASong(){
        println("라라라")
        super.singASong() // 부모 클래스의 메소드 사용!
        println("${name}")
    }
}

fun main(){
    val korean = Korean()
    korean.singASong()
}
```

- 코틀린의 클래스는 기본적으로 `final` 클래스이기 때문에, 아무리 같은 파일에 있더라도 접근을 못한다. 따라서 상속 받는 대상 클래스 앞에 `open`을 넣어줘야 한다.
- 메소드를 오버라이딩 할 때도 마찬가지로 `final`이기 때문에 상속 받는 대상의 메소드 앞에 `open`을 추가해야 한다.

- `Korean`에서 부모 클래스의 `name`을 사용할 수 있는 이유는, Human을 상속할 때, Human의 Constructor에서 namd을 받아오는데, 디폴트 값을 정해주었기 때문에, 생성자의 파라미터가 없는 것까지 만들어 놨기 때문!