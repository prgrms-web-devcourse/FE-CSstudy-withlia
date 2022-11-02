# React Lazy Loading

## Lazy Loading이란

- 리소스를 식별해서 **필요할 때**만 로드하는 전략.
- 페이지 로드 시간을 단축 시킬 수 있음.

## Code-Splitting

- 대부분 `React` 앱들은 `Webpack` 같은 툴을 사용하여 여러 파일을 하나로 합친 번들 파일을 웹 페이지에 포함하여 한 번에 전체 앱을 로드.
- 하지만 규모가 커질 수록 번들의 크기도 커지고 로드 시간이 길어지는 문제가 발생.
- 이를 해결하기 위해 번들을 나눔.(`Splitting`)
- `Code-Splitting`은 런타임에 여러 번들을 동적으로 만들고 불러오는 것이고 `Webpack`과 같은 번들러가 지원하는 기능.
- `Code-Splitting`은 앱이 `Lazy Loading` 하게 도와주고 초기화 로딩에 대한 비용을 줄여줌.

## import()

- `import(moduleName)`
- dynamic import.
- 함수처럼 보이는데 `import` 자체는 keyword임.
  - `const myImport = import` -> Syntax Error.
- `return` 값은 Promise.
  - `moduleName`이 export 하고 있는 모든 값을 갖고 있는 `object`
- `Webpack`이 이 구문을 만나게 되면 앱의 코드를 분할.

```js
import('./math').then((math) => {
  console.log(math.add(16, 26))
})
```

## React.lazy

- `dynamic import`를 사용해서 컴포넌트를 렌더링.
- lazy 컴포넌트는 `Suspense` 컴포넌트 하위에서 렌더링.
- lazy 컴포넌트가 로드되길 기다리는 동안 `fallback`을 통해 예비 컨텐츠를 보여줌.

```js
import React, { Suspense } from 'react'

const OtherComponent = React.lazy(() => import('./OtherComponent'))

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  )
}
```

## Route-based code splitting

- `Code-Splitting` 하기 가장 좋은 곳은 route.
- 대부분 페이지를 한번에 렌더링하기 때문에 사용자가 렌더링 중에 다른 상호작용을 하지 않음.

```js
import React, { Suspense, lazy } from 'react'
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom'

const Home = lazy(() => import('./routes/Home'))
const About = lazy(() => import('./routes/About'))

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Suspense>
  </Router>
)
```

## React 18의 Suspense

- `Suspense`는 `React 16`에 나왔고 `Code-Splitting`을 위해 `React.lazy`와 사용됐었다.
- 그러면 `React 18`에서 다시 언급되고 있는 `Suspense`는 무엇?.
- 기존 `Suspense`(Legacy Suspense) SSR에서 적용할 수 없었음.
  - Server Side에서 만들어서 전달해야하니까 `Lazy Loading`을 적용할 수 없음.
- 새로운 `Suspense`(Concurrent Suspense) `stream`을 활용해서 해결.
  - `ReactDOMServer.renderToReadableStream(element, options);`
  - `stream`으로 `fallback`이 포함 된 초기 HTML을 보낸다.
  - contents가 준비되면 `stream`으로 `Script` HTML 태그를 보내서 `component`를 올바른 위치에 주입시킴.
- `Suspense`를 사용하면 많은 일들이 병렬적으로 일어남. (hydration도)
  ![react18_suspense](https://user-images.githubusercontent.com/16220817/197353592-ae12aa69-a727-406f-9381-dc625e57eb39.png)

## 출처

- [Lazy Loading](https://developer.mozilla.org/en-US/docs/Web/Performance/Lazy_loading)
- [Code-Splitting](https://ko.reactjs.org/docs/code-splitting.html)
- [import()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import)
- [React.lazy 및 Suspense를 사용한 코드 분할](https://web.dev/i18n/ko/code-splitting-suspense/)
- [How Suspense Works in React 18](https://betterprogramming.pub/how-suspense-works-in-react-18-c7617a50447f)
- [번역 리액트 렌더링의 미래](https://junghan92.medium.com/%EB%B2%88%EC%97%AD-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%A0%8C%EB%8D%94%EB%A7%81%EC%9D%98-%EB%AF%B8%EB%9E%98-5b7251bda66d)
