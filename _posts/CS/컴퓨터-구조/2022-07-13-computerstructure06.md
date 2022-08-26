---
title: 컴퓨터구조-기존-컴퓨터의-구조와-설계-part2
author: 박재경
date: 2022-07-13
categories: [CS, 컴퓨터-구조]
tag: [cs, 컴퓨터-구조]
---



## 기본 컴퓨터의 구조와 설계 part2

[강의 링크](https://www.youtube.com/watch?v=eoswnrO_v9g&list=PLc8fQ-m7b1hCHTT7VH2oo0Ng7Et096dYc&index=12)

### 1) 입출력과 인터럽트 (Input-Output and Interrupt)

입출력장치들과 CPU가 서로 통신을 하면서 데이터를 주고 받으며 데이터를 입력하거나 출력합니다.

입출력장치들은 CPU보다 굉장히 느리다. 그래서 CPU는 현재 입출력 장치들이 사용이 가능한지 체크해야한다. 

#### (1) 입출력 구성

![image-20220712223630434](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220712223630434.png)

1. 키보트에서 `g`를 클릭 
2. FGI가 1로 세팅이 된다. (받을 데이터가 있다.) 
3. CPU는 Input Register(INPR)를 통해 데이터를 인터페이스에서 받게된다. 
4. 그 값을 AC로 전송하게 된다. 

<br>

- CPU와 IO 장치의 속도 차이 제어를 위하여 Flag 사용 
  - 현재 입출력 장치가 사용이 가능한지 확인한다. 
  - Buffer overrun 상태
  - BUffer underrun 상태

<br>

`종류`

- FGI
  - 1: 입력 가능한 상태 
  - 0: 입력 블럭킹
- FGO
  - 1: 출력 가능한 상태
  - 0: 출력장치 사용 중
- 인터럽트 (Interrupt)
  - IEN flag에 의하여 제어된다.
    - IEN이 1이면 지금 입출력을 할 수 있는 상태이다. 
  - 입출력 장치에 의해 발생하며 입출력 전체를 제어한다. 

<br>

#### (2) 인터럽트 사이클

![image-20220712232754695](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220712232754695.png)

<br>

- 프로그램 인터럽트
  - 장치가 중비되었을 때 CPU에게 알린다.
  - 인터럽트 발생시 BSA 명령어처럼 동작한다.
  - FGI, FGO 플로그를 사용한다. 
    - 플래그가 set되면 R <- 1
    - R = 1이면 다음 명령어 사이클에 인터럽트 사이클이 실행된다.
  - IEN
    - 인터럽트 enable/ disable 제어한다.
- I/O Program
  - BIOS
  - 입출력 인터럽트 처리 루틴의 집합이다.
- IVT (Interrupt Vector Table)
  - 각 인터럽트에 벡터 번호를 부여한다.
  - 벡터 번호와 인터럽트 처리 루틴 시작번지를 Table로 유지한다.
  - 시스템 부팅시, IVT는 0번 segment에 load
  - 현대의 대부분의 CPU가 IVT를 사용한다. 

<br>

#### (3)  인터럽트 flow

![image-20220712232858057](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220712232858057.png)



- 기본 컴퓨터: 한개의 하나의 인터럽트만 받는다. 동시에 여러개의 인터럽트를 안받는다.
- 그러나 현대의 cpu들은 인터럽트가 수행되는 도중에도 인터럽트를 받을 수 있다. (queue)

1. AR <- 0, TR <- TC
2. M[AR] <- TR, PC <- 0
3. PC <- PC + 1, IEN <- 0, R<-0, SC<-0

<br>

## 기본 컴퓨터의 구조와 설계 part2-2

[강의 링크](https://www.youtube.com/watch?v=zQuOYWLbCI4&list=PLc8fQ-m7b1hCHTT7VH2oo0Ng7Et096dYc&index=13)

### 1) 컴퓨터에 대한 완전한 기술 (Complete Computer Description)

![image-20220713180833128](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220713180833128.png)

<br>

### 2) 기본 컴퓨터의 설계 (Design of Basic Computer)

![image-20220713181756160](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220713181756160.png)

<br>

#### (1) 하드웨어 구성요소

- 16bit 4096워드 메모리
- 9개의 레지스터
  -  AR, PC, DR, AC, IR, TR, OUTR, INPR, SC
- 7개의 플립플롭
  -  I, S, E, R, IEN, FGI, FGO
- 2개의 디코더
  -  3x8(Opcode), 4x16(타이밍)
- 16bit 공통버스
- 제어 논리 게이트
- AC 입력 연결 논리회로 (ALU)

<br>

#### (2) 컴퓨터 동작 흐름

- MRI, RRI, IO 명령 사이클 구현
- 인터럽트 사이클 구현

<br>

#### (3) 레지스터와 메모리에 대한 제어

![image-20220713184439674](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220713184439674.png)

- 설계 순서
  - AR에 대한 LD, CLR, INC 동작의 경우 수집
  - 각 동작들을 OR로 연결한다. 

<br>

### 3) 누산기 논리의 설계 (Design of Accumulator Logic)

#### (1) AC 레지스터관련 회로

![image-20220713190148493](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220713190148493.png)

- AC를 변경하는 경우 수집한다.
- LD, CLR, INC

<br>

#### (2) 레지스터에 대한 제어

![image-20220713190301374](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220713190301374.png)

- LD 신호 제어
  - MRI 명령: AND, ADD, LDA
  - RRI 명령: COM, SHR, SHL
  - IO 명령: INPR
- INR 신호 제어
  - MRI 명령: none
  - RRI 명령: INC
  - IO 명령: none
- CLR 신호 제어
  - MRI 명령: none
  - RRI 명령: CLR
  - IO 명령: none

<br>

#### (3) 가산 논리 회로 

![image-20220713190626826](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220713190626826.png)

- 회로 요소
  - AND gate
  - FUll Adder
  - Inverter
  - Shifter
  - INPR/OUTR

<br>