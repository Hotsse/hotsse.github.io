---
layout: post
title: J2EE 와 EJB, 그리고 Spring
categories: [Java,Spring]
comments: true
---

J2EE (Java To Enterprise Edition) = Java EE -> 그리고 Jakarta EE
- Java 기술로 기업 환경의 App. 을 만드는데 필요한 스펙들을 모아둔 스펙의 집합체
- JVM 자체가 이식성이 뛰어나기에 플랫폼에서 자유롭다는 장점이 있음
- JDK 1.5 이후부터 Java EE (Java Enterprise Edtion) 으로 명칭이 변경되었음
- J2EE 의 주요 구성요소는 아래와 같다
	- Servlet
	- EJB
	- JSP
	- RMI
	- JNDI
	- JDBC
	- JCA
	- JMS

(추가)
- Sun 을 인수한 Oracle 이 2017년까지 Java EE 를 8 까지 이끌어왔지만 수익화 실패 등의 이유로 기술 주도권을 비영리 단체인 Eclipse 재단으로 이관함
- Eclipse 재단은 Java EE -> Jakarta EE 로 명칭을 변경 했으며, 상표권 등의 이유로 패키지명도 javax.* -> Jakarta.* 로 변경되었음 (히스토리 없이 버전을 옮긴 개발자들에게 엄청난 혼란을 야기할 것으로 보임)
- (글 작성 기준 최신 버전인) Spring Framework 5.3.x 기준으로 내부 기술로는 아직 Java EE 8 을 사용하고 있음 (Spring 5 가 2017년에 나왔기 때문) 
- ![image](https://user-images.githubusercontent.com/23256138/145703993-8dc870e5-7010-40ac-9512-59efe446ec18.png){:height="50%" width="50%"}

(추가2)
- 2021년 9월 발표에 의하면, Spring Framework 6 및 Spring Boot 3 이 2022년도 4분기에 Release 될 예정임
- 해당 버전의 스펙은 아래와 같음
	- Java 17+ (Java 버전을 올려야 할 이유가 하나 더 늘었다)
	- Jakarta EE9+ (Spring  은 더 이상 Java EE 에 종속되지 않는다)

(추가3)
- 아니 대체 왜 이름이 쌩뚱맞은 Jakarta 인가 싶어서 찾아봤더니...
- 섬국가인 인도네시아의 수도인 자카르타(Jakarta)는 자바(Java) 라는 지명을 가진 섬에 위치하고 있다고 한다.
- Java 섬 안에 있는 도시 Jakarta...

EJB (Enterprise JavaBeans)
- Java 기반 기업환경의 시스템을 구현하기 위한 서버측 컴포넌트 모델로 Java EE 의 API 중 하나이다
- 과거 표준으로는 서버 비즈니스 로직은 EJB, 화면 로직은 JSP 가 담당하는 것으로 웹 통신 기반을 구축했다
- EJB 는 다음과 같이 구성 되어 있다
	- Session Bean : 주요 비즈니스 로직 및 웹 통신을 담당하는 객체
	- Entity Bean : DB 와 데이터를 관리하는 객체
	- Message-driven Bean : JMS 로 Bean Message 를 날려줌

-----------

그래서 Spring 이 나오게 되는데...
- Spring 이전에는 EJB 가 Java 생태계를 지배하고 있었다.
- 그러나 현업 개발자들로 하여금 EJB로 Enterprise 급 App. 을 개발하는 것은 너무 고된 일이었다.
- 단순함을 강조한 Spring 이 도입됨으로써 얼어붙은 Java EE 의 생태계의 겨울(EJB) 이 지나고 진정한 봄(Spring) 이 온 것이다. (실제로 이런 의미로 이름이 붙여졌다고 한다)

-----------

참고

- https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8
- https://choichumji.tistory.com/133
- https://ko.wikipedia.org/wiki/%EC%97%94%ED%84%B0%ED%94%84%EB%9D%BC%EC%9D%B4%EC%A6%88_%EC%9E%90%EB%B0%94%EB%B9%88%EC%A6%88
- https://www.s-core.co.kr/insight/view/java-ee%EC%97%90%EC%84%9C-jakarta-ee%EB%A1%9C%EC%9D%98-%EC%A0%84%ED%99%98/
- https://spring.io/blog/2021/09/02/a-java-17-and-jakarta-ee-9-baseline-for-spring-framework-6
