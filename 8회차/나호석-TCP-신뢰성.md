# TCP 신뢰성

## 전송 계층의 역할

- 상위 계층인 `Application` 계층의 메세지를 목적지로 전달.
- `process`를 `port`번호로 구분해서 전달.
- `Multiplexing`(sender)
  - 여러 소켓에서 내려오는 데이터들을 모으고, 각각 `segment`로 만들어서 전송.
- `Demultiplexing`(reciever)
  - 전달 받은 `segment`들을 올바른 `socket`으로 이동.

## TCP와 UDP의 차이

- 둘 다 모두 `Multiplexing`, `Demultiplexing`는 수행.
- 하지만 `TCP`는 신뢰성을 보장하고 `UDP`는 보장 없이 전송하기만 한다.
- 그러면 `신뢰성`을 어떻게 보장할 수 있을까?
- `TCP`는 전송계층에서의 프로토콜이고 하위 계층과 라우터들을 지나서 통신이 될텐데 어떤 방식으로 신뢰성이 보장이 될까?

## 예상되는 문제점

- 메세지 `에러`.
  - 내가 보냈던 메세지가 변경되어 에러가 생길 수 있음.
- 메세지 `유실`.
  - 내가 보냈던 메세지가 전송 중에 누락이 될 수 있음.
- 위의 2가지 문제를 해결하면 논리적으로 **신뢰성을 보장할 수 있음**.

## Reliable Data Transfer

- 신뢰성 있는 데이터 교환.
- 패킷을 하나씩 보낸다는 단순한 구조 가정.

### 에러

- 에러 점검.
- 피드백.(ACK-정상, NACK-에러발생)
- 재전송.
  - 재전송 했을 경우 패킷이 중복되면? (ACK를 보냈는데 NACK로 변질되서 다시 재전송을 받을 경우)
  - `sequence number` 사용해서 해결.
  - NACK가 필요 없어짐.
- stop and wait (응답 받을 때까지 멈춰서 기다림)

### 유실

- timeout 활용.
  - 시간내에 응답이 없으면 유실됐다고 판단.
- 3 duplicate ACK
  - 중복된 ACK가 3번 발생하면 유실됐다고 판단 후 재전송(timeout 전에)
  - 총 4번의 같은 ACK를 받음 (중복 3번)

### pipeline

- 위와 같은 로직으로 패킷을 하나씩 보내면 시간이 오래걸림.
- 실제 네트워크에서는 `pipeline`으로 한번에 여러개의 패킷을 동시에 보낸다.
- `window size` 단위로 전송하고 2가지 방식이 있음.
  - `Go-Back-N`: 재전송시 모두 다 재전송.
  - `Selective Repeat`: window size 안에서 유실된 내용만 재전송.
  - [애니메이션 링크](https://www2.tkn.tu-berlin.de/teaching/rn/animations/gbn_sr/)

## 정리

- TCP 프로토콜은 신뢰성을 보장한다.
- `Reliable Data Transfer`는 신뢰성 있는 데이터 교환으로 `에러 문제`와 `유실 문제`를 해결한다.
- 에러 문제는 3가지 단계로 해결한다.
  1. 에러 점검.
  2. 결과 피드백.
  3. 문제가 있을 경우에 재전송.
- 유실 문제는 `timeout`이 지나면 패킷이 유실 됐다고 가정하고 다시 재전송한다.
- 효율을 위해 `pipeline` 형식으로 패킷을 한번에 여러개(`window size`)를 보내고 `Go-Back-N`, `Selective Repeat` 방식이 있다.

## 출처

- [컴퓨터네트워크](http://www.kocw.net/home/cview.do?cid=6166c077e545b736)
- [신뢰적인 전송과 TCP](https://frog-in-well.tistory.com/48)
