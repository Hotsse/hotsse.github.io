---
layout: post
title: Spring Boot Configuration 제어
categories: [Spring]
comments: true
---

프레임워크 별 Configuration 제어

Spring 과 Spring Boot 는 버전에 따라 Configuration 을 구성하는 방식이 다르다. Spring 은 전통적인 XML 기반의 비교적 가독성이 떨어지는 구성 방식을 탈피하였고, Spring Boot 는 자동설정 기능을 통해 빠른 개발환경 구성을 할 수 있게 진화하고 있다.

|구분|Spring 2.5 미만|Spring 2.5 이상|Spring Boot
|------------------------------|------------------------------|------------------------------|------------------------------|
|생성형식|XML|XML, 어노테이션|어노테이션|
|자동설정 여부|-|-|가능|

-------------

Spring 의 전통적인 Configuration 구조 생석 방식

Spring 의 전통적인 Configuration 파일의 생성법(XML 기반)은 다음과 같다.

{% highlight xml %}
<beans>
  <context:annotation-config />
  <mvc:default-servlet-handler/>

  <context:component-scan base-package="com.demo" use-default-filters="false">
    <context:include-filter type="annotation" expression="org.aspectj.lang.annotation.Aspect" />
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller" />
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Component" />
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Service" />
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Repository" />
  </context:component-scan>

  <bean class="org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping"/>
     
  <bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
    <property name="messageConverters">
      <list>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter" />
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
          <property name="supportedMediaTypes">
            <list>
              <value>text/html;charset=UTF-8</value>
            </list>
          </property>
        </bean>
      </list>
    </property>
  </bean>

  <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <property name="maxUploadSize" value="104857600" />
  </bean>
  <bean id="localeResolver" class="org.springframework.web.servlet.i18n.SessionLocaleResolver">
    <property name="defaultLocale" value=""/>
  </bean>

  <!-- ... -->
</beans>
{% endhighlight %}

보편적으로 오래된 레거시 프로젝트에 적용되는 경우가 많아 자잘한 설정이 많다는 점도 있겠지만, 기본적인 부분들까지도 일일이 설정해줘야 하는 번거로움이 있는 것은 사실이다.

-------------

Spring Boot 의 AutoConfiguration

Spring Boot 의 Configuration 구성 방식은 다음과 같다.

{% highlight java %}
package com.nexon.quicksample;
 
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
 
@SpringBootApplication
public class QuicksampleApplication {
 
    public static void main(String[] args) {
        SpringApplication.run(QuicksampleApplication.class, args);
    } 
}
{% endhighlight %}

**@SpringBootApplication** 어노테이션은 기존 Spring 서비스를 구동시키기 위한 모든 기본 설정을 지정해 놓은 어노테이션으로, 그의 내부는 아래 3개의 주요한 어노테이션 기능으로 구성되어 있다.

|어노테이션 명|설명|
|------------|-----|
|@SpringBootConfiguration|Spring Boot 의 고유 설정을 나타내는 어노테이션, Configuration Bean 의 자동검색을 가능하게 해주는 역할을 한다|
|@EnableAutoConfiguration|classpath 로 지정된 경로 내용을 기반으로 영리하게 설정 자동화를 수행하는 자동 설정의 핵심 어노테이션(중요*)|
|@ComponentScan|특정 패키지 경로를 기반으로 @Configuration 에서 사용할 @Component 설정 클래스를 찾는 어노테이션|

@SpringBootApplication 어노테이션 하나만으로 앞 절의 수많은 설정들 중 (거의) 절반 이상이 자동 설정된다. Spring Boot 는 표준이 되는 기본 설정은 굳이 명시하지 않아도 자동 설정된다는 뜻이다. (이를 Spring Boot 의 AutoConfiguration 이라 한다)

Spring Boot 에서 어떠한 기본 설정을 쓰고 있는지는 interface WebMvcConfigurer 의 구현체는 class WebMvcAutoConfigurationAdapter 를 살펴보면 알 수 있다.

-------------

Spring Boot 의 Configuration 구조 생성 방식

앞절의 내용대로, Spring Boot 는 대부분의 설정을 자동적으로 생성해 주지만, 모든 서비스마다 고유한 커스텀 설정은 필요하기에 아래의 방법으로 커스텀 설정을 제어할 수 있다.

{% highlight java %}
package com.nexon.quicksample.core.config;
 
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
 
@Configuration
public class ContextConfig implements WebMvcConfigurer {
 
    /*
    ... 커스텀하게 제어할 설정만 @Override 하여, 추가 제어한다
    */
     
}
{% endhighlight %}

우선 @Configuration 어노테이션으로 해당 클래스가 Configuration 설정을 위한 클래스 임을 선언한다. 이는 전통적인 구성 방식에서 <beans>...</beans> 형식의 *context.xml 파일을 생성하여 web.xml 에 추가하는 것과 같은 행위이다. 앞 절에서 설명한 @SpringBootApplication 어노테이션이 @Configuration 어노테이션이 선언된 클래스들을 자동으로 검색하여 Configuration 설정 파일처럼 인식하게 하는 역할을 한다.

두 번째로 WebMvcConfigurer 를 상속 받는다. 이는 커스텀 설정할 내용에 대한 메서드를 재정의 하기 위한 행동이다. 상속받은 메서드들 중 하나를 재정의(Override) 하는 것이, 곧 기본 설정을 고치는 행위라고 볼 수 있다.

interface WebMvcConfigurer 의 메서드 리스트를 살펴보면 어떠한 설정을 제어할 수 있는지(=어떠한 메서드를 Override 할 수 있는지) 확인할 수 있다