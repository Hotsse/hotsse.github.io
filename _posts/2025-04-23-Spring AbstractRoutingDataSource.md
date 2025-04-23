---
layout: post
title: Spring AbstractRoutingDataSource
categories: [Spring]
comments: true
---

# AbstractRoutingDataSource 란?

AbstractRoutingDataSource는 Spring Framework에서 제공하는 동적 데이터소스 라우팅을 위한 추상 클래스다. 쉽게 말해, 여러 데이터소스를 등록해두고 런타임에 상황에 따라 적절한 데이터소스를 선택해주는 역할을 한다.

---------------

# 주요 역할
1. 다수의 DataSource 관리
  - 하나의 애플리케이션에서 여러 DB 연결을 사용할 수 있도록 함 (예: Master/Slave 구조, 멀티 테넌시 등)
2. 현재 컨텍스트에 맞는 DataSource 결정
  - determineCurrentLookupKey() 메서드를 오버라이딩하여, 현재 어떤 데이터소스를 사용할지 결정함
3. 실제 라우팅 처리
  - 내부적으로는 DataSource 인터페이스를 구현하고 있어서, 마치 일반 DataSource처럼 사용할 수 있음
  - 그러나 실제 DB 연결은 현재 컨텍스트에 따라 선택된 데이터소스를 통해 이루어짐


# 동작 흐름 요약
1. 여러 개의 DataSource 를 Map 형태로 등록 (targetDataSources)
2. determineCurrentLookupKey() aㅔ서드가 호출되어 현재 컨텍스트의 키를 반환
3. 그 키에 해당하는 DataSource 를 찾아서 실제 쿼리 수행 시 사용


# 주로 사용되는 사례
- 읽기/쓰기 분리
- 멀티 테넌시
- 샤딩/분산 DB 구성
- 다중 지역 DB 연결 등

---------------

# 예제 코드
아래는 그룹사의 그룹웨어 서비스의 입장에서 하나의 서비스가 각 법인마다 다른 DataSource 에 연결해야 하는 시나리오이다.

각 요청 접근 시 Interceptor 에서 사용자 토큰에서 사용자의 소속 법인을 조회하여 ContextHolder 패턴을 통해 ThreadLocal 에 소속 법인 정보를 저장한다.

``` java
public class CorporationContextHolder {
    private static final ThreadLocal<String> contextHolder = new ThreadLocal<>();

    public static void setCorpCode(String corpCode) {
        contextHolder.set(corpCode);
    }

    public static String getCorpCode() {
        return contextHolder.get();
    }

    public static void clear() {
        contextHolder.remove();
    }
}
```

``` java
@Component
public class CorporationContextInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        String corpCode = request.getHeader("X-Corp-Code"); // 또는 토큰 파싱 등
        CorporationContextHolder.setCorpCode(corpCode);
        return true;
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        CorporationContextHolder.clear();
    }
}
```

DataSource 접근 시 사용자의 법인코드에 대해 동적 라우팅을 진행한다.

``` java
public class CorporationRoutingDataSource extends AbstractRoutingDataSource {
    @Override
    protected Object determineCurrentLookupKey() {
        return CorporationContextHolder.getCorpCode(); // 현재 법인코드 반환
    }
}
```

각 법인에 대한 DB 정보와 CustomRoutingDataSource 설정 정보를 정의한다.

``` java
@Configuration
public class DataSourceConfig {

    @Bean
    public DataSource dataSource() {
        Map<Object, Object> targetDataSources = new HashMap<>();
        targetDataSources.put("CORP_A", dataSourceForCorpA());
        targetDataSources.put("CORP_B", dataSourceForCorpB());

        CorporationRoutingDataSource routingDataSource = new CorporationRoutingDataSource();
        routingDataSource.setTargetDataSources(targetDataSources);
        routingDataSource.setDefaultTargetDataSource(dataSourceForCorpA()); // default 설정
        routingDataSource.afterPropertiesSet();

        return routingDataSource;
    }

    public DataSource dataSourceForCorpA() {
        return DataSourceBuilder.create()
            .url("jdbc:mysql://hostA:3306/db_corpA")
            .username("userA")
            .password("passA")
            .driverClassName("com.mysql.cj.jdbc.Driver")
            .build();
    }

    public DataSource dataSourceForCorpB() {
        return DataSourceBuilder.create()
            .url("jdbc:mysql://hostB:3306/db_corpB")
            .username("userB")
            .password("passB")
            .driverClassName("com.mysql.cj.jdbc.Driver")
            .build();
    }
}
```

