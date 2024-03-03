---
layout: post
title: PWA 를 native app 으로 빌드하기
categories: [App]
comments: true
---

PWA 를 Android App 으로 변환하기

Trusted Web Activity 란?
- Android App 내에서 풀스크린 웹 콘텐츠를 표현할 수 있는 방법을 제공하는 기술.
- 신뢰할 수 있는 PWA 사이트를 WebView 로 감싼 Android App 으로 감싸서 PlayStore 에 배포 가능한 형태로 변환할 수 있다.
- PWA <-> TWA 사이에서 fingerprint 값을 공유하며, 일치하지 않는(인증되지 않은 앱) 경우는 URL UI 가 노출되는 등 풀스크린을 제공하지 않는다.

------------

PWABuilder 로 TWA 변환 해보기
- https://www.pwabuilder.com/

------------

bubblewrap 로 TWA 변환 해보기
- GoogleChromeLabs/bubblewrap 라이브러리를 사용하여 프로젝트를 빌드한다.
- bubblewrap lib. 설치
``` bash
npm install -g bubblewrap
```
- bubblewrap 으로 TWA 프로젝트 생성
``` bash
bubblewrap init --manifest https://your-pwa.com/manifest.json
```
  - 초기 셋팅인 경우 빌드 환경의 Java Path 와 SDK Path 연결해야 한다.
  - Android App 변환을 위한 기본정보를 요구하나 대부분 manifest.json 기반으로 기본값을 제공한다.
  - Signing Key 가 없는 경우 새로 .keystore 파일을 생성해 준다.
- SHA256 fingerprint 획득
``` bash
keytool -list -v -keystore [keystore 파일명]
```
- SHA256 fingerprint 를 PWA 프로젝트 assetlinks.json 에 등록
  - fingerprint 를 https://your-pwa.com/.well-known/assetlinks.json 에 등록한다.
 ```json
[{
      "relation": ["delegate_permission/common.handle_all_urls"],
      "target": {
        "namespace": "android_app",
        "package_name": "com.your-pwa.twa",
        "sha256_cert_fingerprints": ["INSERT_YOUR_SHA256_FINGERPRINT"]
      }
    }]
```
- TWA 프로젝트 빌드
```bash
bubblewrap build
```

------------

PWA 를 iOS App 으로 변환

- iOS 환경에서는 순수 PWA 만으로 우리가 원하는 목표를 달성할 수 없음을 깨달았다
- Android 진영의 TWA 처럼 PWA 를 iOS App 으로 변환하는 것은 따로 지원하지 않는 것으로 확인했다.
  - PWA 개념 자체가 구글에서 미는거다보니 iOS 에서 소극적으로 지원하고 있음.
- PWA 를 iOS native-app WebView 로 Warpping 후, app 형태로 직접 구현하여 제공하는 것이 유일한 방법이다.
  - 그러나, PWA 를 iOS App > WebView 로 감싼 형태에서는 PWA Web Push API 가 동작하지 않으므로, 순수 Web Push Notification 사용이 불가능하다.
 
Wrapping 전략1. Swift 로 iOS 앱 직접 개발
- 개발 환경 구성
  - Xcode(IDE)
  - Swift(Language)
  - CocoaPods(lib. dep.)
- 샘플 프로젝트(https://github.com/khmyznikov/ios-pwa-wrap)
- PWA사이트에서 알람권한신청 → native app 영역으로 권한신청에 대한 메시지 발송 → 메시지 수신한 native 영역에서 iOS notification 권한 허용 요청 및 획득 가능

Wrapping 전략2. React Native 로 개발
- 추후 업데이트

Wrapping 전략 etc..
- ionic framework
  - capacitor
  - apache cordova
- flutter

------------

App 빌드 구조에 대한 결론
<img width="948" alt="image" src="https://github.com/Hotsse/hotsse.github.io/assets/23256138/e461a5d8-f380-4d55-aace-9f0f8bbf4ffa">

------------

결론 : RN 기반으로 최소한의 native 기능을 제공하고, 메인 서비스는 WebView 안에서 PWA 규격에 맞게 기능을 제공한다.
- RN 을 통해 모바일 환경별 native app 구성에 대한 프로세스를 통일
- 메인 서비스는 WebView 내 웹서비스로 구성하여 조직 주요 기술스택을 따름
- 웹서비스는 PWA 로 구현하여 오프라인 작동 등의 장점도 수용

------------

참조
- https://developer.chrome.com/docs/android/trusted-web-activity?hl=ko
- https://developer.chrome.com/docs/android/trusted-web-activity/quick-start?hl=ko
- https://tunapanini.tistory.com/entry/Bubblewrap-Trusted-Web-ActivityTWA-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EB%A5%BC-%ED%86%B5%ED%95%B4-Progressive-Web-AppPWA%EC%9D%84-APK-%ED%8C%8C%EC%9D%BC%EB%A1%9C-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0
- https://docs.pwabuilder.com/#/builder/app-store?id=building-your-app
- https://velog.io/@gnwjd309/iOS-WKWebView
- https://velog.io/@kerri/Xcode-CocoaPods%EC%BD%94%EC%BD%94%EC%95%84%ED%8C%9F-%EC%84%A4%EC%B9%98-%EB%B0%8F-%EC%82%AC%EC%9A%A9-%EB%B0%A9%EB%B2%95
- https://ionicframework.com/
- https://capacitorjs.com/
- https://cordova.apache.org/
