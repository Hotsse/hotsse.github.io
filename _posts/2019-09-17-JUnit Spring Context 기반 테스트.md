---
layout: post
title: JUnit Spring Context 기반 테스트
categories: [JUnit]
comments: true
---

Spring Framework Context 기반의 JUnit 테스트

Spring (혹은 Boot) 의 Context 와 연동되는 기능을 테스트 하려면, 테스트 진행 시 해당 Context 를 띄우는 작업이 필요하다.  
JUnit 은 간단한 어노테이션 선언만으로 해당 프로젝트의 Context 를 띄우고 그와 연동된 단위 테스트를 테스트 하는 것이 가능하다.

-------------

Test Runner(테스트 러너)

JUnit의 Test Runner는 테스트 클래스 내에 존재하는 각각의 테스트 메서드 실행을 담당하고 있는 Class 이다. 이는 구조에 맞게 테스트 메서드들을 실행하고 결과를 표시하는 역할을 한다.

JUnit 은 기본적으로 **BlockJUnit4ClassRunner** 라는 Test Runner Class 를 사용한다.

-------------

Spring 환경에 맞는 Test Runner 변경

@RunWith 어노테이션은 앞서 설명한 기본으로 설정된 Test Runner 를 변경하기 위해 사용하는 어노테이션이다.
기본 Test Runner 는 Spring (혹은 Boot) 환경에서의 테스트를 지원하지 않기에 이를 Spring 환경에서 테스트 가능한 Test Runner 로 수정해 주어야 한다.

Spring Framework 에서 제공하는 Test Runner 를 사용하면 Spring 환경에서의 JUnit 테스트가 가능해진다.

{% highlight java %}
@RunWith(SpringRunner.class)
{% endhighlight %}

**SpringRunner.class**는 기존에 사용하던 **SpringJUnit4ClassRunner.class**의 가독성을 높인 별칭(Alias) 이자 확장 클래스이다.
낮은 버전의 Framework 에서는 SpringRunner.class 가 지원되지 않는 경우도 있다.

-------------

Spring 환경 테스트를 위한 Context 설정

Spring 테스트가 가능하도록 Test Runner 를 수정했으니, 테스트 실행 시 해당 프로젝트의 Spring Framework Context 가 동작할 수 있도록 설정 해야 한다. Context 설정 방법이나 Framework 에 따라 어노테이션이 다르나 역할은 동일하므로 필요한 상황에 맞는 어노테이션을 사용하면 된다.

모든 Spring 기반 테스트는 Test Runner 도 함께 변경되어야 하므로, 앞서 설명한 @RunWith 어노테이션도 함께 선언한다.

Spring (XML Context) 의 경우 : **@ContextConfiguration**

- **@ContextConfiguration** 은 Spring의 전통적인 XML 방식 Context 설정에 대한 테스트 Context 구성 어노테이션이다. 어노테이션의 옵션인 locations 에 해당 Context의 경로를 지정하면, 테스트 실행 시 해당 정보를 통해 Context 를 로드한다.

{% highlight java %}
@RunWith(SpringRunner.class)
@ContextConfiguration(locations={"file:WebContent/WEB-INF/classes/applicationContext*.xml"})
public class SpringJUnitTest {

    @Test
    public void test(){
        //...
    }
}
{% endhighlight %}

Spring (Annotation Context) 의 경우 : **@SpringApplicationConfiguration**

- **@SpringApplicationConfiguration** 은 Class, 어노테이션 기반 Context 설정에 대한 테스트 Context 구성 어노테이션이다. Spring 은 4.x 부터 Annotation 기반 Context 설정을 지원하고 있으므로, 해당 어노테이션 역시 Spring 4.x 부터 가능한 것으로 볼 수 있다.

{% highlight java %}
@RunWith(SpringRunner.class)
@SpringApplicationConfiguration(ConextConfig.class)
public class SpringJUnitTest {

    @Test
    public void test(){
        //...
    }
}
{% endhighlight %}

Spring Boot 의 경우 : **@SpringBootTest**

- **@SpringBootTest**는 Spring Boot 1.4 부터 지원되는 어노테이션이며, @SpringApplicationConfiguration 을 포함한 Spring Boot 에서 JUnit 을 사용하기 위한 다양한 기능들을 포함하고 있어 손쉽게 단위 테스트 환경을 구성할 수 있는 어노테이션이다.

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



