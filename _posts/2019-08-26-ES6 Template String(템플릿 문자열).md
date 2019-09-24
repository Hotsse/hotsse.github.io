---
layout: post
title: ES6 Template String(템플릿 문자열)
categories: [ES6]
comments: true
---

Template String

Template String 은 여러 형태의 변수 값을 포함하고 문자열을 표현할 때 매우 간결하게 표현할 수 있는 문자열 선언 방식이다.

-------------




선언 방법

{% highlight javascript %}
let story = `Hello World! I'm a programmer from "Korea"`;
console.log(story);
{% endhighlight %}

{% highlight javascript %}
const a = 1, b = 2;
int sum = a+b;

// expect output: "sum calc: 1 + 2 = 3";

// ES5
console.log("sum calc: " + a + " + " + b + " = " + sum); // output: "sum calc: 1 + 2 = 3"

// ES6
console.log(`sum calc: ${a} + ${b} = ${sum}`); // output: "sum calc: 1 + 2 = 3"
{% endhighlight %}

-------------

EL(Expression Language)과의 충돌 문제

EL 은 JSP의 가독성을 높히기 위해 개발된 JSP 코드 표현언어이다. JSP 2.0 부터 지원되며 가독성이 상당히 증가하여 많은 프로젝트에서 사용되고 있다.

허나 EL의 변수 표현방식이 ES6 의 템플릿 문자열과 같은 **${...}** 형식을 취하고 있다.  
이로 인해서 JSP(EL) + ES6 환경에서 해당 표현식을 사용한다면 의도하지 않은 결과(혹은 에러 페이지)를 만날 수도 있다.

스택 오버플로우에서는 아래의 두 가지 방법을 제시하고 있다.

- 블록 감싸기
{% highlight javascript %}
let today = "2019-08-26";
let msg = `오늘은 ${'${today}'} 입니다`;
{% endhighlight %}

- .js 파일 별도 관리
ES6 관련 스크립트를 .js 파일로 별도 관리하는 방식으로 우회하는 방식 

-------------

References

https://takeuu.tistory.com/88?category=737612 [워너비스페셜]
https://okky.kr/article/492977 [OKKY]