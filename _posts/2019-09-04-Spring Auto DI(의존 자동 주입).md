---
layout: post
title: Spring Auto DI(의존 자동 주입)
categories: [Spring]
comments: true
---

Spring 의 DI 설정(Annotation) - 의존 자동 주입

XML 방식은 모든 객체 간의 의존 관계를 Context.xml 에 기재해야 해서, Java Code의 비즈니스 로직과 분리되어 있어 관리가 복잡해 질 수 있다.
어노테이션은 Java Code 내에 **@예약어** 형태로 기입하는 것으로 해당 클래스의 의존 관계를 바로 지정할 수 있는 기능을 제공한다
(어노테이션에 대한 자세한 정보는 따로 정리하도록 하겠다)

--------------

@Autowired

@Autowired 는 생성된 Bean 객체 중에서 타입을 기반으로 가장 적합한 Bean 객체를 선정하여 자동으로 주입해 주는 기능을 한다.

우선 어떠한 의존 주입도 하지 않은 채 Bean 만 생성해 놓는다고 가정하자.


{% highlight xml %}
<!-- applicationContext.xml -->
<?xml version="1.0" encoding="UTF-8">

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="testDao" class="hotsse.TestDao">
    </bean>

    <bean id="testService" class="hotsse.TestService">
    </bean>
</beans>
{% endhighlight %}

아래와 같이 Java Code에 @Autowired 어노테이션을 사용하는 것만으로, Spring 이 자동으로 Bean 객체를 의존 주입한다.
@Autowired 는 Bean 의 타입 기반으로 적합한 객체를 선정한다.

{% highlight java %}
import org.springframework.beans.factory.annotation.Autowired;

public class TestService {

    @Autowired
    private TestDao testDao; // Bean 내에 있는 TestDao 형의 객체를 자동으로 주입

    public void test(){
        testDao.something();
    }
}
{% endhighlight %}

--------------

@Qualifier

앞서 설명한 바와 같이 @Autowired 는 타입 기반으로 Bean 을 검색한다. 그래서 같은 타입의 Bean 이 2개 이상 등록되어 있다면 다음과 같은 에러를 띄우게 된다.

{% highlight text %}
...BeanCreationException: Error creating bean with name '~~~': Injection of autowired dependencies failed;
{% endhighlight %}

@Qualifier 는 같은 타입의 Bean 객체에 대해서 임의의 한정자 값을 추가하여 의존 자동 주입 시에 유니크한 Bean 객체를 선정할 수 있게 한다.

{% highlight xml %}
<!-- applicationContext.xml -->
...
<beans ...>
    <bean id="testDao" class="hotsse.TestDao">
        <qualifier value="test1">
    </bean>

    <bean id="testDao2" class="hotsse.TestDao">
        <qualifier value="test2">
    </bean>

    <bean id="testService" class="hotsse.TestService">
    </bean>
</beans>
...
{% endhighlight %}

{% highlight java %}
/**
* TestService.java
*/
public class TestService {

    @Autowired
    @Qualifier("test2")
    private TestDao testDao; // testDao2 가 의존 주입됨

    public void test(){
        testDao.something();
    }
}
{% endhighlight %}

--------------

@Resource

@Resource 는 @Autowired 와는 달리 타입 기반이 아닌 id 기반으로 Bean 객체를 선정한다.

{% highlight java %}
/**
* TestService.java
*/
public class TestService {

    @Resource(name="testDao2")
    private TestDao testDao; // testDao2 가 의존 주입됨

    public void test(){
        testDao.something();
    }
}
{% endhighlight %}

