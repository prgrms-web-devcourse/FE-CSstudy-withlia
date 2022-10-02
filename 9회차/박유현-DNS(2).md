# Domain

![[https://opentutorials.org/module/3421/20303](https://opentutorials.org/module/3421/20303)](https://s3-ap-northeast-2.amazonaws.com/opentutorials-user-file/module/3421/8338.jpeg)

도메인 이름의 구조 [https://opentutorials.org/module/3421/20303](https://opentutorials.org/module/3421/20303)

<br />

# DNS name resolution

## 재귀적 질의 방법

질의를 받은 DNS 서버가 결과값을 가지고 있지 않다면, **직접 하위 DNS 서버에게 질의**하여 결과값을 응답한다.

## 반복적 질의 방법

질의를 받은 DNS 서버가 결과값을 가지고 있지 않다면, 해당 질의에 **응답할 수 있는 하위 DNS 서버 목록**을 결과값으로 응답한다.

![[https://gaia.cs.umass.edu/kurose_ross/interactive/dns_query.php](https://gaia.cs.umass.edu/kurose_ross/interactive/dns_query.php)](https://gaia.cs.umass.edu/kurose_ross/interactive/dns_query.png)

DNS - ITERATIVE VS RECURSIVE QUERY [https://gaia.cs.umass.edu/kurose_ross/interactive/dns_query.php](https://gaia.cs.umass.edu/kurose_ross/interactive/dns_query.php)

## DNA Lookup 과정

1. 브라우저 주소창에 `www.example.com` 입력 시 컴퓨터에 설정되어 있는 **로컬 DNS**에게 `www.example.com` 의 IP 주소를 **재귀적 질의 방법**으로 질의한다.
2. 로컬 DNS는 `www.example.com - IP 주소` 값이 캐싱되어 있는 경우 브라우저에게 IP 주소를 응답한다. 캐싱되어 있지 않은 경우 모든 DNS 질의는 **반복적 질의 방법**으로 하며, 루트 DNS 서버에게 `www.example.com` 의 IP 주소를 질의한다.
3. **루트 DNS 서버는 com을 관리하는 최상위 레벨 도메인 서버의 주소를 응답한다.**
4. 로컬 DNS 서버는 최상위 레벨 도메인 서버에게 `www.example.com` 의 IP 주소를 질의한다.
5. **최상위 레벨 DNS 서버는 example.com을 관리하는 책임 DNS 서버의 주소를 응답한다.**
6. 로컬 DNS 서버는 책임 DNS 서버에게 `www.example.com`의 IP 주소를 질의한다.
7. **책임 DNS 서버는 `www.example.com`의 IP 주소를 응답한다.**
8. 로컬 DNS 서버는 컴퓨터에게 IP 주소를 응답한다.

<br />

# DNS 캐싱

DNS는 **지연 성능 향상**과 **네트워크 상 DNS 질의/응답 메시지 수를 줄이기** 위해 캐싱을 이용한다. DNS 서버가 DNS 응답을 받았을 때 로컬 메모리에 저장하여 캐싱한다. 동일한 DNS 질의를 받았을 때 메모리에 저장된 응답값을 반환하여 질의 사슬을 우회하도록 한다. 로컬 메모리에 응답값은 DNS 서버에서 설정된 캐시에 대한 TTL(Time to live, 데이터의 유효 기간) 값 동안 저장된다.

<br />

# DNS 레코드

DNS 서버(데이터베이스)는 호스트 네임과 IP 주소를 맵핑하기 위한 **자원 레코드(RR, Resource Record)를** 저장하며 DNS 질의에 대한 응답으로 해당 RR을 **메시지로 응답**한다.

## DNS RR

자원 레코드는 `Name` `Value` `Type` `TTL` 4개의 튜플로 이루어져 있다. `Name`과 `Value`의 의미는 `Type`값에 따라 달라지며 `TTL`은 RR의 유효 기간을 의미한다.

| Type 값 | Name 의미        | Value 의미                  | 예시(Name Value Type)                |
| ------- | ---------------- | --------------------------- | ------------------------------------ |
| A       | 호스트 네임      | IPv4 주소                   | relay1.bar.foo.com, 145.37.93.126, A |
| AAAA    | 호스트 네임      | IPv6 주소                   |                                      |
| NS      | 도메인           | 책임 DNS 서버의 호스트 네임 | foo.com, bar.foo.com NS              |
| CNAME   | 별칭 호스트 네임 | 정식 호스트 네임            | foo.com, relay1.bar.com, CNAME       |
| MX      | 메일 서버 별칭   | 호스트 네임                 | foo.com, mail.bar.foo.com, MX        |

## DNS 메시지

DNS 질의 및 응답 메시지는 아래의 포맷을 가진다.

![DNS Messages [https://electronicspost.com/dns-messages/](https://electronicspost.com/dns-messages/)](https://electronicspost.com/wp-content/uploads/2016/05/2.23.png)

DNS Messages [https://electronicspost.com/dns-messages/](https://electronicspost.com/dns-messages/)

### 헤더

처음 12바이트의 헤더 영역은 **식별자**, **플래그**와 **4개의 “개수” 필드**로 총 6개의 필드로 구성되어 있다. 식별자 필드는 질의를 식별하는 값으로 질의와 응답 간의 일치를 식별하는데 사용된다. 플래그 필드에는 여러 개의 플래그가 있다. 4개의 “개수” 필드는 헤더 다음 데이터 영역에서 오는 데이터 4가지 타입의 발생 횟수를 의미한다.

### 데이터

데이터 영역은 **질문, 답변, 책임, 추가 정보 영역**이 있다.

**DNS 질의/응답 메시지**에 담기는 **질문 영역**은 현재 질의에 대한 정보를 포함하고 있으며, 질의에 대한 RR `Name` `Type` 값이다.

**DNS 응답 메시지**에 담기는 **답변 영역**은 질의에 대한 RR 값이며, **책임 영역**과 **추가 정보 영역**은 DNS 서버에 관련된 정보가 담겨있다. 각각 다른 책임 서버의 RR 값과 다른 도움이 되는 RR 값이 담겨 있다.

`nslookup` 명령어로 DNS 질의와 응답 메시지를 살펴보자

```bash
> nslookup -debug www.naver.com
------------
Got answer:
    HEADER:
        opcode = QUERY, id = 1, rcode = NOERROR
        header flags:  response, want recursion, recursion avail.
        questions = 1,  answers = 1,  authority records = 0,  additional = 0

    QUESTIONS:
        1.63.126.168.in-addr.arpa, type = PTR, class = IN
    ANSWERS:
    ->  1.63.126.168.in-addr.ARPA
        name = kns.kornet.net
        ttl = 29675 (8 hours 14 mins 35 secs)

------------
서버:    kns.kornet.net
Address:  168.126.63.1

------------
Got answer:
    HEADER:
        opcode = QUERY, id = 2, rcode = NOERROR
        header flags:  response, want recursion, recursion avail.
        questions = 1,  answers = 3,  authority records = 0,  additional = 0

    QUESTIONS:
        www.naver.com, type = A, class = IN
    ANSWERS:
    ->  www.naver.com
        canonical name = www.naver.com.nheos.com
        ttl = 5622 (1 hour 33 mins 42 secs)
    ->  www.naver.com.nheos.com
        internet address = 223.130.195.200
        ttl = 178 (2 mins 58 secs)
    ->  www.naver.com.nheos.com
        internet address = 223.130.195.95
        ttl = 178 (2 mins 58 secs)

------------
권한 없는 응답: # 질의 Name을 관리하는 DNS 서버가 직접 응답하지 않고, 다른 DNS 서버에 캐싱된 값이 응답된 경우
------------
Got answer:
    HEADER:
        opcode = QUERY, id = 3, rcode = NOERROR
        header flags:  response, want recursion, recursion avail.
        questions = 1,  answers = 1,  authority records = 1,  additional = 0

    QUESTIONS:
        www.naver.com, type = AAAA, class = IN
    ANSWERS:
    ->  www.naver.com
        canonical name = www.naver.com.nheos.com
        ttl = 5622 (1 hour 33 mins 42 secs)
    AUTHORITY RECORDS:
    ->  nheos.com
        ttl = 51 (51 secs)
        primary name server = gns1.nheos.com
        responsible mail addr = hostmaster.nheos.com
        serial  = 2022093002
        refresh = 10800 (3 hours)
        retry   = 3600 (1 hour)
        expire  = 604800 (7 days)
        default TTL = 180 (3 mins)

------------
이름:    www.naver.com.nheos.com
Addresses:  223.130.195.200
          223.130.195.95
Aliases:  www.naver.com
```

<br />

# Reference

컴퓨터 네트워킹 하향식 접근 7판

[What is DNS? | How DNS works](https://www.cloudflare.com/learning/dns/what-is-dns/)

[DNS에 대한 설명(디테일하게….)](https://hwan-shell.tistory.com/320)

[DNS의 역할과 동작 수행 과정](https://dev-dain.tistory.com/45)

[DNS 재귀적 질의, 반복적 질의](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=nj0803&logNo=10175980049)

[DNS - Iterative vs recursive query](https://gaia.cs.umass.edu/kurose_ross/interactive/dns_query.php)

[DNS Query, DNS Answer   DNS 질의, DNS 응답](http://www.ktword.co.kr/test/view/view.php?m_temp1=2918)

[Resource Record (RR) TYPEs](https://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml#dns-parameters-4)

[DNS 메시지](http://www.ktword.co.kr/test/view/view.php?m_temp1=2251)

[DNS 질의 메시지 설명](https://openstory.tistory.com/45)

[DNS 응답 메시지 설명](https://openstory.tistory.com/46)
