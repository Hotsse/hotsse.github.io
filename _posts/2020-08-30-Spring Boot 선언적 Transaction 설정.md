---
layout: post
title: Spring Boot 선언적 Transaction 설정
categories: [Spring]
comments: true
---

Transaction(트랜잭션) 이란?

데이터베이스의 상태를 변경시키는 작업 또는 한번에 수행되어야 하는 연산들의 단위이다.

트랜잭션 단위의 작업이 끝나면 모든 작업이 동시에 Commit 혹은 Rollback 되어야 한다.

-------------

Spring Boot 에서의 Transaction 설정

Spring 에서는 비즈니스 로직에서 선언적 트랜잭션을 지원하고, XML 파일 혹은 Java Code Config를 통해 초기 설정을 할 수 있다.
하지만, Spring Boot 는 초기 설정 없이 바로 비즈니스 로직에서 어노테이션을 추가하는 것으로 사용이 가능하다.

|옵션|내용|
|------------------------------|------------------------------|
|propagation|트랜잭션 동작 도중 다른 트랜잭션을 호출할 때, 어떻게 처리할 것인지 지정하는 옵션|
|isolation|트랜잭션에서 일관성이 없는 데이터 허용 수준을 설정|
|noRollbackFor|특정 예외 발생 시, rollback 하지 않음을 설정|
|rollbackFor|특정 예외 발생 시, rollback 하도록 설정|
|timeout|지정한 시간 내에 메소드 수행이 완료되지 않으면, rollback 하도록 설정|
|readOnly|읽기 전용 트랜잭션으로 설정|

-------------

테스트

트랜잭션 설정에는 많은 예시가 있지만, 가장 간단한 트랜잭션 내에서 Exception 이 발생했을 시, 모든 변경사항을 Rollback 하는 테스트를 진행해 보자.

- board.xml
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="board">

	<insert id="insertBoard" parameterType="BoardVO">
		INSERT INTO board (
			title
			, content
			, view_cnt
			, use_yn
			, reg_id
			, reg_dtt
		)
		VALUES (
			#{title}
			, #{content}
			, 0
			, 'Y'
			, #{regId}
			, now()
		)
	</insert>
	
	<select id="badSqlQuery">
		asdfasdfasdfasdfasdf <!-- 문법에 맞지 않는 쿼리 -->
	</select>
		
</mapper>
{% endhighlight %}

- BoardDao.java
{% highlight java %}
@Repository
public class BoardDao {
	
	@Autowired
	@Qualifier("sqlSessionTemplate")
	protected SqlSession sqlSession;
	
	public int insertBoard(BoardVO board) throws Exception {
		return this.sqlSession.insert("board.insertBoard", board);
	}
	
	public void badSqlQuery() throws Exception {
		this.sqlSession.selectOne("board.badSqlQuery");
	}
}
{% endhighlight %}

- BoardService.java
{% highlight java %}
@Service
public class BoardService {

	@Autowired
	private BoardDao boardDao;
	
	@Transactional(rollbackFor = Exception.class) // 해당 메서드 내 Exception 발생 시, 모든 변경사항을 Rollback 처리
	public int insertBoardTransaction(BoardVO board) throws Exception {
		
		int count = this.boardDao.insertBoard(board);
		this.boardDao.badSqlQuery(); // Exception 발생
		
		return count;
	}
	
	public int insertBoard(BoardVO board) throws Exception {
		
		int count = this.boardDao.insertBoard(board);
		this.boardDao.badSqlQuery(); // Exception 발생
		
		return count;
	}
	
}
{% endhighlight %}

- SampleApplicationTests.java
{% highlight java %}
@SpringBootTest
class SampleApplicationTests {
	
	@Autowired
	private BoardService boardService;

	@Test
	void transactionTest() throws Exception {
		BoardVO board = new BoardVO();
		board.setTitle("트랜잭션제목");
		board.setContent("트랜잭션내용");
		board.setRegId("트랜잭션아이디");
		
		int count = this.boardService.insertBoardTransaction(board);
		System.out.println("count = " + count);
	}
	
	@Test
	void nonTransactionTest() throws Exception {
		BoardVO board = new BoardVO();
		board.setTitle("'논'트랜잭션제목");
		board.setContent("'논'트랜잭션내용");
		board.setRegId("'논'트랜잭션아이디");
		
		int count = this.boardService.insertBoard(board);
		System.out.println("count = " + count);
	}

}
{% endhighlight %}

테스트 실행 시, 트랜잭션 처리가 되어 있는 "트랜잭션" 의 데이터는 Rollback 되어 insert 되지 않았으며,  
트랜잭션 처리가 되어있지 않은 "'논'트랜잭션" 의 데이터는 Exception 과는 무관하게 insert 가 실행된 부분을 확인할 수 있다.

-------------

마치며

Spring 에서 지원하는 선언적 트랜잭션을 통해 데이터 무결성을 지킬 수 있으며, 용도에 따라서는 외부 DB 서비스의 통신 중단에 대한 서비스 안정성 유지의 목적으로도 사용될 수 있다.  
100% 완전한 유효성 입력 값이나 쿼리는 존재하지 않으므로 필요에 따라 트랜잭션을 적용하는 것은 필수라고 생각한다.
