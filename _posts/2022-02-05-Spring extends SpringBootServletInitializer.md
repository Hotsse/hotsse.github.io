---
layout: post
title: extends SpringBootServletInitializer
categories: [Spring]
comments: true
---

packaging Spring Boot to war

Spring Boot Starter 를 통해 프로젝트를 구성할 때, war packaging 을 선택하고 생성하는 경우, java root 에 @SpringBootApplication 을 포함하는 메인 클래스 이외에 하나의 클래스가 더 생성되는 것을 확인할 수 있다.

``` java
public class ServletInitializer extends SpringBootServletInitializer {

	@Override
	protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
		return application.sources(DemoApplication.class);
	}

}
```

해당 클래스와 상속받고 있는 SpringBootServletInitializer 의 역할은 무엇인지 알아보자.

------------

SpringBootServletInitializer

전통적인 Spring Web App. 구성 시 외부 Tomcat 위에서 동작하도록 하기 위해서는 web.xml (Deployment Descriptor, DD)에 Application Context 를 등록해야만 한다. 이는 Apache Tomcat(Servlet Container) 이 구동될 때 /WEB-INF 디렉토리에 존재하는 web.xml 안의 설정을 읽어 웹 애플리케이션을 구성하기 때문이다.

하지만 Servlet 3.0 스펙으로 업데이트 되면서 web.xml 이 아닌 다른 방식으로도 서블릿 구동이 가능해졌다. 이는 web.xml 설정을 Spring Framework 에서 제공하는 WebApplicationInitializer 인터페이스를 구현하여 대신할 수 있게 됐고, Java Config 기반으로 ServletContext 에 Spring IoC 컨테이너(AnnotationConfigWebApplicationContext) 를 생성하여 추가할 수 있도록 변경됐기 때문이다.

Spring Boot 는 위에서 설명한 WebApplicationInitializer 를 구현한 SpringBootServletInitializer 를 미리 구성했으며, 이를 상속받는 것으로 web.xml 설정을 대신할 수 있게 된다.

------------

java -jar demo-app.war

Spring Boot 는 jar packaging 을 기본으로 하고 있지만, JSP 를 사용해야 한다던지 등의 이유로 war packaging 을 사용해야 하는 경우가 있다. 다만 jar 와 동일하게 embed Tomcat 을 사용하는 방식으로 실행하고자 한다면 war 도 jar 와 동일한 명령어로 실행할 수 있다.

``` bash
java -jar demo-app.war
```
