# π§ Java μμΈμ²λ¦¬

> μμΈ λ°μ μ νλ‘κ·Έλ¨μ λΉ μ μ μ’λ£λ₯Ό λ§κ³  μ μμ μΈ μ€ν μνλ₯Ό μ μ§νλ κ²

[μ°Έκ³ ](http://www.tcpschool.com/java/java_exception_class)

<br>

### try ~ catch

```java
public static void main(String[] args) {
    int[] nums = {10};
    
    try {
    	// μμΈκ° λ°μν  μ μλ μ½λ
        System.out.println(nums[2]);
        
    } catch (ArrayIndexOutOfBoundsException e) {
        // μμΈκ° λ°μνμ λ μ²λ¦¬ν  μ½λ
        System.out.println("λ°°μ΄ ν¬κΈ° νμΈν΄λΌ")
            
    } catch (Exception e) {
        // μμΈκ° λ°μνμ λ μ²λ¦¬ν  μ½λ
    }
    finally {
        // μμΈ λ°μ μ¬λΆμ μκ΄μμ΄ λ¬΄μ‘°κ±΄ μ€νλ  μ½λ, μ€κ°μ returnμ λ§λλ μ€νλλ€!
        System.out.println("μμ λ€ λμμ!!")
    }	
}
```

<br>

### Exception ν΄λμ€

![image-20220611153637314](Java%20%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC.assets/image-20220611153637314.png)



- Checked exception : μμΈμ λν λμ² μ½λκ° μμΌλ©΄ μ»΄νμΌμ΄ μ§νλμ§ μμ

- Unchecked exception (RuntimeExceptionμ νμ ν΄λμ€) : μμΈμ λν λμ² μ½λκ° μλλΌλ μ»΄νμΌμ μ§νλ¨

<br>





