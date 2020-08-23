---
layout: post
title: Spring Boot Thymeleaf 연동
categories: [Spring]
comments: true
---

Thymeleaf 란?

Thymeleaf(타임리프)는 modern server-side Java Template Engine 이다.

시간이 없어서 쉽게 말하자면 Spring 에서의 컨트롤러와 그에 대응하는 View Page(.html, .jsp) 을 이어주는 역할을 한다.

-------------

Spring Boot 에서 Thymeleaf 연동

Spring Boot 에서는 어노테이션, Java Code 기반으로 손쉽게 Thymeleaf 를 연동할 수 있다.

- ContextConfig.java
{% highlight java %}
package com.nexon.quicksample.core.config;
 
import org.springframework.context.annotation.Configuration;
import org.thymeleaf.spring5.SpringTemplateEngine;
import org.thymeleaf.templateresolver.ClassLoaderTemplateResolver;
 
@Configuration
public class ContextConfig implements WebMvcConfigurer {
     
    @Bean
    public SpringTemplateEngine templateEngine() {
        SpringTemplateEngine templateEngine = new SpringTemplateEngine();
        templateEngine.setTemplateResolver(thymeleafTemplateResolver());
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
Spring 에서 Thymeleaf 가 정상적으로 연동될 수 있도록 기본적인 옵션을 설정하고, 이를 Bean 으로 등록하여 모든 컴포넌트에서 불러올 수 있도록 한다.

-------------

테스트

간단한 비즈니스 로직과 함께 Thymeleaf 를 연동하여, Thymeleaf 의 실행 여부를 확인하자.

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
         
        return "thymeleaf"; // /resources/templates/"thymeleaf.html" 을 뷰 페이지로 반환한다.
    }
     
}
{% endhighlight %}

- /resources/templates/thymeleaf.html

{% highlight html %}
<html>
<head>
<title>Thymeleaf Page</title>
</head>
<body>
Thymeleaf 가 연동된 html 페이지 입니다.
</body>
</html>
{% endhighlight %}

이후 Controller 에서 설정한 URL 로 접근하여 thymeleaf.html 페이지로 연결이 되는지 확인한다.