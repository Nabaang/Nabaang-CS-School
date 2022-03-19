# NoSQL(Not Only SQL)

## NoSQL

> No + SQL은 `Not Only SQL`의 약자이다. ~~(NO + SQL이 아니다!)~~

데이터의 형태가 다양해지고, 데이터의 양이 증가하면서 기존의 RDB 형태의 저장 기술로는 데이터를 저장하기 어려워졌다. 이를 해결하기 위해, NoSQL이 등장했다. NoSQL은 RDB가 아닌 데이터베이스를 말한다.

오늘날, 데이터베이스의 종류에는 RDBMS, Data warehouse, NoSQL이 있다. RDBMS는 트랜잭션 처리, Data warehouse는 데이터 분석의 용도로 사용된다. NoSQL은 트랜잭션 처리 및 데이터 분석이 모두 가능하다. 또한, 스키마가 고정되지 않아 데이터를 유연하게 저장할 수 있다.

## CAP 이론

> 네트워크에 장애가 발생했을 때 일관성과 가용성 중 무엇을 택할 것인가?

CAP 이론은 분산 시스템에서 네트워크에 장애가 발생했을 때 일관성 또는 가용성 중 많아도 하나만 선택할 수 있다는 것이다. 즉, 많아도 Consistency(일관성), Availability(가용성), Partitioning(부분 결함 용인) 중 2가지만 만족할 수 있다.

- C(일관성) : 모든 노드들에 동일한 데이터가 저장되어 있어야 한다.
- A(가용성) : 일부 노드에 장애가 발생하더라도, 사용자는 시스템을 이용할 수 있어야 한다.
- P(부분 결함 용인) : 통신으로 인한 장애가 발생하더라도, 사용자는 시스템을 이용할 수 있어야 한다.

CAP 이론을 학습하다 보니 CA, CP, AP를 표 형태로 정리한 글을 많이 보았다. 다양한 의견이 있지만 중론은 P는 현실적으로 포기할 수 없다는 것이다. 현실적으로 네트워크 장애는 막을 수 없기 때문이다.

그러면 C와 A 중 무엇이 더 나은 선택일까? 답은 케바케이다. C를 택하는 경우, 트랜잭션 동기화를 통해 데이터의 일관성을 확보할 수 있지만 동기화 과정에서 성능이 저하된다. 반대로, A를 택하는 경우 동기화가 없어 성능은 좋지만 데이터의 일관성을 포기하는 경우이다.

## NoSQL과 CAP

NoSQL은 분산형 시스템 구조를 따른다. 그래서 분산형 시스템의 특징인 `CAP` 이론을 따른다. 분산형 시스템은 장애에 대응하기 위해 동일한 데이터를 복제 후 여러 노드에 저장한다. 이때 저장되는 노드의 수를 `Replication Factor` 라고 한다. 만약에 특정 노드가 장애가 발생해 더 이상 데이터를 저장할 수 없다면, scale-out 하여 새로운 노드를 생성 후 데이터를 복제하여 replication factor를 유지한다.

## NoSQL Model

> `Eventual Consistency` + `NO shredding`

### Key-Value Model

Key-Value Model은 데이터를 조회할 때 key와 value가 1:1 관계를 갖는다. 데이터를 삭제, 저장, 수정하는 행위 모두 string type의 key값을 기반으로 수행된다. value는 타입이 제한되지 않고 blob 형태로 저장되어 이미지, 비디오 등 다양한 타입이 가능하다.

key-value 모델은 key값을 기반으로 쿼리를 수행하기 때문에 실시간성이 중요한 시스템에 사용된다. 데이터는 메인 메모리, 디스크에 모두 저장할 수 있다. 예로, Redis는 메인 메모리 DB이다. 메인 메모리 DB는 주로 사용자가 자주 찾는 데이터를 메인 메모리에 캐싱해둔다고 생각하면 된다.

장점으로는 속도가 매우 빠르며, 확장에 유연하다. 단점은 Value 타입을 알 수 없으며, 항상 쿼리의 결과가 하나이다. 예를 들어, “value의 타입이 이미지인 모든 데이터”와 같은 서브셋 쿼리가 불가하다.

### Document Model

Document Model의 Document는 Object와 같이 계층적으로 저장된 데이터의 형태를 의미한다. 예로, `XML`, `JSON` 등이 있다.

Document Model의 장점은 데이터를 읽고, 저장하는 과정에서 데이터 맵핑이 필요 없다. 예로, HTML 파일을 RDB 형태로 저장하려면 RDB에 맞게 HTML을 변환해야 한다. 또한, RDB 형태의 데이터를 HTML 형식으로 읽기 위해서는 재조립하는 과정을 거쳐야 한다. 이러한 과정이 모두 오버헤드이다. 이와 달리, Document Model은 Document 형태로 데이터를 저장하여, 데이터 저장 및 읽는 과정에서 데이터 변환의 오버헤드를 줄일 수 있다.

### Column Model

Column Model은 RDB와 유사하다. 다른 점은 칼럼의 값으로 릴레이션을 허용하는 점이다. 기존의 칼럼을 Column Family라고 하며, Colum Family에는 여러 개의 칼럼이 존재하는 릴레이션이 저장될 수 있다. 그래서 데이터를 찾기 위해서는 행 번호, Column Family, Column Name이 필요하며, 여기에 데이터의 버전을 보장하기 위해 Timestamp를 추가적으로 고려한다.

```text
Row -> Column Family -> Column -> Timestamp -> Value
```

Column Model이 Row-Orientend인 RDB와 달리, Column-Oriented로 Column 기반으로 데이터를 자주 찾는 경우에 사용된다. RDB는 애트리뷰트의 값들이 10개 정도만 넘어도 데이터 일관성 및 관리가 어려워 정규화를 통해 애트리뷰트를 다른 테이블로 분해한다. 또한, RDB는 주로 null 값을 허용하지 않는다. 이와 달리, Column Store는 애트리뷰트의 개수가 제한이 없으며 특정 칼럼은 잘 사용되지 않기 때문에 null값이 더 많다. 보통 column의 개수를 수천 ~ 수백만 개 처리하기 위해 다룬다. 또한, 스키마를 미리 만들어 두지 않기 때문에, 애트리뷰트가 필요한 경우 새로 추가 및 삭제한다.

### Ref

- [http://www.kocw.or.kr/home/m/search/kemView.do?kemId=1064626](http://www.kocw.or.kr/home/m/search/kemView.do?kemId=1064626)
- [https://blog.outsider.ne.kr/519](https://blog.outsider.ne.kr/519)
- [https://www.youtube.com/watch?v=Dvi_TsBMFJo](https://www.youtube.com/watch?v=Dvi_TsBMFJo)
- [https://osy0907.tistory.com/95](https://osy0907.tistory.com/95)
- [https://hamait.tistory.com/197](https://hamait.tistory.com/197)
- [http://eincs.com/2013/07/misleading-and-truth-of-cap-theorem/](http://eincs.com/2013/07/misleading-and-truth-of-cap-theorem/)
