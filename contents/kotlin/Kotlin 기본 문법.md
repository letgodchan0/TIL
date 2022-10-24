# Kotlin 기본 문법

[안드로이드 스튜디오 설치](https://developer.android.com/studio)

<br>

### 1. 함수

```kotlin
fun helloWorld() : Unit{
    println("Hello World!")
}
```

- 무엇을 리턴하던지 앞에 `fun`을 써줘야 함

- 아무것도 리턴 형식이 없을 때 `Unit`를 써줘야 함. 자바에서 `void`
  - 리턴 형식이 없으면 `Unit` 생략 가능!!

```kotlin
fun add(a : Int, b : Int) : Int {
    return a + b
}
```

- 파라미터에는 변수가 먼저 오고, 뒤에 타입이온다!
- 자바(int)에서 Int로!

- 어떤 값을 리턴할 때는 꼭 리턴 타입을 적어줘야함!

<br>

### 2. val 과 var

```kotlin
fun hi(){
    val a : Int = 10
    var b : Int = 9
    
    a = 100 // 에러
    b = 100 // 가능
    
    val c = 100
    var name = 'letgodchan0'
    
    var e : String
}
```

- `val`은 변할 수 없는 값!
- `var`는 변할 수 있는 값!
- kotlin은 그냥 타입 알고 있어서 변수 설정할 때 타입 안써줘도 된다. but `val`과 `var`은 써줘야 함!

- 초기값 설정 없이 변수 선언만 하고 싶으면 뒤에 타입을 꼭 써줘야함!

<br>

### 3. String 템플릿

```kotlin
fun main(){
    // 3. String Template

    val name = "letgodchan0"
    val lastName = "kim"
    println("my name is $name")
    println("my name is ${name + lasName} I`m 25")
    
    println("this is 2\$a")
}
```

- `$`로 변수를 넣을 수 있다!

- `$`를 쓰고 싶으면 `\`를 사용하자!

<br>

### 4. 조건식

```kotlin
fun maxBy(a: Int, b :Int){
    if(a > b){
        return a
    }else{
        return b
    }
}
// 3항 연산자 느낌, 자동 타입 추론!
fun maxBy2(a : Int, b : Int) = if (a>b) a else b
```

```kotlin
fun checkNum(score : Int){
    // 그냥 when에서는 else 생략 가능!
    when(score) {
        0 -> println("this is 0")
        1 -> println("this is 1")
        2,3 -> println("this is 2 or 3")
        else -> println("I don`t know")
    }
    
    // score가 1이면 1 리턴, 2이면 2리턴 그렇지 않으면 3, 여기서는 else를 꼭 써줘야 함!!
    var b = when(score){
        1-> 1
        2-> 2
        else-> 3
    }
    
    // else 생략 가능, 90에서 100 사이, 10에서 80 사이
    when(score){
        in 90..100 -> println("You are genius")
        in 10..80 -> println("not bad")
        else -> println("okay")
    }
}
```

- 자바의 switch가 코틀린에서 when!!

<br>

#### Expression vs Statement

```kotlin
fun maxBy2(a : Int, b : Int) = if (a>b) a else b

var b = when(score){
        1-> 1
        2-> 2
        else-> 3
    }
```

- 위와 같이 a 혹은 b라는 값을 만들기 때문에 `Expression`
- 또한 b가 1, 2, 3 중 하나의 값을 가지기 때문에 `Expression`
- kotlin의 모든 함수는 `Expression`
  - `Unit` 타입의 함수도 사실은 `Unit`을 리턴함

<br>

```kotlin
when(score) {
        0 -> println("this is 0")
        1 -> println("this is 1")
        2,3 -> println("this is 2 or 3")
        else -> println("I don`t know")
    }
```

- 값을 만드는 것이 아니고, 실행하는 것이기 때문에 Statement

<br>

### 5. Array와 List

```kotlin
fun array(){
    val array = arrayOf(1, 2, 3)
    val list = listOf(1, 2, 3)
    
    val array2 = arrayOf(1, "d", 3, 3.4f)
    val list2 = listOf(1, "d", 11L)
    
    // 가능
    array[0] = 3
    
    // 값을 변경 못하는 List, 불가능
    list[0] = 2
    // 이렇게 가져올 수만 잇음!!
    list.get(0)
}
```

- Array는 크기가 정해져 있기 때문에 처음에 선언할 때 크기를 할당해주어야 한다.
- List는 수정이 가능한 리스트와 수정이 불가능한 리스트 2개로 구분된다.
- 자바가 배열에 같은 타입만 담을 수 있었다면 코틀린은 아님.. 웰컴 파이썬

<br>

```kotlin
// 값을 변경할 수 있는 리스트 초기화
var arrayList = arrayListOf<Int>()
arrayList.add(10)
arrayList.add(20)

// 값 변경
arrayList.set(0, 15)
```

- 값을 변경할 수 있는 리스트 : arrayListOf
- `set`을 통해 0번째 인덱스의 값을 15로 바꾸겠다는 뜻!

<br>

### 6. 반복문

#### - for문

```kotlin
fun forAndWhile(){
    val students = arrayListOf("joyce", "james", "jenny", "jennifer")
    
    for (name in students) {
        println("${name}") // 그냥 name도 가능!
    }
    for ((index, name) in students.withIndex()){
        println("${index+1}번째 학생 : ${name}")
    }
}
```

- 노멀한 for 문!
- `withIndex()` : 파이썬 enumerate 같은거!

<br>

```kotlin
var sum : Int = 0
// for (i in 1 until 10) 
for (i in 1..10 step 2){
    sum += i
}
```

- 1부터 10까지 2씩 증가하는 for 문
- `until`은 10을 포함하지 않고, 1부터 9까지만!

<br>

```kotlin
var sum : Int = 0
for (i in 10 downTo 1){
    sum += i
}
```

- 10부터 1까지 감소하는 for 문

<br>

#### - while문

```kotlin
var index = 0
while (index < 10){
    println("current index : ${index}")
    index += 1
}
```

<br>

### 7. NonNull과 Nullable

```kotlin
fun nullCheck(){
    var name = "letgodchan0"
    var nameInUpperCase = name.toUpperCase() // 대문자로 바꿔주는 메서드 같은거!
    
    var nullName : String? = null  // ?를 같이 넣어줘야 nullable 타입이된다!!
    var nullNameInUpperCase = nullName?.toUpperCase()
}
```

- NPE : NULL pointer Exception - 자바에서는 컴파일 시점에서는 잡을 수 없고, 런타임에서만 잡을 수 있음, 즉 run을 해야만 알 수 있음!
  - kotlin에서는 컴파일 시점에서 잡아줌!! 그게 바로 `?`
  - `?` 앞에 타입을 꼭 넣어주긴 해야함!
- `var nullNameInUpperCase = nullName?.toUpperCase()` : 만약 nullName이 null 이 아니면, toUpperCase를 하고 null이라면, 바로 null을 반환함, 타입도 null로!!

<br>

#### - 엘비스 연산자 (?:)

```kotlin
fun nullCheck(){
    var name = "letgodchan0"
    val lastName : String? = null
    
    val fullName = name + " " + (lastName?: "No lastName")
}
```

- `?:` lastName이 있으면 lastName을 출력하고, null이라면 뒤에를 출력해라! 즉 디폴트 값을 준거임

<br>

#### - null 아니야 (!!)

```kotlin
fun ignoreNulls(str : String?){
    var mNotNull : String = str!!
}
```

- 인자로 어떤 값을 받을 때, null을 받을 수도 있잖아!!
- 근데 만약에, 인자로 들어오는 값이 절대로 null 아니라고 말해주고 싶을 때 `!!`을 사용!!
- 컴파일한테 이거 null 아니라고 말해주는 것!!

<br>

#### - let

```kotlin
val email : String? = "abcdefgXXXX@naver.com"
email?.let{
    println("my email is ${email}")
}
```

- `email?.let` : email이 null 아니면, let 안에 있는 걸 해라!!
- `let` : 자신의 리시버 객체를 람다식 안으로 옮겨서 실행하는 구문! 즉, email이 null 아니라면 let 내부에서 email이 사용 가능

<br>

