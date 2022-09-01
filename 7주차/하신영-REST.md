# REST

Representational State Transfer

HTTP에 있어서 네트워크 자원을 처리하는 방법 전반을 다루는 아키텍처 스타일에 대한 지침

## 출현 계기

> 어떻게 하면 기존 HTTP와 호환성을 갖게 할까?

1991 www가 탄생하고 HTML이 인터넷 정보를 표현하는 언어가 됩니다. HTML로 정보를 교환하기 위해 HTTP 1.0을 설계하는데 이후 해당 버전을 개선하는 과정에서 기존 버전의 웹을 망가 뜨리지 않으려면 어떻게 할 지 고민이 생깁니다. 1.0 버전과 호환이 되지 않으면 기존 웹 사이트 서비스를 전부 사용할 수 없게 되기 때문입니다. 로이필딩은 HTTP Object Model이란 해결책을 내놓았고 이후 우리가 아는 이름 REST가 됩니다.

## REST API

REST의 내용을 다루기 전에 API에 대해 간략히 설명하고 넘어가겠습니다.

## API

Application Programming Interface

API는 서버와 애플리케이션 간 데이터를 교환하는 매개체, 방법입니다. 클라이언트가 존재하는 API로 서버에 요청을 보내면 서버는 API에 지정된 응답을 클라인트에 내려주는데, 이 때 API의 url이 REST에 입각해 설계된 경우를 REST API라고 할 수 있습니다.

## RESTful이란?

또, RESTful API의 경우 REST API와 별도의 존재라기 보다, REST가 잘 지켜진 API라고 보면 됩니다.

그렇다면 REST의 내용을 살펴보면서 RESTful한 API는 어떻게 만드는 지 알아보도록 하겠습니다.

## REST 아키텍쳐 스타일

1998년 발표된 이름 REST로 더 유명한 이 지침은 다음과 같은 내용으로 이뤄졌습니다. 굉장히 엄격하기에 이 중 일부라도 지켜지지 않으면 로이필딩은 RESTful하지 않다고 말합니다.

### 1. client-server

- clinet: 자원을 요청
- server: 자원을 제공

server는 api를 제공하고 비즈니스 로직 처리 저장을 담당, client는 server로 부터 자원을 받아 인증, 및 정보처리를 담당하는 식으로 역할을 나눠 서로의 의존성이 줄도록 합니다.

### 2. stateless

- 무상태성

API 서버는 들어오는 요청만을 처리해 응답을 내려줄 뿐 요청의 상태 정보를 따로 저장 관리하지 않습니다. (세션, 쿠키 등..) 덕분에 서비스의 자유도가 높아지고 불필요한 정보를 관리하지 않아 구현이 단순해집니다.

### 3. cache

- 캐싱이 가능

REST가 HTTP의 스타일 지침이기에 당연히 HTTP의 캐싱 기능도 사용할 수 있습니다.

### 4. layered system

REST 서버는 다중 계층으로 구성 가능해야 합니다. 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 두거나, PROXY, 게이트웨이 같은 네트워크 기반 미들웨어를 사용할 수 있어야 합니다.

### 5. code-on-demand(optional)

자바스크립트 같은 코드를 서버에서 클라이언트로 보낼 수 있어야 합니다.

### 6. uniform interface

- 가장 지켜지기 어려운 항목

일단, resource가 uri로 식별되야 하고 resource를 업데이트, 삭제, 생성 시 HTTP 메세지에 정보를 담아 전송을 해 조작하도록 한다는 규칙(identification of resources,manipulation of resources through representations)은 지키기 어렵지 않습니다.

하지만 HTTP 메세지가 스스로를 설명하고 애플리케이션의 상태를 Hyperlink로 전이시켜야 된다.는 규칙은 지키기 어렵습니다. (**Self-Descriptive Messages,HATEOAS)**

|                  | HTML            | JSON      |
| ---------------- | --------------- | --------- |
| HyperLink        | 가능(a 태그 등) | 정의 안됨 |
| Self-description | 가능(HTML 명세) | 불완전    |

MediaType이 HTML이라면 괜찮지만, JSON인 경우 hyperlink가 없고 스스로를 설명하는 명세가 없기 때문입니다.

## uniform interface 조건 만족

JSON 타입을 uniform interface에 맞도록 변경하려면 HTTP의 헤더를 사용할 수 있습니다.

### **Self-Descriptive Messages**

- API의 목적지를 알 수 없는 경우
  Host 헤더에 목적지 도메인 기입

```jsx
GET / HTTP / 1.1;
Host: www.example.com;
```

- 내용을 알 수 없는 경우
  해당 JSON의 속성 값이 뭘 의미하는 지 모르는 경우 json 명세를 IANA에 등록 후 Link 헤더에 rel=”profile”로 해당 명세의 url을 기입하면 됩니다.

```jsx
HTTP/1.1 200 OK
Content-Type: application/json
Link: <http://example,com/docs>; rel="profile"
[ { "id": "dsafsd11", "name": "name"} ]
```

### **HATEOAS**

**hypermedia as the engine of application state**

링크를 response data로 내려주거나 Location,Link 헤더 사용해 표시할 수 있습니다.

```jsx
HTTP/1.1 200 OK
Content-Type: application/json
Link: </deatil/>; rel="collection"
Location: /detail
[ { "id": "dsafsd11", "name": "name", link="http://example.com/detail/1"} ]
```

### 문제점

하지만 위와 같은 방법으로 JSON 타입 API를 수정하면 다음과 같은 문제들이 있습니다.

- media type을 매번 지정해줘야 함
- Link에 등록한 명세서를 매번 수정해서 등록해줘야 함
- 서버에서 response data에 link 속성을 직접 넣어주는 작업 필요

<br/><br/>

## 결론

> RESTful한 API를 짜는 건 중요, BUT 집착하지 말자

RESTful API 덕분에 서버와 클라이언트는 독립적으로 작업을 할 수 있습니다. 만약 REST 지침을 전부 어긴다면, 서버가 업데이트 될 때 마다 클라이언트가 수정을 해야되는 끔찍한 상황에 처했을 것입니다.

하지만 미디어 타입이 JSON인 경우처럼 REST에 완벽히 들어 맞는 API를 짜기란 어렵습니다. 이를 해결하는 방법이 있어도, 이게 잘 하는 일일까 싶을 정도로 번거로운 작업들이 존재합니다.

로이필딩 역시, 시스템 전체를 통제할 수 있거나 진화에 관심이 없다면 REST에 따지느라 시간낭비를 하지 말라고 조언합니다.

따라서, REST에 입각해 호환성 높은 API를 구현하되 다음 경우라면 지키기 어려운 uniform interface 조건은 넘어가도록 할 필요가 있다고 생각합니다.

- 지키지 않아도 호환성에 문제가 없고
- HTTP 메시지로 정보를 명시해야 하는 경우가 아니라면

<br/><br/>

## 참고

<https://doctorson0309.tistory.com/658>

<https://velog.io/@nyong_i/%EB%A1%9C%EC%9D%B4-%ED%95%84%EB%94%A9%EC%9D%B4-%EC%9D%B4-%EA%B8%80%EC%9D%84-%EC%A2%8B%EC%95%84%ED%95%A9%EB%8B%88%EB%8B%A4#%EA%B7%B8%EB%9F%BC-api%EB%8A%94-%EA%BC%AD-rest-api%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%98%EB%8A%94%EA%B0%80>

<https://overcome-the-limits.tistory.com/38#%EA%B7%B8%EA%B2%83%EC%9D%80-rest%EA%B0%80-%EC%95%84%EB%8B%88%EB%8B%A4>

[그런 REST API로 괜찮은가](https://www.youtube.com/watch?v=RP_f5dMoHFc)
