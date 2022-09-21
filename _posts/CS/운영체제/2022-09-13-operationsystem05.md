---
title: 운영체제-CPU-스케쥴링
author: 박재경
date: 2022-09-13
categories: [CS, 운영-체제]
tag: [cs, 운영-체제]
---


# 5장-CPU-스케줄링

[강의 링크](http://www.kocw.net/home/search/kemView.do?kemId=1046323)

<br>

## 1. CPU Scheduling의 필요성

### 1) CPU burst, I/O burst

![image-20220906183630032](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220906183630032.png)



- 프로그램 실행 중 주어진 명령문의 종류에 따라 구분한다.
  - I/O burst: I/O를 실행하고 있는 단계
  - CPU burst: CPU만 연속적으로 사용하면서 명령어를 실행하는 단계
- CPU burst와 I/O burst를 번갈아가면서 프로그램을 실행한다. 

<br>

### 2) CPU -burst Time

![image-20220906183732999](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220906183732999.png)

- 여러 종류의 job(=process)이 섞여 있기 때문에 CPU 스케쥴링이 필요하다.
  - CPU과 I/O 장치 등 시스템 자원을 골고루 효율적으로 사용해야 한다. 
- I/O bound job
  - interactive한 job이 많다. (interative job에게 적절한 response 제공 요망)
  - CPU를 잡고 계산하는 시간 보다 I/O에 많은 시간이 필요한 job이다. 
  -  I/O 요청이 주를 이루어 CPU를 사용하는 시간은 짧지만 많은 명령을 수행한다.(빈도가 높다)
  - many short CPU bursts (large number of short CPU bursts)
- CPU bound job
  - 계산위주의 job이다.
  - few very long CPU bursts (small number of long CPU bursts)

<br>

## 2. CPU Scheduler

### 1) 기본 개념

> Ready 상태의 프로세스 중에서 이번에 CPU를 줄 프로세스를 고른다.
{: .prompt-tip }

CPU를 누구에게 줄 지 결정했으면, Dispatcher를 통해 CPU제어권을 CPU Scheduler에 의해  프로세스를 넘긴다. 이 과정을 문맥 교환이라고 한다.

CPU 스케줄링이 필요한 경우는 프로세스에게 다음과 같은 상태 변화가 있는 경우이다. 

- Running =>  Blocked (예: I/O 요청하는 시스템 콜) `nonpreemptive (자발적)`
- Running => Ready (예: 할당시간 만료로 timer interrupt) `preemptive (비자발적)`
- Blocked => Ready (예: I/O 완료 후 인터럽트) `preemptive(비자발적)`
- Terminate `nonpreemptive(자발적)`

 <br>

### 2) CPU 성능 척도

> 성능 척도에 따라 어떤 스케쥴링 알고리즘이 우수한지 판단한다. 
{: .prompt-tip }

| CPU 성능 척도                | 설명                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| CPU 이용률 (CPU Utilization) | 쉬지 않고 얼마나 일을 했는가.<br />전체 시간 대비 이용률     |
| 처리량 (Throughput)          | 몇 개의 일을 처리했는가. <br />주어진 시간에 얼마나 일을 처리했는가. |
| 소요 시간 (Turnaround Time)  | CPU를 사용하러 와서 CPU burst를 다하고 나가는 시간           |
| 대기 시간 (Wating Time)      | ready 큐에서 줄 서서 기다리는 시간                           |
| 응답 시간 (Response Time)    | ready 큐에 들어와서 처음으로 CPU를 얻기까지 걸리는 시간<br />timeSharing환경에서 중요하기에 대기 시간과 별도로 따로 따진다. |

<br>

### 3) CPU 알고리즘

- 비선점형 (Non-Preemptrive)
  - 특정 프로세스의 작업이 끝날 때 까지 CPU를 독점한다. 
  - 빼앗을 수 없다. 
- 선점 (Preemitive)
  - 하나의 프로세스가 CPU를 점유할 떄 다른 프로세스가 CPU를 빼앗을 수 있다. 
  - timer를 활용 

<br>

| CPU 스케쥴링                         | 분류                      | 설명                                                         |
| ------------------------------------ | ------------------------- | ------------------------------------------------------------ |
| FCFS <br />(First-Come First-Served) | `비선점형`                | 프로세스의 도착 순서에 따라서 실행한다. (선입선출법)<br />`Conboy Effect(호위 효과)`: short process after long process<br />먼저 도착한 프로세스의 burst시간이 길면 뒤에 있는 프로세스가 매우 오랜 시간을 기다려야 할 수 있다. |
| SJF <br />(Shorest-Job-First)        | `비 선점형`<br />`선점형` | 각 프로세스의 다음번 CPU burst time을 가지고 CPU에 활용한다.<br />CPU burst가 가장 짧은 프로세스를 먼저 스케쥴한다. <br />주어진 프로세스들에 대해 minimum average waiting time을 보장한다. <br />- `비 선점형` : 일단 CPU를 잡으면 CPU burst가 완료될 때까지 CPU를 선점 당하지 않는다. <br />- `선점형`:현재 수행중인 프로세스의 남은 burst time보다 더 짧은 CPU burst time을 가지는 <br />새로운 프로세스가 도착하면 CPU를 빼앗긴다. (shortest Remaining TIme First)<br /><br />`Starvation 현상`: long process는 평생 CPU를 사용하지 못할 수도 있다... 🙃<br />`다음 CPU burst 예측`: CPU burst타임을 미리 알수는 없지만 추정한다. (과거의 기록을 활용) |
| Priority                             |                           | 높은 우선순위를 가진 Process에 할당한다. <br />SJF는 일종의 priority 스케쥴링이다. (predicted net CPU burstTime)<br /><br />`Starvation 현상`: 우선 순위가 낮은 프로세스는 평생 CPU를 사용하지 못할 수도 있다... 🙃<br />`Aging`: 기아 현상을 해결하기 위한 방법으로 대기 시간이 길어지면 우선순위를 높여준다. |
| Round Robin<br />(RR)                | `선점형`                  | 현대에서 사용하는 CPU 스케쥴링의 기반<br />각 프로세스는 동일한 크기의 할당 시간을 가진다. <br />할당 시간이 지나면 프로세스는 선점당하고 레디큐의 제일 뒤에가서 다시 줄을 슨다.<br />예측할 필요가 없고 응답시간이 빠르다는 장점이 있다. <br />대기시간이 cpu를 사용하는 시간에 비례하여 증가한다. <br /><br />할당시간이 너무 크게 설정하면 성능이 FCFS와 같아진다. <br />할당시간이 너무 적으면 문맥 교환 오버헤드가 커진다. |

<br>

### 4) Real-Time Scheduling

- Hard real-time systems
  - Hard real-time task는 정해진 시간 안에 반드시 끝내도록 스케쥴링 해야 한다.
  - 데드라인을 꼭 지켜야 한다.
- Soft real-time computing
  - Soft real-time task는 일반 프로레스에 비해 높은 우선순위를 갖도록 해야 한다. 

<br>

### 5) Thread Scheduling

- Local Scheduling
  - User level thread의 경우 사용자 수준의 thread library에 의해 어떤 thread를 스케쥴할지 결정한다.
  - OS가 아니라 사용자 프로세스가 직접 결정한다. 
- Global Sheduling
  - Kernel level thread의 경우 일반 프로세스와 마찬 가지로 커널의 단기 스케쥴러가 어떤 thread를 스케쥴을 할지 결정한다.
  - 프로세스 스케쥴링 하듯이 OS가 알고리즘에 근거해서 스케쥴링을 한다. 

<br>

### 6) Algorithm 평가

- Queueing models
  - 이론가들이 사용하는 방법
  - 확률 분포로 주어지는 arrival rate와 service rate 등을 통해 각종 perfomance index를 계산한다.
- Implemetation(구현), Measurement(성능 측정)
  - 실제 시스템에 알고리즘을 구현하여 실제 작업(workload)에 대해서 성능을 측정하고 비교한다.
- Simulation (모의 실험)
  - 알고리즘을 모의 프로그램으로 작성 후 trace를 입력으로 하여 결과를 비교한다. 

<br>

## 3. Ready Queue

### 1) Multilevel Queue

> Ready Queue를 여러개로 분할되어 있다. 
{: .prompt-tip }

<br>

![image-20220913165757043](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220913165757043.png)

프로세스의 우선 순위에 따라 큐가 정해진다. 큐에 대한 스케쥴링이 필요하다. 각 큐에 CPU time을 적절한 비율로 할당한다. 

- foreground queue
  - interactive
  - RR 스케쥴링 알고리즘을 사용한다. 
- background queue
  - batch - no human interaction
  - FCFS 스케쥴링 알고리즘을 사용한다. 

<br>

### 2) Multilevel Feedback Queue

<br>

![image-20220913170907875](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220913170907875.png)

<br>

- Multilevel Queue와 마찬가지로 프로세스 우선 순위에 따라 큐가 정해진다. 
- 프로세스가 다른 큐로 이동이 가능하다. 
- 에이징을 이와 같은 방식으로 구현할 수 있다. 
- Multilevel-feedback-queue scheduler를 정의하는 파라미터들
  - Queue의 수
  - 각 큐의 scheduling algorithm
  - Process를 상위 큐로 보내는 기준
  - Process를 하위 큐로 내쫓는 기준
  - 프로세스가 CPU 서비스를 받으려 할 떄 들어갈 큐를 결정하는 기준 

<br>

## 4.  Multiple-Processor Scheduling

> CPU가 여러 개인 경우 스케쥴링은 더욱 복잡해진다. 
{: .prompt-tip }

- Homogeneous Process인 경우
  - Queue에 한 줄로 세워서 각 프로세서가 알아서 꺼내가게 할 수 있다.
  - 반드시 특정 프로세서에서 수행되어야 하는 프로세스가 있는 경우에는 문제가 더 복잡해진다.
- Load Sharing
  - 일부 프로세서에 job이 몰리지 않도록 부하를 적절히 공유하는 메커니즘이 필요하다.
  - 별개의 큐를 두는 방법 vs 공동 큐를 사용하는 방법
- Symmetric Multiprocessing (SMP)
  - 모든 CPU가 대등하다. 
  - 각 프로세서가 각자 알아서 스케쥴링을 결정한다.
- Asymmetric Multiprocessing
  - 하나의 CPU가 전체적인 컨트롤을 담당한다. 
  - 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지 프로세서는 거기에 따른다. 
