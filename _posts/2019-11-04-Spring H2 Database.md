---
layout: post
title: Spring H2 Database
categories: [Spring]
comments: true
---

H2 Database

H2(Hypersonic 2)는 Java로 작성된 관계형 데이터베이스 관리 시스템이다. H2의 특징은 다음과 같다.
- 초경량화 : 약 2MB의 jar 파일
- 인메모리 DB, 디스크 DB 전부 호환
- 브라우저에서 접속 가능한 콘솔 제공
- JDBC API 기반의 빠른 속도
- 오픈소스(무료)

위와 같은 특지이들로 인해 어플리케이션 개발 단계의 테스트 DB로써 자주 사용된다. DB 환경이 제대로 갖춰지지 않은 신규 서비스 같은 경우에 H2를 사용하면 좋을 것으로 보인다.

-------------

H2 설치 (Dependency 추가)

Spring Boot 의 경우, 공식적으로 지원하는 라이브러리이기에 https://start.spring.io 를 통해 프로젝트를 생성할 시 해당 옵션을 선택하는 것으로 프로젝트에 Dependency를 포함시킬 수 있다.

하지만 그렇지 않은 프로젝트의 경우는 수동으로 Dependency 를 추가해야 한다. 해당 프로젝트의 pom.xml 에 아래의 Dependency 를 추가하자.
(해당 게시글에서는 Spring Boot + MyBatis + H2 의 구조로 연동시킬 계획이기에 Mybatis 에 대한 Dependency 도 추가할 수 있도록 한다)

{% highlight xml %}
<!-- H2 -->
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>runtime</scope>
</dependency>
<!-- MyBatis -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.1</version>
</dependency>
{% endhighlight %}

기본적인 설치는 Dependency 를 추가하고 Maven Build 하는 것으로 완료된다.

-------------

H2 설정

Dependency 가 추가되었다면, application.yml 을 통해 기본 설정을 해야 한다.

{% highlight yaml %}
spring:
  h2:
    console:
      enabled: true
      path: /test_db # 브라우저를 통한 콘솔 접근 URL
  datasource:
    driver-class-name: org.h2.Driver
    jdbc-url: jdbc:h2:file:./h2/test_db;DB_CLOSE_DELAY=-1;DB_CLOSE_ON_EXIT=FALSE # DB 경로 및 옵션
    username: test # ROOT ID
    password: 1234 # ROOT PW
{% endhighlight %}

상기 설정 중 상황에 맞게 수정해야 하는 옵션은 다음과 같다.
- spring.h2.console.path = 콘솔 접속을 위한 URL
- spring.h2.datasource.jdbc-url(url) = 인메모리, 디스크 방식 여부를 결정하며, db 파일 저장 경로 및 기타 옵션을 지정
- spring.h2.datasource.username = Root ID
- spring.h2datasource.password = Root PW

-------------

H2 콘솔 접속

상기 설정을 바탕으로 H2 의 콘솔에 접속하기 위해서는 브라우저에서 설정에 기반한 URL 을 작성하면 된다.

{% highlight http %}
//your.domain.com/test_db
{% endhighlight %}

