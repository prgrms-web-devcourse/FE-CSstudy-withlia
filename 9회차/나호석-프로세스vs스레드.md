# 프로세스vs스레드

## 용어 정리

### 작업(job)

- 실행 할 프로그램 + 데이터.
- 컴퓨터 시스템에 실행 요청 전의 상태.

### 프로세스(process)

- 실행 중인 프로그램.
- 실행을 위해 시스템(커널)에 등록된 작업.
- 자원을 할당 받고 제어를 통해 작업(목적)을 완수
  - 제어 부분 = 스레드

![프로세스_제어부분](https://user-images.githubusercontent.com/16220817/193419183-61964a5d-0829-44d1-a1c7-203fa0711a9e.png)

### 자원(resouces)

- 커널의 관리하에 프로세스에게 할당/반납 되는 수동적 개체(passive entity).
- 분류
  - H/W resouce
    - Processor, memory, disk, monitor, keyboard, etc
  - S/W resouce
    - Message, signal, files, install SWs, etc

### 프로세스 제어 블록(Process Control Block, PCB)

- 각 프로세스들에 대한 정보를 관리.
- OS가 프로세스 관리에 필요한 정보 저장.
- 프로세스 생성 시, 생성 됨.

### 스레드(thread)

- 프로세스가 할당 받은 자원을 이용하는 실행의 단위.
- Light Weight Process(LWP)
- 프로세서(CPU) 활용의 기본 단위

## 프로세스 동작

- 프로세스가 프로세서(CPU)와 모든 자원을 할당 받으면 작업을 수행.
- 인터럽트가 발생하거나 다른 자원이 필요하면 프로세서(CPU)를 반납하게 된다.
- 현재까지 작업한 정보(context)를 `PCB`에 기록한다.
- 작업이 끝난 후 다시 프로세서(CPU)를 할당 받으면 `context`를 불러와서 작업을 이어서 수행한다.
- **이런 `Context Switching(문맥 교환)`은 커널의 개입으로 이루어지고 성능에 큰 영향을 미친다.**
  - 불필요한 `Context Switching`을 줄이기 위해서는? -> 스레드를 사용.

## 스레드

- 프로세스의 제어 부분.
  - 할당 받은 자원을 가지고 작업을 수행하는 부분 -> 프로세스의 흐름의 단위 or 실행의 단위
- 하나의 프로세스에는 여러개의 스레드가 있을 수 있음.
- 프로세스가 할당 받은 자원은 공유해서 사용하고 작업(제어)은 각자 수행.
- 그래서 본인 작업에 관한 정보는 스레드별로 필요하다. (독릭접인 수행을 위해서)
  - ThreadID, Register Set(PC, SP 등), Stack(local data)

![스레드](https://user-images.githubusercontent.com/16220817/193419244-db87b89b-73d9-4649-b9d7-841d8bb7540b.png)

## 프로세스와 스레드의 차이

- 프로세스는 실행 중인 프로그램이고 스레드는 프로세스의 실행 단위다.
- 데이터 공유 측면
  - 프로세스: 프로세스 별로 데이터가 할당되서 다른 프로세스의 데이터를 알 수 없음. IPC 통신을 통해 데이터 공유가 가능.
  - 스레드: 프로세스의 자원은 함께 공유. 각자의 스택을 가지고 작업을 수행.
- 오류로 인해 종료 됐을 때
  - 프로세스: 하나의 프로세스 종료가 다른 프로세스에 영향을 주지 않음.
  - 스레드: 메모리 영역의 내용을 공유하고 있기 때문에 같은 프로세스 내의 다른 스레드들 모두 강제 종료.

## 출처

- [운영체제 강의](https://www.youtube.com/playlist?list=PLBrGAFAIyf5rby7QylRc6JxU5lzQ9c4tN)
- [프로세스와 스레드 차이](https://velog.io/@raejoonee/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80-%EC%8A%A4%EB%A0%88%EB%93%9C%EC%9D%98-%EC%B0%A8%EC%9D%B4)
- [프로세스&스레드](https://gyeong-log.tistory.com/30)
