> `Ajax(Asynchronous Javascript and XML)` : 자바스크립트를 사용하여 브라우저가 서버에게 `비동기 방식으로 데이터를 요청`하고, 서버가 응답한 데이터를 수신하여 웹페이지를 `동적으로 갱신`하는 프로그래밍 방식

- 브라우저에서 제공하는 Web API인 `XMLHttpRequest 객체를 기반으로 동작`
- XMLHttpRequest 객체는 `HTTP 비동기 통신을 위한 프로퍼티와 메서드를 제공`

전통적인 웹 페이지 생명 주기

- Ajax 등장 이전 웹 페이지는 `완전한 HTML을 서버로부터 전송받아 웹 페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작`
  - `화면이 전환되면 서버로부터 새로운 HTML을 전송받아 웹 페이지 전체를 리렌더링했다.`
- 이러한 방식은 다음과 같은 단점이 존재
  1. 이전 웹 페이지와 차이가 없어서 변경할 필요가 없는 부분까지도 새로 HTML을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신이 발생
  2. 변경할 필요가 없는 부분까지 처음부터 리렌더링하기 때문에 화면 전환이 일어나면 화면의 순간적인 깜박임 현상이 발생
  3. 클라이언트 - 서버간의 통신이 동기 방식으로 동작하기 때문에 서버로부터 응답이 있을 때까지 이후 작업은 블로킹(blocking)

Ajax

- `Ajax는 전통적인 방식과 비교했을 때 다음과 같은 장점이 존재`
  1. 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
  2. 변경할 필요가 없는 부분은 리렌더링하지 않는다. 즉 화면의 깜박임 현상이 없다.
  3. 클라이언트 - 서버간의 통신이 비동기 방식으로 작동, 따라서 서버에게 요청을 보낸 후 블로킹(blocking)이 발생하지 않는다.

<br />

# JSON

> `JSON(Javascript Ojbect Notation)` : 클라이언트 - 서버 간의 HTTP 통신을 위한 `텍스트 데이터 포맷`

- 언어에 종속 받지 않는, 언어 독립형 데이터 포맷

### JSON 표기 방식

> `키와 값으로 구성된 순수 텍스트`

- JSON `키는 반드시 쌍따옴표(””)`로 묶어야 한다.

```json
{
  "name": "WI",
  "age": 100,
  "alive": true
}
```

<br />
<br />

### JSON.stringify 메서드

> `객체 → JSON 포맷의 문자열로 변환`

- 클라이언트가 서버로 객체를 전송하려면 객체 → 문자열화 필요
- 이를, `직렬화(serializing)` 라 한다.

```jsx
const obj = {
  name: 'WI',
  age: 100,
  alive: true,
  hobby: ['programming', 'music'],
};

// 객체 -> JSON (serializing)
const json = JSON.stringify(obj);
console.log(json);
// {"name":"WI","age":100,"alive":true,"hobby":["programming","music"]}

const prettyJSON = JSON.stringify(obj, null, 2);
console.log(typeof prettyJSON, prettyJSON);
// string {
//   "name": "WI",
//   "age": 100,
//   "alive": true,
//   "hobby": [
//     "programming",
//     "music"
//   ]
// }

// JSON.stringify(obj, replacer, space) 에서 replacer 메서드 정의
function filter(key, value) {
  // undefined 는 반환 X
  return typeof value === 'number' ? undefined : value;
}

const strFilteredJSON = JSON.stringify(obj, filter, 2);
console.log(typeof strFilteredJSON, strFilteredJSON);
// string {
//   "name": "WI",
//   "alive": true,
//   "hobby": [
//     "programming",
//     "music"
//   ]
// }
```

<br />
<br />

### JSON.parse 메서드

> `JSON 포맷의 문자열 → 객체로 변환`

- 서버로부터 클라이언트에게 전송된 JSON 문자열 데이터 → 객체화 필요
- 이를, `역직렬화(deserializing)` 라 한다.

```jsx
const obj = {
  name: 'WI',
  age: 100,
  alive: true,
  hobby: ['programming', 'music'],
};

// 객체 -> JSON (serializing)
const json = JSON.stringify(obj);

// JSON -> 객체 (deserializing)
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
// object {
//   name: 'WI',
//   age: 100,
//   alive: true,
//   hobby: [ 'programming', 'music' ]
// }
```

<br />
<br />

# XMLHttpRequest 객체

> `XMLHttpRequest` : 자바스크립트를 사용하여 HTTP 요청을 전송시 사용

- 브라우저는 주소창이나 HTML의 form 태그 or a 태그를 통해 HTTP 요청 전송 기능을 기본 제공
- XMLHttpRequest 객체는 `HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 프로퍼티와 메서드를 제공`

<br />

### XMLHttpRequest 객체 생성

> XMLHttpRequest 객체는 `XMLHttpRequest 생성자 함수를 호출하여 생성`

- XMLHttpRequest 객체는 브라우저에서 제공하는 Web API이므로 `브라우저 환경에서만 정상 작동한다.`

```jsx
const xhr = new XMLHttpRequest();
```

<br />

### XMLHttpRequest 객체 - 프로토타입 프로퍼티

| 프로토타입 프로퍼티 | 설명                                  |
| ------------------- | ------------------------------------- |
| readyState          | HTTP 요청의 현재 상태를 나타내는 정수 |

XMLHttpRequest 의 정적 프로퍼티를 값으로 갖는다.

- UNSENT : 0
- OPENED : 1
- HEADERS_RECEIVED : 2
- LOADING : 3
- DONE : 4 |
  | status | HTTP 요청에 대한 응답 상태(HTTP 상태코드)를 나타내는 정수

예) 200 |
| statusText | HTTP 요청에 대한 응답 메시지를 나타내는 문자열

예) “OK” |
| responseType | HTTP 응답 타입

예) document, json, text, blob, arraybuffer 등 |
| response | HTTP 요청에 대한 응답 몸체(response body)
responseType 에 따라 타입이 다르다. |
| responseText | 서버가 전송한 HTTP 요청에 대한 응답 문자열 |

<br />

### XMLHttpRequest 객체 - 정적 프로퍼티

| 정적 프로퍼티    | 값  | 설명                                  |
| ---------------- | --- | ------------------------------------- |
| UNSENT           | 0   | open 메서드 호출 이전                 |
| OPENED           | 1   | open 메서드 호출 이후                 |
| HEADERS_RECEIVED | 2   | send 메서드 호출 이후                 |
| LOADING          | 3   | 서버 응답 중(응답 데이터 미완성 상태) |
| DONE             | 4   | 서버 응답 완료                        |

<br />

### XMLHttpRequest 객체 - 메서드

| 메서드            | 설명                                     |
| ----------------- | ---------------------------------------- |
| open              | HTTP 요청 초기화                         |
| send              | HTTP 요청 전송                           |
| abort             | 이미 전송된 HTTP 요청 중단               |
| setRequestHeader  | 특정 HTTP 요청 헤더의 값 설정            |
| getResponseHeader | 특정 HTTP 요청 헤더의 값을 문자열로 반환 |

<br />

### XMLHttpRequest 객체 - 이벤트 핸들러 프로퍼티

| 이벤트 핸들러 프로퍼티 | 설명                                              |
| ---------------------- | ------------------------------------------------- |
| onreadystatechange     | readyState 프로퍼티 값이 변경된 경우              |
| onerror                | HTTP 요청에 에러가 발생한 경우                    |
| onload                 | HTTP 요청이 성공적으로 완료한 경우                |
| onloadstart            | HTTP 요청에 대한 응답을 받기 시작한 경우          |
| onloadend              | HTTP 요청이 완료한 경우 ( 성공 or 실패하면 발생 ) |
| onabort                | abort 메서드에 의해 HTTP 요청이 중단된 경우       |
| onprogress             | HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생 |
| ontimeout              | HTTP 요청 시간이 초과한 경우                      |

<br />
<br />

# HTTP 요청 전송

> HTTP 요청을 전송하는 과정은 다음 순서를 따른다.

1. `XMLHttpRequest.prototype.open 메서드 → HTTP 요청을 초기화`
2. (필요에 따라) XMLHttpRequest.prototype.setRequestHeader 메서드 → 특정 HTTP 요청의 헤더 값을 설정
3. `XMLHttpRequest.prototype.send 메서드 → HTTP 요청을 전송`

```jsx
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', '/user');

// HTTP 요청 헤더 설정
// 클라이언트 -> 서버로 전송할 데이터의 MIME 타입 지정 : JSON
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send();
```

<br />

### XMLHttpRequest.prototype.open 메서드

> 서버에 전송할 HTTP 요청을 초기화

```jsx
xhr.open(mehtod, url[, async])
```

| 매개변수 | 설명                                                          |
| -------- | ------------------------------------------------------------- |
| method   | HTTP 요청 메서드(”GET”, “POST”, “PUT”, “PATCH”, “DELETE” 등 ) |
| url      | HTTP 요청을 전송할 URL                                        |
| async    | ( 옵션 ) 비동기 요청 여부, default = true                     |

- HTTP 요청 메서드 → `CRUD를 구현`

| HTTP 요청 메서드 | 종류           | 목적                  | 페이로드 |
| ---------------- | -------------- | --------------------- | -------- |
| GET              | index/retrieve | 모든/특정 리소스 취득 | X        |
| POST             | create         | 리소스 생성           | O        |
| PUT              | replace        | 리소스의 전체 교체    | O        |
| PATCH            | modify         | 리소스의 일부 수정    | O        |
| DELETE           | delete         | 모든/특정 리소스 삭제 | X        |

<br />

### XMLHttpRequest.prototype.send 메서드

> open 메서드로 초기화된 HTTP 요청을 서버에 전송

- 요청 메서드가 GET, POST 냐에 따라 전송 방식의 차이가 있다.
  - `GET 요청 메서드` → 데이터를 URL의 일부분인 `쿼리 문자열(query string)로 서버에 전송`
    - 페이로드로 전달한 인수는 무시되고 요청 몸체는 null로 설정된다.
  - `POST 요청 메서드` → 데이터(페이로드,payload)를 `요청 몸체(request body)에 담아 전송`
    - 요청 몸체에 담아 전송할 데이터(페이로드)를 인수로 전달할 수 있다.
      - `페이로드가 객체인 경우 → 반드시 JSON.stringify 메서드로 직렬화한 다음 전송할 것`
      ```jsx
      xhr.send(JSON.stringify(obj));
      ```

<br />

### XMLHttpRequest.prototype.setRequestHeader 메서드

> 특정 HTTP 요청 헤더 값을 설정

- `반드시, open 메서드 호출 이후에 호출할 것`
- 자주 사용하는 HTTP 요청 헤더에는 `Content-type` 과 `Accept` 가 있다.

  - Content-type은 request body에 담아 전송할 데이터의 [MIME](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types) 타입의 정보를 표현
    | MIME 타입 | 서브타입 |
    | ----------- | -------------------------------------------------- |
    | text | text/plain, text/html/, text/css, text/javascript |
    | application | application/json, application/x-www-form-urlencode |
    | multipart | multipart/formed-date |
  - HTTP 클라이너트가 서버에 요청할 때 `서버가 응답할 데이터의 MIME 타입을 Accept 로 설정 가능`

    ```jsx
    xhr.setRequestHeader('accept', 'application/json');
    ```

    - Accept 헤더를 설정하지 않으면 send 메서드가 호출될 때 → Accept 헤더 `*/*` 으로 전송

      \*\*

<br />
<br />

# HTTP 응답 처리

> `서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야 한다.`

- XMLHttpRequest 이벤트 핸들러 프로퍼티가 많았지만, 이 중

  - HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티 값이 변경된 경우 발생하는 → readystatechange 이벤트 캐치
    - send 메서드로 HTTP 요청을 서버에 전송하면 서버는 응답을 반환해야 한다.
    - 하지만, 언제 응답이 클라이언트에 도달하는지 알 수 없기 때문에 사용

  ```jsx
   const xhr = new XMLHttpRequest();

  xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

  xhr.send();

  xhr.onreadystatechange = () => {
  	  // 서버 응답이 아직 완료되지 않았으면 아무런 처리하지 않는다.
      if(xhr.readyState !== XMLHttpRequest.DONE) return;

      if(xhr.status === 200) {
  				// 정상적으로 응답된 상태면 parsing 한 데이터 출력
          console.log(JSON.parse(xhr.response));
      } else {
  				// 정상적인 응답을 받지 못하고 에러면 에러상태코드와 메시지 출력
          console..error('Error', xhr.status, xhr.statusText);
      }
  };
  ```

  - readystatechange 이벤트 대신 → load 이벤트를 캐치할 수도 있다.
    - 이 경우, HTTP 요청이 성공적으로 완료된 경우만 발생하므로, readyState 가 XMLHttpRequest.DONE 인지 확인할 필요가 없다.

  ```jsx
  const xhr = new XMLHttpRequest();

  xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

  xhr.send();

  // onload 메서드는 요청이 성공한다는 전체하에 실행되는 이벤트 핸들러
  xhr.onload = () => {
    if (xhr.status === 200) {
      console.log(JSON.parse(xhr.response));
    } else {
      console.log('Error', xhr.status, xhr.statusText);
    }
  };
  ```

> `ES6`에서 비동기 처리를 위한 또 다른 패턴으로 `프로미스(Promise)를 도입`

- 프로미스는 전통적인 콜백 패턴이 가진 `콜백지옥의 가독성 문제와 에러 처리가 곤란하다는 단점을 보완`
- `비동기 처리 시점을 명확하게 표현할 수 있다는 장점이 있다.`

<br />
<br />

# 비동기 처리 - 기존 콜백 패턴의 단점

> 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.

- 비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않았다 해도 기다리지 않고 `즉시 종료`되기 때문

```jsx
let g = 0;

// setTimeout 비동기 함수의 콜백 함수에서 전역 변수 g의 값이 변경되길 기대
// 실제로는 변수 g의 값은 변경되지 않는다.
// setTimeout 의 콜백함수가 콜 스택으로 이동해서 실행되는 시점에는 이미 console.log 함수가 끝난 시점이기 때문
setTimeout(() => {
  g = 100;
}, 0);

console.log(g); // 0
```

> 따라서, `비동기 함수의 처리 결과(대표적으로, 서버의 응답 등)에 대한 후속 처리는 비동기 함수 내부에서 수행해야 한다.`

- 이때 후속 처리를 위한 비동기 함수를 `콜백 함수로 전달`하는 것이 일반적
- 필요에 따라, `비동기 처리가 성공하면 호출될 콜백 함수`와 `비동기 처리가 실패하면 호출될 콜백 함수를 전달`하는 것

```jsx
// GET 요청을 위한 비동기 함수
// 요청 결과에 따라 수행할 성공,실패 콜백 함수 전달
const get = (url, successCallback, failureCallback) => {
  const xhr = new XMLHttpRequest();
  xhr.open();
  xhr.send('GET', url);

  xhr.onload = () => {
    if (xhr.status === 200) {
      successCallback(JSON.parse(xhr.response));
    } else {
      failureCallback(xhr.status);
    }
  };
};

get('https://jsonplaceholder.typicode.com/posts/1', console.log, console.error);
// {
//   userId: 1,
//   id: 1,
//   title: 'sunt aut facere repellat provident occaecati excepturi optio reprehenderit',
//   body: 'quia et suscipit\nsuscipit recusandae consequuntur …strum rerum est autem sunt rem eveniet architecto'
// };
```

> 다만, 이런식으로 후속 처리에 필요한 비동기 함수를 콜백 함수로 전달하고 호출하는 방식이 `중첩되면 복잡도가 높아지는 현상이 발생` → `콜백지옥(callback hell)`

```jsx
...

get("/step1", (a) => {
  get(`/step2/${a}`, (b) => {
    get(`/step3/${b}`, (c) => {
      get(`/step4/${c}`, (d) => {
        console.log(d);
      });
    });
  });
});
```

> 또한, 콜백 패턴의 문제점 중 가장 심각한 것은 `에러 처리가 곤란`하다는 것

- `에러는 호출자(caller)방향으로 전파된다.`
  - 즉, 현재 실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향으로 전파된다.

```jsx
try {
  setTimeout(() => {
    throw new Error('Error!');
  }, 1000);
} catch (error) {
  // 이벤트 큐에서 콜 스택으로 setTimeout 의 콜백 함수로 이동되어 실행되는 시점에서 setTimout 함수는 이미, 콜 스택에서 pop된 상태이고 주체가 아니다.
  // 콜백 함수의 호출자(caller)가 setTimeout 인데, 에러 발생 시점에 존재하지 않기 때문에 Error 를 캐치할 수 없다.
  console.error('캐치 에러 :', error);
}
```

<br />
<br />

# 프로미스 생성

> `Promise 생성자 함수를 new 키워드와 함께 호출`하므로써 프로미스(`Promise 객체`)를 생성

- 프로미스는 [호스트 객체](https://poiemaweb.com/js-built-in-object)가 아닌 ECAMScript 사양에 정의된 `표준 빌트인 객체`
- Promise 생성자 함수는 `비동기 처리를 수행할 콜백 함수를 인수로 전달 받는다.`
  - `resolve` 와 `reject 함수`를 인수로 전달
  ```jsx
  const promise = new Promise((resovle, reject) => {
    if(비동기 처리 성공) {
      resolve('result');
    } else {
      reject('failure Error');
    }
  })
  ```
- 프로미스는 `현재 비동기 처리가 어떻게 진행되고 있는지`를 나타내는 `상태(state)정보`를 가진다.
  | 프로미스 상태 정보 | 의미 | 상태 변경 조건 |
  | ------------------ | ------------------------------------- | -------------------------------- |
  | pending | 비동기 처리가 아직 수행되지 않은 상태 | 프로미스가 생성된 직후 기본 상태 |
  | fulfilled | 비동기 처리가 수행된 상태( 성공 ) | resolve 함수 호출 |
  | rejected | 비동기 처리가 수행된 상태( 실패 ) | reject 함수 호출 |
  - 기본적으로는 `pending 상태이며, 이후 비동기 처리가 수행되면 비동기 처리 결과에 따라 프로미스 상태가 변경`
    - 비동기 처리 성공 → resolve 함수를 호출 → 프로미스를 fulfilled 상태로 변경
    - 비동기 처리 실패 → reject 함수를 호출 → 프로미스를 rejected 상태로 변경
  - 즉, `프로미스 상태는 resolve 또는 reject 함수를 호출하는 것으로 결정`
  - rejected 또는 fulfilled 상태를 `settled 상태`라고 한다.
    - 즉, settled 상태는 비동기 처리가 수행된 상태를 의미
    - 한 번 settled 상태가 되면 더는 다른 상태로 변화할 수 없다.
- 프로미스는 `비동기 처리 상태와 비동기 처리 결과도 상태로 갖는다.`

  ```jsx
  const fulfilled = new Promise((resolve) => resolve(1));
  console.log(fulfilled);
  /*
  [[Prototype]]: Promise
  [[PromiseState]]: "fulfilled"
  [[PromiseResult]]: 1
  */

  const rejected = new Promise((_, reject) =>
    reject(new Error('failure Error'))
  );
  console.log(rejected);
  /*
  [[Prototype]]: Promise
  [[PromiseState]]: "rejected"
  [[PromiseResult]]: Error: failure Error
  */
  ```

<br />
<br />

# 프로미스 후속 처리 메서드

> 프로미스의 `비동기 처리 상태가 변경되면, 이에 따른 후속 처리를 해야 한다.`

- 프로미스가 `fulfilled 상태가 되면` → 프로미스의 `처리 결과를 가지고 무언가를 수행`
- 프로미스가 `rejected 상태가 되면` → 프로미스의 `처리 결과(에러)를 가지고 에러 처리를 수행`

> 프로미스의 비동기 처리 상태가 변화하면, `후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출`된다.

- 이를 위해, 프로미스는 후속 처리 메서드 `then, catch, finally` 를 제공

<br />

### Promise.prototype.then

> `두 개의 콜백 함수를 인수로 전달받는다.`

- 첫 번째 콜백 함수 → 프로미스가 fulfilled 상태(resolve 함수가 호출된 상태)가 되면 호출, 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 전달받음
- 두 번째 콜백 함수 → 프로미스가 rejected 상태(reject 함수가 호출된 상태)가 되면 호출, 콜백 함수는 프로미스의 에러를 인수로 전달받음

```jsx
new Promise((resolve) => resolve('fulfilled')).then(
  (v) => console.log(v),
  (e) => console.error(e)
);
// fulfilled

new Promise((_, reject) => reject(new Error('rejected'))).then(
  (v) => console.log(v),
  (e) => console.error(e)
);
// Error: rejected
```

- `then 메서드는 언제나 프로미스를 반환`
  - then 메서드의 콜백 함수가 프로미스를 반환하면 그 프로미스를 그대로 반환
  - `값을 반환`하면 그 값을 `암묵적으로 resolve 또는 reject 하여 프로미스를 생성해 반환`

<br />

### Promise.prototype.catch

> `한 개의 콜백 함수를 인수로 전달받는다.`

- 프로미스가 `rejected 상태(reject 함수가 호출된 상태)인 경우만 호출`
- catch 메서드는 then(undefined, onRejected) 과 동일하게 동작
- `catch 메서드도 언제나 프로미스를 반환`

```jsx
new Promise((_, reject) => reject(new Error('rejected'))).catch((e) =>
  console.log(e)
); // Error: rejected

// 위에 catch 문과 같은 기능을 하는 then 버전
new Promise((_, reject) => reject(new Error('rejected'))).then(undefined, (e) =>
  console.log(e)
); // Error: rejected
```

<br />

### Promise.prototype.finally

> `한 개의 콜백 함수를 인수로 전달받는다.`

- 프로미스의 `성공 또는 실패와 상관없이 무조건 한 번 호출된다.`
- 프로미스의 상태와 상관없이 `공통적으로 수행해야 할 처리 내용이 있을 때 유용`
- `finally 메서드도 언제나 프로미스를 반환`

```jsx
const promiseGET = (url) => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();

    xhr.onload = () => {
      if (xhr.status === 200) {
        resolve(JSON.parse(xhr.response));
      } else {
        reject(new Error(xhr.status));
      }
    };
  });
};

promiseGET('https://jsonplaceholder.typicode.com/posts/1')
  .then((res) => console.log(res))
  .catch((err) => console.log(err))
  .finally(() => console.log('End !'));

// {
//   userId: 1,
//   id: 1,
//   title: 'sunt aut facere repellat provident occaecati excepturi optio reprehenderit',
//   body: 'quia et suscipit\nsuscipit recusandae consequuntur …strum rerum est autem sunt rem eveniet architecto'
// }
// End !
```

<br />
<br />

# 프로미스의 에러 처리

> 두 가지 방법이 있다.

- `then 메서드의 두 번째 콜백 함수로 처리하는 방법`

  - then 메서드는 두 개의 콜백 함수를 전달 받는다고 했다.
  - 즉, then(성공 시, 실패 시)를 의미했다.
  - 따라서, then(\_ , onRejected)의 형태로 호출하면 에러 처리 가능

  ```jsx
  const promiseGET = (url) => { ... }

  const wrongURL = "https://wrongURL.com";
  // then 메서드 두 번째 파라미터로 에러 캐칭 메서드를 넘기는 방식
  promiseGET(wrongURL).then(
    (res) => console.log(res),
    (err) => console.log(err),
  );

  ```

- `catch 메서드로 처리하는 방법`

  ```jsx
  const promiseGET = (url) => { ... }

  // catch 메서드를 사용하는 방식
  promiseGET(wrongURL)
    .then((res) => console.log(res))
    .catch((err) => console.log(err));

  // catch 메서드는 내부적으로 다음과 같다.
  promiseGET(wrongURL)
    .then((res) => console.log(res))
    .then(undefined, (err) => console.log(err));
  ```

> 다만, 일반적으로 `프로미스의 에러 처리는 catch 메서드로 처리한다.`

- then 메서드의 두 번째 인수로 에러 처리 콜백 함수에서 발생한 에러는 `캐치하지 못하고 코드가 복잡해져 가독성도 좋지 않다는 단점이 존재`하기 때문

<br />
<br />

# 프로미스 체이닝

> `프로미스 체이닝(Promise chainning)` : then, catch, finally 후속 처리 메서드는 언제나 `프로미스(Promise 객체)를 반환하기 때문에`, `연속적으로 호출(체이닝)이 가능`

- 프로미스는 프로미스 체이닝을 통해 비동기 처리 결과를 전달받아 후속 처리를 하므로 비동기 처리를 위한 콜백 패턴의 문제였던 콜백지옥이 발생하지 않는다.
- 다만, 프로미스도 콜백 패턴을 사용하므로 콜백 함수를 아예 사용하지 않는 것은 아니다.

> `콜백 패턴은 가독성이 좋지 않다.`

- 이 문제를 `ES8`에 도입된 `async / await 를 통해 해결 가능하다.`

  - 프로미스의 후속 처리 메서드 없이 마치 `동기 처리`처럼 프로미스가 처리 결과를 반환하도록 구현 가능

  ```jsx
  const promiseGET = (url) => { ... }
  const url = "https://jsonplaceholder.typicode.com";

  (async () => {
    const { userId } = await promiseGET(`${url}/posts/1`);
    const userInfo = await promiseGET(`${url}/users${userId}`);
    console.log(userInfo);
  })();
  ```

<br />
<br />

# 프로미스 정적 메서드

> Promise 는 생성자 함수, 즉 함수도 객체이므로 메서드를 가진다.

- 5가지 정적 메서드를 제공

<br />

### Promise.resolve / Promise.reject 정적 메서드

> `이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용`

- Promise.resolve → 인수로 전달받은 값을 resolve 하는 프로미스를 생성

  ```jsx
  const resolvedPromise = Promise.resolve([1, 2, 3]);
  // const resolvedPromise = new Promise((resolve) => resolve([1, 2, 3]));

  resolvedPromise.then((v) => console.log(v)); // [ 1, 2, 3 ]
  ```

- Promise.reject → 인수로 전달받은 값(에러)을 reject 하는 프로미스를 생성

  ```jsx
  const rejectedPromise = Promise.reject(new Error('Error !'));
  // const rejectedPromise = new Promise((_, reject) => reject(new Error("Error !")));

  rejectedPromise.catch((err) => console.log(err)); // Error: Error !
  ```

<br />

### Promise.all 정적 메서드

> 여러 개의 비동기 처리를 모두 `병렬(parallel)처리`할 때 사용

- 서로 독립적인 여러 비동기 처리를 Promise.prototype.then 후속 처리 메서드로 체이닝 하는 경우

  ```jsx
  const req1 = () =>
    new Promise((resolve) => setTimeout(() => resolve(1), 3000));
  const req2 = () =>
    new Promise((resolve) => setTimeout(() => resolve(2), 2000));
  const req3 = () =>
    new Promise((resolve) => setTimeout(() => resolve(3), 1000));

  const res = [];
  req1()
    .then((data) => {
      res.push(data);
      return req2();
    })
    .then((data) => {
      res.push(data);
      return req3();
    })
    .then((data) => {
      res.push(data);
      console.log(res);
    })
    .catch((err) => console.log(err));

  // 약 6초 뒤..
  // [ 1, 2, 3 ]
  ```

- 서로 독립적인 여러 비동기 처리를 Promise.all 정적 메서드로 처리하는 경우

  ```jsx
  const req1 = () =>
    new Promise((resolve) => setTimeout(() => resolve(1), 3000));
  const req2 = () =>
    new Promise((resolve) => setTimeout(() => resolve(2), 2000));
  const req3 = () =>
    new Promise((resolve) => setTimeout(() => resolve(3), 1000));

  Promise.all([req1(), req2(), req3()])
    .then((res) => console.log(res))
    .catch((err) => console.log(err));

  // 약 3초 뒤.. ( 가장 오래 걸리는 비동기 처리가 3000ms(약 3초))
  // [ 1, 2, 3 ]
  ```

- Promise.all 메서드는 `프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.`
- 전달받은 모든 프로미스가 `모두 fulfilled 상태가 되면 모든 처리 결과를 배열에 저장해 새로운 프로미스를 반환`
  - Promise.all 메서드가 종료하는 데 걸리는 시간 = 가장 늦게 fulfilled 상태가 되는 프로미스의 처리 시간 + a 시간
  - `순서가 보장된다.`
    - 가장 늦게 fulfilled 되는 프로미스가 배열 요소에 앞에 있어도, 순서대로 배열에 저장 후 새로운 프로미스를 반환
- 전달받은 프로미스 요소들 중, `하나라도 rejected 상태가 되면 나머지 프로미스의 진행 상태와 상관없이 기다리지 않고 즉시 종료`

  ```jsx
  const req1 = () =>
    new Promise((_, reject) =>
      setTimeout(() => reject(new Error('Error 1')), 3000)
    );
  const req2 = () =>
    new Promise((_, reject) =>
      setTimeout(() => reject(new Error('Error 2')), 2000)
    );
  const req3 = () =>
    new Promise((_, reject) =>
      setTimeout(() => reject(new Error('Error 3')), 1000)
    );

  Promise.all([req1(), req2(), req3()])
    .then((res) => console.log(res))
    .catch((err) => console.log(err));

  // 약 1초 뒤..
  // Error: Error 3 << 모든 프로미스 작업 중 가장 빠르게 rejected 되는 프로미스의 reject 함수를 호출
  ```

- 전달받은 이터러블의 `요소들이 프로미스가 아닌 값인 경우, Promise.resolve 메서드를 통해 프로미스로 래핑한다.`

  ```jsx
  Promise.all([1, 2, 3])
    .then((res) => console.log(res))
    .catch((err) => console.log);

  // [ 1, 2, 3 ]
  ```

<br />

### Promise.race 정적 메서드

> Promise.all 메서드와 동일하게 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.

- 다만, Promise.all 메서드처럼 모든 프로미스 요소들이 fulfilled 될 때까지 기다리지 않고, `가장 먼저 fulfilled 상태가 되는 프로미스의 처리 결과를 resolve 하는 새로운 프로미스를 반환`

```jsx
const req1 = () => new Promise((resolve) => setTimeout(() => resolve(1), 3000));
const req2 = () => new Promise((resolve) => setTimeout(() => resolve(2), 2000));
const req3 = () => new Promise((resolve) => setTimeout(() => resolve(3), 1000));

Promise.race([req1(), req2(), req3()])
  .then((res) => console.log(res))
  .catch((err) => console.log(err));

// 약 1초 뒤.. ( 가장 먼저 fulfilled 상태가 되는 프로미스 처리 결과를 resolve 함수 호출로 프로미스 반환 )
// 3
// 나머지 약 2초 뒤, 종료 ( 가장 오래 걸리는 프로미스 기준 )
```

<br />

### Promise.allSetteld 정적 메서드

> `ES11`에 도입, 전달받은 프로미스가 모두 `settled 상태(= 비동기 처리가 수행된 상태)가 되면, 처리 결과를 배열로 반환`

- 프로미스가 fulfilled 상태인 경우 → `{ status, value }` 프로퍼티를 갖는다.
- 프로미스가 rejected 상태인 경우 → `{ status, reason }` 프로퍼티를 갖는다.
- 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.

```jsx
const resolvedPromise = new Promise((resolve) =>
  setTimeout(() => resolve(1), 2000)
);
const rejectedPromise = new Promise((_, reject) =>
  setTimeout(() => reject(new Error('Error !'), 1000))
);

Promise.allSettled([resolvedPromise, rejectedPromise]).then((res) =>
  console.log(res)
);
// [
//   { status: 'fulfilled', value: 1 },
//   {
//     status: 'rejected',
//     reason: Error: Error !
//         at Timeout._onTimeout (C:\Users\forze\Desktop\Docs-modernJS__deepDive\37-Promise\index.js:2:76)
//         at listOnTimeout (node:internal/timers:556:17)
//         at processTimers (node:internal/timers:499:7)
//   }
// ]
```

<br />
<br />

# 마이크로태스크 큐

```jsx
setTimeout(() => console.log(1), 0);

Promise.resolve()
  .then(() => console.log(2))
  .then(() => console.log(3));

// 2 3 1
```

- 위에 예제의 결과는 1 → 2 → 3 이 아닌, `2 → 3 → 1 순`으로 출력된다.

> 그 이유는, `프로미스 후속 처리 메서드의 콜백 함수는` 태스크 큐(task queue, = event queue)가 아닌 `마이크로태스크 큐(microtask queue, = job queue)에 저장된다.`

- 마이크로태스크 큐는 태스크 큐와는 브라우저에서 제공하는 `별도의 큐다.`
  - `마이크로태스크 큐(= 잡 큐) → 프로미스 후속 처리 메서드의 콜백 함수가 일시적으로 보관`
  - `태스크 큐(= 이벤트 큐) → 그 외의 비동기 함수의 콜백 함수나 이벤트 핸들러가 일시적으로 보관`
- `우선 순위 = 마이크로태스크 큐 > 태스크 큐`
  - 즉, 이벤트 루프는 콜 스택이 비면 먼저 마이크로태스크 큐에서 대기하고 있는 함수를 가져와 실행
  - 이후 마이크로태스크 큐가 비면 태스크 큐에서 대기하고 있는 함수를 가져와 실행

<br />
<br />

# fetch

> XMLHttpRequest 객체와 마찬가지로 `HTTP 요청 전송 기능을 제공`하는 `클라이언트 사이드 Web API`

- XTMLHttpRequest 객체보다 사용법이 간단하며, 프로미스를 지원하기 때문에 비동기 처리를 위한 콜백 패턴에서 자유롭다.

```jsx
const promise = fetch(url [, options]);
```

> fetch 함수는 HTTP 응답을 나타내는 `Response 객체를 래핑한 Promise 객체를 반환`

```jsx
fetch('https://jsonplaceholder.typicode.com/todos/1').then((res) =>
  console.log(res)
);

// Response
// {
//   type: 'cors',
//   url: 'https://jsonplaceholder.typicode.com/todos/1',
//   redirected: false,
//   status: 200,
//   ok: true,
//   …
// }
```

- Response 객체의 `Response.prototype` 에는 Response 객체에 포함되어 있는 `HTTP 응답 몸체를 위한 다양한 메서드를 제공한다.`
  - `Response.prototpye.json 메서드` : fetch 함수가 반환한 프로미스가 래핑하고 있는 `MIME 타입이 application/json 인 HTTP 응답 몸체를 취득`
    - Response 객체에서 HTTP Response Body를 취득하여 `역직렬화(deserializing)`한다. ( `서버 응답 데이터 획득` )

<br />

### fetch - CRUD 예시

```jsx
// HTTP Request(요청) 모음 객체 생성
const request = {
  get(url) {
    return fetch(url);
  },

  post(url, payload) {
    return fetch(url, {
      method: 'POST',
      headers: { 'content-Type': 'application/json ' },
      body: JSON.stringify(payload),
    });
  },

  patch(url, payload) {
    return fetch(url, {
      method: 'PATCH',
      headers: { 'content-Type': 'application/json ' },
      body: JSON.stringify(payload),
    });
  },

  delete(url) {
    return fetch(url, { method: 'DELETE ' });
  },
};

const url = 'HTTP 통신 요청할 url';

// GET 예시
request
  .get(url)
  .then((res) => res.json())
  .then((data) => console.log(data));

// POST 예시
request
  .post(url, { obj })
  .then((res) => res.json())
  .then((data) => console.log(data))
  .catch((err) => console.error(err));

// PATCH 예시
request
  .patch(url, { obj })
  .then((res) => res.json())
  .then((data) => console.log(data))
  .catch((err) => console.error(err));

// DELETE 예시
request
  .delete(url, { obj })
  .then((res) => res.json())
  .then((data) => console.log(data))
  .catch((err) => console.error(err));
```

> `ES6`에서 비동기 처리를 위한 또 다른 패턴으로 `프로미스(Promise)를 도입`

- 프로미스는 전통적인 콜백 패턴이 가진 `콜백지옥의 가독성 문제와 에러 처리가 곤란하다는 단점을 보완`
- `비동기 처리 시점을 명확하게 표현할 수 있다는 장점이 있다.`

<br />
<br />

# 비동기 처리 - 기존 콜백 패턴의 단점

> 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.

- 비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않았다 해도 기다리지 않고 `즉시 종료`되기 때문

```jsx
let g = 0;

// setTimeout 비동기 함수의 콜백 함수에서 전역 변수 g의 값이 변경되길 기대
// 실제로는 변수 g의 값은 변경되지 않는다.
// setTimeout 의 콜백함수가 콜 스택으로 이동해서 실행되는 시점에는 이미 console.log 함수가 끝난 시점이기 때문
setTimeout(() => {
  g = 100;
}, 0);

console.log(g); // 0
```

> 따라서, `비동기 함수의 처리 결과(대표적으로, 서버의 응답 등)에 대한 후속 처리는 비동기 함수 내부에서 수행해야 한다.`

- 이때 후속 처리를 위한 비동기 함수를 `콜백 함수로 전달`하는 것이 일반적
- 필요에 따라, `비동기 처리가 성공하면 호출될 콜백 함수`와 `비동기 처리가 실패하면 호출될 콜백 함수를 전달`하는 것

```jsx
// GET 요청을 위한 비동기 함수
// 요청 결과에 따라 수행할 성공,실패 콜백 함수 전달
const get = (url, successCallback, failureCallback) => {
  const xhr = new XMLHttpRequest();
  xhr.open();
  xhr.send('GET', url);

  xhr.onload = () => {
    if (xhr.status === 200) {
      successCallback(JSON.parse(xhr.response));
    } else {
      failureCallback(xhr.status);
    }
  };
};

get('https://jsonplaceholder.typicode.com/posts/1', console.log, console.error);
// {
//   userId: 1,
//   id: 1,
//   title: 'sunt aut facere repellat provident occaecati excepturi optio reprehenderit',
//   body: 'quia et suscipit\nsuscipit recusandae consequuntur …strum rerum est autem sunt rem eveniet architecto'
// };
```

> 다만, 이런식으로 후속 처리에 필요한 비동기 함수를 콜백 함수로 전달하고 호출하는 방식이 `중첩되면 복잡도가 높아지는 현상이 발생` → `콜백지옥(callback hell)`

```jsx
...

get("/step1", (a) => {
  get(`/step2/${a}`, (b) => {
    get(`/step3/${b}`, (c) => {
      get(`/step4/${c}`, (d) => {
        console.log(d);
      });
    });
  });
});
```

> 또한, 콜백 패턴의 문제점 중 가장 심각한 것은 `에러 처리가 곤란`하다는 것

- `에러는 호출자(caller)방향으로 전파된다.`
  - 즉, 현재 실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향으로 전파된다.

```jsx
try {
  setTimeout(() => {
    throw new Error('Error!');
  }, 1000);
} catch (error) {
  // 이벤트 큐에서 콜 스택으로 setTimeout 의 콜백 함수로 이동되어 실행되는 시점에서 setTimout 함수는 이미, 콜 스택에서 pop된 상태이고 주체가 아니다.
  // 콜백 함수의 호출자(caller)가 setTimeout 인데, 에러 발생 시점에 존재하지 않기 때문에 Error 를 캐치할 수 없다.
  console.error('캐치 에러 :', error);
}
```

<br />
<br />

# 프로미스 생성

> `Promise 생성자 함수를 new 키워드와 함께 호출`하므로써 프로미스(`Promise 객체`)를 생성

- 프로미스는 [호스트 객체](https://poiemaweb.com/js-built-in-object)가 아닌 ECAMScript 사양에 정의된 `표준 빌트인 객체`
- Promise 생성자 함수는 `비동기 처리를 수행할 콜백 함수를 인수로 전달 받는다.`
  - `resolve` 와 `reject 함수`를 인수로 전달
  ```jsx
  const promise = new Promise((resovle, reject) => {
    if(비동기 처리 성공) {
      resolve('result');
    } else {
      reject('failure Error');
    }
  })
  ```
- 프로미스는 `현재 비동기 처리가 어떻게 진행되고 있는지`를 나타내는 `상태(state)정보`를 가진다.
  | 프로미스 상태 정보 | 의미 | 상태 변경 조건 |
  | ------------------ | ------------------------------------- | -------------------------------- |
  | pending | 비동기 처리가 아직 수행되지 않은 상태 | 프로미스가 생성된 직후 기본 상태 |
  | fulfilled | 비동기 처리가 수행된 상태( 성공 ) | resolve 함수 호출 |
  | rejected | 비동기 처리가 수행된 상태( 실패 ) | reject 함수 호출 |
  - 기본적으로는 `pending 상태이며, 이후 비동기 처리가 수행되면 비동기 처리 결과에 따라 프로미스 상태가 변경`
    - 비동기 처리 성공 → resolve 함수를 호출 → 프로미스를 fulfilled 상태로 변경
    - 비동기 처리 실패 → reject 함수를 호출 → 프로미스를 rejected 상태로 변경
  - 즉, `프로미스 상태는 resolve 또는 reject 함수를 호출하는 것으로 결정`
  - rejected 또는 fulfilled 상태를 `settled 상태`라고 한다.
    - 즉, settled 상태는 비동기 처리가 수행된 상태를 의미
    - 한 번 settled 상태가 되면 더는 다른 상태로 변화할 수 없다.
- 프로미스는 `비동기 처리 상태와 비동기 처리 결과도 상태로 갖는다.`

  ```jsx
  const fulfilled = new Promise((resolve) => resolve(1));
  console.log(fulfilled);
  /*
  [[Prototype]]: Promise
  [[PromiseState]]: "fulfilled"
  [[PromiseResult]]: 1
  */

  const rejected = new Promise((_, reject) =>
    reject(new Error('failure Error'))
  );
  console.log(rejected);
  /*
  [[Prototype]]: Promise
  [[PromiseState]]: "rejected"
  [[PromiseResult]]: Error: failure Error
  */
  ```

<br />
<br />

# 프로미스 후속 처리 메서드

> 프로미스의 `비동기 처리 상태가 변경되면, 이에 따른 후속 처리를 해야 한다.`

- 프로미스가 `fulfilled 상태가 되면` → 프로미스의 `처리 결과를 가지고 무언가를 수행`
- 프로미스가 `rejected 상태가 되면` → 프로미스의 `처리 결과(에러)를 가지고 에러 처리를 수행`

> 프로미스의 비동기 처리 상태가 변화하면, `후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출`된다.

- 이를 위해, 프로미스는 후속 처리 메서드 `then, catch, finally` 를 제공

<br />

### Promise.prototype.then

> `두 개의 콜백 함수를 인수로 전달받는다.`

- 첫 번째 콜백 함수 → 프로미스가 fulfilled 상태(resolve 함수가 호출된 상태)가 되면 호출, 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 전달받음
- 두 번째 콜백 함수 → 프로미스가 rejected 상태(reject 함수가 호출된 상태)가 되면 호출, 콜백 함수는 프로미스의 에러를 인수로 전달받음

```jsx
new Promise((resolve) => resolve('fulfilled')).then(
  (v) => console.log(v),
  (e) => console.error(e)
);
// fulfilled

new Promise((_, reject) => reject(new Error('rejected'))).then(
  (v) => console.log(v),
  (e) => console.error(e)
);
// Error: rejected
```

- `then 메서드는 언제나 프로미스를 반환`
  - then 메서드의 콜백 함수가 프로미스를 반환하면 그 프로미스를 그대로 반환
  - `값을 반환`하면 그 값을 `암묵적으로 resolve 또는 reject 하여 프로미스를 생성해 반환`

<br />

### Promise.prototype.catch

> `한 개의 콜백 함수를 인수로 전달받는다.`

- 프로미스가 `rejected 상태(reject 함수가 호출된 상태)인 경우만 호출`
- catch 메서드는 then(undefined, onRejected) 과 동일하게 동작
- `catch 메서드도 언제나 프로미스를 반환`

```jsx
new Promise((_, reject) => reject(new Error('rejected'))).catch((e) =>
  console.log(e)
); // Error: rejected

// 위에 catch 문과 같은 기능을 하는 then 버전
new Promise((_, reject) => reject(new Error('rejected'))).then(undefined, (e) =>
  console.log(e)
); // Error: rejected
```

<br />

### Promise.prototype.finally

> `한 개의 콜백 함수를 인수로 전달받는다.`

- 프로미스의 `성공 또는 실패와 상관없이 무조건 한 번 호출된다.`
- 프로미스의 상태와 상관없이 `공통적으로 수행해야 할 처리 내용이 있을 때 유용`
- `finally 메서드도 언제나 프로미스를 반환`

```jsx
const promiseGET = (url) => {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();

    xhr.onload = () => {
      if (xhr.status === 200) {
        resolve(JSON.parse(xhr.response));
      } else {
        reject(new Error(xhr.status));
      }
    };
  });
};

promiseGET('https://jsonplaceholder.typicode.com/posts/1')
  .then((res) => console.log(res))
  .catch((err) => console.log(err))
  .finally(() => console.log('End !'));

// {
//   userId: 1,
//   id: 1,
//   title: 'sunt aut facere repellat provident occaecati excepturi optio reprehenderit',
//   body: 'quia et suscipit\nsuscipit recusandae consequuntur …strum rerum est autem sunt rem eveniet architecto'
// }
// End !
```

<br />
<br />

# 프로미스의 에러 처리

> 두 가지 방법이 있다.

- `then 메서드의 두 번째 콜백 함수로 처리하는 방법`

  - then 메서드는 두 개의 콜백 함수를 전달 받는다고 했다.
  - 즉, then(성공 시, 실패 시)를 의미했다.
  - 따라서, then(\_ , onRejected)의 형태로 호출하면 에러 처리 가능

  ```jsx
  const promiseGET = (url) => { ... }

  const wrongURL = "https://wrongURL.com";
  // then 메서드 두 번째 파라미터로 에러 캐칭 메서드를 넘기는 방식
  promiseGET(wrongURL).then(
    (res) => console.log(res),
    (err) => console.log(err),
  );

  ```

- `catch 메서드로 처리하는 방법`

  ```jsx
  const promiseGET = (url) => { ... }

  // catch 메서드를 사용하는 방식
  promiseGET(wrongURL)
    .then((res) => console.log(res))
    .catch((err) => console.log(err));

  // catch 메서드는 내부적으로 다음과 같다.
  promiseGET(wrongURL)
    .then((res) => console.log(res))
    .then(undefined, (err) => console.log(err));
  ```

> 다만, 일반적으로 `프로미스의 에러 처리는 catch 메서드로 처리한다.`

- then 메서드의 두 번째 인수로 에러 처리 콜백 함수에서 발생한 에러는 `캐치하지 못하고 코드가 복잡해져 가독성도 좋지 않다는 단점이 존재`하기 때문

<br />
<br />

# 프로미스 체이닝

> `프로미스 체이닝(Promise chainning)` : then, catch, finally 후속 처리 메서드는 언제나 `프로미스(Promise 객체)를 반환하기 때문에`, `연속적으로 호출(체이닝)이 가능`

- 프로미스는 프로미스 체이닝을 통해 비동기 처리 결과를 전달받아 후속 처리를 하므로 비동기 처리를 위한 콜백 패턴의 문제였던 콜백지옥이 발생하지 않는다.
- 다만, 프로미스도 콜백 패턴을 사용하므로 콜백 함수를 아예 사용하지 않는 것은 아니다.

> `콜백 패턴은 가독성이 좋지 않다.`

- 이 문제를 `ES8`에 도입된 `async / await 를 통해 해결 가능하다.`

  - 프로미스의 후속 처리 메서드 없이 마치 `동기 처리`처럼 프로미스가 처리 결과를 반환하도록 구현 가능

  ```jsx
  const promiseGET = (url) => { ... }
  const url = "https://jsonplaceholder.typicode.com";

  (async () => {
    const { userId } = await promiseGET(`${url}/posts/1`);
    const userInfo = await promiseGET(`${url}/users${userId}`);
    console.log(userInfo);
  })();
  ```

<br />
<br />

# 프로미스 정적 메서드

> Promise 는 생성자 함수, 즉 함수도 객체이므로 메서드를 가진다.

- 5가지 정적 메서드를 제공

<br />

### Promise.resolve / Promise.reject 정적 메서드

> `이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용`

- Promise.resolve → 인수로 전달받은 값을 resolve 하는 프로미스를 생성

  ```jsx
  const resolvedPromise = Promise.resolve([1, 2, 3]);
  // const resolvedPromise = new Promise((resolve) => resolve([1, 2, 3]));

  resolvedPromise.then((v) => console.log(v)); // [ 1, 2, 3 ]
  ```

- Promise.reject → 인수로 전달받은 값(에러)을 reject 하는 프로미스를 생성

  ```jsx
  const rejectedPromise = Promise.reject(new Error('Error !'));
  // const rejectedPromise = new Promise((_, reject) => reject(new Error("Error !")));

  rejectedPromise.catch((err) => console.log(err)); // Error: Error !
  ```

<br />

### Promise.all 정적 메서드

> 여러 개의 비동기 처리를 모두 `병렬(parallel)처리`할 때 사용

- 서로 독립적인 여러 비동기 처리를 Promise.prototype.then 후속 처리 메서드로 체이닝 하는 경우

  ```jsx
  const req1 = () =>
    new Promise((resolve) => setTimeout(() => resolve(1), 3000));
  const req2 = () =>
    new Promise((resolve) => setTimeout(() => resolve(2), 2000));
  const req3 = () =>
    new Promise((resolve) => setTimeout(() => resolve(3), 1000));

  const res = [];
  req1()
    .then((data) => {
      res.push(data);
      return req2();
    })
    .then((data) => {
      res.push(data);
      return req3();
    })
    .then((data) => {
      res.push(data);
      console.log(res);
    })
    .catch((err) => console.log(err));

  // 약 6초 뒤..
  // [ 1, 2, 3 ]
  ```

- 서로 독립적인 여러 비동기 처리를 Promise.all 정적 메서드로 처리하는 경우

  ```jsx
  const req1 = () =>
    new Promise((resolve) => setTimeout(() => resolve(1), 3000));
  const req2 = () =>
    new Promise((resolve) => setTimeout(() => resolve(2), 2000));
  const req3 = () =>
    new Promise((resolve) => setTimeout(() => resolve(3), 1000));

  Promise.all([req1(), req2(), req3()])
    .then((res) => console.log(res))
    .catch((err) => console.log(err));

  // 약 3초 뒤.. ( 가장 오래 걸리는 비동기 처리가 3000ms(약 3초))
  // [ 1, 2, 3 ]
  ```

- Promise.all 메서드는 `프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.`
- 전달받은 모든 프로미스가 `모두 fulfilled 상태가 되면 모든 처리 결과를 배열에 저장해 새로운 프로미스를 반환`
  - Promise.all 메서드가 종료하는 데 걸리는 시간 = 가장 늦게 fulfilled 상태가 되는 프로미스의 처리 시간 + a 시간
  - `순서가 보장된다.`
    - 가장 늦게 fulfilled 되는 프로미스가 배열 요소에 앞에 있어도, 순서대로 배열에 저장 후 새로운 프로미스를 반환
- 전달받은 프로미스 요소들 중, `하나라도 rejected 상태가 되면 나머지 프로미스의 진행 상태와 상관없이 기다리지 않고 즉시 종료`

  ```jsx
  const req1 = () =>
    new Promise((_, reject) =>
      setTimeout(() => reject(new Error('Error 1')), 3000)
    );
  const req2 = () =>
    new Promise((_, reject) =>
      setTimeout(() => reject(new Error('Error 2')), 2000)
    );
  const req3 = () =>
    new Promise((_, reject) =>
      setTimeout(() => reject(new Error('Error 3')), 1000)
    );

  Promise.all([req1(), req2(), req3()])
    .then((res) => console.log(res))
    .catch((err) => console.log(err));

  // 약 1초 뒤..
  // Error: Error 3 << 모든 프로미스 작업 중 가장 빠르게 rejected 되는 프로미스의 reject 함수를 호출
  ```

- 전달받은 이터러블의 `요소들이 프로미스가 아닌 값인 경우, Promise.resolve 메서드를 통해 프로미스로 래핑한다.`

  ```jsx
  Promise.all([1, 2, 3])
    .then((res) => console.log(res))
    .catch((err) => console.log);

  // [ 1, 2, 3 ]
  ```

<br />

### Promise.race 정적 메서드

> Promise.all 메서드와 동일하게 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.

- 다만, Promise.all 메서드처럼 모든 프로미스 요소들이 fulfilled 될 때까지 기다리지 않고, `가장 먼저 fulfilled 상태가 되는 프로미스의 처리 결과를 resolve 하는 새로운 프로미스를 반환`

```jsx
const req1 = () => new Promise((resolve) => setTimeout(() => resolve(1), 3000));
const req2 = () => new Promise((resolve) => setTimeout(() => resolve(2), 2000));
const req3 = () => new Promise((resolve) => setTimeout(() => resolve(3), 1000));

Promise.race([req1(), req2(), req3()])
  .then((res) => console.log(res))
  .catch((err) => console.log(err));

// 약 1초 뒤.. ( 가장 먼저 fulfilled 상태가 되는 프로미스 처리 결과를 resolve 함수 호출로 프로미스 반환 )
// 3
// 나머지 약 2초 뒤, 종료 ( 가장 오래 걸리는 프로미스 기준 )
```

<br />

### Promise.allSetteld 정적 메서드

> `ES11`에 도입, 전달받은 프로미스가 모두 `settled 상태(= 비동기 처리가 수행된 상태)가 되면, 처리 결과를 배열로 반환`

- 프로미스가 fulfilled 상태인 경우 → `{ status, value }` 프로퍼티를 갖는다.
- 프로미스가 rejected 상태인 경우 → `{ status, reason }` 프로퍼티를 갖는다.
- 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.

```jsx
const resolvedPromise = new Promise((resolve) =>
  setTimeout(() => resolve(1), 2000)
);
const rejectedPromise = new Promise((_, reject) =>
  setTimeout(() => reject(new Error('Error !'), 1000))
);

Promise.allSettled([resolvedPromise, rejectedPromise]).then((res) =>
  console.log(res)
);
// [
//   { status: 'fulfilled', value: 1 },
//   {
//     status: 'rejected',
//     reason: Error: Error !
//         at Timeout._onTimeout (C:\Users\forze\Desktop\Docs-modernJS__deepDive\37-Promise\index.js:2:76)
//         at listOnTimeout (node:internal/timers:556:17)
//         at processTimers (node:internal/timers:499:7)
//   }
// ]
```

<br />
<br />

# 마이크로태스크 큐

```jsx
setTimeout(() => console.log(1), 0);

Promise.resolve()
  .then(() => console.log(2))
  .then(() => console.log(3));

// 2 3 1
```

- 위에 예제의 결과는 1 → 2 → 3 이 아닌, `2 → 3 → 1 순`으로 출력된다.

> 그 이유는, `프로미스 후속 처리 메서드의 콜백 함수는` 태스크 큐(task queue, = event queue)가 아닌 `마이크로태스크 큐(microtask queue, = job queue)에 저장된다.`

- 마이크로태스크 큐는 태스크 큐와는 브라우저에서 제공하는 `별도의 큐다.`
  - `마이크로태스크 큐(= 잡 큐) → 프로미스 후속 처리 메서드의 콜백 함수가 일시적으로 보관`
  - `태스크 큐(= 이벤트 큐) → 그 외의 비동기 함수의 콜백 함수나 이벤트 핸들러가 일시적으로 보관`
- `우선 순위 = 마이크로태스크 큐 > 태스크 큐`
  - 즉, 이벤트 루프는 콜 스택이 비면 먼저 마이크로태스크 큐에서 대기하고 있는 함수를 가져와 실행
  - 이후 마이크로태스크 큐가 비면 태스크 큐에서 대기하고 있는 함수를 가져와 실행

<br />
<br />

# fetch

> XMLHttpRequest 객체와 마찬가지로 `HTTP 요청 전송 기능을 제공`하는 `클라이언트 사이드 Web API`

- XTMLHttpRequest 객체보다 사용법이 간단하며, 프로미스를 지원하기 때문에 비동기 처리를 위한 콜백 패턴에서 자유롭다.

```jsx
const promise = fetch(url [, options]);
```

> fetch 함수는 HTTP 응답을 나타내는 `Response 객체를 래핑한 Promise 객체를 반환`

```jsx
fetch('https://jsonplaceholder.typicode.com/todos/1').then((res) =>
  console.log(res)
);

// Response
// {
//   type: 'cors',
//   url: 'https://jsonplaceholder.typicode.com/todos/1',
//   redirected: false,
//   status: 200,
//   ok: true,
//   …
// }
```

- Response 객체의 `Response.prototype` 에는 Response 객체에 포함되어 있는 `HTTP 응답 몸체를 위한 다양한 메서드를 제공한다.`
  - `Response.prototpye.json 메서드` : fetch 함수가 반환한 프로미스가 래핑하고 있는 `MIME 타입이 application/json 인 HTTP 응답 몸체를 취득`
    - Response 객체에서 HTTP Response Body를 취득하여 `역직렬화(deserializing)`한다. ( `서버 응답 데이터 획득` )

<br />

### fetch - CRUD 예시

```jsx
// HTTP Request(요청) 모음 객체 생성
const request = {
  get(url) {
    return fetch(url);
  },

  post(url, payload) {
    return fetch(url, {
      method: 'POST',
      headers: { 'content-Type': 'application/json ' },
      body: JSON.stringify(payload),
    });
  },

  patch(url, payload) {
    return fetch(url, {
      method: 'PATCH',
      headers: { 'content-Type': 'application/json ' },
      body: JSON.stringify(payload),
    });
  },

  delete(url) {
    return fetch(url, { method: 'DELETE ' });
  },
};

const url = 'HTTP 통신 요청할 url';

// GET 예시
request
  .get(url)
  .then((res) => res.json())
  .then((data) => console.log(data));

// POST 예시
request
  .post(url, { obj })
  .then((res) => res.json())
  .then((data) => console.log(data))
  .catch((err) => console.error(err));

// PATCH 예시
request
  .patch(url, { obj })
  .then((res) => res.json())
  .then((data) => console.log(data))
  .catch((err) => console.error(err));

// DELETE 예시
request
  .delete(url, { obj })
  .then((res) => res.json())
  .then((data) => console.log(data))
  .catch((err) => console.error(err));
```
