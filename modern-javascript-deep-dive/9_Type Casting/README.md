# 타입 변환이란?

- 자바스크립트의 모든 값은 타입을 가지고 있다. (원시 타입, 참조 타입 등)
이러한 데이터 타입을 개발자의 의도에 맞게 변경 할 수 있는데 이러한 작업을 **명시적 타입 변환** 또는 **타입 캐스팅**이라고 한다.

```jsx
// 문자열 타입의 값을 str 변수에 정의
let str = '20211226';
// str 변수를 명시적으로 숫자형 타입으로 변경
let num = parseInt(str);

console.log(typeof str, str); // string 20211226
console.log(typeof num, num); // number 20211226
```

- 개발자의 의도와는 관계없이 표현식을 평가하는 도중 자바스크립트 엔진에 의해 암묵적으로 타입이 변환되는 경우가 있는데, 이를 **암묵적 타입 변환** 또는 타입 강제 전환이라고 한다.

```jsx
// 숫자형 타입의 값을 num 변수에 정의
let num = 20211226;
// str 변수에 num 변수를 할당하면서 + '' 를 붙여 암묵적으로 문자열 타입으로 변환
let str = num + '';
// tmpNum 변수에 str 변수를 할당하면서 변수명 앞에 + 를 붙여 암묵적으로 숫자형타입으로 변환
let tmpNum = +str;

console.log(typeof num, num); // number 20211226
console.log(typeof str, str); // string 20211226
console.log(typeof tmpNum, tmpNum); // number 20211226
```

- 이러한 명시적 타입 변환이나 암묵적 타입 변환은 대상이 되는 값의 데이터 타입을 직접 변경하는 것이 아니다. 원시 타입의 값은 불변하기 때문에 변경 할 수 없다.
즉, 타입 변환의 과정은 표현식 중 하나로, 새로운 메모리 공간에 타입이 변경된 값을 할당한다.

```jsx
let num = 20211226;
// 암묵적 타입 변환
// 타입 변환된 값을 변수나 함수에 할당하지 않기 때문에 이는 메모리에 할당된 후 삭제된다.
num + '';

console.log(typeof num, num); // number, 20211226
```

## 암묵적 타입 변환

### 문자열 타입으로 변환

```jsx
// Template Literal
let value = 'Hello World';
console.log(`NPC : \"${value}\"`); // NPC : "Hello World"

// Number Type
0 + ''
-0 + ''
1 + ''
-1 + ''
NaN + ''
Infinity + ''
-Infinity + ''

// Boolean Type
true + ''
false + ''

// null Type
null + ''

// undefined Type
undefined + ''

// Symbol Type
Symbol('error') // TypeError: Cannot convert a Symbol value to a string

// Object Type
({}) + '' // [object Object]
Math + '' // [object Math]
[] + '' // ''
[10, 20] + '' // "10, 20"
(function() {}) // "function() {}"
Array + '' // "function Array() { [native code] }"
```

### 숫자 타입으로 변환

```jsx
// 산술 연산자
1 - '1' // 0
2 * 10 // 20
1 / 'one' // NaN
1 + '1' // + 연산자는 암묵적으로 문자열 타입으로 변환하도록 한다.

// 비교 연산자
'1' > 0 // true

// String Type
+'' // 0
+'0' // 0
+ '1' // 1
+ 'string' // NaN

// Boolean Type
+true // 1
+false // 0

//null Type
+null // 0 -> falsy type

// undefined Type
+undefined // 0 -> falsy type

// Symbol Type
+Symbol('0') // TypeError: Cannot convert a Symbol value to a number

// Object Type
+{} // NaN
+[] // 0 -> falsy type
+[10, 20] // -> NaN
+(function() {}) // -> NaN
```

### 불리언 타입으로 변환

- 자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 값 또는 Falsy 값으로 구분한다.
- 즉, 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 문맥에서 Truthy 값은 `true` 로, Falsy 값은 `false`로 암묵적 타입 변환이 일어난다.
다음의 값들은 모두 Falsy Type 의 값이다.
    - false
    - undefined
    - null
    - 0, -0
    - NaN
    - ‘’, “”, ``

```jsx
if('') console.log('\'\' is Falsy Type'); // print x
if(true) console.log('true is Truthy Type'); // print o
if(0) console.log('0 is Falsy Type'); // print x
if('str') console.log('\'str\' is Truthy Type'); // print o
if(null) console.log('null is Falsy Type'); // print x
if(undefined) console.log('undefined is Falsy Type'); // print x

// 아래의 조건문의 출력문은 모두 정상적으로 출력된다.
if(!'') console.log('\'\' is Falsy Type');
if(!0) console.log('0 is Falsy Type');
if(!null) console.log('null is Falsy Type');
if(!undefined) console.log('undefined is Falsy Type');
if(!NaN) console.log('NaN is Falsy Type');
if(!false) console.log('false is Falsy Type');
```

## 명시적 타입 변환

- 개발자의 의도에 따라 값의 데이터 타입을 변경한다. 즉, 보는 이로 하여금 어떤 타입으로 변경할건지 의도를 파악 할 수 있다.
- 표준 **Built-In** 생성자 함수를 통해 데이터 타입을 변환하는 방법이 있다.

### 문자열 타입으로 변환

- String 생성자 함수를 new 연산자 없이 호출하는 방법 (Built-In)
- Object.prototype.toString 메소드를 사용하는 방법
- 문자열 연결 연산자를 이용하는 방법 → **암묵적 타입 변환**

```jsx
// String 생성자 > new 연산자 없이 호출하는 방법
// Number Type
String(1);
String(NaN);
String(Infinity);
String(-Infinity);
// Boolean Type
String(true);
String(false);

// Object.prototype.toString 메소드 사용하는 방법
// Number Type
(1).toString();
(NaN).toString();
(Infinity).toString();
(-Infinity).toString();
// Boolean Type
(true).toString();
(false).toString();
```

### 숫자 타입으로 변환

- Number 생성자 함수를 new 연산자 없이 호출하는 방법 (Built-In)
- parseInt(), parseFloat() 함수를 사용 → 문자열만 숫자 타입으로 변환 가능
- + 단항 산술 연산자를 사용 → 암묵적 타입 변환
- * 산술 연산자를 사용 → 암묵적 타입 변환

```jsx
// Number 생성자 -> new 연산자 없이 호출하는 방법
// String Type
Number('0');
Number('-1');
Number('10.05');
// Boolean Type
Number(true); // 1
Number(false); // 0

// parseInt(), parseFloat()
parseInt('0'); // 0
parseFloat('0'); // 0
parseInt('100.01'); // 100
parseFloat('3.14'); // 3.14
```

### 불리언 타입으로 변환

- Boolean 생성자 함수를 new 연산자 없이 호출하는 방법 (Built-In)
- ! 부정 논리 연산자를 두 번 사용하는 방법

```jsx
// Boolean 생성자 -> new 연산자 없이 호출하는 방법
// Number Type
Boolean(0); // false
Boolean(-0); // false
Boolean(1); // true
Boolean(-1); // true
Boolean(NaN); // false
Boolean(Infinity); // true
// String Type
Boolean(''); // false
Boolean('false'; // true
// null Type
Boolean(null); // false
// undefined Type
Boolean(undefined); // false
// Object Type
Boolean({}); // true
Boolean([]); // true

// ! 부정 논리 연산자를 두 번 사용하는 방법
!!0; // false
!!1; // true
!!''; // false
!!'false'; // true
```

## Built-In (표준 내장 객체)

- 자바스크립트 엔진에서 이미 포함하고 있는 Object 를 의미한다.
    - Built-In Operator
        - `++`, `—`, `+`, `-`, ...
    - Built-In Data Type
        - `Undefined`, `Null`, `Boolean`, `Object`, `Number`, `String`
    - Built-In Object
        - `Global`, `Object`, `Function`, `Array`, `Math`, `Date`, `JSON`, ...

# 단축 평가

## 논리 연산자를 사용한 단축 평가

- 논리합(`||`) 과 논리곱(`&&`) 의 평가 결과가 언제나 논리 값이 아닐 수 있다.
- 즉, 논리합과 논리곱 연산자 표현식은 언제나 2개의 피연산자 중 어느 한쪽으로 평가된다.
- 논리 연산의 결과를 결정하는 피연산자를 타입 변환하지 않고 그대로 반환하는 것을 **단축 평가(short-circuit evaluation)**라고 한다.
    - 논리곱 `(&&`)
        
        ```jsx
        // 논리곱 &&
        // 아래 조건문의 논리곱 결과는 true를 반환한다.
        if('true' && 'false') console.log('true!'); // print o
        
        // 좌항의 논리값이 true 이기 때문에 우항의 값이 할당된다.
        'true' && 'false'; // 'false'
        // 우항의 논리값이 false 라도, 좌항의 논리값이 true 이기 때문에 우항의 값 그대로 할당된다.
        'true' && false; // false
        // 좌항의 논리값이 false 라면, 논리곱 자체가 false 이기 때문에 좌항의 논리값이 할당된다.
        false && 'true'; // false
        // 단, 좌항의 값이 불리언 타입이 아니면서 Falsy 한 데이터 타입이라면, 해당 데이터 타입의 값 그대로 할당한다.
        '' && true; // ''
        NaN && true; // NaN
        undefined && true; // undefined
        null && true; // null
        false && true; // false
        ```
        
    - 논리합 (`||`)
        
        ```jsx
        // 논리합 ||
        // 아래 조건문의 논리합 결과는 true를 반환한다.
        if('true' || '') console.log('true!'); // 좌항 Truthy
        if ('' || 'true') console.log('true!'); // 우항 Truthy
        
        // 좌항의 논리값이 Truthy 하다면 좌항의 값 그대로 value 에 할당된다.
        'true' || 'false'; // 'true'
        // 좌항의 논리값이 Falsy, 우항의 논리값이 Truthy 하다면
        // 우항의 값 그대로 할당된다.
        '' || 'false'; // 'false'
        null || 'false'; // 'false'
        NaN || 'false'; // 'false'
        undefined || 'false'; // 'false'
        false || true; // true
        ```
        

### 객체를 가리키기를 기대하는 변수가 
null 또는 undefined 가 아닌지 확인하고 프로퍼티를 참조 할 때

- 객체는 key 와 value 로 구성된 프로퍼티의 집합인데, 이 때 변수의 값이 null 혹은 undefined 인 경우에 객체의 프로퍼티를 참조하면 Type Error 를 발생하면서 프로그램을 강제 종료된다.

```jsx
// null 혹은 undefined 인 경우 프로그램 강제 종료
let obj = null;
let value = obj.key; // TypeError : Cannot read property 'key' of null;

// 단축 평가를 이용해 강제 종료 예방
// obj 변수가 null 혹은 undefined 같은 Falsy 값이면 obj 자체로 평가되고
// obj 변수가 Truthy 값이면 obj.key 값으로 평가된다.
let obj = null;
let value = obj && obj.key; // null

obj = { key: 'value' };
value = obj && obj.key; // 'value'
```

### 함수 매개변수에 기본값을 설정 할 때

- 함수를 호출 할 때 인수를 전달하지 않으면 매개변수에 undefined 가 할당된다.
이 때, 단축 평가를 이용해 매개 변수의 기본값을 설정하면 undefined 로 인해 발생 할 수 있는 에러를 예방 할 수 있다.

```jsx
function getStrLength(str) {
	str = str || '';
	return str.length;
}

getStrLength(); // 0
getStrLength('hello'); // 5

// ES6 의 매개변수 기본값 설정
function getStrLength(str = '') {
	return str.length;
}

getStrLength(); // 0
getStrLength('wow!'); // 4
```

## **옵셔널 체이닝**

- ES11 에 도입된 옵셔널 체이닝 연산자 `?.` 는 좌항의 피연산자가 null 또는 undefined 인 경우 undefined 를 반환하고, 그렇지 않으면 우항의 프로퍼티를 참조한다.

```jsx
let obj = null;

let value = obj?.key;
console.log(value);
```

- 옵셔널 체이닝 연산자는 좌항 피연산자가 Falsy 값이더라도 null 또는 undefined가 아니면 우항의 프로퍼티 참조를 이어간다.

```jsx
let str = '';
console.log(str?.length); // 0

str = null;
console.log(str?.length); // undefined
```

## null 병합 연산자

- ES11 에 도입된 null 병합 연산자 (nullish  coalescing) 연산자 `??` 는 좌항의 피연산자가 null 또는 undefined 인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.

```jsx
console.log(null ?? 'default String'); // "default String"
console.log('' ?? 'default String'); // ""
```