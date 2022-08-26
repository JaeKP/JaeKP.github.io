---
title: 컴퓨터구조-레지스터-전송과-마이크로-연산
author: 박재경
date: 2022-07-06
categories: [CS, 컴퓨터-구조]
tag: [cs, 컴퓨터-구조]
---



## 레지스터 전송과 마이크로 연산 파트1

[강의 링크](https://www.youtube.com/watch?v=LDjco5XJH1E&list=PLc8fQ-m7b1hCHTT7VH2oo0Ng7Et096dYc&index=8)

### 1) 레지스터 전송 언어 (Register Transfer Language)

#### (1) 마이크로 연산 (Micro-operation)

- 레지스터에 저장된 데이터를 가지고 실행되는 기본동작을 의미한다.
- 하나의 clock 시간 동안 실행되는 기본 동작이다. (플립플롭회로로 구성되어 있기 때문에 clock의 개념이 나온다.)
  - shift
  - count
  - clear
  - load
  - 등

<br>

#### (2) 레지스터 전송 언어

- 마이크로 연산, 전송을 간단하고 명료하게 표시하기 위하여 사용하는 기호이다.
- 디지털 컴퓨터의 내부 조직을 상세하게 나타내는 수단으로 사용된다.
- 디지털 시스템의 설계 편의성을 제공한다. 

<br>

#### (3) 레지스터 전송 언어 규칙

- 대문자로 표시한다. (MAR, MBR, AC, PC, DR 등)
- 레지스터는 여러개의 플립플롭으로 구성되어있다.  
  - 그중 가장 왼쪽에 있는 플립플롭에 저장된 비트 값을  MSB(Most Significant Bit)라고 한다.
  - 그중 가장 오른쪽에 있는 플립플롭에 저장된 비트 값을 LSB(Least Significant Bit)라고 한다. 
- 16비트 PC 레지스터의 경우, 
  - 상위 8bit(8~15)를 High 레지스터: PC(H)라고 한다
  - 하위 8bit(0 ~7)를 Low 레지스터: PC(L) 라고 한다. 

<br>

### 2) 레지스터 전송 (Register Transfer)

#### (1) 레지스터 정보 전송

- 치환 (replacement)연산자 사용
  - `R2 <- R1`
  - R1에 있는 데이터를 R2로 전송해라.
- 제어 조건이 있을 경우, 
  - `If (P=1) then (R2 <- R1)`
  - P가 1이면, R1에 있는 데이터를 R2로 전송해라.
- 제어 함수로 표현할 경우, 
  - `P: R2 <- R1`
  - P가 1이면, 다음 문장을 실행하라. 즉, 다음 클럭(t+1타이밍)에 전송완료된다.

<br>

#### (2) 레지스터 전송의 기본 기호

- Register data exchange
  - T:R2 <- R1, R1 <- R2

<br>

### 3) 버스와 메모리 전송 (Bus and Memory Transfers)

#### (1) 공통버스 (Common Bus)

![image-20220706011821633](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220706011821633.png)

<br>

- 레지스터들 사이의 전송 통로
  - 충돌을 막기 위해 동시에 하나의 device에서만 사용이 가능하다. 
  - 어떤 device가 이 버스를 사용하는가.를 제어하는 것이 멀티 플렉서이다. 
- 한 번에 하나의 신호만 전송하도록 제어한다. 
- 멀티플렉서를 사용하여 레지스터를 선택한다. => 공통버스를 제어하는 제어장치 (버스 제어기)

<br>

#### (2) 3-상태 버퍼 (3-state Buffer)

- 멀티플렉서 대신 사용하여 버스 구성을 할 수 있다.
- 3개의 상태로 조작한다.
  - 논리0, 논리 1: 정상적인 버퍼로 동작
  - 고저항상태: 출력 차단

<br>

#### (3) 메모리 전송

- Read: DR <- M[AR] 
  - AR번지에있는 메모리를 데이터 레지스터를 보낸다. 
- Write: M[AR] <- R1
  - R1값을 AR번지에 있는 메모리에 보낸다.

<br>

## 레지스터 전송과 마이크로 연산 파트2

[강의 링크](https://www.youtube.com/watch?v=IUapFpDKhKI&list=PLc8fQ-m7b1hCHTT7VH2oo0Ng7Et096dYc&index=9)

### 1) 산술 마이크로 연산 (Arithmetic Micro-operations)

> 레지스터간 이진 정보 전송
{: .prompt-tip }

#### (1) 이진 가산기

- 두 비트와 이진 캐리의 산술 합을 계산한다.
- 여러 개의 전가산기를 연결한다.

<br>

#### (2) 이진 감가산기

- 보수를 만드는 게이트와 신호를 사용한다.
  - M -> 0: 가산
  - M -> 1: 감산
- A(0-3), B(0-3), S(0-3)는 bus에 연결한다.

<br>

#### (3) 산술회로

- 4개의 전가산기
- 4개의 멀티플렉서
- 2개의 4비트 입력
- 1개의 출력
- 3개의 제어라인

<br>

### 2) 논리 마이크로 연산 (Logic Micro-operations)

> 수치 데이터에 대한 산술 연산
{: .prompt-tip }

<br>

<img src="https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220706184845964.png" width="700px">

<br>

### 3) 시프트 마이크로 연산 (Shift Micro-operations)

> 비수치 데이터에 대한 비트 조작 연산
{: .prompt-tip }

<br>

#### (1) 논리 시프트

- ex) 오른쪽으로 시프트한다:  한 비트씩 오른쪽으로 이동한다. 제일 왼쪽에 있는 남은 한비트가(빈 공간) 0이된다. 

- 즉, 직렬 입력으로 0이 전송된다.
- R1 <- shl(R1)
- R2 <- shr(R2)

<br>

#### (2) 순환 시프트

- 논리 시프트의 확장판이다. 
- ex) 오른쪽으로 시프트한다: 한 비트씩 오른쪽으로 이동한다. 이동하면서 튕겨나가는 비트가 제일 왼쪽에 있는 빈공간을 차지한다. 

- 즉, 직렬 출력이 직렬 입력으로 전송된다.
- cir, cil

<br>

#### (3) 산술 시프트

- 부호 비트를 제외하고 시프트한다. 
- 왼쪽 시프트: x2
- 오른쪽 시프트: /2

<br>
