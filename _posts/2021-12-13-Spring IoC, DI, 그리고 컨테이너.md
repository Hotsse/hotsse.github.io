---
layout: post
title: IoC, DI, 그리고 컨테이너
categories: [Spring]
comments: true
---

IoC, DI, 그리고 컨테이너

제어의 역전 IoC(Inversion of Control)
- 기존 프로그램은 클라이언트 구현 객체가 스스로 필요한 서버 구현 객체를 생성, 연결, 실행했다. 한마디로 구현 객체가 프로그램의 제어 흐름을 스스로 조종했다.
- 반면 Spring 에서는 구현 객체는 자신의 로직을 실행하는 역할만 담당한다. 프로그램의 제어 흐름은 Spring DI Container 가 가져간다.
- 프로그램의 제어 흐름을 (개발자가) 직접 제어하는 것이 아니라 외부(Spring)에서 관리하는 것을 제어의 역전(IoC, Inversion of Control) 이라 한다.

의존관계 주입 DI(Dependency Injection)
- 정적인 클래스 의존관계 : 코드에서 확인할 수 있는 정적인 호출 의존관계
- 동적인 객체 인스턴스 의존관계 : App. 실행 시점에 실제 생성된 객체 인스턴스의 참조가 연결된 의존 관계
- 의존관계 주입을 사용하면 클라이언트 코드를 변경하지 않고, 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있다
- 의존관계 주입을 사용하면 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있다

DI 컨테이너
- Bean 을 생성하여 라이프사이클을 관리하고 Bean 끼리 의존관계를 자동으로 연결해 준다
- IoC 컨테이너, 어셈블러, 오브젝트 팩토리 등으로도 불린다

-----------

참고

- https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8
- https://spring.io
