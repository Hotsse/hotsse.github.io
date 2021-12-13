---
layout: post
title: Tiny Spring App.
categories: [Spring]
comments: true
---

가장 작은 Spring App. 만들기

``` xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.hotsse</groupId>
  <artifactId>simple_di</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <name>SIMPLE_DI</name>
  
  <properties>
    <!-- Java -->
    <java-version>11</java-version>
		
    <!-- Spring  -->
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

``` java
package simple_di.simple.test;

public class TestRepository {

	public String test() {
		return "test123";
	}
}
```

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
