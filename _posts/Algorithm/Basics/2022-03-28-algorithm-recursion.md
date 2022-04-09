---
title: 재귀와 조합적 문제
author: 박재경
date: 2022-03-28
categories: [Algorithm, Basics]
tags: [algorithm, recursion, combination, permutation, dp]
---



# 재귀 & 조합적 문제 (템플릿 위주)

## 1. 반복과 재귀

> 반복과 재귀는 유사한 작업을 수행할 수 있다. 

|                | 반복                  | 재귀                                    |
| -------------- | --------------------- | --------------------------------------- |
| 종료           | 반복문의 종료 조건    | 재귀 함수 호출이 종료되는 베이스 케이스 |
| 수행 시간      | 빠름                  | (상대적) 느림                           |
| 메모리 공간    | 적게 사용             | (상대적) 많이 사용                      |
| 소스 코드 길이 | 길다                  | 짧고 간결                               |
| 소스 코드 형태 | 반복 구조(for, while) | 선택 구조 (if..else)                    |
| 무한 반복시    | CPU를 반복해서 점유   | 스택 오버 플로우                        |

<br>

### 1) 재귀

> 재귀적 정의를 구현하기 위해 재귀호출을 사용

**재귀적 정의 : 부분 문제간의 관계를 기술한다.**

- 큰 문제와 작은 문제 사이의 관계
- 좀 더 작은 문제의 해를 구해서 큰 문제의 해를 구하는 방법
- 관계를 점화식으로 기술한다.

<br>

`사용 용도`

| **그래프의 `깊이 우선 탐색` (ex. 트리 순회)**                |
| ------------------------------------------------------------ |
| **상태공간 트리를 탐색하는 `백트래킹`**                      |
| **`분할 정복` (ex. 이진 탐색, 퀵 정렬, 병합 정렬)**          |
| **`재귀적 DP` 재귀 호출 + 메모이제이션 (재귀적 정의를 포함한다.)** |

<br>

## **2. 조합적 문제** 

> **모든 가능한 경우를 조사해서 해를 찾는다.**

- 모든 가능한 경우를 나열(생성)해야 한다. (generate-and-test, exhaustive search)
- 어려운 문제는 완전 탐색도 쉽지가 않다.
- 조합적 문제  ==> 부분집합, (중복)순열, (중복)조합



**모든 가능한 경우를 체계적으로 탐색하는 방법으로 백트래킹 기법을 사용한다.**

- 상태공간 트리를 탐색 -> 깊이 우선 탐색, 너비 우선 탐색, 최고 우선 탐색
- 가지치기 ==> 분기한정(Branch and bound) 기법

<br>

### **1) 부분집합**

> 집합에 포함된 원소들을 선택하는 것이다. 
>
> 2^n^

#### (1) 재귀 

```python
def recur(cur, cnt):
    if cnt == 종료조건:
        check() # 종료 직전에 해야할 계산!
        return

    if cur == N: #깊이 끝까지 가면 종료
        return

    recur(cur + 1, cnt + 1) #고른다! 인자를 추가해서 값을 넘겨줘도 된다. 
    recur(cur + 1, cnt) #고르지 않는다!
```

- 모든 부분집합을 생성하는 과정을 트리로 디자인
  - 선택의 단계: 요소의 수 N 만큼
  - 매 선택의 순간 선택지: 2가지(포함 또는 미포함) ===> 선택지만큼 재귀 호출

- 선택(결정)의 과정을 트리로 표현

- 매개변수 전달하지 않고, `append()`, `pop()` 가능

<br>

#### (2) 바이너리 카운트

> 원소 수에 해당하는 N개의 비트열을 이용한다. 

n번째 비트 값이 1이면, n번째 원소가 포함되었음을 의미한다. 

```python
arr = [i for i in range(1, 13)]
N = len(arr)

# 원소수가 N인 집합의 모든 부분집합에 대응하는 10진수 => 0 ~ (2^n - 1)
for subset in range(0, 1 << N):
    # subset의 N개의 bit를 조사
    S = cnt = 0
    for i in range(N):  # N => 0 ~ 3
        if subset & (1 << i):
            print(arr[i], end=' ')
            S += arr[i]
            cnt += 1
    print('=>', S, cnt)
```

<br>

### **2) 순열**

> 서로 다른 것들 중 몇 개를 뽑아서 한 줄로 나열 하는 것이다. 

다수의 알고리즘 문제들은 순서화된 요소들의 집합에서 최선의 방법을 찾는 것과 관련이 있다. 

N개의 요소들에 대해서 n! 개의 순열들이 존재하기 때문에, n > 12인 경우, 시간 복잡도가 폭발적으로 올라간다. 

<br>

#### (1) 재귀 호출을 통한 순열 생성 

```python
arr = [1, 2, 3]
N = len(arr)
visit = [0] * N  # 요소의 선택 여부 저장
order = [0] * N  # 실제 순열의 순서를 저장

# =============================================
# 1. 반복 구조
for i in range(N):      # 첫번째 요소 선택
    if visit[i]: continue
    order[0] = arr[i]
    visit[i] = 1
    #----------------------------------------------
    for j in range(N):  # 두번째 요소 선택
        if visit[j]: continue
        order[1] = arr[j]
        visit[j] = 1
        #------------------------------------------
        for k in range(N): # 세번째 요소 선택
            if visit[k]: continue
            order[2] = arr[k]
            visit[k] = 1
            print(order)
            visit[k] = 0
        #------------------------------------------
        visit[j] = 0
    #------------------------------------------
    visit[i] = 0

# =============================================
# 2. 재귀 구조
def perm(k, N):
    if k == N:
        print(order)
    else:
        for i in range(N):  # 첫번째 요소 선택
            if visit[i]: continue
            order[k] = arr[i]
            visit[i] = 1
            #---------------------
            perm(k + 1, N)
            #---------------------
            visit[i] = 0
perm(0, N)

```

- 선택 여부를 전역 배열을 사용해서 생성한다. 

<br>

`비트 연산을 활용`

메모이제이션을 적용하기 위해서 필요하다. 

```python
arr = [10, 20, 30]
N = len(arr)
order = [0] * N   # 순열을 저장, 나의 선택을 저장
def perm(k, n, used):
    if k == n:
        print(order)
    else:
        for i in range(n):
            if used & (1 << i): continue # i번 요소를 이미 선택한 경우
            order[k] = arr[i]    # 나의 선택을 저장
            perm(k + 1, n, used | (1 << i))

perm(0, N, 0)

```

<br>

#### (2) 교환을 통한 생성

각각의 순열들은 이전의 상태에서 단지 두 개의 요소들 교환을 통해 생성된다. 

ex) [**1**, 2, **3**] , [**3**, **2**, 1], [2, **3**, **1**], [**2**, 1, **3**], [**3**, **1**, 2], [1, 3, 2,]

```python

arr = [1, 2, 3, 4]
N = len(arr)

# 반복구조
for i in range(0, N):
    arr[0], arr[i] = arr[i], arr[0]
    # ========================================
    for j in range(1, N):
        arr[1], arr[j] = arr[j], arr[1]
        # ========================================
        for k in range(2, N):
            arr[2], arr[k] = arr[k], arr[2]
            print(arr)
            arr[2], arr[k] = arr[k], arr[2]
        # ========================================
        arr[1], arr[j] = arr[j], arr[1]
    # ========================================
    arr[0], arr[i] = arr[i], arr[0]


# =================================
# 재귀 구조
def perm(k, N):
    if k == N:
        print(arr)
    else:
        for i in range(k, N):
            arr[k], arr[i] = arr[i], arr[k]
            perm(k + 1, N)
            arr[k], arr[i] = arr[i], arr[k]

perm(0, N)
```

<br>

### **3) 조합**

> 서로 다른 n개의 원소 중 r개를 순서 없이 골라낸 것을 조합이라고 부른다. 

`조합의 재귀적 표현`

**~n~C~r~ = ~n-1~C~r-1~ + ~n-1~C~r~** 

 [1, 2, 3, 4, 5] 3개를 뽑는다고 칠 경우,  

1.  1이 무조건 있을 경우 4개중 2개를 뽑는다.
2.  1이 없을 경우 4개중 3개를 뽑는다 .

위의 합이 결국 모든 경우의 수 이다. 

<br>

```python
# 순열
arr = "ABCD"
N = len(arr)
for i in range(N):
    for j in range(N):
        if j == i: continue
            for k in range(N):
                if k == i or k == j: continue
                print(arr[i], arr[j], arr[k])
                
# 조합
for i in range(N):
    for j in range(i+1, N):
            for k in range(j+1, N):
                print(arr[i], arr[j], arr[k])
```

<br>

```python
arr = 'ABCD'
N = len(arr)
R = 3
pick = [0] * R

def comb(k, s):  # s: for문의 시작
    if k == R:
        print(pick)
    else:
        for i in range(s, N):
            pick[k] = arr[i]
            comb(k + 1, i + 1)

comb(0, 0)
```

<br>

```python
def nCr(n, r, s):   # n개에서 r개를 고르는 조합. s 고를 수 있는 구간의 시작 인덱스
    if r==0:
        print(comb)
    else:
        for i in range(s, n-r+1):
            comb[r-1] = A[i]
            nCr(n, r-1, i+1)

n = 5
r = 3
comb = [0]*3
A = [i for i in range(1, n+1)]
nCr(n, r, 0)

```



<br>

## 3. DP

#### (1) 피보나치

- 메모이 제이션 적용하기 전에
  - 점화식을 재귀호출로 구현한 상태
  - 반환값을 처리하는 형태
- 문제 크기 만큼 공간(배열)을 생성한다.
  - 문제의 해가 되지 않는 값으로 초기값을 선택한다.
- 함수 호출 내부에서 매개변수(n)에 해당하는 문제를 이미 풀었는지 확인한다.
  - memo[n] 에 초기값인지 아닌지 확인
  - 풀었다면 memo[n] 을 리턴하고 종료
  - 그렇지 않으면, 계속 진행해서 결과를 계산해서 memo[n]에 저장한다.

```python
# 메모이제이션 적용하기
# n번 피보나치 수를 구하기 위해서 풀어야할 문제들 = 0 ~ N 까지
def fibo(n):
    if memo[n] != -1:
        return memo[n]

    memo[n] = fibo(n - 1) + fibo(n - 2)
    return memo[n]


N = 7
memo = [-1] * (N + 1) # 초기값은 아직 그 문제의 답을 구하지 못했다.
memo[0], memo[1] = 0, 1
print(fibo(N))
```

<br>

- 반복 DP

```python
def fibo_iter(n):
    dp = [0] * (N + 1)
    dp[0], dp[1] = 0, 1

    for i in range(2, n + 1):  # i=> 문제를 나타내는 값
        dp[i] = dp[i - 1] + dp[i - 2]

    return dp[n]
```

<br>
