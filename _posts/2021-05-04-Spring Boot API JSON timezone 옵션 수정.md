---
layout: post
title: Spring Boot API JSON timezone 옵션 수정
categories: [Spring]
comments: true
---

웹 서버의 timezone 을 대한민국에 맞게 수정(UTC+0900) 하고도, 이 웹앱의 API를 통해 시간 정보를 받는 경우 timezone 설정이 제대로 적용되지 않는 케이스가 있다.  
Spring Boot 의 경우, Controller 에서 POJO 형태로 데이터를 반환하는 경우 jackson 라이브러리를 통해 자동적으로 json 변환을 진행한다.
POJO 내 Date 타입의 데이터가 있는 경우 이 jackson 에도 timezone 설정이 있어서 웹 서버의 timezone 설정을 덮어쓰는 경우가 있다. 

-------------

API 제공시에도 Date 값이 계획한 timezone 으로 출력될 수 있도록 jackson 의 옵션값을 설정해 주자. application configuration 값을 수정하면 되며, 하기의 설정은 YAML 기반이다.

- application.yml
{% highlight yaml %}
spring:
  jackson:
    time-zone: Asia/Seoul
{% endhighlight %}
