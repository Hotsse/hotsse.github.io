---
layout: post
title: Push Notification 설정 구성하기
categories: [App]
comments: true
---

PC, Android 환경 설정

https://console.firebase.google.com/u/1/(firebase cloud messaging)
- 프로젝트 생성
- 프로젝트 > 프로젝트 설정 > 일반 > 내 앱 > 앱추가
  - 웹 앱(PC, Android 용) 등록
  - 등록 시 제공받은 앱ID(key) 로 인증하며 npm 으로 연동 가능
 
------------

iOS 환경 설정(FCM based)

https://developer.apple.com/
- 계정 > 프로그램 리소스
- Keys
  - iOS 앱으로 Push 를 날릴 수 있는 권한(Apple Push Notification service)을 가진 key 를 생성한다.
 
https://console.firebase.google.com/u/1/(firebase cloud messaging)
- 프로젝트 > 프로젝트 설정 > 일반 > 내 앱 > 앱 추가
  - Apple 앱(iOS 용) 등록
  - 등록시 제공받은 .plist 파일을 iOS 프로젝트 root 에 추가
- 프로젝트 > 프로젝트 설정 > 클라우드 메시징
  - Apple 앱 구성
    - apple developer 에서 apns key 발급 시 획득한 아래의 정보를 등록한다.
      - APN 인증키 파일(.p8)
      - 키 ID
      - 팀 ID

------------

참조
- https://firebase.google.com/docs/cloud-messaging/ios/first-message?hl=ko
- https://developer.apple.com/documentation/usernotifications/registering_your_app_with_apns
- https://developer.apple.com/documentation/usernotifications#//apple_ref/doc/uid/TP40008194-CH8-SW1
- https://burgerkinghero.tistory.com/1
- https://dokit.tistory.com/49
