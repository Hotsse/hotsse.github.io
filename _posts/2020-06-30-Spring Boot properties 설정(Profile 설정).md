---
layout: post
title: Spring Boot properties 설정(Profile 설정)
categories: [Spring]
comments: true
---

properties 검색

Spring Boot 는 properties 를 검색할 때 classpath 에서 다음과 같은 우선순위로 파일을 검색한다.

이름 우선순위
1. classpath:***/application*.yml
2. classpath:***/application*.yaml
3. classpath:***/application*.properties

경로 우선순위
1. 앱 실행옵션 "--spring.config.location" 에서 지정한 경로
2. ~/resources/config
3. ~/resources
4. classpath:/config
5. classpath:/

-------------

properties vs. YAML

properties
- 전통적인 설정파일 방식
- 많은 프로젝트에서 표준으로 사용되고 있음
- 다량의 환경변수 입력 시 가독성이 떨어짐
{% highlight text %}
environments.dev.url=http://dev.example.com
environments.dev.name=Developer Setup
environments.prod.url=http://another.example.com
environments.prod.name=My Cool App
{% endhighlight %}

YAML
- YAML(Yaml Ain't Markup Language) = 따로 학습이 필요 없는 설정파일 방식
- 들여쓰기로 Depth 가 구분되어 사람이 보기 좋은 구조
- 문서 규칙이 엄격함(들여쓰기, 값 입력 등)
{% highlight yaml %}
environments:
  dev:
    url: http://dev.example.com
    name: Developer Setup
  prod:
    url: http://another.example.com
    name: My Cool App
{% endhighlight %}

-------------

환경별 properties 분기

Spring Boot 는 application-{env}.yml (YAML 기준) 의 형식의 properties 파일을 찾으면 "env" 문구를 해당 properties 의 profile 로 인식한다.

|properties 명|profile 명|
|------------------------------|------------------------------|
|application.yml|기본|
|application-dev.yml|dev|
|application-stage.yml|stage|
|application-real.yml|real|

-------------

profile 설정 및 테스트

임의의 properties 와 profile 을 생성하여 분기 적용 시켜본다.

- application.yml
{% highlight yaml %}
test:
  message: Hello local
{% endhighlight %}

- application-dev.yml
{% highlight yaml %}
test:
  message: Hello dev
{% endhighlight %}

위의 properties 의 값을 불러오는 비즈니스 로직을 작성한다.

- HomeController.java
{% highlight java %}
package me.hotsse.quicksample.home;

// ...import 생략

@RestController
@RequestMapping({"/", "/home"})
public class HomeController {

    // test:message 값을 로드
    @Value("${test.message}")
    private String message;

    @GetMapping("/test")
    public String test() {
        return message;
    }
}
{% endhighlight %}

profile 값을 바꿔가며 실행결과를 확인해보자.

- profile 값이 없는 경우(default)
{% highlight bash %}
java -jar your_app.jar
{% endhighlight %}

- profile 값을 지정한 경우(dev)
{% highlight bash %}
java -jar -Dspring.profile.active=dev your_app.jar
{% endhighlight %}