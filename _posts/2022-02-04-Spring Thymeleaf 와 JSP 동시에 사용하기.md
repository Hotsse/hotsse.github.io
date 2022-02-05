---
layout: post
title: Thymeleaf 와 JSP 동시에 사용하기
categories: [Spring]
comments: true
---

시작하며...

나는 Spring 3 + JSP 조합을 사용하고 있는 레거시 프로젝트를 리뉴얼 하고자 한다. 운영 기간이 10년이 넘는 레거시인 만큼 신규 스펙을 적용한 새 프로젝트를 생성하는 빅뱅 방식으로 리뉴얼 하기는 어려워 보였다.

그래서 Spring Boot 2 + JSP 를 war packaging 방식으로 적용하되, Spring Boot 는 JSP 를 공식적으로 지원하지 않으므로 점진적으로 Thymeleaf 를 적용하여 모든 이관이 완료 되면 jar packaging 으로 전환하고자 한다. 이렇게 되기 위해선 하나의 프로젝트 안에서 Thymeleaf 와 JSP 를 동시에 사용할 수 있어야 한다.

이러한 이유에서 Thymeleaf 와 JSP 를 하나의 프로젝트에서 동시에 사용하는 설정에 대해 기술한다.

------------

프로젝트 설정

application configuration 을 아래와 같이 설정한다. (YAML 을 사용하고 있다)

``` yaml
spring:
  mvc:
    view:
      prefix: /WEB-INF/jsp/	# JSP 파일이 위치할 기본 위치 설정
      suffix: .jsp		# JSP 확장자 설정
  thymeleaf:
    prefix: resources/		# Thymeleaf html 파일이 위치할 기본 위치 설정
    suffix: .html		# Thymeleaf html 확장자 설정
    view-names: th/*		# Thymeleaf 에 연결될 viewname 범위 설정 (중요)
```

사실 위 설정에서 다른 부분은 사족이고 spring.thymeleaf.view-names 설정이 핵심이다.

Controller 에서 View 를 맵핑하기 위해 viewname 을 작성하는데 위의 설정대로 본다면 "th/" 으로 시작하는 viewname 을 가진 맵핑은 Thymeleaf 로 연결되고, 이외의 맵핑은 JSP 로 연결된다.

------------

테스트

``` java
// ...
@GetMapping(value = "/jsp")
public String jsp() {
	return "main";
}

@GetMapping(value = "/th")
public String th() {
	return "th/main";
}
// ...
```

위 코드를 실행하면 아래와 같은 시나리오로 작동한다.
- URI /jsp : webapp/WEB-INF/jsp/main.jsp 파일을 JSP ViewTemplate 으로 맵핑한다.
- URI /th : resources/th/main.html 파일을 Thymeleaf ViewTemplate 으로 맵핑한다.

위와 같이 viewname에 prefix 를 설정하는 방법으로 한 프로젝트 내에서 Thymeleaf 와 JSP 를 동시에 사용할 수 있다.
