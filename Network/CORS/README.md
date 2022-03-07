# CORS와 해결방법

## What is Origin?

![URI Structure](./images/URI_Structure.png)

위의 그림은 URL을 구성하는 요소들이다.

이 중 Origin은 Protocol과 Host, Port의 조합을 의미한다.

위 그림에서는 Port가 없는데 이 때는 Protocol의 Well-known port를 사용한다.

## CORS(Cross Origin Resouce Sharing)

브라우저는 기본적으로 같은 origin의 리소스만 공유할 수 있다.(**SOP** - Same-Origin Policy)

다른 origin간의 리소스 공유는 불가능하다.

CORS는 다른 origin간의 리소스를 공유할 수 있게 할 수 있게하는 HTTP-header 기반의 장치이다.

CORS 허용 여부는 client의 origin과 응답 헤더의 Access-Control-Allow-Origin의 값을 비교해서 결정한다.

## Preflight

브라우저는 리소스 요청을 바로 보내지 않고, 예비 요청(preflight)과 본 요청으로 나누어서 서버로 전송한다.

preflight는 OPTIONS 메소드를 사용한다.

본 요청을 보내기 전에 브라우저 스스로 이 요청을 보내는 것이 안전한지 확인하는 것이다.

만약, 허용되지 않은 요청(허용하지 않는 origin)이라면 본 요청을 보내지 않는다.

http 요청 중에, 단순 요청(Simple Request)의 경우에는 preflight를 보내지 않는다.

## CORS 해결 방법

### 1. _서버에서 Access-Control-Allow-Origin 설정_

response 헤더 중 Access-Control-Allow-Origin의 값을 허용하는 origin으로 설정.

우리가 자주 사용하는 express cors middleware가 위의 헤더 값을 설정해줬음.

### 2. _프록시 서버_

CORS는 브라우저에서 인식하는 에러이기 때문에 가능한 방법.

클라이언트와 서버 사이에 프록시 서버를 둔다.

예로, webpack-dev-server의 proxy 기능이나 CRA로 생성한 프로젝트의 proxy기능이 있다.

---

## 출처

[https://evan-moon.github.io/2020/05/21/about-cors/#simple-request](https://evan-moon.github.io/2020/05/21/about-cors/#simple-request)
