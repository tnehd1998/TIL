# Race Condition

- 여러 프로세스들이 동시에 공유 데이터를 접근하는 상황
- 데이터의 최종 연산 결과는 마지막에 그 데이터를 다룬 프로세스에 따라 달라짐
- 멀티프로세스 환경에서 특정 프로세스 A가 공유되고 있는 메모리의 데이터를 가져간 후, 프로세스 A가 완료한 작업을 반영하기 전 다른 프로세스 B가 반영되기 전의 데이터를 가져가 데이터 불일치가 일어나는 상황
  - 해당 문제를 해결하기 위해 조율을 해줘야 함

# OS에서 race condition은 언제 발생하는가?

1. Kernel 수행 중 인터럽트 발생 시
2. Process가 System call을 하여 kernel mode로 수행 중인데 context switch가 일어나는 경우
3. Multiprocessor에서 shared memory 내의 kernel data

# Process Synchronization 문제

- 공유 데이터 (shared data)의 동시 접근 (concurrent access)은 데이터의 불일치 문제(inconsistency)를 발생시킬 수 있다.
- 일관성 (consistency) 유지를 위해서는 협력 프로세스 (cooperating process) 간의 실행 순서 (orderly execution)를 정해주는 매커니즘 필요
- Race condition을 막기 위해서는 concurrent process는 동기화 (synchronize)되어야 한다.

# The Critical-Section Problem

- n개의 프로세스가 공유 데이터를 동시에 사용하기를 원하는 경우
- 각 프로세스의 code segment에는 공유 데이터를 접근하는 코드인 critical section이 존재
- Problem
  - 하나의 프로세스가 critical section에 있을 때 다른 모든 프로세스는 critical section에 들어갈 수 없어야 한다.

# 프로그램적 해결법의 충족 조건

### Mutual Exclusion

- 프로세스 Pi가 critical section 부분을 수행 중이면 다른 모든 프로세스들은 그들의 critical section에 들어가면 안 된다

### Progress

- 아무도 critical section에 있지 않은 상태에서 critical section에 들어가고자 하는 프로세스가 있으면 critical section에 들어가게 해주어야 한다.

### Bounded Waiting

- 프로세스가 critical section에 들어가려고 요청한 후부터 그 요청이 허용될 때까지 다른 프로세스들이 critical section에 들어가는 횟수에 한계가 있어야 한다

# Semaphores

- 앞에 설명한 방식들을 추상화시킴
- Semaphore S
  - Integer variable
  - 두가지 atomic 연산에 의해서만 접근 가능
    1. P(S) : lock을 거는 기능, 자원을 획득하는 과정
    2. V(S) : lock을 푸는 기능, 자원을 반납하는 과정

# Busy-wait vs Block/wakeup

- Critical section의 길이가 긴 경우 Block/Wakeup이 적당
- Critical section의 길이가 매우 짧은 경우 Block/Wakeup 오버헤드가 busy-wait 오버헤드보다 더 커질 수 있음
- 일반적으로는 Block/wakeup 방식이 더 좋음

# Two Types of Semaphores

- Counting semaphore
  - 도메인이 0 이상인 임의의 정수값
  - 주로 resource counting에 사용
- Binary semaphore (=mutex)
  - 0 또는 1 값만 가질 수 있는 semaphore
  - 주로 mutual exclusion (lock/unlock)에 사용

# Deadlock and Starvation

- Deadlock : 둘 이상의 프로세스가 서로 상대방에 의해 충족될 수 있는 event를 무한히 기다리는 현상
- Starvation : indefinite blocking, 프로세스가 suspend된 이유에 해당하는 세마포어 큐에서 빠져나갈 수 없는 현상

# Classical Problems of Synchronization

- Bounded-Buffer Problem (Producer-Consumer Problem)
  - 데이터를 생성하는 producer와 데이터를 꺼내서 소비하는 consumer가 존재하는 데, 특정 producers들이 같은 공간에 데이터를 생성하거나, consumers들이 같은 공간의 데이터를 소비하려고 할 때 문제가 발생한다.
  - 버퍼의 크기가 유한한 상황에서, 버퍼가 꽉찼는데 producer가 데이터를 생성하려는 상황과 모든 데이터를 버퍼에서 꺼내가 consumer가 소비할 데이터가 없을 때 문제가 발생한다.
- Readers and Writers Problem
  - 공유 데이터인 DB를 데이터를 작성하는 writer가 동시에 접근하는 경우에 문제가 발생한다. lock을 걸어서 하나의 프로세스만 접근하도록 설정해줘야 한다.
  - 데이터를 읽어오는 reader의 경우는 동시에 다른 프로세스가 접근해서 사용해도 아무런 문제가 발생하지 않음
- Dining-Philosophers Problem
  - deadlock의 가능성이 있음, 모든 철학자가 동시에 배가 고파져 왼쪽 젓가락을 집어버리는 경우가 존재함

# Semaphore의 문제점

- 코딩하기 힘들다
- 정확성(correctness)의 입증이 어렵다
- 자발적 협력(voluntary cooperation)이 필요하다
- 한번의 실수가 모든 시스템에 치명적 영향

# Monitor

- 동시 수행중인 프로세스 사이에서 abstract data type의 안전한 공유를 보장하기 위한 high-level synchronization construct
- 모니터 내에서는 한번에 하나의 프로세스만이 활동 가능
- 프로그래머가 동기화 제약 조건을 명시적으로 코딩할 필요없음
- 프로세스가 모니터 안에서 기다릴 수 있도록 하기 위해 condition variable 사용
- Condition variable은 wait와 signal 연산에 의해서만 접근 가능
