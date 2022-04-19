# 스프링 의존성 주입(DI)

스프링 삼각형

스프링의 기반이 되는 설계 개념을 표현한 것.

<img src="https://user-images.githubusercontent.com/51350374/163903321-b0650e7c-4d52-483b-8524-0cb28bd64f31.png" width="250" height="200"/>
스프링이란 IoC와 AOP를 지원하는 경량의 컨테이너 프레임워크이다.

**DI(Dependency Injection)**

- 의존 관계 주입이라고 하며 어떤 개체가 사용하는 의존 객체를 직접 만들어 사용하는게 아니라, 주입 받아 사용하는 방법(new 연산자를 이용해서 객체를 생성하는 것이라고 생각하면 된다)
- ex)장난감은 배터리가 있어야 움직일 수 있으며 즉 배터리에 의존하고있다. 장난감들에게 배터리를 넣어주는 것을 의존성 주입이라고 생각하면 좋다.

```java
@Service
public class BookService {

	private BookRepository bookRepository;

	public BookService(BookRepository bookRepository) {
		this.bookRepository = bookRepository;
	}
}
```

```java
public class BookRepository{

}
```

- BookService클래스가 만들어지기 위해서는 BookRepository 클래스를 필요로 한다.
이것을 **BookService는 BookRepository 클래스의 의존성을 가진다**라고 한다.
- 하지만 이처럼 코드를 설계하면 코드의 재활용성이 떨어지고, BookRepository클래스가 수정 되었을 때, BookService클래스도 함꼐 수정해야하는 문제가 발생한다. 즉, **결합도가 높다는 것**이다.
- BookRepository라는 클래스를 직접 new를 사용하여 객체를 주입하는 것이 아니라 생성자를 사용하여 주입받는 것을 IoC(Inversion of Control)이라고 한다.

new연산자로 주입

```java
public class BookService {
	public void save() {
		BookRepository bookRepository = new BookRepository();
	}
}
```

- 추가적으로 BookService와 BookRepository가 둘다 Bean으로 등록되어 있을 때 BookService의 생성자만 만들어주면 스프링 IoC 컨테이너가 BookRepository에 의존성 주입을 알아서 주입해줌.**(스프링 4.3 이후부터는 생성자가 하나인 경우는 @Autowired를 사용하지 않아도 된다)**
- 하지만 스프링이없이 직접  new연산자로 주입하면 IoC가 가능하다

[스프링 IoC컨테이너란?](https://github.com/outlastudy/2022-weekly-study/blob/%EC%B0%BD%EC%9A%A9/%EC%B0%BD%EC%9A%A9/spring%20DI/spring%20IoC%20container.md)

## 의존성 주입을 사용하는 이유(장점)

1. 의존성이 줄어든다.
2. 재사용성을 높여준다.
3. 테스트에 용이하다.
4. 코드를 단순화 시켜준다.
5. 사용하는 이유를 파악하기 수월하고 코드가 읽기 쉬워지는 점이 있다.(가독성이 높아진다.)
6. 종속성이 감소하기 때문에 변경에 민감하지 않다.
7. 결합도(coupling)는 낮추면서 유연성과 확장성은 향상 시킬 수 있다.
8. 객체간의 의존관계를 설정할 수 있다.
