# Item 1
# 생성자 대신 정적 팩터리 메서드를 고려하라

## 객체를 생성 하는 방법
1. public 생성자
2. 정적 팩터리 메서드(static factory method)

``` java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE
}
```
위의 코드는 boolean 기본 타입의 박싱 클래스인 Boolean에서 발췌한 간단한 예시 코드이다.
기본 타입인 boolean 값을 받아 Boolean 객체 참조로 변환해준다.

> 박싱 클래스란? (wrapper class를 말하는 것 같다.)   
> 자바에는 기본형 자료형 (int, double, char 등등) 이 존재하고 포장 클래스(wrapper class) 
> 가 있어서 기본형 자료형을 객체로 다루어야 할 경우에 사용한다.
> 
> 박싱(boxing)과 언박싱(unboxing)이 존재한다.   
> 박싱은 기본형을 참조형으로 변환하는 것이고 언박싱은 참조형을 기본형으로 바꾸는 것이다. 

## 정적 팩터리 메서드의 장점
1. **이름을 가질 수 있다.**   
   생성자를 통해서 객체를 생성하면 객체의 특성을 제대로 설명하지 못한다. 하지만 정적 팩터리 메서드를 이용하면 이름을 통해서 특성을 설명할 수 있다.   
   BigInteger(int, int, Random) | BigInteger.probablePrime   
   위와 같이 정적 팩터리 메서드를 사용한다면 코드를 읽는 사람도 클래스 설명 문서를 찾아보지 않고도 의미를 알아볼 수 있다. 그러므로 정적 팩터리 메서드의 이름을 특성을 잘 드러나게끔 만들어서 사용하자.

2. **호출될 때마다 인스턴스를 새로 생성하지 않아도 된다.**   
   이 덕분에 불변 클래스는 인스턴스를 미리 만들어 놓거나 새로 생성한 인스턴스를 캐싱하여 재활용하는 식으로 불필요한 객체 생성을 피할 수 있다. 대표적인 예로 Boolean.valueOf(boolean) 메서드는 객체를 아예 생성하지 않기 떄문에 같은 객체가 자주 요청되는 상황이라면 성능을 상당히 끌어올려 준다. **플라이웨이트 패턴**도 이와 비슷한 기법이라 할 수 있다.   
   반복되는 객체 요청에 같은 객체를 반환하는 식으로 정적 팩터리 방식의 클래스는 언제 어느 인스턴스를 살아 있게 할지를 철저히 통제할 수 있다. 이런 클래스를 인스턴스 통제 클래스라 한다.

    > 플라이웨이트 패턴이란?   
    > 어떤 클래스의 인스턴스 한 개만 가지고 여러개의 "가상 인스턴스"를 제공하고 싶을 때 사용하는 패턴이다. 즉, 인스턴스를 가능한 대로 공유시켜 쓸데없이 new 연산자를 통한 메모리 낭비를 줄이는 방식이다.

    > **인스턴스를 통제하는 이유?**   
    > 인스턴스를 통제하면 클래스를 싱글턴(Item 3)으로 만들 수도 있고, 인스턴스화 불가(Item 4)로 만들수도 있다. 또한 불변 값 클래스에서 동치인 인스턴스가 하나뿐임을 보장(Item 17)할 수 있다.(a == b 일때만 a.equals(b)가 성립). 인스턴스 통제는 플라이웨이트 패턴의 근간이 되며, 열거타입(Item 34)은 인스턴스가 하나만 만들어짐을 보장한다.

3. **반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.**   
   이는 반환할 객체의 클래스를 자유롭게 선택할 수 있게 하는 '엄청난 유연성'을 선물한다.   
   API를 만들 때 이를 응용하면 구현 클래스를 공개하지 않고 그 객체를 반환할 수 있어 API를 작게 유지할 수 있다.
   ```java
    public interface Car {...}

    public class ElectricCar implements Car {
        int position = 0;

        public static Car create(int position) {
            return new ElectricCar(position);
        }
    }
   ```
   자바 컬렉션 프레임워크는 45개의 유틸리티 구현체를 제공했는데 이 구현체는 대부분 인스턴스 불가 클래스인 java.util.Collections에서 정적 팩터리 메서드를 통해 얻도록 했다.   
   이렇게 컬렉션을 통해서 우리는 API를 훨씬 작게 만들수 있게 되었고, 정적 팩터리 메서드를 통해서 구현체를 제공 받기 때문에 실제 구현 클래스가 무엇인지 알아보지 않아도 된다.   
   자바 8부터는 public static 메서드를 인터페이스에 추가가 가능하게 되면서 Collections라는 클래스를 만들지 않고도 인터페이스에 구현 가능하게 되었다. 그리고 자바 9부터는 private static 까지 허락 하지만 정적 필드와 정적 멤버 클래스는 여전히 public 이여야 한다.

4. **입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.**   
   반환 타입의 하위 타입이기만 하면 어떤 클래스의 객체를 반환하든 상관없다. 심지어 버전에 따라 다른 클래스의 객체를 반환해도 된다.   
   대표적인 예로 EnumSet 클래스(Item 36)는 public 생성자 없이 정적 팩터리 메서드 만으로 제공하는데 매개변수의 개수에 따라 다른 두가지의 하위 클래스 객체를 반환한다. 64개 이하면 long 변수 하나로 관리하는 RegularEnumSet의 인스턴스를, 65개 이상이면 long 배열로 관리하는 JumboEnumSet의 인스턴스를 반환한다.

5. **정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.**   
   해당 정적 팩터리 메서드가 실행되는 시점에 어떤 코드가 존재하는지에 따라 다르게 작동한다.
   이는 JDBC에서 볼 수 있다. getConnection()을 쓸 때 return 되어서 나오는 객체는 드라이버마다 다르다.

## 정적 팩터리 메서드 단점
1. **상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스르 만들 수 없다.**   
   이러한 이유로 java.util.Collections는 상속할 수 없다.   
   이 제약은 상속 보다 컴포지션(Item 18)을 사용하도록 유도하고 불변 타입(Item 17)으로 만들려면 이 제약을 지켜야 한다는 점에서 오히려 장접이 될 수도 있다.
2. **정적 팩터리 메서드는 프로그래머가 찾기 어렵다.**   
   생성자는 Javadoc이 자동으로 상단에 모아서 보여준다. 하지만 정적 팩터리 메서드는 그렇지 않다.

## 정적 팩터리 메서드 명명 방식
- from : 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드
- of : 여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드
- valueOf : from 과 of의 더 자세한 버전
- instance 혹은 getInstance : 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지는 않는다.
- create 혹은 newInstance : 위와 같지만 매번 새로운 인스턴스를 생성해 반환함을 보장한다.
- getType : getInstance와 같으나 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. "Type"은 팩터리 메서드가 반환할 객체의 타입이다.
- newType : newInstance와 같으나 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다. "Type"은 팩터리 메서드가 반환할 객체의 타입이다.
- type : getType 과 newType의 간결한 버전