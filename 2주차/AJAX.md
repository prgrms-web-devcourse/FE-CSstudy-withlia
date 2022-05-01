# 1. 과거 웹사이트

과거 웹사이트는 지금과는 달리 정적인 HTML만을 사용했으며, 페이지간 이동을 위해서 브라우저를 새로고침을 해야 했습니다.

![traditional-webpage-lifecycle.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0212dc7d-6926-4c95-9b48-45df34c78fce/traditional-webpage-lifecycle.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220501%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220501T162143Z&X-Amz-Expires=86400&X-Amz-Signature=02af1b8a0db406ff648bd61766fb643197063cfbd2724ab648a57d164bd90705&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22traditional-webpage-lifecycle.png%22&x-id=GetObject)

<br/>

즉, 거의 내용이 바뀌지 않는 페이지라도 모든 페이지를 다시 서버에게 요청한 후 페이지를 반영해야 했습니다.

<br/>

이러한 방식은 간단한 페이지라면 크게 문제 될 것은 없었겠지만, 복잡한 응용 페이지의 경우 페이지 인터렉션을 위한 추가적인 지연시간을 야기했습니다.

<br/>

더 나아가 모든 DOM이 다시 그려지기 때문에 브라우저는 전체 HTML 콘텐츠를 DOM 트리로 다시 파싱하는 작업을 매번 거쳐야 했습니다.

<br/>

![화면-기록-2022-04-28-오전-2.52.14.gif](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b68cfb59-1ec4-40dc-a0b7-8728266ed9e5/%E1%84%92%E1%85%AA%E1%84%86%E1%85%A7%E1%86%AB-%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%E1%86%A8-2022-04-28-%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB-2.52.14.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220501%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220501T162201Z&X-Amz-Expires=86400&X-Amz-Signature=97c4b564272b20516970cb88f806f638286a1a2c9df54082771cea5fabfe021d&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2592%25E1%2585%25AA%25E1%2584%2586%25E1%2585%25A7%25E1%2586%25AB-%25E1%2584%2580%25E1%2585%25B5%25E1%2584%2585%25E1%2585%25A9%25E1%2586%25A8-2022-04-28-%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB-2.52.14.gif%22&x-id=GetObject)

<br/>
이는 과거 웹 페이지의 예시입니다.

<br/>
위에서도 언급했다시피, 페이지를 이동할 때마다 새로운 HTML과 페이지 구성을 위한 것을 받아오는 것을 볼 수 있습니다.

<br/>

그래서 이를 조금이나마 해결하기 위해 `iframe`이라는 새로운 방법이 등장했습니다.

<br/>

# 2. iframe

일반적으로 `iframe` 태그는 HTML 페이지 안에서 다른 웹 페이지를 삽입할 수 있도록 하는 태그입니다.

<br/>

때문에, 브라우저의 역할 없이도 요청을 통해 웹 페이지 안에서 새로운 페이지를 보여줄 수 있도록 하였습니다.

<br/>

이는 클라이언트와 서버 간의 비동기 통신의 시초라고 볼 수 있습니다.

<br/>

`iframe` 은 백그라운드에서 요청하기 때문에 사용자는 브라우저의 새로고침 없이, 변경된 페이지를 볼 수 있었습니다.

<br/>

`iframe` 의 사용 예시입니다.

<br/>

```html
<!-- index.html -->
<body>
    <form action="hello.html" target="content">
        <input type="text" />
        <button type="submit">입력</button>
    </form>
    <iframe name="content" frameborder="0"></iframe>
</body>

<!-- hello.html -->

<body>
    <h1>hello</h1>
</body>
```

<br/>

이와 같이 `index.html` 에서 `form` 테그에 Submit 이벤트가 발생하였을 때 `hello.html` 을 불러오는 작업을 수행하게 됩니다.

<br/>

![화면-기록-2022-04-30-오후-1.57.20.gif](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/42538f7d-a915-445e-8c39-a58044008227/%E1%84%92%E1%85%AA%E1%84%86%E1%85%A7%E1%86%AB-%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%E1%86%A8-2022-04-30-%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE-1.57.20.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220501%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220501T162243Z&X-Amz-Expires=86400&X-Amz-Signature=2313516151461bd22fd8a1b742839504bbeb206468a2dea06f97b9d9178f4d20&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2592%25E1%2585%25AA%25E1%2584%2586%25E1%2585%25A7%25E1%2586%25AB-%25E1%2584%2580%25E1%2585%25B5%25E1%2584%2585%25E1%2585%25A9%25E1%2586%25A8-2022-04-30-%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE-1.57.20.gif%22&x-id=GetObject)

<br/>

위 그림과 같이 form 태그가 제출되면 없었던 `hello.html` 이 `iframe` 에 삽입된 것을 볼 수 있습니다.

<br/>

그러나, 기술이 점차 발전하게 되면서 응용 프로그램은 더욱 복잡해져, `iframe` 기술은 모든 요구 사항에 대응하기에는 어려움이 따랐습니다.

<br/>

이는 개발자가 요청에서 헤더를 수동으로 설정과 가져오는 작업을 못 하는 등.. 많은 제약이 따르게 되었습니다.

<br/>

이와 같은 이유로 `iframe` 은 충분하게 좋은 대안이 되었지만, 개발자들은 더 좋은 방법을 찾기 시작했습니다.

<br/>

# 3. ActiveXObject()와 XMLHTTP의 지원

2000년에, Microsoft에서 브라우저가 HTTP 요청을 자바스크립트를 사용해 비동기 방식으로 보내고 응답을 쉽게 처리할 수 있도록 하기 위한 작업을 시작하였습니다.

<br/>

이를 통해 MS는 IE5 부터 `ActiveXObject()` 를 추가하였으며, 이를 통해 `XMLHTTP`를 지원하기 위한 MSXML 라이브러리가 생겨났으며, 이 `XMLHTTP`는 현재 웹 표준을 이끈 엄청난 아이디어라고 합니다.

<br/>

하지만, 그 당시에는 이 엄청난 잠재력에 대해 아는 사람은 거의 없었기에, 많은 주목을 받지는 못하였다고 합니다.

<br/>

# 4. XMLHttpRequest()의 탄생

이후, 모든 브라우저들은 이 기능을 각 브라우저에서도 동작하도록 추가하기 제정하기 위한 움직임이 생겨났으며, Chrome, Firefox, Opera, Safari는 `XMLHttpRequest`를 적용하였습니다.

<br/>

> IE에서는 IE7에서야 `XMLHttpRequest()` 가 추가 되었음에도 불구하고 여전히 `ActiveXObject()` 를 지원하고 있었습니다.

> Microsoft `ActiveXObject()` 를 Edge 브라우저에서 삭제되었으며, `XMLHttpRquest` 만 제공하게 되었습니다.

<br/>

![Google-Maps-Beta1.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4d8f629f-ed6d-43ee-97b2-7a513987852d/Google-Maps-Beta1.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220501%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220501T162309Z&X-Amz-Expires=86400&X-Amz-Signature=6fe6a8c506df064f6397ab7466dd4327c14250030de10e712d08340124e53eab&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Google-Maps-Beta1.png%22&x-id=GetObject)

<br/>

그렇게 그다지 주목받지 않았던 이 기술은 2005년 Google은 Gmail, Google Maps를 출시되면서 주목받기 시작했습니다.

<br/>

이 두 개의 애플리케이션은 `XMLHttpRequest()` 를 통하여 더욱 높은 User Interaction과 풍부한 사용자 경험을 제공하게 되었습니다.

<br/>

추가로 위와 동시에 Google은 검색 엔진에 사용자가 검색하는 도중에 일치하는 추천 글자를 보여주는 Google Suggest를 출시하였고 이 또한 `XMLHttpRequest()` 가 사용되었습니다.

<br/>

# 5. AJAX의 탄생

점차 이 기술은 다른 기술과 잘 융합되었으며, 웹 개발을 위한 단일 단위로 사용되기 시작하였습니다.

<br/>

2005년, Adaptive Path의 Jesse James Garrette는 고객과의 이야기할 때 이 기술들을 반복적으로 명명하는 것이 매우 어렵다고 느끼게 되었고, 이를 종합적으로 설명해줄 수 있는 간단한 이름이 필요했습니다.

<br/>

그래서 그는 Asynchronous JavaScript And XML을 뜻하는 AJAX라고 불렀으며, 2006년에는 W3C 위원회는 이 명칭을 표준화하였습니다.

<br/>

# 6. AJAX는 무엇일까?

지금 우리가 알고 있듯이 AJAX는 이름과 같이 비동기적으로 목적지 서버와 통신한 후, 그에 의한 전송된 응답을 처리하는 기술을 말하기 위한 이름입니다.

<br/>

![ajax-webpage-lifecycle.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/99b1f7f0-5a6f-41b2-89b4-1e0f57bf2571/ajax-webpage-lifecycle.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220501%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220501T162328Z&X-Amz-Expires=86400&X-Amz-Signature=d04beb9a3636688f29ab2ff47bc47fae89e0c9da99b04fbf6cf1af6dc87c527f&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22ajax-webpage-lifecycle.png%22&x-id=GetObject)

<br/>

AJAX의 A는 비동기를 의미하며, 비동기적으로 클라이언트와 **서버간 통신하는데 차단없이 상호작용 할 수 있기 때문에 페이지의 변화 시, 새로고침 없이 반영할 수 있습니다.**

<br/>

따라서, 사용자는 웹 브라우저에서 이렇게 많은 작업이 일어나고 있다는 상황을 인지하지 못하고 실시간으로 변화된 모습에 집중하면 되는 것 입니다.

<br/>

# 7. XMLHttpRequest vs Fetch

현대 Javascript에서는 Ajax 통신을 위해 제공하는 API는 크게 `XMLHttpRequest` 와 `Fetch` 가 존재합니다.

<br/>

앞으로 이 둘의 장단점에 대해 한번 알아보겠습니다.

<br/>

## XMLHttpRequest 예시

<br/>

```jsx
const xhr = new XMLHttpRequest(); // XMLHttpRequest 객체 생성

xhr.open('GET', '/service'); // GET 요청

// HTTP 요청 상태를 나타내는 이벤트 핸들러
xhr.onreadystatechange = () => {
    // HTTTP 요청 상태를 나타내는 정수
    if (xhr.readyState !== 4) return;

    if (xhr.status === 200) {
        // 요청 성공시
        console.log(JSON.parse(xhr.responseText));
    } else {
        // 요청 실패시
        console.log('HTTP error', xhr.status, xhr.statusText);
    }
};

// HTTP 요청 전송
xhr.send();
```

<br/>

위와 같이 XMLHttpRequest는 `XMLHttpRequest()` 객체를 통해 인스턴스를 생성할 수 있으며, 우리가 DOM 조작시 자주 사용하는 사용하는 이벤트 기반으로 동작하게 되며,

<br/>

`onreadystatechange` 를 통해 상태 변경을 `readyState` 로 확인 할 수 있습니다.

<br/>

## Fetch 예시

<br/>

```jsx
fetch('/service', { method: 'GET' })
    .then(res => res.json())
    .then(json => console.log(json))
    .catch(err => console.error('error:', err));
```

위 코드는 `XMLHttpRequest` 와 동일하게 동작합니다.

<br/>

Fetch는 비교적 최근 2015년에 추가된 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API 입니다.

<br/>

이는 이벤트 기반인 `XMLHttpRequest` 와는 달리 ES6 부터 추가된 프로미스 기반으로 동작합니다.

<br/>

```jsx
const getService = async () => {
  const res = await fetch("/service", { method: "GET" }),
    json = await res.json();
  console.log(json);
} 잡기(오류) {
  console.error("Error:", Error);
}
```

<br/>

추가적으로 ES8 부터 `async` `await` 과 함께 명확하게 사용할 수 있습니다.

<br/>

## Fetch API 장점

<br/>

Fetch API의 경우에는 통신을 위한 HTTP 설정 인터페이스를 제공합니다.

<br/>

하지만, `XMLHttpRequest` 는 위와 같은 인터페이스를 제공하지 않습니다.

<br/>

대표적으로 쿠키전송으로 꼽을 수 있는데, `XMLHttpRequest` 는 항상 브라우저 쿠키를 전송하지만 Fetch API는 매개 변수에 명시하지 않는 한 쿠키 전송을 하지 않습니다.

<br/>

```jsx
const res = await fetch('/service', {
    method: 'GET',
    credentials: 'same-origin',
});
```

<br/>

이 뿐만 아니라, Fetch API를 통해 캐싱 제어, CORS 제어, 리다이렉션 제어, 데이터 스트림 등등.. 통신을 위해 필요한 것들을 제공합니다.

<br/>

## XMLHttpRequest 장점

<br/>

### 요청 진행상황을 알 수 있다.

<br/>

`XMLHttpRequest` 는 요청 진행 상황을 모니터링 할 수 있습니다.

```jsx
const xhr = 새로운 XMLHttpRequest();
xhr.upload.onprogress = (p) => {
	// 현재 요청이 몇 퍼센트 진행되었는지 확인
  console.log(Math.round((p.loaded / p.total) * 100) + "%");
};
```

<br/>

하지만, `Fetch`는 이러한 인터페이스를 제공하지 않습니다.

<br/>

이는 파일 업로드 진행상황을 보여줘야 할때 매우 유용하겠네요!

<br/>

### 요청 시간 초과 기능 지원

<br/>

```jsx
const xhr = new XMLHttpRequest();
xhr.timeout = 5000; // 최대 5초
xhr.ontimeout = () => console.log('timeout');
```

<br/>

`XMLHttpRequest` 의 경우, 요청을 허용하는 시간을 명시하는 `timeout` 인터페이스를 제공합니다.

<br/>

```jsx
Promise.race([fetch('/service', { method: 'GET' }), new Promise(resolve => setTimeout(resolve, 5000))]).then(res => console.log(res));
```

<br/>

`Fetch` 는 직접적으로 지원하지 않으나, `Promise.race` 로 처리할 수 있습니다.

<br/>

### 요청 중단 기능

<br/>

```jsx
const xhr = new XMLHttpRequest();
xhr.open('GET', '/service');
xhr.send();
// ...
xhr.onabort = () => console.log('aborted');
xhr.abort();
```

<br/>

`XMLHttpRequest`는 `abort()`를 통해 진행중인 요청을 중단시킬 수 있습니다.

<br/>

Fetch도 `AbortController` 객체로 처리할 수 있지만, `XMLHttpRequest` 보다 간단하진 않습니다.

<br/>

```jsx
const controller = new AbortController();
fetch('/service', {
    method: 'GET',
    signal: controller.signal,
})
    .then(res => res.json())
    .then(json => console.log(json))
    .catch(error => console.error('Error:', error));
// abort request
controller.abort();
```

<br/>

### 명확한 오류 감지

<br/>

일반적으로 우리가 `Fetch`를 사용할 때, `404 Not Found` 와 `500 Internal Server Error` 같은 에러를 `Promise.reject`로 잡아낼 수 있을 것으로 인지하나, `Promise`는 모든 응답에 대해 `resolve` 처리를 하기 때문에 모든 경우를 잡아 낼 수 없습니다.

<br/>

즉, `Promise.reject`는 오직 네트워크 실패와 같은 에러에서만 `reject`를 잡아내기 때문에 명확하지 않을 수 있습니다.

<br/>

`XMLHttpRequest`의 경우에는 단일 콜백 함수가 모든 결과에 대해 처리하기 때문에 더 명확하게 상태를 확인 할 수 있습니다.

<br/>

### 브라우저 지원

<br/>

2015년 이전 IE를 지원해야 하는 프로젝트를 진행해야 한다면 Fetch 보다는 `XMLHttpRequest`가 대안이 될 수 있을 것 입니다.

<br/>

## 정리

<br/>

-   Fetch

    -   프로미스 기반
    -   보다 간편한 사용성
    -   비동기 통신을 위한 다양한 인터페이스 제공
    -   모던 브라우저에 맞는 개발 가능

-   XMLHttpRequest
    -   이벤트 기반
    -   구 버전의 IE 지원
    -   진행 상황 파악의 용이함
    -   조금 더 명확한 오류 감지

# 출처

-   전통적인 웹 브라우저의 생명주기
    [https://poiemaweb.com/img/traditional-webpage-lifecycle.png](https://poiemaweb.com/img/traditional-webpage-lifecycle.png)
-   Ajax 생명주기
    [https://poiemaweb.com/img/ajax-webpage-lifecycle.png](https://poiemaweb.com/img/ajax-webpage-lifecycle.png)
-   Ajax의 역사
    [https://www.codeguage.com/courses/ajax/introduction](https://www.codeguage.com/courses/ajax/introduction)
-   Fetch vs XMLHttpRequest
    [https://medium.com/stackanatomy/ajax-battle-xmlhttprequest-vs-the-fetch-api-d8e6d5703528](https://medium.com/stackanatomy/ajax-battle-xmlhttprequest-vs-the-fetch-api-d8e6d5703528)
