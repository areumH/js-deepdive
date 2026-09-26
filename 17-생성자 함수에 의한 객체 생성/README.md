# 생성자 함수에 의한 객체 생성

## Object 생성자 함수

```js
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'Lee';
person.sayHello = function () {
  console.log('Hi! My name is ' + this.name);
};

console.log(person); // {name: "Lee", sayHello: ƒ}
person.sayHello(); // Hi! My name is Lee
```

- `생성자 함수` = new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수
- 자바스크립트는 Object 생성자 함수 이외에도 String, Number, Boolean, Function, Array, Date, RegExp, Promise 등의 빌트인 생성자 함수를 함께 제공

## 생성자 함수

### 객체 리터럴에 의한 객체 생성 방식의 문제점

- **객체**는 프로퍼티를 통해 객체 고유의 상태를 표현하고, **메서드**를 통해 상태 데이터인 프로퍼티를 참조하고 조작하는 동작을 표현
- 따라서 프로퍼티는 객체마다 프로퍼티 값이 다를 수 있지만 메서드는 내용이 동일한 경우가 일반적
- 프로퍼티 구조가 동일함에도 불구하고 매번 같은 프로퍼티와 메서드를 기술해야함

### 생성자 함수에 의한 객체 생성 방식의 장점

> 객체를 생성하기 위한 템플릿처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있음

```js
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킴
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 인스턴스의 생성
const circle1 = new Circle(5); // 반지름이 5인 Circle 객체를 생성
const circle2 = new Circle(10); // 반지름이 10인 Circle 객체를 생성

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

```
[🗒️ this]

객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수
this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 따라 동적으로 결정됨

- 일반 함수로서 호출 → 전역 객체
- 메서드로서 호출 → 메서드를 호출한 객체 (마침표 앞의 객체)
- 생성자 함수로서 호출 → 생성자 함수가 (미래에) 생성할 인스턴스
```

- 클래스 기반 객체지향 언어의 생성자와는 다르게 그 형식이 정해져 있는 것이 아님
  - 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작
- new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작

```js
// new 연산자와 함께 호출하지 않으면 생성자 함수로 동작하지 않음
// 즉, 일반 함수로서 호출됨
const circle3 = Circle(15);

// 일반 함수로서 호출된 Circle은 반환문이 없으므로 암묵적으로 undefined를 반환
console.log(circle3); // undefined

// 일반 함수로서 호출된 Circle내의 this는 전역 객체를 가리킴
console.log(radius); // 15
```

### 생성자 함수의 인스턴스 생성 과정

#### 1. 인스턴스 생성과 this 바인딩

- 암묵적으로 빈 객체 생성 (= 인스턴스)
- 이 인스턴스는 this에 바인딩됨
- 함수 몸체의 코드가 한 줄씩 실행되는 런타임 이전에 실행됨

#### 2. 인스턴스 초기화

- this에 바인딩되어 있는 인스턴스를 초기화
- 인스턴스에 프러퍼티나 메서드를 추가하고 생성자 함수가 인수로 전달받은 초기값을 인스턴스 프로퍼티에 할당하여 초기화하거나 고정값을 할당

#### 3. 인스턴스 반환

- 생성자 함수 내부에서 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환
- this가 아닌 다른 객체를 명시적으로 반환하면 this가 반환되지 못하고 return문에 명시한 객체가 반환됨

```js
function Circle(radius) {
  // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩됨

  // 2. this에 바인딩되어 있는 인스턴스를 초기화
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };

  // 3. 암묵적으로 this를 반환
  // 명시적으로 원시값을 반환하면 원시값 반환은 무시되고 암묵적으로 this가 반환됨
  return 100;
}

// 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: ƒ}
```

### 내부 메서드 [[Call]]과 [[Construct]]

- 일반 객체는 호출할 수 없지만 함수는 호출할 수 있음
- 따라서 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론, 함수로서 동작하기 위해 함수 객체만을 위한 `[[Envrionment]]`, `[[FormalParameters]]` 등의 내부 슬롯과 `[[Call]]`, `[[Construct]]` 같은 내부 메서드를 추가로 가지고 있음

```js
function foo() {}

// 일반적인 함수로서 호출: [[Call]]이 호출됨
foo();

// 생성자 함수로서 호출: [[Construct]]가 호출됨
new foo();
```

- 내부 메서드 `[[Call]]`을 갖는 함수 객체 → `callable`
  - 호출할 수 없는 객체는 함수 객체가 아니므로 함수로서 기능하는 객체, 즉 함수 객체는 반드시 `callable`
  - 따라서 모든 함수 객체는 내부 `[[Call]]`을 갖고 있으므로 호출할 수 있음
- 내부 메서드 `[[Construct]]`를 갖는 함수 객체 → `constructor`
- 내부 메서드 `[[Construct]]`를 갖지 않는 함수 객체 → `non-constructor`
  - 모든 함수 객체가 `[[Construct]]`를 갖는 것은 아님

### constructor와 non-constructor

- `constructor` - 함수 선언문, 함수 표현식, 클래스(클래스도 함수)
- `non-constructor` - 메서드, 화살표 함수

### new 연산자

- new 연산자와 함께 함수를 호출하면 해당 함수는 생성자 함수로 동작
- 즉, 함수 객체 내부 메서드 중 `[[Construct]]`가 호출됨
- 단, new 연산자와 함께 호출하는 함수는 `non-constructor`가 아닌 `constructor`이어야 함

```js
// 생성자 함수로서 정의하지 않은 일반 함수
function add(x, y) {
  return x + y;
}

// 생성자 함수로서 정의하지 않은 일반 함수를 new 연산자와 함께 호출
let inst = new add();

// 함수가 객체를 반환하지 않았으므로 반환문이 무시됨
//  따라서 빈 객체가 생성되어 반환됨
console.log(inst); // {}

// 객체를 반환하는 일반 함수
function createUser(name, role) {
  return { name, role };
}

// 일반 함수를 new 연산자와 함께 호출
inst = new createUser('Lee', 'admin');
// 함수가 생성한 객체를 반환
console.log(inst); // {name: "Lee", role: "admin"}
```

- 반대로 new 연산자 없이 생성자 함수를 호출하면 일반 함수로 호출됨
- 다시 말해, 함수 객체의 내부 메서드 `[[Construcot]]`가 호출되는 것이 아니라 `[[Call]]`이 호출됨

```js
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수 호출하면 일반 함수로서 호출됨
const circle = Circle(5);
console.log(circle); // undefined

// 일반 함수 내부의 this는 전역 객체 window를 가리킴
console.log(radius); // 5
console.log(getDiameter()); // 10

circle.getDiameter();
// TypeError: Cannot read property 'getDiameter' of undefined
```

### new.target

- new 연산자와 함께 생성자 함수로서 호출 → 함수 내부의 `new.target`은 함수 자신을 가리킴
- new 연산자 없이 일반 함수로서 호출 → 함수 내부의 `new.target`은 `undefined`

```js
// 생성자 함수
function Circle(radius) {
  // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined
  if (!new.target) {
    // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환
    return new Circle(radius);
  }

  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출됨
const circle = Circle(5);
console.log(circle.getDiameter()); // 10
```
