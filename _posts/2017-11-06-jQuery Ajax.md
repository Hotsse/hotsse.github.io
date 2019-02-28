---
layout: post
title: jQuery Ajax
categories: [jQuery]
comments: true
---
jQuery - AJAX

AJAX : Asynchronous JavaScript and XML(비동기 JavaScript와 XML)
웹 페이지를 이동하지 않고 Background에서 data를 송수신하여 화면 전환을 이루게 하는 방법.

-----------------

load 메소드

load() : selector에 해당 주소에서 수신한 정보로 갱신함..

{% highlight javascript %}
$(document).ready(function(){
    $("button").click(function(){
        $("#div1").load("demo_test.txt");
    });
});
// id == div1 인 element에 demo_test.txt 에서 받아온 정보를 덮어씌움.
{% endhighlight %}

콜백함수 구조
{% highlight javascript %}
$("button").click(function(){
    $("#div1").load("demo_test.txt", function(responseTxt, statusTxt, xhr){
        if(statusTxt == "success")
            alert("External content loaded successfully!");
        if(statusTxt == "error")
            alert("Error: " + xhr.status + ": " + xhr.statusText);
    });
});
{% endhighlight %}

responseTxt : (실행 성공 시) load 실행 결과값을 가지고 있음
statusTxt : 호출의 상태 정보를 가지고 있음
xhr - XMLHttpRequest object 를 담고 있음

-----------------

jQuery - AJAX get(), post() 메소드

jQuery get()과 post() 메소드는 비동기적으로 서버에 HTTP GET or POST 방식으로 data를 request하고 그 결과값을 response 하기 위해 사용합니다.

GET vs. POST

GET : 특정 자원에 data 를 요청하기 위해 쓰인다. (읽기 용도로 주로 쓰임)
POST : 특정 자원에 data 를 처리하기 위한 용도로 쓰인다. (쓰기 용도로 주로 쓰임)

{% highlight javascript %}
$("button").click(function(){
    $.get("demo_test.asp", function(data, status){
        alert("Data: " + data + "\nStatus: " + status);
    });
});

$("button").click(function(){
    $.post("demo_test_post.asp",
    {
        name: "Donald Duck",
        city: "Duckburg"
    },
    function(data, status){
        alert("Data: " + data + "\nStatus: " + status);
    });
});
{% endhighlight %}

----------------

.ajax() 메소드


해당 메소드는 많은 옵션을 쉽게 설정할 수 있어, 설정이 까다로운 방식의 request 에 사용하기 적합하다.
method 속성으로 get/post 를 설정할 수도 있기에, .get()/.post() 메소드를 대체할 수 있는 범용적인 기능이다.
{% highlight javascript %}
$("button").click(function(){
    $.ajax({url: "demo_test.txt", method: "post", success: function(result){
        $("#div1").html(result);
    }});
});
{% endhighlight %}
