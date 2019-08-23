---
layout: post
title: ES6 변수 선언 키워드(let, const)
categories: [ES6]
comments: true
---

let과 const

ES5까지는 **var** 키워드로 변수를 선언했었는데, 이는 중복 선언이 가능하며 Function-level scope를 가진다.
사용에 따라서는 편리할수도 있으나 우리들의 전통적인(?) 코딩 스타일에는 부합하지 않아서 의도하지 않은 결과를 내곤 했는데, ES6 에서는 이를 개선한 변수 선원 키워드가 등장했다.

ES6의 **let**과 **const** 키워드는 Block-level scope를 지원한다.

-------------

let 의 특징

- Block-level scope

{% highlight javascript %}
let a = "global";
let g = "global";
{
    let a = "block";
    let b = "block";

    console.log(a); // output: "block";
    console.log(g); // output: "global";
}
console.log(a); // output: "global";
console.log(b); // error
{% endhighlight %}

- 변수 중복 선언 불가

{% highlight javascript %}
var a = 111;
var a = 222;

console.log(a); // output: 222;
{% endhighlight %}
{% highlight javascript %}
var a = 111;
var a = 222; // error
{% endhighlight %}

- 호이스팅 불가

코드 내에 존재하는 변수가 선언되기 전에 호출된 경우, 해당 변수를 미리 메모리 할당해주는 기술을 호이스팅(hoisting) 이라고 한다. 메모리 할당만 해줄 뿐 코드대로 초기화까지는 진행하지 않기 때문에 호이스팅으로 선언된 변수는 undefined 값을 가지게 된다.

호이스팅을 지원하지 않는 let 은 선언 전에 호출된 경우 error 를 내게 된다.

{% highlight javascript %}
console.log(a); // undefined
var a = 1;

console.log(b); // error
let b = 2;
{% endhighlight %}

-------------

const 의 특징

const 는 상수를 정의하는데 사용된다.

- Block-level scope

- 재할당 불가능
{% highlight javascript %}
let a = 111;
a = 222; // input: 222;

const b = 111;
b = 222; // error
{% endhighlight %}

-------------

References

https://takeuu.tistory.com/86?category=737612 [워너비스페셜]