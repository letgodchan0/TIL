# 🧋 Java Collection Framework - List

> 순서가 있고 중복을 허용!

<br>

## ✔️ ArrayList

```java
// ArrayList 객체 생성, 제너릭 맞춰서!

ArrayList arrList = new ArrayList();	// 타입 미설정 시 Object로 선언

ArrayList<Example> arrList = new ArrayList<Example>();	// 타입 설정Example 객체만 사용 가능

ArrayList<Integer> arrList = new ArrayList<Integer>();	// 타입 설정 int 타입만 사용 가능

// 좌변에만 타입 넣어도 갠츈!
ArrayList<Integer> arrList = new ArrayList();	// new에서 타입 파라미터 생략 가능

ArrayList<Integer> arrList = new ArrayList<Integer>(10);	// 초기 용량(capacity) 지정	

ArrayList<Integer> arrList = new ArrayList<Integer>(Arrays.asList(1,2,3)); // 생성 시 값 추가!
```

- 탐색에 유리하지만, 값 추가나 삭제가 불리(느림)

<br>

## ✔️ LinkedList

```java
// ArrayList랑 같음 ㅎㅎ

LinkedList<String> lnkList = new LinkedList<String>();
```

- 탐색에 불리하지만, 값 추가나 삭제가 유리



<br>

#### - add(E e) 데이터 입력

```java
arrList.add(10);
arrList.add(20);
```

<br>

#### - get(int index) : 데이터 추출

```java
// 2번 인덱스의 값 추출
arrList.get(2);

// for문으로 출력!
for (int i = 0; i < arrList.size(); i++){
    System.out.print(arrList.get(i) + " ");
}
```

<br>

#### - size() : 요소의 총 개수

```java
System.out.print("리스트의 크기: " + arrList.sieze());
```

<br>

#### - remove(int i) : 특정 데이터 삭제

```java
// 1번째 데이터 삭제
arrList.remove(1);
```

<br>

#### - remove(Object o)

```java
// "아침" 데이터 삭제, arrList = ["아침", "저녁"]
arrList.remove("아침");
```

<br>



#### - clear() : 모든 데이터 삭제

```java
// arrList 내 모든 요소 삭제
arrList.clear();
```

<br>

#### - contains(Object o) : 특정 객체가 포함되어 있는지 체크

```java
// arrList안에 "저녁"이 있는지 체크, true/false를 반환
arrList.contains("저녁");
```

<br>

#### - isEmpty() : 비어있는지 체크(true, false)

```java
// 리스트 비어 있는지 체크
arrList.isEmpty()
```

<br>

#### - addAll(Collection c) :기존 등록된 컬렉션 데이터 입력

```java
ArrayList<String> tmpList = new ArrayList<>();
tmpList.add("아침");
tmpList.add("저녁");

arrList.addAll(tmpList);
```

- 인자로 전달되는 Collection 객체의 모든 아이템을 리스트에 추가

<br>

#### - iterator() : iterator 인터페이스 객체 반환

```java
Iterator<Integer> iter = arrList.iterator();
while (iter.hasNext()) {
    System.out.print(iter.next() + " ");
}
```

<br>

#### - set() : 요소 변경

```java
// 1번째 요소를 20으로 변경
arrList.set(1, 20)
```

<br>

#### - sort() : 요소의 정렬

```java
Collections.sort(arrList)
```





