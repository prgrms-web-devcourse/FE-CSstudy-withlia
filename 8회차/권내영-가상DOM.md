## 브라우저 렌더링(이전 스터디 발표)

[브라우저 렌더링 및 동작](https://www.notion.so/bcc1874e202c4c6fb17cfaf66183bb3a)

## DOM이란?

html의 요소들을 구성하고 속성이나 디자인, 배치 등을 조작할 수 있는 트리 구조 객체

## 브라우저 렌더링의 비효율성

**브라우저 렌더링 과정**

1. dom 트리 생성
2. render 트리 생성
3. 레이아웃 (reflow)
4. 페인트 (repaint)

브라우저 렌더링 과정은 DOM요소가 바뀔 때 마다 전체 렌더링이 일어나게 된다. **웹브라우저에서 렌더링이 일어나는 과정, CSS연산과 레이아웃, 리페인트하는 비용은 매우 비싸다.**

게다가 현대의 웹 어플리케이션은 SPA 구조로 이루어져 있기 때문에 동일한 페이지에서 DOM이 반복해 변화하게 되고, 사용자와 상호작용이 매우 많이 일어난다.

이때 마다 렌더링 과정을 반복하게 되면 매우 비효율적이고 무거운 과정이 된다.

따라서 리액트와 같은 라이브러리에서는 가상 DOM을 사용해서 비용을 줄인다.

## 가상 DOM이란?

실제 DOM에 접근해 조작하는대신 이를 **복사해** **추상화한** JS 객체를 메모리상에 구성한 것이다.

- html 코드

```jsx
<div class="example">
  <ul>
    <li>hello</li>
    ...
  </ul>
</div>
```

- 가상 DOM 객체 구성

```jsx
let virtualDOM = {
    tag: "div", // 태그 종류
    props: { class: 'example' }, // 요소의 props들
    children: [ // 자식
        **{
            tag: "ul",
            children: [ { tag: "li", children: "hello" } ... ];
        },
    ]
}
```

실제 DOM과 같은 요소들은 들어 있지만 가상 DOM을 조작하는 DOM API가 존재하지 않기 때문에, 생성과 접근이 훨씬 빠르고 가볍다.

가상 DOM에서는 가상적으로 UI를 저장했다가 실제의 DOM과 동기화하는 과정을 거친다.

## React의 가상 DOM의 작동 원리

React에서는 항상 2개의 가상 DOM을 가지고 있다.

- **렌더링 이전 화면 구조**를 나타내는 가상돔
- **렌더링 이후 화면 구조**를 나타내는 가상돔

1. **State가 변경되면, 변경사항이 담긴 가상 DOM을 생성한다.**
2. **업데이트 되기 전 - 업데이트 후의 가상 DOM을 비교한다. (Diffing Algorithm)**

![KakaoTalk_20220925_120258101](https://user-images.githubusercontent.com/75849590/192678694-ec44d334-4190-4821-afbf-b8c646629865.jpg)



3. **바뀐 부분들만 실제 DOM에 적용시킨다. (Reconciliation)**

   - Batch Update

   리액트가 바뀐 부분을 실제로 DOM에 업데이트할 때 변경된 모든 엘리먼트들을 한꺼번에 batch update(집단 업데이트)를 하기 때문에 효율적이다.

   예를 들어 List의 요소가 한번에 여러개 바뀔 때, 그것을 하나 하나 반복해서 업데이트 하는 것이 아니라 한꺼번에 바뀐 모든 부분을 실제 DOM에 집단 업데이트하는 것이다.

![KakaoTalk_20220925_120309956](https://user-images.githubusercontent.com/75849590/192678708-361bc120-6df0-4553-8fe5-8e0a46e33fa0.jpg)


### virtual DOM 과 실제 DOM의 흐름

![img](https://user-images.githubusercontent.com/75849590/192679695-3d0cbb0c-7a7e-42c2-88a9-cb1b6a591615.png)


### Diffing Algorithm

리액트가 2개의 돔 트리를 비교할 때 쓰는 알고리즘이다. 휴리스틱 알고리즘을 사용해 O(n) 시간 복잡도를 가지고 있다.

1. **루트 노드의 타입이 다른 경우**

가상DOM의 root가 다른 경우 새로운 트리를 구축하게 된다. 엘리먼트가 제거되면 해당 엘리먼트와

기존 트리와 관련된 하위 모든 엘리먼트의 state도 사라진다.

`componentWillUnmount()` `UNSAFE_componentWillMount()` `componentDidMount()` 를 호출한다.

```jsx
// div - span 요소는 서로 타입이 다르기 때문에 제거되고 새로운 트리가 구축된다
<div>
  <Counter />
</div>

<span>
  <Counter />
</span>
```

1. **타입은 같지만 속성이 다른 경우**

속성이 다른 경우에는 비교해서 바뀐 내용만 갱신한다. (class 등)

해당 처리는 하위의 자식 노드들에게 재귀적으로 처리한다.

style 갱신 또한 변경 사항만 갱신한다.

```jsx
<div className="before" title="stuff" />
<div className="after" title="stuff" />
```

```jsx
<div style={{color: 'red', fontWeight: 'bold'}} />
<div style={{color: 'green', fontWeight: 'bold'}} />
```

1. **같은 타입의 Component Element**

컴포넌트가 갱신되면 인스턴스는 동일하게 유지되어 state가 유지된다.

또한 새로운 내용 반영을 위해 props를 업데이트한다.

`UNSAFE_componentWillReceiveProps()` `UNSAFE_componentWillUpdate()` `componentDidUpdate()` 를 호출한다.

render() 함수 호출 후 diff 알고리즘 불러 이전과 새 결과에 대해 재귀적으로 비교한다.

1. **반복적인 Element 처리**

DOM 노드의 자식들을 반복적으로 처리할 때 기본적으로 두 DOM에서 순차적으로 비교하고 차이점이 있으면 변경한다.

순회 비교를 하기 때문에 Element 리스트의 앞에 추가하는 것 보다 뒤에 추가하는 것이 성능상 이점이 있다.

```jsx

<ul>
  <li>first</li>
  <li>second</li>
</ul>

<ul>
  <li>first</li>
  <li>second</li>
  <li>third</li>
</ul>

// 앞에 집어 넣으면 차례로 비교하기 때문에 모든 자식 요소를 변경한다.
<ul>
  <li>first</li>
  <li>second</li>
</ul>

<ul>
  <li>three</li>
  <li>first</li>
  <li>second</li>
</ul>
```

이때 이러한 문제를 해결하기 위해 반복적인 요소에서는 key 속성을 사용하기를 권장하고 있다.

![image (8)](https://user-images.githubusercontent.com/75849590/192679277-b0ae150e-f4f1-42de-91cb-cb5714be8235.png)
React에서 key prop을 지정하지 않았을 때 나오는 warning이다

React key속성을 이용해 가상 DOM을 비교한다.

```jsx
<ul>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>

<ul>
  <li key="2014">Connecticut</li>
  <li key="2015">Duke</li>
  <li key="2016">Villanova</li>
</ul>

// 보통 이렇게 쓴다
<li key={item.id}>{item.name}</li>
```

### key 속성에 index를 사용하는 것이 좋지 않은 이유

리액트 공식 문서에서 idx를 key 로 사용하는 것을 권장하지 않고 있다. 이유는 컴포넌트는 key를 보고 해당 엘리먼트가 유지되었는지, 변경되었는 지 판단하는데 index는 항목의 순서가 바뀌었을 때 그것의 index값 또한 바뀌기 때문이다.

단순한 리스트 렌더링 역할만 한다면 모르겠지만, 정렬, 추가, 삭제 등의 작업이 있는 경우에는 예상치 못한 문제가 발생할 수 있다.

따라서 key는 반드시 변하지 않고 예상가능하고, 유일한 값으로 결정되어야 한다.

```jsx
<ul>
  <li key="0">Connecticut</li>
  <li key="1">Duke</li>
  <li key="2">Villanova</li>
</ul>

<ul>
  <li key="0">Tom</li>
  <li key="1">Connecticut</li>
  <li key="2">Duke</li>
  <li key="3">Villanova</li>
</ul>
```

요소를 앞에 추가할 때에는 전체가 리렌더링 된다.

**예외사항**

- 배열과 배열의 항목들이 static해서 변경되지 않는 경우
- 배열의 항목들의 id 값이 없는 경우
- 배열이 절대 재배열되거나 일부가 삭제되지 않는 경우

[Reconciliation - React](https://reactjs.org/docs/reconciliation.html)

[React의 Virtual DOM(VDOM)과 Diffing 알고리즘](https://minemanemo.tistory.com/120)

[Index as a key is an anti-pattern](https://robinpokorny.medium.com/index-as-a-key-is-an-anti-pattern-e0349aece318)
