# π° Vue Intro

- μ¬μ©μ μΈν°νμ΄μ€λ₯Ό λ§λ€κΈ° μν μ§λ³΄μ μΈ μλ°μ€ν¬λ¦½νΈ νλ μμν¬
- νλμ μΈ toolκ³Ό λ€μν λΌμ΄λΈλ¬λ¦¬λ₯Ό ν΅ν΄ SPA(Single Page Application)λ₯Ό μλ²½νκ² μ§μ!



<br>

## π³ SPA (Single Page Application)

- νμ¬ νμ΄μ§λ₯Ό λμ μΌλ‘ λ λλ§ν¨μΌλ‘μ¨ μ¬μ©μμ μν΅νλ μΉ μ νλ¦¬μΌμ΄μ
- λ¨μΌ νμ΄μ§λ‘ κ΅¬μ±λλ©° μλ²λ‘λΆν° μ΅μ΄μλ§ νμ΄μ§λ₯Ό λ€μ΄λ‘λνκ³ , μ΄νμλ λμ μΌλ‘ DOMμ κ΅¬μ±
  - μ²μ νμ΄μ§λ₯Ό λ°μ μ΄νλΆν°λ μλ²λ‘λΆν° μλ‘μ΄ μ μ²΄ νμ΄μ§λ₯Ό λΆλ¬μ€λ κ²μ΄ μλ, νμ¬ νμ΄μ§ μ€ νμν λΆλΆλ§ λμ μΌλ‘ λ€μ μμ±ν¨
- μ°μλλ νμ΄μ§ κ°μ μ¬μ©μ κ²½ν(UX)μ ν₯μ
  - λͺ¨λ°μΌ μ¬μ©λμ΄ μ¦κ°νκ³  μλ νμ¬ νΈλν½μ κ°μμ μλ, μ¬μ©μ±, λ°μμ±μ ν₯μμ λ§€μ° μ€μνκΈ° λλ¬Έ
- λμ μλ¦¬μ μΌλΆκ° CSR(Client Side Rendering)μ κ΅¬μ‘°λ₯Ό λ°λ¦

<br>

## π³ SPA λ±μ₯ λ°°κ²½

- κ³Όκ±° μΉμ¬μ΄νΈλ€μ μμ²­μ λ°λΌ λ§€λ² μλ‘μ΄ νμ΄μ§λ₯Ό μλ΅νλ λ°©μμ΄μμ
  - MPA (Multi Page Application)
- μ€λ§νΈν°μ΄ λ±μ₯νλ©΄μ λͺ¨λ°μΌ μ΅μ νμ νμμ±μ΄ λλλ¨
  - λͺ¨λ°μΌ λ€μ΄ν°λΈ μ±κ³Ό κ°μ ννμ μΉ νμ΄μ§κ° νμν΄μ§
- μ΄λ¬ν λ¬Έμ λ₯Ό ν΄κ²°νκΈ° μν΄ Vue.jsμ κ°μ νλ‘ νΈμλ νλ μμν¬κ° λ±μ₯
  - CSR, SPAμ λ±μ₯
- 1κ°μ μΉ νμ΄μ§μμ μ¬λ¬ λμμ΄ μ΄λ£¨μ΄μ§λ©° λͺ¨λ°μΌ μ±κ³Ό λΉμ·ν ννμ μ¬μ©μ κ²½νμ μ κ³΅

<br>

## π³ CSR (Client Side Rendering)

![image-20220509213317506](Vue%20Intro.assets/image-20220509213317506.png)

- μλ²μμ νλ©΄μ κ΅¬μ±νλ SSR λ°©μκ³Ό λ¬λ¦¬ ν΄λΌμ΄μΈνΈμμ νλ©΄μ κ΅¬μ±
- μ΅μ΄ μμ²­ μ HTML, CSS, JS λ± λ°μ΄ν°λ₯Ό μ μΈν κ°μ’ λ¦¬μμ€λ₯Ό μλ΅λ°κ³  μ΄ν ν΄λΌμ΄μΈνΈμμλ νμν λ°μ΄ν°λ§ μμ²­ν΄ JSλ‘ DOMμ λ λλ§νλ λ°©μ
- μ¦, μ²μμ λΌλλ§ λ°κ³  λΈλΌμ°μ μμ λμ μΌλ‘ DOMμ κ·Έλ¦Ό
- SPAκ° μ¬μ©νλ λ λλ§ λ°©μ

<hr>

- μ₯μ  
  1. μλ²μ ν΄λΌμ΄μΈνΈ κ° νΈλν½ κ°μ
     - μΉ μ νλ¦¬μΌμ΄μμ νμν λͺ¨λ  μ μ  λ¦¬μμ€λ₯Ό μ΅μ΄μ ν λ² λ€μ΄λ‘λ ν νμν λ°μ΄ν°λ§ κ°±μ 
  2. μ¬μ©μ κ²½ν(UX) ν₯μ
     - μ μ²΄ νμ΄μ§λ₯Ό λ€μ λ λλ§νμ§ μκ³  λ³κ²½λλ λΆλΆλ§μ κ°±μ νκΈ° λλ¬Έ
- λ¨μ 
  1. SSRμ λΉν΄ μ μ²΄ νμ΄μ§ μ΅μ’ λ λλ§ μμ μ΄ λλ¦Ό
  2. SEO(κ²μ μμ§ μ΅μ ν)μ μ΄λ €μμ΄ μμ (μ΅μ΄ λ¬Έμμ λ°μ΄ν° λ§ν¬μμ΄ μκΈ° λλ¬Έ)



<br>

## π³ SSR (Sever Side Rendering)

![image-20220509213339595](Vue%20Intro.assets/image-20220509213339595.png)

- μλ²μμ ν΄λΌμ΄μΈνΈμκ² λ³΄μ¬μ€ νμ΄μ§λ₯Ό λͺ¨λ κ΅¬μ±νμ¬ μ λ¬νλ λ°©μ
- JS μΉ νλ μμν¬ μ΄μ μ μ¬μ©λλ μ ν΅μ μΈ λ λλ§ λ°©μ

<hr>

- μ₯μ 
  1. μ΄κΈ° κ΅¬λ μλκ° λΉ λ¦
     - ν΄λΌμ΄μΈνΈκ° λΉ λ₯΄κ² μ»¨νμΈ λ₯Ό λ³Ό μ μμ
  2. SEO(κ²μ μμ§ μ΅μ ν)μ μ ν©
     - DOMμ μ΄λ―Έ λͺ¨λ  λ°μ΄ν°κ° μμ±λμκΈ° λλ¬Έ
- λ¨μ 
  - λͺ¨λ  μμ²­λ§λ€ μλ‘μ΄ νμ΄μ§λ₯Ό κ΅¬μ±νμ¬ μ λ¬
    - λ°λ³΅λλ μ μ²΄ μλ‘κ³ μΉ¨μΌλ‘ μΈν΄ μ¬μ©μ κ²½νμ΄ λ¨μ΄μ§
    - μλμ μΌλ‘ νΈλν½μ΄ λ§μ μλ²μ λΆλ΄μ΄ ν΄ μ μμ

<br>



# π Why Vue.js?

- Vanilla JS
  - ν μ μ κ° μμ±ν κ²μκΈμ΄ DOM μμ 100κ° 
  - μ΄ μ μ κ° λλ€μμ λ³κ²½νλ©΄, DBμ Updateμ λ³λλ‘ DOM μμ 100κ°μ μμ±μ μ΄λ¦μ΄ λͺ¨λ μμ λμ΄μΌ ν¨
  - 'λͺ¨λ  μμ'λ₯Ό μ νν΄μ 'μ΄λ²€νΈ'λ₯Ό λ±λ‘νκ³  κ°μ λ³κ²½ν΄μΌ ν¨
- Vue.js
  - DOMκ³Ό Dataκ° μ°κ²°λμ΄ μκ³  Dataλ₯Ό λ³κ²½νλ©΄ μ΄μ μ°κ²°λ DOMμ μμμ λ³κ²½
  - μ¦, μ°λ¦¬κ° μ κ²½ μ¨μΌ ν  κ²μ μ€μ§ Dataμ λν κ΄λ¦¬ (Developer Exp ν₯μ)

<BR>



## π³ MVVM Pattern

![image-20220509214429744](Vue%20Intro.assets/image-20220509214429744.png)

MVVM Pattern : μ νλ¦¬μΌμ΄μ λ‘μ§μ UIλ‘λΆν° λΆλ¦¬νκΈ° μν΄ μ€κ³λ λμμΈ ν¨ν΄

- Model
  - "Vueμμ Modelμ JavaScript Objectλ€."
  - Object === { key: value }
  - Modelμ Vue Instance λ΄λΆμμ dataλΌλ μ΄λ¦μΌλ‘ μ‘΄μ¬
  - μ΄ dataκ° λ°λλ©΄ View(DOM)κ° λ°μ

- View
  - "Vueμμ Viewλ DOM(HTML)μ΄λ€."
  - Dataμ λ³νμ λ°λΌμ λ°λλ λμ
- View Model
  - "Vueμμ ViewModelμ λͺ¨λ  Vue Instanceμ΄λ€."
  - Viewμ Model μ¬μ΄μμ Dataμ DOMμ κ΄λ ¨λ λͺ¨λ  μΌμ μ²λ¦¬
  - ViewModelμ νμ©ν΄ Dataλ₯Ό μΌλ§λ§νΌ μ μ²λ¦¬ν΄μ λ³΄μ¬μ€ κ²μΈμ§(DOM)λ₯Ό κ³ λ―Όνλ κ²

