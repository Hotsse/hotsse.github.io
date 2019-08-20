---
layout: post
title: Spring > pom.xml > META-INF\MANIFEST.MF 오류 해결
categories: [Spring]
comments: true
---

간혹 새로운 환경에서 프로젝트를 체크아웃 받을 때, pom.xml 에서 아래와 같은 에러가 나오는 경우가 있다.

{% highlight text %}
... \target\m2e-wtp\web-resources\META-INF\MANIFEST.MF (지정된 경로를 찾을 수 없습니다)
{% endhighlight %}

이 문제에 대한 해결 방법을 알아보자.

------------

Project Preferences 설정

{% highlight text %}
Window > Preferences > Maven > Java EE Integration > WAR Project preferences
{% endhighlight %}

상기 옵션에서 아래 항목에 대한 체크를 해제한다.

{% highlight text %}
'Maven Archiver generates files under the build directory'
{% endhighlight %}

-----------

target 클리어

Run As > Maven clean 을 통해 target 폴더를 클리어한다.

에러 문구가 사라진 것을 확인 할 수 있다.