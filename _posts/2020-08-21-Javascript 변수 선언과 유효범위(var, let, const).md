---
layout: post
title: Javascript 변수 선언과 유효범위(var, let, const)
categories: [Javascript]
comments: true
---

변수 선언과 유효범위

자바스크립트의 변수 선언 방식은 총 4가지(ECMAScript6 기준)이다.
(ECMAScript5 기준으로는 2가지)
선언 키워드에 따라서 같은 로직에도 다른 결과가 나타날 수 있으므로, 이를 이해하기 위해 정리한다.

-------------

키워드 없이 선언

키워드 없이 선언된 변수에 대해서는 **전역 변수로 선언** 된다.
기존에 같은 이름을 가진 변수가 있을 경우 암묵적으로 덮어쓰게 된다.

{% highlight javascript %}
var var1 = "global";

function test(){
    var1 = "local";
    var2 = "local";
}

test();

console.log(var1); // output: "local"
console.log(var2); // output: "local"

{% endhighlight %}

-------------

var 선언

var 키워드로 선언된 변수는 **지역 변수의 범주**에 들어간다.
전역에 동일한 변수명이 있을 경우 무시하고 새로운 변수가 생성된다.

{% highlight javascript %}
var var1 = "global";

function test(){
    var1 = "local";
    console.log(var1); // output: "local"
}

test();
console.log(var1); // output: "global"

{% endhighlight %}

-------------

let 선언

let과 var의 차이는 **let 은 Block-level Scope** 이며, **var 는 Function-level Scope** 라는 점이다.
let으로 선언된 변수는 해당 블록을 탈출하는 순간 소멸한다.

{% highlight javascript %}
function test_var(){
    if(true){
        var var1 = "function";
    }
    console.log(var1); // output: "function"
}

function test_let(){
    if(true){
        let var1 = "block";
    }
    console.log(var1); // error
}
{% endhighlight %}

let 은 ECMAScript6 부터 지원하는 키워드이다.

-------------

const 선언

const는 let과 동일하나, 초기화 이후로는 값을 수정할 수 없는 **"상수"**를 지정할 때 쓰인다.

{% highlight javascript %}
const MAX_VALUE = 100; // 명명규칙 > 상수 = 대문자

function test(){
    console.log(MAX_VALUE); // output: 100
    MAX_VALUE = 101; // error
}
{% endhighlight %}

const 은 ECMASCript6 부터 지원하는 키워드이다.