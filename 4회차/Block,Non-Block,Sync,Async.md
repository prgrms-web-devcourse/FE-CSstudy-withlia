

# Block·Non-Block, 동기 ·비동기

## Blcok vs Non-Block

> 다른 주체가 작업할 때 **자신의 제어권이 있는지 없는지**로 볼 수있다.
> 
## Blocking

> 자신의 작업을 진행하다가 다른 주체의 작업이 시작되면 끝날 때까지 **기다렸다가** 자신의 작업을 시작하는 것
> 
## Non-Blocking

> 다른 주체의 작업에 **관련없이** 자신의 작업을 하는 것
> 
---

## 동기 vs 비동기

> 호출된 함수의 함수 **작업 완료 여부를** 신경쓰는 지 (**결과의 처리**)
> 
### Synchornous 동기

> 작업을 동시에 수행하거나 동시에 끝나거나, **끝나는 동시에 바로 시작함을 의미**
> 
### Asynchronous 비동기

> 시작, 종료가 일치하지 않으며, **끝나는 동시에 시작을 하지 않음**을 의미 (바로 처리하지 않아도 됨)
> 
## 4가지 조합

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/deb3043f-7b46-4015-9ef6-d9611f73f115/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220526%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220526T133611Z&X-Amz-Expires=86400&X-Amz-Signature=62f6f61ba0082293df519b69d3439ebcf5fe1117801c0ba3726a2c54c79c835d&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject)

### Blocking / Sync

> Blocking이므로 **다른 작업이 시작되는 동안** **동작하지 않는다.**
> 
> Sync이므로 **결과를 반환하면 그 일을 바로 처리**
> 
- Java에서 입력 요청을 할 때 사용
- 예시
```jsx
* 제가 플젝 기간 중 리아님께 외출을 하기 위해서 QR코드를 요청하기 위해 DM을 보낸다.
* 빠르게 외출처리를 하고 바로 나가야하므로
	리아님이 QR코드를 내주실 때까지 다른 일을 하지 않고 돌아오는 DM을 바라보며 **계속 기다린다
* 리아님으로부터 QR코드를 받는다.**
* 제가 QR코드를 **받자마자** HRD-Net 어플을 통해 외출처리를 한다.  
```

### Non-Blocking / Sync

> Non-Blocking이므로 다른 작업이 있어도 **자신의 제어권을 가지고 일을 함**
> 
> Sync이므로 다른 작업에 대한 **결과를 반환하면 그 일을 바로 처리**
> 
- 예시
```jsx
* 저는 이번 단위기간에 출석을 제대로 지키지 않아 출석일을 계산해보려하나
	특정 조건이 헷갈린다. 저는 리아님께 그런 특정조건에 대해서 문의를 한다.
* 리아님이 답변을 해주기 전까지 저는 데브코스 코어타임 내 공부를 하고 있는다.
* 리아님이 답변을 해주신다.
* 출석일 수에 관해 굉장히 불안한 심리를 가지고 있었기에 답변이 **오자마자 바로 계산을 해본다**.
```

### Blocking / Async

> Blocking이기 때문에 자신의 작업에 대한 제어권이 없다.
> 
> Async이므로 결과를 반환하면 그 일을 바로 처리하지 않고 **나중에 처리**
> 
```jsx
* 저는 공부 중에 리아님께 프동여지도에 대한 이메일 제출 과정에서
	구글형식의 이메일을 다시 보내달라는 연락을 받고 보낸다.
* 저는 새롭게 구성한 G-mail 주소를 보내고
	그것이 잘되었는지 확인하기 위해 리아님의 DM을 기다린다.
* 잘됐다는 답변을 받고 나중에 프동여지도를 확인해보라고 한다.
```

- 비동기인데 굳이 Blocking으로 써야할까?
    - 대개 Non-blocking / Async의 경우에서 개발자의 실수로 발생하는 경우가 있다.

### Non-Blocking / Async

> Non-Blocking이므로 다른 작업이 있어도 **자신의 제어권을 가지고 일을 함**
> 
> Async이므로 결과를 반환하면 그 일을 바로 처리하지 않고 **나중에 처리**
> 
```jsx
* 저는 리아님께 비동기적으로 문의를 드렸습니다.
 (메시지 수신자가 지금 바로 답을 할 수 있건, 없건 신경쓰지 않는다.)
* 몇시간이 지나던, 며칠이 지나던 **상관없이** 저의 공부를 하고 있는다.
* 리아님이 답변을 해주신다.
* 문의에 대한 답변 내용을 **하던 공부를 다하고 나서** **시간될 때 처리**한다.
```

- JS에서 API요청에서의 비동기 처리가 대표적 예

---

### 출처

[https://www.youtube.com/watch?v=oEIoqGd-Sns&t=539s](https://www.youtube.com/watch?v=oEIoqGd-Sns&t=539s)

[https://gyoogle.dev/blog/computer-science/network/Blocking,Non-blocking & Synchronous,Asynchronous.html](https://gyoogle.dev/blog/computer-science/network/Blocking,Non-blocking%20&%20Synchronous,Asynchronous.html)
