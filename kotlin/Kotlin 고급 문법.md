# Kotlin 고급 문법

> 람다, 확장함수

<br>

### 1. 람다

```kotlin
val square : (Int) -> (Int) = {number -> number*number}
val square = {number : Int -> number*number} // 위랑 같음!
// 코틀린이 기본적으로 타입 추론하지만, 익명함수는 어디에는 꼭 타입을 넣어주긴 해야함

fun main(){
    println(square(12))
}
```

- 람다식은 우리가 마치 `value` 처럼 다룰 수 있는 익명함수
  - 메소드의 파라미터로 넘겨줄 수 있다.
  - return 값으로 사용할 수 있다.
- 람다 기본 정의
  - `val lamdaName : Type = {argumentList -> codeBody}`
  - 선언, 람다 이름 : 타입 = 여기 들어오는 인자 리스트를 -> codeBody대로 적용!
  - `val square : (Int) -> (Int) = {number -> number*number}` : 제곱 해주는 익명 함수

<br>

```kotlin
val nameAge = {name : String, age : Int ->
              "my name is ${name} I'm ${age}"
              }

val nameAge = {name : String, age : Int -> }
```

- 나이랑 이름을 인자로 받으면 출력해주는 익명함수
- 2번째 익명함수는 반환하는 것이 없기 때문에 unit을 반환한다! (신기)

<br>

#### - 람다와 확장함수

```kotlin
// 코틀린의 String 클래스에 기능 추가하기

val pizzaIsGreat : String.() -> String = {
    this + " Pizza is the best!"
}

val a = "he said"
println(a.pizzaIsGreat) 	// he said Pizza is the best! 출력
```

- 있는 클래스에서 기능 몇개를 추가하고 싶을 때 확장함수를 사용한다!

- `val pizzaIsGreat : String.() -> String` : 기존의 스트링 클래스를 스트링 타입으로 반환할 껀데, {}안의 코드 블럭을 적용해서 반환해!

<br>

```kotlin
// this와 it 사용

fun extendString(name : String, age : Int) : String{
    val introduceMyself : String.(Int) -> String = {"I am ${this} and ${it} years old"}
    return name.introduceMyself(age)
}

println(extendString("caleb", 28)) // I am caleb and 28 years old 출력!
```

- 위와 같이 파라미터가 `Int` 하나면 `it`을 사용할 수 있다

- `this`가 가리키는 것은 확장함수를 콜 하는 오브젝트 `String`
-  `it`이 가리키는 것은 하나 들어가는 파라미터 `Int` 값

<br>

#### - 람다의 리턴

```kotlin
// 성적을 산출해 주는 람다식

val calculateGrade : (Int) -> String = {
    when (it) {
        in 0..40 -> "fail"
        in 41..70 -> "pass"
        in 71..100 -> "perfect"
        else -> "Error"
    }
}

println(calculateGrade(90))	// perfect 출력
```

- Int 타입을 받고 String을 반환하는 람다식
- 여기서 `else` 부분이 없고 인자로 1,000이 들어오면 어디에도 속하지 않기 때문에 에러가 난다. 따라서 꼭 else를 넣어야 한다. 

- 람다는 리턴을 할 때 마지막 표현식이 리턴의 value 타입을 지정한다!

<br>

#### - 람다를 표현하는 2가지 방법

```kotlin
// 파라미터로 람다 받기

fun invokeLamda(lamda : (Double) -> Boolean) : Boolean {
    return lamda(5.2343)
}

// Double 타입을 인자로 받고 4.3213과 같으면 true 다르면 false를 반환
val lamda = {number : Double -> 
	number == 4.3213
}

println(invokeLamda(lamda)) // false를 반환
println(invokeLamda(true)) // 얘도 가능
println(invokeLamda({it > 3.22}))  // 파라미터가 1개일 때 it이 그것을 가리킴! 
print(invokeLamda { it > 3.22 }) // 마지막 파라미터가 람다식 일 때 이런식으로도 작성 가능
```

- double을 받고 boolean을 리턴하는 람다를 파라미터로 받음
- `invokeLamda`는 boolean을 리턴하고, 인자로 boolean을 리턴하는 람다를 받는다.

<br>

### 2. Data class

```kotlin
data class Ticket(val companyName : String, val name : String, var date : String, var seatNumber : Int)
class TicketNormal(val companyName : String, val name : String, var date : String, var seatNumber : Int)

fun main(){
    val ticketA = Ticket("google", "kim", "2022-10-18", 5)
    val ticketB = TicketNormal("google", "kim", "2022-10-18", 5)

    println(ticketA)
    println(ticketB)
}
```

- data class를 만들어 주면, `toString(), hashCode(), equals(), copy()` 등을 자동으로 만들어 준다.
- 그냥 클래스와 차이점을 보면 출력했을 때 각각 다음과 같다.
  - data class : `Ticket(companyName=google, name=kim, date=2022-10-18, seatNumber=5)`
  - class : `com.example.kotlinpractice.TicketNormal@2812cbfa`

<br>

### 3. Companion object

```kotlin
class Book private constructor(val id : Int, val name : String){
    companion object {
        
        val myBook = "new book"
        
        fun create() = Book(0, myBook)
    }
}
fun main(){
    val book = Book.Companion.create() // 여기서 Companion 생략가능!, Book.create()로!
}
```

- private으로 클래스를 생성할 때 `companion object`를 생성해주어야한다. 그렇지 않으면 객체 생성 못함! 객체 생성 방식도 위와 같다.

- `companion object`는 자바의 static 대신 사용되는 것으로, 정적 메소드나 정적 변수를 선언할 때 사용하는 것, 프라이빗 프로퍼티나 메소드를 읽어올 수 있게끔 하는 것!

<br>

```kotlin
// companion object의 인터페이스 상속

interface IdProvider {
    fun getId() : Int
}

class Book private constructor(val id : Int, val name : String){

    // 상속 부분
    companion object BookFactory : IdProvider{
        // 인터페이스니까 오버라이딩
        override fun getId(): Int {
            return 123
        }
        fun create() = Book(getId(), "animal farm")
    }
}

val bookId = Book.getId()
val bookId = Book.BookFactory.getId()
```

<br>

### 4. Object 클래스

```kotlin
//Singleton Pattern
object CarFactory {
    val cars = mutableListOf<Car>()
    fun makeCar(horsePower : Int) : Car {
        val car = Car(horsePower)
        cars.add(car)
        return car
    }
}

data class Car(val horsePower : Int)

fun main() {
    val car = CarFactory.makeCar(10)
    val car2 = CarFactory.makeCar(200)

    println(car)
    println(car2)
    println(CarFactory.cars.size.toString())
}
```

- object가 다른 클래스와 다른 점 : CarFactory 객체는 모든 앱을 실행할 때 딱 한번 만들어지는 싱글턴 패턴을 적용한 것임
- 실행할 때 딱 한번만 객체를 생성하고 다시는 객체를 생성하지 않기 때문에 불필요하게 메모리가 사용되는 것을 방지