# π§ Java Set

> μμκ° μκ³  μ€λ³΅μ νμ©νμ§ μμ! λΉ λ₯Έ μλ ν¨μ¨μ μΈ μ€λ³΅ λ°μ΄ν° μ κ±°

<br>



## βοΈ HashSet

```java
HashSet<String> hs01 = new HashSet<String>();
HashSet<Integer> hs02 = new HashSet<Integer>();
```

- HashSetμ μ΄λ―Έ μ‘΄μ¬νλ μμμΈμ§ νμνκΈ° μν΄ λ΄λΆμ μΌλ‘ λ€μκ³Ό κ°μ κ³Όμ μ κ±°μΉλ€
  1. ν΄λΉ μμμμ hashCode() λ©μλλ₯Ό νΈμΆνμ¬ λ°νλ ν΄μκ°μΌλ‘ κ²μν  λ²μλ₯Ό μ§μ 
  2. ν΄λΉ λ²μ λ΄ μμλ€μ equals() λ©μλλ‘ λΉκ΅

<br>

## βοΈ TreeSet

```java
TreeSet<Integer> ts = new TreeSet<Integer>();
```

- TreeSet ν΄λμ€λ λ°μ΄ν°κ° μ λ ¬λ μνλ‘ μ μ₯λλ μ΄μ§ κ²μ νΈλ¦¬μ ννλ‘ μμλ₯Ό μ μ₯νκΈ° λλ¬Έμ λ°μ΄ν°λ₯Ό μΆκ°νκ±°λ μ κ±°νλ λ±μ κΈ°λ³Έ λμ μκ°μ΄ λ§€μ° λΉ λ¦
- μλ λ©μλ μΈμλ `subset` λ©μλ μ‘΄μ¬

```java
// 10
System.out.println(ts.subSet(10, 20));
```



<br>

### - add

```java
hs01.add("νκΈΈλ");
hs01.add("μ΄μμ ");

hs02.add(3);
hs02.add(4);
```

<br>

### - remove

```java
// Integer νμ, 1λ² κ° μ­μ 
hs02.remove(1);

// String νμ
hs01.remove("νκΈΈλ");
```

<br>

### - clear

```java
// hashset λ΄ μμ λͺ¨λ μ§μ°κΈ°
hs01.clear()
```

<br>

### - size

```java
// hashset ν¬κΈ° κ΅¬νκΈ°
hs01.size()
```

<br>

### - contains

```java
// hashset λ΄ κ°μ΄ μ‘΄μ¬νλ©΄ true μμΌλ©΄ false
hs01.contains("νκΈΈλ");
```

<br>

### - toArray()

```java
Set<Integer> set = Sets.newHashSet(1, 2, 3);
Integer[] array = new Integer[set.size()];

set.toArray(array);
```

