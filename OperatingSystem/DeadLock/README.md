# 교착 상태(DeadLock)

## 교착 상태란?

두 개 이상의 작업이 서로 상대방의 작업이 끝나기 만을 기다리고 있기 때문에 결과적으로 아무것도 완료되지 못하는 상태

다중 프로그래밍 환경에서 흔히 발생할 수 있는 문제이다.

## 교착 상태의 조건

1971년 E. G. 코프만 교수는 교착상태가 일어나려면 다음과 같은 네 가지 필요 조건을 충족시켜야 함을 보였다.

1. 상호배제(Mutual exclusion) : 하나의 자원은 한 번에 하나의 프로세스만이 사용할 수 있다.
2. 점유대기(Hold and wait) : 프로세스가 할당된 자원을 가진 상태에서 다른 자원을 기다린다.
3. 비선점(No preemption) : 프로세스가 어떤 자원의 사용을 끝낼 때까지 그 자원을 뺏을 수 없다.
4. 순환대기(Circular wait) : 각 프로세스는 순환적으로 다음 프로세스가 요구하는 자원을 가지고 있다.

이 조건 중에 한 가지라도 만족하지 않으면 교착 상태는 발생하지 않는다.  
이 중 순환대기 조건은 점유대기 조건과 비선점 조건을 만족해야 성립하는 조건이므로, 위 4가지 조건은 서로 완전히 독립적인 것은 아니다.

# 교착 상태의 관리

현재의 대부분의 운영체제들은 교착 상태를 막는 것이 불가능하다. 교착 상태가 발생하면 제각기 다른 비표준 방식들로 대응한다.  
대부분의 접근들은 4가지 코프먼 조건들 가운데 하나(순환대기)를 막음으로써 동작한다.

## 1) 교착 상태의 예방

교착 상태는 위에서 언급한 4가지 조건(상호배제, 점유대기, 비선점, 순환대기)를 모두 만족할 때 발생한다.

1가지 조건만 제거 하더라도 교착 상태를 예방할 수 있음.

-   상호배제 조건의 제거
    -   공유자원에 대한 경쟁상태가 발생할 수 있음.
    -   경쟁상태의 문제를 해결하는 thread synchronization이나 immutable object 사용을 고려해야 함.
-   점유와 대기 조건의 제거
    -   프로세스 대기를 없애기 위해서 프로세스가 실행되기 전에 필요한 모든 자원을 할당
        → 자원 낭비 발생
    -   자원을 점유하지 않고 있을 때에만 다른 자원을 요청할 수 있도록
        → 기아상태가 될 수 있음. (우선순위가 계속 밀려 실행되지 못하는 상태)
-   비선점 조건의 제거
    -   모든 자원에 선점을 허용
    -   현재 할당 받을 수 없는 자원을 요청한 경우, 가지고 있던 자원을 모두 반납 후, 새 자원과 이전 자원을 얻기 위해 대기
        → 자원 낭비 발생
-   순환 대기 조건의 제거
    -   자원에 고유한 번호를 할당하고, 번호 순서대로 자원을 요구
        → 자원 낭비 발생

다른 방법들에 비해 자원의 낭비(사용되지 않는 자원이 발생)가 심하다.

## 2) 교착 상태의 회피

교착 상태가 발생하기 전에 교착 상태를 예상하여 **안전한 상태**에서만 자원 요청을 허용하는 방법

안전한 상태란 교착 상태를 발생시키지 않고 자원 할당이 가능하며, 모든 프로세스가 정상적으로 종료할 수 있는 상태를 의미함.

단일 인스턴스 자원 유형에서는 자원 할당 그래프 알고리즘을,  
다중 인스턴스 자원 유형에서는 은행원 알고리즘을 사용한다.

교착상태 회피 알고리즘을 사용하는 것도 상당한 오버헤드를 동반함.

## 3) 교착 상태의 탐지 & 복구

탐지 알고리즘을 사용하여 교착 상태가 발생했는지 확인함.

교착 상태가 탐지되면 복구 기법을 통해 안전한 상태로 복구함.

이 방식은 지속적으로 교착 상태를 확인하는 작업이 필요하기 때문에 성능 저하 발생.

교착 상태 탐지 알고리즘에는 대기 그래프와 은행원 알고리즘이 있다.

탐지 알고리즘의 호출 주기는 탐지 알고리즘의 오버헤드로 시간적 빈도나 프로세스의 교착 상태 연루 빈도에 따라 조절해야 한다.

예) 자원을 할당받지 못하는 시점, 일정 시간마다 호출, cpu 이용률이 특정 값 이하로 떨어지는 시점

교착 상태를 복구하는 방법에는

1.  교착상태에 있는 프로세스를 종료
    -   교착 상태의 프로세스 모두 종료
    -   교착 상태가 제거될 때까지 하나씩 종료
2.  교착 상태에 있는 프로세스가 점유하고 있는 자원을 선점하여 다른 프로세스에 할당

    이 방식에는 다음 사항들을 고려해야함.

    -   희생자 선택 : 최소의 피해를 줄 수 있는 프로세스
    -   롤백 : 선점 된 프로세스를 문제 없던 이전 상태로 롤백, 가장 안전한 방법은 프로세스 중지 후 재시작
    -   기아 상태

## 4) 교착 상태의 무시

위에 언급된 방식들은 모두 성능에 영향을 끼치게 된다.

교착 상태의 발생 확률이 비교적 낮고 위험하지 않다면 별다른 조치를 취하지 않을 수도 있다.
