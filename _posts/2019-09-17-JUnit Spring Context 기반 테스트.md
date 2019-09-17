---
layout: post
title: JUnit Spring Context 기반 테스트
categories: [JUnit]
comments: true
---

Spring Framework Context 기반의 JUnit 테스트

-------------

Spring (XML 기반 Context)

@RunWith
- JUnit 의 테스트 실행방법을 확장할 때 사용
- SpringJUnit4ClassRunner.class, SpringRunner.class 의 지정으로 테스트 중 Spring ApplicationContext 를 생성하고 관리하는 작업을 진행함

Spring 의 경우는 @RunWith 와 @ContextConfiguration 어노테이션으로 구현 가능하다.

{% highlight java %}
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations={"file:WebContent/WEB-INF/classes/applicationContext*.xml"})
public class SpringJUnitTest {

    @Test
    public void test(){
        //...
    }
}
{% endhighlight %}

-------------

Spring(Class Annotation 기반 Context)  
or Spring Boot(1.3 버전 이하)

Spring 4.x 부터는 어노테이션으로 설정된 Class 기반 Context 를 호출하는 것도 가능하다.
(Spring Boot 1.3 까지는 이 방식을 사용한다)

{% highlight java %}
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(ConextConfig.class)
public class SpringJUnitTest {

    @Test
    public void test(){
        //...
    }
}
{% endhighlight %}

-------------

Spring Boot(1.4 버전 이상)

Spring Boot 1.4 이상에서는 @RunWith 와 @SpringBootTest 어노테이션으로 구현 가능하다.

{% highlight java %}
@RunWith(SpringRunner.class)
@SpringBootTest
public class BootJUnitTest {
    
    @Test
    public void test(){
        //...
    }
}
{% endhighlight %}