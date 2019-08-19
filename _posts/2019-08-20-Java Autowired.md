---
layout: post
title: Spring @Resource, @Autowired, @Qualifier
categories: [Spring]
comments: true
---

Spring 에서는 Bean을 주입할 수 있다. Spring의 DI(Dependency Injection, 의존성 주입)이라는 특징을 이용해 xml 설정 파일로도 가능하고, (일반적으로) 어노테이션을 이용할 수도 있다.
최근에는 주로 어노테이션을 사용하는 경우가 많다.

------------

@Resouse

@Resouse 는 주입할 Bean을 객체에 주어진 이름을 기반으로 찾아서 주입 시키는 어노테이션이다.

{% highlight java %}
public class Hello {

    @Resource(name="printer")
    private Printer printer; // bean 중 "printer" 라는 객체를 주입

}
{% endhighlight %}

--------------

@Autowired

@Autowired 는 앞서 설명한 @Resource 와는 달리 이름이 아닌 타입에 의해 종속성이 주입되는 기능을 가진 어노테이션이다.

{% highlight java %}
public class Hello {

    @Autowired
    private Printer printer; // bean 중 Printer형을 가진 객체를 주입

}
{% endhighlight %}


중복된 타입의 Bean이 있는 경우

@Autowired는 타입 기반으로 Bean을 검색하므로, 동일한 타입의 객체가 2개 이상 존재한다면 정상적인 기능을 하기 어렵다.
이를 해결하기 위한 방법은 아래와 같다.

- 배열을 사용
{% highlight java %}
@Autowired
private Printer[] printers; // bean 중 Printer형을 가진 모든 객체를 주입
{% endhighlight %}

- @Qualifier
@Autowired 에 타입 말고도 추가적인 정보를 제공하여, 특정 bean을 검색할 수 있도록 하는 어노테이션이다.

{% highlight java %}
@Component
@Qualifier("dev")
public class Printer {
    //...
}
{% endhighlight %}

{% highlight java %}
public class Hello {

    @Autowired
    @Qualifier("dev") // qualifier 에 "dev" 라는 값이 충족 되어야함
    private Printer printer; // bean 중 Printer형을 가진 객체 주입
}
{% endhighlight %}