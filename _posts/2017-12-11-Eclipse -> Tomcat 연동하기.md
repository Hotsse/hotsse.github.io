---
layout: post
title: Spring Eclipse <-> Tomcat 연동하기
categories: [Spring]
comments: true
---

들어가기 앞서...

이클립스 IDE로 개발한 서버를 톰캣에서 바로 구동하기 위해서는 연동 작업이 필요하다.
이클립스와 톰캣이 설치되어 있는 환경을 기준으로 포스팅 하므로 설치되어 있지 않다면 링크를 통해 설치하길 바란다.

-------------

테스트 프로젝트 생성

서버에 적재하기 위한 테스트 프로젝트를 생성해 보자.

1. New > Spring Legacy Project > Spring MVC Project 를 통해 새 프로젝트를 생성한다.
2. 자신의 개발 환경에 맞는 버전 설정을 진행한다. (자세한 건 다음 포스팅에서...)

-------------

이클립스 설정

실행환경 설정
1. 이클립스의 Window > Preferences 탭을 들어간다.
<img width="626" alt="image" src="https://github.com/user-attachments/assets/0b524191-70ee-4c54-a897-193cf435e0ef">
2. Runtime Environment 를 검색하여 Add 버튼을 누른다.
3. Apache Tomcat v9.0 > Browse... 탭을 들어간다.
<img width="642" alt="image" src="https://github.com/user-attachments/assets/1fa92c1d-1733-4217-b31e-cec73a524142">

4. 연동을 위해 Browse에서 아파치 톰캣의 설치 경로를 지정해 준다.

서버 추가

1. Servers 탭 우클릭 > New > Server 탭으로 메뉴를 띄운다.
2. Apache Tomcat v9.0 선택, 서버 이름을 설정 후 Next 로 진행한다.
<img width="517" alt="image" src="https://github.com/user-attachments/assets/a9f2382a-0625-4cbd-ba39-89891d221161">
3. 현재 가용(Avaliable) 프로젝트 중에 서버에 올리고 싶은 프로젝트를 Add 한다.

서버 설정
1. Severs 탭에 새로 추가된 서버를 더블 클릭한다.
2. 입맛에 맞게 서버 설정을 수정한다.
    - Overview 탭의 Ports 메뉴를 통해 기본 포트를 설정할 수 있다.
    - Modules 탭의 메뉴를 통해 프로젝트를 서버에 올리고 내릴 수 있다.
    - <img width="750" alt="image" src="https://github.com/user-attachments/assets/e2ad5228-48b8-4fc0-821d-a83f0c131439">

  
3. 일관적인 테스트를 위해 Modules > Edit 메뉴를 통해 적재된 프로젝트의 Path를 "/" 로 설정하자.

-------------

구동 해보기(Hello World!)

서버 설정 및 실행
1. Servers 탭에서 서버 우클릭 > Start 메뉴로 서버를 실행시킨다.

테스트

<img width="450" alt="image" src="https://github.com/user-attachments/assets/8be4b94e-a4bc-45ed-af68-22f1eabf051c">
로컬호스트 톰캣서버에 접속해서 서버가 제대로 열렸는지 확인해 보자. Hello world! 가 당신을 반겨준다면 당신은 이제 Spring Framework 를 통해 웹 어플리케이션을 개발할 준비를 성공적으로 마친 것이다.
