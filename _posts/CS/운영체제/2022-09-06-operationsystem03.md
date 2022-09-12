---
title: 운영체제-프로세스
author: 박재경
date: 2022-09-06
categories: [CS, 운영-체제]
tag: [cs, 운영-체제]
---



# 3장-프로세스

[강의 링크](http://www.kocw.net/home/search/kemView.do?kemId=1046323)

<br>

## 1. 프로세스 기본 개념

> 실행 중인 프로그램. (Process is a program in execution)
>
> 운영체제가 프로그램을 메모리에 할당하여 실행하면 이를 프로세스라고 한다. 
{: .prompt-tip }

- 프로세스의 문맥 (context): **프로세스의 현재 상태를 나타내는 데 필요한 요소**
  - CPU 수행 상태를 나타내는 하드웨어 문맥
    - 프로세스는 CPU를 잡고 매 순간 instrtuction을 실행하기 때문에 현재 얼마나 instruction을 실행 했는 지를 확인하기 위해 
    - Program Counter: 현재 어디를 가르키는가
    - 각종 register: register가 어떤 값을 가지고 있는가
  - 프로세스의 주소 공간
    - code, data, stack: 어떤 내용이 들어 있는가
  - 프로세스 관련 커널 자료 구조
    - PCB (Process Control Block): 해당 프로세스의 PCB
    - Kernel stack: 해당 프로세스의 커널 스택

프로세스들이 time-sharing하면서 계속 번갈아 가면서 실행되기 때문에 프로세스의 문맥을 파악해야 다시 실행할 수 있다.  

<br>

### 1) 프로세스의 상태 (Process State)

> 프로세스는 상태가 변경되며 수행된다.
{: .prompt-tip }

<br>

![image-20220906085911207](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220906085911207.png)

<br>

- Running
  - CPU를 잡고 instruction을 수행 중인 상태
  - usermode, monitor mode
- Ready (in memory)
  - CPU를 기다리는 상태 
  - 모든 준비는 끝나있고 CPU만 할당하면 instruction을 실행할 수 있다. 
  - 메모리에 물리적으로 올라가 있는 상태이다.
- Blocked (wait, sleep)
  - CPU를 주어도 당장 isntrtuction을 수행할 수 없는 상태
  - Process 자신이 요청한 event (예: I/O)가 즉시 만족되지 않아 이를 기다리는 상태
  - ex) 디스크에서 file을 읽어 와야 하는 경우
  - **자신이 요청한 event가 만족되면 ready된다.**
- Suspended (stopped)
  - 외부적인 이유로 프로세스의 수행이 정지된 상태
  - 프로세스는 통째로 디스크에 swap out 된다.
  - ex) 사용자가 프로그램을 일시 정지 시킨 경우 (break key)
  - ex) 메모리에 너무 많은 프로세스가 올라와 프로세스를 잠시 중단 시킨다.
  - **외부에서 resume을 해주어야 active된다.** 
- New
  - 프로세스가 생성 중인 상태
- Terminated
  - 수행이 끝난 상태 

<br>

### 2) Process Control Block (PCB)

> 운영체제가 각 프로세스를 관리하기 위해 프로세스 당 유지하는 정보 (data부분에 할당되어 있다.)
{: .prompt-tip }

![image-20220906092457498](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220906092457498.png)

<br>

- **OS가 관리상 사용하는 정보**
  - Proces State, Process ID
  - scheduling Information, Priority
- **CPU 수행 관련 하드웨어 값**
  - Program counter, registers
- **메모리 관련**
  - Code, data, stack의 위치 정보
- **파일 관련**
  - Open file descriptors..

<br>

### 3) 문맥 교환 (Context Switch )

> CPU를 한 프로레스에서 다른 프로세스로 넘겨 주는 과정이다.
{: .prompt-tip }

CPU가 다른 프로세스에게 넘어갈 때 운영체제는 다음을 수행한다.

- CPU를 내어주는 프로세스의 상태를 그 프로세스의 PCB에 저장한다.
- CPU를 새롭게 얻는 프로세스의 상태를 PCB에서 읽어온다.

**즉, 다음에 해당 프로세스의 실행을 재개하기 위해서 프로세스 문맥을 저장해야 한다.** 

시스템 콜이나 인터럽트 발생 시 반드시 문맥 교환이 발생하는 것은 아니고 다른 프로세스에 CPU를 넘겨주어야 문맥 교환이 발생하는 것을 유념해야 한다.

<br>

### 4) 프로세스를 스케줄링하기 위한 큐

> 프로세스들은 각 큐들을 오가며 수행된다.
{: .prompt-tip }

- Job queue: 현재 시스템 내에 있는 모든 프로세스의 집합
- Ready queue: 현재 메모리 내에 있으면서 CPU를 잡아서 실행되기를 기다리는 프로세스의 집합
- Device queues: I/O device의 처리를 기다리는 프로세스의 집합

<br>

### 5) 스케줄러

- **Long-tem scheduler (장기 스케줄러 or JOB scheduler)**

  - 시작 프로세스 중 어떤 것들을 ready queue로 보낼 지 결정한다.
  - 프로세스에 momory(및 각종 자원)을 주는 문제
  - degree of Multiprogramming을 제어한다.
    - 메모리에 올라가 있는 프로세스의 수를 제어한다
    - 메모리에 프로그램이 너무 많이 올라가면 성능이 좋지 않다.
    - 메모리에 프로그램이 너무 적게 올라가도 성능이 좋지 않다. 
  - time sharing system에는 보통 장기 스케줄러가 없다. (무조건 ready이다.)

- **Short-term scheduler (단기 스케줄러 or CPU scheduler)**

  - 어떤 프로레스를 다음 번에 running 시킬 지를 결정한다.

  - 프로레스에 CPU를 주는 문제

  - 충분히 빨라야 한다. (ms 단위)

- **Medium-term scheduler (중기 스케줄러 or Swapper)**
  - 여유 공간을 마련 하기 위해서 프로세스를 통쨰로 메모리에서 디스크로 쫓아낸다.
  - 프로세스에게서 memory를 뺏는 문제
  - degree of Multiprogramming을 제어한다. 
    - time sharing system에서는 장기 스케줄러가 없기 때문에 중기 스케줄러로 제어한다. 

<br>

## 2. Thread

> 프로세스(process) 내에서 실행되는 흐름의 단위로 실질적인 CPU 수행 단위라고 생각하면 된다.
>
> 모든 프로세스에는 한 개 이상의 스레드가 존재하여 작업을 수행한다.
{: .prompt-tip }

<br>

![image-20220906190448958](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220906190448958.png)

<br>

- 프로세스 내부에서 Thread들 끼리 공유하는 부분 (task)
  - code section
  - data section
  - OS resources

- 별도로 갖고 있는 것
  - CPU 수행과 관련된 정보 (PC, register)
  - stack section

<br>

### 1) 스레드 사용 이점

<br>

![image-20220906190250544](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220906190250544.png)

<br>

- 멀티 스레드로 구성된 task 구조에서는 하나의 서버 스레드가 blocked(wait)상태인 동안에도 동일한 task 내의 다른 스레드가 실행(running)되어 빠른 처리를 할 수 있다. `빠른 응답속도`
  - ex) 웹 브라우저 사용시 네트워크에서 웹페이지를 읽어오는 동안 해당 스레드가 blocked상태가 되어도 다른 스레드는 일을 할 수 있음.
- 동일한 일을 수행하는 멀티 스레드가 협력하여 높은 처리율과 성능 향상을 얻을 수 있다. `자원 절약, 자원 공유`
  - 동일한 일을 하는 여러 프로세스를 사용하면 프로세스만의 독자적인 주소공간이 만들어져야 하고 이는 메모리를 차지하게 된다. 
  - 각각의 메모리에 code, data, stack이 올라가야 한다. (메모리 낭비가 심하다)
  - ex) 웹브라우저 여러개 띄우는 경우
- 프로세스를 만드는 것과 문맥 교환을 overhead가 비싸다. (30배, 5배) `효율`
  - 쓰레드 생성은 간단하다. 
  - 쓰레드 간의 CPU 스위치는 간단하다. (동일한 주소 공간을 사용하고 있기에)
- 스레드를 사용하면 병렬성을 높일 수 있다. 
  - CPU가 여러 개라면, 병렬적으로 실행할 수 있다. 

<br>

### 2) 스레드 종류

- 커널 스레드 (Kernel Threads)
  - 커널이 스레드의 존재를 안다.  
- 유저 스레드 (User Threads)
  - 커널의 지원을 받지 않고 사용자 차원에서 생성한 스레드이다. 
  - 구현상의 제약점이 있다. 

<br>