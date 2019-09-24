---
layout: post
title: JUnit @Test 및 Assert Class
categories: [JUnit]
comments: true
---

단위 테스트 기본

단위 테스트를 진행하기 위해선 테스트를 진행하고자 하는 메서드를 단위 테스트 메서드로 지정해야 한다. @Test 어노테이션은 특정 메서드 위에 선언하는 것으로 해당 메서드가 단위 테스트 메서드임을 선언한다.  
@Test 로 선언된 단위 테스틑 메서드는 따로 호출되는 경로가 없어도 단위 테스트 실행 시, JUnit이 자동으로 실행시켜 준다.

단위 테스트 클래스는 관례에 따라 접미사 "Test" 를 붙이도록 한다.

{% highlight java %}
public class UnitTest {

    @Test
    public void test(){

        int a = 1, b = 2;
        System.out.println("a + b = " + (a + b)); // output: "a + b = 3"
    }
}
{% endhighlight %}

해당 코드를 작성 후, Run As... > JUnit Test 메뉴를 통해 단위 테스트를 실행 할 수 있다.

-------------

@Test 옵션

- timeout
timeout 은 테스트 메서드의 수행시간 제한 옵션이다. timeout 옵션이 걸린 테스트 메서드는 옵션에 설정된 시간(ms 단위)을 초과하면 자동으로 테스트 실패로 처리된다.

{% highlight java %}
public class UnitTest {

    @Test(timeout=10)
    public void testTimeout(){

        int sum = 0;
        for(int i=1; i<=100000000; i++){
            sum += i;
        }
    }

}
{% endhighlight %}

상기 테스트 메서드는 10ms 의 수행시간 제한이 있으며, 테스트는 실패할 것이다. (환경에 따라서는.. 어쩌면 될지도?)

- expected
expected 는 테스트 중 Exception 이 발생하는지의 여부로 테스트의 실행 결과를 판단하는 옵션이다.
옵션으로 지정된 Exception 이 발생 시 성공, 발생하지 않은 경우 실패로 간주된다.

{% highlight java %}
public class UnitTest {

    @Test(expected=NumberFormatException.class)
    public void test() {
        
        String str = "isNotInteger";
        int a = Integer.parseInt(str); // NumberFormatException occured
    }
}
{% endhighlight %}

상기 테스트 메서드는 Integer 로 변환 불가능한 문자열을 변환 시도 하였으므로, NumberFormatException 이 발생하며, expected 옵션의 Exception 과 일치하기 때문에, 테스트는 성공하게 된다.

-------------

Assert

assertEquals(a, b);
- 객체 a와 b의 값이 일치함을 검증한다.

assertSame(a, b);
- 객체 a와 b가 일치함을 검증한다. (연산자 ==)

assertArrayEquals(a, b);
- 배열객체 a와 b가 일치함을 검증한다.

assertTrue(a);
- 조건 a가 참인지 검증한다.

assertNotNull(a);
- 객체 a가 null이 아님을 검증한다.

