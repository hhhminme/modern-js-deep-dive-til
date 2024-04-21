# 23장 실행 컨텍스트

## 목차

- [23.1 소스코드의 타입](#23.1)
- [23.2 소스코드의 평가와 실행](#23.2)
- [23.3 실행 컨텍스트의 역할](#23.3)
- [23.4 실행 컨텍스트 스택](#23.4)
- [23.5 렉시컬 환경](#23.5)
- [23.6 실행 컨텍스트의 생성과 식별자 검색 과정](#23.6)
- [23.7 실행 컨텍스트와 블록 레벨 스코프](#23.7)

## 23.1 소스코드의 타입<a name="23.1"></a>

| 소스코드의 타입 | 설명                                                                                        |
| --------------- | ------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| 전역 코드       | 전역에 존재하는 소스코드. 전역에 정의된 함수, 클래스 등 내부 코드는 포함되지 않음           |
| 함수 코드       | 함수 내부에 존재하는 소스코드. 함수 내부에 중첩된 함수, 클래스 등 내부 코드는 포함되지 않음 |
| eval 코드       | 빌트인 전역 함수인 eval 함수에 인수로 전달되어 실행되는 소스코드                            |
| 모듈 코드       | 모듈 내부에 존재하는 소스코드                                                               | 모듈 내부의 함수, 클래스 등 내부 코드는 포함되지 않음 |

**1. 전역 코드**

- 전역 변수를 관리하기 위해 최상위 스코프인 전역 스코프를 생성해야 함
- 전역 코드가 평가되면 전역 실행 컨텍스트가 생성됨

**2. 함수 코드**

- 지역 스코프를 생성하고 지역 변수, 매개변수, arguments 객체를 관리함
- 함수 코드가 평가되면 함수 실행 컨텍스트가 생성됨

**3. eval 코드**

- strict mode에서 자신만의 독자적인 스코프를 생성함
- eval 코드가 평가되면 eval 실행 컨텍스트가 생성됨

**4. 모듈 코드**

- 모듈별로 독립적인 모듈 스코프 생성
- 모듈 코드가 평가되면 모듈 실행 컨텍스트 생성

## 23.2 소스코드의 평가와 실행<a name="23.2"></a>

- 모든 소스코드는 실행에 앞서 평가과정을 거치며 코드를 실행하기 위한 준비를 함
- 자바스크립트 엔진은 소스코드를 소스코드의 평가와 소스코드의 실행 과정으로 나누어 처리함
- 소스코드 평가 과정에서는 실행 컨텍스트를 생성하고 변수, 함수 등의 선언문만 먼저 실행하여 생성된 변수나 함수 식별자를 키로 실행 컨텍스트가 관리하는 스코프(렉시컬 환경의 환경 레코드)에 등록함
- 소스코드 평가 과정이 끝나면 비로소 선언문을 제외한 소스코드가 순차적으로 실행되기 시작함(런타임)

## 23.3 실행 컨텍스트의 역할<a name="23.3"></a>

**1. 전역 코드 평가**

- 소스코드 평가 과정에서는 선언문만 먼저 실행되어, 전역 코드의 변수 선언문과 함수 선언문이 먼저 실행 되고 생성된 전역 변수와 전역 함수가 실행 컨텍스트가 관리하는 전역 스코프에 등록됨

**2. 전역 코드 실행**

- 런타임이 시작되어 전역 코드가 순차적으로 실행됨. 전역 변수에 값이 할당되고 함수 호출되며, 함수 호출 시 순차적으로 실행되던 전역 코드의 실행을 일시 중단 하고 코드 실행 순서를 변경하여 함수 내부로 진입됨

**3. 함수 코드 평가**

- 함수가 호출되면 순차적으로 실행되던 전역 코드의 실행을 일시 중단 하고 코드 실행 순서를 변경하여 함수 내부로 진입됨

**4. 함수 코드 실행**

- 함수 코드 평가 과정이 끝난 후, 런타임이 시작되어 함수 코드가 순차적으로 실행됨
- 코드가 실행되려면 스코프, 식별자, 코드 실행 순서 등 관리 필요

- 실행 컨텍스트는 이 모든 것을 관리하며, 소스코드를 실행하는데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역임

- 실행 컨텍스트는 식별자(변수, 함수, 클래스 등의 이름)를 등록하고 관리하는 스코프 와 코드 실행 순서 관리를 구현한 내부 메커니즘으로, 모든 코드는 실행 컨텍스트를 통해 실행되고 관리됨

- 식별자와 스코프는 실행 컨텍스트의 렉시컬 환경으로 관리하고, 코드 실행 순서는 실행 컨텍스트 스택으로 관리함

## 23.4 실행 컨텍스트 스택<a name="23.4"></a>

- 실행 컨텍스트는 스택 자료구조로 관리하며, 이를 실행 컨텍스트 스택이라고 함
- 코드가 실행되는 시간 흐름에 따라 실행 컨텍스트 스택에는 실행 컨텍스트가 추가되고 제거됨

```js
const x = 1;

function foo() {
  const y = 2;

  function bar() {
    const z = 3;
    console.log(x + y + z);
  }
  bar();
}

foo(); // 6
```

**1. 전역 코드의 평가와 실행**

- 전역 코드를 평가하여 전역 실행 컨텍스트를 생성하고 실행 컨텍스트 스택에 푸시

**2. foo 함수 코드의 평가와 실행**

- 전역 함수 foo가 호출되면 전역 코드의 실행은 일시 중단되고 코드의 제어권이 foo 함수 내부로 이동함. foo 함수 내부의 함수 코드를 평가하여 foo 함수 실행 컨텍스트를 생성하고 실행 컨텍스트 스택에 푸시함

**3. bar 함수 코드의 평가와 실행**

- 중첩 함수 bar가 호출되면 foo 함수 코드의 실행은 일시 중단되고 코드 제어권이 bar 함수 내부로 이동함

**4. foo 함수 코드로 복귀**

- bar 함수가 종료되면 코드의 제어권은 다시 foo 함수로 이동함. bar 함수 실행 컨텍스트를 실행 컨텍스트 스택에서 팝하여 제거하고 실행할 코드가 없으므로 종료됨

**5. 전역 코드로 복귀**

- foo 함수가 종료되면 코드의 제어권은 다시 전역 코드로 이동함. foo 함수 실행 컨텍스트를 실행 컨텍스트 스택에서 팝하여 제거함. 실행할 전역 코드가 남아 있지 않으므로 전역 실행 컨텍스트도 실행 컨텍스트 스택에서 팝되어 실행 컨텍스트 스택에는 아무것도 남지 않음

- 실행 컨텍스트 스택은 코드의 실행 순서를 관리함

- 소스코드가 평가되면 실행 컨텍스트가 생성되고 실행 컨텍스트 스택의 최상위에 쌓임 실행 컨텍스트 스택의 최상위에 존재하는 실행 컨텍스트는 언제나 현재 실행 중인 코드의 실행 컨텍스트

- 실행 컨텍스트 스택 최상위에 존재하는 실행 컨텍스트를 실행 중인 실행 컨텍스트라 부름

## 23.5 렉시컬 환경<a name="23.5"></a>

- 렉시컬 환경은 식별자와 식별자에 바인딩된 값, 상위 스코프에 대한 참조를 기록하는 자료구조로 실행 컨텍스트를 구성하는 컴포넌트

- 실행 컨텍스트 스택이 코드의 실행 순서를 관리한다면 렉시컬 환경은 스코프와 식별자를 관리함

- 렉시컬 환경은 환경 레코드, 외부 렉시컬 환경에 대한 참조 2개의 컴포넌트로 구성됨
  - 환경 레코드: 스코프에 함된 식별자를 등록하고 등록된 식별자에 바인딩된 값을 관리하는 저장소
  - 외부 렉시컬 환경에 대한 참조: 외부 렉시컬 환경에 대한 참조는 상위 스코프를 가리킴

## 23.6 실행 컨텍스트의 생성과 식별자 검색 과정<a name="23.6"></a>

### 23.6.1 전역 객체 생성

- 전역 객체는 전역 코드가 평가되기 이전에 생성됨

- 전역 객체도 프로토타입 체인의 일원임

### 23.6.2 전역 코드 평가

- 소스코드가 로드되면 자바스크립트 엔진은 전역 코드를 평가함

- 전역 코드 평가 단계
  1.  전역 실행 컨텍스트 생성
      - 비어있는 전역 실행 컨텍스트 생성 후 실행 컨텍스트 스택에 푸시. 전역 실행 컨텍스트는 실행 중인 실행 컨텍스트, 실행 컨텍스트 스택의 최상위가 됨
  2.  전역 렉시컬 환경 생성
      - 전역 렉시컬 환경을 생성하고 전역 실행 컨텍스트에 바인딩함. 렉시컬 환경은 환경 레코드, 외부 렉시컬 환경에 대한 참조로 구성됨
        2.1. 전역 환경 레코드 생성 - 전역 환경 레코드는 전역 변수를 관리하는 전역 스코프, 전역 객체의 빌트인 전역 프로퍼티와 빌트인 전역 함수, 표준 빌트인 객체를 제공함 - 전역 환경 레코드는 객체 환경 레코드와 선언적 환경 레코드로 구성됨
        2.1.1. 객체 환경 레코드 생성 - 전역 코드 평가 과정에서 var 키워드로 선언한 전역 변수와 함수 선언문으로 정의된 전역 함수는 전역 환경 레코드의 객체 환경 레코드에 연결된 Bindingobject를 통해 전역 객체의 프로퍼티와 메서드됨
        2.1.2. 선언적 환경 레코드 생성 - 런타임에 실행 흐름이 변수 선언문에 도달하기 전까지 일시적 사각지대에 빠지게 됨
        2.2. this 바인딩 - 전역 환경 레코드의 [[GlobalThisValue]] 내부 슬롯에 this가 바인딩됨
        2.3. 외부 렉시컬 환경에 대한 참조 결정 - 현재 평가 중인 소스코드를 포함하는 외부 소 스코드의 렉시컬 환경(상위 스코프)을 가리킴

### 23.6.3 전역 코드 실행

- 전역 코드가 순차적으로 실행되며, 어느 스코프의 식별자를 참조하면 되는지 결정(식별자 결정)

- 렉시컬 환경에서 식별자를 검색할 수 없으면 외부 렉시컬 환경에 대한 참조가 가리키는 렉시컬 환경(상위 스코프)로 이동하여 식별자 검색

- 전역 렉시컬 환경은 스코프 체인의 종점으로, 전역 렉시컬 환경에서 검색할 수 없는 식별자는 참조 에러 발생함

### 23.6.4 foo 함수 코드 평가

- 전역 코드 평가를 통해 전역 실행 컨텍스트가 생성되었고 전역 코 실행함

- 함수 코드 평가 순서
  1.  함수 실행 컨텍스트 생성
      - 생성된 함수 실행 컨텍스트는 함수 렉시컬 환경이 완성된 다음 실행 컨텍스트에 푸시됨. foo 함수 실행 컨텍스트는 실행 컨텍스트 스택의 최상위(실행 중인 실행 컨텍스트)가 됨.
  2.  함수 렉시컬 환경 생성
      - foo 함수 렉시컬 환경을 생성하고 foo 함수 실행 컨텍스트에 바인딩함
        2.1. 함수 환경 레코드 생성 - 함수 환경 레코드는 매개변수, arguments 객체, 함수 내부에서 선언한 지역 변수와 중첩 함수를 등록하고 관리함
        2.2. this 바인딩 - 함수 환경 레코드의 [[ThisValue]] 내부 슬롯에 this 바인딩됨
        2.3. 외부 렉시컬 환경에 대한 참조 결정 - foo 함수 정의가 평가된 시점에 실행 중인 실행 컨텍스트이 렉시컬 환경의 참조 할당됨

### 23.6.5 foo 함수 코드 실행

- 런타임이 시작되어 소스코드가 순차적으로 실행됨. 식별자 결정을 위해 실행 중인 실행 컨텍스트이 렉시컬 환경에서 식별자 검색 시작

### 23.6.6 bar 함수 코드 평가

- 함수 실행 컨텍스트가 생성되고, 함수 코드 실행함.

### 23.6.7 bar 함수 코드 실행

- 런타임이 시작되어 함수 소스코드가 순차적으로 실행됨. 매개변수에 인수가 할당되고, 변수 할당문이 실행되어 지역 변수에 값이 할당됨.

1. console 식별자 검색
   - 객체 환경 레코드 BindingObject를 통해 전역 객체에서 찾을 수 있음
2. log 메서드 검색
   - console 식별자에 바인딩된 객체에서 log 메서드 검색함
3. 표현식 a + b + x + y + z의 평가
   - console.log 메서드에 전달할 인수 평가를 위해 식별자 검색함
4. console.log 메서드 호출

- 표현식이 평가되어 생성한 값을 console.log 메서드에 전달하여 호출함

### 23.6.8 bar 함수 코드 실행 종료

- console.log 메서드 호출 후 종료되면 실행할 코드가 없으므로 함수 코드 실행 종료됨

### 23.6.9 foo 함수 코드 실행 종료

- bar 함수 종료 후 실행할 코드가 없으면 foo 함수 코드 실행 종료됨

### 23.6.10 전역 코드 실행 종료

- foo 함수 종료 후 실행할 전역 코드가 없으면 전역 코드 실행 종료되며, 전역 실행 컨텍스트도 비게 됨

## 23.7 실행 컨텍스트와 블록 레벨 스코프<a name="23.7"></a>

- if 문의 코드 블록이 실행되면 if 문의 코드 블록을 위한 블록 레벨 스코프가 생성되어야 함

- 선언적 환경 레코드를 갖는 렉시컬 환경을 새롭게 생성하여 기존의 전역 렉시컬 환경을 교체함

- if 문의 코드 블록을 위한 렉시컬 환경의 외부 렉시컬 환경에 대한 참조는 if 문이 실행되기 이전의 전역 렉시컬 환경을 가리킴

# 24장 클로저

- `클로저`: 함수와 그 함수가 선언된 렉시컬 환경과의 조합 (MDN)

```jsx
const x = 1;

function outerFunc() {
  const x = 10;

  function innerFunc() {
    console.log(x); //10
  }

  innerFunc();
}
outerFunc();
```

- innerFunc 함수가 outerFunc 함수 내부에서 정의된 중첩 함수 이므로 outerFunc 의 변수 x에 접근할 수 있다.
- 이런 현상이 일어나는 이유는 자바스크립트가 렉시컬 스코프를 따르는 프로그래밍 언어이기 때문이다.

# 24.1 렉시컬 스코프

- 자바스크립트 엔진은 함수를 **어디서 호출했는지**가 아니라 함수를 **어디에 정의 했는지**에 따라 상위 스코프를 결정한다. 이를 `렉시컬 스코프(정적 스코프)`라 한다.
- 함수를 어디서 호출했는지는 함수의 상위 스코프 결정에 어떤 영향도 주지 못하며, **정의한 위치에서 정적으로 결정되고 변하지 않는다.**
- **렉시컬 스코프**
  - 스코프 = 실행 컨텍스트의 렉시컬 환경
  - 스코프 체인 = `‘외부 렉시컬 환경에 대한 참조’`를 통해 상위 렉시컬 환경과 연결하는 것
  - 함수의 상위 스코프를 결정 = 렉시컬 환경의 `‘외부 렉시컬 환경에 대한 참조’`에 저장할 참조값을 결정
  - 상위 스코프에 대한 참조 = 렉시컬 환경의 `‘외부 렉시컬 환경에 대한 참조’`에 저장할 참조 값
    ⇒ 즉, 상위 스코프에 대한 참조는 **함수 정의가 평가되는 시점**에 **함수가 정의된 환경**에 의해 결정된다. 이것이 **`렉시컬 스코프`**다.

# 24.2 함수 객체의 내부 슬롯 [[Environment]]

- 함수가 정의된 위치와 호출되는 위치는 다를 수 있다.
  따라서 **자신이 정의된 환경(=상위 스코프)**을 기억해야 한다.
  (\* 함수 정의가 위치하는 스코프가 바로 **상위 스코프**다.) - 함수 정의 평가 - 함수는 자신의 내부 슬롯 `[[Environment]]`에 **자신이 정의된 환경(=상위 스코프)의 참조**를 저장한다. 이때 상위 스코프 참조는 **현재 실행 중인 실행 컨텍스트의 렉시컬 환경**을 가리킨다. - 함수 정의가 평가되어 **함수 객체를 생성하는 시점**
  = **상위 함수가 실행**되고 있는 시점
  = **상위 함수의 실행 컨텍스트**가 실행 중 - 함수 호출 - 함수가 호출되었을 때 생성될 **함수 렉시컬 환경의 `‘외부 렉시컬 환경에 대한 참조’`**에 저장될 참조 값이 `상위 스코프`이다.
- 함수 객체는 내부 슬롯 `[[Environment]]`에 저장한 **렉시컬 환경의 참조(= 상위 스코프)**를 자신이 존재하는 한 기억한다.

### 함수 코드 평가 순서

1. 함수 실행 컨텍스트 생성
2. 함수 렉시컬 환경 생성
   1. 함수 환경 레코드 생성
   2. this 바인딩
   3. `외부 렉시컬 환경에 대한 참조` 결정

- `외부 렉시컬 환경에 대한 참조`에는 함수 객체의 내부 슬롯 [[Environment]]에 저장된 렉시컬 환경의 참조 (= **상위 스코프**)가 할당된다.
- 즉, 함수 객체의 내부 슬롯 `[[Environment]]`에 저장된 **렉시컬 환경의 참조**는 바로 함수의 **상위 스코프**를 의미한다.

# 24.3 클로저와 렉시컬 환경

외부 함수보다 **중첩 함수가 더 오래 유지**되는 경우 중첩 함수는 이미 생명 주기가 종료된 외부 함수의 변수를 참조할 수 있다. 이러한 중첩 함수를 `**클로저**`라고 한다.

- `클로저`: 함수와 그 함수가 선언된 렉시컬 환경과의 조합 (MDN)
  ⇒ 즉, ‘그 함수가 선언된 렉시컬 환경’ = 함수가 정의된 위치의 스코프 = 상위 스코프를 의미하는 실행 컨텍스트의 렉시컬 환경이다.
- 자바스크립트의 모든 함수는 자신의 상위 스코프를 기억한다. 상위 스코프는 함수를 어디서 호출하든 유지된다. 함수는 언제나 상위 스코프의 식별자를 참조할 수 있으며 식별자에 바인딩 된 값을 변경할 수 있다.
- ex)

  ```jsx
  const x = 1;

  function outer() {
    const x = 10;
    const inner = function () {
      console.log(x);
    };
    return inner;
  }

  const innerFunc = outer();
  innerFunc(); //10
  ```

  1. **outer 함수 평가**
     1. **outer 함수**가 평가되어 함수 객체를 생성할 때 `현재 실행 중인 컨텍스트의 렉시컬 환경`, 즉 **전역 렉시컬 환경**을 [[Environment]] 내부 슬롯에 **상위 스코프**로서 저장한다.
  2. **outer 함수 호출**
     1. outer 함수를 호출하면 outer 함수의 1️⃣**렉시컬 환경이 생성**되고 2️⃣**[[Environment]] 내부 슬롯에 저장된 전역 렉시컬 환경**을 outer 함수 렉시컬 환경의 `외부 렉시컬 환경에 대한 참조`에 할당한다.
  3. **inner 함수 평가**
     1. **중첩 함수 inner**는 함수 표현식으로 정의했기 때문에 런타임에 평가된다. inner는 자신의 [[Environment]] 내부 슬롯에 **현재 실행 중인 실행 컨텍스트의 렉시컬 환경**을 **상위 스코프**로서 저장한다.
     2. **inner 함수**는 [[Environment]] 내부 슬롯에 상위 스코프를 저장한다. 저장된 상위 스코프는 함수가 존재하는 한 유지된다.
  4. **outer 함수 실행 종료**
     1. outer 함수의 실행이 종료되면 inner 함수를 반환하면서 **outer 함수의 생명 주기가 종료된다.**
     2. outer 함수의 실행 컨텍스트가 실행 컨텍스트 스택에서 제거된다.
        하지만 **outer함수의 렉시컬 환경까지 소멸하는 것은 아니다.**
     3. outer 함수의 렉시컬 환경은 inner 함수의 [[Environment]] 내부 슬롯에서 참조되고 있고, inner 함수는 전역 변수 innerFunc에 의해 참조되고 있으므로 가바지 컬렉션의 대상이 되지 않는다.
  5. **inner 함수 호출**
     1. outer 함수가 반환한 inner 함수를 호출하면 **inner 함수의 실행 컨텍스트가 생성**되어 실행 컨텍스트 스택에 푸쉬된다.
     2. 렉시컬 환경의 **외부 렉시컬 환경에 대한 참조**에는 inner 함수 객체의 **[[Environment]] 내부 슬롯에 저장되어 있는 참조 값**이 할당된다.
     3. 중첩 함수 inner는 **외부 함수의 생존 여부와 상관없이** 자신의 정의된 위치에 의해 결정된 **상위 스코프를 기억**한다.
     4. inner 함수의 내부에서는 상위 스코프와 상위 스코프의 식별자를 참조할 수 있고, 식별자의 값을 변경할 수도 있다.

### 상위 스코프의 어떤 식별자도 참조하지 않는 함수는 클로저가 아니다

- 자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 모든 함수는 클로저다.
- 하지만 대부분의 모던 브라우저는 최적화를 통해 **상위 스코프의 어떤 식별자도 참조하지 않는 경우 상위 스코프를 기억하지 않는다.** 참조하지 않는 식별자를 기억하는 것은 메모리 낭비이기 때문이다.
- 클로저는 중첩 함수가 1️⃣**상위 스코프의 식별자를 참조**하고 있고 \***\*2️⃣**중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정\*\*하는 것이 일반적이다.
- `**자유 변수**`: 클로저에 의해 참조되는 상위 스코프의 변수
  - 클로저closure는 ‘함수가 자유 변수에 대해 닫혀있다’ 는 의미다.
- 클로저는 상위 스코프를 기억해야 하므로 불필요한 메모리 점유를 걱정할 수도 있겠지만, 모던 자바스크립트 엔진은 최적화가 잘 되어 있어서 클로저가 참조하지 않는 식별자는 기억하지 않는다.
- 클로저는 자바스크립트의 강력한 기능으로 필요하다면 적극적으로 활용해야 한다.

# 24.4 클로저의 활용

- 클로저는 **상태를 안전하게 변경하고 유지**하기 위해 사용한다.
- 즉, **상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용**하기 위해 사용한다.
- 외부 상태의 변경이나 가변 데이터를 피하고 **불변성을 지향**하는 **함수형 프로그래밍**에서 부수 효과를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이기 위해 **클로저는 적극적으로 사용된다.**
- ex) 호출 카운터

  - default

    ```jsx
    let num = 0;

    const increase = function () {
      return ++num;
    };

    console.log(increase()); // 1
    console.log(increase()); // 2
    console.log(increase()); // 3
    ```

    - 위 코드가 바르게 동작하기 위한 전제 조건
      1. num 변수 값이 increase 함수가 호출되기 전까지 변경되지 않고 유지되어야 함.
      2. 카운트 상태는 increase 함수만이 변경할 수 있어야 함.
    - 하지만 카운트 상태는 전역 변수를 통해 관리되고 있기 때문에 누구나 접근할 수 있고 변경할 수 있다. **(암묵적 결합)
      ⇒ increase 함수 만이 num 변수를 참조하고 변경할 수 있게 해야 한다.**

  - 전역 변수 num을 increase 함수의 지역 변수로

    ```jsx
    const increase = function () {
      let num = 0;
      return ++num;
    };

    console.log(increase()); // 1
    console.log(increase()); // 1
    console.log(increase()); // 1
    ```

    - 이제 num 변수의 상태는 increase 함수만이 변경할 수 있다.
    - 하지만 상태가 변경되기 이전 상태를 유지하지 못한다.

  - 클로저 사용

    ```jsx
    const increase = (function () {
      let num = 0;
      return function () {
        return ++num;
      };
    })();

    console.log(increase()); // 1
    console.log(increase()); // 2
    console.log(increase()); // 3
    ```

    - increase 변수에 할당된 함수는 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하는 클로저다.
    - 즉시 실행 함수는 호출된 이후 소멸되지만, 즉시 실행 함수가 반환한 클로저는 increase 변수에 할당되어 호출된다. 이때 클로저는 상위 스코프인 즉시 실행 함수의 렉시컬 환경을 기억하고 있다. 따라서 **자유 변수 num**을 **언제 어디서 호출하든지 참조하고 변경할 수 있다.**

  - 카운트 상태 감소 기능 추가해보기

    ```jsx
    const increase = (function () {
    	let num = 0;

    	**// 객체 리터럴의 중괄호는 코드 블록이 아니므로 별도의 스코프를 만들지 않는다.**
    	return {
    		// num: 0 // 프로퍼티는 public하므로 은닉되지 않는다.
    		increase() {
    			return ++num;
    		}
    		decrease() {
    			return num > 0 ? --num : 0;
    		}
    	};
    }());

    console.log(counter.increase()); // 1
    console.log(counter.increase()); // 2

    console.log(counter.decrease()); // 1
    console.log(counter.decrease()); // 0
    ```

  - 생성자 함수로 표현

    ```jsx
    const Counter = (function () {
      let num = 0;

      function Counter() {
        //this.num = 0; // 프로퍼티는 public하므로 은닉되지 않는다.
      }

      Counter.prototype.increase = function () {
        return ++num;
      };

      Counter.prototype.decrease = function () {
        return num > 0 ? --num : 0;
      };

      return Counter;
    })();

    const counter = new Counter();

    console.log(counter.increase()); // 1
    console.log(counter.increase()); // 2

    console.log(counter.decrease()); // 1
    console.log(counter.decrease()); // 0
    ```

    - num은 생성자 함수 Counter가 생성할 인스턴스의 프로퍼티가 아니라 즉시 실행 함수 내에서 선언된 변수다.
      num은 인스턴스를 통해 접근할 수 없으며, 즉시 실행 함수 외부에서도 접근할 수 있는 은닉된 변수다.

- 함수형 프로그래밍에서 클로저를 활용하는 예제

  ```jsx
  function makeCounter(aux) {
    // 카운트 상태를 유지하기 위한 자유 변수
    let counter = 0;

    // 클로저를 반환
    return function () {
      // 인수로 전달 받은 보조 함수에 상태 변경을 위임 함.
      counter = aux(counter);
      return counter;
    };
  }

  // 보조 함수
  function increase(n) {
    return ++n;
  }

  function decrease(n) {
    return --n;
  }

  const increaser = makeCounter(increase);
  console.log(increaser()); // 1
  console.log(increaser()); // 2

  const decreaser = makeCounter(decrease);
  console.log(increaser()); // -1
  console.log(increaser()); // -2
  ```

  - makeCounter 함수는 보조 함수를 인자로 전달받고 함수를 반환하는 고차 함수다.
  - makeCounter 함수가 반환하는 함수는 렉시컬 환경인 makerCounter 함수의 스코프에 속한 counter 변수를 기억하는 클로저다.
  - 주의할 점은 makeCounter 함수를 호출해 함수를 반환할 때, **반환 된 함수는 자신만의 독립된 렉시컬 환경을 갖는다**는 것이다.

    - makeCounter 함수를 호출하면 그때마다 새로운 makeCounter 함수 실행 컨텍스트의 렉시컬 환경이 생성된다.
    - increaser와 decreaser에 할당된 함수는 각각 자신만의 독립된 렉시컬 환경을 갖기 때문에 카운트를 유지하기 위한 자유 변수 counter를 공유하지 않아 **카운터의 증감이 연동되지 않는다.**
    - 증감이 연동된 카운터를 만들려면 렉시컬 환경을 공유하는 클로저를 만들어야 한다.

      ```jsx
      const counter = (function () {
        // 카운트 상태를 유지하기 위한 자유 변수
        let counter = 0;

        // 클로저를 반환
        return function (aux) {
          // 인수로 전달 받은 보조 함수에 상태 변경을 위임 함.
          counter = aux(counter);
          return counter;
        };
      })();

      // 보조 함수
      function increase(n) {
        return ++n;
      }

      function decrease(n) {
        return --n;
      }

      //보조 함수를 전달하여 호출
      console.log(counter(increase)); // 1
      console.log(counter(increase)); // 2

      //자유 변수 공유
      console.log(counter(decrease)); // 1
      console.log(counter(decrease)); // 0
      ```

# 24.5 캡슐화와 정보 은닉

- `캡슐화`: 객체의 상태를 나타내는 **프로퍼티**와 프로퍼티를 참조하고 조작할 수 있는 동작인 **메서드를 하나로 묶는 것**
- `정보 은닉`: 객체의 특정 프로퍼티와 메서드를 감추는 것
  - 정보 은닉은 적절치 못한 접근으로부터 객체의 상태가 변경되는 것을 방지해 정보를 보호하고, 객체 간의 상호 의존성, 즉 **결합도**를 낮추는 효과가 있다.
  - 대부분의 객체지향 프로그래밍 언어는 접근 제한자(public, private, protected)를 선언하여 공개 범위를 한정할 수 있다. 하지만 자바스크립트의 객체의 모든 프로퍼티와 메서드는 기본적으로 public 하다.
- 예제

  - default

    ```jsx
    function Person(name, age) {
      this.name = name; // public
      let _age = age; // private

      this.sayHi = function () {
        console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
      };
    }

    const me = new Person('Lee', 20);
    me.sayHi();
    console.log(me.name);
    console.log(me._age);

    const you = new Person('Kim', 30);
    you.sayHi();
    console.log(you.name);
    console.log(you._age);
    ```

    - name 프로퍼티는 자유롭게 참조하거나 변경할 수 있다.
    - \_age 변수는 Person 생성자 함수의 지역 변수이므로 참조하거나 변경할 수 없다.
    - 하지만 sayHi 메서드는 인스턴스 메서드이므로 Person 객체가 생성될 때마다 중복 생성된다. sayHi 메서드를 프로토타입 메서드로 변경하여 sayHi 메서드의 중복 생성을 방지해보자.

  - sayHi 메서드를 프로토타입 메서드로 변경

    ```jsx
    function Person(name, age) {
      this.name = name; // public
      let _age = age; // private
    }

    Person.prototype.sayHi = function () {
      console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
    };
    ```

    - 이때 Person.prototype.sayHi 메서드 내에서 Person 생성자 함수의 지역 변수 \_age를 참조할 수 없는 문제가 발생한다.

  - 즉시 실행 함수를 사용하여 Person 생성자 함수와 Person.prototype.sayHi 메서드를 하나의 함수로

    ```jsx
    const Person = (function () {
      let _age = 0; // private

      function Person(name, age) {
        this.name = name;
        _age = age;
      }

      Person.prototype.sayHi = function () {
        console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
      };

      return Person;
    })();

    const me = new Person('Lee', 20);
    me.sayHi();
    console.log(me.name);
    console.log(me._age);

    const you = new Person('Kim', 30);
    you.sayHi();
    console.log(you.name);
    console.log(you._age);
    ```

    - Person 생성자 함수와 Person 생성자 함수의 인스턴스가 상속받아 호출할 Person.prototype.sayHi 메서드는 즉시 실행 함수가 종료된 이후 호출된다.
    - Person 생성자 함수와 sayHi 메서드는 이미 종료되어 소멸한 즉시 실행 함수의 지역변수 \_age를 참조할 수 있는 **클로저**다.
    - 하지만 위 코드는 **Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 \_age 변수의 상태가 유지되지 않는다는 문제**가 있다.

    ```jsx
    const me = new Person('Lee', 20);
    me.sayHi();

    const you = new Person('Kim', 30);
    you.sayHi();

    me.sayHi();
    ```

    - 이는 Person.prototype.sayHi 메서드가 단 한 번 생성되는 클로저이기 때문에 발생하는 현상이다.
    - **이처럼 자바스크립트는 정보 은닉을 완전하게 지원하지 않는다.**
      - **(2021년 1월 기준)** 다행히 클래스에 private 필드를 정의할 수 있는 새로운 표준 사양이 제안되어 있다. 이 제안은 Chrom74 이상 브라우저와, Node.js 버전 12 이상에 이미 구현되어 있다.

# 24.6 자주 발생하는 실수

아래는 클로저를 사용할 때 자주 발생할 수 있는 실수를 보여주는 예제다.

- problem

  ```jsx
  var funcs = [];

  for (var i = 0; i < 3; i++) {
    funcs[i] = function () {
      return i;
    }; //1️⃣
  }

  for (var j = 0; j < funcs.length; j++) {
    console.log(funcs[j]()); //2️⃣
  }
  ```

  - 1️⃣에서 함수가 funcs 배열의 요소로 추가된다.
  - 2️⃣에서 funcs 배열의 요소로 추가된 함수를 순차적으로 호출한다.
    ⇒ 0, 1, 2 대신 3을 출력한다.
    for 문의 변수 선언문에서 var 키워드로 선언한 i 변수는 함수 레벨 스코프를 갖기 때문에 전역 변수다. 전역 변수 i에는 0, 1, 2가 순차적으로 할당된다.
    funcs 배열의 요소로 추가한 함수를 호출하면 전역 변수 i를 참고하여 i의 값 3이 출력된다.

- solve 1: **클로저** 사용

  ```jsx
  var funcs = [];

  for (var i = 0; i < 3; i++) {
    funcs[i] = (function (id) {
      return function () {
        return id;
      };
    })(i);
  }

  for (var j = 0; j < funcs.length; j++) {
    console.log(funcs[j]());
  }
  ```

  매개변수 id는 즉시 실행 함수가 반환한 중첩 함수에 묶여있는 자유 변수가 되어 그 값이 유지된다.

- solve 2: ES6의 let 키워드 사용
  위 예제는 자바스크립트의 함수 레벨 스코프 특성으로 인해 var 키워드르 선언한 전역 변수가 되기 때문에 발생하는 현상이다. let 키워드를 사용하면 깔끔하게 해결 된다.

  ```jsx
  const funcs = [];

  for (let i = 0; i < 3; i++) {
    funcs[i] = function () {
      return i;
    };
  }

  for (let j = 0; j < funcs.length; j++) {
    console.log(funcs[j]()); //0 1 2
  }
  ```

  - 함수의 상위 스코프는 for 문의 코드 블록이 반복 실행 될 때마다 식별자의 값을 유지해야 한다. 이를 위해 for 문이 반복될 때마다 **독립적인 렉시컬 환경을 생성**하여 식별자의 값을 유지한다.
  - 이처럼 let이나 const 키워드를 사용하는 반복문은 코드 블록을 반복 실행할 때마다 **새로운 렉시컬 환경을 생성**하여 반복할 당시의 상태를 마치 스냅숏을 찍는 것처럼 저장한다.
  - 단, 이는 반복문의 코드 블록 내부에서 함수를 정의할 때 의미가 있다.
    반복문의 코드 블록 내부에 함수 정의가 없는 반복문이 생성하는 새로운 렉시컬 환경은 반복 직후, 아무도 참조하지 않기 때문에 **가비지 컬렉션**의 대상이 된다.

- solve 3: 함수형 프로그래밍 기법인 **고차 함수**를 사용하는 방법

  ```jsx
  const funcs = Array.from(new Array(3), (_, i) => () => i); // (3) [f, f, f]

  funcs.forEach((f) => console.log(f())); // 0 1 2
  ```

# 25장 클래스

## 목차

- [25.1 클래스는 프로토타입의 문법적 설탕인가?](#25.1)
- [25.2 클래스 정의](#25.2)
- [25.3 클래스 호이스팅](#25.3)
- [25.4 인스턴스 생성](#25.4)
- [25.5 메서드](#25.5)
- [25.6 클래스의 인스턴스 생성 과정](#25.6)
- [25.7 프로퍼티](#25.7)
- [25.8 상속에 의한 클래스 확장](#25.8)

## 25.1 클래스는 프로토타입의 문법적 설탕인가?<a name="25.1"></a>

- 클래스가 필요 없는 프로토타입 기반 객체지향 언어

- ES5에서는 클래스 없이도 생성자 함수, 프로토타입을 통해 객체지향 언어의 상속 구현 가능

```js
// ES5 생성자 함수
var Person = (function () {
  // 생성자 함수
  function Person(name) {
    this.name = name;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log('Hi! My name is ' + this.name);
  };

  // 생성자 함수 반환
  return Person;
})();

// 인스턴스 생성
var me = new Person('Lee');
me.sayHi(); // Hi! My name is Lee
```

- 클래스는 함수이며, 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 문법적 설탕

- 클래스와 생성자 함수는 유사하나 차이점 존재

  1.  클래스를 new 연산자 없이 호출하면 에러 발생. 생성자 함수는 new 연산자 없이 호출하면 일반 함수로서 호출됨
  2.  클래스는 상속을 지원하는 extends, super 키워드 제공. 생성자 함수는 지원하지 않음
  3.  클래스는 호이스팅이 발생하지 않은 것처럼 동작함. 생성자 함수는 함수 호이스팅, 함수 표현식으로 정의한 생성자 함수는 변수 호이스팅이 발생함
  4.  클래스 내의 모든 코드는 암묵적으로 strict mode가 지정되어 실행되며 strict mode 해제할 수 없음. 생성자 함수는 암묵적으로 strict mode가 지정되지 않음
  5.  클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false이어서, 열거되지 않음

- 클래스는 문법적 설탕보다는 새로운 객체 생성 메커니즘으로 보는 것이 합당함

## 25.2 클래스 정의<a name="25.2"></a>

- 파스칼 케이스를 사용하는 것이 일반적

- 클래스는 일급 객체로서 아래의 특징을 가짐

  - 표현식으로 정의 가능
  - 무명의 리터럴로 생성 가능(런타임에 생성 가능)
  - 변수나 자료구조에 저장 가능
  - 함수의 매개변수에게 전달 가능
  - 함수의 반환값으로 사용 가능

- 클래스는 값처럼 사용할 수 있는 일급 객체로, 함수

- 클래스 몸체에는 0개 이상의 메서드만 정의 가능
  - 정의할 수 있는 메서드: constructor(생성자), 프로토타입 메서드, 정적 메서드

```js
// 클래스 선언문
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name; // name 프로퍼티는 public
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }

  // 정적 메서드
  static sayHello() {
    console.log('Hello!');
  }
}

// 인스턴스 생성
const me = new Person('Lee');

// 인스턴스의 프로퍼티 참조
console.log(me.name); // Lee
// 프로토타입 메서드 호출
me.sayHi(); // Hi! My name is Lee
// 정적 메서드 호출
Person.sayHello(); // Hello!
```

## 25.3 클래스 호이스팅<a name="25.3"></a>

- 함수로 평가되어, 런타임 이전에 평가되어 함수 객체 생성함

- 클래스가 평가되어 생성된 함수 객체는 생성자 함수로서 호출할 수 이쓴 constructor

- 생성자 함수로서 호출할 수 있는 함수는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성됨

  - 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재하기 때문

- 클래스 정의 이전에 참조 불가능

```js
console.log(Person); // Reference Error

class Person {}
```

- 클래스 선언문 또한 호이스팅이 발생함
  - let, const 키워드로 선언한 변수처럼 호이스팅됨
  - 클래스 선언문 이전에 TDZ에 빠져서 호이스팅이 발생하지 않는 것처럼 동작함

```js
const Person = '';

{
  // 호이스팅이 발생하지 않는다면 '' 출력됨
  console.log(Person); // Reference Error

  // 클래스 선언문
  class Person {}
}
```

- 모든 선언문은 런타임 이전에 먼저 실행되어서 var, let, const, function, function\*, class 키워드를 사용하여 선언되는 모든 식별자는 호이스팅됨

## 25.4 인스턴스 생성<a name="25.4"></a>

- 생성자 함수이며, new 연산자와 함께 호출되어 인스턴스 생성함

- 클래스는 인스턴스 생성이 유일한 존재 이유로, 반드시 new 연산자와 함께 호출

```js
class Person {}

const me = Person(); // TypeError
```

- 클래스 표현식으로 정의된 클래스는 클래스를 가리키는 식별자를 사용해 인스턴스를 생성하지 않고, 기명 클래스 표현식의 클래스 이름을 사용해 인스턴스를 생성하면 에러 발생함
  - 기명 함수 표현식과 마찬가지로 클래스 표현식에서 사용한 클래스 이름은 외부 코드에서 접근 불가능함

```js
const Person = class MyClass {};

const me = new Person();

console.log(MyClass); // Reference Error

const you = new MyClass(); // Reference Error
```

## 25.5 메서드<a name="25.5"></a>

- 클래스 몸체에는 0개 이상 메서드만 선언 가능

- 클래스 몸체에서 정의할 수 있는 메서드는 constructor, 프로토타입 메서드, 정적 메서드이다.

### 25.5.1 constructor

- 인스턴스를 생성하고 초기화하기 위한 특수 메서드

- 이름 변경 불가

- constructor 프로퍼티는 클래스 자신을 가리킴

  - 클래스가 인스턴스를 생성하는 생성자 함수라는 것을 의미
  - new 연산자와 함께 클래스 호출 시, 클래스는 인스턴스 생성

- constructor 내부에서 this에 추가한 프로퍼티는 인스턴스 프로퍼티가 됨

- 클래스 내에서 최대 한 개만 존재 가능

- 생략 가능하며, 생략 시 빈 constructor가 암묵적 생성됨

  - constructor를 생략한 클래스는 빈 constructor에 의해 빈 객체를 생성함

- 프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 constructor 내부에서 this에 인스턴스 프로퍼티 추가

- 클래스 외부에서 인스턴스 프로퍼티의 초기값을 전달하려면 constructor에 매개변수를 선언하고 인스턴스를 생성할 때 초기값을 전달해야 함

```js
class Person {
  constructor(name, address) {
    // 인수로 인스턴스 초기화
    this.name = name;
    this.address = address;
  }
}

// 인수로 초기값 전달 // 초기값은 constructor에 전달됨
const me = new Person('Lee', 'Seoul');
console.log(me); // Person {name: "Lee", address: "Seoul"}
```

- 인스턴스를 초기화하려면 constructor를 생략하면 안됨

- constructor은 별도의 반환문을 갖지 않음

- 명시적으로 this가 아닌 다른 값을 반환하는 것은 클래스의 기본 동작을 훼손하기 때문에 반드시 return 문을 생략한다

### 25.5.2 프로토타입 메서드

- 생성자 함수를 사용하여 인스턴스를 생성하는 경우, 프로토타입 메서드를 생성하기 위해서 명시적으로 프로토타입에 메서드를 추가함

- 클래스 몸체에 정의한 메서드는 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 됨

- 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 됨

### 25.5.3 정적 메서드

- 인스턴스를 생성하지 않아도 호출할 수 있는 메서드

- 클래스에서 메서드에 static 키워드를 붙이면 정적 메서드가 됨

- 정적 메서드는 클래스에 바인딩된 메서드가 됨

- 클래스는 함수 객체로 평가되어, 자신의 프로퍼티/메서드 소유 가능

- 정적 메서드는 클래스 정의 이후 인스턴스를 생성하지 않아도 호출 가능

- 정적 메서드가 바인딩된 클래스는 인스턴스의 프로토타입 체인상에 존재하지 않아서, 인스턴스로 호출 불가능

### 25.5.4 정적 메서드와 프로토타입 메서드의 차이

1. 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다름
2. 정적 메서드는 클래스로 호출하고, 프로토타입 메서드는 인스턴스로 호출함
3. 정적 메서드는 인스턴스 프로퍼티 참조할 수 없고, 프로토타입 메서드는 인스턴스 프로퍼티 참조 가능

### 25.5.5 클래스에서 정의한 메서드의 특징

1. function 키워드를 생략한 메서드 축약 표현 사용
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없음
3. 암묵적으로 strict mode로 실행됨
4. for ... in 문이나 Object.keys 메서드 등으로 열거 불가
5. 내부 메서드 [[Constructor]]를 갖지 않는 non-constructor이므로 new 연산자와 함께 호출 불가

## 25.6 클래스의 인스턴스 생성 과정<a name="25.6"></a>

1. 인스턴스 생성과 this 바인딩
   - new 연산자와 함께 호출하면 빈 객체 생성
   - 빈 객체가 클래스가 생성한 인스턴스
2. 인스턴스 초기화
   - this에 바인딩되어 있는 인스턴스 초기화
3. 인스턴스 반환
   - 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적 반환됨

## 25.7 프로퍼티<a name="25.7"></a>

### 25.7.1 인스턴스 프로퍼티

- constructor 내부에서 정의

- 인스턴스에 프로퍼티가 추가되어 인스턴스가 초기화됨

- 접근 제한자를 지원하지 않아, 인스턴트 프로퍼티는 언제나 public함

### 25.7.2 접근자 프로퍼티

- 자체적으로 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티(getter 함수, setter 함수로 구성)

### 25.7.3 클래스 필드 정의 제안

- 클래스가 생성할 인스턴스의 프로퍼티를 가리킴

- 클래스 필드에 함수 할당은 권장하지 않음

### 25.7.4 private 필드 정의 제안

```js
class Person {
  // private 필드 정의
  #name = ' ';

  constructor(name) {
    // private 필드 참조
    this.#name = name;
  }
}

const me = new Person('Lee');

console.log(me.#name); // SyntaxError
```

| 접근 가능성                 | public | private |
| --------------------------- | ------ | ------- |
| 클래스 내부                 | O      | O       |
| 자식 클래스 내부            | O      | X       |
| 클래스 인스턴스를 통한 접근 | O      | X       |

- 클래스 외부에서 직접 private 접근 방법은 없으며, 접근자 프로퍼티를 통해 간접적 접근은 가능

### 25.7.5 static 필드 정의 제안

- static 키워드를 사용해 정적 메서드 정의 가능

## 25.8 상속에 의한 클래스 확장<a name="25.8"></a>

### 25.8.1 클래스 상속과 생성자 함수 상속

- 상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것

### 25.8.2 extends 키워드

- 상속을 통해 클래스를 확장하려면 extends 키워드를 사용하여 상속받은 클래스를 정의

- 상속을 통해 확장한 클래스를 **서브클래스**라고 하고, 서브 클래스에 상속된 클래스를 **수퍼클래스**라 부름
  - 서비클래스를 파생 클래스 또는 자식 클래스라고도 부르며, 수퍼 클래스는 베이스 클래스, 부모 클래스라고도 부름

### 25.8.3 동적 상속

- extends 키워드는 생성자 함수를 상속받아 클래스 확장 가능
  - 단 extends 키워드 앞에 반드시 클래스가 있어야 함

### 25.8.4 서브클래스의 constructor

- 서브킄ㄹ래스에서 constructor를 생략하면 암묵적으로 constructor 정의됨

- args는 new 연산자와 함께 클래스 호출 시 전달한 인수의 리스트

```js
constructor(...args) { super(...args); }
```

- Rest 파라미터
  - 매개변수에 ...을 붙이면 Rest 파라미터가 됨. Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받음

### 25.8.5 super 키워드

- super 호출하면 수퍼클래스의 constructor 호출함
- super 참조하면 수퍼클래스의 메서드 호출 가능

**super 호출**

- super 호출 시 수퍼클래스의 constructor 호출함

**super 호출 시 주의 사항** 01. 서브클래스의 constructor를 생략하지 않는 경우 서브클래스의 constructor에서는 반드시 super 호출해야 함 02. 서브클래스의 constructor에서 super 호출하기 전에는 this 참조 불가 03. super은 반드시 서브클래스의 constructor에서만 호출함. 다른 곳에서 호출 시 에러 발생

**super 참조**

- 메서드 내에서 super 참조하면 수퍼클래스의 메서드 호출 가능

1.  서브클래스의 프로토타입 메서드 내에서 super.sayHi는 수퍼클래스의 프로토타입 메서드 sayHi를 가리킴
2.  서브클래스의 정적 메서드 내에서 super.sayHi는 수퍼클래스의 정적메서드 sayHi를 가리킴

### 25.8.6 상속 클래스의 인스턴스 생성 과정

- 상속 관계에 있는 두 클래스가 인스턴스를 생성하는 과정이 클래스가 단독으로 인스턴스를 생성하는 것보다 복잡함

1. 서브클래스의 super 호출

- 수퍼클래스와 서브클래스는 new 연산자와 함께 호출되었을 때 동작이 구분됨
- 서브클래스는 직접 인스턴스를 생성하지 않고, 수퍼클래스에 인스턴스 생성을 위임함. 서브 클래스의 constructor에서 반드시 super를 호출해야하는 이유이다.

2. 수퍼클래스의 인스턴스 생성과 this 바인딩

- 수퍼클래스의 constructor 내부 코드가 실행되기 전에 빈 객체를 생성함. 빈 객체가 바로 클래스가 생성한 인스턴스이며, 인스턴스는 this에 바인딩된다.
- 수퍼클래스의 constructor 내부의 this는 생성된 인스턴스를 가리킴
- 인스턴스는 new.target이 가리키는 서브클래스가 생성한 것으로 처리됨

3. 수퍼클래스의 인스턴스 초기화

- this에 바인딩되어 있는 인스턴스를 초기화함
- this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고, constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티를 초기화함

4. 서브클래스의 constructor로의 복귀와 this 바인딩

- super 호출이 종료되고, 제어 흐름이 서브클래스 constructor로 돌아옴. 이때 super이 반환한 인스턴스가 this에 바인딩됨. 서브클래스는 별도 인스턴스를 생성하지 않고 suepr가 반환한 인스턴스를 this에 바인딩하여 그대로 사용

5. 서브클래스의 인스턴스 초기화

- super 호출 이후 서브클래스의 constructor에 기술되어 있는 인스턴스 초기화가 실행됨
- this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티를 초기화함

6. 인스턴스 반환

- 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환됨

### 25.8.7 표준 빌트인 생성자 함수 확장

- extends 키워드 다음에는 클래스뿐만 아니라 [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식 사용 가능

- Array 생성자 함수를 상속받아