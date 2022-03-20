## 운영체제

- 자원들(HW, SW)을 효율적으로 관리하며, 사용자와 응용 프로그램에 인터페이스(서비스)를 제공해주는 컴퓨터 시스템이다.

### 컴퓨터 시스템의 동작 원리

- 외부 장치(I/O 장치)에서 내부 장치(CPU, 메모리)로 데이터를 읽어와 각종 연산을 수행하며 그 결과를 외부 장치로 다시 내보내는 방식으로 동작한다.

## 프로세스

- 메모리를 할당받아 실행 중인 프로그램.

### 프로세스의 상태

- created : 프로세스를 할당하는 단계
- ready : 프로세서를 제외하고 자원을 모두 할당받은 상태
- running : 프로세서를 할당받아 실행 중인 상태
- asleep : 인터럽트가 발생하여 프로세스가 중단된 상태, ISR 종료 후, ready 상태로 변경
- suspended : 가상 메모리를 사용하여 디스크로 스왑아웃된 상태

### 프로그램과 프로세스의 차이

- 프로그램은 디스크에 존재하는 프로그램(코드, 정적 데이터)인 반면, 프로세스는 프로그램이 메모리를 할당 받은 상태이다.

### 시스템 콜

- 응용 프로그램의 요청에 따라 커널에 접근하기 위한 인터페이스

### 인터럽트

- 입출력 장치나 예외상황의 발생으로 CPU의 정상적인 프로그램 실행을 방해하는 행동

### IPC(Inter Process Communication)

- 프로세스들은 자신만의 공간을 가지기 때문에 다른 프로세스의 공간에는 접근하지 못한다. 이에 서로 간의 통신을 위해 shared memory나 message passing 방식을 이용한다.

### 프로세스 주소 공간(힙, 스택 차이)

- 프로그램이 실행되면 프로세스 주소 공간이 메모리에 할당된다. 목적에 따라 Code, Data, Heap, Stack 영역으로 나뉠 수 있다.
- Heap과 Stack 영역은 같은 공간을 공유하며 같은 공간의 끝과 끝에서 시작한다. 서로를 향해 공간을 할당해가며, 상대 공간을 침범하게 되면 각각 Heap Overflow, Stack Overflow 에러가 발생한다.

### 싱글 스레드와 멀티 스레드

- 싱글 스레드는 공유 자원에 대한 동기화를 신경쓰지 않아도 되며, Context Switching 작업을 요구하지 않는다. 하지만, 여러 개의 CPU를 활용하지 못하는 단점이 있다.
- 멀티 스레드는 프로세스의 자원과 상태를 공유하여 효율적으로 운영할 수 있고, 프로세스보다 생성이 빠르고 문맥교환 또한 빠르다. 하지만, 운영체제의 지원이 필요하고, 스레드 스케줄링을 신경써야 한다.

### 프로세스와 스레드의 차이

- 프로세스는 다른 프로세스의 데이터에 접근할 수 없지만, 스레드는 다른 스레드와 메모리(Stack 영역 제외)를 공유한다

### 멀티프로세스 vs 멀티스레드

- 한 프로그램의 구성을 여러 프로세스로 하냐, 여러 스레드로 하냐에 따라 나뉜다.
- Context Switching은 멀티 프로세스보다 멀티 스레드가 오버헤드가 적기 때문에 빠르다.
- 멀티스레드는 스레드끼리 통신이 간단한 반면, 멀티프로세스는 IPC라는 복잡한 통신 기법을 사용해야 한다.
- 한 프로세스의 문제가 다른 프로세스에게는 영향이 없지만, 한 프로세스의 쓰레드들은 모두 영향을 받게 된다.

## User Level Thread vs Kernel Level Thread

### User Level Thread

- **사용자 영역**의 **스레드 라이브러리**로 구현
- 스레드와 관련된 모든 행위를 사용자 영역에서 하므로 **커널이 스레드의 존재**를 **알지 못함.**
- 다대일 스레드 매핑
- 커널에 독립적이므로 이식성이 높음.
- 스케줄링이나 동기화를 하려고 커널을 호출하지 않으므로 **커널 영역으로 전환하는 오버헤드가 적음**.
- 응용 프로그램에 맞게 유연한 스케줄링 가능.
- 시스템의 동시성을 지원하지 않음.

### Kernel Level Thread

- **커널**이 **스레드**와 관련된 모든 작업을 **관리**함.
- 일대일 스레드 매핑
- 커널이 각 스레드 개별적으로 관리할 수 있음.
  - 동작중인 스레드가 wait 되도 해당 프로세스 내 다른 스레드가 계속 실행 될 수 있음.
- 커널 영역으로 전환하는 오버헤드가 발생

## Thread Safety

- 멀티 스레드 프로그래밍에서 일반적으로 어떤 함수나 변수, 혹은 객체가 **여러 스레드로 부터 동시에** 접근이 이루어져도 프로그램의 실행에 문제가 없음을 뜻한다.

### Thread-safety 지키기 위한 방법

1. Re-entrancy (재진입성)
   - 어떤 함수가 한 스레드에 의해 호출되어 실행 중일 때, 다른 스레드가 그 함수를 호출하더라도 그 결과가 각각에게 올바로 주어져야 함.
2. Thread-local storage
   - 공유 자원의 사용을 최대한 줄여 각각의 스레드에서만 접근 가능한 저장소 들을 사용함으로써 동시 접근을 막음. (공유 상태 피할 수 없을 때)
3. Mutual exclusion
   - 공유 자원을 꼭 사용해야 할 경우 해당 자원의 접근을 세마포어, lock 등으로 통제
4. Atomic operations
   - 공유 자원에 접근할 때 원자 연산을 이용하거나 ‘원자적’으로 정의된 접근 방법을 사용함으로써 상호 배제를 구현할 수 있음.
5. Immutable Object

   객체 생성 이후에 값을 변경 할 수 없도록 만듬.

   Thread Safety in singleton Pattern

   [Single ton](https://www.notion.so/Single-ton-230d8f6d751b48d19a328b6893b10b47)

## 프로세스 스케줄링

- CPU 자원을 어떤 프로세스에 어떻게 할당할지에 대한 정책을 프로세스 스케줄링이라고 함.
- 평균 응답시간 : 얼마 빨리 반응하는지
- 처리량(Throughput): CPU가 단위 시간 동안 처리한 프로세스의 수

### 스케줄링 단계

스케줄링이 **발생하는 빈도, 할당 되는 자원의 종류**에 따라 **장기, 중기, 단기** 스케줄링으로 구분된다.

### 장기 스케줄링(Job scheduling)

- 한정된 메모리를 할당하여 프로세스가 될 Job을 결정하는 단계
- I/O bounded와 Computed bounded 프로세스 적절히 선택 (병목 방지)
  - 시스템 내 프로세스 수 조절(과부하 방지 → 자원 점유를 위한 경쟁 과열 방지)

### 중기 스케줄링(Memory allocation)

- 메모리에 너무 많은 프로세스가 올라 가는 것을 조절(swap-in,swap-out)하는 단계
- ready < - > suspended
- asleep < - > suspended

### 단기 스케줄링(Process schedulling)

- 메모리 내의 준비 상태에 있는 프로세스 중 CPU할당하여 running 상태로 변경할 프로세스를 결정하는 단계

## 프로세스 스케줄링 알고리즘

프로세스 스케줄링은 단기 스케줄링에 해당하며, 크게 **선점, 비선점**으로 구분할 수 있다.

1. 비선점 스케줄링
   - 이미 할당된 자원을 다른 프로세스가 강제로 빼앗아 올 수 없음.
   - 문맥 교환의 오버헤드가 적지만, 대기시간이 길어질 수 있음
2. 선점 스케줄링
   - 우선순위가 높은 이미 할당된 자원을 다른 프로세스가 강제로 빼앗아 올 수 있음.
   - 문맥 교환의 오버헤드가 있지만, 실시간 응답성, 시분할 시스템에 적합

### 비선점 스케줄링 알고리즘

- FCFS(First Come First Service)
  - 도착 순으로 CPU 배정
  - 호위 효과(Convoy Effec) : 수행 중인 긴 작업을 여러 개의 짧은 작업이 기다릴 수 있음.
- SRN(Shortest Process Next), SJF(Shortest Job First)
  - 대기하는 작업 중 실행 시간(Burst Time)이 가장 작은 작업에 CPU 할당
  - Burst Time을 정확히 측정하는 것은 비현실적
  - 무한정 대기하는 기아 현상 발생(실행시간이 긴 작업은 우선순위가 낮음)
- HRRN(High Response Ratio Next)
  - Reponse ratio = (대기시간 + 실행 시간) / 실행 시간
  - Response ratio 가 높을 수록 우선순위가 높음.
  - 수행 시간이 긴 프로세스의 기아 현상을 방지하기 위해

### 선점 스케줄링 알고리즘

- RR(Round-Robin)
  - ime quantum(δ) 개념 → δ이 매우 크면 `FCFS`와 유사 → δ이 매우 작으면 문맥교환 오버헤드
  - time quantum 이후에는 ready queue이 마지막에 쌓임
- SRTN(Shortest-Process Time Next)= SRT(Shortest remaining time)
  - 잔여 실행시간이 적을 수록 우선순위 높음
  - 잔여 실행시간을 추적은 비현실적
- MLQ(Multi Level Queue)
  - 미리 정해진 작업 별 우선순위
  - 작업은 최초 배정 된 Queue에서 이동 불가
  - 우선순위가 낮은 Queue는 기아현상
- MLFQ(Multi Level Feedback Queue)
  - 미리 정해진 작업 별 우선순위
  - `MLQ`와 달리, 큐간 작업이 이동 가능
  - 큐의 우선순위 변경, 큐 간 작업 이동으로 인한 오버헤드, 구현의 어려움

[프로세스 스케줄링](https://www.notion.so/ab09ce7df7ed464b8ad807938fc6e645)

## Critical Section Problem

> **_Critical Section Problem is to design a protocol that the processes can use to cooperate_**

### Critical Section

- 동일한 자원을 동시에 접근하는 코드 영역
- 한 프로세스가 **critical section**에 있다면 다른 프로세스는 출입 금지 🚫

### Critical Section Problem의 해결책

- 상호 배제 (**Mutual Exclusion)**
  - 한 프로세스가 critical section 상에 존재한다면 다른 프로세스들은 critical section에 출입 금지
- 진행(**Progress)**
  - critical section 상에 프로세스가 존재하지 않는 상태에서 들어가려 하는 프로세스가 여러개라면 어느 것이 들어갈지 결정 해주어야 함.
- 한정대기(**Bounded Waiting)**
  - 한 프로세스가 자신의 차례가 올 때까지 critical section에 들어가는 것을 다른 프로세스들에게 양보하는 횟수가 한정되어야 한다

### Hardware 기반의 Critical Section Problem 해결책

- `locking`을 이용해 critical section code를 보호하는 방법
- `atomic`한 hardware instructions을 사용
  - `atomic` = non-interruptible

### Software 기반의 Critical Section Problem 해결책

1. **Mutex Lock**
2. **Semaphores**
3. **Monitors**

### Mutex Lock

- `mutual exclusion` 조건을 충족하기 위해 `locking` 사용
  - _“acquire”_, _“release”_ 2개의 atomic operations
- Boolean variable을 통한 무한 루프를 사용하여 locking 구현 👉🏻 **busy waiting** == **spinlocks**
- 한번에 한개의 스레드만 진입할 수 있으므로 성능 향상을 기대하기 어려움.

### Semaphores

- “wait”, “signal” 2개의 atomic operations로만 공유 자원에 접근 가능
- Semaphore 사용법
  - **Binary Semaphore - 0, 1 불리언 값만을 사용한 mutex lock과 동일하게 동작**
  - **Counting Semaphore - 공유 자원의 인스턴스 개수 만큼의 integer value range를 가짐**

### Busy-wait vs Block-wakeup

> _어떤 방식이 더 좋은걸까?_

- 일반적으로 CPU 소모를 줄일 수 있는 Block-Wakeup 방식이 더 좋다
- 그러나 Block-Wakeup 방식에는 overhead가 존재한다
  - 어떤 Block Process를 wakeup 하는 과정에서 ready queue로 배치하는 과정, 반대로 block 과정에서 waiting queue로 배치하는 과정 이 모든 과정들이 overhead다

👉🏻 따라서 critical section 길이가 길면 Block-wakeup을 짧으면 Busy-wait 방식이 적당하다고 볼 수 있다

### Semaphore가 가지는 문제점

- **P, Q 프로세스 Deadlock 발생** PROCESS P
  - Locking(A)
  - Locking(**B**) PROCESS Q
  - Locking(**B**)
  - Locking(**A**)
- **Starvation (= indefinite blocking)**
  - 프로세스가 semaphore queue에 suspended되어 빠져나오지 못하는 상황

## Monitors

- 공유 자원 데이터 (=condition variable)
  - condition x
  - 모니터 내부에서만 접근 가능
- monitor 내부에는 공유 데이터를 접근하는 코드를 가지고 있다
  - `x.wait()` - x.wait()을 invoke 한 프로세스는 다른 프로세스가 x.signal()을 invoke하기 전까지는 대기 상태가 됨
  - `x.signal()` - 대기 상태인 프로세스 중 하나를 resume한다 (만약 대기 상태 중인 프로세스가 없다면 아무런 일도 일어나지 않는다)
- monitor에서는 한번에 하나의 프로세스만 active 상태로 동작 가능

### Semaphore의 Block-wakeup vs Monitor의 wait-signal

- 한 프로세스가 Sleep하려고 하고 다른 프로세스가 Sleep 하려고 하는 이 프로세스를 WakeUp하려고 하면 Sleep, WakeUp 동작은 동시 실행이 불가능 (생산자-소비자)
- Monitor의 wait-signal의 경우 Monitor 자체적으로 상호 배제가 이루어지기 때문에 Semaphore보다 훨씬 간편하다고 볼 수 있다

### Classical Syncrhonization Problems

1. Bounded Buffer Problem (유한 버퍼 문제)
   - 생산자가 데이터를 버퍼에 삽입하고, 소비자가 데이터를 버퍼에서 읽음.
   - 생산자는 버퍼가 가득차면 더 넣을 수 없음.
   - 소비자는 버퍼가 비면 뺄 수 없음.
2. Reader-Writer Problem (독자 저자 문제)
   - 하나의 버퍼를 공유 하며 이를 접근할때 발생하는 문제
   - 여러 독자가 공유 데이터 읽을 수 있지만 한명의 저자만 공유 데이터 에 쓸 수 있음.
   - writer가 공유 데이터 쓸 때 다른 프로세스는 접근할 수 없음.
3. Dining Philosopher Problem
   - 철학자가 동시에 자신의 왼쪽 포크를 집을 때, 모든 철학자들은 오른쪽의 포크를 기다려야 함.

## 동기 vs 비동기, Blocking vs Non-Blocking

### Blocking vs Non-Blocking

> **_호출되는 함수가 바로 return하느냐 마느냐_**

- `Blocking` : 호출된 함수가 자신의 작업을 모두 마칠 때까지 호출한 함수에게 제어권을 넘겨주지 않고 대기
- `non-blocking` : 호출한 함수에게 제어권을 넘겨주고 호출한 함수가 다른 일을 할 수 있는 기회를 줌

### 동기 vs 비동기

> **_호출되는 함수의 작업 완료 여부를 누가 신경쓰느냐_**

- `Synchronous`: A함수가 B함수를 호출 할 때, B함수의 결과를 A 함수가 처리하는 것
- `Asynchronous`: A함수가 B함수를 호출할 때, B함수의 결과를 B가 처리하는 것

## 데드락(DeadLock)

두개 이상의 작업이 서로 상대방의 작업이 끝나기만으로 기다리고 있기 때문에 결과적으로 아무것도 완료되지 못하는 상태

### 교착 상태 4가지 필요 조건

1. **상호 배제 (Mutual Exclusion)**

   자원은 한번에 하나의 프로세스만 사용 가능하다

2. **점유 대기 (Hold & Wait)**

   최소 하나의 자원을 점유하고 있으면서 다른 프로세스가 사용 중인 자원을 추가적으로 점유하기 위해 대기하는 프로세스가 존재한다

3. **비선점 (No Preemptive)**

   다른 프로세스가 자원을 사용 중인 경우 해당 프로세스의 자원 사용이 끝날 때까지 기다려야 한다

4. **순환 대기 (Circular Wait)**

   프로세스의 집합에서 순환형태로 자원을 대기하고 있다

### 교착 상태 해결 방법

1. **교착 상태 예방 및 회피**

   **예방**

   교착 상태 발생 조건 4가지 중 하나를 제거하면 교착 상태를 예방 가능하다.

   - **상호 배제 부정 -** 여러 프로세스가 공유 자원을 사용하도록 허용
   - **점유 대기 부정 -** 프로세스 실행 전 필요한 모든 자원을 한번에 할당
   - **비선점 부정 -** 점유 중인 자원을 다른 프로세스가 요구하는 경우 해당 자원을 양보
   - **순환 대기 부정 -** 자원에 고유 번호를 할당한 후 프로세스가 순서대로 자원을 요구

   **회피**

   프로세스가 자원을 요구할 때 시스템은 자원을 할당 후에도 해당 프로세스가 안전한 상태로 남아있는지에 대한 여부를 미리 검사해 교착 상태를 회피한다

   - 교착 상태 검사 방식이 인스턴스 자원의 유형에 따라 달라진다
   - 단일 인스턴스 자원 유형 → 자원 할당 그래프 알고리즘
   - 다중 인스턴스 자원 유형 → 은행원 알고리즘

   안전한 상태로 남아있게 되는 경우에만 자원을 할당하고 그렇지 않은 경우에는 다른 프로세스들의 자원 해지 시까지 대기하게 된다.

2. **교착 상태 발견 및 회복**
   - 교착 상태에 있는 프로세스를 종료한다
   - 교착 상태에 있는 프로세스가 점유하고 있는 자원을 선점하여 다른 프로세스에 할당한다

### Race Condition이란?

공유 자원을 여러 개의 프로세스가 동시에 접근하려고 할 때 접근의 타이밍이나 순서 등이 결과값에 영향을 줄 수 있는 상태

## Memory Management

### 메모리란?

메모리는 메인메모리, RAM을 뜻한다. 프로그램 실행 시 필요한 루틴 및 데이터 정보들을 저장하고 사용할 수 있게 하는데 사용되는 공간이다.

### Byte Ordering이란?

- 컴퓨터에 저장되는 데이터의 최소 단위는 1 Byte이다 (Byte)
- 데이터를 메모리에 저장하는 순서에 따라 데이터를 읽는 방식이 달라진다 (Order)
- 따라서 Byte + Order 방식에는 Big Endian 방식과 Little Endian 방식이 있다
  - Big Endian - 오른쪽에서 왼쪽으로 읽는 방식, 앞쪽으로 갈수록 데이터의 단위가 커진다
  - Little Endian - 왼쪽에서 오른쪽으로 읽는 방식, 뒷쪽으로 갈수록 데이터의 단위가 커진다

### 메모리 관리 전략

**메모리 관리 배경**

제한된 물리 메모리의 용량을 효율적으로 사용 및 참조하기 위해 다양한 메모리 관리 전략들이 등장했다

**메모리 관리 전략**

1. **연속 메모리 할당**
   - 프로세스를 메모리에 연속적으로 할당하는 방식으로 할당, 제거를 반복하다 보면 많은 hole들이 산발적으로 생기게 되어 외부 단편화가 발생하게 된다
   - 외부 단편화 문제를 해결하기 위한 First fit, Best fit, Worst fit 등의 다양한 할당 방식이 존재한다
2. **Paging**
   - 메모리 공간에 프로세스가 연속적으로 할당되어야 한다는 제약조건을 없앴다
   - 프로세스가 사용하는 공간을 논리 메모리 공간에서는 여러 개의 페이지로 나누어 관리하고 각 페이지는 물리 메모리의 프레임에 매핑되어 저장된다
     - `페이지` (논리 메모리 공간의 블락 단위), `프레임` (물리 메모리 공간의 블락 단위)는 고정 크기의 블락 단위이다
   - `MMU` 를 활용해 CPU가 프로세스가 연속된 메모리에 할당된 것처럼 인식하게 하지만 내부 단편화 문제가 있다
3. **Segmentation**
   - Paging 기법과는 달리 서로 다른 크기의 논리적 단위인 Segment 단위로 메모리를 분할한다
   - 하지만 외부 단편화 문제가 있다

## 외부 단편화 vs 내부 단편화

### `외부 단편화 (External Fragmentation)`

가변 \*\*\*\*메모리 분할 방식으로 인해 메모리의 여유 공간이 충분함에도 여유 공간들이 여러 조각으로 흩어져 있어서 프로세스를 메모리에 적재하지 못해 발생하는 메모리 낭비 현상

### `내부 단편화 (Internal Fragmentation)`

고정 메모리 분할 방식으로 인해 프로세스가 실제 필요한 메모리 공간의 크기보다 더 큰 메모리를 할당받아 발생하는 메모리 낭비 현상

## Memory 교체

### 페이지 교체 알고리즘

가상메모리를 구현하기 위해 메모리에 적재되어 있는 페이지 중 어떤 페이지를 새로운 페이지와 교체해야 할지를 결정하는데 사용되는 알고리즘

- **FIFO (First In First Out) -** 큐의 가장 앞에 있는 페이지를 교체 대상으로 선정
- **LRU (Least Recently Used) -** 최근 가장 오랫동안 사용되지 않은 페이지를 교체 대상으로 선정
- **NRU (Not Recently Used) -** LRU와 비슷한 알고리즘 but, 참조와 수정 여부에 따라 교체되는 페이지의 우선 순위가 정해진다 (우선순위; 수정 > 참조)
- **LFU (Least Frequently Used) -** 참조 횟수가 가장 낮은 페이지를 교체 대상으로 선정
- **MFU (Most Frequently Used) -** 참조 횟수가 가장 많은 페이지를 교체 대상으로 선정

### 캐시 메모리란?

CPU와 메인 메모리 사이에 위치하는 작지만 매우 빠른 속도의 메모리

- 메인 메모리와 캐시 기억 장치 사이의 정보를 매핑해야 한다.
- CPU가 어떤 데이터를 원하는지를 얼마나 예측할 수 있는지에 따라 캐시 성능이 좌우된다

### 캐시의 지역성 (Locality)

**시간적 지역성 (Temporal Locality)**

한 번 접근된 데이터는 가까운 미래에 또 접근될 가능성이 높다는 것을 의미

**공간적 지역성 (Spatial Locality)**

특정 데이터를 접근했다면 해당 데이터와 가까운 주소에 있는 데이터도 접근될 가능성이 높다는 것을 의미

### 캐시 라인 (Caching Line)

캐시에서 데이터를 저장할 때 특정 크기의 바이트 묶음으로 저장하는데 이를 캐시 라인이라 한다.

메인 메모리와 캐시 사이의 매핑 방식에는 여러 가지가 있다

- **Direct Mapping**
  - cache의 한 위치에는 하나의 block만 들어갈 수 있는 방식이다
- **N-Way Set Associative Mapping**
  - cache의 몇 개의 block을 묶어서 set을 구성하고 cache는 N개의 set으로 이루어져 있다
    - e.g. 2, 4, 8, 16-Way
  - 만약 2-way라면 2개의 블락이 하나의 set에 동시에 들어갈 수 있게 되는 것이다. 그리고 이 경우 자유도가 2이므로 set을 찾은 후 2개의 block을 모두 확인해야 한다. 이 때 block을 찾는 것을 병렬적으로 이루어지기 때문에 hit time이 증가하지는 않는다
- **Fully Associative Mapping**

  - 최대의 자유도를 주는 매핑 방식으로 캐시 메모리의 빈공간에 마음대로 주소를 저장하는 방식이다

- 추가 자료 : [https://gofo-coding.tistory.com/entry/5-Set-Associative-Mapping](https://gofo-coding.tistory.com/entry/5-Set-Associative-Mapping)

## 가상 메모리

### 가상 메모리가 등장 배경은?

- 프로그램의 크기가 메인 메모리의 크기보다 큰 경우에 사용
- 필요한 block만 메인 메모리에 적재하고, 나머지 block은 디스크(swap device)에서 관리

### MMU가 하는일

- 메모리에 직접 접근하여 가상 주소를 실제 주소로 변환

### Demand Paging이란?

- 특정 페이지가 사용될 때 적재하는 기법
- 반대로 pre-paging은 참조 가능성이 높은 페이지를 적재하는 기법(현실적으로 예측 불가)

### 페이지 교체 알고리즘

- FIFO : Queue 자료구조와 같이 선입 선출 방법
- LRU : 가장 최근에 사용되지 않은 페이지 교체(단, page hit 경우에는 update)
- LFU : 가장 적게 사용 된 페이지 교체
- NUR : 참조 및 수정 비트를 두어 4단계 우선순위 그룹으로 나누어 관리
  - 수정 비트 1인 경우 swap-out 하는 과정에서 update의 시간 발생

### Page Fault를 줄이는 방법은?

- 시스템에 적절한 페이지 교체 알고리즘을 사용한다.
- 쓰래싱을 예방한다.
  1. Working Set Model 사용
  2. PFF(Page Fault Frequency) 알고리즘 사용
  3. 프로세스가 필요한 최소한의 프레임 갯수를 보장

### displacement가 존재하는 이유

- 가상 주소(페이지 블록 주소)를 물리적 주소(프레임 넘버의 몇 번째 블록)의 블록 주소로 맵핑하기 위함.

## I/O

### DMA(Direct Memory Access)란?

- CPU의 도움 없이 메모리에 접근하여, 인터럽트 I/O로 인한 CPU의 작업 중지 횟수를 줄여주기 위한 장치이다.
- CPU는 source 및 destination의 주소를 알려주면 DMAC(DMA Controller)에게 알려주면, DMAC는 메모리에 접근할 수 있어 CPU의 작업이 중단되지 않아도 된다.
- DMAC가 I/O 컨트롤러의 Local Buffer에 데이터가 가득 차면 메인 메모리로 데이터를 옮기고 이 과정에 모두 마무리 되면 CPU에 완료 인터럽트를 발생시킨다.

### Blocking vs Non-Blocking(기술적 관점)

- Blocking: 제어권이 다른 작업에 뺐긴 경우, 해당 작업이 완료 된 이후에 자신의 작업일 이어 나가는 방식. **기다리는 동안 자신은 어떠한 작업도 못함(블록)**
- Non-Blocking: 제어권이 다른 작업에 뺏긴 경우에도 자신의 작업을 이어 나가는 방식.

### Synchronous vs Asynchronous(추상적 관점)

- Synchronous: 제어권이 다른 작업에 뺏긴 경우, 관련 없는 다른 작업을 처리하다가 결과값을 받은 경우 **바로 작업을 이어 나가는 방식**
- Asynchronous: 제어권이 다른 작업에 뺏긴 경우, 다른 작업을 하다가 결과값을 받은 경우 **바로 혹은 나중에 작업을 이어 나가는 방식**

블록, 논블록, 동기, 비동기 조합을 이해하기 위해 유명한 [우체국 예시](https://okky.kr/article/442803)를 확인해보자.

## 파일 시스템

### 파일 시스템의 구조

- 파일 시스템은 파티션 단위로 적용된다.
- 파일 시스템은 파일 관리 정책에 영향을 받기 때문에 운영체제 마다 다르다.
- 윈도우는 FAT, 리눅스는 ext3, ext4 등을 채택하고 있다.
- 단, 루트 디렉토리를 기준으로 디렉토리들은 트리 구조로 되어있다.

### 파일 시스템 레이어

1. logical file system : 실제 데이터(내용)을 제외한 정보를 담고 있는 Meta data를 가지고 있다. Directory 구조를 관리하며 FCB를 가지고 있다.
2. file-organiztion module : 물리 블럭과 논리 블럭을 알고 있어서 논리 주소를 물리주소로 변경해 준다.
3. basic file system : 적절한 드라이브를 설정하여 물리 블록에 데이터를 읽기/쓰기 등의 명령을 내린다.
4. I/O control : Device driver와 interrupt handler로 이루어 져있으며 고수준 언어의 명령을 디바이스에 맞는 저수준 언어로 변경해 준다.

### 파일 시스템 구현 방식은?

1. 디스크 공간 할당 방법
   - 연속된 공간 할당
     - 파일의 크기가 변하는 경우 문제 발생
   - 비연속 공간 할당(linked list)
     - 직접 탐색 시간에 비효율적, 포인터 저장 공간 발생
2. 디스크 빈 공간 확인 방법
   - Bit Vector : 모든 블록의 사용 여부를 0,1 flag로 관리
   - Linked List : 빈 블록 끼리Linked List 자료 구조로 관리
   - Grouping : n개의 빈 블록을 그룹을 묶은 후, Linked List로 관리
   - Counting : 연속된 빈 블록을 Table 형태로 관리(key: 빈 블록 번호, value : 빈 블록 이후 몇 개의 빈 블록이 존재하는지)
