# 36. 디스트럭처링 할당

> 구조화된 배열과 같은 이터러블 또는 객체를 비구조화하여 1개 이상의 변수에 개별적으로 할당하는 것
> → 이터러블 또는 객체 리터럴에서 필요한 값만 추출하여 변수에 할당할 때 유용

<br>

## 36.1 배열 디스트럭처링 할당

\*\* **`ES5 에서의 배열 디스트럭처링 할당`**

```jsx
// ES5
var arr = [1, 2, 3];

var one = arr[0];
var two = arr[1];
var three = arr[2];

console.log(one, two, three); // 1 2 3
```

<br>

\*\* **`ES6 에서의 배열 디스트럭처링 할당`**

```jsx
const arr = [1, 2, 3];

// 변수 one, two, three 를 선언하고 배열 arr 을 디스트럭처링하여 할당
// 이때 할당 기준은 배열의 인덱스
const [one, two, three] = arr;
console.log(one, two, three); // 1 2 3
```

- 배열 디스트럭처링 `할당의 대상 (할당문의 우변) 은 이터러블`이어야 한다.
- 이때 `할당 기준은 배열의 인덱스`, 즉 순서대로 할당된다.
  <br>

```jsx
// 할당 연산자 왼쪽에 값을 할당받을 변수 선언
// 이때 변수는 배열 리터럴 형태로 선언
const [x, y] = [1, 2];

// 우변이 이터러블이 아니라면 에러 발생
const [x, y];   // SyntaxError
const [a, b] = {}   // TypeError

// 선언과 할당 분리 가능
// 단, const 키워드로 변수 선언 X
let x, y;
[x, y] = [1, 2];
```

- `값을 할당받을 변수는 배열 리터럴로 선언`
- 우변이 이터러블이 아니라면 에러 발생
- 배열 디스트럭처링 할당의 변수 선언문은 선언과 할당 분리 가능
  - 단, const 키워드로 변수 선언 X
    <br>

```jsx
// 배열 디스트럭처링 할당 기준 -> 배열의 인덱스
const [a, b] = [1, 2];
console.log(a, b); // 1 2

const [c, d] = [1];
console.log(c, d); // 1 undefined

const [e, f] = [1, 2, 3];
console.log(e, f); // 1 2

const [g, , h] = [1, 2, 3];
console.log(g, h); // 1 3

// 변수에 기본값 설정
const [x, y, z = 3] = [1, 2];
console.log(x, y, z); // 1 2 3

// 기본값보다 할당된 값이 우선
const [x, y = 10, z = 3] = [1, 2];
console.log(x, y, z); // 1 2 3

// 변수에 Rest 요소 사용
const [x, ...y] [1, 2, 3];
console.log(x, y);    // 1 [ 2, 3 ]
```

- 배열 디스트럭처링 할당의 기준은 `배열의 인덱스, 즉 순서대로 할당된다.`
  - 이때 변수의 개수와 이터러블의 요소 개수가 반드시 일치할 필요 X
    <br>
- `할당되는 변수에 기본값 설정 가능`

  - 기본값보다 할당된 값이 우선
    <br>

- `할당되는 변수에 Rest 요소 사용 가능`

  - Rest 파라미터와 마찬가지로 반드시 마지막에 위치해야 함
    <br>

- 배열 디스트럭처링 할당은 `이터러블에서 필요한 요소만 추출하여 변수에 할당하고 싶을 때 유용`
  <br>

## 36.2 객체 디스트럭처링 할당

\*\* **`ES5 에서의 객체 디스트럭처링 할당`**

```jsx
// ES5
var user = { firstName: 'Ungmo', lastName: 'Lee' };

var firstName = user.firstName;
var lastName = user.lastName;

console.log(firstName, lastName); // Ungmo Lee
```

<br>

\*\* **`ES6 에서의 객체 디스트럭처링 할당`**

```jsx
const user = { firstName: 'Ungmo', lastName: 'Lee' };

// 변수 lastName, firstName 을 선언하고 user 객체를 디스트럭처링하여 할당
// 이때 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루지며 순서는 의미 X
const { lastName, firstName } = user;
console.log(firstName, lastName); // Ungmo Lee
```

- 객체 디스트럭처링 `할당의 대상 (할당문의 우변) 은 객체`여야 한다.
  - 이때 `할당 기준은 프로퍼티 키`, 즉 순서는 의미가 없으며 선언된 변수 이름과 프로퍼티 키가 일치하면 할당된다.
    <br>

```jsx
// 변수는 객체 리터럴로 선언
const { lastName, firstName } = { firstName : 'Ungmo', lastName : 'Lee' }

// 우변에 객체 또는 객체로 평가될 수 있는 표현식이 아니라면 에러 발생
const { lastName, firstName };    // SyntaxError
const { lastName, firstName } = null;   // TypeError

// 변수에 기본값 설정
const { firstName = 'Ungmo', lastName } = { lastName : 'Lee' };
console.log(firstName, lastName);   // Ungmo Lee

// 변수에 Rest 프로퍼티 사용
const { x, ...rest } = { x : 1, y : 2, z : 3 };
console.log(x, rest);   // 1, { y : 2, z : 3 }

// 객체의 프로퍼티 키와 다른 변수 이름으로 프로퍼티 값을 할당하는 경우
const user = { firstName : 'Ungmo', lastName : 'Lee' };

// 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어진다.
// 프로퍼티 키가 lastName 인 프로퍼티 값을 ln 에 할당하고,
// 프로퍼티 키가 firstName 인 프로퍼티 값을 fn 에 할당한다.
const { lastName : ln, firstName : fn } = user;
console.log(fn, ln);
```

- 프로퍼티 값을 할당받을 변수는 `객체 리터럴 형태로 선언`해야 한다.
  <br>
- `우변에 객체 또는 객체로 평가될 수 있는 표현식을 할당하지 않으면 에러 발생`
  <br>
- `변수에 기본값 설정 가능`
  <br>
- `변수에 Rest 프로퍼티 사용 가능`
  - Rest 파라미터와 마찬가지로 반드시 마지막에 위치해야 함.
    <br>
- 객체 디스트럭처링 할당은 `프로퍼티 키로 필요한 프로퍼티 값만 추출하여 변수에 할당하고 싶을 때 유용`
  <br>

```jsx
const str = 'Hello';
// String 래퍼 객체로부터 length 프로퍼티만 추출
const { length } = str;
console.log(length); // 5

const todo = { id: 1, content: HTML, completed: true };

// todo 객체로부터 id 프로퍼티만 추출
const { id } = todo;
console.log(id); // 1

// 함수 매개변수에 객체 디스트럭처링 할당 사용
function printTodo({ content, completed }) {
  console.log(`할일 ${content}은 ${completed ? '완료' : '비완료'} 상태입니다.`);
}

console.log({ id: 1, content: 'HTML', completed: true }); // 할일 HTML은 완료 상태입니다.
```

- 객체 디스트럭처링 할당은 인수로 객체를 전달받는 함수 매개변수에도 사용 가능
  <br>

```jsx
// 중첩 객체인 경우
const user = {
  name: 'Lee',
  address: {
    zipCod: '123',
    city: 'Seoul',
  },
};

// address 프로퍼티 키로 객체를 추출하고, 이 객체의 city 프로퍼티 키로 값을 추출
const {
  address: { city },
} = user;
console.log(city); // 'Seoul'
```

# 37. Set과 Map

## 37.1 Set

- Set 객체는 `중복되지 않는 유일한 값들의 집합`
- Set 객체의 특성은 수학접 집합의 특성과 일치하며, 교집합/합집합/차집합/여집합 등 구현 가능
- 배열과 유사하지만 다음과 같은 차이가 있음.
  <br>

| 구분                                 | 배열 | Set 객체 |
| ------------------------------------ | ---- | -------- |
| 동일한 값을 중복하여 포함할 수 있다. | O    | X        |
| 요소 순서에 의미가 있다.             | O    | X        |
| 인덱스로 요소에 접근할 수 있다.      | O    | X        |

<br>

#### 1) Set 객체의 생성

> Set 객체는 **Set 생성자 함수로 생성**

<br>

```jsx
const set = new Set();
console.log(set); // Set(0) {}
```

```jsx
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set('hello');
console.log(set2); // Set(4) {'h', 'e', 'l', 'o'}
```

- Set 생성자 함수에 인수를 전달하지 않으면 빈 Set 객체를 생성
  - Set 생성자 함수는 `이터러블을 인수로 전달받아 Set 객체 생성`
  - `이때 이터러블의 중복된 값은 Set 객체에 요소로 저장 X`

<br>

```js
// 배열의 중복 요소 제거
const uniq = (array) => array.filter((v, i, self) => self.indexOf(v) === i);

// Set을 사용한 배열의 중복 요소 제거
const uniq = (array) => [...new Set(array)];
```

- `중복을 허용하지 않는 Set 객체의 특성을 활용하여 배열에서 중복된 요소를 제거`

<br>

#### 2) 요소 개수 확인

> Set 객체의 요소 개수를 확인할 때는 **Set.prototype.size** 프로퍼티 사용

<br>

```jsx
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3
```

```jsx
const set = new Set([1, 2, 3]);

console.log(Object.getOwnPropertyDescriptor(Set.prototype, 'size'));
// {set : undefined, enumerable : false, configurable : true, get : f}

set.size = 10; // 무시된다.
console.log(set.size); // 3
```

- size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티
  - `size 프로퍼티에 생성자를 할당하여 Set 객체의 요소 개수 변경 X`

<br>

#### 3) 요소 추가

> Set 객체에 요소를 추가할 때는 **Set.prototype.add** 메서드 사용

<br>

```jsx
const set = new Set();
console.log(set); // Set(0) {}

set.add(1);
console.log(set); // Set(1) {1}
```

```jsx
const set = new Set();

set.add(1).add(2).add(2);
console.log(set); // Set(2) {1, 2}
```

- add 메서드는 새로운 요소가 추가된 Set 객체를 반환

  - add 메서드를 호출한 후에 `add 메서드를 연속적으로 호출하는 것도 가능`
  - 중복된 요소의 추가는 허용 X => 에러 발생없이 무시됨.
    <br>

```js
const set = new Set();

set
  .add(1)
  .add('a')
  .add(true)
  .add(undefined)
  .add(null)
  .add({})
  .add([])
  .add(() => {});

console.log(set); // Set(8) {1, "a", true, undefined, null, {}, [], () => {}}
```

- `Set 객체는 자바스크립트의 모든 값을 요소로 저장 가능`

<br>

#### 4) 요소 존재 여부 확인

> Set 객체에 특정 요소가 존재하는지 확인하려면 **Set.prototype.has** 메서드 사용

<br>

```jsx
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

- `has 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값 반환`

<br>

#### 5) 요소 삭제

> Set 객체의 특정 요소를 삭제하려면 **Set.prototype.delete** 메서드 사용

<br>

```jsx
const set = new Set([1, 2, 3]);

// 요소 2를 삭제
set.delete(2);
console.log(set); // Set(2) {1, 3}

set.delete(0);
console.log(set); // Set(2) {1, 3}

set.delete(1).delete(3); // TypeError
```

- `delete 메서드는 삭제 성공 여부를 나타내는 불리언 값 반환`

  - add 메서드와 달리 `연속적으로 호출 X`
    <br>

- 인덱스가 아닌 삭제하려는 `요소값 자체를 인수로 전달`해야 함.
  - 존재하지 않는 Set 객체의 요소를 삭제하려 하면 에러 없이 무시됨.
    <br>

#### 6) 요소 일괄 삭제

> Set 객체의 모든 요소를 일괄 삭제하려면 **Set.prototype.clear** 메서드 사용

<br>

```jsx
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}
```

- `clear 메서드는 언제나 undefined 반환`

<br>

#### 7) 요소 순회

> Set 객체의 요소를 순회하려면 **Set.prototype.forEach** 메서드 사용

<br>

```jsx
const set = new Set([1, 2, 3]);

set.forEach((v1, v2, set) => console.log(v1, v2, set));
/*
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
*/
```

- forEach 메서드에는 다음과 같이 3개의 인수를 전달
  - 첫 번째 인수 : `현재 순회 중인 요소값`
  - 두 번째 인수 : `현재 순회 중인 요소값`
  - 세 번째 인수 : `현재 순회 중인 Set 객체 자체`

<br>

```jsx
const set = new Set([1, 2, 3]);

// Set 객체는 Set.prototype 의 Symbol.iterator 메서드를 상속받는 이터러블
console.log(Symbol.iterator in set); // true

// 이터러블인 Set 객체는 for... of 문으로 순회 가능
for (const value of set) {
  console.log(value); // 1 2 3
}

// 이터러블인 Set 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...set]); // [1, 2, 3]

// 이터러블인 Set 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, ...rest] = set;
console.log(a, rest); // 1, [2, 3]
```

- Set 객체는 이터러블이기 때문에 for... of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있음.
  <br>

- Set 객체는 요소의 순서에 의미를 갖지 않지만 `Set 객체를 순회하는 순서는 요소가 추가된 순서를 따름.`
  - 다른 이터러블의 순회와 호환성을 유지하기 위함

<br>

#### 8) 집합 연산

> Set 객체를 통해 교집합, 합집합, 차집합 등을 구현 가능

<br>

##### 1) 교집합

> 집합 A 와 집합 B 의 공통 요소로 구성

<br>

```js
// 1번 방법
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    // 2개의 set의 요소가 공통되는 요소이면 교집합의 대상
    if (this.has(value)) result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
```

```jsx
// 2번 방법
Set.prototype.intersection = function (set) {
  return new Set([...this].filter(v = > set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA 와 setB 의 교집합
console.log(setA.intersection(setB));   // Set(2) {2, 4}
```

<br>

##### 2) 합집합

> 집합 A 와 집합 B 의 중복 없는 모든 요소로 구성

<br>

```js
// 1번 방법
Set.prototype.union = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
```

```jsx
// 2번 방법
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA 와 setB 의 합집합
console.log(setA.union(setB)); // Set(2) {1, 2, 3, 4}
```

<br>

##### 3) 차집합

> A - B 는 집합 A 에는 존재하지만 집합 B 에는 존재하지 않는 요소로 구성

<br>

```js
// 1번 방법
Set.prototype.difference = function (set) {
  const result = new Set(this);

  for (const value of set) {
    result.delete(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

console.log(setA.difference(setB)); // Set(2) {1, 3}
console.log(setB.difference(setA)); // Set(0) {}
```

```jsx
// 2번 방법
Set.prototype.difference = function (set) {
  return new Set([...this].filter((v) => !set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA 에 대한 setB 의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}

// setB 에 대한 setA 의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

<br>

##### 4) 부분 집합과 상위 집합

> 집합 A 가 집합 B 에 포함되는 경우 집합 A 는 집합 B 의 부분 집합이며, 집합 B 는 집합 A 의 상위 집합

<br>

```js
// 1번 방법
// this가 subset의 상위 집합인지 확인
Set.prototype.isSuperset = function (subset) {
  for (const value of subset) {
    // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
    if (!this.has(value)) return false;
  }
  return true;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인
console.log(setA.isSuperset(setB)); // true
```

```jsx
// 2번 방법
// this 가 subset 의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  const supersetArr = [...this];
  return [...subset].every((v) => supersetArr.includes(v));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA 가 setB 의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true

// setB 가 setA 의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

<br>

## 37.2 Map

- Map 객체는 `키와 값의 쌍으로 이루어진 컬렉션`
- 객체와 유사하지만 다음과 같은 차이가 있음.
  <br>

| 구분                   | 객체                    | Map 객체              |
| ---------------------- | ----------------------- | --------------------- |
| 키로 사용할 수 있는 값 | 문자열 또는 심벌 값     | 객체를 포함한 모든 값 |
| 이터러블               | X                       | O                     |
| 요소 개수 확인         | Object.keys(obj).length | map.size              |

<br>

#### 1) Map 객체의 생성

> Map 객체는 **Map 생성자 함수로 생성**

<br>

```jsx
const map = new Map();
console.log(map); // Map(0) {}
```

```jsx
const map1 = new Map([
  ['key1', 'value1'],
  ['key2', 'value2'],
]);
console.log(map1); // Map(2) {'key1' => 'value1', 'key2' => 'value2'}

const map2 = new Map([1, 2]); // TypeError
```

- Map 생성자 함수에 인수를 전달하지 않으면 빈 Map 객체를 생성
  - Map 생성자 함수는 `이터러블을 인수로 전달받아 Map 객체 생성`
  - `이때 이터러블은 키와 값의 쌍으로 이루어진 요소`

<br>

```jsx
const map1 = new Map([
  ['key1', 'value1'],
  ['key1', 'value2'],
]);
console.log(map); // Map(1) {'key1' => 'value1'}
```

- `Map 객체에는 중복된 키를 갖는 요소 X`
  - 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 덮어씌워짐.
    <br>

#### 2) 요소 개수 확인

> Map 객체의 요소 개수를 확인할 때는 **Map.prototype.size** 프로퍼티 사용

<br>

```jsx
const { size } = new Map([
  ['key1', 'value1'],
  ['key2', 'value2'],
]);

console.log(size); // 2
```

```jsx
const map = new Map([
  ['key1', 'value1'],
  ['key2', 'value2'],
]);

console.log(Object.getOwnPropertyDescriptor(Map.prototype, 'size'));
// {set : undefined, enumerable : false, configurable : true, get : f}

map.size = 10; // 무시된다.
console.log(map.size); // 2
```

- size 프로퍼티는 setter 함수 없이 getter 함수만 존재하는 접근자 프로퍼티
  - `size 프로퍼티에 생성자를 할당하여 Map 객체의 요소 개수 변경 X`

<br>

#### 3) 요소 추가

> Map 객체에 요소를 추가할 때는 **Map.prototype.set** 메서드 사용

<br>

```jsx
const map = new Map();
console.log(map); // Map(0) {}

map.set('key1', 'value1').set('key2', 'value2');

console.log(map); // Map(2) {'key1' => 'value1', 'key2' => 'value2'}
```

- set 메서드는 새로운 요소가 추가된 Map 객체를 반환

  - set 메서드를 호출한 후에 `set 메서드를 연속적으로 호출하는 것도 가능`
  - 중복된 키를 갖는 요소의 추가는 허용 X => 에러 발생없이 무시됨.
    <br>

```jsx
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

// 객체도 키로 사용 가능
map.set(lee, 'developer').set(kim, 'designer');

console.log(map); // Map(2) { {name : "Lee"} => 'developer', {name : "Kim"} => 'designer' }
```

- `Map 객체는 객체를 포함한 모든 값을 키로 사용 가능`

<br>

#### 4) 요소 취득

> Map 객체에서 특정 요소를 취득하려면 **Map.prototype.get** 메서드 사용

<br>

```jsx
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map.set(lee, 'developer').set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get('key')); // undefined
```

- get 메서드의 인수로 키를 전달하면 `Map 객체에서 인수로 전달한 키를 갖는 값을 반환`
  - Map 객체에서 인수로 전달한 키를 갖는 요소가 존재하지 않으면 undefined 반환

<br>

#### 5) 요소 존재 여부 확인

> Map 객체에서 특정 요소가 존재하는지 확인하려면 **Map.prototype.has** 메서드 사용

<br>

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
  [lee, 'developer'],
  [kim, 'designer'],
]);

console.log(map.has(lee)); // true
console.log(map.has('key')); // false
```

- `has 메서드는 특정 요소의 존재 여부를 나타내는 불리언 값 반환`

<br>

#### 6) 요소 삭제

> Map 객체의 요소를 삭제하려면 **Map.prototype.delete** 메서드 사용

<br>

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
  [lee, 'developer'],
  [kim, 'designer'],
]);

map.delete(kim);
console.log(map); // Map(1) { {name : "Lee"} => 'developer'}

map.delete('');
console.log(map); // Map(1) { {name : "Lee"} => 'developer'}
```

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
  [lee, 'developer'],
  [kim, 'designer'],
]);

map.delete(lee).delete(kim); // TypeError
```

- `delete 메서드는 삭제 성공 여부를 나타내는 불리언 값 반환`
  - set 메서드와 달리 `연속적으로 호출 X`
  - 존재하지 않는 키로 Map 객체의 요소를 삭제하려 하면 에러 없이 무시됨.

<br>

#### 7) 요소 일괄 삭제

> Map 객체의 모든 요소를 일괄 삭제하려면 **Set.prototype.clear** 메서드 사용

<br>

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
  [lee, 'developer'],
  [kim, 'designer'],
]);

map.clear();
console.log(map); // Map(0) {}
```

- `clear 메서드는 언제나 undefined 반환`

<br>

#### 7) 요소 순회

> Map 객체의 요소를 순회하려면 **Map.prototype.forEach** 메서드 사용

<br>

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
  [lee, 'developer'],
  [kim, 'designer'],
]);

map.forEach((v, k, map) => console.log(v, k, map));
/*
developer {name : "Lee"} Map(2) {
  {name : "Lee"} => "developer"
  {name : "Kim"} => "designer"
}
designer {name : "Kim"} Map(2) {
  {name : "Lee"} => "developer"
  {name : "Kim"} => "designer"
}
*/
```

- forEach 메서드에는 다음과 같이 3개의 인수를 전달
  - 첫 번째 인수 : `현재 순회 중인 요소값`
  - 두 번째 인수 : `현재 순회 중인 요소키`
  - 세 번째 인수 : `현재 순회 중인 Map 객체 자체`

<br>

```jsx
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
  [lee, 'developer'],
  [kim, 'designer'],
]);

// Map 객체는 Map.prototype 의 Symbol.iterator 메서드를 상속받는 이터러블
console.log(Symbol.iterator in map); // true

// 이터러블인 Map 객체는 for... of 문으로 순회 가능
for (const value of map) {
  console.log(value); // [{name : "Lee"}, "developer"] [{name : "Kim"}, "designer"]
}

// 이터러블인 Map 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...map]); // [[{name : "Lee"}, "developer"], [{name : "Kim"}, "designer"]]

// 이터러블인 Map 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, ...rest] = map;
console.log(a, rest); // [{name : "Lee"}, "developer"] [{name : "Kim"}, "designer"]
```

- Map 객체는 이터러블이기 때문에 for... of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있음.

<br>

\*\* **Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공**

| Map 메서드            | 설명                                                                                 |
| --------------------- | ------------------------------------------------------------------------------------ |
| Map.prototype.keys    | Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환     |
| Map.prototype.values  | Map 객체에서 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환     |
| Map.prototype.entries | Map 객체에서 요소키와 요소값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환 |

<br>

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([
  [lee, 'developer'],
  [kim, 'designer'],
]);

// Map.prototype.keys는 Map 객체에서 요소키를 값으로 갖는 이터레이터를 반환
for (const key of map.keys()) {
  console.log(key); // {name: "Lee"} {name: "Kim"}
}

// Map.prototype.values는 Map 객체에서 요소값을 값으로 갖는 이터레이터를 반환
for (const value of map.values()) {
  console.log(value); // developer designer
}

// Map.prototype.entires 는 Map 객체에서 요소키와 요소값을 값으로 갖는 이터레이터 반환
for (const entry of map.entries()) {
  console.log(entry); // [{name: "Lee"}, "developer"] [{name: "Kim"}, "designer"]
}
```

- Map 객체는 요소의 순서에 의미를 갖지 않지만 `Map 객체를 순회하는 순서는 요소가 추가된 순서를 따름.`
  - 다른 이터러블의 순회와 호환성을 유지하기 위함

# 38. 브라우저의 렌더링 과정

- **`파싱`**
  - 파싱 (구문 분석) 은 프로그래밍 언어의 문법에 맞게 작성된 텍스트 문서를 읽어 들여 실행하기 위해 텍스트 문서의 문자열을 토큰으로 분해하고, 토큰에 문법적 의미와 구조를 반영하여 트리 구조의 자료구조인 파스 트리를 생성하는 일련의 과정

<br>

- **`렌더링`**
  - 렌더링은 HTML, CSS, 자바스크립트로 작성된 문서를 파싱하여 브라우저에 시각적으로 출력하는 것

<br>

1. 브라우저는 HTML, CSS, 자바스크립트, 이미지, 폰트 파일 등 **`렌더링에 필요한 리소스를 요청하고 서버로부터 응답을 받는다.`**
   <br>

2. 브라우저의 렌더링 엔진은 서버로부터 응답된 **`HTML 과 CSS 를 파싱하여 DOM 과 CSSOM 을 생성하고 이들을 결합하여 렌더 트리를 생성한다.`**
   <br>

3. 브라우저의 자바스크립트 엔진은 서버로부터 응답된 **`자바스크립트를 파싱하여 AST 를 생성하고 바이트코드로 변환하여 실행한다.`** 이때 자바스크립트는 DOM API 를 통해 DOM 이나 CSSOM 을 변경할 수 있다. 변경된 DOM 과 CSSOM 은 다시 렌더 트리로 결합된다.
   <br>

4. **`렌더 트리를 기반으로 HTML 요소의 레이아웃 (위치와 크기) 을 계산하고 브라우저 화면에 HTML 요소를 페인팅한다.`**

<br>

## 38.1 요청과 응답

- 렌더링에 필요한 리소스는 모두 서버에 존재하므로 필요한 리소스를 서버에 요청하고 서버가 응답한 리소스를 파싱하여 렌더링하는 것 => `브라우저의 핵심 기능`

<br>

\*\* **`리소스란?`**
→ HTML, CSS, 자바스크립트, 이미지, 폰트 등의 정적 파일 또는 서버가 동적으로 생성한 데이터를 의미

<br>

- **서버에 요청을 전송하기 위해 브라우저는 주소창을 제공**

  - 브라우저의 주소창에 URL 을 입력하고 엔터 키를 누르면 URL 의 호스트 이름이 DNS 를 통해 IP 주소로 변환되고, 이 IP 주소를 갖는 서버에게 요청을 전송함.
  - `요청할 정적 파일의 경로와 파일 이름을 URI 의 호스트 뒤의 path 에 기술하여 서버에 요청`
    <br>

- **요청과 응답은 개발자 도구의 Network 패널에서 확인 가능**
  - html 뿐만 아니라 CSS, 자바스크립트, 이미지, 폰트 파일들도 응답된 것을 알 수 있음.
  - `브라우저의 렌더링 엔진이 HTML 을 파싱하는 도중에 외부 리소스를 로드하는 태그를 만나면 HTML 의 파싱을 일시 중단하고 해당 리소스 파일을 서버로 요청하기 때문`
    <br>

\*\* **`외부 리소스를 로드하는 태그란?`**

- CSS 파일을 로드하는 link 태그
- 이미지 파일을 로드하는 img 태그
- 자바스크립트를 로드하는 script 태그 등

<br>

## 38.2 HTTP 1.1과 HTTP 2.0

> HTTP 는 웹에서 브라우저와 서버가 통신하기 위한 프로토콜 (규약)

<br>

#### \*\* `HTTP/1.1 과 HTTP/2 의 차이점`

<br>

- **HTTP/1.1**

  - `커넥션당 하나의 요청과 응답만 처리`
  - 리소스의 동시 전송이 불가능하므로 요청할 리소스의 개수에 비례하여 응답 시간 ↑
    <br>

- **HTTP/2**
  - `커넥션당 여러 개의 요청과 응답이 가능`
  - 여러 리소스의 동시 전송이 가능하므로 페이지 로드 속도 ↑
  - HTTP/1.1 에 비해 페이지 로드 속도가 약 50% 빠르다고 알려짐
    <br>

## 38.3 HTML 파싱과 DOM 생성

- 브라우저의 요청에 의해 서버가 응답한 `HTML 문서는 문자열로 이루어진 순수한 텍스트`
- 순수한 텍스트인 HTML 문서를 브라우저에 시각적인 픽셀로 렌더링하려면 `HTML 문서를 브라우저가 이해할 수 있는 자료구조 (객체) 로 변환하여 메모리에 저장해야 함.`

<br>

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script src="app.js"></script>
  </body>
</html>
```

<br>

- 브라우저의 렌더링 엔진은 아래 그림과 같이 응답받은 **HTML 문서를 파싱하여 브라우저가 이해할 수 있는 자료구조인 DOM (Document Object Model) 생성**
  - `DOM` : HTML 문서를 파싱한 결과물

<br>

1. 서버에 존재하던 HTML 파일이 브라우저의 요청에 의해 응답된다. 이때 **서버는 브라우저가 요청한 HTML 파일을 읽어 들여 메모리에 저장한 다음 메모리에 저장된 `바이트 (2진수)` 를 인터넷을 경유하여 응답한다.**
   <br>

2. 브라우저는 서버가 응답한 HTML 문서를 바이트 (2진수) 형태로 응답받는다.그리고 **응답된 바이트 형태의 HTML 문서는 meta 태그의 charset 어트리뷰트에 의해 지정된 인코딩 방식을 기준으로 `문자열`로 변환된다.**
   <br>

3. **문자열로 변환된 HTML 문서를 읽어 들여 문법적 의미를 갖는 코드의 최소 단위인 `토큰`들로 분해한다.**
   <br>

4. **각 토큰들을 객체로 변환하여 `노드`들을 생성한다.** 토큰의 내용에 따라 문서 노드, 요소 노드, 어트리뷰트 노드, 텍스트 노드가 생성된다. 노드는 이후 DOM 을 구성하는 기본 요소가 된다.
   <br>

5. HTML 요소의 콘텐츠 영역에는 텍스트뿐만 아니라 다른 HTML 요소도 포함될 수 있어 HTML 문서는 중첩 관계를 갖는다. 이때 HTML 요소 간에는 중첩 관계에 의해 부자 관계가 형성되고, 이러한 **HTML 요소 간의 부자 관계를 반영하여 모든 노드들을 트리 자료구조로 구성한다. 이 노드들로 구성된 트리 자료구조를 `DOM` 이라 부른다.**
   <br>

## 38.4 CSS 파싱과 CSSOM 생성

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <link rel="stylesheet" href="style.css" />
  </head>
</html>
```

- 렌더링 엔진은 `HTML 을 처음부터 한 줄씩 순차적으로 파싱하여 DOM 을 생성해나감.`
  <br>

- `DOM 을 생성해나가다가 CSS 를 로드하는 link 태그나 style 태그를 만나면 DOM 생성을 일시 중단`
  <br>

- link 태그의 href 어트리뷰트에 지정된 CSS 파일을 서버에 요청하여 로드한 CSS 파일이나 style 태그 내의 `CSS 를 HTML 과 동일한 파싱 과정 (바이트 → 문자 → 토큰 → 노드 → CSSOM) 을 거쳐 CSSOM 을 생성`

  - ul-li 태그의 상속 관계가 반영되어 아래 그림과 같은 CSSOM 이 생성됨.
    <br>

- `CSS 파싱을 완료하면 HTML 파싱이 중단된 지점부터 다시 HTML 을 파싱하기 시작하여 DOM 생성 재개`
  <br>

## 38.5 렌더 트리 생성

- 렌더링 엔진은 서버로부터 응답된 HTML 과 CSS 를 파싱하여 각각 DOM 과 CSSOM 을 생성
- **`DOM 과 CSSOM 은 렌더링을 위해 렌더 트리로 결합됨.`**
  <br>

#### \*\* **렌더 트리란?**

- **렌더링을 위한 트리 구조의 자료구조**
- **브라우저 화면에 렌더링되는 노드만으로 구성**
  - meta 태그, script 태그 등과 같은 브라우저 화면에 렌더링되지 않는 노드들은 포함 X
  - CSS 에 의해 비표시 (ex. display:none) 되는 노드들도 포함 X
    <br>

<br>

- 이후 완성된 렌더 트리는 각 HTML 요소의 레이아웃 (위치와 크기) 을 계산하는 데 사용되며, 브라우저 화면에 픽셀을 렌더링하는 페인팅 처리에 입력됨.

<br>

- 다음과 같은 경우 반복해서 레이아웃 계산과 페인팅이 실행됨.

  - 자바스크립트에 의한 노드 추가 또는 삭제
  - 브라우저 창의 리사이징에 의한 뷰포트 크기 변경
  - HTML 요소의 레이아웃(위치, 크기)에 변경을 발생시키는 width/height, margin, padding, border, display, top/right/bottom/left 등의 스타일 변경

👉 리렌더링은 성능에 악영향을 주는 작업이기 때문에 빈번하게 발생하지 않도록 주의할 필요가 있음.

<br>

#### \*\* **`렌더 트리와 레이아웃/페인트`**

<br>

## 38.6 자바스크립트 파싱과 실행

- **`DOM`** 은 HTML 문서의 구조와 정보뿐만 아니라 HTML 요소와 스타일 등을 변경할 수 있는 프로그래밍 인터페이스로서 **`DOM API`** 를 제공

  - DOM API 를 사용하면 이미 생성된 DOM 을 동적으로 조작 가능
    <br>

- **`자바스크립트 파싱과 실행`** 은 브라우저의 렌더링 엔진이 아닌 **자바스크립트 엔진이 처리**
  - 자바스크립트 엔진은 `자바스크립트 코드를 파싱하여 CPU 가 이해할 수 있는 저수준 언어로 변환하고 실행하는 역할`
    <br>
- 자바스크립트 코드를 해석하여 **`AST (추상적 구문 트리)`** 를 생성하고, AST 를 기반으로 인터프리터가 실행할 수 있는 중간 코드인 바이트코드를 생성하여 실행함.
  <br>

#### \*\* **`DOM 생성 도중에 자바스크립트 파싱이 일어나는 과정`**

1. 렌더링 엔진은 HTML 을 한 줄씩 순차적으로 파싱하며 DOM 을 생성해 나가다가 자바스크립트 파일을 로드하는 script 태그나 자바스크립트 코드를 콘텐츠로 담은 script 태그를 만나면 DOM 생성을 일시 중단
   <br>

2. script 태그의 src 어트리뷰트에 정의된 자바스크립트 파일을 서버에 요청하여 로드한 자바스크립트 파일이나 script 태그 내의 자바스크립트 코드를 파싱하기 위해 자바스크립트 엔진에 제어권을 넘김.
   <br>

3. 이후 자바스크립트 파싱과 실행이 종료되면 렌더링 엔진으로 다시 제어권을 넘겨 HTML 파싱이 중단된 지점부터 다시 HTML 파싱을 시작하여 DOM 생성 재개
   <br>

#### \*\* **`자바스크립트 파싱과 실행 단계`**

![img](https://devcecy.com/wp-content/uploads/2021/08/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA-2021-08-08-%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE-2.05.43.png)
<br>

#### 1. 토크나이징

- 단순한 문자열인 자바스크립트 소스코드를 어휘 분석하여 **`문법적 의미를 갖는 코드의 최소 단위인 토큰들로 분해`**
  - 이 과정을 '렉싱' 이라고 부르기도 하지만 토크나이징과 미묘한 차이 O
    <br>

#### 2. 파싱

- **`토큰들의 집합을 구문 분석하여 AST (추상적 구문 트리) 를 생성`**
  - AST 는 토큰에 문법적 의미와 구조를 반영한 트리 구조의 자료구조
    <br>

#### 3. 바이트코드 생성과 실행

- 파싱의 결과물로서 생성된 **`AST 는 인터프리터가 실행할 수 있는 중간 코드인 바이트코드로 변환되고, 인터프리터에 의해 실행됨.`**
  <br>

## 38.7 리플로우와 리페인트

- **자바스크립트 코드에 DOM 이나 CSSOM 을 변경하는 DOM API 가 사용된 경우 DOM 이나 CSSOM 이 변경됨.**
  - 이때 변경된 DOM 과 CSSOM 은 다시 렌더 트리로 결합되고, 변경된 렌더 트리를 기반으로 레이아웃과 페인트 과정을 거쳐 브라우저의 화면에 다시 렌더링함. => **`리플로우`** / **`리페인트`**
    <br>

<br>

##### \*\* **`리플로우란?`**

- 레이아웃 계산을 다시 하는 것
- 노드 추가/삭제, 요소의 크기/위치 변경, 윈도우 리사이징 등 레이아웃에 영향을 주는 변경이 발생한 경우에 한하여 실행됨.
  <br>

##### \*\* **`리페인트란?`**

- 재결합된 렌더 트리를 기반으로 다시 페인트하는 것
- 레이아웃에 영향이 없는 변경은 리플로우 없이 리페인트만 실행됨.

<br>

## 38.8 자바스크립트 파싱에 의한 HTML 파싱 중단

- 지금까지 살펴본 바로는 렌더링 엔진과 자바스크립트 엔진은 병렬적으로 파싱을 실행하지 않고 직렬적으로 수행
- `브라우저는 동기적으로, 즉 위에서 아래 방향으로 순차적으로 HTML, CSS, 자바스크립트를 파싱하고 실행`
- script 태그의 위치에 따라 HTML 파싱이 블로킹되어 DOM 생성이 지연될 수 있다는 것을 의미
- **`script 태그의 위치는 body 요소의 가장 아래에 위치시키는 것이 좋음.`**
  - DOM 이 완성되지 않은 상태에서 자바스크립트가 DOM 을 조작하면 에러 발생 가능성 ↑
  - 자바스크립트 로딩/파싱/실행으로 인해 HTML 요소들의 렌더링에 지장받는 일이 발생하지 않아 페이지 로딩 시간 ↓
    <br>

## 38.9 script 태그의 async/defer 어트리뷰트

- **async와 defer 어트리뷰트를 사용하면 HTML 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행됨.**
- 단, async 와 defer 어트리뷰트는 src 어트리뷰트를 통해 **외부 자바스크립트 파일을 로드하는 경우에만 사용 가능**
  - src 어트리뷰트가 없는 인라인 자바스크립트에는 사용 X
    <br>

```html
<script async src="extern.js"></script>
<script defer src="extern.js"></script>
```

- async 와 defer 어트리뷰트를 사용하면 **`HTML 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행`** 되지만, **`자바스크립트의 실행 시점에 차이`** 가 있음.
  <br>

#### \*\* **`async 어트리뷰트`**

- async 어트리뷰트를 사용하면 HTML 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행됨.
- **`단, 자바스크립트의 파싱과 실행은 자바스크립트 파일의 로드가 완료된 직후 진행되며, 이때 HTML 파싱이 중단됨.`**
- `async 어트리뷰트는 순서 보장이 필요없는 script 태그에 유용`
  - script 태그의 순서와는 상관없이 로드가 완료된 자바스크립트부터 먼저 실행되므로 순서가 보장되지 않기 때문
    <br>

#### \*\* **`defer 어트리뷰트`**

<br>

- defer 어트리뷰트를 사용하면 HTML 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행됨.
- **`단, 자바스크립트의 파싱과 실행은 HTML 파싱이 완료된 직후, 즉 DOM 생성이 완료된 직후 진행됨.`**
- `defer 어트리뷰트는 DOM 생성이 완료된 이후 실행되어야 할 script 태그에 유용`
