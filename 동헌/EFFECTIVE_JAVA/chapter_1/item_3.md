## private 생성자나 열거 타입으로 싱글턴임을 보증하라

### 싱글턴이란? 인스턴스를 오직 하나만 생성할 수 있는 클래스를 뜻한다.

## 싱글턴을 생성하는 방법
1. public static final 필드 방식의 싱글턴
```java
public class Elvis {
    public static final Elvis INSTANCE = new Elvis();
    private Elvis() {...}

    public void leaveTheBuilding() {...}
}
```

- private 생성자는 Elvis INSTANCE를 초기화할 때 딱 한번만 호출된다.

- public 이나 protected 생성자가 없으므로 전체 시스템에 인스턴스가 하나뿐임이 보장된다.
- 클라이언트는 손 쓸 방법이 없음
  예외 : 권한이 있는 클라이언트는 리플렉션 API AccessibleObject.setAccessible을 이용해 private 생성자 호출 가능  
  방어 : 생성자를 수정하여 두번째 객체가 생성되려 할 때 예외를 던지게 함

장점
- 해당 클래스가 싱글턴임이 API에 명백히 드러난다.
  public static 필드가 final 이므로 절대로 다른 객체를 참조할 수 없음
- 간결함

단점
- 클라이언트에서 사용하지 않더라도 인스턴스가 항상 생성됨 > 메모리가 낭비됨

2. 정적 팩터리 방식의 싱글턴

```java
public class Elvis {
    private static final Elvis INSTANCE = new Elvis();
    private Elvis() {...}
    public static Elvis getInstance() {return INSTANCE;}

    public void leaveTheBuilding() {...}
}
```

정적 팩터리 방식으로 만들어진 인스턴스는 단 한번만 만들어진다.

getInstance는 항상 같은 객체의 참조를 반환한다.

리플렉션을 통한 예외는 적용됨

장점
1. API를 바꾸지 않고도 싱글턴이 아니게 변경할 수 있음
    예를 들어 호출하는 스레드별로 다른 인스턴스를 넘겨주게 할 수 있음
2. 원한다면 정적 팩터리를 제너릭 싱글턴 팩터리로 만들 수 있음(아이템 30)
3. 정적 팩터리의 메서드 참조를 공급자로 사용할 수 있음(아이템 43, 44)
    ex) Elvis::getInstance를 Supplier<Elvis>로 사용하는 식

위와 같은 장점이 필요하지 않다면, 1번 방식이 좋음

3. 열거 타입 방식의 싱글턴
```java
public enum Elvis {
    INSTANCE;
}
```

싱글턴을 만드는 세가지 방법 중 가장 바람직한 방법
1. public 필드 방식과 비슷하지만 더 간결함
2. 직렬화할 수 있음

대부분 상황에서는 원소가 하나뿐인 열거 타입이 싱글턴을 만드는 가장 좋은 방법임
- 단, 만들려는 싱글턴이 Enum 외의 클래스를 상속해야 한다면 이 방법은 사용할 수 없음
- 열거 타입이 다른 인터페이스를 구현하도록 선언할 수는 있음