---
layout: post
title: Java WebSocket
categories: [Java]
comments: true
---

WebSocket

WebSocket 은 기존의 비연결지향중심의 HTTP 통신의 단점을 극복하고 서버-클라이언트가 전이중 통신 채널을 구축하기 위해 고안된 통신 프로토콜이다. WebSocket 은 2014년 HTML5 의 출시와 동시에 표준이 되었으며, 이를 통해 별도의 추가 채널(플랫폼) 없이 웹만으로도 소켓 통신이 가능하게 되었다.

-------------

서버 구현 (Java)

Java 에서는 javax.websocket 라이브러리를 구현하는 것으로 WebSocket 의 Server Side 구축이 가능하다. 어노테이션 기반 개발로 직관적인 코드를 확인할 수 있다.


{% highlight java %}
import javax.websocket.*;

@ServerEndpoint("/websocket")
public class websocket {

    /**
    * WebSocket 연결 시
    */
    @OnOpen
    public void handleOpen(){
        System.out.println("client is now connected...");
    }

    /**
    * Client 로부터 메시지 수신 시
    */
    @OnMessage
    public String handleMessage(String message){
        System.out.println("receive from client : " + message);
        String replymessage = "echo " + message;
        System.out.println("send to client : " + replymessage);

        return replymessage;
    }

    /**
    * WebSocket 연결종료 시
    */
    @OnClose
    public void handleClose(){
        System.out.println("client is now disconnected...");
    }

    /**
    * 에러 발생 시
    */
    @OnError
    public void handleError(Throwable t){
        t.printStackTrace();
    }
}
{% endhighlight %}

-------------

클라이언트 구현 (Javascript)

웹 브라우저의 클라이언트 개발은 Javascript 의 WebSocket 객체를 구현하는 것으로 가능하다. 각기 필요한 상황에 맞는 콜백 함수를 구현하고 메시지 송신, 연결 종료 등 클라이언트 주도 기능 역시 구현한다.

{% highlight javascript %}
var webSocket = new WebSocket("ws://localhost:8080/websocket"); // ws://{Address}/{EndPoint}

// 연결 시
webSocket.onopen = function(message){
    console.log("websocket is connected...");
}

// 연결 종료 시
webSocket.onclose = function(message){
    console.log("websocket is disconnected...");
}

// 에러 발생 시
webSocket.onerror = function(message){
    console.log("error occured...");
}

// 메시지 수신 시
webSocket.onmessage = function(message){
    console.log(`Receive From Server => ${message.data}`);
}

// 메시지 송신
var sendMessage = function(message){
    webSocket.send(message);
}

// 연결 종료
var disconnect = function(){
    webSocket.close();
}
{% endhighlight %}