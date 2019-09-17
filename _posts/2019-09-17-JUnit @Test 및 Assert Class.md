---
layout: post
title: JUnit @Test 및 Assert Class
categories: [JUnit]
comments: true
---

@Test

단위 테스트를 진행하기 위해선 테스트를 진행하고자 하는 메서드를 단위 테스트 메서드로 지정해야 한다. @Test 어노테이션은 특정 메서드 위에 선언하는 것으로 해당 메서드가 단위 테스트 메서드임을 선언한다.  
@Test 로 선언된 단위 테스틑 메서드는 따로 호출되는 경로가 없어도 단위 테스트 실행 시, JUnit이 자동으로 실행시켜 준다.

{% highlight java %}
public class TestClass {

    @Test
    public void test(){

        int a = 1, b = 2;
        System.out.println("a + b = " + (a + b)); // output: "a + b = 3"
    }
}
{% endhighlight %}

해당 코드를 작성 후, Run As... > JUnit Test 메뉴를 통해 단위 테스트를 실행 할 수 있다.