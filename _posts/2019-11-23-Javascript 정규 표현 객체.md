---
layout: post
title: Javascript 정규 표현 객체
categories: [Javascript]
comments: true
---

Javascript 정규 표현 객체

정규 표현(RegExp) 객체는 입력 요소에 데이터를 규칙에 맞게 작성했는지 판단해서 알려주는 객체이다.  
즉, 내가 지정한 규칙대로 단어가 입력되었는지, 잘못된 단어를 포함하고 있는지 찾을 때 사용한다.

정규 표현 객체를 생성하는 기본형은 두 가지가 있다.

{% highlight javascript %}
var exp1 = new RegExp(패턴, 검색_옵션);
var exp2 = /패턴/검색_옵션
{% endhighlight %}

-------------

검색 옵션

정규 표현에는 검색 옵션이라는 것이 있다. 검색 옵션을 사용하면 일치하는 단어를 찾을 때, 다양한 조건과 규칙을 붙여 검색할 수 있다.

-------------

정규 표현 객체 메서드

지정한 특정 문자열이 정규 표현 규칙에 부합하는지를 체크하는 메서드이다.

{% highlight javascript %}

var reg1 = /css/;

if(reg1.test("html css javascript")){
    alert("SUCCESS");
}
else{
    alert("FAIL");
}

{% endhighlight %}





