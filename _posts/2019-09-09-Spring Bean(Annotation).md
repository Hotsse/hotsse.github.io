---
layout: post
title: Spring Bean(Annotation)
categories: [Spring]
comments: true
---

Bean - Annotation

Bean 선언 방법 중 클래스에 어노테이션을 적용시켜 XML 없이 비즈니스 로직과 동일한 형태로 Bean을 선언하는 형태를 제공한다.

이는 ApplicationContext 클래스를 상속 받은 **AnnotationConfigApplicationContext** 를 기반으로 작동된다.
(AnnotationConfigApplicationContext : **Java Annotation 기반** Bean 객체 생성 정보 호출)

-------------

@Configuration, @Bean

해당 클래스가 Context의 Configuration을 정의하고 있다는 것을 알리기 위해, 클래스 위에 @Configuration 어노테이션을 선언해야 한다.

또한, 해당 클래스 내에서 Bean 객체를 선언하기 위해서 각 메소드를 선언하고 @Bean 어노테이션을 선언하여야 한다.
- 메서드명 : Bean ID
- 반환형 : Bean Class

{% highlight java %}
@Configuration
public class ContextConfig {

    @Bean
    public TestDao testDao() {
        return new TestDao();
    }

    @Bean
    public TestService testService(){
        return new TestService(testDao());
    }

}
{% endhighlight %}

-------------

어노테이션을 사용하여 선언한 Bean 객체를 획득하여 사용하는 것은 XML과 크게 다르지 않다.

{% highlight java %}
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class Main {
    public static void main(String[] args){
    	ApplicationContext ctx = 
            new AnnotationConfigApplicationContext(ContextConfig.class);

	TestService testService = ctx.getBean("testService");

	testService.test();

        ctx.close();
    }
}
{% endhighlight %}

-------------

@EnableWebMvc

Spring Web Application 실행 시, 앞서 설명한 AnnotationConfigApplicationContext 를 통해 컨텍스트를 생성하고 관리하는 것도 가능하나, Spring 에서는 어노테이션과 상속을 통해 이를 더 쉽게 생성하는 방식을 제공하고 있다.

@EnableWebMvc 는 @Configuration 어노테이션과 함께 사용되어, 해당 클래스 안의 Config 정보를 Spring MVC 실행 시에 등록시키는 기능을 제공한다.

{% highlight java %}
@Configuration
@EnableWebMvc
public class ContextConfig implements WebMvcConfigurer {

    @Bean
    public TestDao testDao() {
        return new TestDao();
    }
	
	//...

}
{% endhighlight %}

{% highlight java %}
public class TestService() {

	@Autowired
	private TestDao testDao; // TestDao 형을 가지고 있는 Bean 객체 선정

	public void test(){
		testDao.something(); // ...
	}
}
{% endhighlight %}
