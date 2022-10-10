# The Deadlock Problem

- Deadlock : 일련의 프로세스들이 서로가 가진 자원을 기다리며 block된 상태
- Resource(자원)
  - 하드웨어, 소프트웨어 등을 포함하는 개념
  - (예) I/O Device, CPU cycle, memory space, semaphore 등
  - 프로세스가 자원을 사용하는 절차
    - Request, Allocate, Use, Release

# Deadlock 발생의 4가지 조건

- Mutual exclusion (상호 배제)
  - 매 순간 하나의 프로세스만이 자원을 사용할 수 있음
- No preemption (비선점)
  - 프로세스는 자원을 스스로 내어놓을 뿐 강제로 빼앗기지 않음
- Hold and wait (보유 대기)
  - 자원을 가진 프로세스가 다른 자원을 기다릴 때 보유 자원을 놓지 않고 계속 가지고 있음
- Circular wait (순환 대기)
  - 자원을 기다리는 프로세스간에 사이클이 형성되어야 함

# Deadlock의 처리 방법

### Deadlock Prevention

- 자원 할당 시 Deadlock의 4가지 필요 조건 중 어느 하나가 만족되지 않도록 하는 것

### Deadlock Avoidance

- 자원 요청에 대한 부가적인 정보를 이용해서 deadlock의 가능성이 없는 경우에만 자원을 할당
  - 시스템 state가 원래 state로 돌아올 수 있는 경우에만 자원 할당

### Deadlock Detection and recovery

- Deadlock 발생은 허용하되 그에 대한 detection 루틴을 두어 deadlock 발견시 recover

### Deadlock Ignorance

- Deadlock을 시스템이 책임지지 않음
- UNIX를 포함한 대부분의 OS가 채택

# Deadlock Detection and Recovery

- Recovery
  - Process termination
    - Abort all deadlocked processes
    - Abort one process at a time until the deadlock cycle is eliminated
  - Resource Preemption
    - 비용을 최소화할 victim의 선정
    - Safe state로 rollback하여 process를 restart
    - Starvation 문제
      - 동일한 프로세스가 계속해서 victim으로 선정되는 경우
      - Cost factor에 rollback 횟수도 같이 고려

# Deadlock Ignorance

- Deadlock이 일어나지 않는다고 생각하고 아무런 조치도 취하지 않음
  - Deadlock이 매우 드물게 발생하므로 deadlock에 대한 조치 자체가 더 큰 overhead일 수 있음
  - 만약, 시스템에 deadlock이 발생한 경우 시스템이 비정상적으로 작동하는 것을 사람이 느낀 후 직접 process를 죽이는 등의 방법으로 대체
  - UNIX, Windows 등 대부분의 범용 OS가 채택
