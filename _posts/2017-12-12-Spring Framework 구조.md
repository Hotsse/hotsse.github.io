---
layout: post
title: Spring Framework 구조
categories: [Spring]
comments: true
---

들어가기 앞서...

스프링은 프레임워크이다. 프레임워크는 큰 프로젝트를 쉽게 구축하기 위해 구성된 정규화된 "틀"이라고 말할 수 있다. 이 틀을 이해하고 있다면 스프링의 기본적인 흐름을 이해하는데 큰 도움이 될 것이다.

-------------------

스프링 프레임워크의 구조

디렉토리 구조
<img width="188" alt="image" src="https://github.com/user-attachments/assets/0a74ef7f-82f6-4e9a-92e1-4b2502633d49">

- src\main\java : Java 소스코드가 위치
- src\main\resource : ClassPath에 위치할 자원파일(XML 파일, 프로퍼티 파일 등)이 위치
- src\main\webapp : 웹 어플리케이션의 기준 폴더로 HTML, JSP 파일 등이 위치
    - src\main\webapp\WEB-INF : 웹 설정의 핵심이 되는 web.xml을 포함하고 있음
    - src\main\webapp\WEB-INF\spring : 스프링 설정 파일
    - src\main\webapp\WEB-INF\views : 뷰(HTML, JSP) 파일
- src\main\webapp\resource : 웹 정적파일(.css, .js) 등이 위치
