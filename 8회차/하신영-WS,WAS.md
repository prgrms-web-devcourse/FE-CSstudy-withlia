## 학습 목표

- Web Server 개념 이해
- WAS 개념 이해
- WS와 WAS의 차이점 이해
- WAS형태의 종류와 장단점을 이해

<br/>

# WS& WAS

**Web:** 인터넷 기반으로 정보를 공유하게 해주는 서비스

- url : 주소
- HTTP: 통신규약
- HTML: 정보를 담은 문서

**Server :** 클라이언트에게 네트워크를 통해 정보나 서비스를 제공하는 컴퓨터 시스템

<br/>

## WS

---

웹서버란 클라이언트로부터 HTTP 요청을 받으면 입력받은 url에 따라 HTML 문서처럼 정적인 데이터를 반환하는 서버입니다.

![image](https://user-images.githubusercontent.com/79133602/191782074-a04b85d1-b210-4306-86ea-26c1a61534e8.png)

### 종류

| WS                  |
| ------------------- |
| apache,NginX, IIS,… |

- Apache Server
  - BSD, 리눅스 등 유닉스 계열 뿐만 아니라 윈도우같은 OS에도 운용가능한 서버
  - Netlify가 Apache Server를 사용해 웹호스팅을 함
- NginX
  - Apache 보다 효율적이다. 비동기 event 방식으로 http request를 처리하기에 context switching 비용이 낮고 memory 효율성이 높기 때문
  - Vercel이 NginX를 사용

<br/>

### 한계

> 동적인 컨텐츠 제공 못해

웹서버는 자료를 가공하지 못 합니다. 오로지 해당 url에 지정된 정적인 자료를 반환할 뿐입니다. 초창기 웹사이트는 미리 만들어둔 html문서를 열람하는 데 그쳤기 때문에 이런 특징이 문제가 되지 않았지만 데이터를 동적으로 표현할 필요가 생기면서 한계가 드러났습니다.

예를 들어 깃허브마이페이지를 웹서버만으로 만든다면, 다음과 같은 문서를 4000만개 이상 만들어야 할 것입니다.

![image](https://user-images.githubusercontent.com/79133602/191782184-e8b9a6b1-1d4d-49ef-88a8-27c4b1223a63.png)

당연히, 그런식으로 웹서비스를 운영할 수는 없습니다.

따라서, 바뀌는 부분만 동적으로 가져와 페이지를 렌더링해주는 작업이 필요니다. 이를 위해 데이터베이스 접근 및 동적 컨텐츠 처리 작업을 해줄 수 있는 WAS가 사용됩니다.

<br/><br/>

## WAS

---

> 동적 컨텐츠를 처리
> Web container가 존재

WAS는 웹 서버와 웹 컨테이너가 합쳐진 형태로 웹서버와 데이터베이스 사이에 위치합니다.

- 웹컨테이너
  JSP와 servlet를 실행시킬 수 있는 소프트웨어
- servlet
  자바를 사용해서 요청에 대한 결과를 전송해주는 프로그램
- JSP(Java Server Page)
  ServerSide 스크립트 언어로, HTML에 Java를 사용해 동적 컨텐츠를 표현하는 웹 어플리케이션 도구

웹서버가 클라이언트가 보낸 요청을 WAS로 전달하면 WAS는 데이터베이스에 접근해 비즈니스로직을 처리하고 가공한 데이터를 웹서버를 통해 클라이언트로 전달할 수 있습니다.

![image](https://user-images.githubusercontent.com/79133602/191782287-b12a3111-6b25-4639-b512-d372809230f8.png)

### 예시 ) 레포지토리 삭제 작업

깃허브 레포지토리를 삭제한 뒤 레포지토리 목록 페이지가 바뀌는 과정을 WAS를 통해살펴보면 다음과 같습니다.

- 삭제 url 처리
  삭제버튼을 누른 뒤 delete 요청이 웹서버로 전달되면 웹서버는 해당 요청을 WAS로 보냅니다. WAS는 DB에 접근해서 사용자 데이터에서 해당 레포를 삭제합니다.

![image](https://user-images.githubusercontent.com/79133602/191782720-6d1cb1b8-84da-4d99-9db5-9bb067feb186.png)

- 새로운 목록 반환
  그리고 레포지토리목록 페이로 들어가면 get 요청을 통해 WAS가 DB에 접근 후 업데이트된 레포지토리 목록을 반환해줍니다. 웹서버는 해당 데이터가 반영된 페이지 파일을 클라이언트에 전달, 사용자는 삭제된 목록을 볼 수 있게 됩니다.

![image](https://user-images.githubusercontent.com/79133602/191782377-e796c0a6-1727-42f1-beeb-e696c93c6216.png)

<br/>

### 종류

| WAS                        |
| -------------------------- |
| tomcat,JBoss,jeus,Oracle,… |

- tomcat: http 서버 및 Java servlet container
  웹서버와 연동해 실행할 수 있는 자바환경을 제공, JSP, 자바서블릿을 실행할 수 있음
  메모리가 작기 때문에, 단순한 웹 응용 프로그램이나 전체 Java EE 서버가 필요없는 Spring과 같은 프레임 워크를 사용하는 응용 프로그램에 널리 사용됨

<br/><br/>

## WAS의 구조

---

### 1. WAS에 WS가 포함된 구조

![image](https://user-images.githubusercontent.com/79133602/191782469-32c51665-34cc-42d2-ba76-bea98d7e3518.png)

WAS가 정적 컨텐츠를 처리하는 웹서버와 동적데이터를 처리하는 컨테이너를 모두 가지고 있는 경우입니다. 사용자가 적은 경우라면 하나의 서버에서 모든 작업을 처리할 수 있어서 편리합니다.

<br/>

### 1.1 해당 구조의 문제점

> 서버 과부화 → 나쁜 UX

하지만, 사용자가 많아지고 처리해야 할 비즈니스 로직이 증가한다면 서버에 부담이 됩니다. 처리 속도가 느려지면 페이지 로딩 속도도 느려지기에 사용자 경험에 좋지 않습니다. 따라서, WS를 WAS에서 분리할 필요가 있습니다.

<br/>

---

### 2. WS와 WAS를 분리

![image](https://user-images.githubusercontent.com/79133602/191782524-fbf531ff-643a-4cd2-9c65-b4d415352cc8.png)

먼저 단순히 WS뒤에 하나의 WAS가 오는 경우부터 설명하겠습니다.

- 서버 부화 방지
  WS와 WAS를 분리하면 WS는 정적 데이터만을 처리하고, WAS는 동적데이터만을 처리하기에, 위에서 언급한 서버과부화 문제를 해결할 수 있습니다.
- 보안 강화
  또 WS와 WAS 사이에 방화벽을 두거나, SSL을 통해 데이터를 패킷화하고 암호복호화를 통해 전달할 수 있어 보안 강화에 좋습니다.

<br/>

---

### 3. WS에 여러대의 WAS가 존재

![image](https://user-images.githubusercontent.com/79133602/191782824-dac2dd10-e741-4eec-bfda-14063bf33d0a.png)

여기서 더 나아가 WS에 여러대의 WAS를 연결할 수 있습니다. 해당 구조는 다음과 같은 장점을 갖습니다.

- 로드 밸런싱 가능
  Web Sever를 통해 WAS의 부하가 최소화되는 쪽으로 데이터 처리를 수행할 수 있습니다.
- 에러 핸들링 가능
  failover, faliback 다시 말해, 하나의 WAS가 작동하지 않을 떄 다른 WAS가 해당 요청을 처리하고, 그동안 문제의 WAS를 재시작할 수 있습니다. 덕분에 사용자로 부터 에러를 숨겨 UX를 최적화할 수 있습니다.
- 여러 웹 어플리케이션 서비스 가능
  하나의 서버에 PHP, JAVA 어플리케이션을 함께 사용할 수 있어 기존 WAS에 새로운 웹 어플리케이션을 추가하는 경우 유동적으로 프로그래밍할 수 있습니다.

<br/><br/>

## 결론

---

> WS와 WAS를 분리해서 사용하자

사실 우리는 이미 WS와 WAS를 분리해서 사용하고 있습니다. SPA의 발달로 프론트엔드 서버와 백엔드 서버를 분리할 필요가 있었기 때문입니다.

그렇지만 아직 프로젝트에서 WS에 여러개의 WAS를 둔 적은 없는데, 사용자가 많은 경우 WAS를 여러대 둬서 failover,failback 등 에러 상황에 대처하고 로드 밸런싱으로 최적화를 해야한다는 점을 배웠습니다.

<br/><br/>

## 참고

---

💻 [[WEB] Web Srver와 WAS](https://hyuntaekhong.github.io/blog/WAS/#web-application-server--was)

💻 [[Web] 웹 서버와 WAS의 차이를 쉽게 알아보자](https://codechasseur.tistory.com/25)

💻 [[Web] Web Server vs Web Application Server 웹 서버와 웹 어플리케이션 서버](https://velog.io/@juejue/Web-Web-Server-vs-Web-Application-Server-%EC%9B%B9-%EC%84%9C%EB%B2%84%EC%99%80-%EC%9B%B9-%EC%96%B4%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98-%EC%84%9C%EB%B2%84)

💻 [AP서버 vs Web서버 vs WAS vs DB서버](https://rainkim.tistory.com/35)

💻 [[React] 프론트 엔드와 백 엔드 분리 시 동작 원리 (vs 풀 스택)](https://it-eldorado.tistory.com/85)

💻 [[Servlet] 서블릿(Servlet)이란?](https://velog.io/@falling_star3/Tomcat-%EC%84%9C%EB%B8%94%EB%A6%BFServlet%EC%9D%B4%EB%9E%80)

💻 [아파치 로드밸런싱으로 여러 WAS 운영하기](https://sb.pe.kr/7373)
