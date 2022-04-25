# 생성자에 매개변수가 많다면 빌더를 고려하라

### 정적 팩터리와 생성자에는 동일한 제약이 존재한다. 선택적 매개변수가 많을 경우 적절히 대응하기 어렵다.

```java
public class User {
    private final int age; // 필수
    private final int name; // 필수
    private final int weight; // 선택
    private final int birthday; // 선택
}
```

### 점층적 생성자 패턴
**점층적 생성자 패턴**을 많이 사용하였다. 필수 매개변수와 선택 매개변수 1개를 받는 생성자 선택 매개변수를 2개 받는 생성자 ... 형태로 늘려가는 방식이다.

```java
public User(int age, int name) {
    this.age = age;
    this.name = name;
}

public User(int age, int name, int weight) {
    this(age, name);
    this.weight = weight;
}

public User(int age, int name, int weight, int birthday) {
    this(age, name);
    this.weight = weight;
    this.birthday = birthday;
}
```
### 단점 
- 해당하는 값이 무엇을 뜻하는지 헷갈린다.
- 매개변수 개수를 주의해서 코드를 짜야한다.
- 순서가 바뀌더라도 컴파일러가 알지 못한다.
  
### 자바빈즈 패턴
자바빈즈 패턴이란? 매개변수가 없는 생성자를 객체로 생성한 후 setter 메서드를 통해서 원하는 매개변수의 값을 설정하는 방식이다.

점층적 생성자 패턴에서 보이던 단점을 보완할 수 있었다 하지만 단점은 존재한다.

### 단점
원하는 객체를 만들기 위해서 많은 메서드를 호출하여 값을 세팅해주어야 하고, 객체가 완전히 생성되기 전까지는 일관성이 무너진 상태에 놓이게 된다.
- 일관성이 깨진 객체를 만들면 버그를 심은 코드와 버그 때문에 디버깅이 쉽지 않다.
> 자바빈즈 패턴에서는 클래스를 불변으로 만들 수 없으며 스레드 안정성을 얻으려면 추가 작업을 해줘야한다.

단점을 보완하고자 생성이 끝난 객체를 freezing을 통해서 얼리기 전까지 사용할 수 없게 만드는 방법이 존재한다.

## 빌더패턴
```java
public class User {
    private final int age;
    private final int phoneNo;
    private int weight;
    private int tall;
    private int birthday;

    public User(Builder builder) {
        this.age = age;
        this.phoneNo = phoneNo;
        this.weight = weight;
        this.tall = tall;
        this.birthday = birthday;
    }

    public static class Builder {
        private final int age;
        private final int phoneNo;
        private int weight;
        private int tall;
        private int birthday;

        public Builder(int age, int phoneNo) {
            this.age = age;
            this.phoneNo = phoneNo;
        }

        public Builder weight(int weight) {
            this.weight = weight;
            return this;
        }

        public Builder tall(int tall) {
            this.tall = tall;
            return this;
        }

        public Builder birthday(int birthday) {
            this.birthday = birthday;
            return this;
        }

        public User build() {
            return new User(this);
        }
    }
}

User user = new User.Builder(20, 99999)
                    .weight(20)
                    .tall(190)
                    .birthday(0101)
                    .build();
```
해당 클래스는 불변이고, 모든 세터 메서드 들은 빌더 자신을 반환하기 때문에 연쇄적으로 호출이 가능하게 된다. 이를 fluent API 또는 method chaining이라고 한다.

### 빌더 패턴은 계층적으로 설계된 클래스와 함께 쓰기에 좋다.
각 계층의 클래스에 관련 빌더를 멤버로 정의하여 계층적으로 설계 할 수 있다.
추상 클래스는 추상 빌더로, 구체 클래스는 구체 빌더를 갖게 된다.

``` java
public abstract class Pizza {
    public enum Topping {HAM, MUSHROOM, ONION, SAUSAGE}
    final Set<Topping> toppings;

    abstract static class Builder<T extends Builder<T>> {
        EnumSet<Topping> toppings = EnumSet.noneOf(Topping.class);
        public T addTopping(Topping topping) {
            toppings.add(Objects.requireNonNull(topping));
            return self();
        }

        abstract Pizza build();

        protected abstract T self();
    }

    Pizza(Builder<?> builder) {
        toppings = builder.toppings.clone();
    }
}

public class NyPizza extends Pizza {
    public enum Size { SMALL, MEDIUM, LARGE }
    private final Size size;

    public static class Builder extends Pizza.Builder<Builder> {
        private final Size size;

        public Builder (Size size) {
            this.size = Objects.requireNonNull(size);
        }

        @Override
        public NyPizza build() {
            return new NyPizza(this);
        }

        @Override
        protected Builder self() {return this;}
    }

    private NyPizza(Builder builder) {
        super(builder);
        size = builder.size;
    }
}

NyPizza pizza = new NyPizza.Builder(SMALL)
        .addTopping(SAUSAGE).addTopping(ONION).build();
```

### 빌더 패턴의 단점
객체를 만들려면 그에 앞서 빌더부터 만들어야 한다. 빌더 생성 비용은 크지는 않지만 성능에 민감한 프로젝트에서는 주의해서 사용해야한다고 한다.
점층적 생성자 보다 코드가 장황해서 매개변수가 **4개 이상**은 되어야 값어치를 한다고 한다.
그래도 실무에서는 개발을 하다보면 매개변수가 점점 많아지는 경우가 많기 때문에 처음부터 빌더 패턴 적용을 고려해보는 것도 옳은 방법이라고 한다.