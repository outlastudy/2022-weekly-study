## 람다 표현식 

```java
Function<Integer, Integer> plus10 = (i) -> i + 10;
```
- (인자 리스트) -> {바디}
  - 인자 리스트
    - 인자가 없을때 : ()
    - 인자가 한개일 때 : (one) or one
    - 인자가 여러개 일때 : (one, two)
    - 이자의 타입은 생략 가능, 컴파일러가 추론 하지만 명시할 수도 있다.
      - (Integer one, String two)

  - 바디
    - 화살표 오른쪽에 함수 본문을 정의한다.
    - 여러 줄인 경우에 {}를 사용해서 묶는다.
    - 한 줄인 경우에 생략 가능, return도 생략 가능.
- 변수 캡처
  - 로컬 변수 캡처
    - final이거나 effective final인 경우에만 참조할 수 있다.
  - effective final
    - 자바 8부터 지원하는 "사실상" final인 변수
    - final 키워드 사용하지 않는 변수를 익명 클래스 구현체 또는 람다에서 참조할 수 있다.
  - 익명 클래스 구현체와 달리 '쉐도잉'하지 않는다
    - 익명 클래스는 새로 scope을 만들지만 람다는 람다를 감싸고 있는 스콥과 같다.

```java
public class Foo {
    public static void main(String[] args) {
        Foo foo = new Foo();
        foo.run();
    }

    private void run() {
        // 코드에서 baseNumber를 변경하지 않으면 effective final로 인해
        // final int baseNumber = 10; 와 같다.
        int baseNumber = 10;

        // 쉐도잉
        
        class LocalClass {
            void printBaseNumber() {
                System.out.println(baseNumber);
            }
        };

        Consumer<Integer> integerConsumer = new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) {
                System.out.println(baseNumber);
            }
        };

        IntConsumer printInt = (i) -> {
            System.out.println(i + baseNumber);
        };

    }

}
```

