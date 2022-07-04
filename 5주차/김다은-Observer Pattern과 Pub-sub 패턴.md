# 목차

- [Reactive Programming](#reactive-programming)
- [Observer Pattern](#observer-pattern)
  - [React 에 비유하기](#react-에-비유하기)
- [Pub-Sub 패턴](#pub-sub-패턴)
  - [Redux / Flux Pattern / Context API](#redux--flux-pattern--context-api)
- [참고 자료](#참고-자료)

# Reactive Programming

![React](https://user-images.githubusercontent.com/74234333/177119424-8d1792ff-d721-4e6f-ad9a-a4840f2930b8.png)

React 와 같은 라이브러리를 가지고 프로그래밍을 할 때, "**반응형(Reactive) 프로그래밍**"이라는 용어를 많이 사용한다.

반응형 프로그래밍을 간단하게 말하자면, `x`라는 값이 있다고 할 때, `x`라는 데이터에 관련된 또 다른 값들이 `x` 가 다시 계산되거나 업데이트 될 때, 그에 반응하여 값이 바뀌는 것을 이용하는 프로그래밍이다.

현재 "반응형 프로그래밍"을 토대로 하는 다양한 프레임워크와 라이브러리 등이 있으며, 이러한 것들의 동작을 흔히 **마법처럼 동작한다**라고 한다.

이러한 반응형 동작, 그리고 반응형을 넘어 여러 비동기처리와 같은 로직들이 Observer Pattern 과 Pub-Sub 패턴 등의 원칙 위에서 만들어졌다고 할 수 있다.

<br />

# Observer Pattern

> The observer pattern is a software design pattern in which an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods. <br /><br />observer pattern 은 ‘subject’ 라고 불리오는 object로 이루어진 디자인 패턴이다. 이 ‘subject’는 ‘observers’라고 불리는 observer들에 의존을 하며, 어떤 state가 변하는 경우 subject의 메서드를 호출하는 방식으로 subject의 state 변화를 observer 에게 알린다(notify).

옵저버 패턴은 **관찰자(observers)가 관찰 대상(observed, subject)의 상태 변화를 관찰하기 위해서, 이 둘의 관계를 구조환하는 방법**이다.
이 말은, 관찰자를 두고 이 관찰자가 어떤 객체(subject)의 state 변화를 알아차리기 위하여, 어떤 방법을 사용하는 지에 대한 패턴이라고 할 수 있다.

옵저버 패턴에서는 **관찰자들이 항상 관찰 대상을 주시하고 있는게 아니다. 대신, 변경 사항에 대한 알림을 받기 위하여, 관찰 대상을 구독(subscribe) 하며, 관찰 대상의 state 변화가 생겼을 때, 변화에 대한 알림을 받는다.**

이를 그림으로 나타내면 다음과 같다.

<img src="https://user-images.githubusercontent.com/74234333/177123974-d9f32b13-75e3-4ce0-abd0-a152ee21ede1.png" width="500" alt="observer-diagram"/>

1. 관찰 대상 (subject)는 관찰자 목록을 가진다.
2. 관찰자 (observer)는 상태 변화에 따른 로직을 위한 `update` 메서드를 가진다.
3. 관찰 대상의 상태가 변하면 관찰 대상의 `알림` 메서드를 통하여 상태 변화를 관찰자에게 알린다.
4. `알림` 메서드를 통하여 상태가 변경됨을 알아챈 관찰자는, `update` 메서드를 호출하여 관찰 대상의 `state` 변화에 따른 로직을 수행한다.

<br />

## React 에 비유하기

![react-componentTree](https://user-images.githubusercontent.com/74234333/177126914-985b198a-b292-4386-8fd3-52f9b7f49f3f.png)

React 의 컴포넌트가 리렌더링되는 방식에 대해서 생각해보자.
부모 컴포넌트를 통해서 prop을 받는 자식 컴포넌트가 있다고 할 때, 이 prop이 변경되면 자식 컴포넌트가 리렌더링 된다.

즉, 부모 컴포넌트의 상태가 자식 컴포넌트에 전달이 되면 부모 컴포넌트의 상태와 관련된 모든 데이터들이 리렌더링 되는 것이다.

이를 옵저버 패턴에 빗대어서 생각을 해보면, Observers 을 자식 컴포넌트라고 생각할 수 있고, 부모 컴포는트는 subject라고 생각할 수 있다.

위의 컴포넌트 트리를 오른쪽으로 90도 돌려보면 옵저버 패턴의 도식과 상당히 유사한 그림이 나온다.

<img src="https://user-images.githubusercontent.com/74234333/177128411-0afc6bb1-3ec8-4bb9-af37-050e116a407e.png" width="500" alt="observer-component"/>

1. 관찰 대상 (subject)는 관찰자 목록을 가진다.

**부모 컴포넌트는 자식 컴포넌트에게 있어서 "관찰 대상"이라고 할 수 있다. 즉, 부모 컴포넌트는 자식 컴포넌트에 대한 의존성을 가진다.**

2. 관찰자 (observer)는 상태 변화에 따른 로직을 위한 `update` 메서드를 가진다.

**자식 컴포넌트는 부모 컴포넌트의 변화에 따라 리렌더링하는 로직이 있다.**

3.  관찰 대상의 상태가 변하면 관찰 대상의 `알림` 메서드를 통하여 상태 변화를 관찰자에게 알린다.

**부모 컴포넌트의 상태가 변하면 자식 컴포넌트는 부모 컴포넌트의 상태가 변한 것을 알아챈다.**

4. `알림` 메서드를 통하여 상태가 변경됨을 알아챈 관찰자는, `update` 메서드를 호출하여 관찰 대상의 `state` 변화에 따른 로직을 수행한다.

**부모 컴포넌트의 상태변화를 알아채면, 자식 컴포넌트는 그에 따른 리렌더링을 수행한다.**

따라서 옵저버 패턴을 통하여 리액트의 컴포넌트 간의 데이터 흐름 원리를 파악할 수 있다.

<br />

# Pub-Sub 패턴

`Pub-sub 패턴`은 위의 `Observer 패턴`을 **단일 책임의 원칙**을 기준으로 강화시킨 패턴이라고 생각해볼 수 있다.

위의 Observer 패턴의 경우(React의 부모 컴포넌트와 자식 컴포넌트로 생각해보자), observer와 subject는 서로 강하게 의존하고 있다.

**단일 책임의 원칙**을 가지고 생각해보자. Observer 패턴으로 구현한 코드에서 **데이터 상태를 관리하고 있는 부분**의 책임을 분리해보자.

Pub-Sub 패턴을 도식화해보면 다음과 같이 나타낼 수 있다.

<img src="https://user-images.githubusercontent.com/74234333/177136075-6e574588-1ad8-4cb9-bdff-6734ee8d92e2.png" width="400" alt="pub-sub-pattern"/>

1. data Service에서는 상태와 관련된 로직을 가지고 있다. 이 말은 상태 변경에 따라서, 언제 반응하고 어떻게 반응하는지에 관련된 로직을 가지고 있다는 것이다.
2. `Subscriber`은 data service를 구독하고 있다. 이 구독의 형태는 `Subscribe`가 가지고 있는 함수를 data service에 (콜백 함수의 형태)로 남겨두는 방식이다.
3. `Publisher`은 상태가 변경되었다는 이벤트(로직)을 발행(publish)한다.
4. publish가 되고 나서, data service에서는 상태 변경에 관련된 로직을 수행한다. 그리고 subscribe에서 등록한, 상태에 대한 반응이 필요할 때 이 함수를 호출한다.

결국에는 상태를 관리하는 곳과 상태를 사용하는 부분의 책임을 분리시킨 것이다. 특정 이벤트에 어떤 함수를 구독하도록 만들고 DOM 요소에 의해서 action이 발행되면 그 함수가 호출된다.

이러한 패턴은 비동기 로직를 사용하는 로직에서 특히 빈번하게 관찰할 수 있다.
예를 들어 EventListener 의 동작에서, 자바스크립트 코드에서 event가 실행되었을 때, 반응하고자 하는 코드를 EventListener에 콜백으로 넘기게된다. 이 말은, 이 자바스크립트 코드가 event 에 대한 데이터를 구독하고 있다는 것이고, 데이터가 준비되면 발행하면서 데이터를 넘겨주게 된다.

이런 Pub-Sub 패턴을 두드러지게 관찰할 수 있는 예시를 몇 개 살펴보자.

## Redux / Flux Pattern / Context API

위에서 Pub-Sub 패턴의 Observer 패턴과 구분되는 특징은 **단일 책임의 원칙에 의거하여, 상태를 관리하는 로직을 가진 부분과, 다른 부분을 분리하는 것** 이였다.

React 에서 사용하는 전역 상태 관리에 관한 로직들을 Pub-Sub 패턴으로 생각해볼 수 있다.

![flux-pattern](https://user-images.githubusercontent.com/74234333/177138788-1abd52f1-ad9d-4a96-aff2-7a88c4e16227.png)

이를 위해 Redux에서 사용되는 Flux 패턴에 대해서 생각해보자. Flux 패턴에서보면 화살표가 한 방향으로 흘러가는 것을 볼 수 있다. view는 store에 있는 상태를 display 한다.
즉, view는 store를 subscribe 하고 있다.
여기서 store의 상태를 바꾸고 싶으면 view 에서는 action 을 날려서 dispather를 통해 store의 상태를 업데이트 할 수 있다.
즉, view에서 action을 날리고, dispatcher를 통해 상태를 변경하는 publish를 하는 것이다.

아무튼 store에 상태는 변하지 않는 읽기 전용 값이고, view는 단순히 상태를 보여주는 component일 뿐이다. 이 구조를 통해서 상태의 불변성을 유지할 수 있다.

# 참고 자료

[React / ReactJS의 옵저버 패턴](https://studysection.com/blog/observer-pattern-in-react-reactjs/#:~:text=The%20observer%20pattern%20is%20one,of%20changes%20to%20the%20subject)

[[번역] React Hooks로 알아보는 디자인 패턴](https://delivan.dev/react/programming-patterns-with-react-hooks-kr/)

[(번역) 자바스크립트에서의 옵서버 패턴 - 반응형 동작의 핵심](https://velog.io/@sehyunny/The-Observer-Pattern-in-JavaScript)

[[번역] 초보 프론트엔드 개발자들을 위한 Pub-Sub(Publish-Subscribe) 패턴을 알아보기](https://www.rinae.dev/posts/why-every-beginner-front-end-developer-should-know-publish-subscribe-pattern-kr)

[Observer 패턴과 Publisher/Subscriber(Pub-Sub) 패턴의 차이점](https://jistol.github.io/software%20engineering/2018/04/11/observer-pubsub-pattern/)

[Observer vs Pub-Sub pattern](https://hackernoon.com/observer-vs-pub-sub-pattern-50d3b27f838c)

[What is difference between observer pattern and reactive programming?](https://stackoverflow.com/questions/16652773/what-is-difference-between-observer-pattern-and-reactive-programming)
