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
```

- double을 받고 boolean을 리턴하는 람다를 파라미터로 받음
- `invokeLamda`는 boolean을 리턴하고, 인자로 boolean을 리턴하는 람다를 받는다.