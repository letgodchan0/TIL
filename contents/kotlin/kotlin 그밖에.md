# kotlin 그밖에

> 형 변환, 

<br>



### 1. 형 변환

```kotlin
fun main(){
    var number: Int = 10
    var word: String = number.toString()
    print(word)
}
```

- `to변수()`를 통해 형 변환이 가능하다.
- 코틀린은 암시적 형변환을 지원하지 않는다.

<br>

### 2. Map 

```kotlin
fun main() {
    var dict: MutableMap<Int, String> = mutableMapOf()
    dict[1] = "kim"
    dict[2] = "lee"

    for(i in dict){
        println("${i.key} ${i.value}")
    }
    dict.remove(1)

    println(dict)
    println(dict[2])
}

출력
1 a
2 b
{2=b}
b
```

<br>

### 3. Mutable 리스트 정렬

```kotlin
fun main(args: Array<String>){

    val list = mutableListOf(1, 11, 3, 9, 5, 20, 7)
    
    list.sort()
    println("sorted: $list")
    
    list.reverse()
    println("reverse sorted: $list")
}
```

- `sort()`는 데이터 변경이 가능한 리스트의 요소들을 정렬한다. 리스트 자신이 갖고 있는 요소의 순서를 변경한다.

- `reverse()`는 리스트 자신의 요소 순서를 반대로 변경

<br>

### 4. Immutable 리스트 정렬

```kotlin
fun main(args: Array<String>){

    val list = listOf(1, 11, 3, 9, 5, 20, )

    val sorted = list.sorted()
    println("sorted: $sorted")
    
    val sored2 = list.sored().reversed()
}
```

- `sorted()`는 데이터 변경이 안되는 리스트를 정렬할 때 사용한다. 원본을 변경하지 않고, 정렬된 리스트를 생성하여 리턴한다. 

- `reversed()`는 Immutable 리스트에서 사용하는데, 역순으로 변경된 리스트를 생성하고 리턴한다.