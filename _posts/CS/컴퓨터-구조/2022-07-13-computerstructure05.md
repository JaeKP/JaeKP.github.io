---
title: 5장-기본-컴퓨터의-구조와-설계-part1
author: 박재경
date: 2022-07-13
categories: [CS, 컴퓨터-구조]
tag: [cs, 컴퓨터-구조]
---



## 기본컴퓨터의 구조와 설계 part1-1

[강의 링크](https://www.youtube.com/watch?v=vSnpYzCuwVY&list=PLc8fQ-m7b1hCHTT7VH2oo0Ng7Et096dYc&index=10)

### 1) 기본 컴퓨터 (Basic Computer)

- DEC. Corp 사의 중형 컴퓨터 PDP-11을 지칭한다.
  - 이후 VAX-11 등의 주요 minicomputer의 기본이다. (1970 ~ 1980년대)
- 컴퓨터 구조 설계의 가장 기본적인 부분이다.
- 현대의 CPU들에도 동일하게 적용되는 설계 구조이다. 
- 튜링머신: 프로그램을 기억장치에 저장하고 프로그램을 명령에따라 하나씩 꺼내와서 실행시킨다. 

<br>

### 2) 명령어 코드 (Instruction Codes)

#### (1) 컴퓨터의 동작

- 레지스터 내에 저장된 데이터에 대한 마이크로 연산의 시퀀스에 의하여 정의된다.
- 범용 컴퓨터 시스템에서는 다양한 마이크로 연산 시퀀스를 정의한다.

<br>

#### (2) 명령어 코드

- 컴퓨터에게 어떤 특별한 동작을 수행할 것을 알리는 비트들의 집합이다.
- 연산 코드들로 구성되어 있다.

<br>

**`저장(내장) 프로그램 구조`**

![image-20220706190648268](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220706190648268.png)

<br>

- 명령어의 집합으로 구성된다.
- 각 명령어는 명령어 포맷(컴퓨터를 설계할 때 , 미리 정해진)에 따라서 정의된다.
  - I: Address가 직접 주소, 간접 주소인지 구별한다. 
  - Opcode: 기계 명령 
  - Adress: 명령어가 사용하는 데이터(Operands)의 주소 값을 저장
- 프로그램 실행부분에 따라서 메모리의 다른 부분에 저장된다.
- 명령어 실행 결과는 AC에 저장된다. 

<br>

**💬간접 주소와 직접 주소**

- I비트가 0일 경우 직접 주소, 해당 주소에 데이터가 존재한다. 

- I비트가 1일 경우 간접 주소, 해당 주소에 데이터가 위치하는 주소가 존재한다. 

<br>

#### (3) 컴퓨터 명령어

- 컴퓨터에 대한 일련의 마이크로 연산을 기술한다. (기계어 프로그램)
- 이진 코드로 구성된다.
- 처리할 데이터와 함께 메모리(메인 메모리)에 저장된다.

<br>

#### (4) 프로그램

- 사용자가 원하는 연산과 피연산자가 처리되는 순서를 기술한 컴퓨터 명령어의 집합이다.
- 명령어 처리 과정을 제어한다. 

<br>

#### (5) 내장 프로그램

- 제어 신호에 의하여 명령어의 이진 코드를 해석하여 실행한다.
- 명령어를 저장하여 실행하는 컴퓨터 구동방식

<br>

### 2) 컴퓨터 레지스터 (Computer Registers)

#### (1) 기본 컴퓨터의 레지스터

![computer-registers](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/computer-registers.png)

<br>

#### (2) 버스 시스템의 종류

- 내부 버스
  - CPU(컴퓨터) 내부 레지스터간 연결한다.
  - 공통 버스 시스템
- 외부버스
  - CPU 내부 레지스터와 메모리간 연결한다.
- 입출력 버스
  - CPU와 주변장치(I/O)를 연결한다.

<br>

#### (3) 공통 버스 시스템

- 내부 버스를 통칭한다.
- 내부 버스의 크기(width)로 CPU 워드 크기를 결정한다.
  - 16 bit 컴퓨터 : 내부 버스 / 레지스터 크기가 16 bit
  - 32 bit 컴퓨터: 내부 버스 / 레지스터 크기가 32bit
- 전송 연결 통로
  - 레지스터- 레지스터 데이터 전송을 연결하는 통로이다.
  - 레지스터 - 메모리 데이터 전송을 연결하는 통로이다. (예외적 표현)
  - 한 순간에는 하나의 전송 신호만이 버스에 존재가 가능하다.
    - 2개 이상의 신호 발생 시에는 버스 충돌이 발생한다.
    - 버스 제어기(정확한 타이밍과 MUX 제어 수행)

<br>

**`버스의 동작`**

![image-20220706193242548](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220706193242548.png)

- 레지스터 출력은 버스의 MUX 입력에 연결되어있다. 

- 각 레지스터에 MUX 입력 번호가 설정된다. 
- 레지스터 입력은 버스에 직접 연결한다. (LD로 제어)
- 병렬로드가 가능한 양방향 시프트 레지스터이기 때문에 가능하다. 
  - 레지스터의 LD가 1이면 데이터를 받는다. 
  - 메인 메모리의 경우 write면 데이터를 받는다. 

<br>

## 기본컴퓨터의 구조와 설계 part1-2

[강의 링크](https://www.youtube.com/watch?v=T2oKxvinK84&list=PLc8fQ-m7b1hCHTT7VH2oo0Ng7Et096dYc&index=11)

### 1) 컴퓨터 명령어 (Computer Instructions)

#### (1) 종류

- MRI 명령어(Memory-reference instruction) : 7가지
  - 메모리를 참조하는 명령어
  - 주소비트를 가지고 메인메모리에 접근하는 명령어를 의미한다. 

- RRI 명령어(Register -reference instruction): 12가지
  - 레지스터를 참조하는 명령어
  - 레지스터 사이의 혹은 레지스터를 다루는 명령으로 구성되어있다.

- IO 명령어(Input-output instruction):6가지
  - Io 장치 명령어
  - 입력과 출력과 관련된 명령어


<br>

#### (2) 명령어 형식

Opcode에 따라 명령어 종류를 구분할 수 있다. 

 ![image-20220710203831467](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220710203831467.png)

<br>

### 2) 타이밍과 제어 (Timing and Control)

기본 컴퓨터의 모든 플립플롭과 레지스터는 주 클럭 발생기에 의하여 제어된다.

![image-20220710220125593](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220710220125593.png)

<br>

- 메모리에서 읽어온 명령어는 공통 버스 시스템에 있는 명령어 레지스터 (IR)에 놓이게 된다.
- 이 중에서 연산 코드 부분이 3X8 디코더에 의해 D0에서 D7까지 디코딩된다. 
- 명령어 레지스터의 15번째 비트는 I로 표시되는 플립플롭에 전송되며, 나머지 0에서 11번째 비트들은 제어 논리 게이트로 연결된다.
  4비트 순차 카운터(SC)의 출력은 디코더에 의해 T0에서 T15까지 16개의 타이밍 신호를 생성한다.

<br>

### 3) 명령어 사이클 (Instruction Cycle)

![image-20220710221306576](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220710221306576.png)

<br>

#### (1) 명령어 사이클 단계

- 메모리에서 명령어 가져오기 (Fetch)
- 명령어 디코딩
- 유효주소 (Effective Address) 계산
- 명령어 실행

<br>

#### (2) Fetch

메모리의 CodeSegment에 있는 기계어 코드 명령어를 IR(명령어 레지스터)로 가져온다. 

- CodeSegment 몇 번지에 있는 명령어를 가져올지 파악한다.  `AR <- PC`
  - 명령어 번지는 PC(Program Counter: 지금 가져와야할 명령어의 주소를 기억하는 레지스터)에 저장되어 있다.
  - 버스를 통해 PC에 있는 주소를 AR레지스터로 가져온다.  
  - AR레지스터에 있는 값이 들어오면 그 순간에 메모리의 해당 번지의 값이 자동으로 읽히게 된다. 

- 메모리로부터 IR레지스터로 버스를 통해 이동한다. `IR <- M[AR]`
  - IR 레지스터의 LD입력을 1로 하여 버스의 신호를 IR 레지스터로 이동시킨다. 
- 명령어가 IR로 들어왔기 때문에 그다음에 가져와야할 명령어를 다시 가르킨다. (일반적으도 다음번지인다.)
  - PC <- PC+1

<br>

#### (3) 명령어 디코딩

- IR에 들어간 명령어를 디코딩한다. 3bit Opcode가 무엇인지 판단한다. 
  - 그 결과, 디코더의 어느 출력을 1로할 것 인지 결정한다. 
- 그리고 IR에 있는 다른 주소비트들을 다시 AR로 보낸다. (데이터를 가져오기 위해서)
  - AR <- IR(0-11)

<br>

#### (4) 유효주소 계산

- 3bit Opcode를 통해 어떤 명령어 인지 파악한 후, 명령어를 실행준비한다.  
  - MRI 명령어 여부 (D7이 0이다)
  - RRI 명령어 여부 (D7이 1이다)
  - IO 명령 결정 (D7이 1이다)

- MRI일 경우, 직접 주소인지 간접주소인지 파악하여 실제 명령어의 주소를 가져온다. 
  - Operand가 있는 주소를 알아내는 것이다.

<br>

#### (5) 명령어 실행

- 명령어를 실행한다. 
- 명령어 실행이 끝나면, 다음 명령어를 다시 Fetch한다.

<br>

### 4) 메모리 참조 명령어 (Memory-Reference Instuctions)

![image-20220710223030046](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220710223030046.png)

<br>

![image-20220710223019916](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220710223019916.png)

