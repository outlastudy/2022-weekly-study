# 디자인 패턴

## 디자인 패턴이란?

어떤 문제를 해결하기 위한 방법 혹은 모델로서 설계, 구조상의 문제를 정리한 패턴의 집합이다.

소프트웨어 설계에 있어 공통적인 문제들에 대한 표준적인 해법과 작명법을 제안하며, 특정한 상황에서 구조적인 문제를 해결하는 방식으로 “효율적인 코드를 만들기 위한 방법론”이다.

디자인 패턴을 알고 있다면 여러 다양한 기술 및 프로그래밍 언어도 보다 쉽게 학습할 수 있고 재사용성이 뛰어난 객체 지향 소프트웨어를 개발할 수 있다.

그러기 위해서는 디자인 패턴을 무작정 외우는 것보단 어떠한 패턴이 있고, 수많은 디자인 패턴에서 코딩 노하우를 습득하여 어떠한 코드에 어떤 디자인 패턴을 적용시킬 지 자연스럽게 머릿속과 자신의 코드에 녹이는 것이 좋다.

## 디자인 패턴의 종류

### 생성 패턴(Creational Patterns)

객체 생성에 관련된 패턴이다.

객체의 생성과 조합을 캡슐화하여 특정 객체가 생성되거나 변경되어도 프로그램 구조에 영향이 없게 유연성을 제공한다.

- [싱글톤 패턴(Singleton)](https://github.com/outlastudy/2022-weekly-study/blob/master/%EC%84%B1%ED%97%8C/Design_Pattern/Creational_Patterns/Singleton.md)

- [추상팩토리 패턴(Abstract Factory)](https://github.com/outlastudy/2022-weekly-study/blob/master/%EC%84%B1%ED%97%8C/Design_Pattern/Creational_Patterns/Abstract_Factory.md)

- [빌더 패턴(Builder)](https://github.com/outlastudy/2022-weekly-study/blob/master/%EC%84%B1%ED%97%8C/Design_Pattern/Creational_Patterns/Builder.md)

- [팩토리 메서드 패턴(Factory Method)](https://github.com/outlastudy/2022-weekly-study/blob/master/%EC%84%B1%ED%97%8C/Design_Pattern/Creational_Patterns/Factory_Method.md)

- [원형 패턴(Prototype)](https://github.com/outlastudy/2022-weekly-study/blob/master/%EC%84%B1%ED%97%8C/Design_Pattern/Creational_Patterns/Prototype.md)

### 구조 패턴(Structural Patterns)

클래스나 객체를 조합하여 더 큰 구조로 만드는 패턴이다.

서로 다른 인터페이스를 지닌 2개의 객체를 묶어 단일 인터페이스를 제공하거나 객체들을 서로 묶어 새로운 기능을 제공하는 패턴이다.

- [어댑터 패턴(Adapter)](https://github.com/outlastudy/2022-weekly-study/blob/master/%EC%84%B1%ED%97%8C/Design_Pattern/Creational_Patterns/Adapter.md)

- [컴포지트 패턴(Composite)](https://github.com/outlastudy/2022-weekly-study/blob/master/%EC%84%B1%ED%97%8C/Design_Pattern/Creational_Patterns/Composite.md)

- [파사드 패턴(Facade)](https://github.com/outlastudy/2022-weekly-study/blob/master/%EC%84%B1%ED%97%8C/Design_Pattern/Creational_Patterns/Facade.md)

- [프록시 패턴(Proxy)](https://github.com/outlastudy/2022-weekly-study/blob/master/%EC%84%B1%ED%97%8C/Design_Pattern/Creational_Patterns/Proxy.md)

### 행위 패턴(Behavioral Patterns)

객체나 클래스 사이의 알고리즘이나 책임 분배에 관련된 패턴이다.

한 객체로서 혼자 수행할 수 없는 작업을 여러 객체로 분배 방법과 분배에 있어 객체 사이의 결합도를 최소화 하는 데 중점을 두는 방식이다.

## 참조

- [https://coding-factory.tistory.com/708](https://coding-factory.tistory.com/708)
- [https://gmlwjd9405.github.io/2018/07/06/design-pattern.html](https://gmlwjd9405.github.io/2018/07/06/design-pattern.html)
- [https://minnnne.tistory.com/123](https://minnnne.tistory.com/123)