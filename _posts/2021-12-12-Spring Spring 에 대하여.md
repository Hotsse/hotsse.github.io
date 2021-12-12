---
layout: post
title: Spring 에 대하여
categories: [Spring]
comments: true
---

Spring의 버전 별 특징
- 2003년 Spring Framework 1.0 출시 - XML
- 2006년 Spring Framework 2.0 출시 - XML 편의 기능 지원
- 2009년 Spring Framework 3.0 출시 - Java Config 설정 지원
- 2013년 Spring Framework 4.0 출시 - Java 8 지원
- 2014년 Spring Boot 1.0 출시
- 2017년 Spring Framework 5.0, Spring Boot 2.0 출시 - Reactive Programming 지원
- 2021년 12월 현재 Spring Framework 5.3.13(LTS), Spring Boot 2.6.1

-----------

Environments of Spring
- Spring Framework 는 아래와 같은 구성으로 이루어져 있다
	- Spring Framework
		- 핵심 기술 : Spring DI Container(이게 Spring 의 진짜 핵심이다), AOP, Event 등
		- 웹 기술 : Spring MVC, WebFlux
		- 데이터 접근 기술 : Transaction, JDBC, ORM, XML
		- 기술 통합 : 캐시, 이메일, 원격접근, 스케줄링(Quartz)
		- 테스트 : Spring 기반 테스트 지원
		- 언어 : Java, Kotlin, Groovy
	- Spring Boot : Spring Framework 의 추상화
	- Spring Data
	- Spring Session
	- Spring Security
	- Spring Rest Docs
	- Spring Batch
	- Spring Cloud

-----------

Spring 의 의미? 공식 홈페이지에서도 문맥에 따라 의미가 다르다고 언급했다.
- Spring DI Container (Bean LifeCycle Management)
- Spring Framework
- Spring Framework & Boot 를 포함한 모든 Spring Environments

Spring 의 진짜 핵심
- Spring 은 Java 언어 기반의 프레임워크
- Java 언어의 가장 큰 특징 - 객체 지향 언어
- Spring 은 좋은 객체 지향 애플리케이션을 개발할 수 있게 도와주는 프레임워크

Spring 과 Boot 는 다르지 않다
- Spring Boot 는 Spring Framework 를 간편하게 구축하고 운영하기 위해 구성 및 설정영역, 그리고 많은 라이브러리의 사용을 추상화해 주는 역할을 한다
- 결국 한꺼풀 벗겨놓고 보면 Spring Framework 라는 뜻이다.
- 최근에는 Boot를 기본으로 사용한다.
- Embed Tomcat, Starter Dependency, 3rd Party 자동 구성, 관례에 의한 간결한 설정 등을 제공한다.

-----------

참고

- https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8
- https://spring.io
