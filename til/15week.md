# 47장 에러처리

# 47.1 에러 처리의 필요성

- 에러가 발생하지 않는 코드를 작성하는 것은 불가능하다. 에러는 언제나 발생할 수 있다. 하지만 에러에 대처하지 않고 방치하면 프로그램은 강제 종료된다.
- try…catch 문을 사용해 에러에 적절하게 대응하면 프로그램이 강제 종료되지 않고 계속해서 코드를 실행시킬 수 있다.

```jsx
try {
  foo();
} catch (error) {
  console.error('[에러 발생]', error);
}
```

- 직접적으로 에러를 발생시키지 않는 예외적인 상황이 발생할 수도 있다. 예외적인 상황에 적절하게 대응하지 않으면 에러로 이어질 수 있다.

```jsx
//button 요소가 존재하지 않으면 null을 반환한다.
const $button = document.querySelector('button'); // null

$button.classLIst.add('disabled'); //TypeError: Cannot read property 'classList' of null
```

- querySelector 메서드는 인수로 전달한 CSS 선택자 문자열로 DOM에서 요소 노드를 찾을 수 없는 경우 에러를 발생시키지 않고 null을 반환한다.
  이때 if문 또는 단축 평가, 옵셔널 체이닝 연산자 ?.를 사용하지 않으면 에러로 이어질 가능성이 있다.

```jsx
//button 요소가 존재하지 않으면 null을 반환한다.
const $button = dcument.querySelector('button'); // null
$button?.classList.add('disabled');
```

- 에러나 예외적인 상황은 다양하기 때문에 아무런 조치없이 프로그램이 강제 종료된다면 원인을 파악하여 대응하기 어렵다.
- 언제나 에러나 예외적인 상황이 발생할 수 있다는 것을 전제하고 대응하는 코드를 작성하는 것이 중요하다.

# 47.2 try…catch…finally 문

- 일반적으로 에러 처리를 구현하는 방법은 크게 두 가지가 있다.
  - 예외적인 상황이 발생하면 반환하는 값(null 또는 -1)을 if문이나 단축 평가, 옵셔널 체이닝 연산자를 통해 확인해서 처리하는 방법
  - 에러 처리 코드를 미리 등록해두고 에러가 발생하면 에러 처리 코드로 점프하도록 하는 방법
    - try…catch…finally 문
- 두번째 방법을 `에러 처리`라고 한다.
- try…catch…finally 문은 3개의 코드 블록으로 구성된다.
  - finally는 생략 가능하다.
  - catch문도 생략 가능하지만 catch문이 없는 try문은 의미 없으므로 생략하지 않는다.
  ```jsx
  try {
    //에러가 발생할 가능성이 있는 코드
  } catch (err) {
    // 에러 발생 시 실행. try 코드 블록에서 발생한 에러 객체 실행.
  } finally {
    //에러 발생과 상관 없이 반드시 한 번 실행
  }
  ```
  - err 변수는 try에서 에러가 발생하면 생성되고, catch코드 블록에서만 유효하다.
  - try…catch…finally를 사용하면 프로그램이 강제로 종료되지 않는다.

# 47.3 Error 객체

- Error 생성자 함수는 에러 객체를 생성한다. 에러를 상세히 설명하는 에러 메시지를 생성자 함수에 인수로 전달할 수 있다.
  ```jsx
  const error = new Error('invalid');
  ```
- 에러 객체는 message 프로퍼티와 stack 프로퍼티를 갖는다.
  - `message`: 인수로 전달한 에러 메시지
  - `stack`: 에러를 발생시킨 콜스택의 호출 정보를 나타내는 문자열. 디버깅 시 사용
- 자바스크립트는 Error 생성자 함수를 포함해 7개의 에러 객체를 생성할 수 있는 Error 생성자 함수를 제공한다. 이 에러 객체의 프로토타입은 모두 Error.prototype을 상속받는다.
  | 생성자 함수 | 인스턴스 |
  | -------------- | ------------------------------------------------------------- |
  | Error | 일반적 에러 객체 |
  | SyntaxError | 자바스크립트 문법에 맞지 않는 문을 해석할 때 발생. |
  | ReferenceError | 참조할 수 없는 식별자를 참조했을 때 발생. |
  | TypeError | 피연산자 또는 인수의 데이터 타입이 유효하지 않을 때 발생. |
  | RangeError | 숫자 값의 허용 범위를 벗어났을 때 발생. |
  | URIError | encodeURI, decodeURI 함수에 부적절한 인수를 전달했을 때 발생. |
  | EvalError | eval 함수에서 발생. |
  ```jsx
  1 @ 1; //SyntaxError
  foo(); // ReferenceError: foo is not defined
  null.foo; //TypeError: Cannot read property 'foo' of null
  new Array(-1) // RangeError: Invalid array length
  decodeURIComponent('%'); //URIError: URI malformed
  ```

# 47.4 throw문

- Error 생성자 함수로 에러 객체를 생성한다고 에러가 발생하는 것은 아니다.
- 에러를 발생시키려면 try 코드 블록에서 throw 문으로 에러 객체를 던져야 한다.
  ```jsx
  throw 표현식;
  ```
- throw 문의 표현식은 어떤 값이라도 상관 없지만, 일반적으로 에러 객체를 지정한다. 에러를 던지면 catch문의 에러 변수가 생성되고 던져진 에러 객체가 할당된다. 그리고 catch코드 블록이 실행되기 시작한다.
  ```jsx
  try {
    throw new Error('something wrong');
  } catch (error) {
    console.log(error);
  }
  ```
- 외부에서 전달받은 콜백 함수를 n번만큼 반복 호출하는 repeat함수 구현 예시

  ```jsx
  const repeat = (n, f) => {
    if (typeof f !== 'function') throw new TypeError('f must be a function');

    for (var i = 0; i < n; i++) {
      f(i);
    }
  };

  try {
    repeat(2, 1);
  } catch (err) {
    console.error(err); //TypeError: f must be a function
  }
  ```

# 47.5 에러의 전파

- 에러는 호출자 방향으로 전파된다. 즉, 콜 스택의 아래 방향으로 전파된다.

  ```jsx
  const foo = () => {
    throw Error('foo에서 발생한 에러');
  };

  const bar = () => {
    foo();
  };

  const baz = () => {
    bar();
  };

  try {
    baz();
  } catch (err) {
    console.error(err);
  }
  ```

  - 여기서 foo 함수가 throw한 에러는 호출자에게 전파되어 전역에서 캐치된다.
    (foo 실행 컨텍스트 → bar 실행 컨텍스트 → baz 실행 컨텍스트 → 전역 실행 컨텍스트 )
  - throw된 에러를 캐치하지 않으면 호출자 방향으로 전파된다.
  - 에러를 캐치하여 적절히 대응하면 코드의 실행 흐름을 복구 할 수 있으나, 어디에서도 캐치하지 않으면 프로그램은 강제 종료된다.
  - ⭐ **주의할 점은 비동기 함수인 setTimeout이나 프로미스 후속 처리 메서드의 콜백 함수는 호출자가 없다는 것이다.**
    이들은 태스크 큐나 마이크로태스크 큐에 일시 저장되었다가 콜 스택이 빌 때 콜 스택으로 푸시되어 실행되는데, 스택의 가장 하부에 존재하게 된다. 따라서 에러를 전파할 호출자가 존재하지 않는다.

# 48장 모듈

## 목차

- [48.1 모듈의 일반적 의미](#48.1)
- [48.2 자바스크립트와 모듈](#48.2)
- [48.3 ES6 모듈(ESM)](#48.3)

## 48.1 모듈의 일반적 의미<a name="48.1"></a>

- 모듈이란?
  - 애플리케이션을 구성하는 개별적 요소로서 재사용 가능한 코드 조각
  - 기능을 기준으로 파일 단위로 분리함
  - 자신만의 파일 스코프(모듈 스코프)를 가질 수 있어야 함(개별적 존재로서 애플리케이션과 분리되어 존재)
  - 공개가 필요한 자산에 한정하여 명시적으로 선택적 공개 가능
  - 모듈 사용자는 모듈이 공개한 자산 중 일부 또는 전체를 선택해 자신의 스코프 내로 불러들여 재사용 가능

## 48.2 자바스크립트와 모듈<a name="48.2"></a>

- 브라우저 환경에서 모듈을 사용하기 위해서는 CommonJS or AMD를 구현한 모듈 로더 라이브러리를 사용해야 함

- CommonJS : Node.js
  - Node.js는 ECMAScript 표준 사양이 아니지만, 모듈 시스템 지원함
  - Node.js 환경에서는 파일별로 독립적인 파일 스코프(모듈 스코프)를 가짐

## 48.3 ES6 모듈(ESM)<a name="48.3"></a>

- ES6에서 클라이언트 사이드 자바스크립트에서도 동작하는 모듈이 추가됨

- ESM임(ES6 모듈)을 명확하게 하기 위해 ESM 파일 확장자는 mjs 사용 권장
  - strict mode 적용됨

```html
<script type="module" src="app.mjs"></script>
```

### 48.3.1 모듈스코프

- ESM은 독자적인 모듈 스코프를 가짐

  - 모듈 내에서 var 키워드로 선언한 변수는 전역 변수가 아니며, window 객체의 프로퍼티도 아님

- ESM이 아닌 일반적인 자바스크립트 파일은 script 태그로 분리해 서 로드해도 독자적인 모듈 스코프를 갖지 않음

```js
// foo.js
// X 변수는 전역 변수
var x = 'foo';
console.log(window.x); // foo
```

```js
// bar.js
// x 변수는 전역 변수
// foo.js에서 선언한 전역 변수 x와 중복된 선언
var x = 'bar';

// foo.js에서 선언한 전역 변수 x의 값이 재할당됨
console.log(window.x); // bar
```

### 48.3.2 export 키워드

- 모듈 내부에서 선언한 모든 식별자는 기본적으로 해당 모듈 내부에서만 참조 가능

- 모듈 내부에서 선언한 식별자를 외부에 공개하여 다른 모듈들이 재사용할 수 있게 하려면 export 키워드 사용

```js
// lib.mjs
// 변수의공개
export const pi = Math.PI;

// 함수의 공개
export function square(x) {
  return x * x;
}
// 클래스의 공개
export class Person {
  constructor(name) {
    this.name = name;
  }
}
```

```js
// lib.mjs
const pi = Math.PI;

function square(x) {
  return x * x;
}

class Person {
  constructor(name) {
    this.name = name;
  }
}

// 변수, 함수 클래스를 하나의 객체로 구성하여 공개
export { pi, square, Person };
```

### 48.3.3 import 키워드

- 다른 모듈에서 공개한 식별자를 자신의 모듈 스코프 내부로 로드하려면 import 키워드 필요

- 다른 모듈이 export한 식별자 이름으로 import해야 하며 ESM의 경우 파일 확장자를 생략 불가능

```js
// app.mjs
// 같은 폴더 내의 lib.mjs 모듈이 export한 식별자 이름으로 import한다.
// ESM의 경우 파일 확장자 생략 불가능
import { pi, square, Person } from ' ./lib.mjs';
console.log(pi); // 3.141592653589793 console.log(square(10)); // 100
console.log(new Person('Lee')); // Person { name: 'Lee' }
```

```js
// app.mjs
// lib.mjs 모듈이 export한 식별자 이름을 변경하여 import함
import { pi as PI, square as sq, Person as P } from './lib.mjs';
console.log(PI); // 3.141592653589793
console.1og(sq(2)); // 4
console.log(new P('Kim')); // Person { name: 'Kim' }
```

```js
// app.mjs
// lib.mjs 모듈이 export한 식별자 이름을 변경하여 import함
import { pi as PI, square as sq, Person as P } from './lib.mjs';
console.log(PI); // 3.141592653589793
console.log(sq(2)); // 4
console.log(new P('Kim')); // Person { name: 'Kim' }
```

# 49장 Babel과 Webpack을 이용한 ES6+/ES.NEXT 개발 환경 구축

## 목차

- [49.1 Babel](#49.1)

## 49.1 Babel<a name="49.1"></a>

```js
// ES6, ES7
[1, 2, 3].map((n) => n ** n);
```

```js
// Babel
'use strict';

[1, 2, 3].map(function (n) {
  return Math.pow(n, n);
});
```

- Babel은 최신 사양의 소스코드를 구형 브라우저에서도 동작할 수 있도록 ES5 사양의 소스코드로 변환(트랜스파일링)해줌

### 49.1.1 Babel 설치

```bash
$ mkdir esnext-project && cd esnext-project

$ npm init -y

$ npm i --save-dev @babel/core @babel/cli
```

### 49.1.2 Babel 프리셋 설치와 babel.config.json 설정 파일 작성

- Babel을 사용하라면 `@babel/preset-env` 설치 필요
  - 함께 사용되어야하는 Babel 플러그인을 모아둔 것으로 Babel 프리셋이라고도함
  - Browserslist 형식으로 `.browserslistrc` 파일에 프로젝트 지원 환경 설정 가능. 생략 가능

```bash
$ npm i --save-dev @babel/preset-env
```

```json
// babel.config.json
{
  "presets": ["@babel/preset-env"]
}
```

### 49.1.3 트랜스파일링

- Babel CLI 명령어 사용 가능하나, 트랜스파일링할 때마다 번거로우므로 `npm scripts`에 Babel CLI 명령어 등록해서 사용 가능

```json
// package.json
{
  ...
  "scripts": {
    "build": "babel src/js -w -d dist/js"
  }
}
```

- 위 스크립트의 `build`는 `src/js` 폴더에 있는 모든 자바스크립트 파일들을 트랜스파일링한 후, 결과물을 `dis/js` 폴더에 저장함
  - `-w` : 타킷 폴더에 있는 모든 자바스크립트 파링들의 변경을 감지하여 자동으로 트랜스파일(`--watch`)
  - `-d` : 트랜스파일링된 결과물이 저장될 폴더를 지정. 지정된 폴더가 존재하지 않으면 자동 생성(`--out-dir`)

```js
// src/js/lib.js
export const pi = Math.PI; // ES6

export function power(x, y) {
  return x ** y; // ES7
}

// ES6
export class Foo {
  #private = 10; // 클래스 필드

  foo() {
    // Rest/Spread 메서드
    const { a, b, ...x } = { ...{ a: 1, b: 2 }, c: 3, d: 4 };
    return { a, b, x };
  }

  bar() {
    return this.#private;
  }
}

// src/js/main.js
import { pi, power, Foo } from './lib';

console.log(pi);
console.log(power(pi, pi));

const f = new Foo();
console.log(f.foo());
console.log(f.bar());
```

```bash
$ npm run build
```

### 49.1.4 Babel 플러그인 설치

- 설치가 필요한 Babel 플러그인은 [Babel 홈페이지](https://babeljs.io/)에서 제안 사양 이름 입력 시 플러그인 검색 가능

```bash
$ npm i --save-dev @babel/plugin-proposal-class-properties
```

```json
{
  "presets": ["@babel/preset-env"],
  "plugins": ["@babel/plugin-proposal-class-properties"]
}
```

```bash
$ npm run build
```

```bash
# 트랜스파일링된 main.js 실행
$ node dist/js/main
```

### 49.1.5 브라우저에서 모듈 로딩 테스트

- `main.js`와 `lib.js` 모듈을 트랜스파일링하여 ES5 사양으로 변환된 `main.js` 실행 시 `Node.js` 환경에서 동작한 것이며, Babel이 모듈을 트랜스파일링한 것도 `Node.js`가 기본 지원하는 `CommonJS` 방식의 모듈 로딩 시스템에 따른 것

- 브라우저는 `CommonJS` 방식의 `require` 함수를 지원하지 않으므로 위에서 트랜스파일링된 결과를 그대로 브라우저에서 실행되면 에러가 발생됨
  - 브라우저가 ES6 모듈(ESM)을 사용하도록 Babel을 설정할 수 도 있으나, ESM을 사용하는 것은 문제가 존재하여, Webpack을 통해 해결

## 49.2 Webpack

- 의존 관계에 있는 자바스크립트, CSS, 이미지 등의 리소스들을 하나(또는 여러 개)의 파일로 번들링하는 모듈 번들러

- Webpack 사용 시 의존 모듈이 하나의 파일로 번들링되므로 별도의 모듈 로더가 필요 없음

- 여러 개의 자바스크립트 파일을 하나로 번들링하므로 HTML 파일에서 `script` 태그로 여러 개의 자바스크립트 파일을 로드해야하는 번거로움도 없어짐

### 49.2.1 Webpack 설치

```bash
$ npm i --save-dev webpack webpack-cli
```

### 49.2.2 babel-loader 설치

```bash
$ npm i --save-dev babel-loader
```

```json
{
  "scripts": {
    "build": "webpack -w"
  }
}
```

### 49.2.3 webpack.config.js 설정 파일 작성

- `webpack.config.js`는 Webpack이 실행될 때 참조하는 설정 파일

```js
const path = require('path');

module.exports = {
  // entry file
  // https://webpack.js.org/configuration/entry-context/#entry
  entry: './src/js/main.js',
  // 번들된 js 파일의 이름(filename)과 저장될 경로(path) 지정
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'js/bundle.js',
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        include: [path.resolve(__dirname, 'src/js')],
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env'],
            plugins: ['@babel/plugin-proposal-class-properties'],
          },
        },
      },
    ],
  },
  devtool: 'source-map',
  // https://webpack.js.org/configuration/mode
  mode: 'development',
};
```

```bash
$ npm run build
```

### 49.2.4 babel-polyfill 설치

- Babel을 사용해도 ES5 사양에 대채될 기능이 없는 `Promise`, `Object.assign`, `Array.from` 등은 트랜스파일링되지 못하기 때문에 `@babel/polyfill` 필요
  - 실제 운영 환경에서도 사용되어야하기때문에 dependencies으로 설치

```bash
$ npm i @babel/polyfill
```

```js
// src/js/main.js
import '@babel/polyfill';
import { pi, power, Foo } from './lib';
...
```

- Webpack 사용 경우, `webpack.config.js` 파일의 `entry` 배열에 폴리필 추가

```js
const path = require('path');

module.exports = {
  // entry file
  entry: ['@babel/polyfill', './src/js/main.js'],
};
```

```bash
$ npm run build
```
