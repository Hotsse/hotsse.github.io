---
layout: post
title: Spring Boot Batch Query 설정 및 적용
categories: [Spring]
comments: true
---

Batch Query

비즈니스 로직을 처리하다보면 다수의 데이터를 반복적으로 INSERT 혹은 UPDATE 해야 하는 경우가 생긴다. (일반적으로는 엑셀 업로드를 통한 데이터 밀어넣기(?) 같은 경우가 있다)

이 때 Service 레벨에서 List 나 Array 타입으로 데이터를 받아온 후 반복문을 통해 처리하는게 일반적인데, 적은 양의 데이터라면 큰 문제가 없으나, 100건만 넘어가버려도 서비스 성능에 큰 부하가 걸리게 된다.  
웹서비스와 DB 간에 핑퐁을 오가며 100+건의 데이터를 전달한다는 건 서비스 입장에서는 상당한 I/O Delay 를 초래할 수 있으므로, 이를 해결하기 위한 Mybatis ExecutorType 중 Batch 에 대해 알아보자.

-------------

- DataSourceConfig.java
{% highlight java %}
package com.hotsse.sample;

import javax.sql.DataSource;

import org.apache.ibatis.session.ExecutorType;
import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.SqlSessionTemplate;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class DataSourceConfig {

	/**
	 * DataSource 객체 생성
	 *
	 * @methodName	dataSource
	 * @return
	 */
	@Bean(name = "dataSource")
	@ConfigurationProperties(prefix = "spring.datasource")
	public DataSource dataSource() {
		return DataSourceBuilder.create().build();
	}
	
	/**
	 * SqlSessionFactory 객체 생성
	 * 
	 * @methodName	sqlSessionFactory
	 * @param		dataSource
	 * @param		applicationContext
	 * @return
	 * @throws		Exception
	 */
	@Bean(name = "sqlSessionFactory")
	public SqlSessionFactory sqlSessionFactory(@Qualifier("dataSource") DataSource dataSource, ApplicationContext applicationContext) throws Exception {
		SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
		sqlSessionFactoryBean.setDataSource(dataSource);
		sqlSessionFactoryBean.setMapperLocations(applicationContext.getResources("classpath*:mybatis/mapper/**/*.xml"));
		sqlSessionFactoryBean.setTypeAliasesPackage("**.vo");
		return sqlSessionFactoryBean.getObject();
	}
	
	/**
	 * SqlSessionTemplate 객체 생성
	 * @methodName	sqlSessionTemplate
	 * @param		sqlSessionFactory
	 * @return
	 * @throws		Exception
	 */
	@Bean(name = "sqlSessionTemplate")
	public SqlSessionTemplate sqlSessionTemplate(SqlSessionFactory sqlSessionFactory) throws Exception {
		return new SqlSessionTemplate(sqlSessionFactory);
	}
	
	/**
	 * SqlSessionTemplate 객체 생성(Batch 전용)
	 * @methodName	sqlSessionTemplate
	 * @param		sqlSessionFactory
	 * @return
	 * @throws		Exception
	 */
	@Bean(name = "batchSqlSessionTemplate")
	public SqlSessionTemplate batchSqlSessionTemplate(SqlSessionFactory sqlSessionFactory) throws Exception {
		return new SqlSessionTemplate(sqlSessionFactory, ExecutorType.BATCH);
	}
}
{% endhighlight %}


- DataSourceConfig.java
{% highlight java %}
package com.hotsse.sample;

import org.apache.ibatis.session.SqlSession;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Repository;

@Repository
public class TestDao {

	@Autowired
	@Qualifier("sqlSessionTemplate")
	protected SqlSession sqlSession;
	
	@Autowired
	@Qualifier("batchSqlSessionTemplate")
	protected SqlSession batchSqlSession;
  
	// Batch 없는 버전
	public void insert() {
		for(int i=0; i<1000; i++) {
			this.sqlSession.insert("your.insert.query");
		}
		
	}
	
	// Batch 있는 버전 (월등히 빠를 것이다)
	public void insertBatch() {
		for(int i=0; i<1000; i++) {
			this.batchSqlSession.insert("your.insert.query");
		}
		this.batchSqlSession.flushStatements();
	}
}
{% endhighlight %}
