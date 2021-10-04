---
layout: post
title: Spring MyBatis LocalDate(Time) 으로 바인딩
categories: [Spring]
comments: true
---

LocalDate, LocalDateTime

Java 8 부터 지원하는 java.time 패키지의 라이브러리로써 Java 에서 지원하는 기존 라이브러리(Date, Calendar 등)의 단점을 보완했다. 이로써 기존 라이브러리는 빠른 속도로 java.time 패키지의 LocalDate, LocalDateTime 으로 대체되고 있다...

<a target="_blank" href="https://hotsse.github.io/articles/2021-05/Java-java.util.Date-vs.-java.time.">자세히...</a>

-----------

MyBatis 와 JSR310

MyBatis 를 통해서 날짜 자료형을 바인딩하기 위해서 과거에는 기본적으로는 java.util.Date 클래스를 사용해 왔다. 그러나 위에서 언급했다시피 Java 8 이후 java.time 패키지 라이브러리의 보급되었고 MyBatis 와 Java 사이의 부정교합이 있었다.

MyBatis 는 이를 해결하기 위해 Java 8 에서 Date and Time API 변경을 위해 제안한 JSR310 규격을 맞추기 위한 추가 라이브러리를 내놓게 된다.  
(Java 8 은 2014년 3월에 나온 반면, MyBatis 의 JSR310 호환 라이브러리는 2016년 8월에 나왔으니, 불편한 시간은 그리 짧지 않았다고 보여진다)

----------

MyBatis 에 LocalDate, LocalDateTime 바인딩 하기

MyBatis 에서 LocalDate 나 LocalDateTime 으로 날짜정보를 바인딩 하기 위해선 두 가지 방법이 있다.

첫번째, MyBatis 에서 제공하는 JSR310 호환 라이브러리의 종속성(Dependency) 추가한다.

{% highlight xml %}
<!-- Maven Project 로 간주한다. Gradle 이나 기타 Builder 를 사용하는 경우엔 해당하는 문법을 사용해서 종속성을 추가하자 -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-typehandlers-jsr310</artifactId>
    <version>1.0.2</version>
</dependency>
{% endhighlight %}

상기 종속성을 각 프로젝트의 pom.xml 에 추가하면 MyBatis 에 LocalDate 와 LocalDateTime 에 대한 바인딩이 정상적으로 이루어지는 것을 확인할 수 있다.

두번째, MyBatis 3.4.5 이상 버전을 사용한다.

MyBatis 에서 Java 생태계의 기준점이 된 Java 8 의 스펙을 따르지 않을 이유가 전혀 없기에, 2017년 8월 배포 버전인 3.4.5 버전부터는 추가 라이브러리로만 제공되던 JSR310 스펙에 대한 호환을 기본적으로 호환하고 있다.

{% highlight xml %}

<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.7</version> <!-- 포스팅 당시의 최신버전 -->
</dependency>
{% endhighlight %}

새로 프로젝트를 구축하는 경우엔 별도의 액션을 취하지 않아도 LocalDate 와 LocalDateTime 에 대한 바인딩이 정상적으로 이루어진다. 그러나 레거시 프로젝트를 운영하는 입장에서는 JSR310 호환 하나 때문에 라이브러리 버전을 불쑥 올려버린다는 것은 부담이 많이 될 수도 있다. 어떤 방법을 선택할지는 담당자의 마음이 되겠다.



-----------

참고

https://cchoimin.tistory.com/entry/myBatis-LocalDate-%EB%B0%94%EC%9D%B8%EB%94%A9%ED%95%98%EB%8A%94-%EB%B2%95
