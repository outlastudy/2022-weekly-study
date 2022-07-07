## Enum
JAVA Enum 타입은 일정 개수의 상수 값을 정의하고, 그 외의 값은 허용하지 않는다.
과거에는 특정 상수 값을 사용하기 위해선 모두 상수로 선언해서 사용했다.
```java
public static final String TODAY_MON = "Monday";
public static final String 2022_WEEK_TODAY_TUE = "Tuesday";
```
이렇게 사용하면 개발자가 실수하기도 쉽고 한눈에 알아보기도 쉽지 않다.
그리고 관련있는 값들끼리 묶으려면 접두어를 사용해서 점점 변수명도 지저분해진다.

```java
public enum Day {
    MON, TUE, WED
}
```
이처럼 단순하게 요일을 열거한 Enum 클래스를 만들 수 있다.
하지만 각각의 요소들이 특정한 값을 갖게 하고 싶을 수도 있다.
-> 생성자와 final 필드를 추가한다.

```java
public enum Day {
    MON("Monday"),
    TUE("Tuesday"),
    WED("Wednesday");

    private final String label;

    Day(String label) {
        this.label = label;
    }

    public String label() {
        return label;
    }

}

Day.MON.name() // MON
Day.MON.label() // Monday
```
final 변수 값 이름을 name으로 정하는 것은 피하는 것이 좋다.
Enum 타입에서 name()이라는 메소드를 제공하기 때문에 헷갈릴 수 있다.

### 개발을 할때 Enum을 통해서 얻는 기본적인 장점
문자열과 비교해, IDE의 적극적인 지원을 받을 수 있다.
    - 자동완성, 오타검증, 텍스트 리팩토링 등등
허용 가능한 값들을 제한할 수 있다.
리팩토링시 변경 범위가 최소화 된다.
    - 내용의 추가가 필요하더라도, Enum 코드외에 수정할 필요가 없다.

