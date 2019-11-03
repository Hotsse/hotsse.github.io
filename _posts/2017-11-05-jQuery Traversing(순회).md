---
layout: post
title: jQuery Traversing(순회)
categories: [jQuery]
comments: true
---

Traversing(순회)? 대상이 되는 element에서 목표로 하는 다른 element 까지 이동하는 방법.

-----------



올라가기

parent() : 바로 상위의 부모를 리턴한다.
{% highlight javascript %}
$("span").parent();
{% endhighlight %}

parents() : 바로 상위 ~ 최상위까지의 모든 부모를 리턴한다.
parentsUntil() : 바로 상위 ~ action 안의 조건에 부합하는 element가 나올때까지 모든 부모를 리턴한다.

------------------

내려가기

children() : 바로 하위에 있는 모든 자식들을 리턴한다. (차(sub)하위는 해당 안됨)
- action (  ) 에 조건을 넣어서 필터를 적용할 수 있다.

find() : 대상 element 하위에 있는 모든 자식들을 리턴한다. 
- action (  ) 에 조건을 넣어서 필터를 적용할 수 있다.
