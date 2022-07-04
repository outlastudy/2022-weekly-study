## 자바에서 기본으로 제공하는 Functional Interfaces

### 1. Function<T,R>
- T 타입을 받아서 R 타입을 리턴하는 함수 인터페이스
  - R apply(T t)
- 함수 조합용 메소드
  - andThen
  - compose
``` java
public class Foo {
    // apply 메소드를 구현한다.
    Function<Integer, Integer> plus10 = (i) -> i + 10;
    Function<Integer, Integer> multiply2 = (i) -> i * 2;

    // compose
    plus10.compose(multiply2).apply(2); // 14
    // andThen
    plus10.andThen(multiply2).apply(2); // 24

}
```
### 2. BiFunction<T, U, R>
- 두 개의 값(T, U)를 받아서 R 타입을 리턴하는 함수 인터페이스
  - R apply(T t , U u)

### 3. Consumer<T>
- T 타입을 받아서 아무값도 리턴하지 않는 함수 인터페이스
  - void Accept(T t)
- 함수 조합용 메소드
  - andThen

### 4. Supplier<T>
- T 타입의 값을 제공하는 함수 인터페이스
  - T get()
  
### 5. Predicate<T>
- T 타입을 받아서 boolean을 리턴하는 함수 인터페이스
  - boolean test(T t)
- 함수 조합용 메소드
  - And
  - Or
  - Negate

### 6. UnaryOperator<T>
- Function<T,R>의 특수한 형태로, 입력값 하나를 받아서 동일한 타입을 리턴하는 함수 인터페이스

### 7. BinaryOperator<T>
- BiFunction<T,U,R>의 특수한 형태로, 동일한 타입의 입력값 두개를 받아 리턴하는 함수 인터페이스

