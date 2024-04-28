# 39장 DOM

## 39.1 노드<a name="39.1"></a>

- DOM은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 프로퍼티와 메서드를 제공하는 트리 자료구조

### 39.1.1 HTML 요소와 노드 객체

- HTML 문서를 구성하는 개별적인 요소 의미

- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환됨

- HTML 요소의 어트리뷰트는 어트리뷰트 노드로，HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환

- HTML 문서는 HTML 요소들의 집합으로 이뤄지며, HTML 요소는 중첩 관계를 가짐

- HTML 요소의 콘텐츠영역(시작 태그와 종료 태그 사이)에는 텍스트뿐만 아니라 다른 HTML 요소도 포함 가능

- HTML 요소 간에는 중첩 관계에 의해 계층적인 부자 관계 형성

- HTML 요소 간의 부자 관계를 반영하여 HTML 문서의 구성 요소인 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료 구조로 구성함

#### 트리 자료구조

- 노드들의 계층 구조로 이루어짐

- 부모 노드와 자식 노드로 구성되어 노드 간의 계층적 구조를 표현하는 비선형 자료구조를 말함

- 트리 자료구조는 하나의 최상위 노드에서 시작

- 최상위 노드는 부모 노드가 없으며 루트 노드라고 하며, 루트 노드는 0개 이상의 자식 노드를 가짐

- 자식 노드가 없는 노드는 리프 노드라고 함

- 노드 객체들로 구성된 트리 자료 구조를 DOM이라 하며, 노드 객체의 트리로 구조화되어 있기 때문에 DOM을 DOM 트리라고 부름

### 39.1.2 노드 객체의 타입

- DOM은 노드 객체의 계층적인 구조로 구성되며, 노드 객체는 종류가 있고 상속 구조를 가짐

- 노드 객체는 총 12개의 종류가 있으며, 중요한 노드 타입은 4가지임

  - 문서 노드: DOM 트리의 최상위에 존재하는 루트 노드로서 document 객체를 가리킴
  - 요소 노드: HTML 요소를 가리키는 객체
  - 어트리뷰트 노드: HTML 요소의 어트리뷰트를 가리키는 객체
  - 텍스트 노드: HTML 요소의 텍스트를 가리키는 객체

- 4가지 노드 타입 외에도 주석을 위한 Comment 노드, DOCTYPE을 위한 DocumentType 노드, 복수의 노드를 생성하여 추가할 때 사용하는 DocumentFragment 노드 등 존재

### 39.1.3 노드 객체의 상속 구조

- DOM을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API 사용 가능

- 노드 객체는 자신의 부모, 형제, 자식을 탐색할 수 있으며, 자신의 어트리뷰트와 텍스트를 조작할 수 있음

- DOM을 구성하는 노드 객체는 ECMAScript 사양에 정의된 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체임

- 노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 가짐

- 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속받음

- 배열이 객체인 동시에 배열인 것처럼 input 요소 노드 객체도 다음과 같이 다양한 특성을 갖는 객체이며, 상속을 통해 기능들을 제공받음

- DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작 가능

## 39.2 요소 노드 취득<a name="39.2"></a>

- HTML의 구조나 내용 또는 스타일 등을 동적으로 조작하려면 먼저 요소 노드를 취득해야 함

### 39.2.1 id를 이용한 요소 노드 취득

- `Document.prototype.getElementByld` 메서드는 인수로 전달한 id 어트리뷰트 값을 갖는 하나의 요소 노드를 탐색해 반환함

### 39.2.2 태그 이름을 이용한 요소 노드 취득

- `Document.prototype/Element.prototype.getElementsByTagName` 메서드는 인수로 전달한 태그 이름을 갖는 모든 요소 노드들을 탐색하여 반환함

### 39.2.3 class를 이용한 요소 노드 취득

- `Document.prototype/Element.prototype.getElementsByClassName` 메서드는 인수로 전달한 class 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색하여 반환함

### 39.2.4 CSS 선택자를 이용한 요소 노드 취득

- CSS 선택자는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법

- `Document.prototype/Element.prototype.queryselector` 메서드는 인수로 전달한 CSS 선택자를 만족시키는 하나의 요소 노드를 탐색하여 반환함

### 39.2.5 특정 요소 노드를 취득할 수 있는지 확인

- `Element.prototype.matches` 메서드는 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득할 수 있는지 확인함

### 39.2.6 HTMLCollection과 NodeList

- HTMLCollection과 NodeList 모두 유사 배열 객체이면서 이터러블

- `for ... of`문으로 순회할 수 있으며 스프레드 문법을 사용하여 간단한 배열로 변환 가능

#### HTMLCollection

- `getElementsByTagName`, `getElementsByClassName` 메서드가 반환하는 HTMLCollection 객체는 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는 DOM 컬렉션 객체

#### NodeList

- 대부분의 경우 노드 객체의 상태 변경을 실시간으로 반영하지 않고 과거의 정적 상태를 유지하는 non-live 객체로 동작

- 노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection이나 NodeList 객체를 배열로 변환하여 사용하는 것을 권장

## 39.3 노드 탐색<a name="39.3"></a>

- DOM 트리 상의 노드를 탐색할 수 있도록 Node, Element 인터페이스는 트리 탐색 프로퍼티를 제공

### 39.3.1 공백 텍스트 노드

- 언급하지 않았지만 HTML 요소 사이의 스페이스, 탭, 줄바꿈(개행) 등의 공 백 문자는 텍스트 노드를 생성하며 공백 텍스트 노드라고 함

- 노드를 탐색할 때는 공백 문자가 생성한 공백 텍스트 노드에 주의해야 함

### 39.3.2 자식 노드 탐색

- 자식 노드를 탐색하기 위해서는 다음과 같은 노드 탐색 프로퍼티 사용함(`Node.prototype.childNodes`, `Element.prototype.children`, `Node.prototype.firstChild`, `Node.prototype.lastChild`, `Node.prototype.lastChild`, `Element.prototype.lastElementChild`)

### 39.3.3 자식 노드 존재 확인

- 자식 노드가 존재하는지 확인하려면 `Node.prototype.hasChildNodes` 메서드 사용

- `hasChildNodes` 메서드는 자식 노드가 존재하면 `true`, 자식 노드가 존재하지 않으면 `false` 반환

- `hasChildNodes` 메서드는 `childNodes` 프로퍼티와 마찬가지로 텍스트 노드를 포함하여 자식 노드의 존재를 확인함

### 39.3.4 요소 노드의 텍스트 노드 탐색

- 요소 노드의 텍스트 노드는 `firstChild` 프로퍼티로 접근 가능

- `firstChild` 프로퍼티는 첫 번째 자식 노드를 반환

- `firstChild` 프로퍼티가 반환한 노드는 텍스트 노드이거나 요소 노드

### 39.3.5 부모 노드 탐색

- 부모 노드를 탐색하려면 `Node.prototype.parentNode` 프로퍼티 사용

- 텍스트 노드는 DOM 트리의 최종단 노드인 리프 노드이므로 부모 노드가 텍스트 노드인 경우는 없음

### 39.3.6 형제 노드 탐색

- 부모 노드가 같은 형제 노드를 탐색하려면 다음과 같은 노드 탐색 프로퍼티 사용(`Node.prototype.previousSibling`, `Node.prototype.nextsibling`, `Element.prototype.previousElementSibling`, `Element.prototype.nextElementSibling`)

## 39.4 노드 정보 취득<a name="39.4"></a>

- 노드 객체에 대한 정보를 취득하려면 노드 정보 프로퍼티 사용(`Node.prototype.nodeType`, `Node.prototype.nodeName`)

## 39.5 요소 노드의 텍스트 조작<a name="39.5"></a>

### 39.5.1 nodeValue

- 지금까지 살펴본 노드 탐색, 노드 정보 프로퍼티는 모두 읽기 전용 접근자 프로퍼티

- `Node.prototype.nodeValue` 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티

- nodeValue 프로퍼티는 참조와 할당 모두 가능

- 노드 객체의 nodeValue 프로퍼티를 참조하면 노드 객체의 값을 반환

### 39.5.2 textcontent

- `Node.prototype.textcontent` 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경함

- 요소 노드의 `textcontent` 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역(시작 태그와 종료 태그 사이) 내의 텍스트를 모두 변환함

## 39.6 DOM 조작<a name="39.6"></a>

- 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것

- DOM 조작에 의해 DOM에 새로운 노드가 추가되거나 삭제되면 리플로우와 리페인트 발생하는 원인이 되므로 성능에 영향을 줌

- 복잡한 콘텐츠를 다루는 DOM 조작은 성능 최적화를 위해 주의해서 다루어야 함

### 39.6.1 innerHTML

- `Element.prototype.innerHTML` 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 HTML 마크업을 취득하거나 변경함

### 39.6.2 insertAdjacentHTIviL 메서드

- `Element.prototype.insertAdjacentHTML(position, DOMString)` 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입함

### 39.6.3 노드 생성과 추가

- `innerHTML` 프로퍼티와 `insertAdjacentHTML` 메서드는 HTML 마크업 문자열을 파싱하여 노드를 생성하고 DOM에 반영

- DOM은 노드를 직접 생성/삽입/삭제/치환하는 메서드도 제공함

#### 요소 노드 생성

- `Document.prototype.createElement(tagName)` 메서드는 요소 노드를 생성하여 반환함

#### 텍스트 누드 생성

- `Document.prototype.createTextNode(text)` 메서드는 텍스트 노드를 생성하여 반환함

#### 텍스트 노드를 요소 노드의 자식 노드로 추가

- `Node.prototype.appendChild(childNode)` 메서드는 매개변수 `childNode`에게 인수로 전달한 노드를 `appendChild` 메서드를 호출한 노드의 마지막 자식 노드로 추가함

- `appendChild` 메서드의 인수로 `createTextNode` 메서드로 생성한 텍스트 노드를 전달하면 `appendChild` 메서드를 호출한 노드의 마지막 자식 노드로 텍스트 노드가 추가됨

#### 요소 노드를 DOM에 추가

- `Node.prototype.appendChild` 메서드를 사용하여 텍스트 노드와 부자 관계로 연결한 요소 노드를 다른 요소 노드의 마지막 자식 요소로 추가함

### 39.6.4 복수의 노드 생성과 추가

- 여러 개의 요소 노드를 DOM에 추가하는 경우 `DocumentFragment` 노드를 사용하는 것이 더 효율적

### 39.6.5 노드 삽입

#### 마지막 노드로 추가

- `Node.prototype.appendChild` 메서드는 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로 DOM에 추가함

#### 지정한 위치에 노드 삽입

- `Node.prototype.insertBefore(newNode, childNode)` 메서드는 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입

### 39.6.6 노드 이동

- DOM에 이미 존재하는 노드를 `appendChild` 또는 `insertBefore` 메서드를 사용하여 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가함

### 39.6.7 노드 복사

- `Node.prototype.cloneNode([deep: true ! false])` 메서드는 노드의 사본을 생성하여 반환

### 39.6.8 노드 교체

- `Node.prototype.replaceChild(newChild, oldChild)` 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체함

### 39.6.9 노드 삭제

- `Node.prototype.removeChild(child)` 메서드는 child 매개변수에 인수로 전달한 노드를 DOM에서 삭제

- 인수로 전달한 노드는 `removeChild` 메서드를 호출한 노드의 자식 노드이어야 함

## 39.7 어트리뷰트<a name="39.7"></a>

### 39.7.1 어트리뷰트 노드와 attributes 프로퍼티

- HTML 어트리뷰트는 HTML 요소의 시작 태그에 어트리뷰트 이름="어트리뷰트 값" 형식으로 정의

### 39.7.2 HTML 어트리뷰트 조작

- HTML 어트리뷰트 값을 참조하려면 `Element.prototype.getAttribute(attributeName)` 메서드를 사용하고, HTML 어트리뷰트 값을 변경하려면 `Element.prototype.setAttribute(attributeName, attributevalue)` 메서드를 사용함

### 39.7.3 HTML 어트리뷰트 vs. DOM 프로퍼티

- 요소 노드 객체에는 HTML 어트리뷰트에 대응하는 프로퍼티(이하 DOM 프로퍼티) 존재

- DOM 프로퍼티들은 HTML 어트리뷰트 값을 초기값으로 가지고 있음

- HTML 어트리뷰트의 역할은 HTML 요소의 초기 상태를 지정하는 것(HTML 어트리뷰트 값은 HTML 요소의 초기 상태를 의미하며 이는 변하지 않음)

#### 어트리뷰트 노드

- HTML 어트리뷰트로 지정한 HTML 요소의 초기 상태는 어트리뷰트 노드에서 관리

#### DOM 프로퍼티

- 사용자가 입력한 최신 상태는 HTML 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티가 관리하며, DOM 프로퍼티는 사용자의 입력에 의한 상태 변화에 반응하여 언제나 최신 상태 유지

### 39.7.4 data 어트리뷰트와 dataset 프로퍼티

- data 어트리뷰트와 `dataset` 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환 가능

- data 어트리뷰트는 `data-user-id`, `data-role`과 같이 `data-` 접두사 다 음에 임의의 이름을 붙여 사용

## 39.8 스타일<a name="39.8"></a>

### 39.8.1 인라인 스타일조작

- `HTMLElement.prototype.style` 프로퍼티는 setter와 getter 모두 존재하는 접근자 프로퍼티로서 요소 노드의 인라인 스타일을 취득하거나 추가 또는 변경

### 39.8.2 클래스조작

- `class` 어트리뷰트에 대응하는 `DOM` 프로퍼티는 `class`가 아니라 `className`과 `classList`

#### className

- HTML 요소 의 class 어트리뷰트 값을 취득하거나 변경

#### classList

- `class` 어트리뷰트의 정보를 담은 `DOMTokenList` 객체를 반환

- `DOMTokenList` 객체는 `class` 어트리뷰트의 정보를 나타내는 컬렉션 객체로서 유사 배열 객체이면서 이터러블

### 39.8.3 요소에 적용되어 있는 CSS 스타일 참조

- `style` 프로퍼티는 인라인 스타일만 반환

- 클래스를 적용한 스타일이나 상속을 통해 암묵적으로 적용된 스타일은 `style` 프로퍼티로 참조 불가

- HTML 요소에 적용되어 있는 모든 CSS 스타일을 참조 해야 할 경우 `getComputedStyle` 메서드 사용

## 39.9 DOM 표준<a name="39.9"></a>

- WHATWG이 단일 표준을 내놓기로 두 단체가 합의하여, 총 4가지 버전 존재

# 40장 이벤트

## 40.1 이벤트 드리븐 프로그래밍<a name="40.1"></a>

- 애플리케이션이 특정 타입의 이벤트에 대해 반응하여 어떤 일을 하고 싶다면 해당하는 타입의 이벤트가 발생했을 때 호출될 함수를 브라우저에게 알려 호출을 위임함

  - 이벤트가 발생했을 때, 호출될 함수를 `이벤트 핸들러`라 하고, 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것을 `이벤트 핸들러 등록`이라고 함

- 이벤트와 그에 대응하는 함수(이벤트 핸들러)를 통해 사용자와 애플리케이션이 상호 작용하는 프로그램의 흐름을 이벤트 드리븐 프로그래밍이라고 함(이벤트 중심으로 제어하는 프로그래밍 방식)

## 40.2 이벤트 타입<a name="40.2"></a>

- 이벤트 타입이란?

  - 이벤트의 종류를 나타내는 문자열

- 이벤트 타입 상세 목록
  - MDN Event reference 참고

### 40.2.1 마우스 이벤트

| 이벤트 타입 | 이벤트 발생 시점                                            |
| ----------- | ----------------------------------------------------------- |
| click       | 마우스 버튼을 클릭했을 때                                   |
| dblclick    | 마우스 버튼을 더블클릭했을 때                               |
| mousedown   | 마우스 버튼을 눌렀을 때                                     |
| mouseup     | 누르고 있던 마우스 버튼을 놓았을 때                         |
| mousemove   | 마우스 커서를 움직였을 때                                   |
| mouseenter  | 마우스 커서를 HTML 요소 안으로 이동했을 때(버블링되지 않음) |
| mouseover   | 마우스 커서를 HTML 요소 안으로 이동했을 때(버블링됨)        |
| mouseleave  | 마우스 커서를 HTML 요소 밖으로 이동했을 때(버블링되지 않음) |
| mouseout    | 마우스 커서를 HTML 요소 밖으로 이동했을 때(버블링됨)        |

### 40.2.2 키보드 이벤트

| 이벤트 타입 | 이벤트 발생 시점                                                                                                                                                                                                                                 |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| keydown     | 모든 키를 눌렀을 때 발생 <br/> - control, option, shift, tab, delete, enter, 방향 키와 문자, 숫자, 특수 문자 키를 눌렀을 때 발생 <br/> - 문자, 숫자, 특수문자, enter 키를 눌렀을 때는 연속적으로 발생하지만 그 외의 키를 눌렸을 때는 한번만 발생 |
| keypress    | 문자 키를 눌렀을 때 연속적으로 발생 <br/> - control, option, shift, tab, delete, 방향 키 등을 눌렀을 때는 발생하지 않고 문자, 숫자, 특수 문자, enter 키를 눌렀을 때만 발생 <br/> - 폐지되었으므로 사용하지 않을 것을 권장                        |
| keyup       | 누르고 있던 키를 놓았을 때 한번만 발생함 <br/> - keydown 이벤트와 마찬가지로 control, option, shift, tab, delete, enter, 방향 키와 문자, 숫자, 특수 문자 키를 놓았을 때 발생                                                                     |

### 40.2.3 포커스 이벤트

- `focusin`, `focusout` 이벤트 핸들러를 이벤트 핸들러 프로퍼티 방식으로 등록하면 크롬, 사파리에서 정상 동작 하지 않음
  - `focusin`, `focusout` 이벤트 핸들러는 `addEventListener` 메서드 방식을 사용해 등록해야 함

| 이벤트 타입 | 이벤트 발생 시점                               |
| ----------- | ---------------------------------------------- |
| focus       | HTML 요소가포커스를 받았을 때(버블링되지 않음) |
| blur        | HTML 요소가포커스를 잃었을 때(버블링되지 않음) |
| focusin     | HTML 요소가 포커스를 받았을 때(버블링)         |
| focusout    | HTML 요소가포커스를 잃었을 때(버블링됨)        |

### 40.2.4 폼 이벤트

|이벤트 타입|이벤트 발생 시점|
|submit|1. form 요소 내의 input, select 입력 필드에서 엔터 키를 눌렀을 때 <br/> 2. form 요소 내의 submit 버튼(<button>, <input type="submit">) 클릭 시 <br/> - submit 이벤트는 form 요소에서 발생|
|reset|form요소 내의 reset 버튼을 클릭했을 때(최근에는 사용 안함)|

### 40.2.5 값 변경 이벤트

| 이벤트 타입      | 이벤트 발생 시점                                                                                                                                                                                                                                                                                                      |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| input            | input(text, checkbox, radio), select, textarea 요소의 값이 입력되었을 때                                                                                                                                                                                                                                              |
| change           | input(text, checkbox, radio), select, textarea 요소의 값이 변경되었을 때 <br/> - change 이벤트는 input 이벤트와 달리 HTML 요소가 포커스를 잃었을 때 사용자 입력이 종료되었다고 인식하여 발생함 <br/> - 사용자가 입력을 하고 있을 때는 input 이벤트가 발생하고 사용자 입력이 종료되어 값이 변경되면 change 이벤트 발생 |
| readystatechange | HTML 문서의 로드와 파싱 상태를 나타내는 `document.readyState` 프로퍼티 값('loading', 'interactive', 'complete')이 변경될 때                                                                                                                                                                                           |

### 40.2.6 DOM 뮤테이션 이벤트

| 이벤트 타입      | 이벤트 발생 시점                                            |
| ---------------- | ----------------------------------------------------------- |
| DOMContentLoaded | HTML 문서의 로드와 파싱이 완료되어 DOM 생성이 완료되었을 때 |

### 40.2.7 뷰 이벤트

| 이벤트 타입 | 이벤트 발생 시점                                                                                     |
| ----------- | ---------------------------------------------------------------------------------------------------- |
| resize      | - 브라우저 윈도우(window)의 크기를 리사이즈할 때 연속적으로 발생 <br/> - 오직 window 객체에서만 발생 |
| scroll      | 웹페이지(document) 또는 HTML 요소를 스크롤할 때 연속적으로 발생                                      |

### 40.2.8 리소스 이벤트

| 이벤트 타입 | 이벤트 발생 시점                                                                                                      |
| ----------- | --------------------------------------------------------------------------------------------------------------------- |
| load        | DOMContentLoaded 이벤트가 발생한 이후, 모든 리소스(이미지, 폰트 등)의 로딩이 완료되었을 때(주로 window 객체에서 발생) |
| unload      | 리소스가 언로드될 때(주로 새로운 웹페이지를 요청한 경우)                                                              |
| abort       | 리소스 로딩이 중단되었을 때                                                                                           |
| error       | 리소스 로딩이 실패했을 때                                                                                             |

## 40.3 이벤트 핸들러 등록<a name="40.3"></a>

- 이벤트 핸들러란?

  - 이벤트가 발생했을 때 브라우저에 호출을 위임한 함수
  - 이벤트가 발생하면 브라우저에 의해 호출될 함수

- 이벤트 핸들러 등록
  - 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것
  - 등록 방법 3가지

### 40.3.1 이벤트 핸들러 어트리뷰트 방식

- 이벤트 핸들러 어트리뷰트의 이름은 on 접두사와 이벤트의 종류를 나타내는 이벤트 타입으로 이루어져 있음

- 이벤트 핸들러 어트리뷰트 값으로 함수 호출문 등의 문을 할당하면 이벤트 핸들러가 등록됨

  - 함수 참조가 아닌 함수 호출문 등의 문을 할당함

- 이벤트 핸들러 등록
  - 함수 호출을 브라우저에게 위임하는 것
  - 콜백 함수와 마찬가지로 함수 참조를 등록해야 브라우저가 이벤트 핸들러 호출 가능
  - 함수 참조가 아니라 함수 호출문을 등록하면 함수 호출문의 평가 결과가 이벤트 핸들러로 등록됨
  - 함수를 반환하는 고차 함수 호출문을 이벤트

```html
<!DOCTYPE html>
<html>
  <body>
    <button onclick="sayHi('Lee')">Click me!</button>
    <script>
      function sayHi(name) {
        console.log(`Hi! ${name}.`);
      }
    </script>
  </body>
</html>
```

### 40.3.2 이벤트 핸들러 프로퍼티 방식

- window 객체와 Document, HTMLElement 타입의 DOM 노드 객체는 이벤트에 대응하는 이벤트 핸들러 프로퍼티를 가지고 있음

- 이벤트 핸들러 프로퍼티의 키

  - 이벤트 핸들러 어트리뷰트와 마찬가지로 on 접두사와 이벤트의 종류를 나타내는 이벤트 타입으로 이루어져 있음
  - 이벤트 핸들러 프로퍼티에 함수를 바인딩하면 이벤트 핸들러 등록됨

- 이벤트 핸들러를 등록하기 위해서는 이벤트를 발생시킬 객체인 이벤트 타깃과 이벤트의 종류를 나타 내는 문자열인 이벤트 타입, 이벤트 핸들러를 지정할 필요 존재

  - 버튼 요소가 클릭되면 handleClick 함수를 호출하도록 이벤트 핸들러를 등록하는 경우 이벤트 타깃은 버튼 요소이며, 이벤트 타입은 'click'이며, 이벤트 핸들러는 handleClick 함수

- 이벤트 핸들러는 대부분 이벤트를 발생시킬 이벤트 타깃에 바인딩함

  - 반드시 이벤트 타깃에 이벤트 핸들러를 바인딩해야 하는 것은 아님
  - 이벤트 핸들러는 이벤트 타깃 또는 전파된 이벤트를 캐치할 DOM 노드 객체에 바인딩함

- 이벤트 핸들러 어트리뷰트 방식도 결국 DOM 노드 객체의 이벤트 핸들러 프로퍼티로 변환되므로 결과적으로 이벤트 핸들러 프로퍼티 방식과 동일함

- 이벤트 핸들러 프로퍼티 방식은 이벤트 핸들러 어트리뷰트 방식의 HTML과 자바스크립트가 뒤섞이는 문제 해결 가능
  - 이벤트 핸들러 프로퍼티에 하나의 이벤트 핸들러만 바인딩할 수 있다는 단점 존재

```html
<!DOCTYPE htmt>
<html>
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector('button1);

      // 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩
      $button.onclick = function () {
      	console.log('button click');
      };
    </script>
  </body>
</html>
```

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector('button');

      // 이벤트 핸들러 프로퍼티 방식은 하나의 이벤트에 하나의 이벤트 핸들러만을 바인딩 가능
      // 첫 번째로 바인딩된 이벤트 핸들러는 두 번째 바인딩된 이벤트 핸들러에 의해 재할당되어 실행되지 않음
      $button.onclick = function () {
        console.log('Button clicked 1');
      };

      // 두 번째로 바인딩된 이벤트 핸들러
      $button.onclick = function () {
        console.log('Button clicked 2');
      };
    </script>
  </body>
</html>
```

### 40.3.3 addEventListener 메서드 방식

- `EventTarget.prototype.addEventListener` 메서드(DOM Level 2에서 도입됨)를 사용하여 이벤트 핸들러를 등록 가능

  - 이벤트 핸들러 어트리뷰트 방식과 이벤트 핸들러 프로퍼티 방식은 DOM Level 0부터 제공되던 방식

- `addEventListener` 메서드의 첫 번째 매개변수에는 이벤트의 종류를 나타내는 문자열인 이벤트 타입 전달

- 이벤트 핸들러 프로퍼티 방식과는 달리 `on` 접두사를 붙이지 않음

- 두 번째 매개변수에는 이벤트 핸들러를 전달함

- 마지막 매개변수에는 이벤트를 캐치할 이벤트 전파 단계(캡처링 or 버블링)를 지정함

- 생략 또는 false를 지정 시, 버블링 단계에서 이벤트를 캐치하고 `true`를 지정하면 캡처링 단계에서 이벤트를 캐치함

- 이벤트 핸들러 프로퍼티 방식은 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩하지만 `addEventListener` 메서드에는 이벤트 핸들러를 인수로 전달함

## 40.4 이벤트 핸들러 제거<a name="40.4"></a>

- `addEventListener` 메서드로 등록한 이벤트 핸들러를 제거하려면 `EventTarget.prototype.removeEventListener` 메서드 사용

- `removeEventListener` 메서드에 전달할 인수는 `addEventListener` 메서드와 동일

- `addEventListener` 메서드에 전달한 인수와 `removeEventListener` 메서드에 전달한 인수가 일치하지 않으면 이벤트 핸들러가 제거되지 않음

- `removeEventListener` 메서드에 인수로 전달한 이벤트 핸들러는 `addEventListener` 메서드에 인수로 전달한 등록 이벤트 핸들러와 동일한 함수이어야 함

  - 무명 함수를 이벤트 핸들러로 등록한 경우 제거 불가능
  - 이벤트 핸들러를 제거하려면 이벤트 핸들러의 참조를 변수나 자료구조에 저장해야 함

- 기명 이벤트 핸들러 내부에서 `removeEventListener` 메서드를 호출하여 이벤트 핸들러를 제거하는 것은 가능

- 이벤트 핸들러는 단 한 번만 호출됨

  - 버튼 요소를 여러 번 클릭해도 단 한 번만 이벤트 핸들러가 호출됨

- 기명 함수를 이벤트 핸들러로 등록할 수 없는 경우 호출된 함수(함수 자신을 가리키는) `arguments.callee` 사용 가능

- `arguments.callee`는 코드 최적화를 방해하므로 `strict mode`에서 사용이 금지됨

- 가급적 이벤트 핸들러의 참조를 변수나 자료구조에 저장하여 제거하는 편이 좋음

- 이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러는 `removeEventListener` 메서드로 제거 불가능

- 이벤트 핸들러 프로퍼티 방식으로 등록한 이벤트 핸들러를 제거하려면 이벤트 핸들러 프로퍼티에 null 할당함

```html
<!DOCTYPE html>
<html>
  <body>
    <button>Click me!</button>
    <script>
      const $button = document.querySelector('button');

      const handleclick = () => console.log('button click');

      // 이벤트 핸들러 등록
      $button.addEventListener('click', handleClick);

      // 이벤트 핸들러 제거
      // addEventListener 메서드에 전달한 인수와 removeEventListener 메서드에 전달한 인수가 일치하지 않으면 이벤트 핸들러가 제거되지 않음
      $button.removeEventListener('click', handleClick, true); // 실패
      $button.removeEventListener('click', handleClick); // 성공
    </script>
  </body>
</html>
```

```js
// 이벤트 핸들러 등록
$button.addEventListener('click', () => console.log('button click'));
// 등록한 이벤트 핸들러를 참조할 수 없으므로 제거 불가능
```

## 40.5 이벤트 객체<a name="40.5"></a>

- 이벤트가 발생하면 이벤트에 관련한 다양한 정보를 담고 있는 이벤트 객체가 동적으로 생성됨

- 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달됨

- 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달되어 매개변수 근에 암묵적으로 할당됨

- 이벤트 핸들러 어트리뷰트 방식으로 이벤트 핸들러를 등록했다면 다음과 같이 `event`를 통해 이벤트 객체를 전달받을 수 있음

- 이벤트 핸들러 어트리뷰트 방식의 경우 이벤트 객체를 전달받으려면 이벤트 핸들러의 첫 번째 매개변수 이름이 반드시 `event`이어야 함

- `event`가 아닌 다른 이름으로 매개변수를 선언하면 이벤트 객체를 전달받지 못함
  - 이벤트 핸들러 어트리뷰트 값은 사실 암묵적으로 생성되는 이벤트 핸들러의 함수 몸체를 의미하기 때문
  - 어트리뷰트는 파싱되어 함수를 암묵적으로 생성하여 `onclick` 이벤트 핸들러 프로퍼티에 할당됨
  - 암묵적으로 생성된 `onclick` 이벤트 핸들러의 첫 번째 매개변수의 이름이 `event`로 암묵적으로 명명되기 때문에 `event`가 아닌 다른 이름으로는 이벤트 객체를 전달받지 못함

### 40.5.1 이벤트 객체의 상속 구조

- 이벤트가 발생하면 이벤트 타입에 따라 다양한 타입의 이벤트 객체가 생성됨

  - Event, UIEvent, MouseEvent 등 모두는 생성자 함수
  - 생성자 함수를 호출하여 이벤트 객체 생성 가능

- 이벤트 객체 중 일부는 사용자의 행위에 의해 생성된 것이고 일부는 자바스크립트 코드에 의해 인위적으로 생성된 것

- DOM 내에서 발생한 이벤트에 의해 생성되는 이벤트 객체를 나타냄

- Event 인터페이스에는 모든 이벤트 객체의 공통 프로퍼티가 정의되어 있고, FocusEvent, MouseEvent, KeyboardEvent, WheelEvent 같은 하위 인터페이스에는 이벤트 타입에 따라 고유한 프로퍼티가 정의되어 있음

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      // Event 생성자 함수를 호출하여 foo 이벤트 타입의 Event 객체 생성
      let e = new Event('foo');
      console.log(e);
      // Event {isTrusted: false, type: "foo", target: null, ... }
      console.log(e.type); // "foo"
      console.log(e instanceof Event); // true
      console.log(e instanceof Object); // true

      // FocusEvent 생성자 함수를 호출하여 focus 이벤트 타입의 FocusEvent 객체 생성함
      e = new FocusEvent('focus');
      console.log(e); // FocusEvent {isTrusted: false, relatedTarget: null, view: null, ... }

      // MouseEvent 생성자 함수를 호출하여 click 이벤트 타입의 MouseEvent 객체를 생성함
      e = new MouseEvent('click');
      console.log(e); // MouseEvent {isTrusted: false, screenX: 0, screenY: 0, clientX: 0, ... }

      //KeyboardEvent 생성자 함수를 호출하여 keyup 이벤트 타입의 KeyboardEvent 객체 생성
      e = new KeyboardEvent('change');
      console.log(e); // KeyboardEvent {isTrusted: false, key: code: ctrlKey: false, ... }

      // InputEvent 생성자 함수를 호출하여 change 이벤트 타입의 InputEvent 객체를 생성함
      e = new InputEvent('change');
      console.log(e);
      // InputEvent {isTrusted: false, data: null, inputType: "", ...}
    </script>
  </body>
</html>
```

### 40.5.2 이벤트 객체의 공통 프로퍼티

- Event 인터페이스(Event.prototype)에 정의되어 있는 이벤트 관련 프로퍼티는 UIEvent, CustomEvent, MouseEvent 등 모든 파생 이벤트 객체에 상속됨

- Event 인터페이스의 이벤트 관련 프로퍼티는 모든 이벤트 객체가 상속받는 공통 프로퍼티

- 일반적으로 이벤트 객체의 target 프로퍼티와 currentTarget 프로퍼티는 동일한 DOM 요소를 가리키지만, 이벤트 위임에서는 이벤트 객체의 target 프로퍼티와 currentTarget 프로퍼티가 서로 다른 DOM 요소를 가리킬 수 있음

### 40.5.3 마우스 정보 취득

- click, dblclick, mousedown, mouseup, mousemove, mouseenter, mouseleave 이벤트가 발생하면 생성되는 MouseEvent9 타입의 이벤트 객체는 다음과 같은 고유의 프로퍼티를 가짐
  - 마우스 포인터의 좌표 정보를 나타내는 프로퍼티
  - 버튼 정보를 나타내는 프로퍼티

### 40.5.4 키보드 정보 취득

- keydown, keyup, keypress 이벤트가 발생하면 생성되는 KeyboardEvent 타입의 이벤트 객체는 altKey, ctrlKey, shiftKey, metaKey, key, keyCode 같은 고유의 프로퍼티를 가짐

- keyup 이벤트가 발생하면 생성되는 KeyboardEvent 타입의 이벤트 객체는 입력한 키 값을 문자열로 반환하는 key 프로퍼티를 제공

- 입력한 키와 key 프로퍼티 값의 대응관계는 https://keycode.info 참고

## 40.6 이벤트 전파<a name="40.6"></a>

- 이벤트 전파란?

  - DOM 트리상에 존재하는 DOM 요소노드에서 발생한 이벤트는 DOM 트리를 통해 전파되는 것

- ul 요소의 두 번째 자식 요소인 li 요소를 클릭하면 클릭 이벤트가 발생함

  - 이때 생성된 이벤트 객체는 이벤트를 발생시킨 DOM 요소인 이벤트 타깃을 중심으로 DOM 트리를 통해 전파됨
  - 이벤트 전파는 이벤트 객체가 전파되는 방향에 따라 다음과 같이 3단계로 구분 가능

- 이벤트 타깃(event.target)은 li 요소이고 커런트 타깃(event.currentTarget)은 ul 요소

- 캡처링 단계 : 이벤트가 상위 요소에서 하위 요소 방향으로 전파

- 타깃 단계 : 이벤트가 이벤트 타깃에 도달

- 버블링 단계 : 이벤트가 하위 요소에서 상위 요소 방향으로 전파

- 이벤트 핸들러 어트리뷰트/프로퍼티 방식으로 등록한 이벤트 핸들러는 타깃 단계와 버블링 단계의 이벤트 이벤트만 캐치 가능

- `addEventListener` 메서드 방식으로 등록한 이벤트 핸들러는 타깃 단계와 버블링 단계뿐만 아니라 캡처링 단계의 이벤트도 선별적으로 캐치 가능

- 이벤트는 이벤트를 발생시킨 이벤트 타깃은 물론 상위 DOM 요소에서도 캐치 가능

- DOM 트리를 통해 전파되는 이벤트는 이벤트 패스(이벤트가 통과하는 DOM 트리 상의 경로, `Event.prototype.composedPath` 메서드로 확인 가능)에 위치한 모든 DOM 요소에서 캐치 가능

- 이벤트는 캡처링과 버블링을 통해 전파됨

  - 다음 이벤트는 버블링을 통해 전파되지 않음
  - 이벤트들은 버블링을 통해 이벤트를 전파하는지 여부를 나타내는 이벤트 객체의 공통 프로퍼티 `event.bubbles` 값 모두 `false`

- 포커스 이벤트

  - focus
  - blur

- 리소스 이벤트

  - load
  - unload
  - abort
  - error

- 마우스 이벤트
  - mouseenter
  - mouseleave

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
  </body>
</html>
```

```html
<!DOCTYPE html>
<html>
  <body>
    <ul id="fruits">
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      const $fruits = document.getElementById('fruits1);

      // #fruits 요소의 하위 요소인 li 요소를 클릭한 경우
      $fruits.addEventListener('click', e => {
      	console.log(`이벤트 단계: ${e.eventPhase}`); // 3: 버블링 단계
      	console.log(`이벤트 타깃: ${e.target}`); // [object HTMLLIElement]
      	console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLUListElement]
      });
    </script>
  </body>
</html>
```

## 40.7 이벤트 위임<a name="40.7"></a>

- 여러 개의 하위 DOM 요소에 각각 이벤트 핸들러를 등록하는 대신 하나의 상위 DOM 요소에 이벤트 핸들러를 등록하는 방법

- 이벤트는 이벤트 타깃, 상위 DOM 요소에서도 캐치 가능

- 이벤트 위임을 통해 상위 DOM 요소에 이벤트 핸들러를 등록하면 여러 개의 하위 DOM 요소에 이벤트 핸들러 등록 필요 없음

- 이벤트 위임을 통해 하위 DOM 요소에서 발생한 이벤트를 처리할 때 주의할 점은 상위 요소에 이벤트 핸들러를 등록하기 때문에 이벤트 타깃(이벤트를 실제로 발생시킨 DOM 요소)이 개발자가 기대한 DOM 요소가 아닐 수도 있음

  - 이벤트에 반응이 필요한 DOM 요소에 한정하여 반응이 필요한 DOM 요소에 한정하여 이벤트 핸들러가 실행되도록 이벤트 타깃을 검사할 필요 있음

- `Element.prototype.matches` 메서드는 인수로 전달된 선택자에 의해 특정 노드를 탐색 가능한지 확인해줌

- 일반적으로 이벤트 객체의 `target` 프로퍼티와 `currentTarget` 프로퍼티는 동일한 DOM 요소를 가리키나, 이벤트 위임을 통해 상위 DOM 요소에 이벤트를 바인딩한 경우 이벤트 객체의 `target` 프로퍼티와 `currentTarget` 프로퍼티가 다른 DOM 요소를 가리킬 수 있음

```js
function activate({ target }) {
	// 이벤트를 발생시킨 요소(target)이 ul#fruits 자식 요소가 아니라면 무시
	if (!target.matches('#fruits > li')) return;
	...
```

```js
$fruits.onclick = activate;
```

## 40.8 DOM 요소의 기본 동작 조작

### 40.8.1 DOM 요소의 기본 동작 중단

- 이벤트 객체의 `preventDefault` 메서드는 이러한 DOM 요소의 기본 동작을 중단시킴

```html
<!DOCTYPE html>
<html>
  <body>
    <a href="https://vvwvv.google.com">go</a>
    <input type="checkbox" />
    <script>
      document.querySelector('a').onclick = (e) => {
        // a 요소의 기본 동작 중단
        e.preventDefault();
      };

      document.querySelector('input[type=checkbox]').onclick = (e) => {
        // checkbox 요소의 기본 동작 중단
        e.preventDefault();
      };
    </script>
  </body>
</html>
```

### 40.8.2 이벤트 전파 방지

- 이벤트 객체의 `stopPropagation` 메서드는 이벤트 전파 중지시킴
  - `stoppropagation` 메서드는 하위 DOM 요소의 이벤트를 개별적으로 처리하기 위해 이벤트의 전파 중단시킴

```html
<!DOCTYPE html>
<html>
  <body>
    <div class="container">
      <button class="btn1">Button 1</button>
      <button class="btn2">Button 2</button>
      <button class="btn3">Button 3</button>
    </div>
    <script>
      // 이벤트 위임
      // 클릭된 하위 버튼 요소의 color 변경함
      document.querySelector('.container').onclick = ({ target }) => {
        if (!target.matches('.container > button')) return;
        target.style.color = 'red';
      };
      // .btn2 요소는 이벤트를 전파하지 않으므로 상위 요소에서 이벤트 캐치 불가
      document.querySelector('.btn2').onclick = (e) => {
        e.stopPropagation(); // 이벤트 전파 중단
        e.target.style.color = 'blue';
      };
    </script>
  </body>
</html>
```

## 40.9 이벤트 핸들러 내부의 this

### 40.9.1 이벤트 핸들러 어트리뷰트 방식

- `handleClick` 함수 내부의 this는 전역 객체 window 가리킴

- 이벤트 핸들러 어트리뷰트의 값으로 지정한 문자열은 사실 암묵적으로 생성되는 이벤트 핸들러의 문

- `handleClick` 함수는 이벤트 핸들러에 의해 일반 함수로 호출됨

- 일반 함수로서 호출되는 함수 내부의 `this`는 전역 객체를 가리킴

- `handleClick` 함수 내부 의 `this`는 전역 객체 `window` 가리키나, 이벤트 핸들러를 호출할 때는 인수로 전달한 `this`는 이벤트를 바인딩한 DOM 요소를 가리킴

- 이벤트 핸들러 어트리뷰트 방식에 의해 암묵적으로 생성된 이벤트 핸들러 내부의 `this`는 이벤트를 바인딩한 `DOM	` 요소를 가리킴
  - 이벤트 핸들러 프로퍼티 방식과 동일

```html
<!DOCTYPE html>
<html>
  <body>
    <button onclick="handleClick()">Click me</button>
    <script>
      function handteClick() {
        console.log(this); // window
      }
    </script>
  </body>
</html>
```

### 40.9.2 이벤트 핸들러 프로퍼티 빙식과 addEventListener 메서드 방식

- 이벤트 핸들러 프로퍼티 방식과 `addEventListener` 메서드 방식 모두 이벤트 핸들러 내부의 `this`는 이벤트를 바인딩한 DOM 요소 가리킴

- 이벤트 핸들러 내부의 `this`는 이벤트 객체의 `currentTarget` 프로퍼티와 동일

- 화살표 함수로 정의한 이벤트 핸들러 내부의 `this`는 상위 스코프의 `this`를 가리킴

- 화살표 함수는 함수 자 체의 this 바인딩을 갖지 않음

```html
<!DOCTYPE html>
<html>
  <body>
    <button class="btnl">0</button>
    <button class="btn2">0</button>
    <script>
      const $button1 = document.querySelector('.btn1');
      const $button2 = document.querySelector('.btn2');

      // 이벤트 핸들러 프로퍼티 방식
      $button1.onclick = (e) => {
        // 화살표 함수 내부의 this는 상위 스코프의 this 가리킴
        console.log(this); // window
        console.log(e.currentTarget); // $button1
        console.log(this === e.currentTarget); // false

        // this는 window를 가리키므로 window.textContent에 NaN(undefined + 1) 할당
        ++this.textcontent;
      };

      // addEventListener 메서드 방식
      $button2.addEventListener('click', (e) => {
        // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킴
        console.log(this); // window
        console.log(e.currentTarget); // $button2
        console.log(this === e.currentTarget); // false

        // this는 window를 가리키므로 window.textContent에 NaN(undefined + 1)을 할당함
        ++this.textcontent;
      });
    </script>
  </body>
</html>
```

## 40.10 이벤트 핸들러에 인수 전달<a name="40.10"></a>

- 함수에 인수를 전달하려면 함수를 호출할 때 전달해야 함

- 이벤트 핸들러 어트리뷰트 방식은 함수 호출문을 사용할 수 있기 때문에 인수를 전달할 수 있음

- 이벤트 핸들러 프로퍼티 방식과 `addEventListener` 메서드 방식의 경우 이벤트 핸들러를 브라우저가 호출하기 때문에 함수 호출문이 아닌 함수 자체를 등록해야 함

  - 따라서 인수를 전달 불가하나, 이벤트 핸들러 내부에서 함수를 호출하면서 인수 전달 가능

- 이벤트 핸들러를 반환하는 함수를 호출하면서 인수 전달 가능

```html
<!DOCTYPE html>
<html>
  <body>
    <label>User name <input type="text" /></label>
    <em class="message"></em>
    <script>
      const MIN_USER_NAME_LENGTH = 5; // 이름 최소 길이
      const $input = document.querySelector('input[type-text]');
      const $msg = document.querySelector('.message');
      const checkUserNameLength = (min) => {
        $msg.textcontent =
          $input.value.length < min ? `이름은 ${min}자 이상 입력해주세요` : '';
      };

      // 이벤트 핸들러 내부에서 함수를 호출하연서 인수를 전달함
      $input.onblur = () => {
        checkUserNameLength(MIN_USER_NAME_LENGTH);
      };
    </script>
  </body>
</html>
```

```html
<!DOCTYPE html>
<html>
  <body>
    <label>User name <input type="text" /></label>
    <em class="message"></em>
    <script>
      const MIN_USER_NAME_LENGTH = 5; // 이름 최소 길이
      const $input = document.querySelector('input[type=text]');
      const $msg = document.querySelector('.message');

      // 이벤트 핸들러를 반환하는 함수
      const checkUserNameLength = (min) => (e) => {
        $msg.textcontent =
          $input.value.length < min ? `이름은 ${min}자 이상 입력해 주세요` : '';
      };

      // 이벤트 핸들러를 반환하는 함수를 호출하면서 인수 전달
      $input.onblur = checkUserNameLengh(MIN_USER_NAME_LENGTH);
    </script>
  </body>
</html>
```

## 40.11 커스텀 이벤트<a name="40.11"></a>

### 40.11.1 커스텀 이벤트 생성

- 이벤트 객체는 Event, UIEvent, MouseEvent 같은 이벤트 생성자 함수로 생성 가능

- 이벤트가 발생하면 암묵적으로 생성되는 이벤트 객체는 발생한 이벤트의 종류에 따라 이벤트 타입이 결정됨

- Event, UIEvent, MouseEvent 같은 이벤트 생성자 함수를 호출하여 명시적으로 생성한 이벤트 객체는 임의의 이벤트 타입 지정 가능

- 개발자의 의도로 생성된 이벤트를 커스텀 이벤트라고 함

- 이벤트 생성자 함수는 첫 번째 인수로 이벤트 타입을 나타내는 문자열을 전달 가능

  - 이벤트 타입을 나타내는 문자열은 기존 이벤트 타입을 사용할 수 있음
  - 기존 이벤트 타입이 아닌 임의의 문자열을 사용하여 새로운 이벤트 타입을 지 가능
  - 일반적으로 CustomEvent 이벤트 생성자 함수 사용

- 생성된 커스텀 이벤트 객체는 버블링되지 않으며, `preventDefault` 메서드로 취소할 수 없음

  - 커스텀 이벤트 객체는 `bubbles`와 `cancelable` 프로퍼티의 값이 `false`로 기본 설정됨

- 커스텀 이벤트 객체의 bubbles또는 cancelable 프로퍼티를 `true`로 설정하려면 이벤트 생성자 함수의 두 번째 인수로 bubbles 또는 cancelable 프로퍼티를 갖는 객체를 전달함

- 커스텀 이벤트 객체에는 bubbles 또는 cancelable 프로퍼티뿐만 아니라 이벤트 타입에 따라 가지는 이벤트 고유의 프로퍼티 값을 지정 가능

- 이벤트 객체 고유의 프로퍼티 값을 지정하려면 이벤트 생성자 함수의 두 번째 인수로 프로퍼티를 전달함
  - 이벤트 생성자 함수로 생성한 커스텀 이벤트는 `isTrusted` 프로퍼티의 값이 언제나 `false`
  - 커스텀 이벤트가 아닌 사용자의 행위에 의해 발생한 이벤트에 의해 생성된 이벤트 객체의 `isTrusted` 프로퍼티 값은 언제나 `true`

```js
// KeyboardEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체 생성
const keyboardEvent = new KeyboardEvent('keyup');
console.log(keyboardEvent.type); // keyup

// CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체 생성
const customEvent = new CustomEvent('foo');
console.log(customEvent.type); // foo
```

```js
// InputEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체 생성
const customEvent = new InputEvent('foo');
console.log(customEvent.isTrusted); // false
```

### 40.11.2 커스텀 이벤트 디스패치

- 생성된 커스텀 이벤트는 `dispatchEvent` 메서드로 디스패치(이벤트를 발생시키는 행위) 가능

- `dispatchEvent` 메서드에 이벤트 객체를 인수로 전달하면서 호출하면 인수로 전달한 이벤트 타입의 이벤트 발생

- 일반적으로 이벤트 핸들러는 비동기 처리 방식으로 동작하지만 `dispatchEvent` 메서드는 이벤트 핸들러를 동기 처리 방식으로 호출함

- `dispatchEvent` 메서드를 호출하면 커스텀 이벤트에 바인딩된 이벤트 핸들러를 직접 호출하는 것과 동일

- `dispatchEvent` 메서드로 이벤트를 디스패치 하기 이전에 커스텀 이벤트를 처리할 이벤트 핸들러 등록해야 함

- 기존 이벤트 타입이 아닌 임의의 이벤트 타입을 지정하여 이벤트 객체를 생성하는 경우 일반적으로 `CustomEvent` 이벤트 생성자 함수 사용함

- `CustomEvent` 이벤트 생성자 함수에는 두 번째 인수로 이벤트와 함께 전달하고 싶은 정보를 담은 `detail` 프로퍼티를 포함하는 객체 전달 가능

- 이벤트 객체의 `detail` 프로퍼티(`e.detail`)에 담겨 전달됨

- 이벤트 타입이 아닌 임의의 이벤트 타입을 지정하여 커스텀 이벤트 객체를 생성한 경우 반드시 `addEventListener` 메서드 방식으로 이벤트 핸들러를 등록해야 함

- 이벤트 핸들러 어트리뷰트/프로퍼티 방식을 사용할 수 없는 이유는 'on + 이벤트 타입'으로 이루어진 이벤트 핸들러 어트리뷰트/프로퍼티가 요소 노드에 존재하지 않음

```html
<!DOCTYPE html>
<html>
  <body>
    <button class="btn">Click me</button>
    <script>
      const $button = document.querySelector('.btn');

      // 버튼 요소에 foo 커스텀 이벤트 핸들러 등록
      // 커스텀 이벤트를 디스패치하기 이전에 이벤트 핸들러 등록해야 함
      $button.addEventListener('click', (e) => {
        console.log(e); // MouseEvent {isTrusted: false, screenX: 0, ... }
        alert(`${e} Clicked!`);
      });

      // 커스텀 이벤트 생성
      const customEvent = new MouseEvent('click');

      // 커스텀 이벤트 디스패치(동기 처리)
      // click 이벤트가 발생함
      $button.dispatchEvent(customEvent);
    </script>
  </body>
</html>
```

```js
// CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체 생성
const customEvent = new CustomEvent('foo');
console.log(customEvent.type); // foo
```

# 41장 타이머

# 41.1 호출 스케줄링

- `호출 스케줄링` : 일정 시간이 경과된 이후에 호출되도록 함수 호출을 예약하는 것
- setTimeout, setInterval, clearTimeout, clearInterval
- 브라우저 환경과 Node.js 환경에서 모두 전역 객체의 메서드로서 타이머 함수를 제공한다. 타이머 함수는 `호스트 객체`다.
- 타이머 함수 setTimeout, setInterval은 모두 일정 시간이 경과된 이후 콜백 함수가 호출되도록 타이머를 생성한다.
- 자바스크립트 엔진은 `싱글 스레드`로 동작한다. 이런 이유로 setTimeout과 setInterval은 `비동기 처리 방식`으로 동작한다.

# 41.2 타이머 함수

## 41.2.1 setTimeout / clearTimeout

- setTimeout 함수는 두 번째 인수로 전달받은 시간으로 단 한번 동작하는 타이머를 생성한다.
- 타이머가 만료되면 첫 번째 인수로 전달받은 콜백 함수가 호출된다.

```jsx
const tieoutId = setTimeout(func|code[, delay, param1, param2, ... ]);
```

| 매개변수 | 설명                                |
| -------- | ----------------------------------- |
| func     | 타이머가 만료된 후 호출될 콜백 함수 |
| delay    | 타이머 만료 시간. 생략 시 기본값 0  |

| param1,
param2, … | 콜백 함수에 전달해야할 인수들 |

```jsx
setTimeout(() => console.log('Hi!'), 1000);

setTimeout((name) => console.log(`Hi! ${name}.`), 1000, 'Lee');
```

- setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다.
  (브라우저 환경 - 숫자, Node.js - 객체)
- 타이머 id를 clearTimeout 함수의 인수로 전달하여 타이머를 취소할 수 있다. 즉, clearTimeout 함수는 호출 스케줄링을 취소한다.

```jsx
const timerId = setTimeout(() => console.log('Hi!'), 1000);

clearTimeout(timeId);
```

## 41.2.2 setInterval / clearInterval

- 타이머가 취소될 때까지 콜백함수가 반복 호출된다.
- setInterval 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환한다.
  (브라우저 환경 - 숫자, Node.js - 객체)
- 타이머 id를 clearInterval 함수의 인수로 전달하여 타이머를 취소할 수 있다. 즉, clearInterval 함수는 호출 스케줄링을 취소한다.

```jsx
let count = 1;
const timeoutId = setInterval(() => {
  console.log(count);
  if (count++ === 5) clearInterval(timeoutId);
}, 1000);
```

# 41.3 디바운스와 스로틀

- scroll, resize, input, mosemove 같이 짧은 시간 간격으로 연속해서 발생하는 이벤트.
- 이러한 이벤트에 바인딩한 이벤트 핸들러는 과도하게 호출되어 성능에 문제를 일으킬 수 있다.
- **디바운스와 스로틀은 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 과도한 이벤트 핸들러의 호출을 방지하는 프로그래밍 기법이다.**

```jsx
<!DOCTYPE html>
<html>
<body>
	<button>click me</button>
	<pre>일반 클릭 이벤트 카운터 <span class="normal-msg">0</span></pre>
	<pre>디바운스 클릭 이벤트 카운터 <span class="debounce-msg">0</span></pre>
	<pre>스로틀 클릭 이벤트 카운터 <span class="throttle-msg">0</span></pre>
	<script>
		const $button = document.querySelector('button');
		const $normalMsg = document.querySelector('.normal-msg');
		const $debounceMsg = document.querySelector('.debounce-msg');
		const $throttleMsg = document.querySelector('.throttle-msg');

		const debounce = (callback, delay) => {
			let timeId;
			return event => {
				if (timerId) clearTimeout(timerId);
				timerId = setTimeout(callback, delay, event);
			};
		}

		const throttle = (callback, delay) => {
			let timeId;
			return event => {
				if (timerId) return;
				timerId = setTimeout(() => {
					callback(event);
					timerId = null;

				}, delay, event);
			};
		};

		$button.addEventEventListener('click', () => {
			$normalMsg.textContent = $normalMsg.textContent + 1;
		});

		$button.addEventEventListener('click', debounce(() => {
			$debounceMsg.textContent = $debounceMsg.textContent + 1;
		}, 500));

		$button.addEventEventListener('click', throttle(() => {
			$throttleMsg.textContent = $throttleMsg.textContent + 1;
		}, 500));
	</script>
</body>
</html>
```

## 41.3.1 디바운스

- `디바운스`: 짧은 시간 간격으로 이벤트가 연속으로 발생하면 이벤트 핸들러를 호출하지 않다가 일정 시간이 경과된 이후에 한 번만 호출되도록 한다.
- 짧은시간 간격으로 발생하는 이벤트를 그룹화해서 마지막에 한번만 이벤트 핸들러가 호출되도록 한다.
- ex) input 이벤트
  - 입력 필드에 입력한 값으로 ajax 요청과 같은 무거운 처리를 수행한다면 사용자가 입력을 완료하지 않았어도 ajax 요청이 전송될 것이다. 이는 서버에도 부담을 주는 불필요한 처리이므로 사용자가 입력을 완료했을 때 한 번만 ajax 요청을 전송하는 것이 바람직하다.
  - 사용자가 입력을 완료했는지 여부는 정확히 알 수 없으므로 일정 시간 동안 텍스트 입력 필드에 값을 입력하지 않으면 입력이 완료된 것으로 간주한다.
- ex) resize, input 입력 필드 자동완성 UI 구현, 버튼 중복 클릭 방지
- 위 예제 함수는 간략하게 구현하여 완전하지 않으므로 실무에서는 underscore의 debounce나 lodash의 debounce 함수를 사용하는 것을 권장한다.

## 41.3.2 스로틀

- 일정 시간 간격으로 이벤트 핸들러가 최대 한 번만 호출되도록 한다.
- 짧은 시간 간격으로 연속해서 발생하는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다.
- ex) scroll 이벤트
  - 짧은 시간 간격으로 연속해서 발생하는 이벤트의 과도한 이벤트 핸들러의 호출을 방지하기 위해 throttle 함수는 이벤트를 그룹화해서 일정 시간 단위로 이벤트 핸들러가 호출되도록 호출 주기를 만든다.
- ex) scroll, 무한 스크롤 UI 구현
- 위 예제 함수는 간략하게 구현하여 완전하지 않으므로 실무에서는 underscore의 throttle나 lodash의 throttle함수를 사용하는 것을 권장한다.

# 42장 비동기 프로그래밍

## 목차

- [42.1 동기 처리와 비동기 처리](#42.1)
- [42.2 이벤트 루프와 태스크 큐](#42.2)

## 42.1 동기 처리와 비동기 처리<a name="42.1"></a>

- 함수의 실행 순서는 실행 컨텍스트 스택으로 관리함

  - 실행 컨텍스트 스택에 함수 실행 컨텍스트가 푸시되는 것은 바로 함수 실행의 시작을 의미

- 자바스크립트 엔진은 하나의 실행 컨텍스트 스택을 갖으며, 한 번에 하나의 태스크만 실행할 수 있는 싱글 스레드 방식으로 동작함

  - 함수를 실행할 수 있는 창구가 하나
  - 동시에 2개 이상의 함수를 동시에 실행할 수 없
  - 실행 중인 실행 컨텍스트(최상위 요소)를 제외한 모든 실행 컨텍스트는 모두 실행 대기 중인 태스크들

- 싱글 스레드 방식

  - 한 번에 하나의 태스크만 실행할 수 있음
  - 처리에 시간이 걸리는 태스크를 실행하는 경우 블로킹(작업 중단) 발생함

- 동기 처리

  - 현재 실행 중인 태스크가 종료할 때까지 다음에 실행될 태스크가 대기하는 방식
  - 태스크를 순서대로 하나씩 처리하므로 실행 순서가 보장됨
  - 태스크가 종료할 때까지 이후 태스크들이 블로킹됨

- 비동기 처리
  - 현재 실행 중인 태스크가 종료 되지 않은 상태라 해도 다음 태스크를 곧바로 실행하는 방식
  - 블로킹이 발생하지 않음
  - 태스크의 실행 순서가 보장되지 않음
  - 비동기 함수는 콜백 패턴을 사용
  - 타이머 함수인 setTimeout과 setinterval, HTTP 요청, 이벤트 핸들러는 비동기 처리 방식으로 동작함
  - 이벤트 루프와 태스크 큐와 관계 있음

```js
// sleep 함수는 일정 시간이 경과한 이후에 콜백 함수 호출
function sleep(func, delay) {
  // Date.now()는 현재 시간을 숫자로 반환
  const delayUntil = Date.now() + delay;

  // 현재 시간(Date.now())에 delay를 더한 delayUntil이 현재 시간보다 작으면 계속 반복함
  while (Date.now() < delayUntil);
  // 일정 시간(delay)이 경과한 이후에 콜백 함수 호출
  func();
}

function foo() {
  console.log('foo');
}

function bar() {
  console.log('bar');
}

// sleep 함수는 3초 이상 실행
sleep(foo, 3 * 1000);

// bar 함수는 sleep 함수의 실행이 종료된 이후에 호출되므로 3초 이상 블로킹됨
bar();
// (3초 경과 후) foo 호출 -> bar 호출
```

```js
// 타이머 함수 setTimeout
function foo() {
  console.log('foo');
}

function bar() {
  console.log('bar');
}

// setTimeout 함수는 일정 시간이 경과한 이후에 콜백 함수 foo 호출함
// setTimeout 함수는 bar 함수를 블로킹하지 않음
setTimeout(foo, 3 * 1000);
bar();
// bar 호출 -> (3초 경과 후) foo 호출
```

## 42.2 이벤트 루프와 태스크 큐<a name="42.2"></a>

- 이벤트 루프

  - 동시성 지원(ex. HTML 요소의 애니메이션 효과 및 이벤트 처리, HTTP 요청을 통한 서버에서 데이터를 가져와 렌더링 등)
  - 브라우저에 내장되어 있는 기능 중 하나

- 자바스크립트 엔진

  - 2개의 영역(콜 스택, 힙)으로 크게 구분할 수 있음
  - 태스크가 요청되면 콜 스택을 통해 요청된 작업을 순차적으로 실행함
  - 브라우저에 내장된 자바스크립트 엔진은 싱글 스레드 방식, 브라우저는 멀티 스레드로 동작함

- 브라우저, Node.js(자바스크립트 엔진 구동 환경)

  - 비동기 처리에서 소스코드의 평가와 실행을 제외한 모든 처리를 담당
  - 태스크 큐와 이벤트 루프 제공

- 콜 스택

  - 소스코드(전역 코드나 함수 코드 등) 평가 과정에서 생성된 실행 컨텍스트가 추가되고 제거되는 스택 자료구조인 실행 컨텍스트 스택

- 힙

  - 객체가 저장되는 메모리 공간
  - 실행 컨텍스트(콜 스택의 요소)는 힙에 저장된 객체를 참조

- 태스크 큐

  - 비동기 함수의 콜백 함수 또는 이벤트 핸들러가 일시적으로 보관되는 영역

- 이벤트 루프
  - 콜스택에 현재 실행 중인 실행 컨텍스트가 있는지, 태스크 큐에 대기 중인 함수(콜백 함수, 이벤트 핸들러 등)가 있는지 반복해서 확인함
