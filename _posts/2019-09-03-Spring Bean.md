---
layout: post
title: Spring Bean 관리
categories: [Spring]
comments: true
---

Bean

-------------

GenericXmlApplicationContext : XML 기반 Bean 객체 생성 정보 호출
AnnotationConfigApplicationContext : Java Annotation 기반 Bean 객체 생성 정보 호출
GenericGroovyApplicationContext : Groovy Code 기반 Bean 객체 생성 정보 호출

{% highlight java %}
GenericXmlApplicationContext ctx = 
    new GenericXmlApplicationContext("classpath:applicationContext.xml");

BeanExample be = ctx.getBean("beanEx", BeanExample.class);
{% endhighlight%}

-------------

-------------

-------------

References

https://takeuu.tistory.com/88?category=737612 [워너비스페셜]