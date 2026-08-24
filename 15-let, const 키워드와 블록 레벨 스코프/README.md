# let, const 키워드와 블록 레벨 스코프

## var 키워드로 선언한 변수의 문제점

### 변수 중복 선언 허용

```js
var x = 1;
var y = 1;

// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용
// 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작
var x = 100;
// 초기화문이 없는 변수 선언문은 무시됨
var y;

console.log(x); // 100
console.log(y); // 1
```

- var 키워드로 선언한 변수를 중복 선언하면 초기화문(변수 선언과 동시에 초기값을 할당) 유무에 따라 다르게 동작
  - 초기화문이 있는 변수 선언문 = var 키워드가 없는 것처럼 동작
  - 초기화문이 없는 변수 선언문 = 무시됨 (이때 에러는 발생하지 않음)
- 변수를 중복 선언하고 값까지 할당하면 의도치 않게 먼저 선언된 변수 값이 변경되는 부작용 발생


### 함수 레벨 스코프

> 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 됨


### 변수 호이스팅

> var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있음
<br> 단, 할당문 이전에 변수를 참조하면 언제나 undefined를 반환

```js
// 이 시점에는 변수 호이스팅에 의해 이미 foo 변수가 선언됨 - 1. 선언 단계
// 변수 foo는 undefined로 초기화됨 - 2. 초기화 단계
console.log(foo); // undefined

// 변수에 값을 할당 - 3. 할당 단계
foo = 123;

console.log(foo); // 123

// 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행됨
var foo;
```


## let 키워드

### 변수 중복 선언 금지

> let 키워드로 이름이 같은 변수를 중복 선언하면 문법 에러가 발생


### 블록 레벨 스코프

> let 키워드로 선언한 변수는 모든 코드 블록(함수, if 문, for 문, while 문, try/catch 문 등)을 지역 스코프로 인정하는 **블록 레벨 스코프**를 따름


### 변수 호이스팅

- let 키워드로 선언한 변수는 **선언 단계**와 **초기화 단계**가 분리되어 진행됨
- 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행됨
  - 초기화 단계가 실행되기 이전에 변수에 접근하려고 하면 참조 에러가 발생
- **일시적 사각지대** = 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간

```js
// 런타임 이전에 선언 단계가 실행된다. 아직 변수가 초기화되지 않았음
// 초기화 이전의 일시적 사각 지대에서는 변수를 참조할 수 없음
console.log(foo); // ReferenceError: foo is not defined

let foo; // 변수 선언문에서 초기화 단계가 실행됨
console.log(foo); // undefined

foo = 1; // 할당문에서 할당 단계가 실행됨
console.log(foo); // 1
```

```js
let foo = 1; // 전역 변수

{
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  let foo = 2; // 지역 변수
}
```

- 자바스크립트는 ES6에 도입된 let, const를 포함해서 모든 선언(var, let, const, function, function*, class 등)을 호이스팅함
- 단, ES6에서 도입된 let, const, class를 사용한 선언문은 호이스팅이 발생하지 않는 것처럼 동작함


### 전역 객체와 let

```js
// 이 예제는 브라우저 환경에서 실행해야 함

// 전역 변수
var x = 1;
// 암묵적 전역
y = 2;
// 전역 함수
function foo() {}

// var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티
console.log(window.x); // 1
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있음
console.log(x); // 1

// 암묵적 전역은 전역 객체 window의 프로퍼티
console.log(window.y); // 2
console.log(y); // 2

// 함수 선언문으로 정의한 전역 함수는 전역 객체 window의 프로퍼티
console.log(window.foo); // ƒ foo() {}
// 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있음
console.log(foo); // ƒ foo() {}
```

- let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아님
  - 즉, `window.foo`와 같이 접근할 수 없음
- let 전역 변수는 보이지 않는 개념적인 블록 내에 존재하게 됨

```js
// 이 예제는 브라우저 환경에서 실행해야 함
let x = 1;

// let, const 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아님
console.log(window.x); // undefined
console.log(x); // 1
```


## const 키워드

### 선언과 초기화

> const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 함

```js
{
  // 변수 호이스팅이 발생하지 않는 것처럼 동작
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  const foo = 1;
  console.log(foo); // 1
}

// 블록 레벨 스코프를 가짐
console.log(foo); // ReferenceError: foo is not defined
```

- const 키워드로 선언한 변수는 let 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가짐
- 변수 호이스팅이 발생하지 않는 것처럼 동작


### 재할당 금지

> const 키워드로 선언한 변수는 재할당이 금지됨


### 상수

- const 키워드로 선언된 변수에 원시 값을 할당한 경우 원시 값은 변경할 수 없는 값이고 const 키워드에 의해 재할당이 금지되므로 할당된 값을 변경할 수 있는 방법은 없음
- 상수의 이름은 대문자로 성넌해 상수임을 명확히 나타내며, 여러 단어로 이뤄진 경우 언더스코어(_)로 구분해서 스네이크 케이스로 표현하는 것이 일반적


### const 키워드와 객체

> const 키워드로 선언된 변수에 원시 값을 할당한 경우 값을 변경할 수 없지만 const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있음

```js
const person = {
  name: 'Lee'
};

// 객체는 변경 가능한 값, 따라서 재할당없이 변경이 가능
person.name = 'Kim';

console.log(person); // {name: "Kim"}
```

- const 키워드는 재할당을 금지할 뿐 불변을 의미하지 않음
- 새로운 값을 재할당하는 것은 불가능하지만 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능


## var vs. let vs. const

- ES6을 사용한다면 var 키워드는 사용하지 않음
- 재할당이 필요한 경우에 한정에 let 키워드를 사용 - 이때 변수의 스코프는 최대한 좁게 만듦
- 변경이 발생하지 않고 읽기 전용으로 사용하는(재할당이 필요 없는 상수) 원시 값과 객체에는 const 키워드를 사용
  - const 키워드는 재할당을 금지하므로 var, let 키워드보다 안전