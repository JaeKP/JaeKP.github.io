---
title: 정렬과 검색
author: 박재경
date: 2022-02-17
categories: [Algorithm, Basics]
tags: [algorithm, sort, search, array]
---



# 정렬과 검색

## 1. 정렬

### 1) 버블 정렬

- 첫 번째 원소부터 인접한 원소끼리 값을 비교하면서 자리를 교환한다. 
  - 뒤에서부터 앞으로 정렬을 진행한다. 
  - 내림차순이면 작은 값이 뒤로 가도록 자리를 교환
  - 오름차순이면 큰 값이 뒤로 가도록 자리를 교환

- 크기를 비교하면서 자리를 바꾸는 모습이 방울이 이동하는 것처럼 보여서 거품정렬이라는 이름이 붙었다. 
- 시간 복잡도: O(n**2)

<br>

```python
# 오름차순 버블정렬 
def BubbleSort(arr)              # 매개변수: 미정렬 배열
    N = len(arr)                 # N = 정렬할 배열의 원소의 개수
    for i in range(N-1, 0, -1):  # 범위의 끝 위치
        for j in range(0, i):
            if arr[j] > a[j+1]:
                a[j], a[j+1] = a[j+1], a[j]
            
```

<br>

### 2) 카운팅 정렬

- 정렬할 원소들의 개수를 카운팅하는 작업을 하여 선형 시간에 정렬하는 알고리즘
  - `counting 배열`에 , 정렬할 원소들의 출현 횟수를 저장한다. (index는 원소의 값)
  - `counting 배열`을 누적 counting로 값을 변경
  - `data`의 원소의 값을 뒤에서부터 가져와 `counting 배열`의 값을 인덱스로 하여 새로운 빈 배열 `result`에 값을 넣는다.  
    또한 `counting 배열 `의 값에서 -1을 해준다.
- 별도의 저장공간을 활용한다. (단, 최대치가 너무 크면 비효율적이다.)
- 정수나 정수로 표현할 수 있는 자료에 대해서만 적용이 가능하다. 
  - 각 항목의 발생 회수를 기록하기하기 위해, 정수 항목으로 인덱스 되는 카운트들의 배열을 사용하기 때문이다. 
- 시간 복잡도: O(n+k) 
  - n은 배열의 길이, k는 정수의 최대값

<br>

```python
def Counting_Sort(arr, m):    # 매개변수: 미정렬 배열, 최댓값
    counting = [0]*(m+1)      # 빈 counting 배열 생성
    result = [0]*len(arr)     # 정렬할 빈 배열
    
    # 원소들의 출현 횟수를 저장
    for i in range(0, len(arr)):
        counting[i] += 1   
    
    # 누적합으로 변경
    for i in range(1, m+1):
        counting[i] += counting[i-1]
        
    # 정렬!
    for i in range(len(arr)-1, -1, -1): 
        counting[arr[i]] -= 1
        result[counting[arr[i]]] = arr[i]
```

<br>

### 3) 선택 정렬

- 정렬되지 않은 데이터 중 가장 작은 값의 원소부터 차례대로 선택하여 위치를 교환한다. (오름차순일 때)
  - 앞에서부터 뒤로 정렬을 진행한다. 
  - 주어진 배열 중에서 최솟값을 찾는다.
  - 그 값을 맨 앞에 위치한 값과 교환한다.
  - 맨 처음 위치를 제외한 나머지 배열을 대상으로 위의 과정을 반복. ( 정렬을 위해 최솟값을 찾는 구간을 줄인다. )

- 시간 복잡도: O(n**2)

<br>

```python
# 오름차순
def SelectionSort(arr):                      # 매개변수: 미정렬 배열
    N = len(arr)                 
    for i in range(0, N-1):                  # 미정렬원소가 하나 남은 상황에서 마지막 원소가 가장 큰 값이기때문에 N-1만큼 순회
        min_index = i                        # 최솟값을 옮길 인덱스
        for j in ragne(i+1, N):              # 최솟값 찾을 범위 
           if arr[min_index] > arr[j]:
            min_index = j
        arr[i], arr[min_index] = arr[min_index], arr[i]  #최솟값을 비교 구간 맨 앞으로 이동!
```

<br>

### 4) 셀렉션 알고리즘

> 자료들 중 k번째로 크거나 작은 원소를 찾는 방법이다.

- 최솟값, 최댓값, 중간값 등을 찾을 때 사용하기도 한다. 

- 선택 과정
  - 정렬 알고리즘을 이용하여 자료 정렬하기
  - 원하는 순서에 있는 원소 가져오기
- 시간 복잡도: O(kn)

<br>

```python
def selectionAlgorithm(arr, k):           # 매개변수: 미정렬 배열, 찾는 순서
    N = len(arr)
    for i in range(0, k):                 # 인덱스가 0부터 시작이기 때문에 정렬 후 찾아야 하는 인덱스는 k-1이다. 
        min_index = i                     
        for j in range(i+1, N):
            if arr[min_index] > arr[j]:
                min_index = j
        arr[i],arr[min_index] =  arr[min_index], arr[i]
    return arr[k-1]
```

<br>

## 2. 2차원 배열

```python
2차원 배열 = [[0,0,0,0], [0,0,0,0], [0,0,0,0], [0,0,0,0]]
2차원 배열 = [list([0]*4) for _ in range(4)]
```

| **0** | **0** | **0** | **0** |
| :---: | :---: | :---: | :---: |
| **0** | **0** | **0** | **0** |
| **0** | **0** | **0** | **0** |
| **0** | **0** | **0** | **0** |

- 세로 길이(행의 개수) => row
- 가로 길이(열의 개수) => columm

<br>

### 1) 배열 순회

> n x m 배열의 원소를 조사하는 방법

<br>

#### (1) 행 우선 순회

```python
for i in ragne(n):        # row (행)
    for j in range(m):    # column (열)
        arr[i][j] 
```

<br>

#### (2) 열 우선 순회

```python
for j in ragne(m):        # column (열)
    for i in range(n):    # row (행)
        arr[i][j]
```

<br>

#### (3) 지그재그 순회

```python
for i in range(n):
    for j in range(m):
        arr[i][j + (m-1-2j)*(i%2)]
```

- **짝수 행  (i % 2 == 0) **
  - **j : 0 -> M-1**
  - `arr[i][j]`
- **홀수 행  (i % 2 == 1) **
  - **j : M-1 -> 0** 
  - `arr[i][M-1-j]`

<br>

### 2) 델타

|                      | (i : -1,  j : +0 )  |                      |
| :------------------: | :-----------------: | :------------------: |
| **(i : +0, j : -1)** |   **arry[i, j]**    | **(i : +0, j : +1)** |
|                      | **(i : +1, j : 0)** |                      |

- 한 좌표에서 4방향의 인접 요소를 탐색하는 방법이다.
- 가장자리 원소들은 네 방향에 원소가 존재하지 않기 때문에 이를 고려해야 한다. 

```python
for di, dj in [(0,1), (1, 0), (0, -1), (-1, 0)]:  # 우하좌상의 순서
    ni = i + di
    nj = j + dj
    if 0<= ni <N and 0 <= nj < M:                 # 2차원 배열 범위 
        arr[ni][nj]
```

<br>

### 3) 전치 행렬

> 행과 열의 값이 반대인 행렬

| **1** | **2** | **3** |
| :---: | :---: | :---: |
| **4** | **5** | **6** |
| **7** | **8** | **9** |

- 행과열의 값을 반대로 변경! 
  - **2 (0, 1) ↔ 4 (1,0)** 
  - **3 (2, 0) ↔ 7 (0, 2)**
  - **6 (1, 2) ↔ 8 (2, 1)** 
- 행과 열의 번호가 같으면  변화가 없다. 

| **1** | **4** | **7** |
| :---: | :---: | :---: |
| **2** | **5** | **8** |
| **3** | **6** | **9** |

<br>

```python
for i in range(n):       # 행 (row)
    for j in range(m):   # 열 (column)
        if i < j :
            arr[i][j], arr[j][i] = arr[j][i], arr[i][j]
```

<br>

## 3. 검색

> 저장되어 있는 자로 즁에서 원하는 데이터를 검색(탐색)하는 과정

<br>

### 1) 선형 검색 

- 가장 간단하고 직관적인 알고리즘
- 시작부터 끝까지 내가 찾는 데이터가 맞는지 확인하는 방법이다.
  - 첫번째 인덱스부터 목표 데이터를 찾을 때 까지 값을 하나하나 확인한다. 
  - 검색대상을 찾게되면 종료한다.
  - 혹은 데이터를 다 검색했으면 종료한다. 
- 검색 대상이 너무 많을 경우 비효율적이다.
- 시간 복잡도: O(n)

​	<br>

```python
# 정렬되어 있지 않은 경우
def sequentialSearch(arr, find):     # 매개변수: 정렬 되지 않은 배열, 찾아야 하는 값
    N = len(arr)
    i = 0
    while i < N and arr[i] !== find:
        i += 1
```

<br>

```python
# 오름차순으로 정렬되어 있는 경우
def sequentialSearch(arr, find):    # 매개변수: 정렬된 배열, 찾아야 하는 값
    N = len(arr)
    i = 0 
    while i < N and arr[i] < find:
        i += 1
    if a[i] == find:
        return i 
```

<br>

### 2) 이진 검색

> 자료의 가운데에 있는 항목의 값과 찾아야 하는 값과 비교한다.
> 그 후,  다음 검색의 위치를 찾고 다시 중간값과 비교 하는 과정을 반복한다. 

- 반씩 나누어서 검색한다. 
  - 배열의 중간 값과 찾는 값을 비교
  - 만약 찾는 값이 중간 값보다 작으면, 자료의 왼쪽 반에 대해서 다시 검색을 진행한다. 
  - 만약 찾는 값이 중간 값보다 크면, 자료의 오른쪽 반에 대해서 다시 검색을 진행한다.
  - 위의 과정을 반복한다. 
- 자료가 정렬되어 있는 상태여야 한다. 

<br>

```python
def binarySearch(arr, find):         # 매개변수: 정렬된 배열, 찾아야 하는 값
    N = len(arr)
    start = 0                        # 검색을 진행하는 시작 인덱스 
    end = N-1                        # 검색을 진행하는 마지막 인덱스
    while start <= end:
        middle = (start+end)//2      # 중간 값
        if arr[middle] == find:      # 검색!
            return True
        elif arr[middle] > find:     # 중간 값 > 찾아야 하는 값
            end = middle -1
        elif arr[middle] < find:     # 중간 값 < 찾야아 하는 값
            start = middle +1
    return False                     # 실패!
```

<br>
