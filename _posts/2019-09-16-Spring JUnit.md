---
layout: post
title: Spring JUnit 
categories: [Spring]
comments: true
---

JUnit

JUnit(제이유닛)은 **Java 프로그래밍 언어용 유닛 테스트 프레임워크**이다. JUnit은 테스트 주도 개발 면에서 중요하며 SUnit과 함께 시작된 XUnit이라는 이름의 유닛 테스트 프레임워크 계열의 하나이다.

JUnit은 컴파일 타임에 jar로서 링크된다. 프레임워크는 JUnit 3.8 이하의 경우 junit.framework 패키지 밑에 상주하며, JUnit 4 이상의 경우 **org.junit** 패키지 밑에 상주한다.

JUnit은 산업 표준이라고 불릴 만큼 Java 프로젝트에 널리 사용되고 있으며, 그 증거로 2013년 GitHub에 호스팅된 Java 프로젝트 중 30% 이상이 JUnit 라이브러리를 포함하고 있다고 한다.

(해당 게시글은 JUnit4 기준으로 작성하도록 하겠다)

-------------

JUnit 라이브러리 추가

JUnit 을 사용하기 위해 Maven 기반 Spring (or Boot) 프로젝트에 JUnit 라이브러리 의존성을 추가해야 한다.

Spring 의 경우 junit 라이브러리를 pom.xml 를 통해 Dependency를 추가하도록 한다.

{% highlight xml %}
<!-- Spring > pom.xml -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.7</version> <!--의존성에 적합한 버전으로 수정-->
    <scope>test</scope>
</dependency>
{% endhighlight %}

Spring Boot 의 경우는 spring-boot-starter-test 패키지에 JUnit을 포함한 다양한 라이브러리들이 포함되어 있어, 해당 패키지에 대한 Dependency 만 추가해 주면 된다.

{% highlight xml %}
<!-- Spring Boot > pom.xml -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
{% endhighlight %}

-------------

단위 테스트 기본 구성

Maven 프로젝트를 생성하게 되면 Maven 의 관례에 따라 ~/src/test/java 구조의 패키지가 생성된다. 개발자는 단위 테스트를 위해 앞서 언급한 패키지 내에 java 코드를 생성하고 구동하는 것으로 단위 테스트가 가능하다.

@RunWith(SpringJUnit4ClassRunner.class) 
@ContextConfiguration(locations={"file:WebContent/WEB-INF/classes/applicationContext*.xml"})

@RunWith(SpringRunner.class)
@SpringBootTest

-------------

@Test

-------------

Assert