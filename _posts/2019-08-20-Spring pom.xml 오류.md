---
layout: post
title: Spring > pom.xml > 오류 해결
categories: [Spring]
comments: true
---

간혹 새로운 환경에서 프로젝트를 체크아웃 받을 때, 이런 저런 이유로 pom.xml 에서 에러가 나곤 한다.
그에 대한 해결 방법을 알아보자.

-------------

META-INF\MANIFEST.MF 오류

아래와 같은 에러가 나는 경우에 대한 해결 방법이다.

{% highlight text %}
... \target\m2e-wtp\web-resources\META-INF\MANIFEST.MF (지정된 경로를 찾을 수 없습니다)
{% endhighlight %}


Project Preferences 설정

{% highlight text %}
Window > Preferences > Maven > Java EE Integration > WAR Project preferences
{% endhighlight %}

상기 옵션에서 아래 항목에 대한 체크를 해제한다.

{% highlight text %}
'Maven Archiver generates files under the build directory'
{% endhighlight %}


target 클리어

Run As > Maven clean 을 통해 target 폴더를 클리어한다.

에러 문구가 사라진 것을 확인 할 수 있다.

-------------

pom.xml 첫 줄 > unknown 에러

이유 없이 pom.xml 첫 줄에 unknonw(Maven Configuration Problem) 오류가 나는 경우가 있다.
빌드 시에 Maven 의존성의 합이 맞지 않아서 생기는 오류라고 한다. 빌드나 실행에 있어서 오류가 나지는 않지만 빨간 x자가 개발자의 마음을 심란하게 하니 아래의 수정 방법으로 에러를 해결하자.

- Maven > Update Project
프로젝트 우클릭 > Maven > Update Project 로 메이븐 업데이트를 진행.

- pom.xml 에서 Maven 버전 변경
pom.xml
{% highlight xml %}
<properties>
    <java.version>1.8</java.version>
    <maven-jar-plugin.version>3.1.1</maven-jar-plugin.version>
</properties>
{% endhighlight %}

이후 방법 1과 같이 메이븐 업데이트를 진행.

이로써 unknown 에러에 대한 해결이 가능하다.
