---
title: 컴퓨터구조-데이터의-표현
author: 박재경
date: 2022-07-02
categories: [CS, 컴퓨터-구조]
tag: [cs, 컴퓨터-구조]
---



## 데이터의 표현 파트 1

[강의링크](https://www.youtube.com/watch?v=aSocCv3SC2k&list=PLc8fQ-m7b1hCHTT7VH2oo0Ng7Et096dYc&index=6)

### 1) 데이터의 종류 (Data Types)

#### (1) 컴퓨터 레지스터에서 쓰이는 데이터 종류

- 산술 연산용 숫자 (Numeric): 원래 숫자처리를 하기 위해 만들어진 것이 컴퓨터이다. 
- 데이터 처리용 영문자(Alpha)
- 특수 목적용 기호 (Special)

<br>

#### (2) 진수와 진법

- radix: 진법의 기수에 해당 (10, 2, 8, 16...)
- 10진수
- **2진수: 컴퓨터의 이진법 체계 **
  - 2진화 8진수 (Octal)
  - 2진화 16진수 (Hexadecimal)
  - 2진화 10진수 (BCD: Binary Code Decimal)
- 8진수
- 16진수

<br>

![image-20220629105633942](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220629105633942.png)

<br>

#### (3) 영숫자(AlphaNumeric) 표시

- ASCll Code: 7bits (+ 1parity bits) 
  - 미국에서 사용되는 `표준`
  - 상위 4비트의 존비트와 하위 4비트의 숫자비트로 구성된다. 
- EBCDIC Code: 16bits, IBM internal code
- UniCode: 16bits/ 32bits
  - ASCll코드의 표현의 한계를 해결 (ASCII 코드는 7비트이기 때문) => 16비트를 사용하여 세계 모든 언어의 문자와 그밖의 기호에 코드 값을 부여
  - 다른 나라의 문자를 사용할 경우, ASCll코드로는 표현을 하기 어렵기 때문에 Unicode 체계가 자리잡게 되었다. 
  - 기본적으로 ASCll코드와 유사하다. 

<br>

**`USA ASCll Code TABLE`**

<img src="https://raw.githubusercontent.com/JaeKP/image_repo/main/img/ascii_table_black.png" width=800px>

<br>

### 2) 보수 (Complements)

#### (1) 정의

- 보충을 해주는 수
- 진법의 기수 r에 대응하는 역 값
- 뺄셈과 논리 계산에 사용된다. 

<br>

#### (2) (r-1)의 보수 체계

- r-1에서 각 자리 숫자를 뺀다. 
- ex) 10진수 12389에 대한 9의 보수 = 99999 - 12389 = 87610
- ex) 2진수 0011에 대한 1의 보수  = 1111 - 0011 = 1100  `xor`

<br>

#### (3) (r)의 보수 체계

- r-1의 보수에 1을 더한다. 
- ex) 10진수 12389에 대한 10의 보수 = 100000 -12389 = 87611
- ex) 2진수 0011에 대한 2의 보수 = 1100 + 1 = 1101 

<br>

 💬 **컴퓨터는 원래 뺄셈, 곱하기, 나누기를 할 줄 못한다?**  

- 원래 cpu는 해당 기능이 없다.
- 모든 연산을 덧셈으로 한다.  (가산기를 이용해 음수 연산을 한다.)
  - 곱셈: 덧셈의 반복
  - 뺄셈:  보수 연산 (보수의 덧셈)
  - 나눗셈: 보수 연산의 반복


<br>

## 데이터의 표현 파트 2

[강의링크](https://www.youtube.com/watch?v=bysGzutpRgc&list=PLc8fQ-m7b1hCHTT7VH2oo0Ng7Et096dYc&index=7)

### 1) 고정 소수점 표현 (Fixed Point Representation)

<br>

![image-20220704160237100](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220704160237100.png)

<br>

#### (1) 정의

- 소수점의 위치를 결정하여 숫자를 표현한다.
- 레지스터 비트에 소수점 위치를 표시한다. 
- 16bit 정수의 경우, 최우측 (LSB)에 소수점 자리가 위치한다.
- 부동 소수점의 경우, 레지스터 비트 앞/ 중간에 소수점 자리 위치한다. 

<br>

#### (2) 정수의 표현 

- MSB로 부호를 표현한다.
- 양수는 MSB가 0,  음수는 MSB가 1이다.

<br>

**💬-14를 표시한다면..?**

음수이기 때문에 MSB는 1이다.

14는 1110이기 때문에  -14를 16bit로 표현한다면 1 0001110이 된다.

만약 부호화된 1의보수로 표현하면 , 1110001이다.

만약 부호화된 2의 보수로 표현하면,  1 1110010이다.  => 대부분의 컴퓨터. CPU에서 활용된다. 

<br>

#### (3) 오버플로우(overflow)발생

- N자리의 두수를 더하여 N+1자리의 합이 발생했을때, 가수와 피가수의 부호와 관계없이 발생한다.
- 정해진 레지스터의 비트수로 인한 문제이다.
- 정해진 비트수 내에서만 연산이 가능한 컴퓨터에서 발생한다. 

<br>

**`발생원인`**

![image-20220704165249316](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220704165249316.png)

- 연산 결과값이 레지스터의 비트수를 초과할 경우 발생한다.  
- 두 수의 부호가 같을 경우에만 발생한다. (부호 비트를 더해버려서) 
- 레지스터에 저장된 연산 결과값은 잘못된 값으로 저장된다. 

<br>

**`처리 방법`**

- 오버플로우 발생을 미리 확인한다.
  - MSB의 두 캐리 비트의 값이 서로 다르다. 
  - 8번째 비트와 7번째 비트의 값이 다르면 오버플로우다. 
- 연산을 처리하지 않고 인터럽트 또는 에러 처리한다. 

<br>

### 2) 부동 소수점 표현 (Floating Point Representation)

#### (1) 부동소수점 표시 방법 (IEEE 754)

![image-20220704170558564](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220704170558564.png)

<br>

- 가수와 지수로 표현한다. (32bit, 64bit 등 컴퓨터마다 다르다.)
- 가수(matissa): 분수(fraction), 정수값 표시한다. 
- 지수(component): 십진, 이진 소수점 위치를 표시한다.

<br>

**💬 +1001.11을 32bit로 표현한다면?**

- fraction: 01001110
- component: 000100
- 부호비트, 지수비트(8bit), 가수비트(23bit)로 구성된다.

0   1000100(128biased)   00111000000000000000000(가장 좌측 1을 제외한다.) 

가수의 경우, 어차피 소수점 다음 자리는 무조건 1이니까 더 많은 수를 표현을 하기 위해서 가장 좌측의 1을 제외한다. 

<br>

#### (2) 정규화(Normalization)

- 부동소수점 숫자에서 최상위 비트가 0이 아닌 경우, 0이 있으면 Mantissa의 소수점 위치가 이동한다.
- 이동한 만큼 exponent의 값이 변경된다.

<br>

### 3) 기타 이진 코드 (Other Binary Codes)

![other-binary-code](https://raw.githubusercontent.com/JaeKP/image_repo/main/img/other-binary-code.jpg)

<br>

#### (1) Grey Code

- 한 숫자에서 다음 숫자로 변할 때 한 비트만 변동한다.
- 제어 계통에 주로 사용된다.
- 여러 전기 신호가 동시에 바뀔 때 에러 발생률이 낮다. 

<br>

#### (2) BCD Code

- 10진수에 대한 2진수를 표현한다. (i.e.8421 code)
- 4bit를 사용한다. (0~9까지 사용한다.)

<br>

#### (3) Excess-3

- binary + 3(0011)으로 표현한다. 
- 암호 교신의 기본 코드로 파생 암호 발생 방법에 사용된다. 

<br>

### 4) 에러 검출 코드 (Error Detection Codes)

#### (1) Parity bit

<img src="https://raw.githubusercontent.com/JaeKP/image_repo/main/img/image-20220704175330047.png" width="400px">

<br>

- 외부 잡음에 의한 에러 발생을 검출하기 위해 별도의 데이터 bit인 Parity bit를 사용한다. 
- 짝수(even) 패리티/ 홀수(odd) 패리티가 있다. 
  - 짝수 패리티: 실제 송신하고자 하는 데이터의 각 비트의 값 중에서 1의 개수가 짝수가 되도록 패리티 비트를 정한다.
  - 홀수 패리티: 실제 송신하고자 하는 데이터의 각 비트의 값 중에서 1의 개수가 홀수가 되도록 패리티 비트를 정한다.
- 가장 간단하고도 일반적인 방법이지만, 2개 비트 동시 에러의 경우 검출이 불가능하다. 

<br>

#### (2) Parity bit 적용

- 송신측: 패리티 발생기
- 수신측: 패리티 검사기
- 수신측의 패리티 검새결과가 데이터 패리티와 일치하면 에러가 없는 것이다. => 0 출력
- 수신측의 패리티 검사결과가 데이터 패리티와 불일치하면 에러가 발생한 것이다 => 1출력

<br>