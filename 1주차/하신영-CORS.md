# 🔍목차

[1.출처란?](#출처란)

[2.SOP ](#sop)

[3.CORS](#cors)

[4.CORS 시나리오](#cors-시나리오)

[5.만약 CORS를 사용하지 않는다면?](#만약-cors를-사용하지-않는다면)

[6.CORS 에러 해결 방법](#cors-에러-해결-방법)

<hr>

<br/><br/>

# CORS 에러!

![image](https://user-images.githubusercontent.com/79133602/161915110-b5c3f79f-bffc-4765-8640-d2cee4a5a406.png)

![image](https://user-images.githubusercontent.com/79133602/161915747-f9237397-f822-414a-83c0-eb7bf52890bf.png)


이 에러는 왜 뜬 걸까요?

내용을 보면 CORS 정책에 따라 다른 출처에 접근하지 못합니다 라고 써있습니다. 여기서 말하는 출처와 CORS 정책이란 무엇이고 왜 다른 출처에 접근하지 못하게 막았는지 알아보도록 하겠습니다.

<br/><br/>
<br/>




# 출처란?

서버의 위치를 의미하는 URL은 여러개의 구성요소로 이뤄져 있습니다. 

![image](https://user-images.githubusercontent.com/79133602/161909298-4d03dc5a-502a-4350-9013-90e102939ce7.png)


이중에서 Protocol, Host 그리고 Port까지를 출처라고 합니다. 보통 Port는 생략되기 때문에 예시로 든 위 URL에서 출처는 https://github.com 입니다.

<br/>

이제, 다음 표를 보면서 URL의 출처, 그리고 출처가 예시와 같은지 알아보겠습니다. 



| URL | 같은 출처  |  설명 |
|:-- | :--: | :-- |
| https://github.com/other?tab=other | O | 경로만 다르지 Protocol,Host, 그리고 생략된 Port가 일치 |
| https://github.com:8080 | ? | Port가 달라서 익스플로러 제외 다르다고 인식 |
| http://github.com | X | Protocol이 다름 |
| http://github.co.kr | X |  Host가 다름 |


<br/>

이 출처를 보고 브라우저는 리소스를 공유해도 될지 판단합니다. 판단 기준으로 다음 2가지 정책을 따르고 있습니다.

<br/><br/>
<br/>


# SOP 
> Same-Origin Policy

같은 출처끼리만 리소스를 공유하는 정책입니다. 출처가 다른 사이트 간 통신이 일어나는 경우 보안에 취약해질 수 밖에 없습니다. 리소스 공유 범위를 같은 출처로 제한하면 외부의 공격을 원천 봉쇄할 수 있기에 사용합니다.

하지만 웹은 오픈스페이스 환경에서 다른 출처의 리소스를 가져다 쓰는 일이 많기 때문에 예외 조항으로 CORS 정책을 두고 있습니다. 

<br/>
<br/>
<br/>


# CORS


> Cross-Origin Resource Sharing

다른 출처끼리 리소스를 공유할 수 있게 해주는 정책입니다. HTTP 헤더를 사용해, 다른 출처의 리소스를 요청하면 브라우저가 헤더의 내용을 보고 권한을 부여합니다. 

<br/>
<br/>


# Cross Origin Request 

> CORS 요청의 흐름


1. 브라우저는 헤더의 origin에 리소스를 달라고 요청하는 출처를 입력하고 이를   서버로 보냄

2. 서버가 헤더의 Access-Control-Allow-Origin에 접근허용 출처를 담아 브라우저에 보냄

3. 브라우저는 origin과 Access-Control-Allow-Origin을 비교한 뒤 응답이 유효한지 판단

<br/>




여기에 덧 붙여, 다음 세가지 시나리오에 따라 변경되기에 개발자는 자신의 요청이 어떤 시나리오에 해당되는지 알아야 에러를 고치기 쉽습니다.

<br/>
<br/>
<br/>


# CORS 시나리오

## 1. Simple Request

> 허락 없이도 다른 출처에 요청을 보냄

특정 조건을 만족하는 요청의 경우 브라우저의 허락이 없이 바로 다른 출처에 HTTP요청을 보낼 수 있습니다. 

![image](https://user-images.githubusercontent.com/79133602/162001039-6fc1f6c2-dcc8-4b2a-b8c2-fb535e32c4ea.png)


요청을 받은 서버는 브라우저에 Access-Control-Allow-Origin을 보내고, 그제서야 브라우저가 CORS 정책 위반여부를 판단합니다.


### ✔ 특정 조건

1. 요청의 method가 GET, POST, HEAD인 경우
2. Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width같은  지정된 기본 헤더를 사용
3. Content-Type의 경우 application/x-www-form-urlencoded,multipart/form-data, text/plain만 허용

<br/>


### 🤦‍♀️ 조건을  보시면 알겠지만... 

이 조건을 만족하기란 매우 어렵습니다... HTTP API가 대부분text/xml , application/json 로 이뤄졌고 헤더의 경우 명시된 내용보다 더 많은 헤더를 추가적으로 사용하고 있기 때문입니다. 

<br/>

즉, 그만큼 기본이고 간단한 요청이니 브라우저 넌 잠깐 쉬고 있으라는 뜻입니다. 

<br/>
<br/>
<br/>


## 2. Preflight Request
> 예비요청을 보고 안전하면 본 요청을 보냄

우리가 웹 어플리케이션을 개발할 때 가장 자주 겪는 시나리오 입니다. 
간단한 요청이 아닐 경우 브라우저는 서버에 요청을 하기 전 preflight라는 예비요청을 먼저 OPTIONS 메소드를 사용해 전송합니다. 

<br/>

![image](https://user-images.githubusercontent.com/79133602/162000507-d8b2725a-ea41-423c-b579-542156b12dac.png)



preflight엔 대략적으로 다음과 같은 내용이 들어갑니다.


```
Origin: 다른 출처의 리소스를 요청하는 녀석의 출처
Aceess-Control-Request-Method: 요청의 메소드 GET,...
Access-Control-Request-Headers: 요청의 추가 헤더

```
<br/>


서버는 preflight에 응답해 헤더에 다음과 같은 정보를 넣어 보냅니다.

```
Aceess-Control-Allow-Origin: 서버측 허가 출처
Aceess-Control-Allow-Method: 서버측 허가 메소드
Access-Control-Allow-Headers: 서버측 허가 헤더
Access-Control-Max-Age: preflight 응답 캐시 시간

//응답 캐시 시간동안 응답이 스킵됩니다. 이를 통해 초단위로 업뎃되는 데이터의 경우 필요한 순간만 요청을 해 리소스 낭비를 줄일 수 있습니다. 
```


브라우저는 서버측 허가 출처 중에 현재 리소스 요청을 날린 출처가 있는지 확인하고, 있다면 리소스 접근을 허락하고 없다면 CORS 에러를 띄웁니다. 

<br/>
<br/>
<br/>


## ⚠ 주의: 예비 요청 성공해도 CORS 에러?

> **preflight 성공 !== CORS 통과**

중요한건 서버에서 브라우저에 보내는 응답헤더입니다. 예비 요청의 성공은 요청이 서버에 전달됐음 거기까지입니다!

예비 요청 이후 본요청에 대한 응답으로 **Aceess-Control-Request-Origin에** 허가된 출처를 알려주는데, 여기 **origin이 없다면?** 


**CORS 에러입니다!**

<br/><br/>
<br/>



## 3. Credentialed Request 
> **인증된 요청 사용**

다른 출처간 통신에서 보안을 강화하고 싶을 때 사용하는 시나리오입니다.

기본적으로 브라우저가 제공하는 API는 별도의 옵션 없이 브라우저의 쿠키나 인증과 관련된 헤더를 요청에 함부로 담지 않는데 CredentialS 옵션을 사용하면 가능합니다. 

<br/>


| 옵션 | 설명 |
|:-- | :-- |
| same-origin(default) | 같은 출처 간 요청에만 인증 정보를 담을 수 있다. |
| include | 모든 요청에 인증 정보를 담을 수 있다. |
| omit | 모든 요청에 인증 정보를 담지 않는다. |

<br/>


위 옵션을 넣어서 브라우저에 다른 출처 리소스 접근 권한을 요청하면 이제 더 엄격한 심사를 받게 됩니다. 

<br/>
<br/>


## 👀 예시

<br/>

```
//요청시 credentails 옵션
fetch('다른 출처',{ credentials: 'include' })
```
서버가 모든 출처에게 리소스 접근을 허가한다고 해도 모든 요청에 인증 정보를 담은 경우 브라우저는 CORS 에러를 내보냅니다.
```
//서버의 응답
Access-Control-Allow-Origin: * 
```

왜냐하면 인증 정보를 아무 요청에 줘버릴 수 없기 때문입니다. 

이경우 CORS를 피하기 위해선 다음과 같이 서버의 응답헤더를 바꿔줘야 합니다.
```
Access-Control-Allow-Origin: * 대신 특정 URL
Access-Control-Allow-Credentials: true
```

<br/>
<br/>
<br/>




# 만약 CORS를 사용하지 않는다면?


어짜피 정해진 서버하고만 통신을 할텐데 왜 그렇게 귀찮은 정책을 만들어서 개발자들을 피곤하게 할까요?

> 사용자 공격에 취약한 웹

브라우저 개발자 도구를 열어 보면 웹의 코드를 볼 수 있습니다. 아무리 스크립트를 난독화했다고 해도 사람이 읽을 수 있는 수준이고, 콘솔창을 통해 직접 스크립트 조작을 할 수 도 있습니다.

그러다보니 나쁜 맘을 먹으면 CSRF(브라우저에 이상한 요청 넣기), XSS(브라우저에 저장된 사용자 정보 탈취) 같은 공격을 할 수 있고, 이를 방지하기 위해 저런 정책이 필요합니다


<br/>
<br/>
<br/>


# CORS 에러 해결 방법

그러니 우리는 반드시 CORS 정책을 따라 다른 출처의 리소스에 접근해야 합니다. 만약 에러를 만났다면 다음과 같은 방법으로 해결하도록 합니다. 

<br/>

## 1. proxy 설정

> 프론트엔드 개발자

사실 CORS를 가장 마주치는 사람은 프론트엔드 개발자입니다. 백엔드 서버와 프론트엔드 서버를 달리해서 작업을 하는데 ajax 요청을 보내면 바로 CORS에러가 뜹니다. 

```
axios.delete('http://localhost:8080/api/delete/1')
```

그럴 땐 백엔드 서버를 프록시 서버로 등록해 CORS를 우회하면 됩니다. 


<br/>
<br/>

다음과 같은 서버를 가졌다고 가정하고 예를 들어보겠습니다.

```
백엔드 서버: http://localhost:8080
프론트 서버: http://localhost:3000

//axios의 path는 미들웨어 프록시 뒤 url만 입력
axios.delete('/api/delete/1')
```

<br/>

## 1.1 webpack-dev-server



웹팩과 webpack-dev-server 라이브러리가 제공하는 프록시 기능을 사용하기 위해 다음 코드를 입력합니다.

```
module.exports = {
    devServer: {
        proxy: {
            '/api': { //api로 시작하는 url은 아래 출처로 연결시켜준다.
            target: 'http://localhost:8080',
                pathRewrite: { '^/api': '' }
            }
        }
    }
}
```

그러면 /api로 시작하는 url로 가는 요청을 보낼 때 브라우저는 http://localhost:3000이 보낸 것으로 인지해 CORS를 따지지 않고 웹팩은 브라우저 모르게 해당 요청을 http://localhost:8080로 보냅니다.

<br/><br/>

## 1.2 http-proxy-middleware

만약 webpack-dev-server가 아니라 node 서버를 사용하고 있다면 http-proxy-middleware 라이브러리를 사용해 프록시 설정을 합니다. 

<br/>

src/SetupProxy.js 파일을 만들고 아래처럼 코드를 입력하면 끝입니다!

```
const { createProxyMiddleware } = require('http-proxy-middleware');

module.exports = function(app) {
  app.use(
    '/api',
    createProxyMiddleware({
      target: 'http://localhost:8080',
      changeOrigin: true
    })
  )
}
```

<br/><br/>

## 1.3 React package.json

리액트 CRA를 사용하고 있는 경우 더욱 쉽게 프록시 설정을 할 수 있습니다. 

package.json 파일에 객체형태로 입력하면 끝입니다.

```
"proxy": {
    "/api":{
        "target": "http://localhost:8080"
    }
}
```

<br/><br/>

## 2. 보안 설정 수정

> 백엔드 개발자

이 모든 프론트엔드의 노력에도 여전히 좌절할 에러의 산이 남아있습니다.

<br/>

백엔드 http://localhost:8080 서버가 delete 메소드를 금지한다면? 

<br/>

또다시... 우리는 에러를 만나게 됩니다. 


그래서 백엔드 개발자는 원만한 협업을 위해 서버 보안 설정을 바꿔줘야 합니다. 

<br/><br/>

## 👀 백엔드 spring SecurityConfig 설정

<br/>

http://localhost:8080이 자바 스프링으로 만든 백엔드 서버라고 가정해보겠습니다.

SecurityConfig 서버 보안 설정 파일을 보면

```
    public CorsConfigurationSource corsConfigurationSource(){
        CorsConfiguration config = new CorsConfiguration();

        config.addAllowedOrigin("http://localhost:3000");

        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
    }
```

 현재 프론트 서버의 접근을 허락하지만... 기본 get,post를 제외한 메소드는 받아주지 않습니다. 그리고 추가적인 헤더, credentials도 받지 않기에 해당 내용을 사용하고 싶다면 아래와 같이 코드를 바꿔줘야 합니다. 

```
    public CorsConfigurationSource corsConfigurationSource(){
        CorsConfiguration config = new CorsConfiguration();
        config.addAllowedOrigin("http://localhost:3000");
        config.addAllowedMethod("*");
        config.addAllowedHeader("*");
        config.setAllowCredentials(true);

        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
    }
```

<br/><br/>
 
 ## ⚠ 주의

 > **config.AllowedOrigin("*") 안돼!**

스프링에서 config.AllowedOrigin("*")하면 Aceess-Control-Allow-Origin, 즉 모든 출처가 서버에 접근할 수 있게 됩니다. 보안적으로 심각한 문제가 있기 때문에 만약 접근 허가가 필요한 출처가 있다면 귀찮더라도 직접 입력해야 합니다!

<br/><br/><br/><br/>

# 참고

💻 [MDN Web Docs : CORS](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)

💻 [CORS는 왜 이렇게 우리를 힘들게 하는걸까?](https://evan-moon.github.io/2020/05/21/about-cors/)

💻 [나를 너무나 힘들게 했던 CORS 에러 해결하기](https://xiubindev.tistory.com/115)
