---
layout: post
title: Tiny Spring App.
categories: [Spring]
comments: true
---

가장 작은 Spring App. 만들기

spring-context 만을 사용하여 아주 간단한 기능만 지원하는 작은 Spring App. 을 만들어본다.  
프로젝트의 스펙은 아래와 같다
- Maven Project
- Spring Framework 5.3.13 (only spring-context)
- Java 11

------------

pom.xml
- Maven 을 통해 spring.context 라이브러리만 설치한다
- 그러나 Library Dependency 에 의해 5개 정도의 라이브러리가 추가로 설치된다 (하지만 spring-context 만 사용한다)

``` xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.hotsse</groupId>
  <artifactId>simple_di</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <name>SIMPLE_DI</name>
  
  <properties>
    <java-version>11</java-version>
    <org.springframework-version>5.3.13</org.springframework-version>
  </properties>
  
  <dependencies>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${org.springframework-version}</version>
    </dependency>    
  </dependencies>
</project>
```

TestRepository.java

``` java
package simple_di.simple.test;

public class TestRepository {

	public String test() {
		return "test123";
	}
}
```

TestService.java

``` java
package simple_di.simple.test;

public class TestService {

	private final TestRepository testRepository;
	
	public TestService(TestRepository testRepository) {
		this.testRepository = testRepository;
	}
	
	public String test() {
		return testRepository.test();
	}
}
```

AppConfig.java
- @Configuration 과 @Bean 을 통해 위에서 작성한 클래스들을 Bean 객체로 생성한다
- @Configuration 가 선언된 클래스 안에서 @Bean 으로 작성한 객체는 Spring DI Container에 의해 무조건 싱글톤임을 보장받는다.

``` java
package simple_di.simple;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import simple_di.simple.test.TestRepository;
import simple_di.simple.test.TestService;

@Configuration
public class AppConfig {

	@Bean
	public TestService testService() {
		return new TestService(testRepository());
	}
	
	@Bean
	public TestRepository testRepository() {
		return new TestRepository();
	}
}
```

Main.java
- 위에서 구성된 구현 객체를 통해 테스트를 진행한다

``` java
package simple_di.simple;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import simple_di.simple.test.TestService;

public class Main {

	public static void main(String[] args) {
		
		ApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
		
		TestService testService = ac.getBean(TestService.class);
		System.out.println(testService.test());
	}
}
```
