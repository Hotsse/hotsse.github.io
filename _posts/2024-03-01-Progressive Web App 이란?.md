---
layout: post
title: pwa
categories: [App]
comments: true
---

PWA(Progressive Web Application)

- 웹을 기반으로 최대한 앱의 사용자 경험을 제공하기 위한 표준 기술/규약들의 집합
- 웹과 native-app의 기능 모두의 이점을 갖도록 특정 기술과 표준 패턴을 사용해 개발된 웹 기반의 앱.
- HTML/CSS/JS 를 포함한 일반 웹 기술들을 사용하여 만들어지며, 표준을 준수하는 브라우저를 사용하는 어떠한 플랫폼에서라도 동작하도록 고안되었다.
- 오프라인 작업, 푸시 알림, 장치 하드웨어 접근, 데스크톱과 모바일 장치의 접근 등으로 native-app 과 유사한 UX를 창출을 가능케 한다.
- 웹사이트이기 때문에 애플 앱 스토어나 구글 플레이와 같은 디지털 배급 시스템을 통해 웹 앱을 설치할 필요 없이, 사이트에서 native-app 형태로 설치가 가능하다.

------------

PWA 의 장단점

장점
- native-app 보다 좋은 점
  - Web/AOS/iOS 등 다수의 플랫폼에 대해 하나의 코드로 동일한 서비스를 개발할 수 있다. (개발/유지비용이 저렴하다)
  - 스토어에서 설치할 필요가 없다.
  - app 배포 시, 사용자가 수동으로 업데이트 할 필요가 없다.
  - 일반 mobile-web 보다 좋은 점
- native-app 과 같은 사용자 경험을 제공할 수 있다.
  - Service Worker 기반의 캐싱 기능을 통해 오프라인, 저속도 네트워크에서도 비교적 쾌적한 기능을 제공할 수 있다.

단점
- native-app 보다 안좋은 점
  - native-app 에 비해 일부 하드웨어 기능에 접근하는데 제한이 있을 수 있다.
    - iOS 진영은 PWA 지원에 매우 소극적임 (수익구조 관련 문제)
  - 사용자의 브라우저와 그 버전에 따라 기능이 제한될 수 있다.
    - 그럴 일은 없겠지만 팀쿡이 갑자기 iOS에서 PWA 지원을 종료해버리면 대략난감
  - 브라우저 위에서 동작하기에 native-app 에 비해 비교적 느릴 수 있다.
    - 그래서 Service Worker 의 역할이 중요하다

------------

PWA 의 조건
- 사용자의 기기에 설치가 가능해야 한다.
  - 사용자 입장에서 native-app 과 동일한 사용자 경험(UX)를 제공할 수 있어야 한다.
- 오프라인, 저속도 네트워크 환경에서도 동작해야 한다.
  - Service Worker 와 캐싱 기술을 이용하여 불안정한 네트워크에서도 native-app 과 같이 동작할 수 있어야 한다
- 백그라운드 동기화가 가능해야 한다.
  - app 화면이 켜져 있지 않은 상황에서도 백그라운드에서 필요한 동작을 수행할 수 있어야 한다(Service Worker)
- 사용자 재참여를 유도할 수 있도록 푸시 알림이 가능해야 한다.
  - Web Push API 활용
- 다양한 Web API를 사용하여 native-app과 같은 사용성을 갖출 수 있어야 한다.
- HTTPS 프로토콜을 통해 제공되어야 한다.
- 모든 브라우저에서 동작해야 하며, 모든 화면 크기에 대응해야 한다.

------------

PWA 시작하기

필수요소
- 서비스 가능한 웹 프로젝트와 서버...
- Manifest JSON 파일
``` json
{
  "name": "TEST_APP",
  "short_name": "TEST_APP",
  "description": "This is Test App for me",
  "start_url": "/",
  "display": "standalone",
  "orientation": "any",
  "theme_color": "#FFFFFF",
  "background_color": "#FFFFFF", 
  "prefer_related_applications": true,
  "icons": [
    {
      "src": "/asset/image/favicon-512.png",
      "sizes": "512x512",
      "type": "image/png"
    },
    {
      "src": "/asset/image/favicon-192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/asset/image/favicon-48.png",
      "sizes": "48x48",
      "type": "image/png"
    },
    {
      "src": "/asset/image/favicon-24.png",
      "sizes": "24x24",
      "type": "image/png"
    }
  ]
}
```
- 보안(https) 도메인에서 제공되는 웹사이트 (localhost 는 예외적으로 사용가능)
- 디바이스에서 앱을 나타낼 아이콘 이미지
- Service Worker(Chrome 브라우저에서만 필수)

``` javascript
importScripts('https://storage.googleapis.com/workbox-cdn/releases/6.5.4/workbox-sw.js');
 
self.addEventListener('widgetinstall', (event) => {
    event.waitUntil(updateWidget(event));
});
 
self.addEventListener('widgetresume', (event) => {
    event.waitUntil(updateWidget(event));
});
 
self.addEventListener('widgetclick', (event) => {
    if (event.action == "updateName") {
        event.waitUntil(updateName(event));
    }
});
 
self.addEventListener('widgetuninstall', (event) => {});
 
const updateWidget = async (event) => {
 
    const widgetDefinition = event.widget.definition;
 
    const payload = {
        template: JSON.stringify(await (await fetch(widgetDefinition.msAcTemplate)).json()),
        data: JSON.stringify(await (await fetch(widgetDefinition.data)).json()),
    };
 
    await self.widgets.updateByInstanceId(event.instanceId, payload);
}
 
const updateName = async (event) => {
    const name = event.data.json().name;
 
    const widgetDefinition = event.widget.definition;
 
    const payload = {
        template: JSON.stringify(await (await fetch(widgetDefinition.msAcTemplate)).json()),
        data: JSON.stringify({name}),
    };
 
    await self.widgets.updateByInstanceId(event.instanceId, payload);
}
 
workbox.precaching.precacheAndRoute(self.__WB_MANIFEST || []);
```

PWA 를 지원하고 있는지 확인하기

Lightouse 를 통해 검증하기
- Google Chrome Lighthouse는 웹 페이지의 성능, 접근성, 프로그레시브 웹 앱, SEO 등 여러 측면을 평가하는 오픈 소스 자동화 도구입니다.
- 사용자는 Lighthouse를 사용하여 웹 애플리케이션의 품질을 개선할 수 있는 구체적인 권장 사항을 얻을 수 있습니다.
- Chrome DevTools 내장 도구로 사용하거나, 명령줄 도구로 실행하거나, Node 모듈로 프로그래밍 방식으로 사용할 수 있습니다.

PWABuilder 를 통해 online 으로 검증하기
- https://www.pwabuilder.com

------------

고찰

- PWA 가 웹기술만 가지고 native app 에 준하는 사용자경험을 제공할 수 있는 것은 맞으나, iOS 의 소극적인 지원으로 아직 native app 을 완벽히 대체할 수준은 아니라고 생각한다.
- 웹 기반으로 제공하는 서비스가 부수적으로 적은 비용을 가지고 모바일을 지원하고자 하는 경우는 PWA 가 적합할 것으로 본다.
- 반대로, 모바일을 메인 타겟으로 삼는 서비스를 만들고자 한다면 PWA 가 매혹적일수는 있으나 RN, Flutter 등을 쓰는 것이 개발 프로세스, 사용자경험 측면에서 더 나을 것 같다. (다음 장에서 이어서 설명)

------------

참조
- https://developer.mozilla.org/ko/docs/Web/Progressive_web_apps
- https://developer.mozilla.org/ko/docs/Web/API
- https://sylagape1231.tistory.com/123
- https://www.pwabuilder.com/
- https://firebase.google.com/docs/cloud-messaging?hl=ko
- https://developer.chrome.com/docs/workbox?hl=ko
