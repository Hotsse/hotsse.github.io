---
layout: post
title: Spring Boot Logging 구성 및 제어
categories: [Spring]
comments: true
---

slf4j 와 logback

Spring Boot 는 Auto Configuration 에 의해 slf4j 와 logback 을 기본 Logging Framework 로 지원하고 있다.

- logback : Java 오픈소스 기반의 프레임워크로 기존 프레임워크들(log4j, log4j2, JUL)에 비해 월등한 성능을 자랑한다.
- slf4j : Logging 시스템을 위한 인터페이스로 Facade 패턴을 적용하여 유연성을 보장한 모델이다 -> 서비스 운영 중에 다른 Logging Framework 로 전환해도 손쉽게 진행할 수 있다.

-------------

그냥 System.out.println() 쓰면 안될까?

System.out 은 출력하는 모든 내용을 "STDOUT" 으로 출력한다. 이는 리다이렉션을 통해 파일 출력 등으로 전환하는 것으로 로깅 시스템과 비슷한 수준의 작동을 구현할 수는 있다.  
그러나 System.out 은 다음과 같은 이유로 Logging Framework 의 대체제가 될 수 없다.

- 시스템 출력 레벨의 저수준에서 개발자가 직접 제어하는 것은 위험한 방법임
- 로그의 중요도에 따른 레벨 분기가 불가능함
- 로그의 표준화된 레이아웃을 제공할 수 없음

-------------

Logging 맛보기

위에서 언급했다시피, Spring Boot 는 이미 Logging 기능을 지원하고 있으므로 추가적인 Dependency 를 pom.xml 에 추가할 필요 없이 비즈니스 로직에서 바로 사용해 볼 수 있다.

- HomeController.java

{% highlight java %}
package com.nexon.quicksample.core.base;
 
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;
 
import lombok.extern.slf4j.Slf4j;
 
@RestController
@RequestMapping("/home")
@Slf4j
public class HomeController {
 
    @RequestMapping("/logTest")
    public String logTest() {
 
        log.trace("trace");
        log.debug("debug");
        log.info("info");
        log.warn("warn");
        log.error("error");
 
        return "Hello Log Test!";
    }
     
    // ...
}
{% endhighlight %}

@Slf4j 어노테이션은 Lombok 에서 지원하는 어노테이션으로 slf4j 의 Logger 의 인스턴스를 log 라는 이름으로 생성해준다.  
@Slf4j 의 추가는 아래의 코드를 적은 것과 같다고 볼 수 있다.

{% highlight java %}
private final Logger log = LoggerFactory.getLogger(this.getClass()); // == @Slf4j
{% endhighlight %}

-------------

Logging 커스텀 설정

Logging 에 대한 추가 설정이 없는 경우 Framework 에서 지정하고 있는 기본 설정을 따르지만 서비스 특성 별로 설정을 수정하는 작업은 필수이다.

application.yml 에서 Logging 을 위한 커스텀 설정을 테스트 해보자.

{% highlight yaml %}
logging:
  pattern:
    console: "[%-5level] %d{yyyy-MM-dd HH:mm:ss} : %30logger{5} - %msg%n"
  level:
    root: info                                      # 기본 로그레벨 = INFO
    com.nexon.quicksample.home.controller: trace    # ...home.controller 로그 레벨 = TRACE
    com.nexon.quicksample.home.service: info        # ...home.service 로그 레벨 = INFO
    com.nexon.quicksample.home.dao: error           # ...home.dao 로그 레벨 = DAO
{% endhighlight %}

|옵션 Key|내용|비고|
|------------------------------|------------------------------|------------------------------|
|logging.pattern|로그 메시지의 레이아웃을 지정하는 옵션.<br>로그 출력 방식에 따라 레이아웃을 다르게 지정할 수 있다.<br>- logging.pattern.console = 콘솔 출력<br>- logging.pattern.file = 파일 출력|레이아웃 가이드<br>http://logback.qos.ch/manual/layouts.html#ClassicPatternLayout|
|logging.level|출력하고자 하는 최소 로그 레벨을 지정하는 옵션.<br>Package, Class 단위로 지정할 수 있다|-|

-------------

로그 레벨 단계

로그 레벨은 중요도에 따라서 5단계로 지정하고 있다. 로그 레벨이 지정되어 있다면 그보다 낮은 우선도의 로그는 출력되지 않는다. (기본 레벨 = INFO)

(높은 번호 = 높은 우선 순위)
1. TRACE
2. DEBUG
3. INFO
4. WARN
5. ERROR

예) 로그 레벨 = INFO -> 출력되는 로그 = {INFO, WARN, ERROR}

로컬, 개발환경 같은 경우는 DEBUG 까지 로그를 풀어 더 자세한 정보를 확인하는 것으로 디버깅을 진행하며, 운영환경에서는 INFO 나 WARN 수준으로 레벨을 설정하여 불필요한 로그가 쌓여 시스템 장애가 생기는 것을 방지한다.