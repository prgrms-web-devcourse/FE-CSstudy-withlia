# 🔍목차

1. [이벤트 제어가 필요한 순간](#1-이벤트-제어가-필요한-순간)
2. [Debounce](#2-debounce)<br>
3. [Debounce 사용방법](#3-debounce-사용방법)<br>
4. [Throttle](#4-throttle)
5. [Throttle 사용방법](#5-throttle-사용방법)<br>
6. [정리](#6-정리)<br>

---

<br><br>

# 1 이벤트 제어가 필요한 순간

돔 이벤트 발생 시 콜백을 실행하도록 했다면 이벤트가 필요 이상으로 발생할 때 제어가 필요합니다. 예를 들어 textarea에 keyup이 될 때 마다 post 요청을 보낸다고 가정해 보겠습니다.

```js
    textarea.addEventListener('keyup', (e) => {
        postApi({
            ...this.state
            content: e.target.value
        })
    })

    const postApi = async (state) => {
      await request(url,state)
    }
```

이 경우 키가 눌릴 때마다 해당 api에 요청을 보내고, 철자 하나하나가 문서로 저장되기 때문에 '워크시트'라는 제목으로 요청을 보내고 싶었던 사람은 다음과 같이 끔찍한 결과를 맞이합니다.

![image](https://user-images.githubusercontent.com/79133602/165584418-156984d8-9641-4208-b0f7-a759bbdfca98.png)

글목록이 의미 없는 제목들로 가득차는 결과적 문제도 있지만 더 큰 문제들은 따로 있습니다.

바로 성능과 비용 문제입니다.

겨우 4글자를 쓰는데 10번의 요청을 보냈다면 제대로 된 글을 쓸 땐 셀 수 없는 요청을 보내게 될 겁니다. 당연히 사이트는 과도한 콜백을 처리하느라 빠르게 작동하지 못할 테고 이는 사용자 경험을 떨어뜨립니다.

게다가 유료 api를 사용하는 경우 비용이 많이 나오게 됩니다.

그렇다면 이렇게 이벤트가 과하게 발생하는 경우 어떤식으로 문제를 해결하면 좋을까요?

<br/><br/>

# 이벤트 제어 : Debounce & Throttle

돔 이벤트와 함수사이에 제어계층을 만들면 해당 문제를 해결할 수 있는데, 주로 디바운스와 쓰로틀기법을 사용합니다.

# 2 Debounce

> 연이어 실행되는 함수 중 마지막 함수(또는 제일 첫번째)만 실행

디바운스는 이벤트를 그룹화해서 특정 시간이 지난 후 하나의 이벤트만 발생하도록 하는 기법입니다.

![image](https://user-images.githubusercontent.com/79133602/165986879-b33486bd-77cb-462d-9412-78114fb86256.png)

예로 그림을 보시면 위 Raw events over time의 막대는 이벤트 발생 내역입니다. 여기에 0.4초로 디바운싱을 하면 이벤트가 0.4초동안 발생하지 않았을 때 마지막 이벤트로 함수 호출을 합니다.

<br/>

# 3 Debounce 사용방법

이벤트 발생시 호출하는 함수에 setTimeout()을 줘서 일정시간이후의 이벤트만 유효하게 만들면 됩니다.

## 3.1 예시: 글 자동 저장

아까 처음 언급했던 keyup 이벤트가 발생할 때마다 글을 저장하는 함수 호출 문제도 디바운싱으로 해결할 수 있습니다.

```js
textarea.addEventListener('keyup', async (e) => {
    let timer = null

    if(timer){
        clearTimeout(timer)
    }

    timer = setTimeout(
      await postApi({
          ...this.state
          content: e.target.value
    }),일정시간)
})
```

'워크시트'를 다 입력할 때까지 여러번 keyup 이벤트가 발생하는데, 이때 setTimeout() 함수를 쓰면 일정시간이 지날 때까지 함수 호출을 하지 않습니다.

여기서 중요한 점은 setTimeout()이 이벤트가 발생할 때마다 새로 만들어진다는 사실입니다.

위 디바운스 기법으로 만든 코드를 보면 , 먼저 setTimeout 함수를 변수에 (여기선 timer) 담아 초기화하고, 혹시 이전에 만든 변수가 존재한다면 해당 변수의 setTimeout()을 없애고 새로 setTimeout()을 만들어 줍니다.

만약 clearTimeout(timer)를 하지 않는 다면 10개의 이벤트마다 setTimeout이 존재하게 되므로 꼭 필요한 코드입니다.
<br><br>

## ⚠ 주의

> 혹시 헷갈리신 분이 있을진 모르겠지만

전 처음에 setTimeout()의 두번째 매개변수로 주는 지연시간이 글을 다 입력할 때까지 필요한 시간이라고 생각했습니다. 그래서 10000, 즉 10초를 줬는데, 해당 시간은 keyup을 마지막으로 하고 난 다음 아무것도 하지 않은 순간을 의미했습니다. 그래서 300, 0.3초정도면 충분했습니다.

<br><br>

## 3.2 예시: 회원가입 유효성 검사

회원 가입시 입력값의 유효성 검사를 할 때도 디바운싱을 사용할 수 있습니다.

<p class="codepen" data-height="300" data-default-tab="result" data-slug-hash="mVGVOL" data-user="dcorb" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/dcorb/pen/mVGVOL">
  Debouncing keystrokes Example</a> by Corbacho (<a href="https://codepen.io/dcorb">@dcorb</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

글자가 전부 입력됐을 때 마지막 결과물만 검사를 하면 효율성을 높일 수 있습니다.

<br/><br/>

## 3.3 예시: 창조절

창조절 이벤트에도 디바운스를 사용하면 효율적입니다. 창 너비가 500px미만이 되면 돔 조작 함수를 호출할 때 만약 resize될 때마다 조건문을 실행하면 500px이하에서 창조절시 불필요한 함수 호출이 발생합니다.

아래 예시를 보면 아시겠지만 디바운스를 주고 안주고에 따라 창조절 이벤트의 횟수가 확연히 차이가 납니다.

<p class="codepen" data-height="300" data-default-tab="result" data-slug-hash="XXPjpd" data-user="dcorb" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/dcorb/pen/XXPjpd">
  Debounce Resize Event Example</a> by Corbacho (<a href="https://codepen.io/dcorb">@dcorb</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

어짜피 최종적으로 결정한 창크기만이 유의미하기 때문에 다음과 같이 디바운싱을 해서 최적화를 해줘야 합니다.

이번엔 클로저를 이용해서 모듈화된 디바운스 함수를 사용해보겠습니다.

```js
const debounce = (fn, delay) => {
  let timer = null
  return function () {
    clearTimeout(timer)

    timer = setTimeout(fn, delay)
  }
}
```

디바운스 함수의 매개변수로 이벤트 발생시 호출할 함수와, 마지막 이벤트로부터 지연시간을 받습니다. 원리는 아까와 같지만 재사용성, 가독성이 좋기에 추천하는 방법입니다.

```js
const fn = () => {
  돔조작
}

window.addEventListener('resize', (e) => {
  if (window.innerWidth < 500) {
    debounce(fn, 300)
  }
})
```

창조절 이벤트가 발생할 때 호출할 함수를 디바운스로 감싸주면 해당 창조절이 더이상 일어나지 않을 때 일정시간 이후 돔조작을 합니다.

하지만 디바운스 방법엔 문제점이 있는데요 즉각적으로 함수를 호춯해 결과물을 화면에 나타나야 되는 순간 setTimeout()때문에 아무 것도 보여줄 수 없습니다.

예를 들어 스크롤을 내릴 때 디바운스 기법으로 api를 호출하면 사용자는 스크롤을 내려도 한동안 아무 것도 볼 수 없습니다.

<br/><br/>

# 4 Throttle

> 일정 시간 동안 한번만 실행되도록 제한

그럴 땐 쓰로틀을 사용하면 됩니다. 쓰로틀은 일정 기간을 주기로 함수 호출을 보장합니다. 때문에 디바운스와 달리 마지막 이벤트를 기다릴 필요가 없고 계속 스크롤 이벤트를 주더라도 중간중간 페이지를 가져올 수 있습니다.

![image](https://user-images.githubusercontent.com/79133602/165988811-2ce1f4f8-8bdc-4152-8441-0bf7bb970832.png)

## 5 Throttle 사용방법

이벤트가 발생할 때마다 throttle의 매개변수에 호출할 함수와 일정시간을 입력합니다. 그리고 timer가 false인 경우에만 일정시간이 지난 후 함수를 실행합니다.

```js
const throttle = (fn, delay) => {
  return function () {
    let timer

    if (!timer) {
      timer = setTimeout(() => {
        fn()
        timer = null
      }, 일정시간)
    }
  }
}
```

여기서 눈여겨 볼 점은 timer가 false인 조건문입니다. 해당 조건 덕에 쓰로틀은 디바운스와 달리 처음 이벤트 이후 계속 이벤트가 추가되도 첫 이벤트의 함수를 실행합니다. 그리고 다시 timer를 null로 만들기 때문에 일정시간마다 다음 이벤트를 받아올 수 있습니다.

## 5.1 예시: 노션 목차

노션 클론으로 만든 사이트의 제목은 현재 디바운스로 바뀌고 있습니다.

![노션 제목 디바운싱](https://user-images.githubusercontent.com/79133602/166110501-3d1c76a6-c49f-42d8-af8c-98f25c4e2583.gif)

하지만 실제 사이트는 쓰로틀링 기법을 써서 변경사항을 사용자가 실시간으로 볼 수 있게 해줍니다.

![노션 제목 쓰로틀링](https://user-images.githubusercontent.com/79133602/166110499-523b9cab-2d03-47f3-b140-c09d5301eca1.gif)

당연히 사용자 입장에선 쓰로틀링 기법이 더 편할 것입니다.

<br/><br/>

# 6 정리

> 적재적소의 기법을 찾자

디바운스와 쓰로틀의 차이점은 연속적인 이벤트가 끝나길 기다리냐의 여부입니다. 디바운스는 연속적인 이벤트가 다 끝나고 나서 마지막 이벤트만 실행하도록하고, 쓰로틀은 마지막 이벤트를 기다리는 대신 일정시간마다 이벤트를 실행합니다.

아래 예시 처럼 모든 이벤트를 처리할 때, 쓰로틀 기법을 쓸 때, 디바운스 기법을 쓸 때 이벤트 내 함수를 실행하는 횟수는 다릅니다.

<p class="codepen" data-height="300" data-default-tab="result" data-slug-hash="jXrYQz" data-user="jaehee" style="height: 300px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;">
  <span>See the Pen <a href="https://codepen.io/jaehee/pen/jXrYQz">
  The Difference Between Throttling, Debouncing, and Neither</a> by jaeheekim (<a href="https://codepen.io/jaehee">@jaehee</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

그러므로 최적화가 필요하지만 주기적인 이벤트 호출이 필요하다면 쓰로틀을 마지막 이벤트만 유효하게 하고 싶다면 디바운스를 선택해야 합니다.

> 디바운스가 필요하지만 마지막을 기다리고 싶지 않다면?

만약 디바운스를 쓰고 싶지만 화면이 뜰 때까지 기다리는 게 싫다면 낙관적 업데이트를 하면 됩니다.

## 낙관적 업데이트

> 데이터 추가 요청 성공여부와 상관없이 업데이트

낙관적 업데이트는 데이터 입력 후 해당 데이터가 서버에 잘 들어갔는지 확인하지 않고 직접 데이터를 기존 데이터에 추가해주는 행위입니다.

## 예시: 글 자동 저장

만약 글을 자동저장할 때 post 요청 후 get요청으로 값을 받을 때까지 화면 변경이 되지 않는다면 사용자 입장에서 불편할 것입니다. 이때 다음과 같이 post요청 전 입력값을 현재 state에 추가하는 setState함수를 쓰면 낙관적 업데이트를 할 수 있습니다.

```js
timer = setTimeout(
  this.setState({
    ...this.state,
    content: e.target.value
  })
  postApi({
      ...this.state
      content: e.target.value
}),일정시간)
```

이경우 get 요청이 실행될 때까지 기다릴 필요가 없고 즉각적으로 변경사항이 반영되기 때문에 사용자 친화적인 사이트를 구현할 수 있습니다.

<br/><br/><br/>

# 5 참고

💻 [디바운스와 스로틀 그리고 차이점](https://webclub.tistory.com/607)

💻 [Throttle, Debounce 개념 잡기](https://medium.com/@progjh/throttle-debounce-%EA%B0%9C%EB%85%90-%EC%9E%A1%EA%B8%B0-19cea2e85a9f)

💻 [쓰로틀링과 디바운싱](https://www.zerocho.com/category/JavaScript/post/59a8e9cb15ac0000182794fa)

💻 [CodePen .debounce vs \*.throttle](https://codepen.io/sumolari/pen/MWKzwgb)

💻 [CodePen Debouncing keystrokes](https://codepen.io/dcorb/pen/mVGVOL)

💻 [CodePen_Debounce Resize](https://codepen.io/dcorb/pen/XXPjpd)

💻 [CodePen The Difference Between Throttling, Debouncing, and Neither](https://codepen.io/jaehee/pen/jXrYQz)
