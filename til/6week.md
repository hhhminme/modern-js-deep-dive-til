# 15장 let, const 키워드와 블록 레벨 스코프

## var 키워드로 선언한 변수의 문제점

- 변수 중복 선언 허용
  - 의도치 않게 먼저 선언된 변수 값이 변경되는 부작용이 있다.
- 함수 레벨 스코프
  - 전역 변수를 남발할 가능성을 높인다. 이로 인해 의도치 않게 전역 변수가 중복 선언되는 경우가 발생한다.
- 변수 호이스팅
  - 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다. 문제는 에러를 발생시키진 않는데 프로그램 흐름에 안맞고 가독성을 흐트려트린다는 단점이 존재한다.

## let 키워드

- 변수 중복 선언 금지
  - 변수 중복 선언하면 문법 에러 발생
- 블록 레벨 스코프
  - 코드 블록만을 지역 스코프로 인정.
- 변수 호이스팅
  - 변수 선언문 이전에 참조하면 참조 에러가 발생. 단 let 키워드로 선언한 변수는 선언 단계와 초기화 단계가 분리되어 진행. 초기화 단계 실행 이전에 변수에 접근하면 참조 에러가 발생. 이때 스코프 시작 지점부터 초기화 단계 시작 지점까지 변수를 참조할 수 없는데 이를 temporal dead zone 이라고 부른다.
- 전역 객체와 let
  - let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.

## const 키워드

- const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야한다.
- 재할당 금지
- 상수
  - const 키워드로 선언된 변수에 원시 값을 할당한 경우 원시 값은 변경할 수 없는 값이므로 재할당이 불가능하다.
- const 키워드와 객체
  - const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다. const 키워드는 재할당을 금지할 뿐 “불변”을 의미하지는 않는다.

그렇다면 우리는 무엇을 써야하는가?

- es6를 사용한다면 var 키워드는 사용하지 않는다.
- 재할당이 필요한 경우에 한정해 let 키워드를 사용하고, 이때 변수의 스코프를 최대한 좁게 만든다.
- 변경이 발생하지 않고 읽기 전용으로 사용하는 원시 값과 객체는 const 를 사용하다.
- 결론 : const 쓰자.

# 16장 프로퍼티 어트리뷰트

## 내부 슬롯과 내부 메서드

자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드이다. 외부에서는 접근 불가.

## 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티 상태를 나타내는 프로퍼티 어트리뷰트(값, 갱신 가능 여부, 열거 가능 여부)를 기본값으로 자동 정의한다. 그리고 이 정보를 담은 객체를 프로퍼티 디스크립터 객체라고 부른다.

## 데이터 프로퍼티와 접근자 프로퍼티

데이터 프로퍼티는 프로퍼티 디스크립터 객체에 정의된 값들이다. 접근자 프로퍼티는 다른 데이터 프로퍼티 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티이다. 예를 들면 get 등이 있다.

## 객체의 변경 방지

처음알았는데 preventExtensions, seal, freeze 등의 함수를 통해서 객체의 확장, 밀봉, 동결 시킬 수 있다. 그런데 이런건 얕은 방지만 가능해서 전체 동결시키고 싶다면 Object.freeze 메서드를 호출해야한다.

# 17장 생성자 함수에 의한 객체 생성

## Object 생성자 함수

new Object() 연산자를 통해서 객체 인스턴스를 생성할 수 있다. 그런데 객체 리터럴로 생성하는데 더 편해서 별로 유용하지 않다.

## 생성자 함수

### 객체 리터럴에 의한 객체 생성 방식의 문제점

프로퍼티 구조가 동일하다면 매번 비슷한 코드를 작성해야해서 불편하다.

### 생성자 함수에 의한 객체 생성 방식의 장점

프로퍼티 구조가 동일한 객체 여러개를 간편하게 생성할 수 있다는 장점이 있다.

### 생성자 함수의 인스턴스 생성 과정

암묵적으로 인스턴스를 생성하고 생성된 인스턴스를 초기화한 후 암묵적으로 인스턴스를 반환함.

1. 인스턴스 생성과 this 바인딩
   1. 암묵적으로 생성된 빈 객체인 인스턴스는 this 에 바인딩 됨
2. 인스턴스 초기화

   this 에 프로퍼티를 추가하거나 할당하는 식으로 개발자가 처리함

3. 인스턴스 반환

   생성자 함수 내부의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환됨

# 18장 함수와 일급객체

## 일급 객체

1. 무명의 리터럴로 생성할 수 있다. (런타임에 생성가능하다)
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수에 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.

⇒ 자바스크립트의 함수는 위의 조건을 모두 만족하므로 일급 객체이다.

⇒ 그럼 객체와 함수의 차이가 뭘까?

일반 객체는 호출할 수 없지만 함수 객체는 호출할 수 있다. 그리고 함수 객체는 일반 객체에는 없는 고유 프로퍼티를 소유한다.

## 함수의 객체 프로퍼티

- arguments 프로퍼티
  - 현재 일부 브라우저에서 지원하고 있지만 es3부터 표준에서 폐지됨. 이 때 모든 인수는 암묵적으로 arguments 객체의 프로퍼티로 보관된다.
- caller 프로퍼티
  - 비표준 프로퍼티라서 넘기겠음
- length 프로퍼티
  - 선언한 매개변수의 갯수
- name 프로퍼티
  - es6부터 정식 표준이 됨. 근데 그 이전에서는 달리 동작해서 쓰지 않겠음.
- **proto** 접근자 프로퍼티
  - 모든 객체는 prototype 이라는 내부 슬롯을 가짐. `__proto__` 프로퍼티는 prototype 내부 슬롯이 가리키는 프로퍼타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티임.
- prototype 프로퍼티
  - prototype 프로퍼티는 생성자 함수로 호출할 수 있는 함수 객체. 생성할 인스턴스의 프로포타입 객체를 가리킴.