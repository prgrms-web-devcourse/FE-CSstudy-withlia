# 🔍목차

[1.OAuth란?](#oauth란?)

[2.OAuth의 탄생배경과 변화](#oauth의-탄생배경과-변화)

[3.OAuth 구성](#oauth-구성)

[4.OAuth 인증 과정](#oauth-인증-과정)

<hr>

# OAuth란?

> 접근 권한을 위임해주는 개방형 표준 프로토콜

여러분은 사이트에 가입하지 않고 다른 사이트의 계정을 사용해 로그인을 해 본 경험이 있을 것 입니다.

![image](https://user-images.githubusercontent.com/79133602/167872843-2d8e28b7-d788-448e-af1c-87748ff3a1dc.png)

그 때 사용되는 프로토콜이 OAuth입니다.

간단히 로그인 할 수 있단 장점 외엔도 다른 사이트의 기능을 사용할 수 있는데 예로 연동된 구글계정으로 로그인 후 구글 캘린더 정보를 가져와 해당 사이트에서 사용하는 경우가 그렇습니다.

<br/><br/>

# OAuth의 탄생배경과 변화

> 인증정보를 다른 사이트에 넘겨주면 위험해!

OAuth를 사용하기 전엔 다른 사이트의 리소스를 가져오기 위해 인증정보를 해당 사이트에 저장해 사용했습니다. 이 경우 사용자와 다른 사이트는 인증정보를 신뢰해도 될지 모를 사이트에 넘겨야 하니 불안했고, 해당 사이트는 인증정보를 관리해야 되니 부담이 됐습니다.

위 문제를 해결하기 위해 2006년 트위터 개발자, Gnolia 개발자가 OAuth를 만들었습니다.

하지만 초기버전 OAuth은 인증토큰이 만료되지 않아 보안에 결함이 있었고, 구현이 복잡했기에 그다음 버전 OAuth 2.0이 등장하게 됩니다.

<br/>

> OAuth 2.0

OAuth 2.0은 이전 버전과 달리 다음과 같은 특징을 갖고 있어서 더욱 편리하고 안전합니다.

1. https가 암호화를 진행
2. 다양한 인증방식 지원
3. api 인증서버 분리 가능
4. access token 인증토큰에 만료기간 존재

<br/>

> 간략히 보는 두 버전의 차이점

|            | OAuth 1.0                     | OAuth2.0                                             |
| :--------: | :---------------------------- | :--------------------------------------------------- |
|   참여자   | 사용자, 소비자, 서비스 제공자 | 리소스 소유자, 클라이언트, 권한 서버, 리소스 서버    |
|    토큰    | Request Token, Access Token   | Refresh Token, Access Token                          |
|  유효기간  | Access Token 유효기간 없음    | Access Token 유효기간 존재, Refresh Token으로 재발급 |
| 클라이언트 | 웹                            | 웹, 앱                                               |

<br/><br/>

# OAuth 구성

|         요소         | 설명                                                       |
| :------------------: | :--------------------------------------------------------- |
|    Resource owner    | 유저, 사용자                                               |
|        Client        | 애플리케이션 서버(개인정보 달라고 하는 서버)               |
| Authorization Server | 개인 정보 접근 권한을 부여하는 서버 (개인 정보 주는 서버 ) |
|   Resource Server    | 유저의 개인 정보를 갖고 있는 회사 서버                     |
|     Access Token     | 개인정보 접근 권한 증명서                                  |
|    Refresh Token     | Access Token을 재발급 받기 위한 증명서                     |

<br/><br/>

# OAuth 인증 과정

![image](https://user-images.githubusercontent.com/79133602/167851004-394bec59-ae5c-4b11-8890-f1e84621b119.png)

예로 PAYCO OAuth 인증과정을 살펴보도록 하겠습니다. 만약 특정에 사이트에 회원가입 후 로그인을 하는 대신 PAYCO 아이디와 비밀번호로 로그인을 한다면 다음과 같은 과정을 거칩니다.

![image](https://user-images.githubusercontent.com/79133602/167874835-fd4f451b-0a6c-48d5-999e-f0452d029fd0.png)

1. Resource owner가 client에 PAYCO 로그인 요청을 합니다.

2. client가 Resource owner 대신 PAYCO에 로그인을 시도합니다.

<hr>

2번 과정 이후 Resource Server의 응답 헤더를 보면 여러 속성들이 있는데 그중에서 Location이 중요합니다.

```
Location: https://www.resourceServer.com/?client_id=1&scope=A&redirect_uri=https://client/callback
```

**Location:** 해당 값에 PAYCO로그인 링크가 들어있어서 사용자가 직접 PAYCO에 들어가지 않아도 해당 창에서 로그인할 수 있습니다.

Location안에는 Resource Server와 공유할 정보들이 담겨있는데 다음과 같습니다.

**client_id:** 사용자의 id로 Resource Owner에게 알립니다.

**scope:** 사용자가 이용할 Resource Server의 API 서비스입니다. 구글로 로그인시, 구글 캘린더를 scope로 지정하면 해당 사이트에서 구글 캘린더 정보를 가져와 사용할 수 있습니다.

**redirect_uri:** 응답을 보낼 때 리다이렉트해줄 링크

<hr>

![image](https://user-images.githubusercontent.com/79133602/167875146-c13bcab1-3cb6-4ba3-90a7-c58879894400.png)

3. PAYCO는 client를 통해 Resource owner의 ID,PW를 받아옵니다.

<hr>

이 때 클라이언트에게 scope에 해당되는 권한을 부여할 것인지 확인을 받습니다. 만약 허락한다면 해당 개인정보가 client에게 제공됩니다.

<hr>

![image](https://user-images.githubusercontent.com/79133602/167875693-930a5a55-6cfa-4a86-8cfb-0057312fdac7.png)

5. 허가를 누른 뒤 해당 ID,PW로 resource가 있다면 PAYCO는 Resource owner에게 Authorization Code를 발급합니다.

6. PAYCO에서 리다이렉트 메세지로 Authorization code를 Resource owner에게 보냈기 때문에 사용자는 직접 페이지 이동을 할 필요 없이 PAYCO에서 Client로 바로 이동할 수 있습니다.

![image](https://user-images.githubusercontent.com/79133602/167876180-af243f6d-c8a8-4e5e-a5a9-455ff6d98999.png)

7. 이동 후 Authorization code를 받은 Client는 해당 코드를 PAYCO에 보내 Access Token을 요청합니다.

8. Authorization code가 유효하다면 PAYCO는 Access Token과 Refresh Token을 client에 보냅니다.

9. Token을 받고 나서 client는 사용자에게 인증완료, 로그인 성공을 알립니다.

![image](https://user-images.githubusercontent.com/79133602/167876356-8a16c48c-ee9e-4d7f-9f2e-58b570676f0e.png)

10. 이제 사용자가 PAYCO에서 제공하는 기능을 API로 요청한다면 client가 Access Token을 사용해 해당 기능을 가져옵니다.

11. token에 문제가 없다면 resource server는 서비스를 제공합니다. 다만, token이 문제가 없어도 만료된 경우 에러를 출력하기 때문에 그 땐 재로그인을 해줘야 합니다.

## 전체적인 흐름

![image](https://user-images.githubusercontent.com/79133602/167876551-34784769-65af-4b6a-be60-8918411be8e7.png)

<br/><br/>

## ⚠ 주의: Access Token 만료

Access Token은 보통 만료 기간이 짧은데 이를 모르고 만료된 Access Token을 사용해 API 요청을 하면 401에러가 발생합니다. 만약 그럴 때마다 재 로그인을 해야 된다면 번거롭겠죠.

그래서 Resource Server는 Access Token을 발급할 때 Refresh Token을 함께 제공합니다. Client는 Access Token으로 API를 호출하다가 만료가 되서 에러가 발생하면 Refresh Token을 써서 새로운 Access Token을 발급합니다. 이때 Resource Owner가 재로그인처럼 뭔가를 입력할 필요가 없고 자동으로 발급이 되기 때문에 편리합니다.

물론 Refresh Token도 유효기간이 있지만 기간이 깁니다. 여기서 왜 Refresh Token을 Access Token 대신 쓸 수 없는지 궁금해하실 수도 있는데, Resource Token은 리소스 서버에선 전송되지 않습니다. 처음부터 Access Token을 재발급하는데 사용할 목적으로 만들어진 토큰이고 Access Token의 만료기간이 짧은 이유가 보안을 위해서란 점을 상기해 보면 당연합니다.

<br/><br/><br/><br/>

# 참고

💻 [OAuth 개념 정리](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-OAuth-20-%EA%B0%9C%EB%85%90-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC)

💻 [payco 개발자 센터](https://developers.payco.com/guide)

💻 [OAuth 개념 및 동작 방식 이해하기](https://tecoble.techcourse.co.kr/post/2021-07-10-understanding-oauth/)

💻 [Showerbugs](https://showerbugs.github.io/2017-11-16/OAuth-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C)

💻 [[OAuth 2.0] OAuth란?](https://velog.io/@undefcat/OAuth-2.0-%EA%B0%84%EB%8B%A8%EC%A0%95%EB%A6%AC)

💻 [테드의 기술블로그 ⛏](https://hwannny.tistory.com/92)
