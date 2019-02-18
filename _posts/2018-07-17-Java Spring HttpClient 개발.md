---
layout: post
title: Java Spring HttpClient 개발
categories: [Spring]
comments: true
---

Third Party API 를 사용하기 위해서는 때론 Server to Server 통신을 해야할 때도 있다.
(Client to Server 통신은 보안 상의 이유로 많은 제약이 있기 때문에...)

httpClient 는 Spring(Java) 에서 기본적으로 제공하는 패키지로 Request 정보를 원하는 URI 에 송신하고, 통신이 성공했을 시 Response 를 응답받으며, 이를 파싱하여 통신을 완료할 수 있다.

------------

4가지의 HTTP Method

HTTP 1.1 부터 기본적인 HTTP Method 인 GET, POST 이외에도, OPTION METHOD 인 PUT, DELETE 등의 추가 메소드가 지원되고 있다.
HTTP 1.1 RESTful한 API 통신을 위한 서버를 개발한다고 하면 위의 4가지(GET, POST, PUT, DELETE) 통신에 대한 응답 모듈을 개발하여야 한다.

--------------

각 HTTP Method 별 Request 객체 생성

하기 메소드들은 각 HTTP Method 별로 통신을 하기 위한 진입 메소드이다.
(HTTP Method 별로 메소드를 구분해 놓은 이유는 아래에 적도록 하겠다)

{% highlight java %}
/**
 * <pre>
 * HTTP Method(Get) 로 Request 진행
 * </pre>
 * @methodName    : executeHttpGet
 */
private Map<String, String> executeHttpGet(String uri) throws Exception{
 
    // 메소드 변수 선언
    HttpGet httpRequest = new HttpGet();
 
    // HTTP Request 공통 모듈 실행
    Map<String, String> map = new HashMap<String, String>();
    map = executeHttpClient(httpRequest, uri, map);
 
    return map;
}
 
/**
 * <pre>
 * HTTP Method(DELETE) 로 Request 진행
 * </pre>
 * @methodName    : executeHttpDelete
 */
private Map<String, String> executeHttpDelete(String uri) throws Exception{
 
    // 메소드 변수 선언
    HttpDelete httpRequest = new HttpDelete();
 
    // HTTP Request 공통 모듈 실행
    Map<String, String> map = new HashMap<String, String>();
    map = executeHttpClient(httpRequest, uri, map);
 
    return map;
}
 
/**
 * <pre>
 * HTTP Method(POST) 로 Request 진행
 * </pre>
 * @methodName    : executeHttpPost
 */
private Map<String, String> executeHttpPost(String uri, String body) throws Exception{
 
    // 메소드 변수 선언
    HttpPost httpRequest = new HttpPost();
 
    // 바디 설정
    if(body != null) {
        HttpEntity entity = new ByteArrayEntity(body.getBytes("UTF-8"));
        httpRequest.setEntity(entity);
    }
 
    // HTTP Request 공통 모듈 실행
    Map<String, String> map = new HashMap<String, String>();
    map = executeHttpClient(httpRequest, uri, map);
 
    return map;
}
 
/**
 * <pre>
 * HTTP Method(PUT) 로 Request 진행
 * </pre>
 * @methodName    : executeHttpPut
 */
private Map<String, String> executeHttpPut(String uri, String body) throws Exception{
 
    // 메소드 변수 선언
    HttpPut httpRequest = new HttpPut();
 
    // 바디 설정
    if(body != null) {
        HttpEntity entity = new ByteArrayEntity(body.getBytes("UTF-8"));
        httpRequest.setEntity(entity);
    }
 
    // HTTP Request 공통 모듈 실행
    Map<String, String> map = new HashMap<String, String>();
    map = executeHttpClient(httpRequest, uri, map);
 
    return map;
}
{% endhighlight %}


상기 두 개의 메소드(GET, DELETE) 와 그 아래의 메소드(POST, PUT) 의 차이점은 무엇일까?
그것은 매개 변수로 body 를 받으며, Request 객체 내에 해당 body 를 삽입할 수 있다는 차이점이다.

GET과 DELETE 는 통신에 body 가 필요하지 않으므로, 객체 내에 해당 기능이 구현되어 있지 않지만,
POST과 PUT 은 setEntity(String body); 라는 기능이 구현되어 있다.

------------------

통신 및 Response 파싱

Request 객체가 생성되었다면, 적합한 URI 에 해당 정보로 통신을 취하고 응답받은 Response 객체를 파싱하여, 원하는 결과를 얻는 일만 남았다.
하기 메소드에 그에 대한 내용이 구현되어 있다.

{% highlight java %}
/**
 * <pre>
 * HTTP Client Request 공통 실행 함수
 * </pre>
 * @methodName    : executeHttpClient
 */
private Map<String, String> executeHttpClient(HttpRequestBase httpRequest, String uri, Map<String, String> map){
 
    // 클라이언트 변수 선언
    CloseableHttpClient httpClient = HttpClients.createDefault();
 
    // 대상 URI 맵핑
    httpRequest.setURI(URI.create(uri));
 
    
    /*
    통신에 계정 권한이 필요한 경우, 헤더에 Base64 인코딩된 계정정보를 보내는 방식인 BaseAuth 방식을 사용할 수 있다.
    (그외의 방법으로는 OAuth 2.0 등의 방식이 있으며, 이는 따로 검색을 해보길 바란다.
    // Base64 인코딩
    String basicAuth = Base64.encodeBase64String(("YOUR-ACCOUNT-ID" + ":" + "YOUR-ACCOUNT-PW").getBytes());
    */
    
 
    // 헤더 설정
    httpRequest.setHeader("Authorization", "Basic " + basicAuth);
    httpRequest.setHeader("Content-Type", "application/json");
 
    CloseableHttpResponse httpResponse = null;
    try {
 
        // HTTP Request 실행
        httpResponse = httpClient.execute(httpRequest);
 
        // statusCode
        int statusCode = httpResponse.getStatusLine().getStatusCode();
        map.put("statusCode", String.valueOf(statusCode));
 
        // Response 결과 분기
        if((statusCode / 100) == 2) {
            map.put("result", "OK");
        }
        else {
            map.put("result", "ERROR");
        }
 
    }
    catch(Exception e) {
        e.printStackTrace();
    }
    finally {
        // Response 및 Client 객체 세션 종료(중요!)
        closeInstant(httpResponse);
        closeInstant(httpClient);
    }
 
    return map;
}
{% endhighlight %}

상기 메소드 중에서, 특히 눈여겨 봐야할 것은 finally 구문이다.
상기 소스코드는 어느 한줄이 빠져도 통신이 제대로 되지 않기 때문에, 디버깅이 쉬우나, finally 구문의 session close 를 하지 않는다면, 세션이 종료되지 않고 지속적으로 쌓여, 서버 과부하를 초래할 수 있다.
(Open 한 Session 에 대해서는 반드시 Close 할 수 있도록 한다)

--------------------

메소드 호출(통신 실행 예제)

{% highlight java %}
public @ResponseBody Map<String, String> updateComment(@RequestParam Map<String, Object> param, ModelMap model, HttpServletRequest request, HttpServletResponse response) throws Exception{
 
    // ------- 파라미터 및 쿠키 정보 획득
    // AD 계정 로드
    HashMap<String, String> ldapMap = getMyLdapInfo(request, response);
    String ldapId = StringUtils.defaultIfEmpty(ldapMap.get("ldapId"), "");
    String ldapPw = StringUtils.defaultIfEmpty(ldapMap.get("ldapPw"), "");
 
    // 파라미터 획득 및 파싱
    String key = StringUtils.defaultIfEmpty(param.get("key").toString(), ""); // 이슈 키
    String id = StringUtils.defaultIfEmpty(param.get("id").toString(), ""); // 댓글 ID
    String comment = StringUtils.defaultIfEmpty(param.get("comment").toString(), ""); // 수정될 댓글 본문
 
    comment = comment.replaceAll("\r", "<br/>");
    comment = comment.replaceAll("\n", "<br/>");
 
    // BODY(JSON) 생성
    /*
     * {
     *     "body" : "댓글 내용"
     * }
     * */
    HashMap<String, String> jsonMap = new HashMap<String, String>();
    jsonMap.put("body", comment);
 
    JSONObject jsonObject = new JSONObject(jsonMap);
    String body = jsonObject.toJSONString();
 
    // ------- HTTP Request
    // (PUT)http://works.eduwill.net/rest/api/2/issue/{issueKey}/comment/${commentId}
    String uri = JIRAURI + "/rest/api/2/issue/" + key + "/comment/" + id;
    Map<String, String> map = executeHttpPut(ldapId, ldapPw, uri, body);
 
    return map;
}
{% endhighlight %}