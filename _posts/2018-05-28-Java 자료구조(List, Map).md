---
layout: post
title: Java 자료구조(List, Map)
categories: [Java]
comments: true
---

Map 선언 및 사용법

{% highlight java %}
import java.util.*;
 
Map<Object, Object> map = new HashMap<Object, Object>();
 
map.put("key", "value"); // 데이터 삽입
 
Object value = map.get("key"); // 데이터 로드
{% endhighlight %}

------------

Map 순회(foreach)

{% highlight java %}
import java.util.*;
 
Map <String, Object> map = new HashMap();
 
/*
...
*/
 
for (Map.Entry<String, Object> entry : map.entrySet()) {
    String key = entry.getKey();
    Object value = entry.getValue();
    /*
    ...
    */
}
{% endhighlight %}

--------------------

Java List Class

{% highlight java %}
import java.util.*;
 
// 선언
// List는 추상 클래스이므로, 이를 구현한 ArrayList 로 생성
List<Integer> list = new ArrayList<Integer>();
 
// 데이터 
list.add(1);
list.add(2);
list.add(3);
 
// 순회
// 방법 1
for(int i=0; i<list.size(); i++){
    System.out.println("list(" + i + ") = " + list.get(i));
}
 
// 방법 2
for(int element : list){
    System.out.println("list-elements = " + element);
}
{% endhighlight %}
