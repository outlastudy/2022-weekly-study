# **Reflection**

구체적인 클래스 타입을 알지 못해도 그 클래스의 정보에 접근할 수 있게 해주는 자바 API

```java
public class Car {
    private final String name;
    private int position;

    public Car(String name, int position) {
        this.name = name;
        this.position = position;
    }

    public void move() {
        this.position++;
    }

    public int getPosition() {
        return position;
    }
}
```

```java
public static void main(String[] args) {
    Object obj = new Car("foo", 0);
		obj.move();
}
```

```java
public static void main(String[] args) throws Exception {
    Object obj = new Car("foo", 0);
    Class carClass = Car.class; // 클래스의 이름으로 클래스의 정보를 가져옴
    Method move = carClass.getMethod("move");

    // move 메서드 실행, invoke(메서드를 실행시킬 객체, 해당 메서드에 넘길 인자)
    move.invoke(obj, null);

    Method getPosition = carClass.getMethod("getPosition");
    int position = (int)getPosition.invoke(obj, null);
    System.out.println(position);
    // 출력 결과: 1
}
```

### 가능한 이유

- JVM이 실행되면 사용자가 작성한 코드를 바이트 코드로 변환 → static 영역에 저장
- Reflection API는 이 정보를 바탕으로 가져옴

### 단점

- 오버헤드 : 컴파일 타임이 아닌 런타임에 동적으로 타입을 분석하고 정보를 가져와서??
    
    ⇒ JVM을 최적화 할 수 없음 (자바는 정적언어라서 컴파일 타임에 타입을 결정)
    
     → ex) 이미 인스턴스를 만들었어도 다시 접근하면 메모리 낭비
    
- 컴파일 타임에 확인되지 않고 런타임 시에만 발생하는 문제 발생 가능
- private 인스턴스 변수, 메소드에 접근해서 내부를 노출하면서 추상화가 깨짐 ??
    
    (접근 지시자 무시가능)
    

⇒ **<span style="color:red;">애플리케이션 개발</span>보단 <span style="color:yellow">프레임워크나 라이브러리</span>에서 많이 사용**



### Spring에서의 Reflection API

- **Spring Container의 BeanFactory**
    
    → Bean은 애플리케이션이 실행한 후 **런타임에 객체가 호출될 때** 동적으로 객체의 인스턴스를 생성 (이때 사용) 
    

- **Spring Data JPA**
    
    → Entity에 기본 생성자가 필요한 이유
    
     ⇒ 동적으로 객체 생성시 Reflection API를 활용하기 때문
    
    - Reflection API로 생성자의 인자 정보를 가져올 수 없음 ⇒ 객체 생성 불가능
        
        객체를 생성하면 필드 값등을 Reflection API로 넣어줄 수 있음