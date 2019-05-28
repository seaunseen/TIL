# Summary

## js

### [x] 이벤트 버블링, 캡처링

- 버블링 : 하위->상위로 이벤트가 전파됨(body까지)
- 캡처링 : 버블링의 반대방향. (cature:true를 해줘야 일어남.)

**이벤트 위임**

이벤트 버블링 동작을 통해 이벤트 위임으로 응용할 수 있다.

- 만약 100개의 아이템 요소에 클릭 이벤트에 반응하는 처리를 해야한다면 100개에 바인딩 해야한다.
- 이는 코드가 매우 길어지고, 동적으로 아이템이 추가될 경우,
- 아직 추가되지 않은 요소는 DOM에 존재하지 않으므로 이벤트 핸들러를 바인딩할 수 없다.
- 이럴 때, 이벤트 위임을 사용.

상위 부모엘리먼트에 이벤트 핸들러를 바인딩 하여, 말 그대로 **이벤트를 위임** 한다.

- 이는 버블링이라는 특성을 이용한 것이다.
- 이벤트 발생시킨 요소를 알아내기 위해 `event.target`을 이용.

### [x] 실행 컨텍스트

**실행 가능한 코드가 실행되기 위해 필요한 환경**

일반적으로 실행 가능한 코드는 전역, 함수 내 코드, (eval은 제외)

코드 실행하면, EC가 스택에 생성되고 소멸한다.

_EC의 3가지 객체_

- 실행에 필요한 여러 정보들을 담을 객체 생성

1. VO(변수 객체)

- 변수, 인자, 함수 선언에 관한 정보를 담음
- 함수 컨텍스트는 AO를, 전역 컨텍스트는 GO를(인자 없음)

2. Scope chain

- 함수가 선언된 당시의 EC 스코프를 가진다.
- 함수 실행 중에 변수를 만나면 AO 검색, AO에 없으면 스코프체인을 따라 검색.

3. this

- 함수 **호출 방식** 에 의해 결정

### this

- 함수 **호출 방식** 에 의해 결정
- 호출에 따라 결정되므로 **동적** 결정.
  - 선언시 결정되는 렉시컬 스코프와 다르다

_함수 호출 방식_

1. 함수 호출
2. 메소드 호출
3. 생성자 함수 호출
4. apply/call/bind 호출

- **1번은 둘다 어디에서 선언되었든 관계없이 this는 전역객체 바인딩**
- 2번에서 메소드 내부의 내부함수의 경우 this는 window, 왜냐면 호출방식이 객체의 메소드가 아닌 단순 함수 호출이므로
- 4번은 this를 전달하여 함수 호출.

_2번_

- this는 해당 메소드를 소유한 객체에 바인딩

_3번_

- new 연산자로 호출시, 해당 함수는 **생성자 함수** 로 동작

### 함수는 1급객체다?

- 변수나 객체에 저장 가능
- 함수는 파라미터로 전달 가능
- 반환 값으로 사용 가능

### 렉시컬 스코프

- **함수를 어디서 호출하였는지가 아닌, 함수 선언 위치에 따라 상위스코프 결정**
- 전자가 동적 스코프, 후자가 렉시컬 스코프

### 스코프

스코프는 **참조 하는 대상을 찾기** 위한 규칙.

모든 변수는 스코프를 갖는다.

이 스코프는 **선언 위치** 에 의해 결정된다.

- 전역 스코프는 전역 (어디서든) 참조 가능
- 지역 스코프는 선언된 그 지역과 그 아래 지역에서 참조 가능

#### block-level(C language) VS function-level scope

_block-level_

- block단위로 스코프 결정. (if문 내에서 선언한 변수는 if문 밖에서 접근 불가.)

_function-level_

- 함수 코드 블록 내에서 선언된 변수는 함수 코드 블록 내에서만 유효.

> ES6에서 도입된 `let`은 블록 레벨 스코프 사용 가능.

### 프로토타입과 함수

**함수**

일반 객체와 다르게, 함수 객체만의 표준 프로퍼티가 있음

> ES5 명세에 모든 함수가 `prototype` 프로퍼티를 가져야한다고 명시.
> prototype : 프로토타입 객체를 가리킨다.
> `__proto__` : 객체 자신의 부모 역할을 하는 프로토타입 객체.

어떤 함수가 생성자로 동작하여, 객체를 생성한다고 하자.
이 객체의 **proto**는 생성자함수.prototype을 가리킨다.

```js
// 상속

function Parent(name) {
  this.age = 15;
}

Parent.prototype.sayHi = function() {
  console.log("hi ", this.name);
};

function Child() {
  this.name = name;
}

Child.prototype = new Parent();

Child.prototype.sayHi = function() {
  console.log("안녕 ", this.name);
};
```

### 클로저

__2개의 함수로 만들어진 환경__

- 클로저가 생성될 때 그 범위에 있던 여러 지역 변수들이 포함된 context임.

__클로저 생성하기__

- 내부함수를 외부함수의 반환값으로 사용한다.
- 내부 함수는 외부 함수의 실행 환경에서 실행된다.
- 내부 함수에서 사용되는 변수는 외부 함수의 변수 스코프에 있다.

> 함수 outerFunc는 내부함수 innerFunc를 반환하고 생을 마감했다. 즉, 함수 outerFunc는 실행된 이후 콜스택(실행 컨텍스트 스택)에서 제거되었으므로 함수 outerFunc의 변수 x 또한 더이상 유효하지 않게 되어 변수 x에 접근할 수 있는 방법은 달리 없어 보인다. 그러나 위 코드의 실행 결과는 변수 x의 값인 10이다. 이미 life-cycle이 종료되어 실행 컨텍스트 스택에서 제거된 함수 outerFunc의 지역변수 x가 다시 부활이라도 한 듯이 동작하고 있다. 뭔가 특별한 일이 일어나고 있는 것 같다.

이처럼 자신을 포함하고 있는 외부함수보다 내부함수가 더 오래 유지되는 경우, 외부 함수 밖에서 내부함수가 호출되더라도 외부함수의 지역 변수에 접근할 수 있는데 이러한 함수를 클로저(Closure)라고 부른다.

>내부 변수는 하나의 클로저에만 종속될 필요는 없다. 외부 함수가 실행될 때마다 새로운 scope chain과 새로운 내부 변수를 생성한다.(실행될 때마다 새로운 유효 범위를 생성하는데 이는 메모리 누수를 발생시킬 수 있다) 또, 클로저가 참조하는 내부 변수는 실제 내부 변수의 복사본이 아닌 그 내부 변수를 직접 참조한다.

__응용 : 캡슐화(정보 은닉)와 모듈 패턴__

```js
var person = function(arg) {
  var name = arg ? arg : '';

  return {
    getName: function() {
      return name;
    },
    setName: function(arg) {
      name = arg;
    }
  }
}
```

__클로저가 가장 유용한 경우 : 현재 상태를 기억하고 변경된 최신 상태를 유지__

- 클로저를 사용하지 않고 현재 상태를 기억하려면  전역 변수 사용해야함


## frontend

### CORS

Same Origin Policy 정책을 따르기 때문에 발생.
프로토콜, 호스트명, 포트가 같아야 한다.

웹보안을 위한 좋은 정책인데 개발시 귀찮은 정책.

__Cross-Origin Resource Sharing__ (다른 서버의 리소스 사용가능)
- SOP은 클라이언트인 웹 브라우저가 결정.
- CORS는 `Access-Control-Allow-Origin` 헤더를 추가하여 동작하므로 서버의 응답을 가로채서 이 헤더를 추가해주면 되긴됨(웹팩 프록시나 다른 플러그인들 이렇게 동작)

CORS 설정하기
- Same Origin이 아닐 경우, 브라우저는 preflight를 보낸다.(OPTIONS 메소드로 요청을 미리 날려보고 권한 확인)
- 서버에서 권한이 확인되면 `Access-Control-Allow-Origin`에 요청한 URL 추가해줌.

### 스크립트 태그의 위치

- HTML 구조와 CSS 스타일을 렌더링하는 도중 JS 를 만나면,
  - JS 해석과 구현이 완료될 때까지 브라우저 렌더링을 멈춘다.
  - 이 때 화면이 멈추는 현상이 발생할 수 있는데, 이는 사용자 경험에 좋지 않다.
- 때문에, JS 삽입 위치에 따라 스크립트 실행순서와 브라우저 렌더링에 영향을 미친다.

- 즉, head태그나 body태그 내부에 가장 상단에 위치하면
  - DOM을 해석하는 중에 스크립트를 읽기 때문에 장시간 화면 노출 안될 수 있음.
- 대부분 document.onload나 바디 태그 내부에 최하단에 위치 시킨다.

### 자바스크립트는 어떻게 동작하는가?

1. 메모리 힙: 메모리 할당 일어남
2. 콜스택 : 코드 실행되면서 스택 프레임 쌓임
3. 이벤트 루프 : 호출 스택이 비워질 때마다 큐에서 콜백함수를 꺼내와서 실행
  - 이벤트 루프는 '현재 실행중인 태스크가 없을 때'(주로 호출 스택이 비워졌을 때) 태스크 큐의 첫 번째 태스크를 꺼내와 실행한다.
4. 태스크 큐 : 콜백 함수들이 대기하는 큐 (FIFO)
  - 모든 비동기 API들은 작업이 완료되면 콜백 함수를 태스크 큐에 추가한다.

5. 마이크로 태스크
  - Promise도 비동기라 태스크 큐에 추가될거 같지만, 얘는 다르게 마이크로 태스크를 사용.
  - 마이크로 태스크는 일반 태스크보다 더 높은 우선순위를 갖는다.
  - 즉 태스크에 대기중인 태스크가 있더라도 마이크로 태스크가 먼저 실행된다.

> 웹 워커 : 각각 독립적인 이벤트 루프

__런타임__

