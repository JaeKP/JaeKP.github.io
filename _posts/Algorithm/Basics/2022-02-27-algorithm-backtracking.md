---
title: 백트래킹
author: 박재경
date: 2022-02-27
categories: [Algorithm, Basics]
tags: [algorithm, dfs, backtracking]
---



# 백트래킹

`알고리즘 문제 유형`

- ##### 결정 문제(decision problem)

  - Yes(True, 1) or No(False, 0) 로 답하는 문제

- ##### 최적화 문제(optimization problem)

  - 가장 어려운 문제 유형, 지수승 이상의 시간 복잡도를 요하는 문제
  - 문제의 조건에 해당하는 값이 최대 혹은 최소가 되는 경우를 찾는 문제
    - ex> 최단경로
  - 최적해를 찾는 문제 --> 모든 후보해(가능한 모든 경우)를 조사해야 한다.

<br>

## 1. 최적화 문제 

- 완전 탐색이 기본이다. 그러나, 모든 후보해(가능한 경우)를 나열(생성)해야 한다. 그래서 시간이 느리다.

- 일반적으로, 모든 가능한 경우를 쳬계적으로 따지는 방법으로 백트래킹을 적용한다. (시간을 줄이기 위한 노력...)
  - 상태공간 트리(그래프)로 모델링(디자인)
  - 트리/그래프 탐색방법으로 DFS, BFS 사용
- 모든 가능한 경우는 조합적 문제와 연관 있다.
  - 부분집합, 순열, 조합, ..
  - 2^n, n!, ..
- Backtracking => 상태공간트리 탐색 + 가지치기
- Dynamic programming => 문제간의 관계 +  메모이제이션

<br>

## 2. 백트래킹이란

>  해를 찾는 도중에 막히면(즉, 해가 아니면) 되돌아가서 다시 해를 찾는 기법이다. 
>
>  백트래킹 기법은 최적화 문제와 결정 문제를 해결할 수 있다. 

**여러가지 선택지(옵션)들이 존재하는 상황에서 한가지를 선택한다. 선택이 이루어지면 새로운 선택지들의 집합이 생성된다.**

**이런 선택을 반복하면서 최종 상태에 도달한다. (올바른 선택을 계속하면 목표 상태에 도달한다. )**

<br>

### 1) 깊이 우선탐색과의 차이점

- 깊이 우선 탐색이 모든 경로르 추적하는 데 비해 백트래킹은 불필요한 경로를 조기에 차단한다. 
- 깊이 우선 탐색을 가하기에는 경우의 수가 너무 많다. N!가지의 경우의 수를 가진 문제에 대해 깊이 우선 탐색을 가하면 당연히 처리 불가능하다. 
- 백트래킹 알고리즘을 적용하면 일반적으로 경우의 수가 줄어들지만, 이 역시 최악의 경우에는 여전히 지수함수 시간을 요하므로 처리가 불가능하다. 

<br>

### 2) 절차 

- 상태 공간 트리의 깊이 우선 검색을 실시한다.
- 각 노드가 유망한지를 점검한다.
- 만일 그 노드가 유망하지 않으면, 그 노드의 부모 노드로 돌아가서(backtracking) 검색을 계속한다. 

<br>

### 3) 가지치기

- 불필요한 경로를 조기에 차단해서 효율을 높인다.
- 즉, 어떤 노드의 유망성을 점검한 후에 유망(promising)하지 않다고 결정되면 그 노드의 부모로 되돌아가 다음 자식 노드로 간다. ``
  - 어떤 노드를 방문하였을 때, 그 노드를 포함한 경로가 해답이 될 수 없으면 그 노드는 유망하지 않다고 하며, 반대로 해답의 가능성이 있으면 유망하다고 한다. 
  - 유망하지 않는 노드가 포함되는 경로는 더 이상 고려하지 않는다.  

<br>

## 3. 예시_부분 집합 생성

- 모든 부분집합을 생성하는 과정을 트리로 디자인

  - 선택의 단계: 요소의 수 N 만큼

  - 매 선택의 순간 선택지: 2가지(포함 또는 미포함) ===> 선택지만큼 재귀 호출

- 선택(결정)의 과정을 트리로 표현
  - 그림을 그리자.

<br>

```python
arr = 'ABCD'; N = len(arr)
bits = [0] * N
# =======================================
# 1단계 반복 구조
for i in range(2):
    bits[0] = i
    for j in range(2):
        bits[1] = j
        for k in range(2):
            bits[2] = k
            print(bits)

# =======================================
# 2단계 재귀구조로 bit 표현 생성
def subset(k, n): # k: 함수호출의 깊이, 노드의 높이, for문의 중첩횟수
    if k == n:
        print(bits, end=' ')
        for i in range(n):
            if bits[i]: print(arr[i], end=' ')
        print()
    else:
        bits[k] = 0
        subset(k + 1, n)
        bits[k] = 1
        subset(k + 1, n)
subset(0, N)

# =======================================
# 3단계
# 전체집합에서 부분집합 생성하고 합계산하기
arr = [1, 2, 1, 2]
N, K = 4, 3
bits = [0] * N
def subset(k, n): # k: 함수호출의 깊이, 노드의 높이, for문의 중첩횟수
    if k == n:
        print(bits, end=' ')
        S = 0
        for i in range(n):
            if bits[i]:
                print(arr[i], end=' ')
                S += arr[i]
        print('==>', S)
    else:
        bits[k] = 0
        subset(k + 1, n)
        bits[k] = 1
        subset(k + 1, n)

subset(0, N)

# =======================================
# 4단계
# 부분집합을 생성하는 과정에서 합을 계산해서 매개변수로 전달하기
arr = [1, 2, 1, 2]
N, K = 4, 3
bits = [0] * N
def subset(k, n, cur_sum): # k: 함수호출의 깊이, 노드의 높이, for문의 중첩횟수
    if k == n:             # cur_sum: 현재까지 선택한 요소의 합
        print(bits, end=' ')
        S = 0
        for i in range(n):
            if bits[i]:
                print(arr[i], end=' ')
                S += arr[i]
        print('==>', S, cur_sum)
    else:
        bits[k] = 0     # k번 요소를 포함하지 않음
        subset(k + 1, n, cur_sum)
        bits[k] = 1     # k번 요소를 포함
        subset(k + 1, n, cur_sum + arr[k])

subset(0, N, 0)

# =======================================
# 5단계
# 가지치기 작용하기
arr = [i for i in range(1, 11)]
N, K = len(arr), 10
cnt = 0
def subset(k, n, cur_sum): # cur_sum: 현재까지 선택한 요소의 합
    if cur_sum > K:
        return

    if k == n:
        global cnt
        if cur_sum == K:
            cnt += 1
    else:
        subset(k + 1, n, cur_sum)
        subset(k + 1, n, cur_sum + arr[k])

subset(0, N, 0); print(cnt)

#=======================================
#6 단계
# 가지치기 작용하기

arr = [i for i in range(1, 11)]
N, K = len(arr), 10
cnt = 0
def subset(k, n, cur_sum, rem_sum): # cur_sum: 현재까지 선택한 요소의 합
    if cur_sum > K:
        return
    if cur_sum == K:
        global cnt
        cnt += 1
        return
    if cur_sum + rem_sum < K:
        return

    if k == n:
        return

    subset(k + 1, n, cur_sum, rem_sum - arr[k])
    subset(k + 1, n, cur_sum + arr[k], rem_sum - arr[k])

subset(0, N, 0, sum(arr)); print(cnt)

```

<br>
