# ๐ง Java Collection Framework - List

> ์์๊ฐ ์๊ณ  ์ค๋ณต์ ํ์ฉ!

<br>

## โ๏ธ ArrayList

```java
// ArrayList ๊ฐ์ฒด ์์ฑ, ์ ๋๋ฆญ ๋ง์ถฐ์!

ArrayList arrList = new ArrayList();	// ํ์ ๋ฏธ์ค์  ์ Object๋ก ์ ์ธ

ArrayList<Example> arrList = new ArrayList<Example>();	// ํ์ ์ค์ Example ๊ฐ์ฒด๋ง ์ฌ์ฉ ๊ฐ๋ฅ

ArrayList<Integer> arrList = new ArrayList<Integer>();	// ํ์ ์ค์  int ํ์๋ง ์ฌ์ฉ ๊ฐ๋ฅ

// ์ข๋ณ์๋ง ํ์ ๋ฃ์ด๋ ๊ฐ ์ธ!
ArrayList<Integer> arrList = new ArrayList();	// new์์ ํ์ ํ๋ผ๋ฏธํฐ ์๋ต ๊ฐ๋ฅ

ArrayList<Integer> arrList = new ArrayList<Integer>(10);	// ์ด๊ธฐ ์ฉ๋(capacity) ์ง์ 	

ArrayList<Integer> arrList = new ArrayList<Integer>(Arrays.asList(1,2,3)); // ์์ฑ ์ ๊ฐ ์ถ๊ฐ!
```

- ํ์์ ์ ๋ฆฌํ์ง๋ง, ๊ฐ ์ถ๊ฐ๋ ์ญ์ ๊ฐ ๋ถ๋ฆฌ(๋๋ฆผ)

<br>

## โ๏ธ LinkedList

```java
// ArrayList๋ ๊ฐ์ ใใ

LinkedList<String> lnkList = new LinkedList<String>();
```

- ํ์์ ๋ถ๋ฆฌํ์ง๋ง, ๊ฐ ์ถ๊ฐ๋ ์ญ์ ๊ฐ ์ ๋ฆฌ



<br>

### - add(E e) ๋ฐ์ดํฐ ์๋ ฅ

```java
arrList.add(10);
arrList.add(20);
```

<br>

### - get(int index) : ๋ฐ์ดํฐ ์ถ์ถ

```java
// 2๋ฒ ์ธ๋ฑ์ค์ ๊ฐ ์ถ์ถ
arrList.get(2);

// for๋ฌธ์ผ๋ก ์ถ๋ ฅ!
for (int i = 0; i < arrList.size(); i++){
    System.out.print(arrList.get(i) + " ");
}
```

<br>

### - size() : ์์์ ์ด ๊ฐ์

```java
System.out.print("๋ฆฌ์คํธ์ ํฌ๊ธฐ: " + arrList.sieze());
```

<br>

### - remove(int i) : ํน์  ๋ฐ์ดํฐ ์ญ์ 

```java
// 1๋ฒ์งธ ๋ฐ์ดํฐ ์ญ์ 
arrList.remove(1);
```

<br>

### - remove(Object o)

```java
// "์์นจ" ๋ฐ์ดํฐ ์ญ์ , arrList = ["์์นจ", "์ ๋"]
arrList.remove("์์นจ");
```

<br>

### - clear() : ๋ชจ๋  ๋ฐ์ดํฐ ์ญ์ 

```java
// arrList ๋ด ๋ชจ๋  ์์ ์ญ์ 
arrList.clear();
```

<br>

### - contains(Object o) : ํน์  ๊ฐ์ฒด๊ฐ ํฌํจ๋์ด ์๋์ง ์ฒดํฌ

```java
// arrList์์ "์ ๋"์ด ์๋์ง ์ฒดํฌ, true/false๋ฅผ ๋ฐํ
arrList.contains("์ ๋");
```

<br>

### - isEmpty() : ๋น์ด์๋์ง ์ฒดํฌ(true, false)

```java
// ๋ฆฌ์คํธ ๋น์ด ์๋์ง ์ฒดํฌ
arrList.isEmpty()
```

<br>

### - addAll(Collection c) :๊ธฐ์กด ๋ฑ๋ก๋ ์ปฌ๋ ์ ๋ฐ์ดํฐ ์๋ ฅ

```java
ArrayList<String> tmpList = new ArrayList<>();
tmpList.add("์์นจ");
tmpList.add("์ ๋");

arrList.addAll(tmpList);
```

- ์ธ์๋ก ์ ๋ฌ๋๋ Collection ๊ฐ์ฒด์ ๋ชจ๋  ์์ดํ์ ๋ฆฌ์คํธ์ ์ถ๊ฐ

<br>

### - iterator() : iterator ์ธํฐํ์ด์ค ๊ฐ์ฒด ๋ฐํ

```java
Iterator<Integer> iter = arrList.iterator();
while (iter.hasNext()) {
    System.out.print(iter.next() + " ");
}
```

<br>

### - set() : ์์ ๋ณ๊ฒฝ

```java
// 1๋ฒ์งธ ์์๋ฅผ 20์ผ๋ก ๋ณ๊ฒฝ
arrList.set(1, 20)
```

<br>

### - sort() : ์์์ ์ ๋ ฌ

```java
Collections.sort(arrList)
```





