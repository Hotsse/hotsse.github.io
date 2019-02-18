---
layout: post
title: Java Integer Class
categories: [Java]
comments: true
---

int <=> 2진수 String

{% highlight java %}
// int to 2진수 String
int n = 55;
String bin = Integer.tobinaryString(n); // 110111

// 2진수 String to int
String bin2 = "110111";
int n2 = Integer.parseInt(bin2, 2);
{% endhighlight %}