# π§ Java Map

> Keyμ valueλ₯Ό νλμ Entryλ‘ λ¬Άμ΄μ λ°μ΄ν°λ₯Ό κ΄λ¦¬, μμλ μμΌλ©° ν€μ λν μ€λ³΅μ μμ!

<br>



## βοΈ HashMap

```java
HashMap<String, Integer> hm = new HashMap<String, Integer>();
```

<br>

## βοΈ TreeMap

```java
TreeMap<Integer, String> tm = new TreeMap<Integer, String>();
```

<br>



### - put(K key, V alue) : λ°μ΄ν° μλ ₯

```java
// put() λ©μλλ₯Ό μ΄μ©ν μμμ μ μ₯

hm.put("μΌμ­", 30);
hm.put("μ­", 10);
hm.put("μ¬μ­", 40);
```

<br>

### - get(Object key) :λ°μ΄ν° μΆμΆ

```java
//get() λ©μλλ₯Ό μ΄μ©ν μμμ μΆλ ₯

System.out.println(hm.get("μΌμ­"));

for (String key : hm.keySet()) {
    System.out.println(String.format("ν€ : %s, κ° : %s", key, hm.get(key)));
}
```

<br>

### - remove(K key) : μμ μ κ±°

```java
hm.remove("μ¬μ­");
```

<br>

### - replace() : μμ μμ 

```java
hm.replace("μ΄μ­", 200);
```

<br>

### - size() :μμμ μ΄ κ°μ

```java
hm.size();
```

<br>

### - containsKey(key) : νΉμ ν key ν¬ν¨ μ¬λΆ

```java
hm.containsKey("μΌμ­")
```

<br>

### - entrySet() : ν€μ κ°μ λ°ν

```java
Set<Entry<String, String>> entrySet = map.entrySet();

for (Entry<String, String> entry : entrySet) {
    System.out.println(String.format("ν€ : %s, κ° : %s", entry.getKey(), entry.getValue()));

}
```







