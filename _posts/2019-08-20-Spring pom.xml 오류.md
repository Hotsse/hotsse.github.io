---
layout: post
title: Spring > pom.xml > ���� �ذ�
categories: [Spring]
comments: true
---

��Ȥ ���ο� ȯ�濡�� ������Ʈ�� üũ�ƿ� ���� ��, �̷� ���� ������ pom.xml ���� ������ ���� �Ѵ�.
�׿� ���� �ذ� ����� �˾ƺ���.

------------

META-INF\MANIFEST.MF ����

�Ʒ��� ���� ������ ���� ��쿡 ���� �ذ� ����̴�.

{% highlight text %}
... \target\m2e-wtp\web-resources\META-INF\MANIFEST.MF (������ ��θ� ã�� �� �����ϴ�)
{% endhighlight %}


Project Preferences ����

{% highlight text %}
Window > Preferences > Maven > Java EE Integration > WAR Project preferences
{% endhighlight %}

��� �ɼǿ��� �Ʒ� �׸� ���� üũ�� �����Ѵ�.

{% highlight text %}
'Maven Archiver generates files under the build directory'
{% endhighlight %}


target Ŭ����

Run As > Maven clean �� ���� target ������ Ŭ�����Ѵ�.

���� ������ ����� ���� Ȯ�� �� �� �ִ�.

------------

pom.xml ù �� > unknown ����

���� ���� pom.xml ù �ٿ� unknonw(Maven Configuration Problem) ������ ���� ��찡 �ִ�.
���� �ÿ� Maven �������� ���� ���� �ʾƼ� ����� ������� �Ѵ�. ���峪 ���࿡ �־ ������ ������ ������ ���� x�ڰ� �������� ������ �ɶ��ϰ� �ϴ� �Ʒ��� ���� ������� ������ �ذ�����.

- Maven > Update Project
������Ʈ ��Ŭ�� > Maven > Update Project �� ���̺� ������Ʈ.

- pom.xml ���� Maven ���� ����
pom.xml
{% highlight xml %}
<properties>
    <java.version>1.8</java.version>
    <maven-jar-plugin.version>3.1.1</maven-jar-plugin.version>
</properties>
{% endhighlight %}

���� ��� 1�� ���� ���̺� ������Ʈ�� ����.

�̷ν� unknown ������ ���� �ذ��� �����ϴ�.