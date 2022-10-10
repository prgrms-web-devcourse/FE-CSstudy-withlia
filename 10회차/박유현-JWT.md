# 들어가기 전

## HTTP

서버와 클라이언트가 데이터를 주고 받기 위한 프로토콜로 **비연결성** 및 **무상태성**이라는 특성을 가진다. 따라서 서버는 클라이언트에 대한 상태 정보 및 통신의 상태를 알 수 없고, 클라이언트를 식별할 수 없다.

## 쿠키, Cookie

쿠키는 HTTP의 **무상태성을 해결**하고자 나온 것으로 서버가 클라이언트에 **정보를 저장**하고자 웹 브라우저에 전송하는 작은 데이터를 의미한다. 쿠키는 주로 세션 관리, 사용자 설정, 트래킹 목적으로 사용된다.

## 세션, Session

세션은 쿠키를 기반으로, **클라이언트의 정보를 서버에 저장하는 방법**과 **클라이언트가 서버와 연결을 시작하는 순간부터 끝내는 시점까지의 시간대**를 의미한다. 주로 서버에서 클라이언트를 식별하기 위해 **세션 ID**를 부여하고 쿠키에 저장한다.

## 인증, Authentication

인증은 **사용자의 신원을 검증**하는 것을 의미하며 주로 로그인, OTP(One Time Password)를 통해 검증한다.

## 인가, Authorization

인가(권한 부여)란 신원이 검증된 사용자에게 **특정 리소나 기능에 접근할 수 있는 권한을 부여**하는 것을 의미한다. 서버와 클라이언트 관점에서 보면, 클라이언트가 요청을 보냈을 때 해당 **요청을 실행할 수 있는 권한이 있는지를 확인**하는 과정이다.

## 세션 기반 인증

세션을 통해 인증하는 방식으로 주로 아래의 절차를 따른다.

1. 클라이언트가 ID/PW 로그인을 한다.
2. 서버에서 데이터베이스를 통해 ID/PW를 검증한다.
3. 서버에서 클라이언트의 **세션 ID를 발급한 뒤 Session storage에 저장**한다.
4. 서버에서 **쿠키에 발급한 세션 ID를 담아** 클라이언트에게 응답한다.
5. 이후 클라이언트의 요청이 들어올 때마다 서버는 쿠키에 담긴 **세션 ID를 검증**한 후 응답한다.

## 토큰 기반 인증

세션 기반 인증의 한계점을 보완하고자 나온 인증 방식으로 사용자 정보를 서버가 아닌 **클라이언트에 저장하는 무상태성 방식**이다. 인증하는 방식은 주로 아래의 절차를 따른다.

1. 클라이언트가 ID/PW 로그인을 한다.
2. 서버에서 데이터베이스를 통해 ID/PW를 검증한다.
3. 서버에서 인증에 필요한 클라이언트의 정보로 토큰을 발행한 뒤 클라이언트에게 전달한다.
4. 클라이언트는 토큰을 웹 저장소에 보관하여, 이후 인증이 필요한 요청 시 토큰을 저장소에서 꺼내 HTTP Authorization header에 담아 요청한다. 서버에서는 토큰을 검증한 뒤 응답한다.

<br />

# JWT, JSON Web Token

JWT는 **JSON 객체**로 **안전**하게 **정보**들을 교환하는 방법이며, 현재 **토큰 기반 인증**에서 ****가장 많이 사용되는 **인터넷 표준**이다. JWT는 헤더, 페이로드, 서명 3개의 파트로 구성되어 있고, 각 파트는 JSON 객체이다. 서버와 클라이언트가 토큰을 전달할 때에는 각 파트를 base64로 인코딩한 뒤 ‘.’을 구분자로 `헤더.페이로드.서명`으로 변환된 **문자열**로 전송한다.

JWT에는 Access Token, Refresh Token 2가지 종류가 있다. Access Token은 인증에 사용되는 토큰이며, Refresh Token은 Access Token을 발급받는 토큰이다.

<br />

# JWT 구조

### 헤더

헤더에는 보편적으로 2개의 키와 값으로 구성된다. 토큰의 타입을 명시하는 `typ`과 서명 알고리즘을 명시하는 `alg` 이다.

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

## 페이로드

페이로드는 클레임들을 담는 파트이다. 여기서 **클레임이란 인증에 필요한 데이터나 추가 데이터를 키와 값의 형태**로 나타낸 것을 말한다. 클레임에는 크게 **registered, public, private** 세 가지 타입이 있다.

### 1. Registered claims

표준에 등록된, 토큰에 대한 정보를 담는 클레임으로 총 7개가 있다.

| 클레임 이름 | 클레임 값 |
| --- | --- |
| iss (issuer) | JWT를 발행한 주체를 나타낸다. |
| sub (subject) | JWT의 주체를 나타낸다. |
| aud (audience) | JWT 발급의 사용자를 나타낸다. |
| exp (expiration time) | JWT의 만료 날짜/시간을 나타낸다. |
| nbf (not before) | JWT의 사용을 허용하는 날짜/시간을 나타낸다. |
| iat (issued at) | JWT가 발행된 날짜/시간을 나타낸다. |
| jti (JWT id) | JWT에 대한 고유 식별자를 나타낸다. |

### 2. Public claims

사용자가 정의할 수 있는 클레임을 뜻하며, 다른 클레임 이름과 겹치지 않도록 주의해야 한다.

### 3. Private claims

서버와 클라이언트 간 데이터를 공유할 때 정의하는 클레임을 뜻하며, 동일하게 클레임 이름이 겹치지 않도록 주의해야 한다.

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

## 서명

서명은 서버에서 토큰을 발행할 때 **토큰의 base64로 인코딩된 `헤더.페이로드` 값**과 **시크릿 키**를 이용하여 (헤더에 `alg` 클레임에 명시된) 서명 알고리즘으로 서명한 값이 담기는 파트다.  즉 `alg` 값이 `HS256` 이라면, 서명에는 아래의 값이 담긴다.

```jsx
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secretkey)
```

서버의 시크릿키로 서명이 되었기에 JWT의 유효성을 검증할 수 있게 된다.


![image](https://user-images.githubusercontent.com/96400112/194753922-d9f63207-8b75-4717-9d4b-cdab2528f485.png)

![image](https://user-images.githubusercontent.com/96400112/194753944-e4af34bd-a198-42cf-9578-23ab4fdaf982.png)





링크오션 Refresh Token

<br />

# JWT 장단점

## 장점

1. 토큰 기반 인증이므로 서버에서는 인증 정보에 대한 별도의 저장소가 필요없고 stateless해진다.
2. JWT이 유효한 토큰인지 검증할 수 있는 값들을 자체적으로 담고 있다.
3. JSON 형식이기에 다른 토큰 방식보다 데이터의 크기가 작고 간결하다. (세션 기반 인증보다는 크다)

## 단점

1. **페이로드에 담긴 데이터**는 암호화되지 않고 base64로 인코딩된 값으로 **누구나 접근**할 수 있다. 그러므로 민감한 정보를 담아서는 안 된다.
2. 서명에 사용한 서버의 **시크릿 키가 유출**되면 이를 이용해 페이로드 값을 **변조하여 유효한 토큰을 생성**할 수 있다.
3. 무상태성이므로 악성 유저가 JWT를 갈취하여 요청해도 서버에서는 식별할 수 없고 해당 **토큰을 무효화할 수 없다.**
4. 무상태성이므로 서버에서 클라이언트의 접속을 강제로 끊을 수 없다.

<br />

# JWT 보안 전략

JWT의 보안 수준을 높이기 위한 여러 가지 방법이 있다.

## **토큰의 만료기간을 짧게 설정하기**

토큰을 갈취당해도 만료 기간을 짧게하여 피해를 최소화할 수 있지만 만료될 때마다 사용자가 다시 로그인해야 하는 불편함이 생긴다.

## **Refresh Token**

위 방법과 더불어 Access Token을 재발급할 수 있는 만료 기간이 긴 Refresh Token도 같이 발행하는 방법이다. Access Token이 만료되어도 Refresh Token으로 다시 재발급받으면 되므로 로그인을 다시 하지 않아도 된다. 하지만 이 역시 Refresh Token을 갈취당하면 소용없다.

## **Refresh Token Rotation 기법**

Refresh Token 갈취에 대한 해결 방법으로 Refresh Token으로 Access Token을 재발급받을 때 Refresh Token 또한 재발급하여 Refresh Token으로 단 한 번만 Access Token을 재발급 받을 수 있게 한다. Refresh Token의 **재사용**은 탈취로 간주하고 이전 Refresh Token들을 **폐기**한다. 시나리오를 통해 살펴보자.

1. 🎅🏼 **사용자**가 로그인하여 Access Token(1)과 Refresh Token(1)을 발급받았다.
2. 😈 **악성 사용자**가 🎅🏼 **사용자**의 Refresh Token(1)을 갈취했다.
3. 🎅🏼 **사용자의** Access Token(1)이 만료되어 Refresh Token(1)으로 서버에 재발급을 요청하여  Access Token(2)와 Refresh Token(2)을 발급받았다.
4. 😈 **악성 사용자**가 Refresh Token(1)로 서버에 Access Token 재발급을 요청한다.
5. 서버에서는 Refresh Token(1)의 재사용을 감지하고 갈취되었음을 알 수 있다. Refresh Token(1)과 Refresh Token(2) 모두 유효하지 않은 토큰으로 판단한다.
6. 이후 🎅🏼 **사용자와** 😈 **악성 사용자의** Refresh Token으로는 Access Token을 재발급받을 수 없다. 즉 🎅🏼 **사용자는** 다시 로그인하여 유효한 Refresh Token을 발급받아야 한다.

해당 방법은 Refresh Token들을 서버 DB에 저장해야 하며 조회 비용이 생기는 단점이 있다.

⇒ 여러 방법을 이용해 보안 수준을 높여도 역시나 Access Token을 갈취당하면 만료기간 내에는 무효화시킬 수 없다는 문제와 단점들이 존재하기에, 프로덕션에서는 JWT 대신 세션 기반 인증을 사용한다고 한다.

<br />

# 참고

세션 기반 인증 vs. 토큰 기반 인증과 JWT 보안 [https://blog.lgcns.com/2687](https://blog.lgcns.com/2687)

인증 방식: Cookie & Session vs JWT [https://tecoble.techcourse.co.kr/post/2021-05-22-cookie-session-jwt/](https://tecoble.techcourse.co.kr/post/2021-05-22-cookie-session-jwt/)

Refresh Token 안전하게 사용하기 [https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/](https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/)
