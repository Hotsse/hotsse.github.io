---
layout: post
title: Spring Boot Thymeleaf Layout 적용
categories: [Spring]
comments: true
---

Thymeleaf Layout 란?

모든 프론트 페이지가 그러하듯이 html 구조 내에서 재사용 되는 부분들이 존재한다. (head or footer 등...)

레이아웃은 페이지 전체에 대한 구성과 재사용되는 코드들을 미리 정의해 둔 파일이며, 이후에 실제 페이지 파일들은 변경되는 부분에 대한 코드만 작성하는 구조가 된다.

-------------

Spring Boot 에서의 Thymeleaf Layout 초기 구성

Thymeleaf Layout 을 연동하기 위해서는 기본 Dependency set 에서는 제공하지 않는 Dependency 를 추가 해야한다.

- pom.xml
{% highlight xml %}
<dependencies>
    <!-- ... -->
    <dependency>
        <groupId>nz.net.ultraq.thymeleaf</groupId>
			<artifactId>thymeleaf-layout-dialect</artifactId>
	</dependency>
    <!-- ... -->
</dependencies>
{% endhighlight %}

이후에는 Java Configuration 에서 Bean 객체로 생성해 둔 Thymeleaf TemplateEngine 객체에 레이아웃을 활성화 하는 옵션을 추가한다.

- ContextConfig.java
{% highlight java %}
package com.nexon.quicksample.core.config;
 
import org.springframework.context.annotation.Configuration;
import org.thymeleaf.spring5.SpringTemplateEngine;
import org.thymeleaf.templateresolver.ClassLoaderTemplateResolver;

import nz.net.ultraq.thymeleaf.LayoutDialect;
 
@Configuration
public class ContextConfig implements WebMvcConfigurer {
     
    @Bean
    public SpringTemplateEngine templateEngine() {
        SpringTemplateEngine templateEngine = new SpringTemplateEngine();
        templateEngine.setTemplateResolver(thymeleafTemplateResolver());
        templateEngine.addDialect(new LayoutDialect()); // thymeleaf-dialect 활성화
        return templateEngine;
    }

    @Bean
    public ClassLoaderTemplateResolver thymeleafTemplateResolver() {
        ClassLoaderTemplateResolver templateResolver = new ClassLoaderTemplateResolver();
        templateResolver.setPrefix("templates/"); // 모든 뷰 페이지는 /resources/templates/ 내부에서 검색한다.
        templateResolver.setSuffix(".html"); // 모든 뷰 페이지는 .html 이다.
        templateResolver.setTemplateMode("HTML"); // HTML 형식으로 읽는다.
        templateResolver.setCacheable(false); // 캐싱하지 않는다.
        return templateResolver;
    }     
}
{% endhighlight %}

이로써 초기 구성은 완료 되었고, 이제 실제로 레이아웃 파일과 페이지 파일을 작성하여 테스트 할 수 있도록 한다.

-------------

테스트

간단한 페이지 소스코드와 함께 Thymeleaf Layout 를 연동하여, 동작 여부를 확인하자.

우선 페이지를 연결 시켜줄 Controller 의 RequestMapping 을 구성한다.

- HomeController.java
{% highlight java %}
package com.nexon.quicksample.home;
 
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
 
@Controller
@RequestMapping(value = "/")
public class HomeController {
     
    @RequestMapping("/thymeleaf")
    public String thymeleaf() {
         
        System.out.println("HomeController:thymeleaf");
         
        return "pages/thymeleaf"; // /resources/templates/"thymeleaf.html" 을 뷰 페이지로 반환한다.
    }
     
}
{% endhighlight %}

이후에는 레이아웃 페이지 코드를 작성한다.

- /resource/templates/layouts/default.html
{% highlight html %}
<!DOCTYPE html>
<html lang="ko"
	xmlns:th="http://www.thymeleaf.org"
	xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout">
<head>
	<title>THYMELEAF with Layout</title>
	<meta charset="utf-8">
</head>
<body>

<header>
	<h1>HEADER</h1>
</header>

<div layout:fragment="content">
</div>

<footer>
    <p>FOOTER</p>
</footer>

</body>
</html>
{% endhighlight %}

layout:fragment 로 지정해 둔 공간에 실제 페이지 컨텐츠를 끼워맞추는 식으로 전체 페이지가 구성되는 방식이다.  
마지막으로 컨텐츠 페이지가 될 html 코드를 작성하여 페이지를 완성하자.

- /resources/templates/pages/thymeleaf.html

{% highlight html %}
<html lang="ko"
	xmlns:th="http://www.thymeleaf.org"
	xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout"
    layout:decorator="layouts/default">

<div layout:fragment="content">
Thymeleaf Cotent!!
</div>

</html>
{% endhighlight %}

이후 Controller 에서 설정한 URL 로 접근하여 default.html 레이아웃이 적용된 thymeleaf.html 페이지로 연결이 되는지 확인한다.