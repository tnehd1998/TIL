모두를 위한 열린 강의 KOCW의 이화여자대학교의 [반효경 교수님의 2014년도 강의](http://www.kocw.net/home/search/kemView.do?kemId=1046323)를 듣고 정리한 내용입니다.

# 컴퓨터 + 입출력 장치

- 컴퓨터 : CPU + Memory
- 입출력 장치 : Disk, 키보드, 마우스, 프린터, 모니터 …

# 입출력 장치

- 각 입출력 장치에는 CPU같은 역할을 하는 device controller가 붙어 있음
- 각 입출력 장치에는 각각의 작업 공간인 local buffer가 존재한다.

# CPU

- Register : 메모리보다 더 빠르면서 정보를 저장할 수 있는 작은 공간
- Mode bit : 현재 실행되고 있는 것이 운영체제인지 사용자 프로그램인지를 구분해줌
  - 1 : 사용자 모드, 사용자 프로그램 수행
  - 0 : 모니터 모드 (커널 모드, 시스템 모드), OS 코드 수행
- Interrupt line : interrupt가 걸려 중단된 작업을 작성해놓는 공간

# Timer

- CPU를 특정 프로그램이 독점하는 것을 방지하는 기능
- 정해진 시간이 흐르면 CPU의 제어권이 넘어가도록 interrupt를 발생 시킴
  - 타이머는 매 클럭 틱 때마다 1씩 감소
  - 0이 되면 인터럽트 발생

# Device Controller

- I/O device controller
  - 해당 I/O 장치유형을 관리하는 일종의 작은 CPU
  - 제어 정보를 위해 control register, status register를 가짐
  - Local buffer를 가짐 (일종의 data register)
- I/O는 실제 device와 local buffer 사이에서 일어남
- Device controller는 I/O가 끝났을 경우 interrupt로 CPU에 그 사실을 알림

- Device driver (장치구동기) : OS 코드 중 각 장치별 처리 루틴 -> software
- Device controller (장치제어기) : 각 장치를 통제하는 일종의 작은 CPU -> hardware

# DMA (Direct Memory Access) controller

- 직접 메모리를 접근할 수 있는 controller
- I/O 장치가 interrupt를 자주 걸게되면 CPU가 많은 방해를 받기 때문에 해당 경우를 방지하기 위해 CPU 대신 I/O device의 내용을 메모리에 복사해주는 역할을 함
  - 해당 상황을 통해 CPU가 interrupt 걸리는 빈도수를 줄이기에 CPU를 더 효율적으로 사용할 수 있게 해줌
- 빠른 입출력 장치를 메모리에 가까운 속도로 처리하기 위해 사용
- CPU의 중재 없이 device controller가 device의 buffer storage의 내용을 메모리에 block 단위로 직접 전송
- 바이트 단위가 아니라 block 단위로 인터럽트를 발생시킴

# Memory controller

- 누가 메모리에 접근할 수 있게 할지 조율하는 controller
  - CPU, DMA controller가 동시에 접근하는 경우 조율을 함

# 입출력(I/O)의 수행

- 사용자 프로그램의 I/O 방법
  - 시스템콜(system call) : 사용자 프로그램은 운영체제에게 I/O 요청
  - trap을 사용하여 인터럽트 벡터의 특정 위치로 이동
  - 제어권이 인터럽트 벡터가 가리키는 인터럽트 서비스 루틴으로 이동
  - 올바른 I/O 요청인지 확인 후 I/O 수행
  - I/O 완료 시 제어권을 시스템콜 다음 명령으로 옮김

# Interrupt

- 인터럽트를 당한 시점의 register와 program counter를 save한 후 CPU의 제어를 인터럽트 처리 루틴에 넘긴다.
- Interrupt의 넓은 의미
  - Interrupt (하드웨어 인터럽트) : 하드웨어가 발생시킨 인터럽트
  - Trap (소프트웨어 인터럽트)
    - Exception : 프로그램이 오류를 범함 경우
    - System call : 프로그램이 커널 함수를 호출하는 경우
- 인터럽트 관련 용어
  - 인터럽트 벡터 : 해당 인터럽트의 처리 루틴 주소를 가지고 있음
  - 인터럽트 처리 루틴 (Interrupt Service Routine, 인터럽트 핸들러) : 해당 인터럽트를 처리하는 커널 함수

# System Call

- 사용자 프로그램이 운영체제의 서비스를 받기 위해 커널 함수를 호출하는 것

# 동기식 입출력과 비동기식 입출력

- 동기식 입출력 (Synchronous I/O)
  - I/O 요청 후 입출력 작업이 완료된 후에야 제어가 사용자 프로그램에 넘어감
  - 구현 방법 1
    - I/O가 끝날 때까지 CPU를 낭비시킴
    - 매시점 하나의 I/O만 일어날 수 있음
  - 구현 방법 2
    - I/O가 완료될 때까지 해당 프로그램에게서 CPU를 빼앗음
    - I/O 처리를 기다리는 줄에 그 프로그램을 줄 세움
    - 다른 프로그램에게 CPU를 줌
- 비동기식 입출력 (Asynchronous I/O)
  - I/O가 시작된 후 입출력 작업이 끝나기를 기다리지 않고 제어가 사용자 프로그램에 즉시 넘어감
- 두 경우 모두 I/O의 완료는 인터럽트로 알려줌

# 서로 다른 입출력 명령어

- 두가지 메모리 접근 방식이 존재함
  - I/O를 수행하는 special instruction
    - 메모리 접근과 I/O 접근이 분리되어 있음
  - Memory Mapped I/O
    - 메모리와 I/O 접근이 연결되어 있음

# 저장장치 계층 구조

- Primary(Executable) : Registers (1), Cache Memory (2), Main Memory (3)
  - Primary의 특징 : 속도가 빠르고, 저장 공간이 작으며, 휘발성(전원이 나가면 저장되지 않는)이며, CPU가 접근 할 수 있다.
- Secondary : Magnetic Disk (4), Optical Disk (5), Magnetic Tape (6)
  - Secondary의 특징 : 속도가 느리며, 저장 공간이 크며, 비휘발성이며, CPU가 접근할 수 없다.
- Caching : copying information into faster storage system, 데이터의 재사용을 위해 속도가 더 빠른 저장 공간에 데이터를 저장하는 행위

# 커널 주소 공간의 내용

- Code (커널 코드)
  - 시스템콜, 인터럽트 처리 코드
  - 자원 관리를 위한 코드
  - 편리한 서비스 제공을 위한 코드
- Data
  - PCB, CPU, Memory, Disk를 저장하는 자료구조
- Stack
  - Ex) Process A의 커널 스택
  - Ex) Process B의 커널 스택

# 사용자 프로그램이 사용하는 함수

- 사용자 정의 함수 : 자신의 프로그램에서 정의한 함수
- 라이브러리 함수 : 자신의 프로그램에서 정의하지 않고 가져다 쓴 함수, 자신의 프로그램의 실행 파일에 포함되어 있다.
- 커널 함수 : 운영체제 프로그램의 함수, 커널 함수의 호출 = 시스템 콜
