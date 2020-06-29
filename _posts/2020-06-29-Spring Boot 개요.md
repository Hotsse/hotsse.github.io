---
layout: post
title: Spring Boot 개요
categories: [Spring]
comments: true
---

Spring Boot

스프링 프레임워크 개발팀은 전통적인 J2EE 웹 개발을 "겨울(Winter)" 라고 생각하고, 겨울의 종료와 함께 새로운 시작을 알리는 의미에서 프로젝트명을 "봄(**Spring**)" 이라 이름지었다.

-------------

Spring Boot 란?

Spring Boot 는 필요한 환경설정을 최소화하고 개발자가 비즈니스 로직에 집중할 수 있도록 도와주는 Spring Framework 의 서브 프레임워크이다.
(공식 홈페이지의 첫 문장에서 볼 수 있듯이, Spring Boot 는 그저 "just run" 만 하면 된다는 걸 강조하고 있다)

-------------

"Spring Boot" is "Spring"

Spring Boot 는 개발자가 비즈니스 로직 개발에 더욱 집중할 수 있도록 개발자와 Framework 의 사이에서 초기 설정과 버전, 의존성 관리를 보조 해주는 장치일 뿐, 실제로는 Spring 과 같다.

-------------

Spring Boot 의 장점

- 환경설정의 자동화, 간소화 제공
- Dependency 관리 자동화 제공
- Embed Server 제공

Spring Boot 의 단점

- 자동화, 간소화된 환경으로 인해 커스텀 설정이 많은 서비스에는 다소 부적합

-------------

Spring vs. Spring Boot

|구분|Spring|Spring Boot|
|------------------------------|------------------------------|
|환경설정|강력한 기능 지원! 그러나 이를 사용하기 위한 복잡하고 반복적인 설정 작업.<br>환경설정의 기본 제공이 없어 최소한의 서비스를 개발하려고 해도 다양한 설정들을 직접 지정해야 함.<br>-> 각 기업(커뮤니티) 별로 미리 정해놓은 커스텀 설정이 존재하는 경우도 있으나, 그렇지 않은 경우는 구글링을 통해 일일이 설정을 진행 해야함.<br><br>Spring 2.5 미만 : XML 기반<br>Spring 2.5 이상 : Java Code, Annotation 기반|기본 설정 제공! 커스텀이 필요한 설정만 부분적으로 수정.<br>@SpringBootApplication 어노테이션 하나로 서비스 개발을 위한 초기 기본 설정이 대부분 완료됨. (이를 Auto Configuration 이라 함)<br>-> 서비스 별로 커스텀 설정을 위해서는 프레임워크에서 상속받은 자바 메서드를 오버라이드하여 재구성 할 수 있음|
|Dependency 관리|앱을 위한 의존성과 버전 관리를 개발자 주도적으로 진행함.<br>-> 신규 의존성 추가나 버전 업그레이드 시 호환성 이슈가 있을 수 있음|앱을 위한 의존성과 버전 관리를 프레임워크 주도적으로 진행함 (수동 버전 관리 지양)<br>-> 프레임워크 레벨에서 안정화 된 의존관계와 버전으로 자동 설정<br>-> 초기 구축 및 추후 서비스 버전 업그레이드에 용이함|
|서버 관리|Tomcat, Jetty 등 현재 Spring 의 호환성에 맞는 서버 버전을 직접 선택 해야함(개발자 주도적 버전 관리)|프레임워크 내에 서버가 내장(Embed Tomcat) 되어 있으며 Spring Boot 버전 별로 서버 자동 제공(프레임워크 주도적 버전 관리)|
|MVC 로직 개발|@Controller, @Service, @Repository, @Component 등 어노테이션 기반으로 Bean 을 등록하여 프레임워크 내에서 Bean 을 관리하고 느슨한 결합(Decuppling) 을 통해 유연한 서비스 제공|Spring 과 동일|

-------------

주요 Dependency 버전 호환 정보 (2.2.5 기준)

Spring boot 는 버전 별로 최적화된 Dependency 버전들을 관리하고 있다. Dependency 추가 시 버전 정보를 기입하지 않는다면, 자동적으로 아래 리스트에 따른 최적 호환 버전으로 설정된다. (리스트에 존재하지 않는 Custom Dependency 의 경우는 필히 버전을 지정해줘야 한다)  
{% highlight http %}
https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/html/appendix-dependency-versions.html#appendix-dependency-versions
{% endhighlight %}