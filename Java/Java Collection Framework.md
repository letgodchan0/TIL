# 🧋 Java Collection Framework

> 데이터를 저장하는 자료 구조와 데이터를 처리하는 알고리즘을 구조화하여 클래스를 구현해 놓은 것!

[참고](http://www.tcpschool.com/java/java_collectionFramework_concept)

<br>

## ✔️ Collection Interface

| interface | 특징                                                         |
| --------- | ------------------------------------------------------------ |
| List      | 순서가 있는 데이터의 집합, 순서가 있으니까 데이터의 중복을 허락<br> `ArrayList`, `LinkedList` |
| Set       | 순서를 유지하지 않는 데이터의 집합, 순서가 없어서 같은 데이터를 구별할 수 없음 -> 중복x<br > `HashSet`, `TreeSet` |
| Map       | key와 value의 쌍으로 데이터를 관리하는 집합, 순서는 없고 key의 중복 불가, value는 중복 가능 <br> `HashMap`, `TreeMap` |

<br>

## ✔️ Collection 주요 메소드

| 메소드                       | 설명                                                |
| ---------------------------- | --------------------------------------------------- |
| `boolean add(E e)`           | 해당 컬렉션에 전달된 요소를 추가                    |
| `void clear ()`              | 해당 컬렉션의 모든 요소 제거                        |
| `boolean contains(Object o)` | 해당 컬렉션이 전달된 객체를 포함하는지 확인         |
| `boolean equals(Object o)`   | 해당 컬렉션과 전달된 객체가 같은지 확인             |
| `boolean isEmpty()`          | 해당 컬렉션이 비어있는지 확인                       |
| `iterator<E> iterator()`     | 해당 컬렉션의 반복자(iterator)를 반환               |
| `boolean remove(Object o)`   | 해당 컬렉션에서 전달된 객체를 제거                  |
| `int size()`                 | 해당 컬렉션의 요소의 총 개수를 반환                 |
| `Object[] toArray()`         | 해당 컬렉션의 모든 요소를 Object 타입의 배열로 반환 |





