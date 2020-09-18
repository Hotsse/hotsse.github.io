---
layout: post
title: Spring @PostConstruct, @PreDestroy
categories: [Spring]
comments: true
---

@PostConstruct 어노테이션

@PostConstruct 는 의존성 주입(쉽게 말해 Autowired)이 이루어진 후 초기화를 수행하는 메서드 어노테이션이다. @PostConstruct가 붙은 메서드는 프레임워크에 의해 Bean 에 등록된 후 수행된다.  
이 메서드는 다른 리소스에 의해 호출되지 않는다해도 수행된다.

- 해당 Bean 의 초기화 작업에 의존성이 있는 다른 Bean 의 리소스가 필요한 경우(ex.DB 조회를 통한 데이터 초기화)
- 서비스 실행 시 단 1회 실행됨을 보장하기 위한 경우

-------------

@PreDestroy 어노테이션

@PreDestroy 어노테이션은 @PostConstruct 와 비슷한 맥락으로, Bean 의 마지막 소멸 단계에서 Bean 을 제거하기 전에 처리해야 할 작업이 있는 경우 수행하는 메서드 어노테이션이다.

-------------

테스트

서비스 실행 시, DB에서 게시판의 카테고리 리스트 데이터를 조회하여 필드에 저장하고, Bean 소멸 시 이 필드값을 소멸시키는 테스트를 진행해 보자.

- board.xml
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="board">

	<select id="getBoardCategories" resultType="BoardCategoryVO">
		SELECT cat_id as catId
			cat_nm as catNm
		FROM board_category
		WHERE use_yn = 'Y'
		ORDER BY sort ASC
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
	
	public List<BoardCategoryVO> getBoardCategories() throws Exception {
		System.out.println("run BoardDao::getBoardCategories()");
		return this.sqlSession.selectList("board.getBoardCategories");
	}
}
{% endhighlight %}

- BoardService.java
{% highlight java %}
@Service
public class BoardService {

	@Autowired
	private BoardDao boardDao;

	private final List<BoardCategoryVO> categories = null;

	// Bean 생성 > 의존성 주입 > @PostConstruct 실행
	@PostConstruct
	public void init() throws Exception {
		System.out.println("run BoardService::init()");
		categories = this.boardDao.getBoardCategories(); // 의존성이 있는 다른 Bean 의 리소스를 사용할 수 있음
	}

	// @PreDestroy 실행 > Bean 제거
	@PreDestroy
	public void detsroy() throws Exception {
		System.out.println("run BoardService::destroy()");
		categories = null;
	}

	public List<BoardCategoryVO> getCategories() throws Exception {
		return this.categories;
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
	void contextLoads() throws Exception {

		List<BoardCategoryVO> categories = this.boardService.getCategories();
		System.out.println(categories.toString());
	}
}
{% endhighlight %}

테스트 실행 시, 별도의 호출이 없더라도 @PostConstruct 의 메서드가 실행되어, DB 조회와 콘솔이 찍히는 부분을 확인 할 수 있으며, 반대로 서비스 종료 시 @PreDestroy 의 메서드가 수행되는 부분을 확인할 수 있다.
서비스 실행 도중 변동이 없음이 보장되는 DB 데이터나 External API 데이터 같은 경우 위와 같이 서비스 실행 시에 일회성으로 데이터를 조회하여 서비스 전체의 성능을 향상 시키는 방법을 도모할 수 있다.  
(Caching 개념으로 사용할 수 있으나, 남발하는 건 역시 좋지 않다.)

-------------

XML 으로 Bean 을 관리하는 경우

어노테이션을 사용하지 않고 XML 으로 Bean 을 관리하는 경우 아래와 같이 할 수 있다.

{% highlight xml %}
<!-- ... -->

<!-- init-method, destroy-method 에 로직 상에 존재하는 메서드명을 작성 -->
<bean id="boardService"
	class="com.quicksample.nexon.board.service.BoardService"
	init-method="init"
	destroy-method="destroy">
</bean>

<!-- ... -->
{% endhighlight %}