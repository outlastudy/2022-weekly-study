# 빌더 패턴(Builder Pattern)

## 빌더 패턴이란?

복잡한 객체를 생성하는 방법을 정의하는 클래스와 표현하는 방법을 정의하는 클래스를 별도로 분리하여, 서로 다른 표현이라도 객체를 생성할 수 있는 동일한 절차를 제공하는 패턴이다.

생성해야 되는 객체가 Optional한 속성을 많이 가질 때 주로 사용한다.

## 코드를 통한 설명

```java
public class User {
	private int userId;   // 필수
	private String name;  // 필수
	private String sex;  // 선택
	private int age;      // 선택
	private String email; // 선택
}
```

위와 같이 User 클래스에 필수적으로 받아야 하는 정보와 선택적으로 받아야 하는 정보가 있다고 가정한다. 빌더 패턴을 사용하지 않는다면 일반적으로 **점층적 생성자 패턴**(Overloading을 통해 생성자를 여러 개 만드는 것)을 적용한다.

```java
public class User {
    private int userId;
    private String name;
    private String sex;
    private int age;
    private String email;

    public User(int userId, String name) {
        this.userIdx = userId;
        this.name = name;
    }

    public User(int userId, String name, String sex) {
        this.userIdx = userId;
				this.name = name;
        this.sex= sex;
    }
	
			...

}
```

하지만 위와 같이 점층적 생성자 패턴을 사용하게 되면 코드가 길어지고, 지저분해진다.

```java
User user = new User(1, "홍길동", "Male", 28, "gil-dong@example.com");
```

그리고 위와 같이 작성하다 보면 실수로 순서를 바꾸거나 생성자 인자가 많을 때 알아보기 힘들다는 불편한 점이 존재한다.

그래서 이러한 단점을 보완하기 위해 setter 메소드를 사용한 자바 빈(Bean) 패턴이 고안되었다.

```java
public static void main(String[] args) {
	User user = new User();
	user.setUserId(1);
	user.setName("홍길동");
	user.setSex("Male");
	user.setAge(28);
	user.setEmail("gil-dong@example.com");
}
```

가독성은 훨씬 좋아지고 객체를 생성하기도 쉬워졌지만, setter를 이용하여 객체를 생성하게 되면 함수 호출을 1회 이상으로 여러 번 호출해야 하며, 객체의 일관성(consistency)가 일시적으로 깨질 수 있다. 또한 immutable 객체를 생성할 수 없다.(객체가 변할 여지가 존재 → 스레드간 공유 가능한 상태가 존재하기 때문)

따라서 생성자 패턴과 자바 빈 패턴의 장점을 결합한 것이 **빌더 패턴**이다.

필요한 객체를 직접 생성하는 것이 아닌 필수 인자들을 생성자에 전부 전달하여 빌더 객체를 만들고, 선택 인자는 가독성 좋은 코드로 인자를 넘길 수 있다. 이러한 장점들을 가지며 객체 일관성 마저 깨지 않는다.

```java
/** User **/
public class User {
	// require
	private int userId;
	private String name;
	
	// optional
	private String sex;
	private int age;
	private String email;

	private User(UserBuilder builder) {
		this.userId = builder.userId;
		this.name = builder.name;
		this.sex = builder.sex;
		this.age = builder.age;
		this.email = builder.email;
	}

	// Builder Class
	public static class UserBuilder {
		// require
		private int userId;
		private String name;
		
		// optional
		private String sex;
		private int age;
		private String email;

		public UserBuilder(int userId, String name) {
			this.userId = userId;
			this.name = name;
		}

		public UserBuilder sex(String sex) {
			this.sex = sex;
			return this;
		}

		public UserBuilder age(int age) {
			this.age = age;
			return this;
		}

		public UserBuilder email(String email) {
			this.email = email;
			return this;
		}

		public User build() { return new User(this); } // this == UserBuilder

}
```

여기서 살펴볼 것은 User 클래스가 setter 메소드가 없고(따로 getter 메소드는 생략), public 생성자가 없다는 것이다. 그렇기에 User 객체를 얻기 위해선 오직 UserBuilder 클래스를 통해서만 가능하다.

```java
public class TestBuilderPattern {
	public static void main(String[] args) {
		User user = new User.UserBuilder(1, "홍길동")
												.sex("Male")
												.age(28)
												.email("gil-dong@example.com")
												.build();
	}
}
```

## 빌더 패턴을 사용하는 이유

### 1. 필요한 데이터만 설정

- 생성자 패턴에서 age라는 파라미터가 필요 없는 상황이라면 age가 없는 생성자를 새로 만들어야 함
- 불필요한 코드의 양을 줄이는 이점을 가져옴

### 2. 유연성 확보

- 생성자 패턴에서 새로운 변수 weight를 추가한다면 기존의 코드를 전부(노가다..) 수정해야하는 매우 곤란한 상황이 생김
- 새로운 변수가 추가되어도 기존의 코드에 영향을 주지 않을 수 있음

### 3. 가독성 향상

- 직관적으로 어떤 데이터에 어떤 값이 설정되는지 쉽게 파악 가능

### 4. 변경 가능성 최소화

- setter (수정자 패턴)으로 구현하면 변경 가능성을 열어두어 유지보수 시 값이 할당된 지점을 찾기 힘들게 하여 불필요한 코드 리딩 유발
- 클래스 변수의 변경 가능성을 최소화 하기 위해 final로 선언하여 불변성 확보
    
    ```java
    public class User {
    	private final int userId;
    	...
    }
    ```
    
- 경우에 따라 클래스 변수에 final을 붙일 수 없는 경우도 있음
- final 없이 setter을 구현하지 않아도 동일한 효과를 얻음

## 결론

객체를 생성하는 **대부분의 경우 빌더 패턴을 적용**하는 것이 좋다. 물론 예외 케이스도 존재한다.

- 객체의 생성을 라이브러리로 위임하는 경우
- 변수의 개수가 2개 이하이며, 변경 가능성이 없는 경우

예를 들어 **엔티티(Entity) 객체 또는 도메인(Domain) 객체로부터 DTO를 생성하는 경우**라면 직접 빌더를 만들고 하는 작업이 번거로우므로 **MapStruct나 Model Mapper와 같은 라이브러리를 통해 생성**을 위임할 수 있다. **변수가 늘어날 가능성이 거의 없으며, 변수의 개수가 2개 이하인 경우**에는 정**적 팩토리 메소드를 사용**하는 것이 더 좋을 수도 있다. 왜냐하면 불필요하게 빌더를 사영해서 코드가 비대해질 수 있기 때문이다. 그 외에도 빌더보다 다른 방법을 이용하는게 좋은 상황이 있을 수 있으므로 **변수의 개수와 변경 가능성을 중점적으로 보고 빌더 패턴을 적용할지 판단**하면 된다.

## 참조

- [https://mangkyu.tistory.com/163](https://mangkyu.tistory.com/163)
- [https://devlog-wjdrbs96.tistory.com/207](https://devlog-wjdrbs96.tistory.com/207)
- [https://readystory.tistory.com/121](https://readystory.tistory.com/121)
- [https://4z7l.github.io/2021/01/19/design_pattern_builder.html](https://4z7l.github.io/2021/01/19/design_pattern_builder.html)