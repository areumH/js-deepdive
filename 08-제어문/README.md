# 제어문

- 조건에 따라 코드 블록을 실행(조건문)하거나 반복 실행(반복문)할 때 사용
- 코드의 실행 흐름을 인위적으로 제어 가능


## 블록문

> 0개 이상의 문을 중괄호로 묶는 것 (= 코드 블록 = 블록)

- 자바스크립트는 블록문을 하나의 실행 단위로 취급
- 블록문은 언제나 문의 종료를 의미하는 자체 종결성을 갖기 때문에 블록문의 끝에는 세미콜론을 붙이지 않음

```js
// 블록문
{
  var foo = 10;
}

// 제어문
var x = 1;
if (x < 10) {
  x++;
}

// 함수 선언문
function sum(a, b) {
  return a + b;
}
```


## 조건문

> 주어진 조건식의 평가 결과에 따라 코드 블록(블록문)의 실행을 결정

### if ... else 문

- if 문의 조건식이 불리언 값이 아닌 값으로 평가되면 자바스크립트 엔진에 의해 암묵적으로 불리언 값으로 강제 변환되어 실행할 코드 블록을 결정
- if 문과 else 문은 2번 이상 사용할 수 없지만 else if 문은 여러 번 사용 가능
- 대부분의 if ... else 문은 삼항 조건 연산자로 바꿔쓸 수 있음
- if ... else 문은 표현식이 아닌 문 - 값처럼 사용할 수 없기 때문에 변수에 할당할 수 없음

```js
// x가 짝수이면 result 변수에 문자열 '짝수'를 할당하고, 홀수이면 문자열 '홀수'를 할당
var x = 2;
var result;

if (x % 2) { // 2 % 2는 0, 이때 0은 false로 암묵적 강제 변환
  result = '홀수';
} else {
  result = '짝수';
}

console.log(result); // 짝수

// 0은 false로 취급
var result = x % 2 ? '홀수' : '짝수';
console.log(result); // 짝수
```

### switch 문

> 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case 문으로 실행 흐름을 옮김

- case 문 - 상황을 의미하는 표현식을 지정하고 콜론으로 마침
- switch 문의 표현식과 일치하는 case 문이 없다면 실행 순서는 default 문으로 이동
- 논리적 참, 거짓보다는 다양한 상황에 따라 실행할 코드 블록을 결정

```js
// 월을 영어로 변환 (11 → 'November')
var month = 11;
var monthName;

switch (month) {
  case 1: monthName = 'January';
  case 2: monthName = 'February';
  case 3: monthName = 'March';
  case 4: monthName = 'April';
  case 5: monthName = 'May';
  case 6: monthName = 'June';
  case 7: monthName = 'July';
  case 8: monthName = 'August';
  case 9: monthName = 'September';
  case 10: monthName = 'October';
  case 11: monthName = 'November';
  case 12: monthName = 'December';
  default: monthName = 'Invalid month';
}

console.log(monthName); // Invalid month
```
- **폴스루**(fall through): 문을 실행한 후 switch 문을 탈출하지 않고 switch 문이 끝날 때까지 이후의 모든 case문과 default 문을 실행
- break 키워드로 구성된 break 문은 코드 블록에서 탈출하는 역할을 함
- default 문에는 break 문을 생략하는 것이 일반적

```js
// 올바른 switch 문

// 월을 영어로 변환 (11 → 'November')
var month = 11;
var monthName;

switch (month) {
  case 1: monthName = 'January';
    break;
  case 2: monthName = 'February';
    break;
  case 3: monthName = 'March';
    break;
  case 4: monthName = 'April';
    break;
  case 5: monthName = 'May';
    break;
  case 6: monthName = 'June';
    break;
  case 7: monthName = 'July';
    break;
  case 8: monthName = 'August';
    break;
  case 9: monthName = 'September';
    break;
  case 10: monthName = 'October';
    break;
  case 11: monthName = 'November';
    break;
  case 12: monthName = 'December';
    break;
  default: monthName = 'Invalid month';
}

console.log(monthName); // November
```


## 반복문

> 조건식의 평가 결과가 참인 경우 코드 블록을 실행
<br> 그 후 조건식을 다시 평가하여 여전히 참인 경우 코드 블록을 다시 실행, 조건이 거짓일 때까지 반복

### for 문

```js
for (변수 선언문 또는 할당문; 조건식; 증감식) {
  조건식이 참인 경우 반복 실행될 문;
}
```

- 조건식이 거짓으로 평가될 때까지 코드 블록을 반복
- 변수 선언문은 단 한 번만 실행
- 변수 선언문, 조건식, 증감식은 모두 옵션이므로 반드시 사용할 필요는 없지만, 어떤 식도 선언하지 않으면 무한 루프가 됨

```js
// 무한루프
for (;;) { ... }
```


### while 문

> 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행 - 반복 횟수가 불명확할 때 주로 사용

- 조건문의 평가 결과가 거짓이 되면 코드 블록을 실행하지 않고 종료
- 조건식의 평가 결과가 불리언 값이 아니면 불리언 값으로 강제 변환하여 논리적 참, 거짓을 구별


### do ... while 문

> 코드 블록을 먼저 실행하고 조건식을 평가 - 코드 블록은 무조건 한 번 이상 실행됨


## break 문

> 레이블 문, 반복문 또는 switch 문의 코드 블록을 탈출

- 레이블 문, 반복문, switch 문의 코드 블록 외에 break 문을 사용하면 SyntaxError(문법 에러)가 발생
- 레이블 문 = 식별자가 붙은 문 - 프로그램의 실행 순서를 제어하는 데 사용

```js
// foo라는 레이블 식별자가 붙은 레이블 문
foo: console.log('foo');
```

- 중첩된 for 문의 내부 for 문에서 break 문을 실행하면 내부 for 문을 탈출하여 외부 for 문으로 진입
- 이때 내부 for 문이 아닌 외부 for 문을 탈출하려면 레이블 문을 사용
- break 문을 반복문, switch 문에서 사용할 경우 break 문에 레이블 식별자를 지정하지 않음

```js
// outer라는 식별자가 붙은 레이블 for 문
outer: for (var i = 0; i < 3; i++) {
  for (var j = 0; j < 3; j++) {
    // i + j === 3이면 outer라는 식별자가 붙은 레이블 for 문을 탈출
    if (i + j === 3) break outer;
    console.log(`inner [${i}, ${j}]`);
  }
}

console.log('Done!');
```


## continue 문

> 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킴
<br> break 문처럼 반복문을 탈출하지는 않음


