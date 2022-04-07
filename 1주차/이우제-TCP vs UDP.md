# TCP vs UDP

# 1. 전송 계층 (Transport Layer)

### 먼저 `TCP`,`UDP`에 대해 알기 위해서는 먼저, OSI 7계층 중 4계층에 속하는 `Transport Layer`에 대해서 알아야 합니다.

---

## 1-1. 전송 계층

> **`전송계층(Transport layer)`** 은 서로 다른 **호스트(송·수신자)에서** 실행되는 애플리케이션  **프로세스** 간의 **논리적 통신**을 제공합니다.
> 
<img
  src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4d619c01-a6b7-402c-bfc3-4ec648732327/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220406%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220406T131315Z&X-Amz-Expires=86400&X-Amz-Signature=41e8481865ef1c6d0127b280f8badc81b255fd3614e680b5b5a3e58b799132ad&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject"
  width="300"
  height="300"
/>

**프로세스? 논리적 연결?**  

용어에 대해서 먼저 알고 가야할 것 같습니다.

### 프로세스

> 메모리에서 동작 중인 **프로그램**이라고 생각하면 됩니다.
> 

컴퓨터에서 동작하는 프로그램인 **카카오톡, Slack** 등이 그 예가 될 수 있죠.

### 논리적 통신

> 애플리케이션 관점에서 프로세스들이 동작하는 호스트들이 **직접 연결된 것처럼 보인다는 것**을 의미합니다.
> 

**내 컴퓨터에서 상대방 컴퓨터와 메세지를 주고 받는 것**이 **논리적 통신**의 예에 해당합니다.

반대의 의미로 **물리적** 이라는 말이 있죠. 물리적은 말 그대로 케이블로 연결해 통신하는 것과 같은 것을 의미합니다.

   

---

## 1-2. 포트 번호 (Port Number)

### 네트워크 계층에서의 논리적 통신과의 차이점

- 네트워크 계층 프로토콜은 **서로 다른 호스트에**서의 논리적 통신을 제공
- 전송 계층 프로토콜은 **서로 다른 호스트에서 동작하는 프로세스**들 사이의 논리적 통신을 제공

예시를 들어보겠습니다.

제가 slack을 통해 리아님께 비동기적 커뮤니케이션으로 문의를 남기려고 합니다. 

**네트워크 계층에서는 IP Adress를 통해** 저의 컴퓨터에서 리아님 컴퓨터로 메세지를 보낼 수 있습니다. 

그렇다면 메세지가 도착한 리아님 컴퓨터에서는 **어떤 프로그램(프로세스**)에 해당 메세지를 전송해야하는가?

<img
  src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/05cfa977-c9f7-4d76-981a-ed7d48d49aa1/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220406%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220406T131721Z&X-Amz-Expires=86400&X-Amz-Signature=83cc8598f3a60dee8f3f387ebfdb35f7b09b1f1bbb61699ae860b49dd352f3b4&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject"
  width="500"
  height="350"
/>


이렇게 **올바른 프로세스에**  메세지를 전달하기 위해 **식별자인 포트 번호(Port Number)가 필요합니다.**

**포트 번호**를 통해서 **원하는 프로세스인 Slack** 에 데이터가 전달돼 저의 비동기적 커뮤니케이션을 통한 문의를 할 수 있게 된거죠.

**HTTP는 80번, HTTPS에 443번, 이메일 전송을 하는 SMTP는 25번처럼 규약**처럼 정해져 사용되는 포트 번호들이 있습니다. 

---

## 1-3. 전송 계층의 역할

✨ 논리적 통신**을 통한 데이터의 전달**

✨ **흐름 제어(flow control), 혼잡 제어(congestion control)**을 통해 안전하게 데이터가 전달될 수 있도록 지원

### 이런 전송 계층의 프로토콜 중 가장 잘 알려진 것이 **TCP**와 **UDP입니다.**

이제부터 TCP와 UDP의 특징을 중점으로 알아보겠습니다.

---

# 2. TCP(Transmission Control Protocol)

**TCP는 전송 제어 프로토콜** 입니다.

![TCP-Header](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0dae2b93-226f-4bc5-82a9-a11f288fd551/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220406%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220406T131749Z&X-Amz-Expires=86400&X-Amz-Signature=4d995eb14546ad1892cc3714f081e15b071dfb2609d6c2f4b0f5233277167ad9&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

4계층의 전송의 기본 단위인 세그먼트의 일부분 중 **헤더**라는 것이 있습니다.

**TCP의 헤더**는 이처럼 상당히 **복잡하게** 생겼습니다.

왜 그런지 TCP의 특징부터 알아보겠습니다.

---

## 2-1. TCP의 특징

TCP의 특징은 **친절하다**는 것입니다. **연결이 되고나서 데이터를 보내고 잘 받았는지 물어봅니다.**

그렇기 떄문에 신뢰성이 높겠죠.

✨ **양방향** 통신

✨ **연결지향형** 프로토콜

✨ **흐름제어, 혼잡제어**를 해서 **신뢰성이 높습니다**.

✨ 데이터의 유실 없이 전송하고, 데이터의 전송 순서를 보장

😶 안전하지만 상대적으로 **느립니다.**

따라서 **데이터가 손실 없이 순차적으로 안전하게 전송**돼야 하는 **이메일, 파일 전송** 등에 사용됩니다.

---

## 2-2. TCP를 이용한 통신과정

### 연결 수립 과정 - 3Way Handshake

> TCP를 이용한 데이터 통신을 할 때 프로세스와 프로세스를 연결하기 위해 가장 먼저 수행되는 과정입니다.
> 
1. 클라이언트가 서버에게 요청 패킷을 보내고
2. 서버가 클라이언트 요청을 받아들이는 패킷을 보내고
3. 클라이언트는 이를 최종적으로 수락하는 패킷을 보낸다.

<img
  src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8eca221a-3b20-4e99-9b68-ca79b98d4cf0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220406%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220406T131822Z&X-Amz-Expires=86400&X-Amz-Signature=6f2e8adf8ae219379aed847544117557e942abaad5a3ef18b547f889c15225ff&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject"
  width="500"
  height="400"
/>

이 과정을 통해 클라이언트와 서버는 데이터를 주고 받을 준비가 되었다는 것을 서로에게 알려주고 이후 데이터 전송에 필요한 시퀀스 번호를 알 수 있게 됩니다.

간단히 말하면 **친절하게 연결을 확인하는 과정이 3-way handshake** 것입니다.

이처럼 **연결 해제 과정**은 **4-way handshake로** 이루어집니다.

그만큼 TCP는 **신뢰성을 유지하기 위해** 이러한 **복잡한 통신 과정**을 거치기 때문에 헤더가 상당히 무겁습니다.

---

## 2-3. TCP의 흐름제어

**흐름 제어**는 위와 같이 송신 측과 수신 측의 **버퍼 크기 차이로 인해 생기는** **데이터 처리 속도 차이**를 **해결하기 위한 기법**입니다.

TCP는 데이터를 교환함과 동시에 헤더에 기록된 정보를 이용해 데이터의 **신뢰성 제어와 흐름 제어**를 동시에 진행합니다.

흐름제어 기법에는 **Stop and Wait 프로토콜, Sliding Window** 기법이 있다.

![stop-and-wait](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/babce04c-cdde-48ba-9c36-578f5a0a2fe0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220406%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220406T131949Z&X-Amz-Expires=86400&X-Amz-Signature=4bd0e229f7973d9bb5a01f33fe39635e6fa0a6bf4e79314cbea621bb8c53897a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

 **Stop and Wait 프로토콜**은 전송한 패킷에 대해 확인 응답(ACK)을 받으면 다음 패킷을 전송하는 제어 기법입니다. 

1. 송신자는 패킷을 보내면서 타이머를 동작시킵니다. 
2. 수신자는 패킷을 올바르게 받으면 **즉시 응답(ACK)합니다.**
3. 송신자는 ACK을 받으면 **바로** 패킷을 또 보냅니다**.** 

(송신자가 ACK을 받지 못하고 tiemout이 일어날 시 해당 패킷을 재전송합니다.)

 **Sliding Window**는 **데이터 흐름을 동적으로 조절하는 제어 기법입니다.**

수신 측에서는 자신이 현재 수신 버퍼에서 처리할 수 있는 용량을 헤더에 실어서 보내주는데,

송신 측은 **패킷이 도착하는데 걸린 시간**과 **수신 측의 윈도우 사이즈**를 고려해서 오버플로가 발생하지 않게 **데이터의 양을 적당히 조절**합니다.

---

## 2-4. TCP의 혼잡제어

**혼잡제어**는 특정 순간에 너무 많은 네트워크 요청이 몰리거나, 여러 사용자가 한 네트워크를 동시에 쓰는 경우 **혼잡이 발생하는데 이를** **방지**하거나 **제거**하는 기법입니다.

혼잡제어 기법에는 **AIMD (Additive Increse/Multicative Decrease), Slow Start**가 있습니다.

![AIMD](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/3dfd9164-29d8-4c36-86d4-f4870176dea5/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220406%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220406T132021Z&X-Amz-Expires=86400&X-Amz-Signature=4b273a82f8c4a793b1154dc7f4216a1495fc3a5c5dcf3ded2fed7afc95d0a26e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

**AIMD**는 전송률을 **1씩 증가시키다가** 만약, **전송에 실패하면 전송율을 반으로 줄여 관리**하는 기법입니다.

![Slow-start](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9ea3fd0a-9a93-4ec1-ba11-2b3e6ef42982/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220406%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220406T132053Z&X-Amz-Expires=86400&X-Amz-Signature=27511628c300e376f737641d0f2843f9c221d624895a24e0c1909cd8387f5065&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

**Slow Start는 전송률을 exponential하게(1,2,4,8 ...) 증가**시키다가 **혼잡이 감지되면 전송률 크기를 1로 줄이는 기법**입니다.

이외에도 **Fast Restransmit, Fast Recovery** 등의 기법이 있습니다.

---

# 3. UDP(User Datagram Protocol)

![starcraft](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a1381a35-c7e3-420e-921b-d4c08a317fd7/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220406%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220406T132122Z&X-Amz-Expires=86400&X-Amz-Signature=3fed80aa4c86530ff74102ddea3155bb166a7d5f829a4ad10cdc214705f4f2db&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

**UDP**는 사용자 데이터그램 프로토콜로 스타크래프트 게임을 해보셨던 분이라면 친숙한 이름입니다.

![udp-header](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/82db976d-d065-432e-9508-a53f54a27f2b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220406%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220406T132146Z&X-Amz-Expires=86400&X-Amz-Signature=3c7775db05ad4777f4712da773f1d6693ca3c68c30be73b4d68d2057961c9f37&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

  UDP의 헤더는 TCP와 달리 **간단한 구조**로 전송을 위한 포트번호와 패킷의 길이, 그리고

오류검출을 위해서 사용자가 선택적으로 사용할 수 있는 체크섬정도로만 구성되어있습니다.

이제 **UDP**의 특징에 대해서 알아보겠습니다.

---

## 3-1. UDP의 특징

UDP는 **제대로 연결을 하던말던 데이터를 일단 던진다**고 생각하시면 이해하기 쉽습니다.

상대방의 응답을 확인하지 않고 **무조건 보내거나, 무조건 받는 통신**을 하는 것이죠. 그렇기 때문에 데이터의 유실이 생길 수 있어 신뢰성을 보장할 수 없습니다.

✨ 단방향 통신

✨ 흐름제어, 혼잡제어가 없다 → **전송 속도가 비교적으로 빠르다**,

✨ 송신자 입장에서 편하다. 데이터를 뿌리면 그만이다.

😶**데이터가 정확한지 알 수 없고, 데이터의 순서도 보장할 수 없다 → 신뢰성을 보장할 수 없다**.

그렇기 때문에 신뢰성보다 **속도가 중요한** **DNS,** **라이브 스트리밍 서비스** 등에서 사용됩니다.

---

## 3-2. HTTP/3가 UDP를 선택한 이유

<img
  src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/16c31740-a201-4b8f-a497-584b83242871/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220406%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220406T132214Z&X-Amz-Expires=86400&X-Amz-Signature=0cdcb5ef27bb608c6b7a75c2fb7b494388f95b9e5ad7f9a8fa84021038a6e98e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject"
  width="200"
  height="200"
/>

**HTTP3**는 HTTP의 세 번째 메이저 버전으로 기존의 HTTP/1, HTTP/2와는 다르게 **UDP 기반의 프로토콜인** **QUIC(Quick UDP Internet Connection)를 사용하여 통신한다고 합니다.**

**TCP**는 송수신자가 **서로 신뢰성있는 통신**을 할 수 있도록 몇 가지 방법을 사용해 **레이턴시가** **발생**할 수 밖에 없고, 이 과정은 프로토콜이 생길 때부터 정의된 **표준**이므로 무시할 수가 없습니다.

따라서 레이턴시를 줄이면서 표준을 무시하지 않기 위해서 프로토콜 자체를 손봐야하는데 뜯어고치기에는 **TCP는 너무 오래되고 복잡했습니다.**

아까 전에 봤듯이 UDP의 헤더는 널널한 공간을 가지고 있기에 **커스터마이징이 용이하다는 장점이 있습니다.**

QUIC은 **TCP를 사용하지 않기 때문에** 통신을 시작할 때 번거로운 **3 Way Handshake 과정을 거치지 않아도 됩니다.** 

**첫번째 핸드쉐이크를 거칠 때**, **연결 설정에 필요한 정보와 함께 데이터도 보내버려** 연결을 대략 **TCP보다 3분의 1시간에** 완료할 수 있습니다.

웹 서비스 동향을 알려주는 **w3tech**에서 발표한 내용에 따르면, **QUIC**은  2022년 4월 기준 모든 웹사이트 중 **24.6%가 사용하고 있다고 합니다.**

---

# 4. 요약

![tcp_udp](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/cad9c7cc-7a4d-4dbf-9ddd-62b1c406d648/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220406%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220406T132234Z&X-Amz-Expires=86400&X-Amz-Signature=90240a3b8ae654c7c5222be50a37dfe8d9e78e5eaf377d6cb207460811cf840e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

  이 사진은 TCP와 UDP의 특성을 잘 보여주는 그림이 인상적이어서 가져왔습니다.

**서로 연결이 되어있는지 제대로 확인을 하고 나서 데이터를 전송해 신뢰성이 높은 TCP**

**제대로 연결을 하던말던 데이터를 일단 던져 신뢰성이 보장되지 않는 UDP를** 잘 보여주는 것 같습니다.

### Quiz

<details>
<summary>Netflix는 TCP를 사용할까 UDP를 사용할까?</summary>
<div markdown="1">
   정답:  TCP
    
    이유: TCP가 비디오 품질을 높이고 네트워크 정체 문제를 줄이는데 용이하기 떄문
    
    Netflix와 같은 온라인 스트리밍 서비스는 시청자가 시청하기 전에 미리 가져오고 버퍼링하는 데 중점을 두는데, TCP는 flow control과 congestion control을 할 수 있기 때문
    
    ✨버퍼링 (정보의 송수신을 원활하도록 하기 위해서 일시적으로 저장하여 작업의 처리 속도 차이를 흡수시키는 방법)
</div>
</details>
    

---

## 출처

****[10분 만에 훑어보는 TCP와 UDP](https://wormwlrm.github.io/2021/09/23/Overview-of-TCP-and-UDP.html)****

**[Why does Netflix use TCP but not UDP for streaming video?](https://www.geeksforgeeks.org/why-does-netflix-use-tcp-but-not-udp-for-streaming-video/)**

****[10분 만에 훑어보는 TCP와 UDP](https://wormwlrm.github.io/2021/09/23/Overview-of-TCP-and-UDP.html)****

**[[네트워크] TCP/IP 흐름 제어 & 혼잡 제어](https://steady-coding.tistory.com/507)**

**[HTTP/3는 왜 UDP를 선택한 것일까?](https://evan-moon.github.io/2019/10/08/what-is-http3/)**

**[스타크래프트 속 작은 네트워크 디테일](https://brunch.co.kr/@b437407ee87f499/8)**