---
layout: post
title: Java Integer Class
categories: [Java]
comments: true
---

int <=> n진수 String

``` java
int radix = 2; // 2진수
int from = 55; // origin

// int to n진수 String
String str = Integer.toString(from, radix); // expect: "110111"

// n진수 String to int
int to = Integer.parseInt(str, radix); // expect: 55
```
