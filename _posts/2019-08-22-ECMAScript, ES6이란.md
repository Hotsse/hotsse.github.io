---
layout: post
title: ECMAScript, ES6이란?
categories: [ES6]
comments: true
---

자바스크립트란?

자바스크립트(Javascript)는 1995년 넷스케이프(Netscape) 웹 브라우져에서 웹페이지에 동적인 요소를 구현하기 위해서 발명 되었다. 그 후 많은 다른 웹 브라우져들 또한 이 언어를 탑재하기 시작했고, 그 결과로 현대의 웹 어플리케이션의 구현을 가능하게 만든 언어이다. 이 언어로 인해 웹 어플리케이션에서 더 이상 사용자가 페이지 새로고침 또는 페이지를 새로 불러오지 않고도 웹과 직접적인 연결이 가능하게 되었다.

-------------

ECMAScript가 생긴 이유?

자바스크립트(Javascript)가 넷스케이프(Netscape) 브라우져만이 아니라 다른 웹 브라우져들의 지원까지 받기 시작하면서 다양한 웹 브라우져에서 자바스크립트(Javascript)가 공통되게 잘 작동하기 위해서 표준 규격이 필요해졌는데, 이 때문에, ECMA 국제 기구에서 “ECMAScript Standard”라 불리는 스크립트 표준이 만들어지게 된다. 자바스크립트와 비슷한 뜻으로 많이 들어본 사람들이 있을텐데, Javascript는 ECMAScript와 BOM(Browser Object Model) 와 DOM(Document Object Model)이라는 1개의 코어와, 2개의 모델로 이루어져 있다. ECMAScript 와 Javascript 는 비슷한 뜻으로 자주 쓰이나 작은 차이를 가지고 있다는 걸 알아두자.

-------------

ECMAScript란?

ECMAScript는 자바 스크립트를 이루는 코어(Core) 스크립트 언어로, 웹 환경에서만 호스트 되는 언어가 아니다. 웹 환경은 ECMA 스크립트가 호스트되는 환경들 중 하나일 뿐이다. ECMA 스크립트 호스트 환경은 ECMA 스크립트 실행 환경이 구현되있고, 각각 그 환경에 알맞는 확장성을 가지고 있다. 예를들어 웹 브라우져 환경에서는 BOM(Browser Object Model)과 DOM(Document Object Model)이 그 확장성이 되겠다. 이러한 확장성들은 ECMA 스크립트의 문법과 기능에 맞춰 기능의 확장을 가능게 한다. 자바스크립트의 document 객체가 좋은 예이다.

-------------

ES5 vs ES6

ES5(ECMAScript5)는 ECMAScript 2009 라고도 불리며, 이름에서 유추할 수 있듯이 2009년에 표준 규격으로 개발되어 현재까지도 널리 사용되고 있는 스크립트 표준 규격이다.  
하지만 어느 언어든지 오래 사용하다보면 해당 언어에 대한 단점이 부각되기 마련이다. 전 세계적으로 각광받는 자바스크립트 또한 예외는 아니다.  

ES6(ECMAScript6)는 완전히 새로운 개념이 아니다. ES5와 같이 스크립트 표준 규격이며, ES5의 다음 버전이다. 단지 그 뿐이다.  
다만 ES5 이후 6년 뒤에 발표된 표준 규격이라 사용자 편의성이나 당대의 트렌드를 고려한 신규 문법들이 추가되었다.

아래는 그에 대한 예이다.

{% highlight javascript %}
// add function with ES5
var add_es5 = function(a, b){
    return a + b;
}

console.log(add_es5(2,3)); // output: 5

// add function with ES6's Arrow Function
var add_es6 = (a, b) => a + b;

console.log(add_es6(2,3)); // output: 5
{% endhighlight %}

{% highlight javascript %}
var name = "홍길동"
var age = "20";

// print string with ES5
conesole.log("name : " + name + " / age : " + age); // output: "name: 홍길동 / age : 20"

// print string with ES6's Template String
console.log("name : ${name} / age : ${age}"); // output: "name: 홍길동 / age : 20"
{% endhighlight %}

-------------

References

https://takeuu.tistory.com/93?category=737612 [워너비스페셜]