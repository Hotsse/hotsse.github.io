---
layout: post
title: ES6 Arrow Function(화살표 함수)
categories: [ES6]
comments: true
---

Arrow Function

Arrow Function 은 익명함수를 화살표 기호를 사용하는 것으로 간략하게 선언할 수 있는 새로운 함수 선언 방식이다.

-------------

선언 방법

- 매개변수가 없을 때
{% highlight javascript %}
// ES5
function(){ ... }

// ES6
() => { ... }
{% endhighlight %}

- 매개변수가 한 개일 때
{% highlight javascript %}
// ES5
function(x) {
    return x*x; // output: pow(x)
}

// ES6(case 1)
x => x*x; // output: pow(x)

// ES6(case 2)
x => {
    return x*x; // output: pow(x)
}
{% endhighlight %}

- 매개변수가 두 개 이상일 때
{% highlight javascript %}
// ES5
function(x,y){
    return x+y; // output: x+y
}

// ES6(case 1)
(x,y) => x+y; // output: x+y

// ES6(case 2)
(x,y) => {
    return x+y; // output: x+y
}
{% endhighlight %}

-------------

익명함수를 변수에 할당하고 사용하는 예제

{% highlight javascript %}
var openPopup = (idx)
    => window.open(`openPopup.do?idx=${idx}`, "", ""); // Template String 사용
{% endhighlight %}

-------------

Lexical this

자바스크립트의 this는 함수 호출 패턴에 따라 this에 바인딩 되는 객체가 달라진다.  
허나 Arrow Function은 항상 자신을 포함하는 외부 scope에서 this를 받는데 이를 Lexical this라 한다.

Arrow Function 을 사용하면 this를 보다 직관적으로 사용할 수 있다.

-------------

References

https://takeuu.tistory.com/88?category=737612 [워너비스페셜]

