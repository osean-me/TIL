# 객체 리터럴

## 객체란?

- 자바스크립트는 객체 기반 프로그래밍 언이이며, 자바스크립트를 구성하는 거의 모든 것이 객체로 이루어져있다. 원시 값을 제외한 나머지 값은 모두 객체이다. (함수, 배열, 정규 표현식 등)
- 원시 타입은 단 하나의 값만 나타내지만, **객체 타입은 다양한 타입의 값 을 하나의 단위로 구성한 복합적인 자료 구조이다.** 또한, 원시 타입은 불변한 값(immutable value)인데 반해 객체 타입은 가변한 값(mutable value)이다.
- 객체는 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티는 키와 값으로 구성된다.

```jsx
let obj = {}; // 객체 > 프로퍼티가 0개여도 객체이다.
let obj = { 
	key: 'value' 
}; // 객체
```

- 자바스크립트에서 사용 할 수 있는 모든 값은 프로퍼티 값이 될 수 있다. 자바스크립트의 함수는 일급 객체이므로 값으로 취급 할 수 있다.
    - ***일급 객체?
    > 다른 객체에 일반적으로 적용 가능한 연산을 모두 지원하는 객체를 가리킨다.
    > 변수에 할당(assignment) 할 수 있다.
    > 다른 함수를 인자(argument)로 전달 받는다.
    > 다른 함수의 결과로서 리턴 될 수 있다.***
    
- 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해서 메소드라고 부른다.
    - 프로퍼티 → 객체의 상태를 나타내는 값이다. (data)
    - 메소드 → 프로퍼티를 참조하고 조작 할 수 있는 동작(behavior)

```jsx
function getFunc() {
	...
} // 함수

let obj = {
	key: 'value', // 프로퍼티
	getMethod: () => {
		...
	} // 메소드
}
```

## 객체 리터럴에 의한 객체 생성

- 자바나 C++ 같은 클래스 기반 객체지향 언어는
    1. 사전에 클래스를 정의하고
    2. 해당 클래스가 필요한 시점에 new 연산자와 함께 생성자를 호출해서 인스턴스를 생성한다.
    - *여기서 클래스란 객체의 정의를 의미하고, 인스턴스는 클래스에 의해 생성되어 메모리에 저장된 실체를 말한다.*
- 자바스크립트는 프로토타입 기반의 객체지향 언어로서, new 연산자와 생성자로 인스턴스를 생성하는 방법 외에 다양한 방법을 제공한다.
    - 객체 리터럴
        
        ```jsx
        // 객체 리터럴
        let obj = {};
        let human = {
        	name: "심성헌",
        	age: 25
        };
        ```
        
    - Object 생성자 함수
        
        ```jsx
        // Object 생성자 함수
        let dog = new Object();
        dog.name = "또익";
        dog.sex = "male";
        ```
        
    - 클래스 및 생성자 함수 (ES6)
        
        ```jsx
        // 클래스 정의
        class Cat {
        	constructor(name, sex) {
        		this.name = name;
        		this.sex = sex;
        	}
        	
        	// 메소드
        	sayMeow() {
        		console.log(this.name + " : 야옹~");
        	}
        }
        
        // new 연산자와 Cat 클래스의 생성자 함수 호출
        let cheese = new Cat("나비", 1);
        cheese.sayMeow(); // 나비 : 야옹~
        ```
        
    - Object.create 함수
        
        ```jsx
        // Object.create(...)
        let frog = Object.create({});
        frog.name = "김개굴";
        frog.age = 10;
        console.log(frog) // { name: "김개굴", age: 10 }
        
        // 리터럴로 만들어진 객체를 넣으면 해당 객체 리터럴은 초기화 된다.
        let nemo = Object.create({ name: "니모" });
        nemo.sex = "male";
        console.log(nemo); // { sex: "male" }
        nemo.name = "니모";
        console.log(nemo); // { name: "니모", sex: "male" }
        
        ```
        
- 객체 리터럴은 중괄호 내에 **0개 이상의 프로퍼티를 정의**하며, 변수에 할당되는 시점에 자바스크립트 엔진은 객체 리터럴을 해석해 객체를 생성한다.
- 객체 리터럴의 중괄호는 코드 블록을 의미하지 않으며, 코드 블록의 닫는 중괄호 뒤에는 세미콜론을 붙이지 않는다. 하지만 **객체 리터럴은 값으로 평가되는 표현식**이기 때문에 중괄호가 닫히면 세미콜론을 붙인다.
- 객체 리터럴에 프로퍼티를 포함시켜 객체 생성과 동시에 프로퍼티를 만들 수 있고, 객체 생성 이후에 동적으로 프로퍼티를 추가하거나 삭제 할 수 있다.
    
    ```jsx
    // 객체 리터럴 내에 프로퍼티의 Key 와 Value 를 정의하므로써 객체 생성과 프로퍼티를 동시에 생성한다.
    let human = {
    	name: "심성헌",
    };
    console.log(human); // { name: "심성헌" }
    
    // 생성된 객체에 동적으로 프로퍼티를 추가 할 수 있다.
    human.age = 27;
    console.log(human); // { name: "심성헌", age: 27 }
    
    human["skills"] = ["javascript", "java", "spring", "mysql"];
    console.log(human); // { name: "심성헌", age: 27, skills: ["javascript", "java", "spring", "mysql"] }
    ```
    

## 프로퍼티

- **객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.**
- Key → 빈 문자열을 포함한 모든 문자열 또는 심볼 값
- Value → 자바스크립트에서 사용 할 수 있는 모든 값
    
    ```jsx
    let obj = {
    	name: "심성헌", // string
    	age: 10, // number
    	skills: [ // array
    		"javascript", 
    		"java", 
    		"spring", 
    		"mysql"
    	],
    	tmpObj: { // object
    		key: "key",
    		value: "value",
    		// 프로퍼티의 Key 값으로 숫자도 가능하지만, 결과적으로 숫자의 모양을 한 문자열이다.
    		1: "number",
    		// 프로퍼티의 Key는 Camel Case 를 따르며, 해당 케이스를 벗어난 값은 큰 따옴표, 작은 타옴표, 템플릿 리터럴로 감싸줘야 한다.
    		"special-key": "hello",
    	}
    };
    
    // 프로퍼티 Key 로 Symbol 을 사용할 때, 해당 Key 에 접근하기 위해서는 기준이 된 Symbol 을 통해 접근해야 한다.
    let newSymbol = Symbol('new-symbol');
    obj[newSymbol] = "Hello World";
    console.log(obj[newSymbol]); // "Hello World"
    
    console.log(obj); // {name: '심성헌', age: 10, skills: Array(4), tmpObj: {…}, Symbol(new-symbol): 'Hello World'}
    ```
    
- ES6 에 추가된 기능으로, 이미 생성된 객체를 객체 리터럴에 넣으면 해당 객체의 식별자 명과 해당 값이 각각 프로퍼티의 Key 와 Value 로 정의되어 객체가 생성된다.
    
    ```jsx
    let name = "심성헌";
    let age = 20;
    let human = {name, age};
    
    console.log(human); // {name: '심성헌', age: 20}
    ```
    
- 프로퍼티는 delete 연산자를 이용해서 동적으로 삭제 할 수 있다. 단, delete 연산자의 피연산자는 프로퍼티 값에 접근 할 수 있는 표현식이어야 한다. 존재하지 않는 프로퍼티를 삭제하면 에러없이 무시된다.
    
    ```jsx
    let dog = {
    	name: "꼬망",
    	sex: "male",
    };
    
    console.log(dog); // { name: "꼬망", sex: "male" }
    
    delete dog.sex;
    console.log(dog); // { name: "꼬망" }
    ```
    

### 메서드

- 자바스크립트에서 사용 할 수 있는 모든 값은 프로퍼티의 Value 로 사용 할 수 있다. 즉, 자바스크립트의 함수 또한 **일급 객체**이기 때문에 프로퍼티의 Value 로 사용 가능하다.
- 다만 일반 함수와 구분하기 위해서 프로퍼티에 사용된 함수는 **메소드**라고 칭한다.

```jsx
// 함수
function sayHello() {
	console.log("Hello~");
}

let cat = {
	sayHello: () => {
		console.log("meow~");
	},
};

sayHello(); // Hello~ > 함수 호출
cat.sayHello(); // meow~ 메소드 호출
```