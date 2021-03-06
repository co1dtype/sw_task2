# 과제#2

> 리눅스 명령어와 vim 에디터에서 매크로 관련하여 조사

- 이 저장소는 리눅스 명령어( top, ps, jobs, kill)와 vim 에디터에서 매크로(q , @) 사용방법에 대해 설명합니다.
- `README.md` 에서 사용되는 이미지는 해당 저장소의 issues에 저장해두었습니다.
- 무작정 명령어에 대한 많은 설명을 열거하는 것보다 자주 쓸만하다고 판단 되는 것만 정리하였습니다.

**Version**

- Linux: Ubuntu 20.04.4 LTS ([WSL](https://docs.microsoft.com/ko-kr/windows/wsl/install))
- Vim:  8.1.3741

------

# 리눅스 명령어

리눅스 명령어( top, ps, jobs, kill)에 대해 설명합니다.  

## top

![image](https://user-images.githubusercontent.com/76248669/172048035-fd1538ee-64e9-43e3-a6c8-05182da1a070.png)

`top` 명령어는 현재 OS의 상태를 나타내주는 명령어이다. 

기본적으로 3초마다 업데이트를 하며 여러가지 정보를 알려준다. 

윗줄부터 한줄씩 해석해보자.    

![image](https://user-images.githubusercontent.com/76248669/172055030-51f87bb3-cc3f-4638-a5dd-859f53002a0c.png)


왼쪽부터 시스템 현재 시간, 현재 접속중인 유저 세션 수를 나타낸다.

load average는 **CPU Load의 이동 평균를 표시**한다. 왼쪽부터 1분, 5분, 15분에 대한 평균값이다. 

![image](https://user-images.githubusercontent.com/76248669/172048477-eac06b3b-c47b-4750-9881-7cd52efd2bc9.png)

Tasks는 프로세스 개수를 의미하며 이 줄에서는 현재 프로세스의 상태를 나타내어 준다.  

크게 4가지 상태를 보여주는데 실행중, 대기, 중지, 좀비상태인 프로세스의 개수를 보여준다.  

좀비상태는 부모 노드가 먼저 종료될 때 다른 자식 노드로 닿을 수 없는 경우를 의미한다. process는 트리구조를 형성하는 데 자료구조상 부모노드가 메모리 반납이나 종료한다면 자식 노드의 주소를 전달할 수 없게 되므로 이런 현상이 일어난다.  

![image](https://user-images.githubusercontent.com/76248669/172048865-cc73c48b-7016-4022-a533-e3870332479a.png)

이 부분은 CPU 사용량을 볼 수 있는 부분이다.  

요소에 대한 설명은 다음과 같다. 

- us : 프로세스의 유저 영역에서의 CPU 사용률
- sy : 프로세스의 커널 영역에서의 CPU 사용률
- ni : 프로세스의 우선순위(priority) 설정에 사용하는 CPU 사용률
- id : 사용하고 있지 않는 비율
- wa : IO가 완료될때까지 기다리고 있는 CPU 비율
- hi : 하드웨어 인터럽트에 사용되는 CPU 사용률
- si : 소프트웨어 인터럽트에 사용되는 CPU 사용률
- st : CPU를 VM에서 사용하여 대기하는 CPU 비율

![image](https://user-images.githubusercontent.com/76248669/172048838-182c323c-60ae-4593-bb84-b8caacc380f5.png)

이 부분은 메모리 사용량을 볼 수 있는 부분이다. 

왼족부터 총 메모리 양, 사용가능한 메모리 양, 사용중인 메모리 양을 의미한다.  

![image](https://user-images.githubusercontent.com/76248669/172048821-8e68c5e4-29bb-49a3-98a1-04093bf65d94.png)

다음은 디테일한 요소를 출력해주는 구간이다.  

요소에 대한 설명은 다음과 같다.  

```
PID : 프로세스 ID (PID)
USER : 프로세스를 실행시킨 사용자 ID
PRI : 프로세스의 우선순위 (priority)
NI : NICE 값. 일의 nice value값이다. 마이너스를 가지는 nice value는 우선순위가 높음.
VIRT : 가상 메모리의 사용량(SWAP+RES)
RES : 현재 페이지가 상주하고 있는 크기(Resident Size)
SHR : 분할된 페이지, 프로세스에 의해 사용된 메모리를 나눈 메모리의 총합.
S : 프로세스의 상태 [ S(sleeping), R(running), W(swapped out process), Z(zombies) ]
%CPU : 프로세스가 사용하는 CPU의 사용율
%MEM : 프로세스가 사용하는 메모리의 사용율
COMMAND : 실행된 명령어
```

## ps

![image](https://user-images.githubusercontent.com/76248669/172049143-68a027ad-0f36-4c0e-85b0-bd2cfc523074.png)

`ps `명령어는 현재 실행중인 프로세스 목록과 상태를 보여준다. 

줄 수 있는 옵션은 다음과 같다. 

| 옵션  |                             내용                             |
| :---: | :----------------------------------------------------------: |
|  -A   |                  모든 프로세스를 출력한다.                   |
|  -a   | 세션 리더(일반적으로 로그인 셸)을 제외하고 데몬 프로세스처럼 터미널에 종속되지 않은 모든 프로세스를 출력한다. |
|  -e   |     커널 프로세스를 제외한 모든 프로세스를 출력해 준다.      |
|  -f   |       풀 포맷으로 보여준다 (Generates a full listing)        |
| -o 값 | 유닉스 스타일로 출력해주는 옵션으로 UID, PID, PPID등이 함께 표시된다. |
|  -M   | 출력 포맷을 지정하는 옵션으로 값으로는 pid, tty, time, cmd 등을 지정할 수 있다. |
|  -m   |       프로세스들 뿐만 아니라 커널 스레드들도 보여준다.       |
|  -p   |               특정 PID를 지정할 때 사용합니다                |
|  -r   |             현재 실행 중인 프로세서를 보여준다.              |
|  -u   | 특정 사용자의 프로세스 정보를 확인할 때 사용한다. 사용자를 지정하지 않으면 현재 사용자를 기준으로 정보를 출력한다. |
|  -x   | 로그인 상태에 있는 동안 아직 완료되지 않은 프로세서들을 보여준다. 유닉스 시스템은 사용자가 로그아웃 한 후에도 임의의 프로세서가 게속 동작하게 할 수 있다. 그러면 그 프로세서는 자신을 실행시킨 셸이 없이도 계속 자신의 일을 수행하는데 이러한 프로세스는 일반적인 ps 명령으로 확인할 수 잆다. 이 때 -x 옵션을 사용하면 자신의 터미널이 없는 프로세서들을 확인할 수 있다. |

![image](https://user-images.githubusercontent.com/76248669/172050074-53717e92-2d5e-414e-bbdc-89079589b455.png)

개인적으로 `-e`, `-f` 만 쓰는 것 같다.  이것만 알아도 쓰는 데 거의 지장은 없었다. 

`ps -ef` 을 주면 다음과 같이 나오게 된다.  

요소들의 의미는 다음과 같다. 

|  항목   |                           의미                            |
| :-----: | :-------------------------------------------------------: |
|   UID   | SYSTEM V계열에서 나타나는 항목으로 프로세스 소유자의 이름 |
|   PID   |                    프로세스의 식별변호                    |
|  PPID   |                     부모 프로세스 ID                      |
|  STIME  |             프로세스가 시작된 시간 혹은 날짜              |
|   TTY   |                 프로세스와 연결된 터미널                  |
|  TIME   |                     총 CPU 사용 시간                      |
| COMMAND |                  프로세스의 실행 명령행                   |

## jobs, kill

`jobs`는 백그라운드로 실행되는 작업목록을 보여주는 명령어이다.  

딥러닝을 진행할 때 강제로 코드를 중지하면 메모리에 그대로 남게 되는 데 이때 메모리를 초기화하는 방법을 실습하면서 명령어를 설명하겠다. 

![image](https://user-images.githubusercontent.com/76248669/172051768-2a6af090-6221-4758-a36d-1e700f6796b5.png)

상태는 대표적으로 Running(작업중), Done(작업 완료), stopped(일시 중지)가 있다. 

줄 수 있는 옵션은 대표적으로 다음과 같다.

|  옵션   |                    설명                    |
| :-----: | :----------------------------------------: |
|   -l    |  프로세스 그룹 ID를 state 필드 앞에 출력   |
|   -n    | 프로세스 그룹 중에 대표 프로세스 ID를 출력 |
|   -p    |     각 프로세스 ID에 대해 한 행씩 출력     |
| command |            지정한 명령어를 실행            |

`kill` 명령어는 프로세스에 시그널을 보내는 명령어이다.  

강제종료하는 명령어가 아니라 종료 시그널을 보내 종료시키는 명령어이다. (물론 강제 종료 시그널인 `SIGKILL` 이 있긴 함.)

![image](https://user-images.githubusercontent.com/76248669/172052012-3b1458af-a64a-4b77-bfa3-701d57290505.png)

`kill -l` 을 입력하면 줄 수 있는 종료 시그널이 출력된다. 

kill 명령어는 다음과 같이 사용할 수 있다.

`kill -(보낼 시그널) PID` 

이 정도만 알면 그래픽카드 메모리를 초기화 시킬 수 있다. 실습해보자. 

## 응용 (GPU 초기화)

![image](https://user-images.githubusercontent.com/76248669/172052263-1ff211c2-5bcb-4392-b140-c4b30d718de6.png)

다음은 터미널에서 `nvidia-smi` 옵션을 줘 현재 사용하고 있는 GPU 상태를 출력하였다. 

내가 사용하고 있는 GPU는 4번이며 딥러닝 학습 중 과적합이 된 것같아 학습을 강제중지 시켰다.  

학습을 강제중지시켜서 메모리에 값이 그대로 남아있게 되었고 이 문제를 해결해보자!

1. `ps aux | grep python` 를 입력하여 내가 지금 사용하고 있는 python 파일의 pid를 찾는다.

   ![image](https://user-images.githubusercontent.com/76248669/172052512-44ed0fb2-0eef-4572-a6db-28c859a3f374.png)

   이 중 내가 사용하고 있는 파이썬 파일은

   ![image](https://user-images.githubusercontent.com/76248669/172052593-d601cb63-d271-4801-a890-4f374e19e46f.png)

   드래그 한 부분이 PID 번호이다. 

2. `sudo kill -9 44581`를 입력하여 명령어를 내린다. 

3.  `nvidia-smi`를 입력하여 GPU 상태를 확인한다. 

   ![image](https://user-images.githubusercontent.com/76248669/172052673-32de9105-5f81-4551-be17-bb7adf1c35f0.png)

4번 GPU의 메모리가 깔끔하게 초기화가 성공했음을 알 수 있다. 

# VIM

vim 에디터에서 매크로(q , @) 사용방법에 대해 설명합니다.

## q , @

시용법은 매우 간단하다.

> q[매크로이름] -> 입력 -> q
>
> @[매크로이름]
>
> (반복횟수숫자)@[매크로이름]

q로 만들어서 @로 실행하는 것이다.

응용파트에서 자세히 사용하는 방법을 적어보겠다.

## VimGolf로 매크로 실습하기 

![image](https://user-images.githubusercontent.com/76248669/172054197-ed2c6904-b84a-413c-870e-967a5934e265.png)

다음과 같은 파일에 주석문을 2줄 써주면 되는 간단한 문제이다.

코드에 주석을 넣어주는 행동이 겹치기 때문에 매크로를 사용해주면 간편하게 처리할 수 있다. 

![image](https://user-images.githubusercontent.com/76248669/172054443-da8f6c3f-eba9-4e89-ad1a-8b897d988f1f.png)

`qa`를 입력하여 a라는 이름을 가진 매크로를 만들었다. 왼쪽 하단을 보면 `recording @a` 라고 표시되고 있는 데 이렇게 되면 입력되는 모든 키를 녹화하게 된다.  중립모드에 진입하여 `q`를 입력해주면 녹화가 종료된다. 

`@a`를 입력하면 a라는 매크로를 실행하게 된다. 





------

참고

https://sabarada.tistory.com/146
https://ironmask84.tistory.com/355
https://gracefulprograming.tistory.com/127
