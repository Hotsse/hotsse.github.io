---
layout: post
title: Spring Bean 관리(XML)
categories: [Spring]
comments: true
---

Bean

Spring 에서의 Bean 이란 자주 사용하는 클래스에 대한 객체를 Singleton의 형태로 생성하여, Component 화 시킨 것이다.
이렇게 생성된 Bean 은 Spring Framework 가 직접 관리하게 되며(이를 IoC 라고 함), 개발자는 필요할때마다 이 Bean을 호출하여 사용할 수 있다.

-------------

Bean 선언의 3가지 방법

Spring에서 Bean을 선언하는 방법은 크게 3가지를 지원하고 있다.
GenericApplicationContext을 선언 방법에 따라서 다르게 상속하여 사용된다.

- GenericXmlApplicationContext : **XML 기반** Bean 객체 생성 정보 호출
- AnnotationConfigApplicationContext : **Java Annotation 기반** Bean 객체 생성 정보 호출 (일반적으로 사용)
- GenericGroovyApplicationContext : Groovy Code 기반 Bean 객체 생성 정보 호출

-------------

XML 메타 정보를 통한 Bean 객체 선언 방법

이번 게시물에서는 GenericXmlApplicationContext 기반으로 XML 메타 정보를 통해 Bean 을 선언하고, 이를 접근하는 방법에 대해 알아보자.

-------------

예제 클래스 생성

Bean 객체로 선언할 예제 클래스를 생성한다. 간단한 예제를 아래의 코드로 구현해 보았다.

{% highlight java %}
public class BeanExample {

    private String format;

    public String print(String text){
        return String.format(format, text);
    }

    public void setFormat(String format){
        this.format = format;
    }

}
{% endhighlight %}

-------------

applicationContext.xml 생성 및 테스트

XML 기반의 Bean 선언은 <bean> 태그를 통해 가능하다. 태그 내 요소는 다음과 같다.
- id : 해당 Bean의 이름
- class : 해당 Bean을 생성할 모체 클래스
- property : Bean에 전달할 파라미터들

{% highlight xml %}
<?xml version="1.0" encoding="UTF-8">

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="beanEx" class="hotsse.BeanExample">
        <property name="format" value="print test : %s" />
    </bean>

</beans>
{% endhighlight %}

XML 을 생성한 뒤에는 다음과 같이 어플리케이션 실행 시에 만들어진 xml 파일을 기반으로 GenericXmlApplicationContext 를 선언하여 컨텍스트를 생성하고, 컨텍스트 안에 있는 bean을 getBean() 메서드를 통해 접근하여 사용할 수 있다. 

{% highlight java %}
import org.psringframework.context.support.GenericXmlApplicationContext;

public class Main {
    public static void main(String[] args){
        GenericXmlApplicationContext ctx = 
            new GenericXmlApplicationContext("classpath:applicationContext.xml");

        BeanExample be = ctx.getBean("beanEx", BeanExample.class);

        String msg = be.print("테스트 내용");
        System.out.println(msg);

        ctx.close();
    }
}
{% endhighlight %}
