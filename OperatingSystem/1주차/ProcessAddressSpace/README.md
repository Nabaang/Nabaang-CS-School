# 프로세스 주소 공간(+ 힙, 스택 차이)

프로그램이 실행되면 프로세스 주소 공간(Process Address Space)이 Memory에 할당(생성)된다.

1. Code 영역(text 영역)

-   실행할 프로그램의 코드(기계어 코드)가 저장
-   컴파일 타임에 결정됨.
-   Read-Only
-   같은 프로그램 간에 공유 가능

2. Data 영역

-   프로그램의 전역 변수와 정적 변수가 저장(항상 접근 가능한 변수)
-   Read-Write 가능
-   변수의 초기화 여부에 따라 Data와 BSS(Block Stated Symbol)로 나누기도 함.

3. Heap 영역

-   참조형 데이터의 값이 저장
-   런타임에 크기가 결정
-   사용자에 의해 메모리 공간이 동적으로 할당, 해제됨.

4. Stack 영역

-   함수, 지역 변수, 매개변수가 저장
-   함수 호출과 함께 할당되며, 함수 호출이 완료되면 소멸된다.
-   원시타입의 데이터는 값과 함께 할당되고, 참조 데이터는 Heap의 주소를 할당한다.

⋇ Heap과 Stack은 같은 공간을 공유한다?

Heap과 Stack은 같은 공간을 공유하며 서로 끝과 끝에서 시작한다. 서로의 방향으로 공간을 할당해가며, 상대 공간을 침범하는 일이 발생할 수 있다. 이를 각각 Heap Overflow, Stack Overflow라고 한다.

---

### 출처

[https://whereisusb.tistory.com/10](https://whereisusb.tistory.com/10)
[https://gona.tistory.com/4](https://gona.tistory.com/4)
