# 양방향 통신

## Polling vs Long Polling vs Socket

모두 양방향 통신을 구현하기 위한 방법이다.
http는 단방향 통신 방식을 사용한다.
요청을 하는 client가 있고, 요청에 응답하는 server가 있다.
이러한 http에 약간의 트릭을 사용해서 실시간으로 동작하는 것처럼 동작하게 하는 기술들이다.

### Polling

http request를 계속(주기적으로) 보내서 이벤트 내용을 전달받는 방식이다.

위의 방법 중 가장 쉬운 방법이지만, 클라이언트가 계속해서 request를 보내기 때문에 서버의 부담이 증가하게 된다.

항상 이벤트 내용을 전달받는 것은 아니고, 빈 메시지(이벤트가 없다)를 받을 수도 있다.

매번 http connection을 맺는 것도 부담이다.

connection 비용 때문에 빠른 응답을 기대하기도 어렵다.

### Long Polling

클라이언트가 http request를 계속 보내는 것이 아니라 일단 request를 보내 놓는다

이후, 서버는 클라이언트로 response를 바로 보내는 것이 아니라,

전달할 이벤트가 있을 때 response를 보낸다.

클라이언트는 response를 받자마자 다시 request를 보내는 방식이다.

일반적인 polling 방식보다는 서버의 부담은 줄겠지만,

클라이언트가 요청을 보내는 주기가 짧아진다면, polling 방식과 차이가 없어진다.

### Streaming

long polling과 마찬가지로 일단 request를 보낸다.

다른점은 server event가 발생하고 response를 보내는데, 기존의 request를 끊지 않는다는 점이다.

connection을 매번 다시 연결하지 않아도 되서 connection에 대한 부담은 줄어든다.

하지만, Long Polling이나 Streaming은 서버에서 클라이언트로 메시지를 보낼 수는 있으나 역은 성립하지 않는다.(정확히는 원하는 타이밍에 메시지를 보낼 수 없다)

### Socket

Socket은 하나의 TCP connection에서 양방향 통신을 지원하는 프로토콜이다.

실시간 데이터 통신을 제공할 수 있다.

client, server 모두 데이터를 전송할 수 있다.

HTML5 표준의 일부로 webSocket이 만들어졌다.

소켓 통신을 하기위해 웹 소켓 프로토콜 핸드 쉐이크를 진행하는데 이를 http로 수행한다.

webSocket은 port 번호를 http와 동일하게 사용하기 때문에 방화벽 설정 없이 사용할 수 있다.

## WebSocket vs Socket.io

| 비교                           | WebSocket                                 | Socket.io                                                                             |
| ------------------------------ | ----------------------------------------- | ------------------------------------------------------------------------------------- |
| 개념                           | HTML5 웹 표준 기술,TCP 연결 기반 프로토콜 | WebSocket으로 동작하는 라이브러리(정확하게는 WebSocket뿐만 아니라 다른 방식들도 지원) |
| 통신 방식                      | full-duplex 통신 지원                     | event기반 통신 지원                                                                   |
| proxy, load balancer 지원 여부 | 미지원                                    | 지원                                                                                  |
| broadcasting 지원 여부         | 미지원                                    | 지원(room)                                                                            |
| fallback(차선책) option        | 미지원                                    | 지원(Long Polling)                                                                    |

(추가) Socket.io가 WebSocket으로 동작하기도 하지만 항상 WebSocket을 사용하도록 보장되는 것이 아니기 때문에 서로(Socket.io와 WebSocket)간의 통신은 불가능 하다.
i.e. WebSocket server ↔️ Soket.io client (통신 불가능, 반대의 경우도 마찬가지)

### 그러면 Socket.io가 짱인가?

그렇지 않다.

Socket.io가 많은 기능을 지원하는 만큼 편의성은 좋지만 그만큼 무겁기도 하다.
그래서 성능은 Socket.io보다 WebSocket이 더 좋다.

결국 무엇을 쓰느냐 하는 문제는 속도와 편의성 간의 trade-off를 고려해서 선택하면 된다.
여러 기능보다는 속도가 중요하다고 하면 WebSocket을,
속도보다는 편리한 기능들이 중요하다고 하면 Socket.io를 선택하면 된다.

---

### 출처

[https://rubberduck-debug.tistory.com/123](https://rubberduck-debug.tistory.com/123)
[https://anuradha.hashnode.dev/short-polling-vs-long-polling-vs-web-sockets](https://anuradha.hashnode.dev/short-polling-vs-long-polling-vs-web-sockets)
