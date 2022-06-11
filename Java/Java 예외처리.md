# 🧋 Java 예외처리

> 예외 발생 시 프로그램의 비 정상 종료를 막고 정상적인 실행 상태를 유지하는 것

[참고](http://www.tcpschool.com/java/java_exception_class)

<br>

### try ~ catch

```java
public static void main(String[] args) {
    int[] nums = {10};
    
    try {
    	// 예외가 발생할 수 있는 코드
        System.out.println(nums[2]);
        
    } catch (ArrayIndexOutOfBoundsException e) {
        // 예외가 발생했을 때 처리할 코드
        System.out.println("배열 크기 확인해라")
            
    } catch (Exception e) {
        // 예외가 발생했을 때 처리할 코드
    }
    finally {
        // 예외 발생 여부와 상관없이 무조건 실행될 코드, 중간에 return을 만나도 실행된다!
        System.out.println("위에 다 돌았음!!")
    }	
}
```

<br>

### Exception 클래스

![image-20220611153637314](Java%20%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC.assets/image-20220611153637314.png)



- Checked exception : 예외에 대한 대처 코드가 없으면 컴파일이 진행되지 않음

- Unchecked exception (RuntimeException의 하위 클래스) : 예외에 대한 대처 코드가 없더라도 컴파일은 진행됨

<br>





