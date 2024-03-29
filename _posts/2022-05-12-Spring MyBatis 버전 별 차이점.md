---
layout: post
title: MyBatis 버전 별 차이점
categories: [Spring]
comments: true
---

MyBatis 를 통한 실행 결과 획득

MyBatis 를 통해 쿼리 실행 결과를 Spring 영역으로 가져오는 방법은 크게 4가지가 있다.
- select : 실행결과를 parameter 인 ResultHandler 에 반환
- selectMap : 실행결과를 Map 형태로 반환
- selectOne : 실행결과를 Object 형태로 반환
- selectList : 실행결과를 List 형태로 반환

------------

버전 별 차이

mybatis-3.0.6 의 스펙은 아래와 같다.
- void select()
- Map selectMap()
- Object selectOne()
- List selectList()

mybatis-3.1.0 의 스펙은 아래와 같다.
- void select()
- <K,V> Map<K,V> selectMap()
- <T> T selectOne()
- <E> List<E> selectList()

mybatis-3.0.6 까지는 반환형이 Object 나 List 의 형태여서 DAO 나 Mapper 에서 casting 로직이 별도로 필요했으나, mybatis-3.1.x 버전이 되면서 POJO 형태로 데이터를 반환받는 경우 Generic Type 을 적용하여 별도의 로직 없이 쿼리 결과를 반환형에 맞게 제공받을 수 있게 되었다.

``` java
// 과거
public FooVO foo() {
	return (FooVO) fooSqlSession.selectOne("foo");
}

public List<FooVO> foolist() {
	return (List<FooVO>) fooSqlSession.selectList("foolist"); // casting 하지 않으면 type safety warning 발생
}
```

``` java
// 현재
public FooVO foo() {
	return fooSqlSession.selectOne("foo");
}

public List<FooVO> foolist() {
	return fooSqlSession.selectList("foolist");
}
```

여담이지만, Generic Type 이 추가된 J2SE 5(Java 1.5) 는 2004년에 나왔는데, MyBatis 진영은 Generic Type 을 2012년에 와서야 적용했다는 사실은 조금 생각해봄직 하다.

참조
- https://javadoc.io/doc/org.mybatis/mybatis/3.0.6/org/apache/ibatis/session/SqlSession.html
- https://javadoc.io/doc/org.mybatis/mybatis/3.1.0/org/apache/ibatis/session/SqlSession.html
