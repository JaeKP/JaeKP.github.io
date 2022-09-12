---
title: 운영체제-프로세스-관리
author: 박재경
date: 2022-09-06
categories: [CS, 운영-체제]
tag: [cs, 운영-체제]
---


# 4장-프로세스-관리

[강의 링크](http://www.kocw.net/home/search/kemView.do?kemId=1046323)

<br>

## 1. 프로세스 관리

### 1) 프로세스 생성

> 부모 프로세스가 자식 프로세스를 생성한다. (시스템 콜을 통해 요청)
{: .prompt-tip }

프로세스의 트리 (계층 구조)가 형성된다.

- 프로세스는 자원을 필요로 한다.
  - 운영체제로부터 받는다.
  - 부모와 공유한다.
- 자원의 공유
  - 부모와 자식이 모든 자원 을 공유하는 모델
  - 일부를 공유하는 모델
  - 전혀 공유하지 않는 모델
- 수행 (Execution)
  - 부모와 자식은 공존하면서 수행되는 모델
  - 자식이 종료(terminate)될 때까지 부모가 기다리는(wait) 모델
- 주소 공간 (Address space)
  - 자식은 부모의 공간을 복사한다 (binary and OS data)
  - 자식은 그 공간에 새로운 프로그램을 올린다.

<br>

**`예시 -  유닉스`**

- `fork()`시스템 콜이 새로운 프로세스를 생성한다.
  - 부모를 그대로 복사한다.
  - 주소 공간을 할당한다.
- `fork()`다음에 이어지는 `exec()` 시스템 콜을 통해 새로운 프로그램을 메모리에 올린다.

<br>

### 2) 프로세스 종료

- 프로세스가 마지막 명령을 수행한 후 운영체제에게 이를 알려준다 `exit 시스템 콜`
  - 자식이 부모에게 output data를 보내게 된다. (via wait)
  - 자식이 부모보다 항상 먼저 종료된다.
  - 프로세스의 각종 자원들이 운영체제에게 반납된다.
- 부모 프로세스가 자식의 수행을 종료시킨다. `aboart 시스템 콜`
  - 자식이 할당 자원의 한계치를 넘었거나 자식에게 할당된 태스크가 더 이상 필요하지 않을 때 종료된다.
  - 부모가 (exit)하면 자동으로 자식도 종료된다.
    - 운영체제는 부모 프로세스가 종료하는 경우 자식이 더 이상 수행되도록 두지 않는다.
    - 단계적인 종료이다.

<br>

## 2. 프로세스와 관련된 시스템 콜

| 프로세스와 관련한 시스템 콜 |                                        |
| --------------------------- | -------------------------------------- |
| `fork()`                    | create a child                         |
| `exec()`                    | overlay new image                      |
| `wait()`                    | sleep until child is done              |
| `exit()`                    | frees all the resources, notify parent |

<br>

### 1) fork()

> 프로세스를 생성할 때 호출하는 시스템 콜

- 새롭게 생성된 자식 프로세스는 새로운 PID를 갖게 되어 호출한 부모 프로세스를 복사한다.
- 자식 프로세스는 부모와 독립되 물리 메모리 공간을 갖게 된다. 

```c
// fork() 시스템 콜
// creates new address space that is a duplicate of the caller.

int main()
{
  int pid;
  pid = fork(); // parent면 양수를 반환, child면 0을 반환한다.
  if (pid==0)
    printf("\n Hello, I am child\n")
  else if (pid > 0)
    printf("\n Hello, I am parent\n")
}
```

<br>

### 2) exec()

> 현재 프로세스 공간을 새로운 프로세스 이미지로 덮어버린다. 
{: .prompt-tip }

```c
// exec() 시스템 콜
// replaces the memory image of the caller with a new program

int main()
{
  int pid;
  pid = form();
  if (pid==0)
  {
    printf("\n Hello, I am child! Now I'll run date\n")
    execl("/bin/date", "/bin/date", (char*)0);
  }
  else if (pid >0)
    printf("\n Hello, I am parent\n")
}
```

<br>

### 3) wait()

> 자식 프로세스가 종료되기 전까지 부모 프로세스가 기다리게 하는 시스템 콜
{: .prompt-tip }

프로세스 A가 `wait()`시스템 콜을 호출하면 

- 커널은 child가 종료될 때까지 프로세스 A를 sleep 시킨다 (block 상태)
- child process가 종료되면 커널은 프로세스 A를 깨운다 (ready 상태)

```c
// 예시

int main()
{
  int pid;
  pid = fork(); 
  if (pid==0)
    printf("\n Hello, I am child\n")
  else if (pid > 0)
    printf("\n Hello, I am parent\n")
    wait() // parent면 wait() 호출
}
```

<br>

### 4) exit()

> 프로세스를 종료시킬 때 호출하는 시스템 콜
{: .prompt-tip }

- 자발적인 종료
  - 마지막 statement 수행 후 `exit()` 시스템 콜을 통해 호출한다.
  - 프로그램에 명시적으로 적어주지 않아도 main 함수가 리턴되는 위치에 컴파일러가 넣어준다.
- 비자발적인 종료
  - 부모 프로세스가 자식 프로세스를 강제 종료한다.
    - 자식프로세스가 한계치를 넘어서는 자원 요청
    - 자식에게 할당된 태스크가 더 이상 필요하지 않음
  - 키보드로 kill, break 등을 친 경우
  - 부모가 종료하는 경우
    - 부모 프로세스가 종료하기 전에 자식들이 먼저 종료된다. 

<br>

## 3. 프로세스 간 협력

### 1) 프로세스 종류

- 독립적 프로세스
  - 프로세스는 각자의 주소 공간을 가지고 수행되므로 원칙적으로 하나의 프로세스는 다른 프로세스의 수행에 영향을 미치지 못한다.
- 협력 프로세스
  - 프로세스 협력 매커니즘을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있다. 

<br>

### 2) 프로세스 간 협력 메커니즘 (IPC: Interprocess Communication)

- 메시지를 전달하는 방법 (message passing): 커널을 통해 메시지를 전달한다. 프로세스 사시의 공유 변수를 일체 사용하지 않고 통신하는 시스템이다.
  - Direct Communication: 통신하려는 프로세스의 이름을 명시적으로 표시한다.
  - Indirect Communication: mailbox (또는 port)를 통해 메시지를 간접적으로 전달한다. 
- 주소 공간을 공유하는 방법 (shared memory) : 서로 다른 프로세스 간에도 일부 주소 공간을 공유한다. (커널의 도움을 받아서 공유)
  - 프로세스는 각자 주소 공간이 있어서 원래 서로 접근할 수 없다. 
  - 물리적인 메모리에 매핑할 때 공유하도록 생성한다. 

<br>

**`thread`**

thread는 사실상 하나의 프로세스이므로 프로세스 간 협력으로 보기는 어렵지만 동일한 process를 thread들 간에는 주소 공간을 공유하므로 협력이 가능하다. 

<br>