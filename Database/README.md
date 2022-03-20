### 데이터베이스를 사용하는 이유

-   파일 시스템의 문제점(데이터의 독립성, 무경설, 보안성, 일관성, 중복성)을 해결하기 위해 만들어짐

### RDBMS vs NoSQL

-   RDBMS는 모든 데이터를 2차원 테이블 형태로 표현하며 테이블 간의 관계를 표현하기 위해 외래키와 조인을 사용한다. scale-up 방식
-   NoSQL는 관계를 정의하지 않으며 여러 방식(key-value, document, column, graph)으로 데이터를 표현한다. scale-out 방식

### 인덱스란?

-   데이터베이스에서 검색 성능(속도)을 높여주는 자료구조

### 인덱스의 자료구조

-   해시 테이블 - key의 해시 값으로 데이터 탐색, O(1), 등호(=) 연산에 최적화
-   B-Tree - 균형 트리, O(logN), 모든 데이터 순회 시 트리의 모든 노드를 방문해야 함.
-   B+Tree - leaf node 에만 데이터를 저장, 인접한 leaf node 끼리도 연결되어 있음. full scan 시 선형 탐색

### Primary index vs Secondary index

-   Primary index는 중복을 허용하지 않으며(unique), 인덱스와 데이터가 정렬된 순서로 저장된다.
-   Secondary index는 중복을 허용하며, 인덱스는 정렬된 순서를 보장하지만 데이터는 정렬된 순서를 보장하지 않아도 된다.

### Composite Index

-   두 개 이상의 컬럼을 합쳐서 만든 인덱스
-   컬럼들이 결합된 순서에 따라 성능 차이가 있을 수 있다.
    (먼저 결합되는 컬럼이 많은 데이터를 걸러낼수록 좋음)

### Index에 고려해야할 사항

-   분포도가 좋아야 함.
-   자주 조합되어 사용되는 경우는 결합 인덱스(결합되는 순서도 중요)를 사용하자.

---

### Transaction

-   하나의 논리적 작업 단위를 구성하는 일련의 연산들의 집합.

### 연산

-   Commit : 갱신 연산이 완료되었다고, 트랜잭션 관리자에게 알려주고, 결과를 DB에 반영하는 연산
-   Rollback: 트랜잭션 도중에 수행한 모든 연산을 취소하고, 트랜잭션 수행 이전의 상태로 돌아가는 연산

### 트랜잭션의 특성 및 상태

![Commit 상태](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7f4f2566-a315-43f5-abe5-1b2e805870cd/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220320%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220320T051951Z&X-Amz-Expires=86400&X-Amz-Signature=2d1751c6932fbafb1ad16007de16aa224121c1846ca6edbcf1b6855f4d481b6a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

-   Active → 현재 트랜잭션이 진행 중인 상태
-   Partially Commited → 부분적으로 커밋된 상태
-   Commited → 트랙잭션이 커밋된 상태
-   Failed → 실패한 상태
-   Terminated → 종료된 상태

### ACID

-   Atomicity : 트랜잭션을 하나의 작업 단위로 보고, 부분적으로 실행되다가 중단되지 않는 것을 보장
-   Consistency : 트랜잭션 실행이 성공적으로 완료하면 언제나 일관성있는 데이터베이스 상태로 유지해야 한다. 예로, 모든 계좌(마이너스 통장이 아닌)는 잔고가 있어야 하는데 이를 위반하는 트랜잭션은 중단된다.
-   Isolation : 트랜잭션을 수행 중에 다른 트랜잭션이 끼어들지 못하도록 보장해야 한다.
-   Durability : 성공적으로 수행된 트랜잭션은 영원히 반영되어야 한다.

### BASE

-   Basically Available - 무조건적으로 일관성을 추구하기보다는 BASE-modelled NoSQL 데이터베이스는 데이터를 여러 노드 간의 복사나 확장을 통해서 접근을 허용해준다
-   Soft State - 데이터는 시간에 따라 계속 변한다
    -   즉, 떨어진 노드 간의 데이터 업데이트는 데이터가 노드에 도달한 시점에서 갱신된다.
-   Eventually Consistent - BASE는 “immediate consistency”를 절대로 지켜낼 수는 없지만 data reads는 언제나 가능하다
-   Marketing or Customer Service Companies → social network search를 하는데 있어서 유연성이 높은 BASE를 더 선호할 것
-   NoSQL

---

### Lock

-   특정 데이터에 여러 트랜잭션이 무분별하게 동시에 접근하는 것을 막는 방식
-   공유락과 배타락으로 구분 가능.

### 공유 잠금(Shared Lock or Read Lock)

-   데이터를 읽을 때 사용한다.
-   읽는 작업들이 동시에 자원에 접근해도 문제가 발생하지 않음.
-   공유 락이 걸린 레코드는 UPDATE, DELETE, SELECT FOR UPDATE 불가능
-   배타 잠금이 걸린 상태라면 읽지 못함(공유 잠금을 걸지 못함)

### 배타 잠금(Exclusive Lock or Write Lock)

-   데이터를 쓸 때 사용한다.
-   공유 잠금과 배타 잠금 모두에서 충돌이 발생
    -   읽는 중에 쓰기 불가, 쓰는 중에 쓰기 불가
-   배타 잠금이 해제될 때 까지 자원에 접근 불가

---

### NoSQL

-   **No + SQL**은 **Not Only SQL**의 약자이다. ~~(NO SQL이 아니다!)~~
-   기존의 관계형 데이터베이스 보다 더 융통성 있게 데이터 모델을 사용하고 데이터의 저장 및 검색이 빠름.
-   고정된 스키마를 갖지 않음.
    -   key-value 의 쌍으로 구성

### CAP 이론

분산 시스템에서 네트워크 장애상황일 때 일관성과 가용성 중 많아도 하나만 선택할 수 있다.

-   Consistency(일관성)
    -   모든 노드들에 동일한 데이터가 저장되어 있어야 한다.
-   Availability(가용성)
    -   일부 노드에 장애가 발생하더라도, 사용자는 시스템을 이용 할 수 있어야 한다.
-   Partitioning(분산 허용성)
    -   통신으로 인한 장애가 발생하더라도, 사용자는 시스템을 이용할 수 있어야 한다.

### 저장 방식에 따른 분류

-   Key-Value Model
    -   데이터를 조회 할때, key와 value 가 1:1 관계를 갖는 경우를 의미
    -   속도가 빠르며 확장에 유연하지만 value 타입을 알 수 없으며, 항상 쿼리의 결과가 하나이다.
        -   “value의 타입이 이미지인 모든 데이터”와 같은 서브셋 쿼리가 불가
-   Document Model
    -   document는 object와 같이 계층적으로 저장된 데이터의 형태를 의미 (XML,JSON 등)
    -   데이터를 읽고, 저장하는 과정에서 별도의 매핑 과정이 필요하지 않음.
-   Column Model
    -   RDB와 유사하지만, 다른 점은 칼럼의 값으로 또 다른 릴레이션을 허용함.
    -   기존의 컬럼을 column family, column family에는 여러개의 칼럼이 존재하는 릴레이션이 저장될 수 있다. 그래서 데이터를 찾기 위해서는 행 번호, Column Family, Column Name이 필요하며, 여기에 데이터의 버전을 보장하기 위해 timestamp를 추가적으로 고려한다. - Row -> Column Family -> Column -> Timestamp -> Value
        ![Column table](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/52ab24f7-2893-4361-a170-ebfc677035fc/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220320%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220320T052055Z&X-Amz-Expires=86400&X-Amz-Signature=3606b1dda750671ee05fb8d05e13c8e0a58479a02de2b3fb5304a64b873a972e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)
    -   Row-Oriented인 RDB와 달리, Column-Oriented로 Column 기반으로 데이터를 자주 찾는 경우를 위함.
    -   구글의 Big Table, 페이스북의 cassandra,HBASE

---

## 클러스터링 VS 리플리케이션

### 클러스터링

-   **동기 방식**으로 노드들간의 데이터를 동기화
-   노드들 간의 데이터를 동기화하여 **항상 일관성 있는 데이터**를 얻을 수 있음.
-   1개의 노드가 죽어도 다른 노드가 살아 있어 무중단 시스템 운영이 가능.
-   여러 노드들 간의 데이터 동기화 하는 시간이 필요하므로 Replication에 비해 쓰기 성능이 떨어짐.
-   데이터 동기화에 의해 스케일링 한계가 있음.

### 리플리케이션

-   두 개 이상의 DBMS 시스템을 **수직적인 구조(Master / slave)**로 구축하는 방식
-   비동기 방식으로 노드들간의 데이터를 동기화
-   Master에서 데이터의 수정 사항 반영, slave에서 실제 데이터를 복사
-   비동기 방식으로 지연 시간이 거의 없음.
-   master가 다운되면 복구 및 대처가 까다로움.
-   노드들 간의 데이터 일관성(불일치)을 보장하지 못함.

---

## Statement vs PreparedStatement

-   가장 큰 차이점은 caching 여부

### statement

-   string 형태로 sql 작성하고, 매개 변수를 전달할 수 없음
-   DDL문(Create, alter, drop ..)에서 주로 사용
-   실행 시 매번 컴파일 하기 때문에 성능적인 면에서 안좋음.
    -   DB에 해당 쿼리로 접근 계획은 캐싱 됨.

### preparedStatement

-   SQL문은 프리 컴파일 되어 PreparedStatement Object에 캐싱됨.(application Layer)
-   매개변수 전달이 필요한 sql 작성 시 용이
    -   사용자 입력값으로 쿼리 생성
    -   반복 수행 작업일 경우
-   캐싱 영역의 한계로 필요한 문장만 적용하는 것을 권장
