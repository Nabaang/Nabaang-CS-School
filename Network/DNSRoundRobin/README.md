# DNS Round Robin

DNS는 Domain Name을 IP로 변환하는 기술이었다. 하나의 Domain Name에 여러 IP 주소가 매핑되어 있을 수 있다.
DNS Round Robin은 DNS의 주소를 여러 대의 서버 IP 리스트 중에 랜덤하게 하나 혹은 여러개를 선택하여 사용자에게 알려준다.(최근에는 여러개를 제공하고 클라이언트에서 선택하는 경우가 많음)
결과적으로 사용자들이 자연스럽게 나뉘어 접속하면서 서버의 부하가 분산되는 방식이다.

## 언제 사용하는가?

지리적으로 복수의 웹 서버가 멀리 떨어져 있어서 실시간으로 헬스 체크가 어려울 때 사용
혹은, 적은 비용으로 구현할 때..

## 단점?

Round Robin DNS는 로드밸런싱 기능이 없기에 별도의 헬스 체크도 없다. 그래서 HA(High Availability) 용도로는 적합하지 않다. (개인적으로는 하나의 Domain Name에 여러개의 IP를 제공하는 이유로 보임, 처음 선택한 IP가 접속이 안되면 다음 IP를 사용하여 접속을 시도한다)

## DNS 캐싱

DNS 정보도 브라우저, 로컬 DNS resolver 등에 캐싱된다.
캐싱 주기(TTL : Time To Live) 동안은 동일한 도메인에 대해 캐싱된 IP 주소를 사용한다.
캐싱 주기 설정을 고민해야함. propagation과 빈번한 조회간의 Trade-off.
캐싱 주기를 길게 잡으면, 변경된 DNS 정보의 propagation이 오래 걸린다.
반대로, 캐싱 주기를 짧게 잡으면, 도메인 조회가 빈번해진다.
실제로는 캐싱 시간을 짧게 가져가는 추세라고 함.

---

## 출처

[https://m.blog.naver.com/sehyunfa/221691155719](https://m.blog.naver.com/sehyunfa/221691155719)
