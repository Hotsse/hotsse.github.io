---
layout: post
title: 객체지향 설계 SOLID 원칙
categories: [Java]
comments: true
---

객체지향 설계 SOLID 원칙

단일 책임 원칙 (Single responsibility principle, SRP)
- 한 클래스는 하나의 책임만 가져야 한다.
- 책임의 단위는 모호하며, 문맥과 상황에 따라 다르다.
- 변경이 있을 때, 파급효과가 적으면 SRP 를 따르고 있다고 판단한다.

개방-폐쇄 원칙 (Open/closed principle, OCP)
- 소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.
- Java의 다형성을 활용하여 인터페이스를 통한 확장 개발이 가능하나, 호출코드에서 구현 객체(class) 를 지정해야 하기에, 순수 Java 만으로는 호출부의 코드 변경을 막을 수 없다.
  - ex) MemberRepository m = new MemoryMemberRepository(); // 구현 객체의 변경을 위해선 호출코드를 변경할 수 밖에 없다.
- "Spring DI Container 가 이를 보완해 줄 수 있다."

리스코프 치환 원칙 (Liskov substitution principle, LSP)
- 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.
- 구현 객체는 인터페이스의 의도대로 규약을 지켜 구현되어야 한다.
  - ex) (interface)car.excel() 의 구현체인 (class)k3.excel() 을 뒤로 가게 구현한다면 잘못된 것이다. (의도와 맞지 않음)

인터페이스 분리 원칙 (Interface segregation principle, ISP)
- 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다.
- 관심사와 목적의 분리가 가능하다면 인터페이스는 쪼개는 것이 좋다.
- 단일 책임 원칙(=SRP) 와도 일맥상통한 부분이다.

의존관계 역전 원칙 (Dependency inversion principle, DIP)
- 프로그래머는 추상화에 의존해야지, 구체화에 의존하면 안된다. 의존성 주입은 이 원칙을 따르는 방법 중 하나다.
- 프로그래머는 인터페이스의 구현객체에 대해서 전혀 알지 못하더라도 인터페이스만 가지고 개발이 가능해야 한다.
  - ex) MemberRepository m; // 해당 코드만 가지고도 정상적인 호출이 가능해야 한다.
- 순수 Java 로는 인터페이스를 구현한 구현 객체를 지정하지 않으면 실행이 불가하기에 위의 예제를 지킬 수 없다.
- "Spring DI Container 가 이를 보완해 줄 수 있다."

------

다형성을 예시로 이해하자면...
- ![image](https://user-images.githubusercontent.com/23256138/145704389-37f93995-7448-4497-bcae-6a69d5a1912c.png){:height="40%" width="40%"}
- ![image](https://user-images.githubusercontent.com/23256138/145704394-6ec8bcdd-565a-4ae3-a445-c8d401bad952.png){:height="40%" width="40%"}

------

고찰
- 모든 설계에 역할(interface)과 구현(class)을 분리하자.
- App. 설계는 역할 중심적이어야 하며, 구현 객체는 언제든지 유연하게 변경할 수 있도록 만드는 것이 좋은 객체지향 설계다.
- 이상적으로는 모든 설계에 인터페이스를 부여해야 한다.

실무 고민
- 인터페이스를 도입하면 추상화라는 비용이 발생한다. 개발자는 추가 요구사항 개발을 위해 인터페이스와 구현 객체를 전부 수정해야 하며, 해당 구현을 들여다 보기 위해 인터페이스와 구현 객체를 함께 봐야 하는 1 Depth 의 작업이 더 필요하다.
  - ex) MemberService -> MemberServiceImpl // 간단한 기능 구현과 분석에 있어어도, 2개의 객체를 다 들여다봐야 한다
- 기능을 확장할 가능성이 없다면, 구현 객체를 직접 호출하고, 향후 확장이 필요할 때 인터페이스를 도입하도록 리팩토링 하는 것도 방법이다.
- 개발 스펙이 고정된 작은 사이즈의 프로젝트에서는 위의 방법이 적절하다고 개인적으로 생각한다.

-----------

참고

- https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8
