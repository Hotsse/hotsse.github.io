---
layout: post
title: Spring Framework 개발환경 구축하기
categories: [Spring]
comments: true
---

스프링(Spring) 개발환경

스프링 개발환경을 구축하기 위해서는 다음과 같은 소프트웨어의 설치가 필요하다.
- JDK(JRE 포함)
- 이클립스(Eclipse)
- STS 플러그인
- 아파치 메이븐(Apache Maven)
- 아파치 톰캣(Apache Tomcat)

Java 개발자라면 JDK 와 이클립스는 이미 설치되었을 것이라 가정하고 생략한다.

-------------

이클립스 STS 플러그인 설치하기

STS 란?

STS(Spring Tool Suite)은 Spring Framework 기반 개발을 지원하는 통합 개발 환경이자 도구이다. IDE를 추가로 설치하는 대신 해당 환경을 플러그인을 통해 이클립스에서 실행할 수 있다.

STS 플러그인 설치하기

1. 이클립스에서 Help > Eclipse Marketplace... 에 들어간다.
<img width="798" alt="image" src="https://github.com/user-attachments/assets/6accf4f0-0db9-4746-b1cb-5257465b301f">
2. 검색창에 STS 를 입력하여 해당 플러그인을 검색한다.
3. 절차에 따라 플러그인을 설치한다.
<img width="719" alt="image" src="https://github.com/user-attachments/assets/3b8e90e0-1de0-49a1-9a18-de59cc1fdf6b">

설치 확인

프로젝트 탐색기 우클릭 > New > Project... > Spring 검색 을 통해 Spring 관련 프로젝트 모델이 있음을 확인한다.  
(Spring Legacy Project > Spring MVC Project 모델로 프로젝트를 생성)

-------------

메이븐(Maven) 설치하기

메이븐이란?

아파치 메이븐(Apache Maven)은 아파치 소프트웨어 재단에서 개발한 Java용 프로젝트 관리 도구이다. 아파치 라이선스로 배포되는 오픈 소스 소프트웨어이다.

메이븐 설치하기
아파치 메이븐 공식 홈페이지 링크(메이븐 다운로드) : https://maven.apache.org/download.cgi
- 메이븐 압축 파일을 다운로드하고 적절한 경로에 압축을 푼다.

환경변수 설정

환경변수는 OS 상에서 동작하는 응용소프트웨어가 참조하기 위한 설정들이다. Java가 OS 상에서 원활하게 구동되기 위해서는 환경변수의 설정이 필요하다.
1. 컴퓨터 > 속성 > 고급 시스템 설정 > 고급 > 환경 변수
2. 시스템 변수에 아래와 같이 설정한다
    - M2_HOME : "C:\Program Files\Apache Software Foundation\apache-maven-3.5.0" (메이븐 설치 경로)
    - PATH : ";%M2_HOME%\bin" 추가

설치 확인

cmd 창에서 [mvn -version] 명령어를 입력 시에, 메이븐의 버전에 대한 정보가 출력된다면 올바르게 설치된 것이다.

-------------

톰캣(Tomcat) 설치하기
톰캣이란?
아파치 톰캣(Apache Tomcat)은 아파치 소프트웨어 재단에서 개발한 서블리시 컨테이너(웹 컨테이너)만 있는 WAS(Web Application Server)이다. 톰캣은 웹 서버와 연동하여 실행할 수 있는 자바 환경을 제공하여 JSP와 Java Servlet이 실행될 수 있는 환경을 제공한다.

톰캣 설치하기
아파치 톰캣 공식 홈페이지 링크(톰캣 다운로드) : https://tomcat.apache.org/download-10.cgi
<img width="794" alt="image" src="https://github.com/user-attachments/assets/7c8bcc35-18ad-49b4-8763-d45b505bb140">


1. 개발환경에 맞춰 톰캣 설치파일을 다운로드 한다. Windows OS의 경우 64-bit Windows Service Installer를 다운로드 받는것이 편하다.
2. 압축을 풀고, 설치 프로그램을 통해 설치할 수 있다. 톰캣은 Java를 이용하기 때문에, 미리 설치된 JRE의 경로를 지정해야 한다.

설치 확인

톰캣 구동 후 로컬호스트에 접속하면 Example 페이지가 열리는 것을 확인할 수 있다.
