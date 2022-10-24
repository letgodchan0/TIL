# Java call by ~

> call by value와 call by reference

<br>

### - call by value

```java
void func(int n) {
    n = 20;
}

void main() {
    int n = 10;
    func(n);
    printf("%d", n);
}
```

- 함수가 호출될 때 메모리 공간 안에서는 함수를 위한 별도의 임시 공간이 생성된다. (종료 시 해당 공간이 사라진다.)
- call by value 호출 방식은 함수 호출 시 전달되는 변수 값을 복사해서 함수 인자로 전달한다. 이 때 복사된 인자는 함수 안에서 지역적으로 사용되기 때문에 `local value` 속성을 가진다. 
- 출력되는 값은 10

<br>

### - call by reference

```java
void func(int *n) {
    *n = 20;
}

void main() {
    int n = 10;
    func(&n);
    printf("%d", n);
}
```

- call by reference 호출 방식은 함수 호출 시 인자로 전달되는 변수의 레퍼런스를 전달한다.

- 따라서 함수 안에서 인자 값이 변경되면, 아규먼트로 전달된 객체의 값도 변경된다.
- 출력되는 값은 20

<br>

```text
자바의 경우, 항상 call by value로 값을 넘긴다. 
reference type (참조 자료형)을 넘길 시에는 해당 객체의 주소값을 복사하여 이를 가지고 사용한다.
```

