---
layout: post
title: Spring Project Lombok
categories: [Spring]
comments: true
---

Project Lombok

Lombok 은 Java 기반에서 기계적으로 작성하는 VO 관련 작업을 보다 쉽게 하게 해주는 도구이다. Lombok 을 통해 VO 의 멤버에 대한 Getter, Setter, ToString, hashCode 에 대한 메서드 작업을 어노테이션을 통해 깔끔하게 작성할 수 있는 장점이 있다.

-------------

Lombok 설치 (Dependency 추가)

Spring Boot 의 경우, 공식적으로 지원하는 라이브러리이기에 https://start.spring.io 를 통해 프로젝트를 생성할 시 해당 옵션을 선택하는 것으로 프로젝트에 Dependency를 포함시킬 수 있다.

하지만 그렇지 않은 프로젝트의 경우는 수동으로 Dependency 를 추가해야 한다. 해당 프로젝트의 pom.xml 에 아래의 Dependency 를 추가하자.

{% highlight xml %}
<dependency>
	<groupId>org.projectlombok</groupId>
	<artifactId>lombok</artifactId>
	<optional>true</optional>
</dependency>
{% endhighlight %}

-------------

Lombok 설치 (Lombok 프로그램 설치 및 IDE 연동)

Dependency 가 추가되었다면, 하기 경로의 Local Repository 로 이동하여 Lombok 의 jar 파일을 찾는다.

{% highlight dos %}
cd C:\Users\{User}\.m2\repository\org\projectlombok\lombok\{Version}\
{% endhighlight %}

상기 경로의 jar 파일을 실행하는 것으로 Lombok 을 설치할 수 있다.

{% highlight dos %}
java -jar lombok-{Version}.jar
{% endhighlight %}

설치 UI 를 따라 설치하되, 설치 이전에 IDE 는 종료되어 있어야 하며, 해당 IDE 의 실행파일을 지정해 주어야 연동 작업이 진행된다.

Eclipse 의 경우, 설치가 완료된 후 eclipse.ini 에 해당 옵션이 추가 되었는지 확인한다.

{% highlight ini %}
-vmargs
-javaagent:lombok.jar
{% endhighlight %}

이로써 모든 설치가 완료 되었으며, IDE 재시작 및 적용하고자 하는 프로젝트의 빌드를 진행하면 사용할 준비가 완료된다.

-------------

Before Lombok

{% highlight java %}
public class UserVO {
    private String id;
    private String nm;
    private int age;

    public String getId() {
        return id;
    }
    public String getNm() {
        return nm;
    }
    public int getAge() {
        return age;
    }
    public void setId(id) {
        this.id = id;
    }
    public void setNm(nm) {
        this.nm = nm;
    }
    public void setAge(age){
        this.age = age;
    }
    @Override
    public String toString() {
        return "UserVO [id=" + id + ", nm=" + nm + ", age=" + age + "]";
    }
}
{% endhighlight %}

VO 의 각 멤버에 대한 Getter, Setter, ToString 메서드는 각 IDE 기능을 통해 손쉽게 추가할 수 있긴 하다.  
하지만 멤버의 수정이 있을 경우, 그에 대한 메서드까지 전부 수정해야 한다는 번거로움이 있을 뿐더러, 일정한 규칙으로 작용하는 메서드가 불필요하게 소스에 나열되어 있기에 가독성을 해치기도 한다.

-------------

After Lombok

{% highlight java %}
import lombok.*;

// @Data -> 모든 어노테이션 적용
@Getter
@Setter
@ToString
public class UserVO {
    private String id;
    private String nm;
    private int age;
}
{% endhighlight %}

lombok 은 라이브러리에서 제공하는 어노테이션을 적용하는 것으로 멤버에 필요한 메서드를 직접 생성해주는 강력한 기능을 가지고 있다.  
각 메서드별로 어노테이션이 나뉘어 있으나, lombok 에서 제공하는 모든 기능을 사용하려면 **@Data** 어노테이션을 적용하는 것으로 한방에 해결이 가능하다. (하지만 무분별한 @Data 어노테이션의 남용은 의도하지 않은 메서드까지 추가/수정 되어 기능 장애를 일으킬 수도 있으니 신중하게 사용해야 할 것이다)

-------------
