---
title: 스택과 DFS
author: 박재경
date: 2022-02-24
categories: [Algorithm, Basics]
tags: [algorithm, stack, dfs]
---



# Stack

> 물건을 쌓아 올리듯 자료를 쌓아 올린 형태의 자료 구조

마지막에 삽입한 자료를 가장 먼저 꺼낸다는 특징이 있다.  (후입선출)

**즉, 순서대로 기록하고 역순으로 읽어와야 하는 작업을 할 때 사용한다.** 



## 1. Stack

### 1) 연산

1. **삽입** `push`

   저장소에 자료를 저장한다. 

2. **삭제** `pop`

   저장소에서 자료를 꺼낸다. 꺼낸 자료는 삽입한 자료의 역순으로 꺼낸다. 

3. **스택이 공백인지 아닌지를 확인하는 연산** `isEmpty`

4. **스택의 top에 있는 item(원소) 을 반환하는 연산 (삭제하지않고 리턴)** `peek`

<br>

### 2) 구현

- 미리 크기가 정해진 배열을 만들어 놓으면 더 빠르다. ( 그렇지 않을 경우, 추가 제거 할때 마다 크기가 계속 바뀐다. ) 
- 하지만 필수는 아니다. 

```python
# ==============================
# 저장소 - 전역변수, 함수 구현
N = 5
S = [0] * N
top = -1
def isEmpty():
    return top == -1

def push(item):
    global top
    if top == N - 1:
        return overflow
    top += 1
    S[top] = item

def pop():       # 데이터를 제거 하지 않는다. 그냥 top의 위치를 옮길 뿐... 
    global top
    if top == -1:
        return empty
    ret = S[top]
    top -= 1
    return ret
#==================================
for i in range(3):
    push(i + 1)

while top != -1:
    print(pop())
# while not isEmpty():
#     print(pop())
# =====================================
```

```python
# 직접 index를 조작해서 push, pop 을 구현
N = 5
S = [0] * N
top = -1

#==================================
for i in range(3):
    top += 1
    S[top] = i + 1

while top != -1:
    print(S[top])
    top -= 1
```

<br>

```python
# List의 append(), pop() 활용 
S = []

#==================================
for i in range(3):
    S.append(i + 1)

while S:
    print(S.pop())
```

<br>

## 2. Function call

:pencil2: 참고자료: https://wikidocs.net/3106

- 가장 마지막에 호출된 함수가 가장 먼저 실행을 완료하고 복귀하는 후입 선출 구조이므로, 후입선출 구조의 스택을 이용하여 수행 순서를 관리한다. 
- 함수 호출이 발생하면, 호출한 함수 수행에 필요한 지역변수, 매개변수 및 수행 후 복귀할 주소 등의 정보를 스택 프레임에 저장하여 시스템 스택에 삽입한다.
- 함수의 실행이 끝나면 시스템 스택의 top 원소 (스택 프레임)를 삭제(pop)하면서 프렘임에 저장되어 있던 복귀 주소를 확인하고 복귀한다.
- 함수 호출과 복귀에 따라 이 과정을 반복하여 전체 프로그램 수행이 종료되면 시스템 스택은 공백 스택이 된다. 

<br>

## 3. Stack활용: 깊이 우선 탐색 (Depth First Search, DFS)

> 비선형 자료(그래프)로 표현된 모든 자료를 빠짐 없이 검색하는 것.

- 실세계의 모습을 추상화하는 도구로 많이 사용한다.
- 그래프는 정점(vertex, V)들과 간선(edge, E)들의 집합이다.
- 정점은 개체(사물, 사람, 개념...)에 대응하고, 간선은 개체들 사이의 관계를 의미한다.
  - 친구관계
- 동동한 관계(무방향) / 의존적 관계(방향)
- 그래프를 저장하는 것은 간선들의 정보를 저장하는 것이다.

  - 각 정점마다 인접정점들의 정보를 저장

  - 간선 하나를 표현하려면 두 정점의 정보가 필요하다.
- 그래프 탐색을 포함해서 대부분의 그래프 알고리즘의 기본 작업

  - 특정 정점의 인접정점들을 찾는 작업

<br>

### 1) 원리

|                             DFS                              |
| :----------------------------------------------------------: |
| 한 방향으로 갈 수 있는 경로가 있는 곳까지 깊이 탐색해 가다가 더 이상 갈 곳이 없게 되면, 가장 마지막에 만났던 갈림길 간선이 있는 정점으로 되돌아와서 다른 방향의 정점으로 탐색을 계속 반복하여 결국 모든 정점을 반복하는 순회방법 |
|           **재귀로 구현 혹은 반복으로 구현(스택)**           |

- `스택`을 활용하는 이유
  - 되돌아가면서 역순으로 갈림길이 있는지 확인하기 때문이다. 
- 알고리즘
  1. 시작 정점 v를 결정하여 방문한다.
  2. 정점 v에 인접한 정점 중에서 방문하지 않은 정점 w가 있으면, 정점 v를 스택에 push하고 정점 w를 방문한다. 그리고 w로 v로 하여 다시 2를 반복한다.
     -  방문하지 않은 정점이 없으면, 탐색의 방향을 바꾸기 위해서 스택을 pop하여 받은 가장 마지막 방문 정점을 v로 하여 다시 2를 반복한다. 
  3. 스택이 공백이 될 때 까지 2를 반복한다.

<br>

### 2) 구현

> 다음은 연결되어 있는 두 개의 정점 사이의 간선을 순서대로 나열 해 놓은 것이다. 
>
> 모든 정점을 깊이 우선 탐색하여 화면에 깊이 우선 탐색 경로를 출력하시오. 시작 정점은 1로 시작하시오. 
>
> 정점 개수: 7 간선: 8
> 1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7

#### (1) 인접 2차 배열 혹은 리스트 만들기

그래프를 저장하는 방법

- V: 정점들의 집합, |V|: 정점수, E: 간선들의 집합, |E|:간선수
- 인접 행렬 생성: |V| x |V| 크기의  2차원 배열에 인접한 정점을 기록
- 혹은 인접 리스트 생성: 인접한 정점을 기록 

```python
# 입력
V, E = map(int, input().split())      # V:정점 수 E:간선 수 
arr = list(map(int, input().split())) 

# 배열
adj = [[0]*(V+1)for _ in range(V+1)]  # 인덱스가 0부터 이기에 V+1
for i in range(E):
    n1, n2 = arr[i*2], arr[i*2+1]
    adj[n1][n2] = 1                   # n1과 n2는 인접하다. 
    adj[n2][n1] = 1                   # 방향 표시가 없는 경우


# 리스트
adjList = [[] for _ in range(V+1)]
for i in range(E):
    n1, n2 = arr[i*2], arr[i*2+1]

    adjList[n1].append(n2)          # n1과 n2는 인접하다. 
    adjList[n2].append(n1)          # 방향 표시가 없는 경우 

print(adj)                          # 2차원 배열로 표시 
print(adjList)                      # 리스트로 표시 
```

- adj 

  |      | [0]  | [1]  | [2]  | [3]  | [4]  | [5]  | [6]  | [7]  |
  | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
  | [0]  | 0    | 0    | 0    | 0    | 0    | 0    | 0    | 0    |
  | [1]  | 0    | 0    | 1    | 1    | 0    | 0    | 0    | 0    |
  | [2]  | 0    | 1    | 0    | 0    | 1    | 1    | 0    | 0    |
  | [3]  | 0    | 1    | 0    | 0    | 0    | 0    | 0    | 1    |
  | [4]  | 0    | 0    | 1    | 0    | 0    | 0    | 1    | 0    |
  | [5]  | 0    | 0    | 1    | 0    | 0    | 0    | 1    | 0    |
  | [6]  | 0    | 0    | 0    | 0    | 1    | 1    | 0    | 1    |
  | [7]  | 0    | 0    | 0    | 1    | 0    | 0    | 1    | 0    |

<br>

#### (2) 스택을 활용하여 구현

```python
visited = [False] * (N+1)   # 초기화 
stack = []          # 초기화
```
- **visited배열**
	
	| [0]  |  [1]  |  [2]  |  [3]  |  [4]  |  [5]  |  [6]  |  [7]  |
	| :--: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
	|      |   1   |   2   |   3   |   4   |   5   |   6   |   7   |
	|      | False | False | False | False | False | False | False |

```python
def dfs(graph, v):  # graph: 그래프  # v: 시작 정점
    # 초기화 (기본값으로 시작 정점의 값을 넣는다.)
    visited = [False] * (V + 1)  # 인접정점의 방문여부를 확인하기 위한 리스트
    stack = [v]                  # stack을 위한 리스트
    result = [v]                 # 출력을 위한 리스트

    # 시작 정점
    visited[v] = True

    while stack:                 # 스택이 비워지면 끝난다!
        v = stack.pop()
        for i in graph[v]:
            if visited[i] == False:  # 인접 정점이고 방문하지 않았다면
                visited[i] = True    # 방문!
                stack.append(i)
                result.append(i)                                    
                
    return result

print(*dfs(adj, 1))      # 1 2 3 7 6 4 5
```

<br>

### 3) 적용

> - 유향인지 무향인지 확인한다.
>
> - 처음에 정점수(V)와 간선수(E)가 주어진다.
>
> - 이후에 간선수 만큼의 정보가 한 줄 또는 E 개줄로 주어진다.
>
> - 사용되는 정점의 번호는 보통 1번 부터 V 까지 사용한다.
>
>   - 문제에 따라 0번 정점을 포함하는 경우도 있음

```plaintext
7 8      # 정점수 간선수
1 2
1 3
2 4
2 5
4 6
5 6
6 7
3 7
-------------------------------
7 8      # 정점수 간선수
1 2 1 3 2 4 2 5 4 6 5 6 6 7 3 7
```

- **DFS 재귀**

```python
V, E = map(int, input().split()) # 정점수, 간선수

G = [[] for _ in range(V + 1)]

# 간선수 만큼 입력
for _ in range(E):
    u, v = map(int, input().split())
    G[u].append(v)
    G[v].append(u)

visited = [0] * (V + 1) # 정점수 만큼 방문정보 저장하는 배열
# DFS 재귀
def DFS(v): # v = 현재 방문하는 정점 번호
    # 방문 표시하고
    visited[v] = 1; print(v, end=' ')
    # v의 인접 정점을 찾는다.
    for w in G[v]:
        # 방문정보를 체크하고
        if not visited[w]:
            DFS(w)

DFS(1)
```

<br>

#### 그래프경로 문제

- **반환값 이용해서 탐색 중단하기**

```python
def DFS(v):  # 현재 방문하는 정점 번호
    visited[v] = 1;
    if v == e:
        return 1

    for w in G[v]:
        if not visited[w]:
            if DFS(w):
                return 1
    return 0
```

- **반환값 모아서 처리하기**

```python
def DFS(v):  # 현재 방문하는 정점 번호
    visited[v] = 1;
    if v == e:
        return 1

    ret = 0
    for w in G[v]:
        if not visited[w]:
            ret += DFS(w)   

    return ret
```

<br>

## 4. Stack활용: 계산기

> 문자열로 된 계산식이 주어질 때, 스택을 이용하여 이 계산식의 값을 계산할 수 있다. 

### 1) 원리

- `중위 표기법`의 수식을 `후위 표기법`으로 변경한다. (스택이용)
  - 중위 표기법 (infix notation): 연산자를 피연산자의 가운데 표기하는 방법
  - 후위 표기법(postfix notation): 연산자를 피연산자 뒤에 표기하는 방법
- `후위 표기법`의 수식을 스택을 이용하여 계산한다. 

<br>

### 2) 구현

> (6+5*(2-8)/2)는 문자열로 된 계산식 이다. 이 계산식을 후위 표기식으로 바꾸어 계산하는 프로그램을 작성하시오.

<br>

#### (1) 중위 표기식의 후위표기식 변환 

1. 입력 받은 중위 표기식에서 토근을 읽는다. 
2. 토근이 피연산자이면 토큰을 출력한다. 
3. 토큰이 연산자(괄호포함)일 때, 이 토큰이 스택의 top에 저장되어 있는 연산자보다 우선순위가 높으면 스택에 push하고, 그렇지 않다면 스택 top의 연산자의 우선순위가 토큰의 우선순위보다 작을 때까지 스택에서 pop한 후 토큰의 연산자를 push한다. 만약 top에 연산자가 없다면 push한다.
4. 토큰이 ')'이면 스택 top에 '('가 올 때까지 스택에 pop연산을 수행하고 pop한 연산자를 출력한다. 왼쪽 괄호를 만나면 pop만 하고 출력하지는 않는다. 
5. 중위 표기식에 더 읽을 것이 없다면 중지하고, 더 읽을 것이 있다면 1부터 다시 반복한다.
6. 스택에 남아 있는 연산자를 모두 pop하여 출력한다. 

| 토큰 | isp(in-stack priority) | icp(in-coming priority) |
| ---- | ---------------------- | ----------------------- |
| )    | -                      | -                       |
| *, / | 2                      | 2                       |
| +, - | 1                      | 1                       |
| (    | 0                      | 3                       |

```python
word = '(6+5*(2-8)/2)'

isp = {'*':2, '/': 2, '+': 1, '-': 1, '(':0}
icp = {'*':2, '/': 2, '+': 1, '-': 1, '(':3}

# 중위 표기식의 후위 표기식 변환
stack = []    # stack계산을 위한 리스트
words = []    # 출력 할 리스트

for token in word:
    # 연산자인 경우
    if token in isp:
        
        # 스택이 비어있다면 추가
        if not stack:
            stack.append(token)
       
    	# toekn을 키로 갖는 icp 딕셔너리의 값 > stack[-1]을 키로 갖는 isp 딕셔너리 값이면, stack에 추가
        elif isp[stack[-1]] < icp[token]:
            stack.append(token)
        
        # stack[-1]을 키로 갖는 isp 딕셔너리 값 >= toekn을 키로 갖는 icp 딕셔너리의 값이면, pop!
        else:
            while icp[token] <= isp[stack[-1]]:
                words.append(stack.pop())
                 if not stack:               # 스택이 비어있다면 반복문을 빠져나가서 그냥 추가한다.
                        break
            stack.append(token)

    # 닫는 괄호인 경우, 여는 괄호를 만나기 전까지 pop!
    elif token == ')':
        while stack[-1] != '(':
            words.append(stack.pop())
        stack.pop()                          # 짝을 이룬 여는 괄호는 버린다.

    # 피연사자인 경우, 출력준비..
    else:
        words.append(token)

# 스택에 남은 연산자 pop!
while stack:
    words.append(stack.pop())

print(*words)     # 6 5 2 8 - * 2 / +
```

<br>

#### (2) 후위 표기법의 수식을 스택을 이용하여 계산

1. 피연산자를 만나면 스택에 push한다.
2. 연산자를 만나면 필요한 만큼의 피연산자를 스택에서 pop하여 연산하고, 연산결과를 다시 스택에 push 한다.
3. 수식이 끝나면, 마지막으로 스택을 pop하여 출력한다. 

```python
# 후위 표기법의 수식을 스택을 이용하여 계산
for token in words:
    # 연산자인 경우,
    if token == '+':
        stack.append(stack[-2]+stack[-1])  
        stack.pop(-2)   # 연산후, 제거 
        stack.pop(-2)   # 연산후, 제거
    elif token == '-':
        stack.append(stack[-2]-stack[-1])
        stack.pop(-2)
        stack.pop(-2)
    elif token == '*':
        stack.append(stack[-2]*stack[-1])
        stack.pop(-2)
        stack.pop(-2)
    elif token == '/':
        stack.append(stack[-2]/stack[-1])
        stack.pop(-2)
        stack.pop(-2)

    # 피 연산자인 경우, 숫자로 변환하여 스택에 추가!
    else:
        stack.append(int(token))

print('%d' % stack[0] )    # -9
```

<br>
