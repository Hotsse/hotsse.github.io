---
layout: post
title: jQuery Get, Set, Add, Remove
categories: [jQuery]
comments: true
---

jQuery는 JavaScript처럼 대상 HTML elements에서 값을 읽어올 수도 있고, 반대로 값을 대입할 수도 있다.

-------------------


Get Content - text(), html(), val()

text() - 대상 elements 안의 텍스트만을 반환한다.
{% highlight javascript %}
$("#btn1").click(function(){
    alert("Text: " + $("#test").text());
});
{% endhighlight %}

html() - 대상 elements 안의 HTML markup을 포함한 모든 내용을 반환한다
{% highlight javascript %}
$("#btn2").click(function(){
    alert("HTML: " + $("#test").html());
});
{% endhighlight %}

val() - 대상 elements 의 value 값을 반환한다(input 등)
{% highlight javascript %}
$("#btn1").click(function(){
    alert("Value: " + $("#test").val());
});
{% endhighlight %}

----------------

Set Content - text(), html(), val()

Get Content의 반대 역할을 한다. ( ) 안의 내용이 비어 있으면 Get, 내용이 존재하면 해당 값으로 Set 이 된다.
{% highlight javascript %}
$("#btn1").click(function(){
    $("#test1").text("Hello world!");
});
$("#btn2").click(function(){
    $("#test2").html("<b>Hello world!</b>");
});
$("#btn3").click(function(){
    $("#test3").val("Dolly Duck");
});
{% endhighlight %}

정적인 값 대신에 function을 넣어서 처리하는 방식도 가능하다.
{% highlight javascript %}
$("#btn1").click(function(){
    $("#test1").text(function(i, origText){
        return "Old text: " + origText + " New text: Hello world!
        (index: " + i + ")"; 
    });
});
{% endhighlight %}

Set Content - attr()

대상이 되는 elements의 속성(Attribute)도 수정할 수 있다.
{% highlight javascript %}
$("button").click(function(){
    $("#w3s").attr({
        "href" : "https://www.w3schools.com/jquery";,
        "title" : "W3Schools jQuery Tutorial"
    });
});
{% endhighlight %}

--------------------------

Add Content 

append() - 해당 elements 안에서 가장 마지막에 내용을 추가
{% highlight javascript %}
$("p").append("Some appended text.");
{% endhighlight %}

prepend() - 해당 elements 안에서 가장 처음에 내용을 추가
{% highlight javascript %}
$("p").prepend("Some prepended text.");
{% endhighlight %}

after() - 해당 elements 바로 뒤에 내용을 추가
{% highlight javascript %}
$("img").after("Some text after");
{% endhighlight %}

before() - 해당 elements 바로 앞에 내용을 추가
{% highlight javascript %}
$("img").before("Some text before");
{% endhighlight %}

---------------------

Remove Content

remove() - selecter에 해당하는 태그를 페이지 내에서 삭제한다. action의 (  ) 에는 추가적인 필터를 작성할 수도 있다.
{% highlight javascript %}
$("p").remove(".test, .demo"); // <p> 태그 중에 class명이 test나 demo인 element 를 삭제한다.
{% endhighlight %}

empty() - 해당 element 내의 모든 content를 삭제한다.
{% highlight javascript %}
$("#div1").empty();
{% endhighlight %}

