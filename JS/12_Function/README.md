# 함수

## 함수란?

- 일련의 과정을 문(statement)로 구현하고, 코드 블록(스코프, `{ ... }`)로 감싸서 하나의 실행 단위로 정의한 것을 의미한다.
- 매개 변수(parameter) 를 인자로 받고, 인수(argument)를 입력하거나 반환값(return value)을 출력한다.
- 함수는 함수 정의(function definition)를 통해 생성되며, 생성된다고 해서 바로 함수가 실행되는 것이 아니라 스코프 내에서 정의된 함수를 호출(function call/invoke) 해줘야 한다.

```jsx
function add(x, y) {
	if (typeof x !== 'number' || typeof y !== 'number') {
		console.log("파라미터로 숫자만 전달해주세요.");
		return false;
	}

	return x + y;
}

console.log(add(10, 10)); // 20
console.log(add(10, "10")); // "파라미터로 숫자만 전달해주세요."
```

## 함수를 사용하는 이유

- 코드의 재사용
- 유지보수의 편의성
- 코드의 신뢰성
- 코드의 가독성

## 함수 리터럴

- 함수 이름
    - 함수 이름은 하나의 식별자이다. 즉, 식별자 네이밍 규칙을 준수해야 한다.
    - 함수 이름은 함수 스코프 내에서만 참조할 수 있는 식별자이다.
    - 함수 이름은 생략 가능하며, 이름이 존재하는 함수를 기명 함수(named function), 이름이 없는 함수를 익명 함수(annonymous function) 이라고 한다.
- 매개변수 목록
    - 0개 이상의 매개변수를 소괄호로 감싸고 쉼표로 구분한다.
    - 각 매개변수에는 함수를 호출할 때 지정한 인수가 순서대로 할당된다.
    즉, 매개변수 목록은 순서에 의미가 있다.
    - 매개변수는 함수 스코프 내에서 변수와 동일하게 취급된다. 때문에 매개변수 또한 식별자 네이밍 규칙을 준수해야 한다.
- 함수 스코프
    - 함수가 호출되었을 때, 일괄적으로 실행될 문들을 하나의 실행 단위로 정의한 코드 블록을 의미한다.
    - 함수 호출에 의해 함수 스코프가 실행된다.
- 함수는 객체이다. 때문에 함수는 함수 객체만의 고유한 프로퍼티를 가진다.
때문에 일반 객체는 호출할 수 없지만 함수는 호출할 수 있다. (함수는 일급 객체)

```jsx
function add(x, y) {
	return x + y;
}

/*
	ƒ add(x, y)
		arguments: null
		caller: null
		length: 2
		name: "add"
		prototype: {constructor: ƒ}
		[[FunctionLocation]]: VM116:1
		[[Prototype]]: ƒ ()
		[[Scopes]]: Scopes[1]
*/
console.dir(add);

/*
	ƒ add(x, y)
		x: 10 -> 고유 프로퍼티
		arguments: null
		caller: null
		length: 2
		name: "add"
		prototype: {constructor: ƒ}
		[[FunctionLocation]]: VM116:1
		[[Prototype]]: ƒ ()
		[[Scopes]]: Scopes[1]
*/
add.x = 10; // add() 함수의 고유 프로퍼티 생성
console.dir(add);
```

## 함수 정의

```jsx
// 함수 선언문
function a() {}

// 함수 표현식
// 기명 함수 > 식별자에 할당된 함수명을 가진다.
let b = function bb() {};
// 익명 함수 > 변수명 그대로 함수명을 가진다.
let c = function() {};

// Function 생성자 함수
let constructorFunc = new Function('x', 'y', 'return x + y');

// 화살표 함수 (ES6)
let arrow = () => {};
```

### 함수 선언문

- 함수 선언문은 함수 리터럴과 형태가 동일하다.
- 함수 리터럴은 함수 이름을 생략할 수 있으나 함수 선언문은 함수 이름을 생략할 수 없다.

```jsx
// 함수 리터럴는 익명 함수로 선언할 수 있다.
let func = function() {};

// 함수 선언문은 익명 함수로 선언할 수 없다.
// Uncaught SyntaxError: Function statements require a function name
function() {};
```

- **함수 선언문은 표현식이 아닌 문(statement)이다.**
함수 선언문이 만약 표현식인 문이라면 완료 값 `undefined` 대신 표현식이 평가되어 생성된 함수가 출력되어야 한다.
- 하지만 자바스크립트에서 `{}` 는 중의적인 표현이기 때문에 자바스크립트 엔진은 문맥에 따라 객체 리터럴로 해석할 수도, 함수 리터럴로 해석할 수도 있다.

```jsx
// 함수 선언문
// 함수 선언문의 경우 자바스크립트 엔진이 암묵적으로 생성한 식별자이다.
function add(x, y) {};
add();
/*
	arguments: null
		caller: null
		length: 2
		name: "add" -> 자바스크립트 엔진에 의해 암묵적으로 생성된 함수명, 식별자
		prototype: {constructor: ƒ}
		[[FunctionLocation]]: VM369:3
		[[Prototype]]: ƒ ()
		[[Scopes]]: Scopes[1]
*/
console.dir(add);

// 함수 리터럴을 파라미터로 사용하면 함수 선언문이 아니라 함수 리터럴 표현식으로 해석된다.
// 그룹 연산자 내의 피연산자는 값으로 평가될 수 있는 표현식이어야 하기 때문에 함수 리터럴 표현식으로 해석된다.
(function sayHello() { console.log('Hello'); });
// sayHello() 함수는 해당 함수 몸체 내에서만 참조할 수 있는 식별자이기 때문에 함수를 호출할 수 없다.
sayHello(); // Uncaught ReferenceError: sayHello is not defined
```

## **함수 생성 시점과 함수 호이스팅**

- 자바스크립트는 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행된다. (변수 호이스팅을 생각해보자.)
- 때문에 함수 선언문으로 정의된 함수는 자바스크립트 엔진의 **함수 호이스팅에 의해 가장 먼저 함수 객체로 생성된다.**
- 하지만 변수에 할당된 함수 표현식의 경우, 변수에 의해 할당되는 함수 리터럴인 문이다.
변수 선언이 런타임 이전에 실행되어 undefined  로 초기화되고, 변수 할당문의 값은 할당문이 실행되는 시점, 즉 런타임 시점에 평가되므로 함수 표현식의 함수 리터럴도 할당문이 실행되는 시점에 평가되어 함수 객체가 된다.

```jsx
// 함수 참조
console.dir(add); // f add(x, y)
console.dir(sub); // Uncaught ReferenceError: sub is not defined

// 함수 호출
console.log(add(10, 10));
console.log(sub(20, 20));

// 함수 선언문
function add(x, y) {
	return x + y;
}

// 함수 표현식
// 함수 호이스팅이 아니라 변수 호이스팅이 발생하기 때문에 해당 변수의 할당문이 런타임 시점에 평가된다.
// 때문에 sub 변수를 변수 호이스팅에 의해 상단에서 호출할 경우, 해당 함수 할당문이 평가되지 않은 시점이기 때문에 에러가 발생한다.
let sub = function(x, y) {
	return x - y;
}
```