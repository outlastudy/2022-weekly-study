# 싱글톤 패턴(Singleton Pattern)

## 싱글톤 패턴이란?

전역 변수를 사용하지 않고 객체 인스턴스가 오직 1개만 생성되는 패턴으로 생성된 객체를 어디에서든지 참조하도록 하는 패턴이다.

하나의 인스턴스만을 생성하는 책임이 있고 getInstance 메서드를 통해 모든 클라이언트에게 동일한 인스턴스를 반환하는 작업을 한다.

## 싱글톤 패턴을 사용하는 이유

한번의 new 연산자를 통해 고정된 메모리 영역을 사용하기에 추후 해당 객체에 접근 시 메모리 낭비를 방지할 수 있다. 또한 이미 생성된 인스턴스를 활용하기에 속도 측면에서도 이점이 있다. (두 번째 이용부터 객체 로딩 시간이 줄어듦)

또한 싱글톤 인스턴스가 전역으로 사용되는 인스턴스이기에 다른 클래스의 인스턴스들이 접근하여 사용할 수 있어 클래스 간 데이터 공유가 쉽다. 하지만 여러 클래스의 인스턴스에서 싱글톤 인스턴스 데이터에 동시 접근을 하게 되면 동시성 문제가 발생할 수 있다.

이 외에도 도메인 관점에서 인스턴스가 한 개만 존재하는 것을 보증하고자 할 때 싱글톤 패턴을 사용한다.

## 싱글톤 패턴의 문제점

### 1. 싱글톤 패턴을 구현하는 많은 코드 필요

멀티스레딩 환경에서 발생할 수 있는 동시성 문제 해결을 위해 syncronized 키워드를 사용해야한다.

### 2. 테스트의 어려움

싱글톤 인스턴스는 자원을 공유하고 있기에 테스트가 결정적으로 격리된 환경에서 수행하려면 매번 인스턴스를 초기화 시켜주어야 한다. 그렇게 하지 않으면 애플리케이션 전역에서 상태를 공유하기에 테스트 수행이 온전하지 못하다.

### 3. 의존 관계상 클라이언트가 구체 클래스에 의존

new 키워드를 직접 사용하여 클래스 안에서 객체를 생성하고 있으므로, 이는 SOLID 원칙 중 DIP를 위반하게 되고 OCP 원칙 또한 위반할 가능성이 높다.

> DIP(의존 관계 역전 원칙, Dependency inversion principle)
- 의존 관계를 맺을 때, 변화하기 쉬운 것보단 변화하기 어려운 것에 의존해야 한다.
> 

> OCP(개방-폐쇄 원칙, Open-Closed Principle)
- 기존의 코드를 변경하지 않고(Closed) 기능을 수정하거나 추가할 수 있도록(Open) 설계해야 한다.
> 

### 4. 그 외

자식 클래스를 만들 수 없으며, 내부 상태를 변경하기 어려움이 있는 등 여러가지 문제가 존재한다.

즉, 이러한 문제들을 가지고 있는 싱글톤 패턴은 유연성이 많이 떨어지는 패턴이다.

## 예제 - 프린터 관리자 만들기

- 프린터 하나를 여러명이 공유해서 사용한다 가정

```java
public class Printer {
	public Printer() { }
	public void print(Resource r) { ... }
}
```

- Printer 클래스를 사용해 프린터를 이용하려면 Client 프로그램에서 new Printer()가 반드시 한번만 호출되도록 해야한다. (프린터는 하나이기에)
- 이를 해결하는 방법은 생성자를 외부에서 호출할 수 없게 하는 것
    - Printer 클래스 생성자를 private으로 선언

```java
public class Printer {
	private Printer() { } // Printer 생성자를 외부에서 사용 불가
	public void print(Resource r) { ... }
}
```

- 자기 자신 프린터에 대한 인스턴스를 하나 만들어 외부에 제공해줄 메서드가 필요하다.
    - static 메서드 / static 변수
    - 만약 new Printer()가 호출되기 전이면 인스턴스 메서드인 print() 메서드는 호출할 수 없다.

```java
public class Printer {
	// 외부에 제공할 자기 자신의 인스턴스
	private static Printer printer = null;
	private Printer() { }
	// 자기 자신의 인스턴스를 외부에 제공
	public static Printer getPrinter() {
		if (printer == null) {
			// Printer Instance 생성
			printer = new Printer();
		}
		return printer;
	}
	public void print(String str) {
		System.out.println(str);
	}
}
```

- Client

```java
public class User {
  private String name;
  public User(String name) { this.name = name; }
  public void print() {
    Printer printer = printer.getPrinter();
    printer.print(this.name + " print using " + printer.toString());
  }
}
public class Client {
  private static final int USER_NUM = 5;
  public static void main(String[] args) {
    User[] user = new User[USER_NUM];
    for (int i = 0; i < USER_NUM; i++) {
      // User 인스턴스 생성
      user[i] = new User((i+1))
      user[i].print();
    }
  }
}
```

### 문제점

**다중 스레드**에서 Printer 클래스를 이용할 때 인스턴스가 1개 이상 생성되는 경우가 발생할 수 있다.

- **경합 조건**(Race Condition)을 발생시키는 경우
    1. Printer 인스턴스가 아직 생성되지 않았을 때 스레드 1이 getPrinter 메서드의 if문을 실행해 이미 인스턴스가 생성되었는지 확인한다. 현재 Printer 변수는 null.
    2. 스레드 1이 생성자를 호출하여 인스턴스를 생성하기 전 스레드 2가 if문을 실행하여 printer 변수가 null인지 확인한다. 현재 printer 변수는 null이므로 인스턴스를 생성하는 생성자를 호출하는 코드를 실행하게 된다.
    3. 스레드 1도 스레드 2와 마찬가지로 인스턴스를 생성하는 코드를 실행하게 되면 결과적으로 Printer 클래스의 인스턴스가 2개 생성된다.

> 경합 조건(Race Condition) : 메모리와 같은 동일한 자원을 2개 이상의 스레드가 이용하려고 경합하는 현상
> 

### 해결책

사실 **다중 스레드 애플리케이션이 아닌 경우 아무런 문제가 없다.**

- 정적 변수에 인스턴스를 만들어 바로 초기화 하는 방법(Eager Initialization)
- 인스턴스를 만드는 메서드에 동기화 하는 방법(Thread-Safe Initialization)

1. Eager Initialization
    
    ```java
    public class Printer {
       // static 변수에 외부에 제공할 자기 자신의 인스턴스를 만들어 초기화
       private static Printer printer = new Printer();
       private Printer() { }
       // 자기 자신의 인스턴스를 외부에 제공
       public static Printer getPrinter(){
         return printer;
       }
       public void print(String str) {
         System.out.println(str);
       }
    }
    ```
    
    <aside>
    💡 static 변수 : 객체가 생성되기 전 클래스가 메모리에 로딩될 때 만들어져 초기화가 한 번만 실행된다.
    
    </aside>
    
2. Thread-Safe Initialization
    
    ```java
    public class Printer {
       // 외부에 제공할 자기 자신의 인스턴스
       private static Printer printer = null;
       private int counter = 0;
       private Printer() { }
       // 인스턴스를 만드는 메서드 동기화 (임계 구역)
       public synchronized static Printer getPrinter(){
         if (printer == null) {
           printer = new Printer(); // Printer 인스턴스 생성
         }
         return printer;
       }
       public void print(String str) {
         // 오직 하나의 스레드만 접근을 허용함 (임계 구역)
         // 성능을 위해 필요한 부분만을 임계 구역으로 설정한다.
         synchronized(this) {
           counter++;
           System.out.println(str + counter);
         }
       }
    }
    ```
    
    - 인스턴스를 만드는 메서드를 임계 구역으로 변경
        - 다중 스레드 환경에서 동시에 여러 스레드가 getPrinter 메서드를 소유하는 객체에 접근하는 것을 방지한다.
    - 공유 변수에 접근하는 부분을 임계 구역으로 변경
        - 여러 개의 스레드가 하나뿐인 counter 변수 값에 동시에 접근해 갱신하는 것을 방지한다.
    - getInstance()에 Lock을 하는 방식이라 속도가 느리다.

## 결론

오직 하나의 인스턴스 생성을 보증하여 효율을 찾을 수 있지만 그에 못지않게 많이 딸려오는 문제점도 많다. 싱글톤 패턴은 단독으로 사용한다면 객체 지향에 위반되는 사례들이 많지만 스프링 컨테이너 같은 프레임워크의 도움을 받으면 문제점이 보완되면서 장점의 혜택을 누릴 수 있다.

위에서 살펴본 사용 시 장점과 문제점을 trade-off 하여 잘 고려해 사용하는 것이 좋다.

## 참조

- [https://tecoble.techcourse.co.kr/post/2020-11-07-singleton/](https://tecoble.techcourse.co.kr/post/2020-11-07-singleton/)
- [https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html](https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html)
- [https://devmoony.tistory.com/43](https://devmoony.tistory.com/43)