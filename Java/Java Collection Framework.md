# π§ Java Collection Framework

> λ°μ΄ν°λ₯Ό μ μ₯νλ μλ£ κ΅¬μ‘°μ λ°μ΄ν°λ₯Ό μ²λ¦¬νλ μκ³ λ¦¬μ¦μ κ΅¬μ‘°ννμ¬ ν΄λμ€λ₯Ό κ΅¬νν΄ λμ κ²!

[μ°Έκ³ ](http://www.tcpschool.com/java/java_collectionFramework_concept)

<br>

## βοΈ Collection Interface

| interface | νΉμ§                                                         |
| --------- | ------------------------------------------------------------ |
| List      | μμκ° μλ λ°μ΄ν°μ μ§ν©, μμκ° μμΌλκΉ λ°μ΄ν°μ μ€λ³΅μ νλ½<br> `ArrayList`, `LinkedList` |
| Set       | μμλ₯Ό μ μ§νμ§ μλ λ°μ΄ν°μ μ§ν©, μμκ° μμ΄μ κ°μ λ°μ΄ν°λ₯Ό κ΅¬λ³ν  μ μμ -> μ€λ³΅x<br > `HashSet`, `TreeSet` |
| Map       | keyμ valueμ μμΌλ‘ λ°μ΄ν°λ₯Ό κ΄λ¦¬νλ μ§ν©, μμλ μκ³  keyμ μ€λ³΅ λΆκ°, valueλ μ€λ³΅ κ°λ₯ <br> `HashMap`, `TreeMap` |

<br>

## βοΈ Collection μ£Όμ λ©μλ

| λ©μλ                       | μ€λͺ                                                |
| ---------------------------- | --------------------------------------------------- |
| `boolean add(E e)`           | ν΄λΉ μ»¬λ μμ μ λ¬λ μμλ₯Ό μΆκ°                    |
| `void clear ()`              | ν΄λΉ μ»¬λ μμ λͺ¨λ  μμ μ κ±°                        |
| `boolean contains(Object o)` | ν΄λΉ μ»¬λ μμ΄ μ λ¬λ κ°μ²΄λ₯Ό ν¬ν¨νλμ§ νμΈ         |
| `boolean equals(Object o)`   | ν΄λΉ μ»¬λ μκ³Ό μ λ¬λ κ°μ²΄κ° κ°μμ§ νμΈ             |
| `boolean isEmpty()`          | ν΄λΉ μ»¬λ μμ΄ λΉμ΄μλμ§ νμΈ                       |
| `iterator<E> iterator()`     | ν΄λΉ μ»¬λ μμ λ°λ³΅μ(iterator)λ₯Ό λ°ν               |
| `boolean remove(Object o)`   | ν΄λΉ μ»¬λ μμμ μ λ¬λ κ°μ²΄λ₯Ό μ κ±°                  |
| `int size()`                 | ν΄λΉ μ»¬λ μμ μμμ μ΄ κ°μλ₯Ό λ°ν                 |
| `Object[] toArray()`         | ν΄λΉ μ»¬λ μμ λͺ¨λ  μμλ₯Ό Object νμμ λ°°μ΄λ‘ λ°ν |





