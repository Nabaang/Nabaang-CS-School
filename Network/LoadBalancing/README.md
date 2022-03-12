# Load Balancing

## 정의

로드 밸런싱은 네트워크 트래픽을 여러 서버로 분산시키는 과정이다. 이는 하나의 서버가 높은 부하를 받지않도록 보장한다. 골고루 부하를 분산하면서 어플리케이션의 responsivenese를 개선시키고, 이는 사용자에게 가용성을 증가시킨다. 현대의 어플리케이션에서 로드 밸런서는 필수이다. 소프트웨어 로드 밸런서에는 어플리케이션의 보안도 추가할 수 있다.

## 알고리즘

다양한 로드 밸런싱 방법이 있다. 상황에 맞는 알고리즘을 사용하는게 가장 좋다.

-   Least Connection Method : 가장 적은 connection을 가진 server로 트래픽을 보낸다.
-   Least Response Time Method : active한 connection이 가장 적으면서 평균 응답 시간이 가장 작은 서버로 보낸다.
-   Round Robin Method : 동일하게 번갈아가면서 서버들에게 트래픽을 보낸다.
-   IP Hash : client의 IP 주소가 서버를 결정한다.

## Load Balancing Benefits

network traffic을 분산하는 것외에도 benefit이 많다.
traffic bottleneck이 발생하기 전에 예측할 수 있다. 이를 기반으로 자동화와 business decision을 돕는다.

OSI 계층에서 4~7계층에 위치한다.
L4에서는 IP나 Port를 기준으로 traffic을 분산한다.
L7에서는 content switching을 추가할 수 있다. HTTP header, uri, SSL session ID, HTML form data를 기준으로 라우팅을 결정한다.(우리 나방 프로젝트의 경우에는 uri를 기반으로 content switching을 수행함)

---

## 출처

[https://avinetworks.com/what-is-load-balancing/https://avinetworks.com/what-is-load-balancing/](https://avinetworks.com/what-is-load-balancing/)
