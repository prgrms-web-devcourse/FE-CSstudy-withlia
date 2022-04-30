# 1. 과거 웹사이트

과거 웹사이트는 지금과는 달리 정적인 HTML만을 사용했으며, 페이지간 이동을 위해서 브라우저를 새로고침을 해야 했습니다.

![traditional-webpage-lifecycle.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0212dc7d-6926-4c95-9b48-45df34c78fce/traditional-webpage-lifecycle.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220430%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220430T072222Z&X-Amz-Expires=86400&X-Amz-Signature=0cbb65563893d3eac0a13e2352e511f8db7d8c554d5e6182848331c3b20d67ed&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22traditional-webpage-lifecycle.png%22&x-id=GetObject)

즉, 거의 내용이 바뀌지 않는 페이지라도 모든 페이지를 다시 서버에게 요청을 한 후 페이지를 반영해야 했습니다.

이러한 방식은 간단한 페이지라면 크게 문제 될 것은 없었겠지만, 복잡한 응용 페이지의 경우 페이지 인터렉션을 위한 추가적인 지연시간을 야기했습니다.

더 나아가 모든 DOM이 다시 그려지기 때문에 브라우저는 전체 HTML 콘텐츠를 DOM 트리로 다시 파싱하는 작업을 매번 거쳐야 했습니다.

![화면-기록-2022-04-28-오전-2.52.14.gif](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b68cfb59-1ec4-40dc-a0b7-8728266ed9e5/%E1%84%92%E1%85%AA%E1%84%86%E1%85%A7%E1%86%AB-%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%E1%86%A8-2022-04-28-%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB-2.52.14.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220430%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220430T072249Z&X-Amz-Expires=86400&X-Amz-Signature=0bd85f41197329337c42c61f4a1ecc218e5f9a1c9b8132e923c8d02a54dc8d5a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2592%25E1%2585%25AA%25E1%2584%2586%25E1%2585%25A7%25E1%2586%25AB-%25E1%2584%2580%25E1%2585%25B5%25E1%2584%2585%25E1%2585%25A9%25E1%2586%25A8-2022-04-28-%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB-2.52.14.gif%22&x-id=GetObject)

이는 과거 웹 페이지의 예시입니다.

위에서도 언급 했다시피, 페이지를 이동할 때 마다 새로운 HTML과 페이지 구성을 위한 것을 받아오는 것을 볼 수 있습니다.

그래서 이를 조금이나마 해결하기 위해 `iframe`이라는 새로운 방법이 등장했습니다.

# 2. iframe

일반적으로 `iframe` 태그는 HTML 페이지 안에서 다른 웹 페이지를 삽입할 수 있도록 하는 태그입니다.

때문에, 브라우저의 역할 없이도 요청을 통해 웹 페이지 안에서 새로운 페이지를 보여줄 수 있도록 하였습니다.

이는 클라이언트와 서버간의 비동기 통신의 시초라고 볼 수 있습니다. 

`iframe` 은 백그라운드에서 요청하기 때문에 사용자는 브라우저의 새로고침 없이, 변경된 페이지를 볼 수 있었습니다.

`iframe` 의 사용 예시입니다. 

```html
<!-- index.html -->
<body>
    <form action="hello.html" target="content">
        <input type="text" />
        <button type="submit">입력</button>
    </form>
    <iframe name="content" frameborder="0" ></iframe>
</body>

<!-- hello.html -->

<body>
    <h1>hello</h1>
</body>
```

이와 같이 `index.html` 에서 `form` 테그에 Submit 이벤트가 발생하였을 때 `hello.html` 을 불러오는 작업을 수행하게 됩니다.

![화면-기록-2022-04-30-오후-1.57.20.gif](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/42538f7d-a915-445e-8c39-a58044008227/%E1%84%92%E1%85%AA%E1%84%86%E1%85%A7%E1%86%AB-%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%E1%86%A8-2022-04-30-%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE-1.57.20.gif?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220430%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220430T072307Z&X-Amz-Expires=86400&X-Amz-Signature=764b3a23b3b4e2d4e06b7356113c5cb2734c95e5d2f98c22aa95dbc29fb79ec0&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2592%25E1%2585%25AA%25E1%2584%2586%25E1%2585%25A7%25E1%2586%25AB-%25E1%2584%2580%25E1%2585%25B5%25E1%2584%2585%25E1%2585%25A9%25E1%2586%25A8-2022-04-30-%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE-1.57.20.gif%22&x-id=GetObject)

위 그림과 같이 form 태그가 제출되면 없었던 `hello.html` 이 `iframe` 에 삽입 된 것을 볼 수 있습니다.

그러나, 기술이 점차 발전하게 되면서 응용 프로그램은 더욱 복잡해져, `iframe` 기술은 모든 요구 사항을 대응하기에는 어려움이 따랐습니다.

이는 개발자가 요청에서 헤더를 수동으로 설정과 가져오는 작업을 못하는 등.. 많은 제약이 따르게 되었습니다.

이와 같은 이유로 `iframe` 은 충분하게 좋은 대안이 되었지만, 개발자들은 더 좋은 방법을 찾기 시작했습니다.

# 3. ActiveXObject()와 XMLHTTP의 지원

2000년에, Microsoft에서 브라우저가 HTTP 요청을 자바스크립트를 사용해 비동기 방식으로 보내고 응답을 쉽게 처리할 수 있도록 하기 위한 작업을 시작하였습니다.

이를 통해 MS는 IE5 부터 `ActiveXObject()` 를 추가 하였으며, 이를 통해 `XMLHTTP`를 지원하기 위한 MSXML 라이브러리가 생겨나게 되었으며, 이 `XMLHTTP`는 현재 웹 표준을 이끈 엄청난 아이디어라고 합니다.

하지만, 그 당시에는 이 엄청난 잠재력에 대해 아는 사람은 거의 없었기에, 많은 주목을 받지는 못하였다고 합니다.

# 4. XMLHttpRequest()의 탄생

이후, 모든 브라우저들은 이 기능을 각 브라우저에서도 동작하도록 추가하기 제정하기 위한 움직임이 생겨났으며, Chrome, Firefox, Opera, Safari는 `XMLHttpRequest`를 적용하였습니다.

> IE에서는 IE7에서야 `XMLHttpRequest()` 가 추가 되었음에도 불구하고 여전히 `AcriveXObject()` 를 지원하고 있었습니다.
> 

> Microsoft `ActiveXObject()` 를 Edge 브라우저에서 삭제되었으며, `XMLHttpRquest` 만 제공하게 되었습니다.
> 

![Google-Maps-Beta1.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4d8f629f-ed6d-43ee-97b2-7a513987852d/Google-Maps-Beta1.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220430%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220430T072330Z&X-Amz-Expires=86400&X-Amz-Signature=cc9c5dc6b1a86d9c7d47ca171bed93fd9bc010da034858b80aaae33e937cc0c5&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Google-Maps-Beta1.png%22&x-id=GetObject)

그렇게 그닥 주목 받지 않았던 이 기술은 2005년 Google은 Gmail, Google Maps를 출시되면서 주목을 받기 시작했습니다.

이 두개의 애플리케이션은 `XMLHttpRequest()` 를 통하여 더욱 높은 User Interaction과 풍부한 사용자 경험을 제공하게 되었습니다. 

 

추가적으로 위와 동시에 Google은 검색 엔진에 사용자가 검색하는 도중에 일치하는 추천 글자를 보여주는 Google Suggest를 출시하였고 이 또한 `XMLHttpRequest()` 가 사용되었습니다.

# 5. AJAX의 탄생

점차 이 기술은 다른 기술과 잘 융합 되었으며, 웹 개발을 위한 단일 단위로 사용 되기 시작하였습니다.

2005년, Adaptive Path의 Jesse James Garrette는 고객과의 이야기할 때 이 기술들을 반복적으로 명영하는 것이 매우 어렵다고 느끼게 되었고, 이를 종합적으로 설명해줄 수 있는 간단한 이름을 필요로 했습니다.

그래서 그는 Asynchronous JavaScript And XML을 뜻하는 AJAX라고 불렀으며, 2006년에는 W3C 위원회는 이 명칭을 표준화 하였습니다.

# 6. AJAX는 무엇일까?

지금 우리가 알고 있듯이 AJAX는 이름과 같이 비동기적으로 목적지 서버와 통신 한 후, 그에 의한 전송된 응답을 처리하는 기술을 말하기 위한 이름입니다.

AJAX의 A는 비동기를 의미하며, 비동기적으로 클라이언트와 서버간 통신하는데 차단없이 상호작용 할 수 있기 때문에 페이지의 변화 시, 새로고침 없이 반영할 수 있습니다.

따라서, 사용자는 웹 브라우저에서 이렇게 많은 작업이 일어나고 있다는 상황을 인지하지 못하고 실시간으로 변화된 모습에 집중하면 되는 것 입니다.

따라서, AJAX 애플리케이션이 작동하기 위해, 다음 세 가지 기술을 필요로 합니다.

- HTML을 사용해, 문서의 구조와 의미 체계를 만들고 웹 페이지의 내용을 반영합니다.
- JavaScript는 `XMLHttpRequest()` 를 사용하여, AJAX 엔진을 생성합니다.
- HTML DOM은 이동 중에 동적으로 문서를 변경할 수 있어야 합니다.

하지만, 기술이 발전하면서 추가적으로 아래 요소까지 요구하게 되었습니다.

- CSS는 웹 페이지를 스타일링하고 눈에 잘 띄도록 합니다.
- XML은 트리 구조로 되어 있는 데이터를 클라이언트와 서버간 통신할 때 사용되는 포멧입니다.
- JSON은 자바스크립트 객체 형태로 쉽게 볼 수 있는 데이터 포멧입니다.

# 출처

- 전통적인 웹 브라우저의 생명주기
    
    [https://poiemaweb.com/img/traditional-webpage-lifecycle.png](https://poiemaweb.com/img/traditional-webpage-lifecycle.png)
    

- Ajax의 역사
    
    [https://www.codeguage.com/courses/ajax/introduction](https://www.codeguage.com/courses/ajax/introduction)
