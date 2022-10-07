# 1. 모듈이 나오게 된 배경

우리가 개발을 하게 되면 코드마다 각각의 책임이 나뉘어지게 되며, 이에 따라 여러 개의 파일로 분리 해야하는 시점이 오게 됩니다. 이때 분리된 파일은 각각을 ‘모듈’이라고 부르게 됩니다.

이전 자바스크립트 초창기에 역할도 많이 없었고 크기도 작고 단순했기 때문에 ‘모듈’에 대한 필요성은 크게 대두되지 않았습니다. 그런데, 2005년 Ajax가 나타나게 되면서 자바스크립트의 중요성은 그전보다 더 부각되었습니다. Ajax와 함께 자바스크립트의 연상이 증가 되었고 2008년 Google에서 공개한 V8 엔진은 더 많은 주목을 받게 되었습니다.

이에 따라 아래와 같은 모듈 시스템이 나오게 되었습니다.

- AMD
  - 가장 오래된 모듈 시스템중 하나로, require.js라는 라이브러리를 통해 처음 개발되었습니다.
- CommonJS
  - Node.js 서버를 위해 만들어진 모듈 시스템입니다.
  - `export`, `require`를 사용하는 것이 특징이다.
- UMD
  - AMD와 CommonJS와 같은 다양한 모듈 시스템을 함께 사용하기 위해 만들어졌습니다.

하지만, ES2015 부터 자바스크립트에서도 본격적으로 ESM (ECMAScript module)이라는 모듈 시스템을 도입하게 되면서 공식 스팩으로 들어오게 되었습니다. 이는 `export`, `import` 문법을 사용하는 것이 특징입니다.

> 참고로 Node.js는 ES2015 모듈 시스템을 제공하고 있긴 하지만, 자신만의 모듈 시스템인 CommonJS를 주로 사용합니다.

# 2. Webpack은 왜 필요한가?

## 웹팩 이전의 문제점

### 파일의 전역화

이전에는 여러 자바스크립트 파일을 사용하기 위해서 아래와 같이 여러 개의 스크립트 태그로 사용해야 했습니다.

```jsx
<body>
  <script src="./good.js"></script>
  <script src="./hello.js"></script>
</body>
```

위 방법의 문제점은 여러개의 자바스크립트 파일은 하나의 자바스크립트 파일 처럼 동작하게 됩니다. 즉, 하나의 전역을 공유하게 되어 문제를 발생 시킬 수 있는 것이죠. 이는 각 파일을 모듈로 분리하여 각자의 독립적인 스코프를 가질 수 있도록 하거나, IIFE를 사용하여 해결할 수 있습니다.

### IE를 포함한 구형 브라우저는 모듈 시스템을 지원하지 않는다.

![스크린샷 2022-10-02 오전 10.59.30.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/315fa2a6-38a7-4385-b4a5-3b208a7b27d4/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-10-02_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_10.59.30.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221002T052124Z&X-Amz-Expires=86400&X-Amz-Signature=d91a6c9709ba5044626857700dcff83b670d72db6a1ae49b65d84bd4397ea25b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-10-02%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25A5%25E1%2586%25AB%252010.59.30.png%22&x-id=GetObject)

안타깝게도 IE에서는 모듈 시스템이 존재하지 않기 때문에 이를 사용하기 위해서는 따로 라이브러리를 사용해야 합니다.

### 웹 애플리케이션의 성능

만약, 한 애플리케이션에 파일이 많아지게 되면 매 요청마다 필요로 하는 파일 또는 리소스를 많이 받아오게 될 것 입니다. 물론 파일이 많지 않은 경우에는 큰 문제가 되지 않겠지만 파일이 많아지게 되고 네트워크 성능 좋지 않은 경우에는 사용자는 단순한 로딩 화면만 보게 될 것 입니다.

## 웹팩으로 해결할 수 있는 것

### 파일의 전역화 및 구형 브라우저 지원

우리는 웹팩과 함께 바벨 트랜스파일러를 사용할 수 있습니다. 바벨은 트랜스파일링을 통해 ES2015 모듈을 AMD, CJS 등 여러 다른 형식으로 변환해줍니다.

### 브라우저별 HTTP 요청 숫자의 제약

![1_XCDi2LEqZLkTMNjJcQmj-g.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/5f01e0df-2c51-4ae0-9b7e-701345bf9afc/1_XCDi2LEqZLkTMNjJcQmj-g.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221002T052151Z&X-Amz-Expires=86400&X-Amz-Signature=965c58ab636340c0d4c999cfe25422c5916d2ce25707915e4bb694466eee7f71&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%221_XCDi2LEqZLkTMNjJcQmj-g.png%22&x-id=GetObject)

위 네트워크 요청 바를 보게 되면 6개 단위 별로 리소스를 받아오고 나머지 리소스는 유휴 상태에 빠져 있는 것을 볼 수 있다. 그 이유는 HTTP 1.X 에서는 브라우저마다 한번에 출처에서 허용 가능한 리소스의 양이 다르기 때문이다.

웹팩을 이용해 여러 개의 파일을 하나로 합치면 위와 같이 브라우저별 HTTP 요청 숫자 제약을 피할 수 있습니다.

### Dynamic Loading & Lazy Loading 지원

**Require.js**와 같은 라이브러리를 쓰지 않으면 동적으로 원하는 순간에 모듈을 로딩하는 것이 불가능 했습니다. 그러나 이젠 웹팩의 **Code Splitting** 기능을 이용하여 원하는 모듈을 원하는 타이밍에 로딩할 수 있습니다.

- **코드 스플리팅이란?**
  웹팩이라는 번들링을 통해 우리는 하나의 파일로 코드를 구성할 수 있습니다. 하지만, 페이지가 커짐에 따라 해당 기능에 필요 없음에도 로드를 해야하는 문제가 있었고, 한 개의 큰 번들로만 묶이는 것 또한 웹 성능에 영향을 주게 됩니다.
  이를 위해 코드스플리팅을 활용하여 큰 번들로 묶지 않고 적당 수준의 번들 사이즈로 만들면 웹 페이지의 성능을 보다 높일 수 있습니다.

# 3. Webpack

![스크린샷 2022-10-02 오후 12.53.18.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ff645274-42de-46d6-acbd-72cad6968208/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-10-02_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.53.18.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221002T052207Z&X-Amz-Expires=86400&X-Amz-Signature=b6889a9095baca56b9b985f591f68aec714f2b5dd271d1deea5d262ea8eaafed&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA%25202022-10-02%2520%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE%252012.53.18.png%22&x-id=GetObject)

웹팩은 자바스크립트 애플리케이션을 위한 정적 모듈 번들러 입니다. 번들링이란, 작업의 효율성을 위해 모듈들의 의존성 관계를 파악하여 그룹화 시켜주는 작업을 말합니다.

즉, 그림에서와 같이 웹팩은 우리가 사용하는 여러 HTML, 자바스크립트, CSS를 각각의 한 파일로 병합 및 압축해주는 라이브러리입니다.

## Webpack 구성

웹팩의 흐름은 **엔트리 파일 설정 → 로더 모듈 옵션 설정 → 추가 플러그인 적용 → 아웃풋**과 같이 동작하게 됩니다.

### entry

![9ldv44awmyjlww4bqsti.png](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2dcb7210-6bb2-4ae7-adfc-fea52db81643/9ldv44awmyjlww4bqsti.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20221002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20221002T052224Z&X-Amz-Expires=86400&X-Amz-Signature=ee3c535cea0bcc54c1ba5897e73efcac2f25925169c503c77c7d5eb31de6cd1d&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%229ldv44awmyjlww4bqsti.png%22&x-id=GetObject)

엔트리 포인트는 webpack이 내부의 디펜던시 그래프를 생성하기 위해 사용해야 하는 모듈입니다. 이를 통해 의존 관계를 맺어가면서 다른 모듈과 라이브러리를 찾게 됩니다.

기본적으로 `./src/index.js` 이지만, 우리는 `webpack.config` 로 다른 엔트리 포인트를 커스텀하여 사용할 수 있습니다.

```jsx
module.exports = {
  entry: './src/index.js',
};
```

추가적으로 MPA의 경우 엔트리 포인트가 여러개 일 수 있는데, 이 또한 아래와 같이 사용할 수 있습니다.

```jsx
entry: {
  main: './src/index.js',
  subpage: './src/pages/subpage.js'
},
```

### Loaders

webpack은 기본적으로 JavaScript와 JSON 파일만을 이해합니다. 로더를 사용하면 webpack이 알지 못하는 유형의 파일을 유효한 모듈로 변환하여 디팬던시 그래프에 추가하게 됩니다. 따라서, 우리는 로드를 사용하여 파일을 TypeScript

로더는 두 가지 속성을 가지게 됩니다.

1. 변환이 필요한 파일을 식별하는 `test` 속성
2. 변환을 수행하는데 사용되는 로더를 나타내는 `use` 속성

```jsx
module: {
  rules: [
    {
      test: /\.s?css$/, // 파일은 css 파일
      use: ['style-loader', 'css-loader', 'postcss-loader', 'sass-loader'], // css를 위한 로더 목록
    },
  ],
},
```

위와 같이 여러 개의 로더를 사용하려면 배열을 통해 체인을 구성하면 됩니다. 주의 해야할 점이 있다면 체인은 역순으로 실행되게 되며, 이전 로더의 결과가 현재 로더에게 전달되다 마지막 로더가 자바스크립트를 반환하는 형식으로 진행됩니다.

즉, 위 예제의 경우 sass-loader → postcss-loader → css-loader → style-loader 순으로 진행됩니다.

추가적으로, 로더에는 무엇이 있는지 알기 위해서 [로더의 목록](https://webpack.kr/loaders)을 확인해주세요.

### Plugins

플러그인은 번들 최적화, 애셋 관리, 환경 변수 주입과 같은 광범위한 작업을 수행할 수 있습니다. 따라서, 로더가 할 수 없는 작업을 수행할 목적으로 사용됩니다. 플러그인을 사용하려면 `require()` 를 통해 플러그인 라이브러리를 불러와 `plugins` 배열에 추가해야 합니다.

```jsx
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack'); // 내장 plugin에 접근하는 데 사용

module.exports = {
  module: {
    rules: [{ test: /\.txt$/, use: 'raw-loader' }],
  },
  plugins: [
    new HtmlWebpackPlugin({ template: './src/index.html' }),
    // 자동으로 HTML을 주입해주는 플러그인입니다.
  ],
};
```

추가적으로, 플러그인에는 무엇이 있는지 알기 위해서 [플러그인 목록](https://webpack.kr/plugins)을 확인해주세요.

### Output

Output 속성은 생성된 번들을 내보낼 위치와 이 파일의 이름을 지정하는 방법을 알려주는 역할을 합니다. 기본은 `.dist/main.js` 로, 파일이 생성되면 `./dist` 폴더로 설정됩니다.

즉, 우리가 빌드를 하면 `./dist` 폴더가 생성되는 이유가 그 이유입니다.

추가적으로, Output에는 무엇이 있는지 알기 위해서 [Output 목록](https://webpack.kr/configuration/output)을 확인해주세요.

# 4. 번들러 비교

번들러는 웹팩만 있는 것이 아닙니다. 그 이외에도 Parcel과 Rollup이 존재합니다.

### Parcel

Parcel은 2017년에 나온 웹 애플리케이션의 번들러입니다. 이는 `zero-configutation` 을 지향하기 때문에 우리가 따로 설정하지 않아도 바로 사용할 수 있습니다. 이것이 가능 한 이유는 Parcel은 엔트리 포인트가 아닌 애플리케이션 진입을 위한 HTML을 찾고 이에 따른 애셋들을 찾기 때문입니다.

추가적으로 알고 싶다면 [Parcel 공식 홈페이지](https://parceljs.org/)를 참고해주세요.

### Rollup

Rollup은 웹팩과 유사하지만, 큰 차이점은 ES6 모듈 형식으로 빌드 결과물을 출력할 수 있어 이를 라이브러리나 패키지에 활용할 수 있다는 점 입니다.

## 성능 비교

### 환경설정

- Parcel의 경우 따로 설정할 필요 없어 바로 실행할 수 있습니다.
- Rollup은 상대경로를 지원합니다.
- Webpack은 상대경로를 지원하지 않아 `path.resolve`와 `path.join` 으로 처리해야 합니다.

> Parcel > Rollup ≥ Webpack

### Tree Shaking

Tree Shaking은 사용되지 않는 코드를 제거하여 번들의 크기를 최적화 하는 것을 말하며, 이는 앱의 성능을 최적화하는데 매우 중요합니다.

- Parcle은 ES6와 CJS 모듈에 대해 Tree Shaking을 지원하며, 캐싱을 하여 빠른 속도를 보여줍니다.
- Rollup은 정적 분석 후, 실제로 사용하지 않는 코드를 제외합니다.
- Webpack의 경우 Tree Shaking을 위해 아래와 같은 조건이 필요합니다.
  - ES6 문법을 필요로 합니다.
  - `package.json` 에 추가적인 `SideEffects` 플래그를 필요로 합니다.
  - 추가적인 Minifier 플러그인이 필요합니다. (eg: `UglifyJSPlugin`).

> Parcel > Rollup > Webpack

### 코드 스플리팅

앱이 커짐에 따라 번들 크기도 커짐으로 코드 스플리팅을 통해 웹 페이지의 성능을 향상 시킬 수 있습니다.

- Webpack은 최소한의 작업과 로드 시간을 보여줍니다.
- Rollup과 Parcel의 모두 코드 스플리팅을 지원하지만, 자그마한 이슈를 보입니다.

따라서, 이들중 가장 안정적이게 지원하는 것은 Webpack입니다.

> Webpack > Rollup, Parcel

### Reload

- Webpack은 `webpack-dev-server` 를 제공하여 live-reload를 지원합니다.
- Rollup은 `rollup-plugin-serve`를 제공하지만, live-reload를 위해선, `rollup-plugin-livereload`를 설치해야 합니다.
- Parcel은 Dev Server가 내장되어 있고 파일 변경시 자동적으로 리로드 하지만, HTTP logging, Hooks, middleware를 사용할 때 이와 관련된 이슈가 발생하였습니다.

> Webpack > Rollup, Parcel

### Hot Module Replacement

HMR과 live-reload와의 차이점은 따로 새로고침 할 필요 없이 런타임에 브라우저에서 모듈을 자동으로 업데이트 하여 개발 환경을 개선할 수 있다는 점 입니다.

즉, 새로 페이지의 방문없이 반영되어 기존의 상태값을 유지시킬 수 있다는 장점이 있습니다.

- Webpack은 `webpack-dev-server` 를 통해 지원합니다.
- Parcel은 HMR에 대한 지원을 내장하고 있습니다.
- Rollup은 `rollup-plugin-hotreload`를 출시하였습니다.

하지만, 이에 대한 가장 안정된 번들러는 Webpack입니다.

> Webpack > Rollup, Parcel

## 결론

> Building a basic app and want to get it up and running quickly? Use Parcel.
>
> Building a library with minimal third-party imports? Use Rollup.
>
> Building a complex app with lots of third-party integrations? Need good code splitting, use of static assets, and CommonJs dependencies? Use webpack.

- 만약, Non-config로 빠른 프로젝트를 수행하고 싶다면 Parcel
- 써드 파티 라이브러리를 최소화 하여 다른 사람이 사용하는 라이브러리를 구성하고 싶다면 Rollup
- 실질적으로 프로덕션 앱을 만들고 싶다면 Webpack

# 참고

[모듈 소개](https://ko.javascript.info/modules-intro)

[브라우저 HTTP 최대 연결수 알아보기](https://medium.com/@syalot005006/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-http-%EC%B5%9C%EB%8C%80-%EC%97%B0%EA%B2%B0%EC%88%98-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-3f7aa1453bc2)

[How Webpack uses dependency graph to build modules](https://dev.to/jasmin/how-dependancy-graph-in-webpack-resolve-module-dependency-5ej4)

[Rollup vs. Parcel vs. Webpack: Which is the Best Bundler?](https://betterprogramming.pub/the-battle-of-bundlers-6333a4e3eda9)
