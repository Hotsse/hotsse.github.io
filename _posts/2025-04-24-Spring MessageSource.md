---
layout: post
title: Spring MessageSource
categories: [Spring]
comments: true
---

# Spring MessageSource 란?

Spring MessageSource는 **국제화(i18n)**와 다국어 메시지 처리를 지원하기 위해 제공되는 Spring의 인터페이스다.  
UI나 로그, 예외 메시지를 코드에 하드코딩하지 않고 **메시지 번들(properties 파일)**을 통해 관리할 수 있게 해준다.

---------------

## 주요 기능
1. 메시지 추출
  - 코드(key) 기반으로 메시지를 가져옴
2. 다국어 지원
  - 요청의 Locale 에 따라 다른 메시지를 반환
3. 매개변수 포맷팅
  - 메시지 내 {0}, {1} 같은 부분에 값 삽입 가능
  - 그러나 실제 DB 연결은 현재 컨텍스트에 따라 선택된 데이터소스를 통해 이루어짐
4. 기본 메시지 설정
  - 키가 없을 경우 기본 메시지를 설정 가능

---------------

## 예제 코드
아래는 글로벌 그룹사에 대한 그룹웨어 서비스를 제공하기 위해 사용자의 법인에 따라 다른 언어를 노출해야 하는 시나리오이다.

각 요청 접근 시 Interceptor 에서 사용자 토큰에서 사용자 언어를 조회하여 LocaleContextHolder 에 해당 정보를 저장한다.  
(LocaleContextHolder 는 Spring 에서 제공하는 ThreadLocale<Locale> 기반의 Context 저장소이다)

``` java
@Component
public class LocaleInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        String lang = request.getHeader("Accept-Language"); // 또는 "X-Locale", 커스텀 헤더 가능
        Locale locale = (lang != null) ? Locale.forLanguageTag(lang) : Locale.getDefault();
        LocaleContextHolder.setLocale(locale); // ThreadLocal에 저장
        return true;
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        LocaleContextHolder.clear();
    }
}
```

MessageSource 를 통해 다국어 메시지 데이터를 조회한다. (다국어 호출 시 별도의 Locale 을 지정하지 않으면 LocaleContextHolder 기준으로 다국어 언어를 조회한다)

``` java
@Autowired
private MessageSource messageSource;

public String getWelcomeMessage() {
    return messageSource.getMessage("greeting", new Object[]{"죠르디"});
}

```

각 message 에는 언어별 메시지 정보를 정의한다.

```
# messages_ko.properties
greeting=안녕하세요, {0}님!

# messages_en.properties
greeting=Hello, {0}!
```
