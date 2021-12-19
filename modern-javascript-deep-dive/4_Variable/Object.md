# Objects

## Properties

- 자바스크립트에서 객체는 여러 속성(Properties)들을 담고 있는 그룹으로 볼 수 있다.
- 제한적이지만 객체 리터럴 문법을 통해 몇 가지 속성을 초기화하거나 속성을 추가, 제거 할 수 있으며, Properties 값은 객체를 포함하여 어떤 자료형도 될 수 있다.
- Properties 는 Key 값으로 식별되며, 이러한 **Key** 는 **String 혹은 Symbol Type** 만 사용된다.

### Data Property

<aside>
💡 Object.defineProperty() 를 이용하여 객체의 Key 를 설정하고, 해당 Key 의 Value 값을 연결함과 동시에 여러 특성들을 설정 할 수 있다.

</aside>

- **Value**
    - 설정하고자 하는 Key 의 Value 값을 설정한다.
    - 기본값은 undefined
- **Writable**
    - 해당 특성이 false 라면 해당 특성이 설정된 Key 값은 Value 값을 변경 할 수 없다.
    - 기본값은 false
- **Enumerable**
    - 해당 특성이 false 라면 해당 특성이 설정된 Key 값은 열거 시 제외된다.
    - 기본값은 false
- **Configurable**
    - 해당 특성이 false 라면, delete 연산자를 통해 해당 Key 를 삭제 할 수 없으며, Value 와 Writable 특성 외에는 변경 할 수 없다.
    - **Enumerable 특성의 영향을 받는다.**
    - 기본값은 false

```jsx
// Properties
let obj = {};

// Object.defineProperty() 를 통해 obj 의 Property 설정
Object.defineProperty(obj, 'name', {
    value: '심또익', // name 의 value 값 설정
    writable: true, // name 의 value 값 재설정 여부
    enumerable: true, // key 열거 여부
    configurable: true, // delete 연산자를 통해서 Property 허용 여부
});

// Property 추가
obj.sex = 'male'; // Literal Properties
obj['age'] = 27; // Computed Properties > 런타임 시점에서 결정될 때 사용된다.

/*
    Writable
        - true > name 속성의 value 수정 가능
        - false > name 속성의 value 수정 불가능
*/
obj.name = '심꼬망'; // Property 수정

/*
    Enumerable
        - true > name 속성 열거 가능
        - false > name 속성 열거 불가능
*/
for (let key in obj) {
    console.log(key + ' : ' + obj[key] );
}

/*
    Configurable
        - true : delete 를 이용해 속성 삭제 가능
        - false : delete 를 이용해 속성 삭제 불가능
        - Enumable 영향을 받으며, Value 와 Writable 특성 외에는 변경 할 수 없다.
*/
delete obj.name;
console.log(obj);
```

### Accessor

<aside>
💡 식별자에 객체를 정의 할 때 작성하며, 값을 가져오거나 값을 저장하기 위해서 사용된다.

</aside>

- **Get**
    - 함수처럼 정의되지만 엄연히 속성이다.
    - 해당 속성 호출 시, 정의된 함수가 함께 호출되고 함수의 return 값을 해당 속성값으로 가져온다.
- **Set**
    - 함수처럼 정의되지만 엄연히 속성이다.
    - 함수 호출처럼 파라미터로 데이터를 전달하여 속성값을 설정하는 것이 아닌, 재정의를 통해 해당 객체 생성 시 정의된 함수를 호출하여 함수 내부의 로직에 맞춰 객체의 값을 재설정한다.
- **Enumerable**
    - Data Property 와 동일
- **Configurable**
    - Data Property 와 동일

```jsx
let obj = {
    name: '심꼬망',
    age: 10,
    /**
     * key: getName
     * value: return
     */
    get getName() {
        return this.name;
    },
    /**
     * @param {{ name: string; age: number; }} person
     */
    set setPerson(person) {
        this.name = person.name;
        this.age = person.age;
    }
};

// Getter > 함수처럼 호출하지 않고, 속성 그대로 호출한다.
console.log(obj.getName);

// Setter > 함수처럼 파라미터 인자를 전달하지 않고, = 연산자를 통해 값을 재정의한다.
obj.setPerson = {
    name: '심또익',
    age: 20
};
console.log(obj);
```

## Arrays

- 배열은 정수키(index) 를 가지는 데이터의 집합 객체이다.
- 배열 객체는 length 속성이 포함되어 있어 해당 속성을 통해 배열의 Index 길이를 알 수 있다.
- `push()` 함수 혹은 리터럴 방식(`array[10] = ‘hello world’`)으로 새로운 값을 배열에 추가 할 수 있다.

```jsx
// 배열 객체 정의
let array = [];
// 'a' 데이터 추가
array.push('a');
// 'b' 데이터 추가
array[1] = 'b';
// 최종 데이터 출력
console.log(array); // ['a', 'b']
// 배열 길이 출력
console.log(array.length); // 2
```

## Typed Arrays

- 이진 데이터 버터 같은 뷰를 기술한다. (무슨 말이지?)
- ES5 부터 추가된 새로운 기능이며, 별도의 생성자를 가지고 있지 않기 때문에 다양한 전역 속성을 이용한다.

```jsx
Int8Array();
Uint8Array();
Uint8ClampedArray();
Int16Array();
Uint16Array();
Int32Array();
...
```

## Keyed Collections

### Sets

- Set 객체는 여러 Value 값을 가지는 데이터 집합이다.
- 프로그램 구동 중 Value 를 추가하거나 삭제, 변경 할 수 있지만 순서대로 저장되지 않기 때문에 인덱스를 이용해서 특정 위치의 Value 값을 조회 할 수 없다.
- Set 객체는 중복을 허용하지 않는다.
- Set 과 Array 간 컨버팅을 지원한다.

```jsx
// 생성자를 이용해 Set 객체를 식별자에 정의
let set = new Set();

set.add('hi');
set.add('안녕하세요');
set.add('おはよう');

set.has('hi'); // true
set.delete('안녕하세요'); // Set 객체 내에 '안녕하세요' 데이터 삭제
console.log(set.size); // 2

// Array to Set
set = new Set(['a', 'b', 'c']);

// Set to Array
let array = Array.from(set);
```

### Maps

- ECMA5 에 발표된 자료구조로,  Key / Value 의 구조를 가지며 데이터를 추가하거나 삭제, 변경이 가능하다.
- Key 값은 중복을 허용하지 않는다. 따라서, 이전에 동일한 Key 값이 Map 객체에 존재한다면 새로 저장하는 객체로 덮어 씌운다.
- **Object 객체와 차이점은?**
    - Object → 
    1. String, Symbol 타입만 Key 값으로 설정 할 수 있다.
    2. 데이터의 순서를 보장하지 않는다.
    - Maps → 
    1. 어떤 자료형이든 Key 값으로 설정 할 수 있다.
    2. 데이터의 순서를 보장한다.
    3. Object 의 인스턴스 객체이다.

```jsx
// 생성자를 이용해 Map 객체를 식별자에 정의
let map = new Map();

// Key: String, Value: Array
map.set('todo', ['장보기', '공과금 내기']);

// Key: Array, Value: String
map.set(['1'], 'array');

// map 출력
console.log(map);

// 특정 Key 의 Value 출력
console.log(map.get('todo')); // ['장보기', '공과금 내기']

// 특정 Key 삭제
map.delete('todo');
console.log(map); // ['1']: 'array' 

// 특정 Key 존재여부
console.log(map.has('todo')); // false

// Map 객체 초기화
map.clear();
console.log(map); // Map(0) {size: 0}
```

## Weak Collections

- Not Iterable
    - Weak Collectons 은 Key 와  Value 의 약한 결합을 이루고 있기 때문에 해당 객체가 참조되지 않을 경우 GC 에 의해 메모리 상에서 지워 질 수 있기 때문에 Iterable 하지 않고, 만약  Iterable 하더라고 이는 비 결정성을 야기 할 수 있다.
    - Map, Set 객체의 경우 Key, Value 의 강한 결합을 이루고 있어, 만약 많은 양의 메모리를 할당하고 있다면 해당 객체 내부에서 참조되지 않는 Key, Value 를 정리하기 위해서는 많은 비용을 소모해야 한다.
    **(메모리 누수)**

### WeakSets

- 전반적으로 Set 객체와 비슷한 구조를 가지며, 몇몇의 메소드는 제외된다.
- Set 객체와는 달리 WeakSet 객체는 Value 로 무조건 Object 를 가진다.
- 메모리 누수가 발생되지 않기 때문에 DOM 요소를 키로 저장 하거나,
추적을 위한 DOM 요소를 WeakSet 에 저장 할 수 있다.
- `new`, `has()`, `add()`, `delete()`

### WeakMaps

- 전반적으로 Map 객체와 비슷한 구조를 가지며, 몇몇의 메소드는 제외된다.
- Key 로 저장되는 값은 반드시 Object 여야 한다.
- `new`, `has()`, `get()`, `set()`, `delete()`

---

- 참고
    - [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures#objects)
    - [Mozila Hacks](http://hacks.mozilla.or.kr/2015/12/es6-in-depth-collections/)