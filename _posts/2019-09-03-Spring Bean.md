---
layout: post
title: Spring Bean 관리
categories: [Spring]
comments: true
---

Bean

-------------

예제 클래스 생성

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