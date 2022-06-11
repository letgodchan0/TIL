# 🧋 Java Set

> 순서가 없고 중복을 허용하지 않음! 빠른 속도 효율적인 중복 데이터 제거

<br>



## ✔️ HashSet

```java
HashSet<String> hs01 = new HashSet<String>();
HashSet<Integer> hs02 = new HashSet<Integer>();
```

- HashSet에 이미 존재하는 요소인지 파악하기 위해 내부적으로 다음과 같은 과정을 거친다
  1. 해당 요소에서 hashCode() 메소드를 호출하여 반환된 해시값으로 검색할 범위를 지정
  2. 해당 범위 내 요소들을 equals() 메소드로 비교

<br>

## ✔️ TreeSet

```java
TreeSet<Integer> ts = new TreeSet<Integer>();
```

- TreeSet 클래스는 데이터가 정렬된 상태로 저장되는 이진 검색 트리의 형태로 요소를 저장하기 때문에 데이터를 추가하거나 제거하는 등의 기본 동작 시간이 매우 빠름
- 아래 메소드 외에도 `subset` 메소드 존재

```java
// 10
System.out.println(ts.subSet(10, 20));
```



<br>

### - add

```java
hs01.add("홍길동");
hs01.add("이순신");

hs02.add(3);
hs02.add(4);
```

<br>

### - remove

```java
// Integer 타입, 1번 값 삭제
hs02.remove(1);

// String 타입
hs01.remove("홍길동");
```

<br>

### - clear

```java
// hashset 내 요소 모두 지우기
hs01.clear()
```

<br>

### - size

```java
// hashset 크기 구하기
hs01.size()
```

<br>

### - contains

```java
// hashset 내 값이 존재하면 true 없으면 false
hs01.contains("홍길동");
```

<br>

### - toArray()

```java
Set<Integer> set = Sets.newHashSet(1, 2, 3);
Integer[] array = new Integer[set.size()];

set.toArray(array);
```

