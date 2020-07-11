---
layout: post
title: Spring Boot Interceptor 관리
categories: [Spring]
comments: true
---

Interceptor 란?

정식 명칭은 Handler Intercpetor(핸들러 인터셉터) 이다. 이 Interceptor 는 URL 기반으로 일부 혹은 전체의 클라이언트 요청을 가로채서 공통된 비즈니스 로직을 실행하는 역할을 한다.

설정에 따라 클라이언트의 요청이 컨트롤러에 가기 전과, 응답이 클라이언트에 가기 전에 실행된다. 즉, Intercpetor 는 DispatcherServlet 이 컨트롤러를 요청하기 전, 후에 요청과 응답을 가로채서 비즈니스 로직을 통해 데이터를 검증, 가공할 수 있도록 해 준다.

-------------

Spring Boot 에서 Interceptor 구현

Spring Boot 에서는 어노테이션, Java Code 기반으로 손쉽게 Intercpetor 를 구현할 수 있다.

- CommonInterceptor.java
{% highlight java %}
package com.nexon.quicksample.core.interceptor;
 
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
 
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;
 
@Component
public class CommonInterceptor implements HandlerInterceptor {
     
    @Override
    public boolean preHandle(HttpServletRequest req, HttpServletResponse res, Object handler) throws Exception {
         
        System.out.println("CommonInterceptor::preHandle");
         
        return true;       
    }
     
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
 
        System.out.println("CommonInterceptor::postHandle");
    }
     
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
 
        System.out.println("CommonInterceptor::afterCompletion");
    }

}
{% endhighlight %}
우선 Interceptor 의 기본 골격인 interface HandlerInterceptor 를 구현하는 것으로 Interceptor 를 생성한다.

- ContextConfig.java
{% highlight java %}
package com.nexon.quicksample.core.config;
 
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
 
import com.nexon.quicksample.core.interceptor.CommonInterceptor;
 
@Configuration
public class ContextConfig implements WebMvcConfigurer {
     
    @Autowired
    private CommonInterceptor commonInterceptor;
     
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(commonInterceptor);
    }
     
}
{% endhighlight %}
interface WebMvcConfigurer 를 상속받은 Configuration 구현체에서 addInterceptors() 메서드를 Override 하여 interceptorRegistery 에 앞서 생성한 interceptor 를 추가하는 것으로, Spring Boot 에 Interceptor 추가가 가능하다.

-------------

테스트

간단한 비즈니스 로직과 함께 로그를 확인하여, Intercpetor 의 실행 여부와 프로세스 흐름을 확인하자.

- HomeController.java
{% highlight java %}
package com.nexon.quicksample.home;
 
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;
 
@RestController
@RequestMapping({"/", "/home"})
public class HomeController {
     
    @RequestMapping("/hello")
    public String hello() {
         
        System.out.println("HomeController:hello");
         
        return "Hello World!";
    }
     
}
{% endhighlight %}

결과는 다음과 같은 순서로 출력될 것이다.

{% highlight text %}
CommonInterceptor::preHandle
HomeController::hello
CommonInterceptor::postHandle
CommonInterceptor::afterCompletion
{% endhighlight %}

-------------

postHandle 과 afterCompletion 의 차이(추가)

postHandle 과 afterCompletion 은 Response(응답)이 클라이언트로 전송되기 전에 실행된다는 점은 같지만 차이가 존재한다.

순서 : postHandle -> afterCompletion 의 순으로 실행된다.  
에러 여부(중요*) : 만약 비즈니스 로직 처리 중에 에러가 발생된다면 afterCompletion 은 실행되지만 postHandle 은 실행되지 않는다.  
(afterCompletion 은 어떠한 상황에서도 실행된다고 생각할 수 있다.)