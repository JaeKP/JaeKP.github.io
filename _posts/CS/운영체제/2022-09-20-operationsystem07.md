---
title: 운영체제-Deadlocks
author: 박재경
date: 2022-09-20
categories: [CS, 운영-체제]
tag: [cs, 운영-체제]
---


# 7장-Deadlocks (교착 상태)

[강의 링크](http://www.kocw.net/home/search/kemView.do?kemId=1046323)

<br>

## 1. Deadlock Problem

### 1) 정의

> 데드락이란 일련의 프로세스들이 서로가 가진 자원을 기다리며 block된 상태를 의미한다.
{: .prompt-tip }

여기서 자원이란

- 하드웨어, 소프트웨어 등을 포함하는 개념
- ex) I/O Device, CPU Cycle, Memory Space, Semaphore 등

<br>

프로세스가 자원을 사용하는 절차는 크게 4단계를 거치게 된다.

1. 자원 요청 `Request`
2. 자원 할당 `Allocate`
3. 자원 사용 `Use`
4. 자원 반납 `Release`

<br>

### 2) 발생 조건

4가지 조건을 모두 충족시켜야 데드락이 발생한다. 

- Mutual Exclusion (상호 배제)
  - 매 순간 하나의 프로세스만이 자원을 `독점적`으로 사용한다. 
- No Preemption (비선점)
  - 프로세스는 자원을 스스로 내어놓을 뿐 강제로 빼앗기지는 않는다.
- Hold and Wait (보유대기)
  - 자원을 가진 프로세스가 다른 자원을 기다릴 때 보유 자원을 놓지 않고 계속 가지고 있다.
- Circular Wait (자원 대기)
  - 자원을 기다리는 프로세스간에 사이클이 형성되어야 한다. 

<br>

### 3) 자원 할당 그래프

![image-20220920124257153](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220920124257153.png)

- 자원(R) -> 프로세스(P): 해당 자원이 해당 프로세스에 속해있다.
- 프로세스(P) -> 자원(R): 해당 프로세스가 해당 자원을 요청했다. (아직 미획득 상태)

<br>

자원 할당 그래프에서 Cycle이 없으면 데드락이 아니다. 만약 사이클이 있다면 다음의 조건을 충족하면 데드락이다.

- 만약 자원당 instance(점)이 하나밖에 없으면 데드락이다. 
- 자원당 인스턴스가 여러개일 경우에는 데드락일 수도 있고 아닐수도 있다. 

<br>

## 2. Deadlock의 처리 방법

- Deadlock Prevention
  - 자원 할당 시 Deadlock의 4가지 필요 조건 중 어느 하나가 만족되지 않도록 하는 것
- Deadlock Avoidance
  - 자원 요청에 대한 부가적인 정보를 이용해서 deadlock의 가능성이 없는 경우에만 자원을 할당한다.
  - 시스템 state가 원래 state로 돌아올 수 있는 경우에만 자원을 할당한다. 
- Deadlock Detection and Recovery
  - Deadlock 발생은 허용하되 그에 대한 detection 루틴을 두어 deadlock 발견 시 recover한다.
- Deadlock Ignoracne
  - Deadlock을 시스템이 책임지지 않는다.
  - Deadlock을 방지하는 것이 오버헤드가 너무 많이 들기 때문에 유저가 해결하도록 한다. 
  - UNIX를 포함한 대부분의 OS가 채택한다. 

<br>

### 1) Deadlock Prevention (미연에 방지한다)

>  Deadlock 필요조건 중 하나를 충족시키지 않게 하여 미연에 방지한다.
{: .prompt-tip }

- Mutual Exclusion
  - 공유해서는 안되는 자원의 경우 반드시 성립해야 한다. 
- Hold and Wait 
  - 프로세스가 자원을 요청할 때 다른 어떤 자원도 가지고 있지 않아야 한다. 
  - `방법1` 프로세스 시작 시 모든 필요한 자원을 할당 받게 하는 방법이다. 
  - `방법2` 자원이 필요할 경우 보유 자원을 모두 놓고 다시 요청한다. (자진해서 반납을 함으로 해결한다.)
- No Preemption
  - Process가 어떤 자원을 기다려야 하는 경우 이미 보유한 자원이 선점된다.
  - 모든 필요한 자원을 얻을 수 있을 때 그 프로세스는 다시 시작된다.
  - State를 쉽게 save하고 restore할 수 있는 자원에서 주로 사용한다. (CPU, memory)
- Circular Wait
  - 모든 자원 유형에 할당 순서를 정하여 정해진 순서대로만 자원을 할당한다. 
  - 예를 들어 순서가 3인 자원 R3을 보유 중인 프로세스가 순서인 1인 자원 R1를 할당받기 위해서는 우선 R3를 Release해야 한다. 

원천적으로 Deadlock을 막을 수 있지만, 굉장히 비효율적이다. (자원에 제약을 두기 때문이다)

<br>

### 2) Deadlock Avoidance

- 프로세스가 시작될 때, 평생에 쓸 자원을 알고 있다고 가정한다.
  -  Deadlock이 될 수있는 자원 할당 요구를 허락하지 않음으로 Deadlock을 피한다. 
  - 자원당 인스턴스가 1개일 때는 자원 할당 그래프를 활용한다. 
  - 자원당 인스턴스가 여러개일 때는 Banker's Algorithm을 활용한다. 

- 자원 요청에 대한 부가 정보를 이용해서 자원 할당이 deadlock으로부터 안전(safe)한지를 동적으로 조사해서 안전한 경우에만 할당한다.
- 가장 단순하고 일반적인 모델은 프로세스들이 필요로 하는 각 자원별 최대 사용량을 미리 선언하도록 하는 방법이다. 

<br>

✅ **Deadlock Avoidance는 safe한 상태를 유지하는 것이다**

`safe state`는 시스템 내의 프로세스에 대한 safe sequence가 존재하는 상태이다. 

safe sequence란

- 프로세스의 sequence가 safe하려면 프로세스의 자원 요청이 `가용자원 + 모든 프로세스의 보유자원`에 의해 충족되어야 한다. 
- 조건을 만족하면 다음 방법으로 모든 프로세스의 수행을 보장한다.
  - Pi의 자원 요청이 즉시 충족될 수 없으면 모든 Pj  (j < i )가 종료될 때까지 기다린다.
  - Pi-1이 종료되면 Pi의 자원요청을 만족시켜 수행한다. 

<br>

### 3) Deadlock Detection and recovery

> Deadlock은 빈번히 발생하는 이벤트가 아니기 때문에, 미연에 방지 하기 위해 처리를 하기보다 발생하면 처리를 한다. 
{: .prompt-tip }

<br>

![image-20220920182313407](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220920182313407.png)

우측을 Wait for graph라고 부른다. Wait for graph는 사이클을 빠르게 찾을 수 있기 때문에, Deadlock이 발생하는 지를 빠르게 확인할 수 있다. 

자원 당 인스턴스가 여러 개있을 때는 위와 같은 발견으로 deadlock을 찾기 어렵고 다른 방법으로 찾는다.

여튼 deadlock을 발견하면 이를 recovery해야 한다. 

- ProcessTermination
  - 프로세스를 종료시킨다. 
- Resouces Preemption
  - 프로세스에서 자원을 뺏는다. 
  - 비용을 최소화할 victim을 선정한하고 safe state로 rollback하여 process를 restart한다. 
  - Starvation문제가 발생할 수 있다..
    - 동일한 프로세스가 계속해서 victim으로 선정되는 경우
    - cost factor에 rollback 횟수도 같이 고려한다.

<br>

### 4) Deadlock Ignoracne

> Deadlock이 일어나지 않는다고 생각하고 아무런 조치도 취하지 않는다. 
{: .prompt-tip }

- Deadlock이 매우 드물게 발생하므로 deadlock에 대한 조치 자체가 더큰 오버헤드일 수도 있다.
- 만약, 시스템 deadlock이 발생한 경우 시스템이 비정상적으로 작동하는 것을 사람이 느낀 후 직접 프로세스를 죽이는 등의 방법으로 대처한다.
- UNIX, WIndows등 대부분의 범용 OS가 채택하고 있다. 

<br>