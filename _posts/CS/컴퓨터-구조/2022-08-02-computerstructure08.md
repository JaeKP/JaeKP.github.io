---
title: 컴퓨터구조-입출력-구조
author: 박재경
date: 2022-08-02
categories: [CS, 컴퓨터-구조]
tag: [cs, 컴퓨터-구조]
---



## 입 출력 구조 파트1

[강의 링크](https://www.youtube.com/watch?v=jbx2HolVQqk&list=PLc8fQ-m7b1hCHTT7VH2oo0Ng7Et096dYc&index=27)

### 1) 주변장치 (Peripheral Devices)

#### (1) 종류

- 입력장치
  - 키보드, 마우스, 디지타어지, 마이크 ,카메라, 3축셍서 등
- 출력 장치
  - 모니터, 프린터, 스피커, 서보 장치 등
- 입 출력 장치
  - 저장 장치, TV-인코더, LC 터치 스크린 등

<br>

#### (2) 인터 페이스

> 주변 장치를 CPU에 연결하는 방법 (어떠한 device들이 CPU와 어떻게 연결하는가)
{: .prompt-tip }

<br>

- 통신을 하는 데이터가 어떻게 표현되는가
  - ASCII (American Standard Code For Information Interchange) 문자를 사용한다.  
  - Alphanumeric , 제어 문자 

- 모든 device는 인터페이스를 가지고 있다. 

<br>

### 2) 입출력 인터페이스 (Input-Output Interface)

> CPU는 입출력 버스의 정보 흐름을 관리하는 I/O 제어를 갖고 있으며, 메모리로부터 명령어를 받아 인터페이스를 통하여 주변 장치와 통신한다. 
>
> 인터페이스는 명령을 해석하고 주변 장치 제어기에 신호를 보낸다.
{: .prompt-tip }

<br>

#### (1) 입출력 인터페이스의 기능

- **CPU와 I/O 장치의 차이점을 해결한다.**
  - I/O 장치는 CPU에 비해 굉장히 느리다.
- **동작 방식 차이에 따른 데이터 신호값을 변환한다.**
  - 키보드, printer, disc가 사용하는 신호 방식과 cpu 사용하는 신호 방식이 다르다.
  - 신호 방식 
    -  TTL: 0이면 0볼트, 1이면 5볼트
    - RS-232-C: 0이면 5볼트,1이면 -3볼트
  - 서로 다른 신호 방식의 차이를 위함
- **전송 속도 동기화**
- **데이터 코드 형식과 메모리 워드 형식 간 변환**
  - ASCII코드만 사용하는 것이 아니다!
- **주변 장치들 간의 동작 숭서와 순위 중재 및 조정한다.** 
  - I/O 장치들이 동시에 CPU를 사용하려고 할때 중재

<br>

#### (2) I/O 버스와 인터페이스 모듈

> 입출력 버스를 통해 CPU와 인터페이스를 연결한다.
{: .prompt-tip }

<br>

- 인터페이스가 실행하는 I/O 커맨드

| 입출력 커맨드      | 설명                            |
| ------------------ | ------------------------------- |
| 제어 커맨드        | 주변 장치를 활성화, 동작을 정의 |
| 테스트 커맨드      | 상태를 확인                     |
| 데이터 출력 커맨드 | 데이터의 출력을 전송            |
| 데이터 입력 커맨드 | 데이텨의 입력을 전송            |

<br>

#### (3) I/O 대 메모리 버스 

> CPU는 I/O 뿐만 아니라 메모리 장치와도 통신한다. 
>
> 컴퓨터의 버스가 I/O와 메모리와 통신하는 세 가지 방법은 다음과 같다.
{: .prompt-tip }

<br>

- 메모리 와 I/O는 각자 다른 버스를 사용한다.
  - CPU와 IOP를 별도로 가지는 시스템에 사용된다.
- 메모리와 I/O가 공통의 버스, 개별 제어 라인을 사용한다.
- 메모리와 I/O가 공통의 버스와 제어라인을 사용한다. 

<br>

## 입출력 구조 파트2

[강의 링크](https://www.youtube.com/watch?v=9faaqyzw28I&list=PLc8fQ-m7b1hCHTT7VH2oo0Ng7Et096dYc&index=28)

### 1) 비동기 데이터 전송 (Asynchronous Transfer)

> 인터페이스를 통해 데이터를 전송하고 받을 때, 통신하는 규격이 중요하다. 
>
> 신호를 전송하는 규격과 규칙이 있다. 
{: .prompt-tip }

<br>

#### (1) 스트로브 제어

<img src="https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220802181515237.png" alt="image-20220802181515237" style="zoom:67%;" />

<br>

- 전송의 시간을 맞추기 위해 단 하나의 제어 라인을 사용한다.
- 송신 장치과 수신 장치 사이에는 데이터를 운반하는 데이터 버스와 유용한 데이터가 버스에 있음을 알리는 스트로브가 존재한다.

- Source에서의 데이터 전송을 원한다면, (Source initiate)

  - Source에서 데이터를 데이터 버스에 올리고 스트로브 신호를 발생킨다.

  - Destination이 데이터를 받아들일 만큼 충분한 시간 동안 데이터를 활성화된 상태로 만든다.
  - Destination은 Source가 데이터를 보내고 있다는 것을 인지하게 되어 데이터(한 비트)를 수신 한다. 
    - 스트로브 신호를 통해 이게 나에게 보내는 데이터인지 확인할 수 있다. 
  - 스트로브 신호가 꺼지면 Destination이 데이터를 보내는 시간이 끝난 것을 알게 된다. 

- Destination에서의 데이터 수신을 원한다면, (Destination initiate)

  - Destination에서 스트로브 신호를 발생시킨다. 

  - Source가 데이터 버스에 데이터(한 비트)를 띄운다.
    - 스트로브 신호를 통해 데이터를 받을 준비가 된 것을 확인 할 수 있다. 
  - Destination은 Source가 보낸 데이터를 받고 스트로브 신호를 끈다.
  - 스트로브 신호가 꺼지면 Source는 Destination이 데이터를 받았음을 알게 된다. 

<br>

:white_check_mark:**문제점**

- Source initiate의 경우 Destination이 데이터를 받았음을 알 수 없다. 
  - 스트로크 신호가 끊기거나 오류가 생기면,
  - Destination은 Source가 데이터를 보내고 있음을 모르게 되어 데이터를 수신하지 않는다. 
  - Source는 일정 시간이 지나면 자동으로 데이터를 그만 보내기 때문에 Destination이 데이터를 안받았는지 모른다. 
- Destinaion initiate의 경우 Source가 Destination이 데이터를 수신할 준비가 된 것을 모를 수 있다.
  - 스트로크 신호가 끊기거나 오류가 생기면,
  - Source는 Destination이 스트로크 신호를 보내고 있음을 모른다.
  - Source가 데이터를 송신하지 않아,  Destination은 Source가 보낼 데이터가 없다고 인지하게 된다. 

상호 간에 데이터를 받았는지 확인하는 절차가 필요하다.

<br>

#### (2) 핸드쉐이킹 (HandShaking) 제어

<img src="https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220802182851605.png" alt="image-20220802182851605" style="zoom:67%;" />

<br>

- Source에 보내는 제어 신호를 추가하여 스트로브 제어의 문제점을 해결할 수 있다.
- 서로 악수하듯이 확인해서 핸드쉐이킹이라고 부른다.

- Source에서의 데이터 전송을 원한다면, (Source initiate)

  - Source에서 데이터를 버스에 올리고 valid 신호를 보낸다.

  - Destination이 버스에서 데이터를 받은 후 accepted 신호를 보낸다.
  - 서로 신호를 끄면서, 데이터가 잘 전송 됨을 알 수 있다. 
  - 만약 accepted 신호를 일정 시간동안 안 보내면, time out 에러가 발생한다. 

- Destination에서의 데이터 수신을 원한다면, (Destination initiate)

  - Destination에서 ready for data 신호를 보낸다.

  - Source에서 데이터 버스에 데이터를 올린 뒤, 데이터와 valid 신호를 보낸다.
  - 서로 신호를 끄면서, 데이터가 잘 전송 됨을 알 수 있다. 
  - 만약 valid신호를 일정 시간동안 안 보내면, 

<br>

#### (3) 비동기 직렬 전송

![image-20220802184505290](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220802184505290.png)

- **TTL 전송**
  - 특별하지 않은 이상, 컴퓨터의 디바이스간의 전송은 TTL전송을 사용한다. 
  - 0V, 5V 신호로 데이터를 전송한다.
  - 시작 비트1, 데이터 비트 8, 정지 비트 2. 총 11비트 단위로 8비트 데이터를 전송한다. 
    - 한 비트당 핸드 쉐이킹으로 보낸다....
  - 데이터 신호(비트)의 간격 = 1/  전송속도
    - 데이터의 신호간격이 짧아질 수록 전송속도가 빨라진다.
    - 그래서 신호의 간격이 중요하다. 
- **전송 규칙**
  - 데이터가 전송되지 않을 때에는 항상 1신호(5v)를 유지한다.
  - 문자 전송의 시작은 시작 비트 0으로 표시한다.
  - 시작 비트 뒤로 8개의 데이터 비트를 표시한다.
  - 마지막 비트 후, 2비트 이상의 1신호를 유지한다.

<br>

#### (4) 비동기 통신 인터페이스

> I/O 장치의 비동기 통신을 위한 인터페이스로 UART가 있다.
{: .prompt-tip }

<br>

- UART
  - 일반적인 인터페이스의 형태이다. 
  - shife레지스터에 의해 한비트씩 8비트 전송을 하는 장치이다.

<br>

### 3) 전송 모드 (Modes of Transfer)

> 컴퓨터와 I/O장치 사이의 데이터 전송은 여러 가지 모드로 나뉘어진다.
> CPU의 도움을 통해 메모리로 전송하거나 I/O가 CPU의 도움 없이 데이터를 직접 메모리로 전송하는 방법이 있다.
{: .prompt-tip }

<br>

#### (1) 입출력 전송 모드의 종류

- 프로그램된 I/O (Programmed I/O)
  - 입출력 명령에 의하여 동작한다.
  - 프로세서 레지스터와 주변 장치간 데이터 전송을 수행한다.
  - 레지스터와 메모리간 데이터 전송을 수행한다.
  - 주변 장치의 플래그에 기반한 입출력을 수행한다.
  - 즉, CPU가 주관하여 I/O에서 입출력이 발생하는지 계속 확인한다. 
- 인터럽트에 의한 I/O (Interruped I/O)
  - 인터럽트에 의하여 입출력을 수행한다.
  - 입출력이 준비되면 프로세서 인터럽트 요구를 수행한다.
  - 인터럽트 처리 후 본래 프로그램을 계속 수행한다.
  - 즉, CPU는 가만히 있고 I/O가 인터럽트를 통해 `나 지금 데이터 전송한다`라고 알려준다. 그때 프로그램된 I/O방식으로 데이터를 수행한다. 
- 직접 메모리 접근 (DMA: Direct Memory Access)
  - 데이터를 메모리 버스를 통하여 전송한다.
  - DMA를 수행하는 전용 하드웨어 (DMA Controller)를 사용한다.
  - 사이클 스틸링에 의한 버스를 효율화한다.
  - 즉, CPU가 관여하지 않는다. 일종의 작은 형태의 iop라고 할 수 있다. 

<br>

#### (2) 프로그램된 I/O

![image-20220802185918143](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220802185918143.png)

- I/O장치는 핸드셰이킹 방식으로 데이터를 전송한다.

<br>

## 제 11 장. 입출력 구조 part-3

[강의 링크](https://www.youtube.com/watch?v=ufXNH7RsAro&list=PLc8fQ-m7b1hCHTT7VH2oo0Ng7Et096dYc&index=29)

### 1) 우선순위 인터럽트 (Priority Interrupt)

#### (1) 인터럽트 우선 순위

- 동시에 발생된 인터럽트의 우선 순위를 결정한다.
- 하드웨어 또는 소프트웨어 적으로 결정한다.

<br>

#### (2) 데이지 체인 (Daisy Chain) 우선 순위 인터럽트

- 우선 순위에 따라서 인터럽트 처리 프로그램의 벡터 주소(VAD)를 CPU로 전달한다. 

<img src="https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220802190152402.png" alt="image-20220802190152402" style="zoom:67%;" />

<br>

- 인터럽트 발생하여 인터럽트를 처리할 수 있으면 CPU가 interrupt acknowledge를 보낸다.
- 만약 device1, device3에서 인터럽트가 발생하면, 먼저 device1에 interrupt acknowledge를 보낸다.
- device1의 인터럽트 서비스 루틴을 실행한다. 
  - VAD(Vector address)1 -> 인터럽트 서비스 루틴 시작 위치)
- device1의 interrupt acknowledge를 device2를 보낸다. 
- device2는 인터럽트를 발생시키지 않은 것이기 때문에 device3에게 그대로 다시 보낸다.
- device3는 인터럽트가 발생했기 때문에, VAD3를 CPU에 보내 서비스 루틴을 실행한다.

그 결과, 설계자는 중요한 device를 가장 먼저 interrupt를 실행하도록 설계해야 한다. 

<br>

#### (3) 병렬 우선순위 인터럽트(Parallel Priority Interrupt)

- 우선 순위 인코더를 사용한다.
- 우선 순위에 따른 VAD를 생성한다. 
  - 설계자는 인터럽트 서비스 루틴이 시작하는 VAD를 잘 설계해야 한다. 
- CPU에 인터럽트를 전달한다.
- Mask 레지스터를 사용한다. 
  - 인터럽트 요청의 상태를 조절할 수 있다. 

<br>

#### (4) 인터럽트 동작

- 초기 동작
  - 보다 낮은 단계의 mask 레지스터 비트를 0으로 set한다. (내 아래에 있는 애들의 인터럽트는 무시한다!)
  - 인터럽트 상태 비트 IST <- 0  ( 더 높은 단계의 인터럽트가 올 수 있도록 한다. )
  - 프로세서에 레지스터 비용(PSW)를 저장한다.
  - 인터럽트 인에이블 비트 IEN <- 1 (더 높은 단계의 인터럽트가 올 수 있도록 한다. )
  - 인터럽트 서비스 프로그램을 계속 실행한다.
- 최종 동작
  - 인터럽트 인에이블 비트 IEN <- 0  (이제는 완전히 다른 인터럽트를 막는다. )
  - PSW의 내용을 CPU에 복귀한다.
  - 인터럽트에 관계된 레지스터 비트를 클리어 한다.
  - 보다 낮은 순위의 mask비트를 1로 set한다.
  - 복귀 주소를 PC에 저장한다
  - 인터럽트 인에이블 비트 IEN <- 1 

<br>

### 2) 직접 메모리 접근 (Direct Memory Access)

> I/O장치가 CPU를 거치지 않고 메모리에 접근한다. 
{: .prompt-tip }

<br>

#### (1) DMA 전송 구조

- I/O 입출력을 처리하기 위해서 CPU를 방해하지 않는다.
- CPU가 사용하는 버스를 사용할 수 없기 때문에 메모리와 I/O가 각자 다른 버스를 사용하지 않는다면, DMA를 사용하기 어렵다.
- 그래서 사이크 스틸링을 사용한다.
  - CPU의 명령어 사이클에서 메모리에 접근 안하는 타이밍(버스를 사용 안하는) 타이밍에  CPU대신에 메모리 버스를 사용한다. 

<br>

#### (2) DMA 동작 순서

![image-20220802194326044](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220802194326044.png)

<br>

- DMA 전송
  - 전송 초기화를 실행 한다.
  - 메모리 버스와 입출력 버스간 연결한다.
  - 메모리 버스로 직접 데이터를 전송한다.
  - 완료 후, 전송 종료 인터럽트를 전송한다.
- DMA 전송 초기화
  - 데이터 전송 메모리 주소를 전송한다. (Address register)
  - 전송할 데이터 워드 수를 전송한다. (Word count register)
  - 읽기 / 쓰기 신호를 결정한다. (R/W)
  - DMA 전송 시작 신호 전송

<br>

### 3) 입출력 프로세서 (Input-Output Processor)

> DMA의 확장판
{: .prompt-tip }

<br>

#### (1) 정의 

<img src="https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220802194541529.png" alt="image-20220802194541529" style="zoom:80%;" />

- 입출력 장치와의 직접적인 통신을 전담한다.
- 채널(Channel)로 호칭한다.
- CPU급의 DMA 제어기이다.
- DMA제어기와 달리 IOP는 고유의 명령어를 실행시킬 수 있다.

<br>

### 4) 직렬 통신(Serial Communication)

#### (1) 문자 지향 프로토콜 (Character-Oriented Protocol)

![image-20220802194808455](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220802194808455.png)

| SYN  | SYN  |     Start Of Header     | STX  | Text | ETX  |           BCC           |
| :--: | :--: | :---------------------: | :--: | :--: | :--: | :---------------------: |
|      |      | (text에 대한 에러 정보) |      |      |      | (text에 대한 에러 정보) |

- 문자를 기반하는 text를 보낼 때는 위와 같은 형식으로 보내야 한다. 
- 데이터 전송을 위한 약속이다.

<br>
#### (2) 비트지향 프로토콜 (Bit-Oriented Protocol)

![image-20220802195229762](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220802195229762.png)

- 비 문자 데이터 전송용 format이다.
- Contorol field에 따라서 여러 데이터 형태를 결정한다. 

<br>