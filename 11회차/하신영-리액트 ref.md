## React Ref란?

> reference의 줄임말로 참고하는 대상

리액트의 Ref는 React 엘리먼트나 DOM 노드를 참고할 수 있게 해주는 객체입니다. 노드에 Ref 속성을 지정해 뒀다면, 렌더링 시 생성된 React 엘리먼트 또는 DOM 노드가 current속성에 담깁니다. 따라서 current를 통해 노드 또는 엘리먼트를 조작할 수 있습니다.

<br/>

### Ref의 값은 노드에 따라 달라

아래 노드들이 렌더링 되고 `useEffect`로 `console.log(ref)`를 찍으면 어떤 값이 나올까요?

```jsx
<input ref={ref}/> //HTMLElement
<Input ref={ref}/> //클래스형 컴포넌트
<Input ref={ref}/> //함수형 컴포넌트
```

<details>
<summary>답</summary>
    1. HTMLElement : React.craeteRef()로 생성된 ref는 current 프로퍼티에 DOM 엘리먼트 받음
    2. 커스텀 클래스 컴포넌트:  current에 마운트된 컴포넌트의 인스턴스
    3. 함수 컴포넌트: 인스턴스가 없기에 ref 속성 사용못해 
  
</details>

<br/>

![image](https://user-images.githubusercontent.com/79133602/195910253-ceecd2ed-7e65-48c2-8ade-b87fdd1aa6ec.png)

<br/>

## Ref를 사용하는 경우

- focus, 텍스트 선택 영역, 미디어 재생 관리,
- 애니메이션 직접 실행
- 서드 파티 DOM 라이브러리를 React와 사용할 때
- 단, 선언적으로 해결할 수 있는 문제는 ref 지양

버튼을 누르면 노래를 재생하고 정지할 수 있는 MusicPlayer가 있다고 해보겠습니다. 현재, audio와 button 태그가 상태값을 공유하고 있지 않는데요. 어떻게 연동이 되길래 button으로 audio 태그를 조작할 수 있는 걸까요?

![image](https://user-images.githubusercontent.com/79133602/195910503-6c5a63d8-85b5-4aae-88c1-17210164ab4d.png)

<br/>

답은 당연히 audio의 ref를 사용한 것이겠죠.

```jsx
const audioRef = useRef < HTMLAudioElement > null;

<audio ref={audioRef} />;
```

<br/>

그렇다면 코드를 보면서 어떤식으로 ref를 사용했는 지 알아보도록 하겠습니다. 아래는 MusicPlayer의 전체코드입니다. 코드를 보시면 재생, 정지 버튼을 누를 때 각각 `handlePlay, handlePause` 함수를 호출하고 있습니다. 그리고 해당 함수들은 렌더링이 후 audioRef.current에 audio 노드가 담겼을 때 `play, pause` 메서드를 사용해 audio를 조작합니다.

```jsx
import { useRef } from "react";

const MusicPlayer = () => {
  const audioRef = useRef < HTMLAudioElement > null;

  const handlePlay = () => {
    audioRef.current?.play();
  };

  const handlePause = () => {
    audioRef.current?.pause();
  };

  return (
    <>
      <figure>
        <figcaption>music-player</figcaption>
        <audio
          ref={audioRef}
          controls
          src="https://drive.google.com/uc?export=download&id=1Yb2IL4-3fvwgzxLcIqPWRrdugXFsJVxA"
        >
          <a href="https://drive.google.com/uc?export=download&id=1Yb2IL4-3fvwgzxLcIqPWRrdugXFsJVxA">
            Download audio
          </a>
        </audio>
      </figure>
      <button onClick={handlePlay}>재생</button>
      <button onClick={handlePause}>정지</button>
    </>
  );
};

export default MusicPlayer;
```

그런데 여기서 한 가지 의문이 생깁니다. 바닐라 자바스크립트에선 DOM API를 사용해서 직접 요소에 접근했는데, 왜 리액트는 Ref를 사용하도록 권장하는 걸까요?

<br/>

### DOM API를 사용하면 안돼?

> DOM API는 정확성이 떨어져서 지양!

리액트는 가상돔으로 실제돔을 그리기 때문에 실제돔을 조작하는 DOM API를 사용하면 정확성이 떨어집니다. 해당 돔이 현재 가상돔으로 그려낼 실제 돔인지 알 수 없기 때문입니다.

또, 리액트 시스템을 벗어나 직접 실제 돔을 조작한다면 라이프사이클에 맞춰서 돔요소를 가져올 수 없습니다. 라이프 사이클을 예측하지 못한 다면 잘 못 된 값을 가져올 수 있기에 이 역시 정확성이 떨어집니다.

반면, 리액트의 Ref 객체는 리액트의 라이프사이클에 맞춰 동작합니다. 공식문서엔 다음과 같이 설명하고 있습니다.

<br/>

> _컴포넌트가 마운트될 때 React는 current 프로퍼티에 DOM 엘리먼트를 대입하고, 컴포넌트의 마운트가 해제될 때 current 프로퍼티를 다시 null로 돌려놓습니다. ref를 수정하는 작업은 componentDidMount 또는 componentDidUpdate 생명주기 메서드가 호출되기 전에 이루어집니다._

<br/>

이를 좀 더 쉽게 설명하자면, Ref는 렌더링이 끝난 뒤 (componentDidMount) 또는 업데이트로 리렌더링이 끝난 뒤 (componentDidUpdate) current에 노드가 담깁니다. 즉, 실제돔에 리액트 노드가 렌더될 때까지 ref엔 값이 담기지 않기에 정확한 값을 받을 수 있습니다.

때문에 리액트는 리액트의 Lifecycle을 따르지 않아 정확한 값을 보장할 수 없는 DOM API 대신 Ref를 사용해서 변경된 값을 안정적으로 받아오길 권장합니다.

<br/>

## Ref 사용법

리액트에서 Ref 사용법은 네가지가 있습니다.

### 1. createRef

> class 객체형 컴포넌트에서 ref 생성 시 사용하던 메서드

```jsx
class Form extends Component {
  state = {
    value: "",
  };

  inputRef = React.createRef();

  handleChange = (e) => {
    this.setState({ value: this.inputRef.current.value });
  };

  render() {
    return <input ref={this.inputRef} onChange={handleChange} />;
  }
}
export default Form;
```

함수형에서도 사용 할 수 있지만 함수형 컴포넌트에서는 상태가 바뀔 때 마다 리렌더링이 발생하기 때문에 createRef도 여러번 호출됩니다. 이러면 리액트 가상돔의 diffing 알고리즘을 통과해 새로 그려지지 않을 떄도 ref 객체가 계속 만들어 지기 떄문에 문제가 됩니다.

<br/>

### 2. useRef

> 위 현상을 해결하기 위한 API

```jsx
function Form() {
  const [value, setValue] = useState("");
  const inputRef = useRef(); // createRef()면 input 요소가 새로 만들어지지 않아도
  //렌더링 될 때마다 ref객체를 만들어버림
  const handleChange = (e) => {
    setValue(inputRef.current.value);
  };

  return <input ref={inputRef} onChange={handleChange} />;
}

export default Form;
```

**DOM요소가 새로 그려질 때만 ref 객체를 생성**하기 때문에 input 요소가 onChange될 떄마다 렌더링이 발생해도 **inputRef객체는 한 번만 생성**됩니다.

<br/>

### 3. callbackRef

별도의 API가 아니라 ref 속성을 활용하는 방식입니다. ref에 노드가 할당되는 순간 특정 작업을 처리하고 싶을 때 사용하는데, ref 속성에 특정 작업을 처리하는 콜백함수를 넣어주기에 callbackRef라고 부릅니다. 그런데, 이 경우 current 속성은 커녕 Ref 객체도 아닌데 요소 값은 어떻게 받을까요?

```jsx
function Test() {
  const callbackRef = (element) => {
    console.log(element);
  };

  return (
    <div ref={callbackRef}>
      <button></button>
      <button></button>
    </div>
  );
}

export default Test;
```

![image](https://user-images.githubusercontent.com/79133602/195910814-6ed1f4b4-afba-408f-954e-ab48a7dee531.png)

노드가 생성될 때 콜백함수의 인자로 해당 노드를 받아 올 수 있습니다. 위 예시를 보면 element를 인자로 받아 콘솔에 요소가 찍히는 것을 알 수 있습니다.

<br/>

**callbackRef는 리렌더링을 발생시키지 않도록!**

`ref는 리렌더링을 발생시키지 않는다` 는 목적에 부합하도록 코드를 짤 필요가 있습니다. 따라서, 콜백에서 직접 비즈니스 로직을 처리하는 대신 해당 ref 노드를 setState한뒤 state가 의존성 배열값인 useEffect 안에서 비즈니스로직을 처리하도록 하면 더 정확하겠죠.

<br/>

다음 예시는 react-query의 useinfinitequery를 위해 IntersectionObserver로 감시할 `InfiniteScrollDiv` 에 callbackRef를 사용한 경우입니다. 현재 `InfiniteScrollDiv` 는 `hasNextPage`가 true인 경우에만 ref가 생성되는데요. 이 때 콜백으로 해당 요소를 setState하고 useEffect에서 intersectionObserver로 무한 스크롤 fetch를 처리하면 ref 대신 state로 리렌더링이 발생하는 모습이 됩니다.

```jsx
const Search = () => {
  const [targetRef, setTargetRef] = useState<HTMLDivElement>();

	//생략...

  const callbackRef = useCallback((node: HTMLDivElement) => {
    if (node !== null) {
      setTargetRef(node);
    }
  }, []);

  useEffect(() => {
    if (!targetRef) {
      return;
    }

    const observer: IntersectionObserver = new IntersectionObserver(
      onIntersect,
      options
    );
    observer.observe(targetRef);

    return () => observer && observer.disconnect();
  }, [targetRef]);

  return (
      <List>
        {searchResult.pages.map((group, i) => (
              <Item key={i} item={item}/>
        )}
        {hasNextPage && (
					//hasNextPage때문, 그냥 ref면 못 찾아, 콜백 ref로 하면 useState에 값을 넣고, setState되는 시점에 부수효과 줄 수 있다,
          <InfiniteScrollDiv ref={callbackRef}/>
        )}
      </List>

  );
};

export default Search;

```

### 4. forwardRef

> 함수형 컴포넌트에서 자식 컴포넌트의 prop으로 ref를 넘겨주고 싶을 떄 사용

리액트에선 key, ref 같은 속성은 prop 값으로 넘겨줄 수 없습니다. 하지만 상위 컴포넌트가 하위 컴포넌트 내부 요소에 접근해야한다면 prop으로 ref를 넘겨줄 필요가 있습니다. 이 때, forwardRef를 사용하면 해결할 수 있습니다.

예로, 아까 처음 봤던 MusicPlayer 코드를 다음과 같이 추상화한다면, forwardRef가 필요할 것입니다.

```jsx
import { useRef } from "react";

const MusicPlayer = () => {
  const audioRef = useRef < HTMLAudioElement > null;
  return (
    <>
      <Audio ref={audioRef} />
      <Controller ref={audioRef} />
    </>
  );
};

export default MusicPlayer;
```

이렇게 forwardRef로 해당 컴포넌트를 감싸주면, 두번 째 인자로 ref를 받아올 수가 있습니다.

```jsx
import { forwardRef } from "react";

const Audio = forwardRef((props, ref) => {
  return (
    <figure>
      <figcaption>music-player</figcaption>
      <audio
        ref={ref}
        controls
        src="https://drive.google.com/uc?export=download&id=1Yb2IL4-3fvwgzxLcIqPWRrdugXFsJVxA"
      >
        <a href="https://drive.google.com/uc?export=download&id=1Yb2IL4-3fvwgzxLcIqPWRrdugXFsJVxA">
          Download audio
        </a>
      </audio>
    </figure>
  );
});

export default Audio;
```

ref로 audio 노드의 메서드를 조작해야하는 button 역시 forwardRef를 써서 ref를 받아오면 됩니다 .

```jsx
import { forwardRef } from "react";

const Controller = forwardRef((props, ref) => {
  const handlePlay = () => {
    ref.current?.play();
  };

  const handlePause = () => {
    ref.current?.pause();
  };

  return (
    <>
      <button onClick={handlePlay}>재생</button>
      <button onClick={handlePause}>정지</button>
    </>
  );
});

export default Controller;
```

<br/>

## useRef 응용

### 변수관리 & 최적화

가변값을 Ref로 변수관리하면 ref 값은 변해도 리렌더링이 발생하지 않기에 최적화가 가능합니다. 주로 다음과 같은 경우 사용합니다.

- setTimeout, setInterval을 통해 만들어진 id
- 외부라이브를 사용해 생성된 인스턴스
- 스크롤 위치

예로 아래 useIntervalFn 훅을 보면 setInterval로 만들어진 id를 intervalId로 관리합니다. 덕분에 id 값이 일정 주기로 변해도 리렌더링은 발생하지 않습니다. 또 콜백함수도 ref로 관리한 덕에 setInterval 시작 후 콜백 함수가 바뀌더라도 interval가 끝나지 않습니다.

```jsx
import { useEffect, useState, useRef, useCallback } from "react";

const useIntervalFn = (fn, ms) => {
  const intervalId = useRef();
  const callback = useRef(fn); // 매우 중요 -> setInterval 시작 후에 fn이 바뀌어도 interval이 끝나지 않는다.

  useEffect(() => {
    callback.current = fn;
  }, [fn]);

  const run = useCallback(() => {
    intervalId.current && clearInterval(intervalId.current);

    intervalId.current = setInterval(() => {
      callback.current();
    }, ms);
  }, [ms]);

  const clear = useCallback(() => {
    intervalId.current && clearInterval(intervalId.current);
  }, []);

  useEffect(() => clear, [clear]); // 훅이 사라질 떄도 clear를 해줘야 해 안그러면 해당 컴포넌트, 훅이 사라져도 intervalId 남아있어

  return [run, clear];
};

export default useIntervalFn;
```

<br/>

## 번외

> ref 값이 여러개라면 하나의 useRef로 관리가능

```jsx
const oneRef = useRef();
const twoRef = useRef();
const threeRef = useRef();
.
.
<div ref={oneRef}/>
const compactRef = useRef([]);
<div ref={(el)=> (compactRef.current[0] = el)/>
```

<br/>

## 의문

> 그냥 autofocus 속성 쓰면 안돼…?

```jsx
<input autoFocus />
```

<br/>

## 결론

리액트에서 돔에 접근할 떈 왜 DOM API가 아닌 Ref를 써야하는지 이유도 모른채 그냥 코딩을 하곤 했습니다. 비단 저만 그런건 아니라고 생각합니다. 몰라도 코딩은 가능하지만 정확한 작동원리를 모르면 DOM API를 무심코 사용할 수도 있고 다른 개념을 이해하는데도 어려움이 있기에 알아둘 필요가 있습니다.

이번 글을 작성하면서, 다음과 같은 내용을 배웠습니다. 여러분도 아래 개념을 숙지해 리액트 ref를 정확하게 활용할 수 있었으면 좋겠습니다.

- 가상돔을 사용하는 특성상 정확한 돔 정보를 얻기 위해선 라이프 사이클에 맞춰 동작하는 Ref가 필요
- ref는 돔 조작, 가변적인 값을 변수화할 때 사용
- 함수형 컴포넌트는 state가 변할 때 마달 리렌더링이 발생하기에 렌더링을 기준으로 Ref 객체를 만드는 createRef가 아니라 돔을 새로 그릴 때만 Ref객체를 만드는 useRef를 사용
- 컴포넌트 prop으로 ref를 내려보내고 싶다면 forwardRef를 사용
- ref가 만들어질 때 특정 로직을 처리하고 싶다면 callbackRef를 사용

<br/>
<br/>
<br/>

## 참고

[Ref와 DOM - React](https://ko.reactjs.org/docs/refs-and-the-dom.html#when-to-use-refs)

[React ref 톺아보기](https://tecoble.techcourse.co.kr/post/2021-05-15-react-ref/)

[[React🌀] Ref 에 대한 고찰 🔍 / 1️⃣ - Ref 의 활용과 useRef](https://programming119.tistory.com/265)

[[React🌀] Ref 에 대한 고찰 🔍 / 2️⃣ - useRef 와 useState, 그리고 global Variable](https://programming119.tistory.com/266)

[React에서의 querySelector, 잘 쓰고 있는 걸까?(feat. Ref)](https://mingule.tistory.com/61)

[[React] useRef는 처음이라 :: 개념부터 활용 예시까지](https://mnxmnz.github.io/react/what-is-use-ref/)
