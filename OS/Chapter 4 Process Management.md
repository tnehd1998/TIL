모두를 위한 열린 강의 KOCW의 이화여자대학교의 [반효경 교수님의 2014년도 강의](http://www.kocw.net/home/search/kemView.do?kemId=1046323)를 듣고 정리한 내용입니다.

# 프로세스 생성 (Process Creation)

- 부모 프로세스 (Parent process)가 자식 프로세스(children process) 생성 (fork)
- 프로세스의 트리(계층 구조)형성
- 프로세스는 자원을 필요로 함
  - 운영체제로 받는다
  - 부모와 공유한다
- 자원의 공유
  - 부모와 자식의 모든 자원을 공유하는 모델
  - 일부를 공유하는 모델
  - 전혀 공유하지 않는 모델
- 수행 (Execution)
  - 부모와 자식은 공존하며 수행되는 모델
  - 자식이 종료(terminate)될 때까지 부모가 기다리는(wait) 모델
- 주소 공간 (Address space)
  - 자식은 부모의 공간을 복사함 (binary and OS data)
  - 자식은 그 공간에 새로운 프로그램을 올림
- 유닉스의 예
  - fork() 시스템 콜이 새로운 프로세스를 생성
    - 부모를 그대로 복사 (OS data except PID + binary)
    - 주소 공간 할당
  - Fork 다음에 이어지는 exec() 시스템 콜을 통해 새로운 프로그램을 메모리에 올림

# 프로세스 종료 (Process Termination)

- 프로세스가 마지막 명령을 수행한 후 운영체제에게 이를 알려줌 (exit)
  - 자식이 부모에게 output data를 보냄 (via wait)
  - 프로세스의 각종 자원들이 운영체제에게 반납됨
- 부모 프로세스가 자식의 수행을 종료시킴 (abort)
  - 자식이 할당 자원의 한계치를 넘어섬
  - 자식에게 할당된 태스크가 더 이상 필요하지 않음
  - 부모가 종료(exit)하는 경우
    - 운영체제는 부모 프로세스가 종료하는 경우 자식이 더 이상 수행되도록 두지 않는다.
    - 단계적인 종료

# 프로세스와 관련한 시스템 콜

### fork() 시스템 콜

- fork() 시스템 콜을 통해 자신의 복사본인 자식 프로세스를 생성한다.
- A process is created by the fork() system call.
  - 부모의 복사본인 자식 프로세스를 새로운 주소값과 함께 생성한다.
  - Creates a new address space that is a duplicate of the caller

### exec() 시스템 콜

- exec() 시스템 콜을 통해 프로세스는 다른 프로그램을 호출한다.
- A process can execute a different program by the exec() system call.
  - 기존의 프로그램을 다른 프로그램으로 대체한다.
  - Replaces the memory image of the caller with a new program.

### wait() 시스템 콜

- 프로세스 A가 wait() 시스템 콜을 호출하면
  - 커널은 child가 종료될 때까지 프로세스 A를 sleep시킨다 (Block 상태)
  - Child process가 종료되면 커널은 프로세스 A를 깨운다 (Ready 상태)

### exit() 시스템 콜

- 프로세스의 종료
  - 자발적 종료
    - 마지막 statement 수행 후 exit() 시스템 콜을 통해
    - 프로그램에 명시적으로 적어주지 않아도 main 함수가 리턴되는 위치에 컴파일러가 넣어줌
  - 비자발적 종료
    - 부모 프로세스가 자식 프로세스를 강제 종료시킴
      - 자식 프로세스가 한계치를 넘어서는 자원 요청
      - 자식에게 할당된 태스크가 더 이상 필요하지 않음
    - 키보드로 kill, break 등을 친 경우
    - 부모가 종료하는 경우
      - 부모 프로세스가 종료하기 전에 자식들이 먼저 종료됨

# 프로세스 간 협력

### 독립적 프로세스 (Independent process)

- 프로세스는 각자의 주소 공간을 가지고 수행되므로 원칙적으로 하나의 프로세스는 다른 프로세스의 수행에 영향을 미치지 못함

### 협력 프로세스 (Cooperating process)

- 프로세스 협력 메커니즘을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있음

### 프로세스 간 협력 메커니즘(IPC: Interprocess Communication)

- 메세지를 전달하는 방법
  - Message passing : 커널을 통해 메세지 전달
- 주소 공간을 공유하는 방법
  - Shared memory : 서로 다른 프로세스 간에도 일부 주소 공간을 공유하게 하는 shared memory 메커니즘이 있음
  - Thread : thread는 사실상 하나의 프로세스이므로 프로세스 간 협력으로 보기는 어렵지만 동일한 process를 구성하는 thread들 간에는 주소 공간을 공유하므로 협력이 가능

# Message Passing

- Message system : 프로세스 사이에 공유 변수(shared variable)를 일체 사용하지 않고 통신하는 시스템
- Direct communication : 통신하려는 프로세스의 이름을 명시적으로 표시
- Indirect communication : mailbox (또는 port)를 통해 메세지를 간접 전달
