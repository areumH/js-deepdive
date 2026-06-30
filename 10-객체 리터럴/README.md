# 객체 리터럴

> 자바스크립트 = 객체 기반의 프로그래밍 언어
> <br> 자바스크립트를 구성하는 거의 모든 것이 객체이며, 원시 값을 제외한 나머지 값(함수, 배열, 정규 표현식)은 모두 객체

- 객체는 프로퍼티와 메서드로 구성된 집합
- **프로퍼티** = 객체의 상태를 나타내는 값(data)
- **메서드** = 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작(behavior)

## 객체 리터럴에 의한 객체 생성

```
[🗒️ 인스턴스]

클래스에 의해 생성되어 메모리에 저장된 실체 - 객체지향 프로그래밍에서 객체는 클래스와 인스턴스를 포함한 개념
클래스는 인스턴스를 생성하기 위한 템플릿의 역할
```

- 자바스크립트는 프로토타입 기반 객체지향 언어로서 클래스 기반 객체지향 언어와는 달리 다양한 객체 생성 방법을 지원
  - 객체 리터럴, Object 생성자 함수, 생성자 함수, Object.create 메서드, 클래스(ES6)
- **리터럴** = 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용하여 값을 생성하는 표기법
  - 객체 리터럴은 중괄호({...}) 내에 0개 이상의 프로퍼티를 정의
  - 변수에 할당되는 시점에 자바스크립트 엔진은 객체 리터럴을 해석해 객체를 생성

```js
var empty = {}; // 빈 객체
console.log(typeof empty); // object
```

- 객체 리터럴의 중괄호는 코드 블록의 의미하지 않음
- 코드 블록의 닫는 중괄호 뒤에는 세미콜론을 붙이지 않지만, 객체 리터럴은 값으로 평가되는 표현식이므로 객체 리터럴의 닫는 중괄호 뒤에는 세미콜론을 붙임


## 프로퍼티 

> 객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성

- **프로퍼티 키**: 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
- **프로퍼티 값**: 자바스크립트에서 사용할 수 있는 모든 값

```js
var person = {
  firstName: 'Ung-mo',
  last-name: 'Lee' // SyntaxError: Unexpected token -
};

var person = {
  firstName: 'Ung-mo', // 식별자 네이밍 규칙을 준수하는 프로퍼티 키
  'last-name': 'Lee'   // 식별자 네이밍 규칙을 준수하지 않는 프로퍼티 키
};

console.log(person); // {firstName: "Ung-mo", last-name: "Lee"}
```

- 식별자 네이밍 규칙을 따르지 않는 이름에는 반드시 따옴표를 사용
- 빈 문자열, 예약어를 프로퍼티 키로 사용해도 에러가 발생하지 않지만, 권장하지 않음

```js
var foo = {
  0: 1,
  1: 2,
  2: 3
};

console.log(foo); // {0: 1, 1: 2, 2: 3}
```
- 프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 암묵적 타입 변환을 통해 문자열이 됨
- ex. 프로퍼티 키에 숫자 리터럴을 사용하면 따옴표는 붙지 않지만 내부적으로는 문자열로 변환

```js
var foo = {
  name: 'Lee',
  name: 'Kim'
};

console.log(foo); // {name: "Kim"}
```
- 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어씀


## 메서드

> 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드라고 부름
<br> **메서드** = 객체에 묶여있는 함수

```js
var circle = {
  radius: 5, // ← 프로퍼티

  // 원의 지름
  getDiameter: function () { // ← 메서드
    return 2 * this.radius; // this는 circle을 가리킴
  }
};

console.log(circle.getDiameter()); // 10
```


## 프로퍼티 접근

- **마침표 표기법** - 마침표 프로퍼티 접근 연산자(.) 사용
- **대괄호 표기법** - 대괄호 프로퍼티 접근 연산자([...]) 사용

```js
var person = {
  name: 'Lee'
};

// 마침표 표기법에 의한 프로퍼티 접근
console.log(person.name); // Lee

// 대괄호 표기법에 의한 프로퍼티 접근
console.log(person['name']); // Lee

console.log(person[name]); // ReferenceError: name is not defined
console.log(person.age); // undefined
```

- 대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 함
- 객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환

```js
var person = {
  'last-name': 'Lee',
  1: 10
};

person.'last-name';  // -> SyntaxError: Unexpected string
person.last-name;    // -> 브라우저 환경: NaN
                     // -> Node.js 환경: ReferenceError: name is not defined
person[last-name];   // -> ReferenceError: last is not defined
person['last-name']; // -> Lee

// 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표를 생략 가능
person.1;     // -> SyntaxError: Unexpected number
person.'1';   // -> SyntaxError: Unexpected string
person[1];    // -> 10 : person[1] -> person['1']
person['1'];  // -> 10
```
- Node.js 환경에서는 현재 name이라는 식별자 선언이 없으므로 ReferenceError 에러가 발생
- 브라우저 환경에서는 name이라는 전역 변수(전역 객체 window의 프로퍼티)가 암묵적으로 존재
  - 전역 변수 name은 창(window)의 이름을 가리키며, 기본값은 빈 문자열 
  - `person.last-name`은 `undefined - ''`과 같으므로 NaN이 
  

## 프로퍼티 삭제

> **delete 연산자** - 객체의 프로퍼티를 삭제
<br> delete 연산자의 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 함
<br> 만약 존재하지 않는 프로퍼티를 삭제하면 아무런 에러 없이 무시됨

```js
var person = {
  name: 'Lee'
};

// 프로퍼티 동적 생성
person.age = 20;

// person 객체에 age 프로퍼티가 존재
// 따라서 delete 연산자로 age 프로퍼티를 삭제할 수 있음
delete person.age;

// person 객체에 address 프로퍼티가 존재하지 않음
// 따라서 delete 연산자로 address 프로퍼티를 삭제할 수 없다. 이때 에러가 발생하지 않음
delete person.address;

console.log(person); // {name: "Lee"}
```

## ES6에서 추가된 객체 리터럴의 확장 기능

### 프로퍼티 축약 표현

```js
// ES6
let x = 1, y = 2;

// 프로퍼티 축약 표현
const obj = { x, y };

console.log(obj); // {x: 1, y: 2}
```

- ES6에서는 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할 수 있음
- 이때 프로퍼티 키는 변수 이름으로 자동 생성


### 계산된 프로퍼티 이름

```js
// ES5
var prefix = 'prop';
var i = 0;

var obj = {};

// 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;
obj[prefix + '-' + ++i] = i;


// ES6
const prefix = 'prop';
let i = 0;

// 객체 리터럴 내부에서 계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
const obj = {
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i,
  [`${prefix}-${++i}`]: i
};

console.log(obj); // {prop-1: 1, prop-2: 2, prop-3: 3}
```

- 문자열 또는 문자열로 타입 변환을 할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수 있음
- 단, 프로퍼티 키로 사용할 표현식을 대괄호([...])로 묶어야 함


### 메서드 축약 표현

```js
// ES5
var obj = {
  name: 'Lee',
  sayHi: function() {
    console.log('Hi! ' + this.name);
  }
};


// ES6
const obj = {
  name: 'Lee',
  // 메서드 축약 표현
  sayHi() {
    console.log('Hi! ' + this.name);
  }
};

obj.sayHi(); // Hi! Lee
```

- 메서드를 정의할 때 function 키워드를 생략한 축약 표현 사용 가능
- 메서드 축약 표현으로 정의한 메서드는 프로퍼티에 할당한 함수와 다르게 동작