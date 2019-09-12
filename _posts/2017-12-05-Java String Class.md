---
layout: post
title: Java String Class
categories: [Java]
comments: true
---


String <-> char

{% highlight java %}
// char to String
char c = 'A';
String str = String.valueOf(c);

// String to char
String str2 = "A";
char c2 = str2.charAt(0);
{% endhighlight %}

------------------

String <-> char Array

{% highlight java %}
// String to char Array
String str = "charArray";
char[] c_arr = str.toCharArray();

// char Array to String
String newStr = c_arr.toString();
{% endhighlight %}

----------------

String.split

{% highlight java %}
String str = "Hotsse~^~Developer~^~01012345678"
String []arr = str.split("~^~");
for(String tmp : arr){
    System.out.println(tmp);
}
{% endhighlight %}
