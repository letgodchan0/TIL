# 🧋 Java Map

> Key와 value를 하나의 Entry로 묶어서 데이터를 관리, 순서는 없으며 키에 대한 중복은 없음!

<br>



## ✔️ HashMap

```java
HashMap<String, Integer> hm = new HashMap<String, Integer>();
```

<br>

## ✔️ TreeMap

```java
TreeMap<Integer, String> tm = new TreeMap<Integer, String>();
```

<br>



### - put(K key, V alue) : 데이터 입력

```java
// put() 메소드를 이용한 요소의 저장

hm.put("삼십", 30);
hm.put("십", 10);
hm.put("사십", 40);
```

<br>

### - get(Object key) :데이터 추출

```java
//get() 메소드를 이용한 요소의 출력

System.out.println(hm.get("삼십"));

for (String key : hm.keySet()) {
    System.out.println(String.format("키 : %s, 값 : %s", key, hm.get(key)));
}
```

<br>

### - remove(K key) : 요소 제거

```java
hm.remove("사십");
```

<br>

### - replace() : 요소 수정

```java
hm.replace("이십", 200);
```

<br>

### - size() :요소의 총 개수

```java
hm.size();
```

<br>

### - containsKey(key) : 특정한 key 포함 여부

```java
hm.containsKey("삼십")
```

<br>

### - entrySet() : 키와 값을 반환

```java
Set<Entry<String, String>> entrySet = map.entrySet();

for (Entry<String, String> entry : entrySet) {
    System.out.println(String.format("키 : %s, 값 : %s", entry.getKey(), entry.getValue()));

}
```







