## V8 Object property 저장 방식

V8에서 객체라면( key가 string | Sybol ) Properties라는 별도의 배열 구조에 속성들을 저장합니다. 이 때 배열과 달리 key로 Properties 내 속성의 위치를 파악할 수 없습니다. (key ≠ index)

![image](https://user-images.githubusercontent.com/79133602/199002545-f294e29e-28cb-49f9-a719-bd0096c783db.png)

> Named properties are stored in a similar way in a separate array. However, unlike elements, we cannot simply use the key to deduce their position within the properties array; we need some additional metadata.

그래서 속성의 위치를 메모리에 저장하는데, 이를 컴파일 시 알 수 없습니다.

<br/>

### 다른 언어는?

> **정적 타입은 컴파일 시 속성의 위치를 알 수 있다!**

정적타입 언어들은 컴파일 시 타입이 결정되기에 속성의 메모리 위치를 실행 전에 결정할 수 있습니다. 하지만 자바스크립트는 런타임 시 객체의 속성이 동적으로 변하기에 그럴 수 없습니다.

### 그럼 속성의 메모리 위치를 어떻게 찾지?

> **속성 위치 따로 저장 + 동적 탐색**

이를 해결하기 위해 자바스크립트는 딕셔너리와 유사한 구조로 속성의 위치를 메모리에 저장합니다.

> **동적 조회가 필요해 성능 저하**

하지만, 한 가지 문제가 있습니다. 자바스크립트가 동적 타입의 언어라는 점입니다. 자바스크립트는 객체가 생성된 이후에도 속성을 추가하거나 삭제할 수 있기 때문에 속성의 위치가 광범위해질 수 밖에 없습니다. 이렇게 범위가 넓다면 크기가 지정된 다른 언어들에 비해 탐색 범위가 넓어서 비효율적일 수 밖에 없습니다.

<br/>

## 히든 클래스

> 객체 모양, 속성을 인덱스로 저장, 인라인 캐싱

이를 해결하기 위해 자바스크립트 엔진(V8)은 히든 클래스를 사용합니다. 히든 클래스는 정적타입 언어에서 사용하는 고정 객체 레이아웃과 유사하게 작동합니다. 다만, 런타임에 생성된다는 차이가 있습니다.

### 예시

예를 들어 Student라는 생성자함수로 kid라는 인스턴스를 만들어보면 다음과 같은 순서로 진행됩니다.

```jsx
function Student(name, age) {
  this.name = name;
  this.age = age;
}
let kid = new Student("happy", 18);
```

1. new Student('happy',18)가 실행되는 시점, 아직 [this.name](http://this.name) = name을 읽어들이기 전 인스턴스는 빈 객체 {}
2. [this.name](http://this.name) = name이 수행되면 name: ‘happy’를 객체에 추가
3. [this.](http://this.name)age = age이 수행되면 age: 18을 객체에 추가

### **여기서 히든클래스가 어떻게 사용되는가?**

히든 클래스엔 Transition Table, Property Table이 존재합니다.

- **Transition Table**
  이정표, **전환 정보**를 담고 있어서 특정 property가 추가되면 어떤 히든 클래스로 이동해야되는 지 알 수 있다. 런타임 시 필드 구조가 변할 때 생성, 포인터로 클래스 전환
  - **전환정보 :** 새로운 히든 클래스가 생성되고 해당 클래스를 참조하게 되면 이전 클래스에 어떤 프로퍼티로 해당 전환이 일어났는지 기록
- **Property Table**
  property의 메모리 위치 기록, offset의 값에 속성 값 기록

![image](https://user-images.githubusercontent.com/79133602/199002624-a70f264f-3d45-4233-bee1-4c09c6896e1d.png)

1. **오프셋 없이 히든 클래스 생성 (HiddenClass0),**

   ⇒ kid는 HiddenClass0 참조

2. **HiddenClass0을 상속받은 자식 HiddenClass1의 오프셋 0에 name 프로퍼티 할당**

   ⇒ kid는 HiddenClass1 참조

   ⇒ **HiddenClass0에 전환정보담기**( name을 추가하면 HiddenClass1로 전환된다.)

3. **HiddenClass1을 상속받은 자식 HiddenClass2의 오프셋 1에 age 프로퍼티 할당**

   ⇒ kid는 HiddenClass2 참조

   ⇒ **HiddenClass1에 전환정보담기**( age를 추가하면 HiddenClass2로 전환된다.)

### 쉬운 비유법

> 광교 찾아가기

지역을 속성, 해당 지역 찾아가기를 속성의 메모리 주소 찾기라고 생각해보겠습니다. 이 때 hiddenClass는 길 찾기 정보가 될 것입니다.

hiddenClass 길 찾기 정보가 없이 여러분에게 광교로 오라고 하면, 찾기가 어렵습니다. 반면, 여러분이 사는 곳에서 광교로 오는 신분당선 hiddenClass 정보가 있다면 쉽게 찾아 올 수 있습니다.

<br/>

## 히든 클래스 최적화

덕분에 새로운 Student 인스턴스를 만들 때도 새로 히든 클래스를 만드는 게 아니라 기록된 전환 정보를 따라 클래스를 참조해 자원 낭비를 최소화할 수 있습니다.

### 인라인 캐싱

> 객체 접근 시 조회 작업 생략

이전에 조회했던 오프셋을 캐싱해 특정 프로퍼티의 메모리주소로 바로 이동할 수 있게 하는 최적화 방식입니다.

- 예) name 프로퍼티 실제 메모리 주소 위치를 offset에 저장 ⇒ offset에 저장된 위치로 바로 이동

### 주의: 인라인 캐싱이 안되는 경우

복잡한 지하철 노선도 + 길찾기 어플이 없다면?

```jsx
function Student(name, age) {
  this.name = name;
  this.age = age;
}
let kid = new Student("happy", 18); // name, age는 인라인 캐싱 가능
let kid2 = new Student("sad", 19);
kid.grade = "A"; // 인라인 캐싱 안돼
kid.gender = "M"; // 인라인 캐싱 안돼
```

인스턴스의 구조가 동일하지 않다면 각각 히든 클래스를 따로 만들기 때문에 인스턴스의 속성값을 비교해야 되서 속성 조회작업을 생략할 수 없습니다.

따라서 인스턴스의 구조를 동일하게 짜도록 해야 좋습니다.

<br/><br/><br/>

## 참고 💻

💻 [Map - JavaScript | MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)

💻 [Fast properties in V8](https://v8.dev/blog/fast-properties)

💻 [V8 엔진에 대해서](https://lee33398.tistory.com/32)

💻 [객체에 프로퍼티 설정 시 성능 이슈](https://ohgyun.com/764)

💻 [제발 한국인이라면 자바스크립트 Object를 Map 처럼 사용하지 맙시다.](https://shanepark.tistory.com/220)

💻 [자바스크립트는 어떻게 작동하는가: V8 엔진의 내부 + 최적화된 코드를 작성을 위한 다섯 가지 팁](https://engineering.huiseoul.com/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%9E%91%EB%8F%99%ED%95%98%EB%8A%94%EA%B0%80-v8-%EC%97%94%EC%A7%84%EC%9D%98-%EB%82%B4%EB%B6%80-%EC%B5%9C%EC%A0%81%ED%99%94%EB%90%9C-%EC%BD%94%EB%93%9C%EB%A5%BC-%EC%9E%91%EC%84%B1%EC%9D%84-%EC%9C%84%ED%95%9C-%EB%8B%A4%EC%84%AF-%EA%B0%80%EC%A7%80-%ED%8C%81-6c6f9832c1d9)

💻 [자바스크립트 성능의 비밀 (V8과 히든 클래스)](https://ui.toast.com/posts/ko_20210909)

💻 [V8의 히든 클래스 이야기](https://engineering.linecorp.com/ko/blog/v8-hidden-class/)
