# HTTP의 역사와 요소

### 목차

- **HTTP의 특징**
    - Method와 경로
    - Header
    - Body **(본문)**
    - Status Code
- **HTTP의 역사**
    - HTTP의 역사와 함께 요소, 특징의 변화

---

# 1. HTTP의 특징

### Hyper Text

> 참조를 통해 독자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트
> 

### 프로토콜 ( Protocol)

프로토콜은 컴퓨터 내부에서, 또는 컴퓨터 사이에서 데이터의 교환 방식을 정의하는 규칙 체계.

## HTTP (**Hyper Text Transfer Protocol)**

> 웹 브라우저와 웹 서버가 통신하는 절차와 형식을 규정한 것
> 

> Chrome과 같은 웹 브라우저가 클라이언트
> 

- **Request / Response (요청 /응답)** 동작에 기반해 서비스 제공
- **상태가 없는(stateless) 프로토콜**
    - 데이터를 주고 받기 위한 각각의 데이터 요청이 서로 독립적으로 관리가 된다.
    - 서버는 세션과 같은 별도의 추가 정보를 관리하지 않아도 된다. → 다수의 요청 처리 및 서버의 부하를 줄일 수 있는 성능 상의 이점이 생긴다.
- HTTP 프로토콜은 일반적으로 **TCP/IP 통신 위에서 동작**하며 **기본 포트는 80**번
- OSI 7계층인 **Application layer**의**프로토콜**
- 웹 표준 데이터(HTML 문서 뿐만 아니라 이미지, 동영상 등)를 받아오는 프로토콜
- HTTP 0.9 ~ 2까지는 기본적으로 TCP전송 프로토콜을 사용
- 전송 프로토콜 TCP 3way로 연결 / UDP 그냥 보낸다

---

# 2. HTTP의 역사

## ⌛ 1990년 HTTP 0.9

> 1990년, **최초** 버전인 **HTTP/0.9**는 HTML document를 요청해서 가져오기만 하는 단순한 프로토콜
> 

### 특징

- HTTP 초기 버전에는 버전 번호가 없었다.
    - HTTP/0.9는 이후에 차후 버전과 구별하기 위해 0.9로 불리게 됐다.
- ‘브라우저가 문서를 요청하면 서버는 데이터를 반환한다’는 웹의 기본 뼈대는 완성 됐으나 아래와 같이 할 수 없는 일이 많았다.
    - 하나의 문서를 전송하는 기능만 존재
    - **HTML 문서만 전송 가능 (다른 형식 불가)** (http header가 없어서)
    - 클라이언트 쪽에서 검색 이외의 **요청을 보낼 수 없었다.**
        - HTML 문장 내에서 `<isindex>` 태그를 사용하면 텍스트 입력란이 생겨 검색할 수 있다.
        - 검색을 하면 주소 끝에 ? 기호와 단어를 붙여 검색요청을 보낸다.
    - 새로운 문장을 전송하거나 **갱신 또는 삭제할 수 없었다.**
    - 요청이 올바른지 혹은 서버가 올바르게 응답했는지 아는 방법이 없었다.
    

### 요소

- **Method는 오직 GET 뿐,  경로  有**
- Header 없음
- **Body 존재**
- Status Code 없음 → error는 오직 보내는 HTML 문서의 내용으로만 알 수 있었다.

---

## ⌛1996년 HTTP/1.0

### 특징

- HTTP의 기본 4요소가 모두 함께 등장
- 요청 시 HTTP 버전이 추가됐다

```jsx
* HTTP 1.0, assume close after body
< HTTP/1.0 200 OK
< Date:Tue, 05 Apr 2016 13:58:43 GMT
< Content-Length: 32
< Content-Type: text/html; charset=utf-9
<
<html><body>hello</body></html>
* Closing connection 0
```

하지만 ..

- 하나의 URL은 하나의 TCP연결 → 요청 콘텐츠마다 TCP 세션을 맺어야한다.
    - **단순 동작(연결 수립, 동작, 연결 해제)이 반복**되어 TCP 세션 처리 **부하 문제** 발생
    - 클라이언트의 응답 속도가 느리다.
    - 한 번 HTTP가 응답할 때마다 한 파일 밖에 반환하지 못함

### 요소

- **Method**
    - 여러 메소드들의 등장
        - **GET**: 서버에 헤더와 콘텐츠 요청
        - **POST**:  새로운 문서 투고
        - HEAD: 서버에 헤더만 요청
        - **PUT**: 이미 존재하는 URL의 문서를 갱신
        - **DELETE**: 지정된 URL의 문서를 삭제
        - 그 외 LINK, UNLINK 등 1.1에서 삭제된 메소드들이 있다.
- **Header** 생김 → 프로토콜을 극도로 유연하고 확장 가능하도록 만들어 줌
    - `Content-type` → MIME 타입이라는 식별자를 기술해 파일 종류를 지정, HTML 파일 외 다른 문서 전송 가능
    - `User-Agent`: 클라이언트가 자신의 어플리케이션 이름을 기술
    - `Autorization`: 특별한 클라이언트에만 통신을 허가할 때 인증 정보를 서버에게 전달
    - 이외에도 `Refer, Content-Length, Content-Encoding, Date` 등이 Header
- **Body**
    - 1.0에서는 요청과 응답 양쪽에 헤더가 포함돼 바디와 헤더를 분리할 필요가 있게 되었다.
    - 요청에도 콘텐츠를 포함할 수 있게 돼 새로운 역할이 늘어났다.
- **Status Code**
    - 상태코드가 생겨 서버가 **어떻게 응답했는지 한눈에** 알 수 있게 되었다.
    - **요청에 대한 결과에 따른 동작**을 할 수 있게 되었다.(특정 방법으로 그것의 **로컬 캐시를 갱신**하는 등)

---

## ⌛1997년 HTTP/1.1

> **고속화**와 **안정성**을 추구한 확장
> 

### 특징

- **TLS(Transport Layer Security)에 의한 암호화 통신을 지원** → 강력한 인증 절차가 생기게 되었다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/991c47c3-4455-4fa9-85cf-ff357c0dbc96/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220430%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220430T144518Z&X-Amz-Expires=86400&X-Amz-Signature=ff4527ff6a3c6bb3eb9bb9b41070609d5a2d843c3d94ca554a249d46dbe935a6&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- **통신 고속화**
    - **Keep-Alive**가 **기본적으로 유효 (Persistent connection)**
    - **파이프라이닝(Pipelineing)**
    - HTTP/1.0에서의 통신은 10개의 API 통신 → 10번의 반복된 연결/ 해제 과정
    - HTTP/1.1:  일정시간 클라이언트 서버와 API서버간의 연결 정보를 **기억해** 반복적으로 일어나는 통신에 연결의 맺고 끊음을 줄인다.
        
        ![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0628ca7b-f96d-4c57-94f7-33c54b68bd78/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220430%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220430T144629Z&X-Amz-Expires=86400&X-Amz-Signature=fafea399a8d0372243d46ee91595e86d8c1da304ab7b6bc640a8f5f290c9fef6&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)
        

---

### 요소

- **Method**
    - `PUT,` `DELETE` 가 **필수 메소드**가 되었다. (HTTP/1.0에서는 옵션)
    - OPTION, TRACE, CONNECT 메소드가 추가됐다.
- **Header**
    - Host 헤더를 사용한 가상 호스트를 지원
    - 한대의 웹 서버로 하나의 도메인만 다루는 것이 아닌 여러 웹 서버를 운영할 수 있게 됐다.
- **Body**
    - Body 전송 확인
    - 클라이언트에서 서버로 데이터를 보내기 전 일단 받아들일 수 있는지 물어보고 나서 데이터를 보내는 2단계 전송할 수 있게 됐다.

---

### Keep-Alive

- Keep-Alive를 사용하면 연속된 요청에 접속을 계속 이용해 핸드셰이크 횟수를 줄인다. → TCP/IP 접속까지의 대기시간이 줄어들고, 통신 처리량이 많아진다.
- 요청 Header에 `Connection: Keep-Alive` 를 추가해 Keep-ALive를 이용할 수 있었다.
- Keep-ALive는 HTTP/1.0 사양에는 들어가지 않았지만, 몇몇 브라우저에서는 이미 지원하고 있었다.
- HTTP/1.1에서부터는 이 동작이 **기본적**으로 되어 있다.
- Keep-Alive을 이용한 통신은 헤더에 Connection: Close를 부여해 접속을 끊을 수 있으나, **실제로는 타임아웃으로 접속이 끊어지기를 기다린다.**
    - 지속 시간은 클라이언트와 서버 모두 가지고 있다. 짧은 쪽으로 사용됨

 

---

### 파이프라이닝(Pipelining)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/cb55418f-6a73-44bb-bbe8-73759fa58a48/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220430%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220430T144645Z&X-Amz-Expires=86400&X-Amz-Signature=ab844fe622008fcb1642e9ebaa0e64f051ce7b16684a8ad0dc31388ec65a2b2f&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

> 파이프라이닝은 **최초의 요청이 완료되기 전에 다음 요청을 보내는** 기술. 다음 요청까지의 대기 시간을 없앰으로써, 네트워크 가동률을 높이고 성능을 향상시킨다.
> 
- **Keep-Alive 이용을 전제**로 하며, 서버는 요청이 들어온 순서대로 응답을 반환한다.
- 하지만 실제로 사용했을 때 문제점이 발생했다. **(Head of Line blocking)**
    - **요청받은 순서대로 응답**해야만 하므로
    - 응답 생성에 시간이 걸리거나 크기가 큰 파일을 반환하는 처리가 있으면 **다른 응답에 영향을 줬다**.
- HTTP/2서 **스트림**이라는 새로운 구조로 다시 태어나게 되었다.

---

## ⌛2015년 HTTP/2

> HTTP/1.1이 오랫동안 사용되다가 16년만에 새로운 규격이 등장
> 

> HTTP/2의 가장 큰 목적은 **통신 고속화**
> 

### 특징

- **스트림을** 사용해 **바이너리 데이터를 다중으로 송수신하는 구조로 변경**
    - 텍스트 기반 프로토콜 → **바이너리 기반** 프로토콜
    - HTTP/1.1까지는 **하나의 요청이 TCP소켓을 독점** → 하나의 서버에 2~6개의 TCP 접속을 해서 병렬화
    - HTTP/2에서는 하나의 TCP 접속 안에 **스트림**이라는 **가상의 TCP 소켓을 만들어 통신**
    - **스트림은 일반 TCP소켓과 같은 핸드셰이크가 필요 없어서 통신 용량이 허락하는 한, 몇 만번의 접속도 병렬화 가능 (Multiplexing 다중화)**
    - HTTP의 Head of line blocking 문제 해결

- **스트림 내 우선 순위 설정**과 서버 사이드에서 데이터 통신을 하는 **서버 사이드 푸시를 구현**했다.
    - 서버 푸시를 이용해 CSS, JS, 이미지 등 웹페이지를 구성하는 파일 중 **우선순위가 높은** 콘텐츠를 클라이언트가 요구하기 전에 전송할 수 있게됐다.
    - 푸시된 콘텐츠는 사전에 캐시에 들어간다.

- 헤더가 압축할 수 있게 되었다.
    - HPACK이라는 방식의 key-value로 이루어진 사전의 사전을 사용하는 방식을 통해 압축
    - 페이지 로드 시간을 감소

---

### ⌛ QUIC, HTTP/3

> 구글에서 만든, UDP 소켓상에서 만들어진 프로토콜
> 

> 아직 HTTP/2의 점유율도 50퍼가 되지 않지만 등장했다.
> 

> **HTTP/3**에서는 전송 계층 부분에 TCP 대신 **QUIC을 사용**한다.
> 

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/94e1e218-edbc-4e2d-8b39-bd1446a8bcf5/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220430%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220430T144707Z&X-Amz-Expires=86400&X-Amz-Signature=dc7b2a8f64a18c4d40c8f968f933e9a7a0c8b2e5e7339363612fdbc6d246d28a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

- TCP는 너무 복잡하고 무거워 뜯어고치기에는 어렵다.
- 그래서 **커스터마이징이 용이**하다는 장점을 가진 **UDP를 사용**
- QUIC은 UDP상에서 TCP에서의 재전송처리, 혼잡제어 등을  자체적으로 구현했다.

### QUIC의 특징

- QUIC은 3way 핸드셰이크 과정 대신 첫번째 핸드셰이크를 거칠 때, 연결 설정에 필요한 정보와 함께 데이터도 보내버린다.
    - **연결 설정 시 레이턴시 감소시켜** 연결 설정에 소요되는 시간이 반 정도 밖에 안된다.
    - 그리고 한번 연결에 성공했다면 서버는 그 설정을 캐싱해놓고 있다가, 다음 연결 때는 캐싱해놓은 설정을 사용해 레이턴시를 줄인다.
- **다중화 지원** - head of line blocking 문제 해결
- QUIC은 **Connection ID를 사용**하여 서버와 연결을 생성한다.
    - Connection ID는 랜덤한 값일 뿐, 클라이언트의 IP와는 전혀 무관한 데이터이기 때문에 클라이언트의 IP가 변경되더라도 기존의 연결을 계속 유지
    - 예를들어 wi-fi에서 셀룰러로 전환되거나 그 반대의 경우에도 연결을 유지할 수 있게 한다.
    - TCP의 경우 소스의 IP 주소와 포트, 연결 대상의 IP 주소와 포트로 연결을 식별해 IP가 바뀌면 연결 끊긴다.
- QUIC은 이미 Chrome에 포함돼있다. **w3tech**에서 발표한 내용에 따르면, **QUIC**은 2022년 4월 기준 모든 웹사이트 중 **24.6%가 사용**하고 있다.

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4a936264-4cb4-42e9-9dfb-0e59e0bf5d1b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220502%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220502T095544Z&X-Amz-Expires=86400&X-Amz-Signature=1ff2114c2d92c347c9234cd775e4936daa375a107b20dc5ccd1c3fe6279708fd&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

---

### 마무리

- 저번 발표에서 바로잡을 점
- 넷플릭스는 TCP를 사용할까? UDP를 사용할까?

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/781d4fe5-c3dc-4e45-a5bc-012073ba9fbb/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220430%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220430T144738Z&X-Amz-Expires=86400&X-Amz-Signature=e5d70b0f7ce5f4906aedb27738ce4263a0428d97d7aeaa48f826c0009e58ddb2&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/66a81ed1-b89d-4d4a-ac9b-e924742d5597/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220430%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220430T144751Z&X-Amz-Expires=86400&X-Amz-Signature=8d704f8690ec388594585f07955af09cdaaf54f1f38c608efea952faefa66e45&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

---

### 출처

[리얼월드 HTTP](https://withbundo.blogspot.com/2021/02/httphol-head-of-line-blocking.html)

[https://www.w3.org/Protocols/](https://www.w3.org/Protocols/)

[https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP#http0.9_–_원-라인_프로토콜](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP#http0.9_%E2%80%93_%EC%9B%90-%EB%9D%BC%EC%9D%B8_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)

[https://joshua1988.github.io/web-development/http-part1/](https://joshua1988.github.io/web-development/http-part1/)

[https://evan-moon.github.io/2019/10/08/what-is-http3/](https://evan-moon.github.io/2019/10/08/what-is-http3/)