# Data Type

- 자바스크립트의 데이터 타입에는  Primitive Values 와 Objects 가 존재한다.

### Primitive Values

<aside>
💡 자바스크립트의 원시 타입 데이터는 직접적으로 표현되는 불변의 데이터이다.

</aside>

- **Boolean**
    - 논리값을 나타내는 데이터로, true 혹은 false 를 가질 수 있다.
    - Primitive Type 과 Reference Type의 Boolean 이 존재한다.
- **Null**
    - 자바스크립트의 null은 의도적으로 값이 없다는 것을 명시하기 위한 Primitive Type 의 데이터다. (즉, null 은 Reference Type 이 아니다.)
    - null 인 데이터를 typeof 연산자를 통해 콘솔에 출력해보면 객체로 표시된다. (아래 코드)
    이는 초기 자바스크립트의 버그다.
    - typeof 구현 코드에서 Primitive Type 체크가 작성되어 있지만 null 은 제외되어 있기 때문에, 이에 대한 결과로 ‘object’ 가 출력된다.
    이를 고치지 않는 이유는 많은 웹 사이트에서 기존의 typeof 로 작성되어 있기 때문에 이를 수정할 경우 많은 문제를 야기 할 수 있기 때문이다.
    
    ```jsx
    let source = null;
    console.log(typeof source === 'object') // true
    ```
    
- **Undefined**
    - 정의되지 않았다는 의미를 가진다.
    - 식별자로 선언은 되었지만, 해당 식별자에 값이 할당되지 않은 변수는 undefined 를 출력한다.
    
    ```jsx
    // declaration
    let source;
    console.log(typeof source === 'undefined'); // true
    // array
    source = [];
    console.log(typeof source === 'undefined'); // true
    // object
    source = {};
    console.log(typeof source === 'undefined'); // true
    ```
    
- **Number**
    - 정수와 음수, 실수까지 표현하며, IEEE 64비트 754 값 사이의 숫자를 표현한다. (최대 2^52)
    - 해당 값의 범위를 넘어가는 경우 +infinty, -infinte 를 출력하며, 숫자가 아닌 값은 NaN(Not a Number) 를 출력한다.
    
    ```jsx
    // 최대값
    console.log(Number.MAX_VALUE); // 1.7976931348623157e+308
    // 최소값
    console.log(Number.MIN_VALUE); // 5e-324
    
    let num = Math.pow(2, 52);
    console.log(num); // O
    console.log(num + 1); // O
    
    console.log(42 / +0); // +infinty
    console.log(42 / -0); // -infinty
    ```
    
- **BigInt**
    - 최근에 추가된 숫자형 타입이다.
    - 자바스크립트가 연산 할 수 있는 값보다 더 큰 값을 다룰 수 있게 한다.
    - 정수 리터럴 끝에 n 을 붙이거나 BigInt 함수를 호출하면 BigInt 타입의 값을 만들 수 있다.
    
    ```jsx
    let num_1 = 999999999999999999999n;
    let num_2 = BigInt('999999999999999999999');
    let num_3 = BigInt(100); // 100n 과 동일
    
    	console.log(Number.MAX_SAFE_INTEGER);
    	console.log(Number.MIN_SAFE_INTEGER);
    ```
    
- **String**
    - 텍스트 데이터를 나타내기 위한 Primitive Type 데이터이다.
    - 16비트의 부호 없는 정수값이며, 문자열의 각 요소는 인덱스의 각 위치를 차지한다. 즉, 문자열의 첫 번째 요소의 인덱스 값은 0 이고, 이후의 문자열은 차례대로 인덱스 값을 가지게 된다.
    - 자바스크립트에서 문자열은 불변성(Immutable)이라는 특징을 가진다. 이는 메모리에 할당된 문자열을 수정 할 수 없다는 의미를 가지지만, 이를 참조하여 수정된 문자열을 새롭게 메모리에 할당하여 사용 할 수 있다.
    
    ```jsx
    let hello = 'Hello';
    let helloWorld = str + ' World';
    
    /*
    	hello와 helloWorld 식별자의 값은 각각 다른 메모리에 할당되어 있으며,
    	각 식별자의 문자열을 수정 할 경우, 해당 문자열 값 자체가 수정되는 것이 아닌
    	수정된 문자열을 새롭게 메모리에 할당하고, 각 식별자가 참조하고 있는 메모리 주소를
    	수정된 문자열이 할당된 메모리 주소로 변경한다.
    */
    ```
    
- **Symbol**
    - 자바스크립트는 객체 프로퍼티 키로 오직 String 과 Symbol 타입만을 허용하는데, 이러한 Symbol은 유일한 식별자를 만들고 싶을 때 사용한다.
    - Symbol 타입은 유일성이 보장되는 자료형이기 때문에, 설명이 동일한 Symbol 을 만들어도 두 Symbol 은 다른 객체이며, Symbol 의 설명은 해당 데이터에 어떠한 영향을 주지않고 이름표 역할만을 한다.
    - Java 의 Enum 처럼 사용 할 수 있다.
    
    ```jsx
    let symbol_1 = Symbol('symbol');
    let symbol_2 = Symbol('symbol');
    console.log(symbol_1 === symbol_2); // false
    ```
    
- **참고**
    - [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#boolean_type)
