# REST API
REST API라는 용어는 비교적 친숙한 용어다. 
우리가 이미 많이 사용하고 있는 공개 API인 카카오, 네이버, 공공 API 등에 적혀있는 사항을 보면 대부분 REST API 형식으로 만들어져 있다는 것을 알 수 있다.  
이처럼 REST API는 웹 표준 수준 만큼 모든 곳에서 쓰이고 있다. 

## REST와 API의 사전적 정의

>REST(Representational State Transfer)는 월드 와이드 웹과 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 아키텍처의 한 **형식**이다.

> API(Application Programming Interface, 응용 프로그램 프로그래밍 인터페이스)는 **응용 프로그램에서 사용할 수 있도록, 운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다.**


## API
> API(Application Programming Interface, 응용 프로그램 프로그래밍 인터페이스)는 응용 프로그램에서 사용할 수 있도록, 
운영체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스를 뜻한다.

UI(사용자 인터페이스)가 컴퓨터 <=> 인간 간의 연결이라면, API는 에플리케이션 <=> 서버를 연결해 주는 인터페이스이다. 
즉, 프로그램과 프로그램 사이에서 서로 상호작용하는 것을 도와주는 매개체이다.  

>내가 만드는 프로그램(응용 프로그램)에서 누군가가 만든 기능(운영체제나 프로그래밍 언어가 제공하는 기능)을 제어할 수 있게 하는 매개체가 API라고 할 수 있다. 


## REST
>REST(Representational State Transfer)는 월드 와이드 웹과 같은 분산 하이퍼미디어 시스템을 위한 소프트웨어 아키텍처의 한 형식이다.

즉, 자원의 상태(정보)를 주고 받는 **형식 또는 규칙**이라고 생각하면 된다. 
REST는 기본적으로 **웹의 기존 기술**과 **HTTP프로토콜을 그대로 활용**하기 때문에 웹의 장점을 최대한 활용할 수 있도록 만들어진 아키텍처이다. 


> **REST API는 REST 규칙에 따라 웹 서버와 어플리케이션을 이어주는 인터페이스라고 생각하면 된다.** 

### REST의 등장
REST API의 등장은 2000년도에 HTTP의 주요 저자 중 한 사람인 로이 필딩이 웹 설계의 우수성을 제대로 활용하지 못하는 모습을 보고 
웹의 장점을 최대한 활용할 수 있는 아키텍쳐로 REST를 발표 했다. 
최근의 서비스 / 어플리케이션의 개발은 아이폰, 안드로이드, 웹 등 다양해지고 있기 때문에 범용적으로 사용성을 보장하는 REST API가 널리 쓰이게 되었다. 

### REST의 특징
#### Uniform (유니폼 인터페이스)
URI 사용, HTTP메소드 사용 등의 지정된 인터페이스를 준수한다. 
HTTP 표준에만 따른다면, 안드로이드/IOS 플랫폼이든, 특정 언어나 기술에 종속되지 않고 모든 플랫폼에 사용할 수 있으며, URI로 지정한 리소스에 대한 조작이 가능한 아키텍처 스타일을 의미한다.

#### Stateless (무상태성)
REST는 무상태성 성격을 가진다. 다시 말해 작업을 위한 상태정보를 따로 저장하고 관리하지 않는다. 세션 정보나 쿠키 정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 단순히 처리하면 된다. 그래서 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해진다.

#### Cacheable (캐시 가능)
REST의 가장 큰 특징 중 하나는 HTTP라는 기존 웹 표준을 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 그대로 활용할 수 있다. 따라서 HTTP가 가진 캐싱 기능을 활용해서, 서버의 응답 메시지 같은 것을 캐싱할 수 있다. 

#### Self-descriptiveness (자체 표현 구조)
REST API 메시지만 보고도 이를 쉽게 이해할 수 있는 자체 표현 구조로 되어 있다. 

#### Client - Server 구조
REST 서버는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보) 등을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로 간 의존성이 줄어들게 된다.

#### 계층형 구조
REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 한다.

### REST의 구성
모든 데이터 구조와 처리 방식은 URL을 통해 정의된다.

rest의 구성은 다음과 같다. 

>자원(Resource)<br>
행위(Verb)<br>
표현(Representations)

자원 (Resource)
모든 자원에 고유한 ID가 존재하고, 이 자원은 Server에 존재한다.
자원을 구별하는 ID는 HTTP URI이다.

행위 (Verb) - Http Method
HTTP 프로토콜의 Method를 사용한다.
HTTP 프로토콜은 GET, POST, PUT, DELETE와 같은 메서드를 제공한다.

표현 (Representaion of Resource)
Client가 자원의 상태 (정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답을 보낸다.
REST에서 하나의 자원은 JSON, XML 등 여러 형태로 나타낼 수 있다.

요약하자면, HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, HTTP Method(POST, GET, PUT, DELETE)를 통해 
해당 자원에 대한 CRUD를 한다.  

#### CRUD동작

Create : 데이터 생성(POST)
Read : 데이터 조회(GET)
Update : 데이터 전체 수정(PUT) /부분 수정(PATCH)
데이터 전체 수정 시 보내지 않은 다른 정보들은 삭제 / 부분 수정 시 보내지 않은 다른 정보들은 유지
Delete : 데이터 삭제(DELETE)

### REST API 설계 규칙
REST API 설계 규칙
- 슬래시(/)는 계층 관계를 나타내는데 사용한다.
ex) https://localhost:5000/products/categories
- URI 마지막 문자로 슬래시(/)를 사용하지 않는다.
URI는 리소스의 유일한 식별자로 사용되어야하기 때문에, 슬래시를 붙인 것과 안 붙인 것의 차이가 명확하므로 괜한 혼동을 일으키지 않기 위해 슬래시를 마지막에 사용하지 않는다.
ex) https://localhost:5000/products/categories/

- 하이픈( - )은 URI 가독성을 높이는데 사용한다. 밑줄( _ )은 가독성을 위해 사용하지 않는다.

- URI 경로에는 소문자를 사용한다.

- 파일 확장자는 포함하지 않습니다. 필요한 경우 Accept header에 명시한다. 

### REST API 예시
블로그 POST에 대한 CRUD 작업

모든 포스트를 조회한다.
- [GET] /posts

특정 포스트를 조회한다
-[GET] /post/:id

포스트를 생성한다
- [POST] /posts

특정 포스트를 수정한다
- [PUT] /posts/:id

특정 포스트를 삭제한다
- [DELETE] /posts/:id


### 카카오 local api로 보는 rest api
요청형식
> GET /v2/local/search/address.${FORMAT} HTTP/1.1

실제 요청
>GET "https://dapi.kakao.com/v2/local/search/address.json" \
  -H "Authorization: KakaoAK ${REST_API_KEY}" \
  --data-urlencode "query=전북 삼성동 100" 

응답
> HTTP/1.1 200 OK
  "documents": [
    {
      "address_name": "전북 익산시 부송동 100",
      "y": "35.97664845766847",
      "x": "126.99597295767953",
    }
]
...

링크 : https://developers.kakao.com/docs/latest/ko/local/dev-guide

### 확인 문제 OX
- REST API는 json으로만 데이터를 주고받는다.
- 부분 수정은 PATCH를 사용하고, 수정 시 보내지 않은 데이터들은 유지된다. 
- URI가 같더라도 method에 따라 실제 수행하는 것이 다를 수 있다. 


