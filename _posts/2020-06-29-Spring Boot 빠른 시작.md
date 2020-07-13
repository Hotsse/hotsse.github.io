---
layout: post
title: Spring Boot 빠른 시작
categories: [Spring]
comments: true
---

프로젝트 생성

IDE 별로 프로젝트 생성 과정이 다르기 때문에 Spring 공식 홈페이지에서 프로젝트 생성 툴을 지원하고 있다.
{% highlight http %}
https://start.spring.io
{% endhighlight %}

- 프로젝트 기본 설정에서 빌드(프로젝트) 관리 도구, 언어, 버전 및 프로젝트 기본 정보를 입력
- 프로젝트에서 사용하고자 하는 Dependency 들을 선택 후 Generate 버턴으로 프로젝트 생성 -> 선택한 Dependency 들은 pom.xml 에 자동으로 등록됨
- zip 파일로 생성된 프로젝트를 workspace 에 압축해제

-------------

개발환경 설정(Eclipse 기준)

프로젝트 Import
- Project Explorer > Import... > Import > Maven > Existing Maven Projects > Browse... > 프로젝트 디렉토리 선택 > Finish

JDK 설정
- Project Explorer > Properties > Java Build Path > JDK 설정

-------------

빌드

|Traditional|Configurational|
|------------------------------|------------------------------|
|빌드<br>- Run As... > Maven clean<br>- ... > Maven install|초기 세팅<br>- Run As... > Maven build... > Goal : clean install, check Skip Test > Apply > Run<br><br>빌드<br>- Run As... > Maven build|
|특징<br>- clean, install 을 수동으로 진행해야 해서 절차가 많음<br>- 매 빌드 시마다 Test 가 진행되어 불필요한 과정이 추가됨<br>- 빌드 속도가 상대적으로 느림|특징<br>- 빌드 과정을 하나의 세트로 묶어서 관리함<br>- Skip Tests 옵션으로 불필요한 빌드 과정을 제거함<br>- 빌드 속도가 상대적으로 빠름|

-------------

서비스 구동

- Project Explorer > Run As... > Spring Boot App 으로 서버 구동

-------------

비즈니스 로직 구성 및 테스트

(JSP, JDBC 구성은 커스텀 설정이 필요하여 REST API 기반의 임시 데이터 생성 로직으로 테스트 한다)

package 및 class 구성
{% highlight bash %}
me.hotsse.quicksample/
└── home
    └── HomeController.java
    └── HomeDao.java
    └── HomeService.java
    └── HomeTestVO.java
{% endhighlight %}

- HomeController.java
{% highlight java %}
package me.hotsse.quicksample.home;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
 
import org.springframework.web.bind.annotation.RestController;
 
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
 
@RestController
@RequestMapping({"/", "/home"})
public class HomeController {
     
    @Autowired
    private HomeService homeService;
 
    @GetMapping("")
    public HomeTestVO index(HttpServletRequest req, HttpServletResponse res) {
        return homeService.getTest();
    }     
}
{% endhighlight %}

- HomeService.java
{% highlight java %}
package me.hotsse.quicksample.home;
 
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
 
@Service
public class HomeService {
 
    @Autowired
    private HomeDao homeDao;
     
    public HomeTestVO getTest() {
        return homeDao.getTest();
    }     
}
{% endhighlight %}

- HomeDao.java
{% highlight java %}
package me.hotsse.quicksample.home;
 
import org.springframework.stereotype.Repository;
 
@Repository
public class HomeDao {
 
    public HomeTestVO getTest() {
         
        // JDBC 연동은 추가 설정이 필요한 관계로 데이터를 임시 생성하는 것으로 대체한다.
        HomeTestVO test = new HomeTestVO();
        test.setParam1("value1");
        test.setParam2("value22");
        test.setParam3("value333");
         
        return test;
    }
}
{% endhighlight %}

- HomeTestVO.java
{% highlight java %}
package me.hotsse.quicksample.home;

import lombok.Data;

@Data
public class HomeTestVO {

    private String param1;
    private String param2;
    private String param3;
}
{% endhighlight %}
