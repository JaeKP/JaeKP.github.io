---
title: 컴퓨터구조-디지털-부품
author: 박재경
date: 2022-06-28
categories: [CS, 컴퓨터-구조]
tag: [cs, 컴퓨터-구조]
---



## 디지털 부품 파트1

[강의링크](https://www.youtube.com/watch?v=aj74NlGUAk4&list=PLc8fQ-m7b1hCHTT7VH2oo0Ng7Et096dYc&index=4)

### 1) 집적회로 (Integrated Circuits)

#### (1) 정의

- 디지털 게이트를 구성하는 전자 부품을 포함하는 실리콘 반도체 칩이다.
- 칩 내부에 여러가지 소자들이 존재한다. (대부분 우리가  컴퓨터에서 사용하는 칩은 논리게이트가 있는 칩이다.)
- 즉, 칩 내부에 게이트들이 연결되고 외부로도 핀을 통해 연결된다. 
- 칩의 등록번호로 구분한다. (dataBook을 통하여 정보 확인이 가능하다. )

<br>

#### (2) 집적 규모에 따른 분류

> IC 내부에 얼마나 많은 디지털 게이트나 전자부품이 몰려있는가에 따라 구분된다. 
{: .prompt-tip }

- SSL: 소규모, 10개 이하의 게이트들로 구성된다. 
- MSI: 중규모, 10~200개의 게이트들로 구성된다. ex) 디코더, 가산기, 플립플롭, 레지스터 등 
- LSI: 대규모, 200~1,000개의 게이트들로 프로세서나 메모리 칩과 같은 디지털 시스템을 형성한다.  ex) 규모가 작은 칩
- VLSI: 초대규모, 수천개 이상의 게이트 집적, 대형 메모리나 마이크로 컴퓨터 칩을 구성한다.  ex) CPU

<br>

#### (3) 디지털 논리군에 따른 분류 

> 어떤 논리군에 있느냐에 따라 분류가 가능하다.
{: .prompt-tip } 

- TTL: Tnansister-Transistor Logic 
  - 일반 로직 회로 부품 
  - 일반적인 게이트 단위로 만들어진 회로 
  - and 혹은 not 게이트인 트랜지스터로 구성

<br>

- ECL: Emitter-Coupled Logic
  - 고속 논리 시스템용 부품 (1~2ns 이하), 슈퍼 컴퓨터 용
  - 하나의 게이트의 입력부터 출력까지 걸리는 시간(게이트 딜레이) 이 매우 짧다.
  - TTL은 게이트 딜레이가 1ms~2ms인데 ECL은 TTL보다 굉장히 빠르기때문에 고속 논리를 요구하는 컴퓨터에서 사용된다.
  - 하지만 그만큼 비싸다.  

<br>

- MOS: Metal Oxide Semiconductor
  - 고밀도 집적회로용 부품

<br>

- CMOS: Complement Metal Oxide Semiconductor
  - 고밀도 회로, 단순한 제조공정
  - MOS에 비해 저전력
  - ex) 베터리 전력 소모에 예민한 핸드폰에 사용된다. 

<br>

### 2) 디코더 (Decoders)

> 컴퓨터에서 가장 많이 사용되는 조합회로이다. 
{: .prompt-tip }

#### (1) 정의

- N비트의 이진 정보를 서로 다른 2의 n승개의 원소 정보로 출력한다. 
  - 2개의 입력 => 4가지 (2의 2승) 출력 `2X4 decoder`
  - 3개의 입력 => 8가지 (2의 3승) 출력 `3X8 decoder`
- 즉, 여러 개의 장치가 있는데 그중에 한개의 장치만 키거나 끄는 기능을 한다. 
- 입력순서와 출력순서는 높은 비트의 입력이나 출력이 먼저다.

<br>

![3-to-8-line-decoder](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/3-to-8-line-decoder.png)





<br>

#### (2) NAND 게이트로 이루어진 디코더

- and게이트로 만들어지는 게이트보다는 nand게이트로 이루어진게 더 경제적이고 사용이 좋다.
- 즉, 출력 신호가 다 1이고 내가 고른 것만 0이 되는것이 더 경제적이다. (대부분의 출력회로가 1로 유지된다.)
- 원인: CMOS 회로의 영향때문이다.  신호가 1이 되면 전력을 사용하지 않게 되고 0이 되면 전력을 사용하게 된다. 

<br>

![2-to-4-decoder-nand](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/2-to-4-decoder-nand.jpg)

<br>

#### (3) 인코더

> 디코더와 반대의 개념인 조합회로
{: .prompt-tip }

- 디코더와 반대 동작을 수행한다. 
- 2의 N승의 입력에 대하여 N 이진 코드를 출력한다. 
- 한번에 하나의 입력만이 1의 값을 가질 수 있다. 

<br>

### 3) 멀티플렉서 (Multiplexers)

#### (1) 정의

- N개의 선택 입력에 따라서 2의 N승개의 출력을 하나의 출력에 선택적으로 연결한다.
- 다중 입력 중 하나를 선택하여 출력으로 연결된다.
- 네트워크 스위치 (전화, LAN, WAN)의 기본 구조 요소이다.  
  - 전화: 여러개의 전화기 중 하나를 선택하는 전화기의 교환기
  - LAN: 
  - 여러 ip 의 입력 중 내 ip만 보내주는..
- 여러개의 입력중 하나를 고르기 위해 Selector가 존재한다. ex) 만약 입력이 8개면 selector는 3개 (2의 3승 = 8이니까)

<br>

![4-to-one-multi-plexers](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/4-to-one-multi-plexers.png)

<br>

### 4) 레지스터 (Registers)

#### (1) 정의

- 컴퓨터의 기억장치 중에서 가장 빠르고 용량이 적다. (비쌈ㅎ)

- N비트의 데이터를 저장하는 기능을 가진 반도체 소자로 구성된 단위 논리 집단
- `N비트 레지스터`: N비트의 이진 정보를 저장한다. 
  - N개의 플립플롭과 조합회로로 구성된다.
  - 플립플롭은 1bit를 저장 가능하다.

<br>

![4-bit-register](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/4-bit-register.png)

<br>

💬 **무엇을 저장하는가..**

- CPU가 요청을 처리하는 데 필요한 데이터 
- CPU는 계산이 목적이기 때문에, 레지스터는 연산 과정에서 임시로 처리할 데이터를 저장하거나 메모리의 주소를 저장한다. 

<br>

#### (2) 레지스터의 필요성

- CPU의 계산속도 보다 RAM에 데이터를 접근하는 시간이 더 오래 걸린다. 
- CPU가 임시 사용할 수 있는 저장 공간을 만들어서 CPU 효율을 높인다.

<br>

#### (3) 기본 레지스터

- 클럭펄스 타이밍에 입력값이 레지스터에 저장된다.
- 레지스터에 저장된 값은 항상 출력에서 참조가 가능하다.
- Clear, Clock 입력을 제공한다. 

<br>

#### (4) 병렬로드 가능한 4비트 레지스터

- 4비트의 데이터를 동시에 입력이 가능하다.
- Load, Clock 입력을 제공한다. 

<br>

## 디지털 부품 파트 2

[강의링크](https://www.youtube.com/watch?v=7VPjQMeiHg0&list=PLc8fQ-m7b1hCHTT7VH2oo0Ng7Et096dYc&index=5)

### 1) 시프트 레지스터 (Shift Registers)

#### (1) 정의

- 레지스터에 저장된 이진 정보를 단방향/ 양방향으로 이동이 가능한 레지스터이다.

- 클럭, 퍼스가 들어왔을 때 레지스터에 있는 정보를 단방향으로 비트를 이동시킨다. 
- 각 FF들의 입력이 출력과 cascade로 연결된다.
- 공통의 colck이 다음 상태로의 이동을 제어한다. 

<br>

![image-20220628173033829](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220628173033829.png)

<br>

#### (2) 병렬로드를 가지는 양방향 시프트 레지스터 (General Register)

- 병렬로드, 왼쪽 / 오른쪽 시프트, 병렬출력이 가능하다.
- 동기화된 clock에 의하여 동작한다.
- 범용 레지스터를 지칭한다. 

<br>

![image-20220628183506186](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220628183506186.png)

<br>

### 2) 이진 카운터 (Binary Counters)

#### (1) 정의

- 레지스터의 값이 정해진 순서대로 상태(출력) 변이가 수행되는 레지스터이다. 
- 상태(출력)는 Clock이나 외부 입력에 따라 변이 된다. 
- 용도
  - 사전의 발생 횟수 카운트
  - 동작 순서 제어 타이밍 신호 발생에 적용한다. 
  - 컴퓨터는 프로그램 저장 방식이기 때문에 프로그램을 미리 저장해놓고 일정한 순차에 의해 실행한다. => 카운터

<br>

![image-20220628183235398](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220628183235398.png)

<br>

#### (2) 병렬입력을 가진 이진 카운터

- 카운터의 초기값을 설정할 수 있다.
- 병렬 입력을 통하여 초기값을 로드한다.
- Load, Clear, Increment기능이 있다. 

<br>

### 3) 메모리 장치 (Memory Unit)

#### (1) 정의

- 정보의 입출력 기능을 가지는 저장 요소들의 집합
- Word 단위로 정보를 저장한다.
  - Word: 입출력에서 하나의 단위로 취급되는 비트의 그룹이다. (한번에 읽고 쓰는)
  - 16bit 컴퓨터: 레지스터/ 메모리 버스의 크기가 16 bit (2byte) => 데이터를 주고받는 기본단위가 16비트
  - 64bit 컴퓨터: 레지스터/ 메모리 버스의 크기가 64 bit (8byte)  => 데이터를 주고받는 기본단위가 64비트
- `Byte`: 워드의 기본단위

<br>

#### (2) RAM (Random Access Memory): 휘발성 메모리

- Word의 물리적인 위치에 관계 없이 데이터에 접근할 수 있다.
- 모든 데이터 위치에 대하여 동일한 접근 시간을 가진다.
- N비트의 입력 / 출력 (Word 크기와 동일하다)
- K개의 주소 라인으로 2의 k승개 word 중 하나를 선택한다. (메모리 번지) => `address`
- 읽기 / 쓰기를 지정한다 (R/W)

<br>

![image-20220628174732294](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220628174732294.png)

<br>

#### (3) ROM (Read Only Memory): 비 휘발성 메모리

- 한 번 저장된 데이터를 읽기만 가능하다. 
- 1 word가 N비트이고 M월드를 저장하는 N x M ROM이다.
- ROM에 저장된 M word를 접근할 수 있는 K개의 주소를 입력한다. (2의 k승  =  M )

<br>

![image-20220628182525222](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220628182525222.png)

<br>

 💬 **ROM의 종류**

- Mask ROM: 석판화 (Lithography)방식으로 구워져 나오는 ROM이다.
- PROM: 한번만 프로그램 가능하다.
- EPROM : UV에 의한 데이터 삭제 (ROM 초기화) 및 재 프로그래밍 가능하다.
- EEPROM: 전기 신호에 의한 데이터 삭제/ 초기화 및 재 프로그래밍이 가능하다.

<br>

 💬 **ROM의 기능을 하는 RAM: 비휘발성 RAM, 특정한 신호가 있을 때만 write할 수 있다.**

- Flash-RAM 
  - BIOS
  - USB memory
  - SD card
- NV-RAM 
  - Non-Volatile RAM
  - Battery Backup RAM : 내장 메모리에 의해 데이터를 홀드하고 있다. 

<br>