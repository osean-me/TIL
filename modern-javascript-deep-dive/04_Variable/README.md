# Memory

- 메모리는 데이터를 저장할 수 있는 **메모리 셀(Memory Cell)** 의 집합체이다. 메모리 셀 하나의 크기는 1 byte (8 bit) 이며, 컴퓨터틑 메모리 셀의 크기, 즉 1 byte 단위로 데이터를 저장(write)하거나 읽어(read)들인다.
- 각 셀은 고유의 메모리 주소를 가지는데, 이 메모리 주소는 메모리 공간의 위치를 의미하며, 0부터 시작해서 메모리의 크기만큼 정수로 표시된다.
- **컴퓨터는 모든 데이터를 2진수로 처리한다.** 메모리에 저장되는 데이터는 데이터의 종류(숫자, 텍스트, 이미지, 동영상 등)와 상관없이 모두 2진수로 저장된다.
- 자바스크립트는 리터럴과 연산자를 통해 결과값이 도출되었을 때, 이 값을 개발자가 직접적으로 접근 할 수 없도록 제한한다.
    - **메모리 주소를 통해 값에 직접 접근하는 것은 시스템 상 오류를 일으킬 수 있다.**
- 프로그래밍 언어는 기억하고 싶은 값을 메모리에 저장하고, 저장된 값을 읽어 들여 재사용하기 위해서 변수 메커니즘을 이용한다. (Variable)

# Variable

- 변수는 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해 붙여진 이름을 의미한다.
- 이러한 **변수는 프로그래밍 언어의 컴파일러 또는 인터프리터에 의해 값이 저장된 메모리 공간의 주소로 치환되어 실행**된다.
그렇기 때문에 개발자가 직접 메모리 주소를 통해 값을 저장하고 참조할 필요가 없으며, 변수를 통해 안전하게 데이터 접근이 가능하다.
- 변수는 하나의 값을 저장하기 위한 메커니즘이며, 여러 개의 데이터를 저장하기 위해서는 그만큼의 변수를 선언해야한다. 하지만 배열이나 오브젝트 같은 자료구조를 사용하면 하나의 변수에 여러  개의 값을 저장 할 수 있다.

# Identifier

- 변수의 이름을 **식별자(Identifier)** 라고하는데, 식별자는 어떤 값을 구별해서 식별할 수 있는 고유한 이름을 말한다.
- 데이터는 메모리 공간에 저장되어 있기 때문에 식별자는 메모리 공간에 저장되어 있는 어떤 값을 구별해서 식별해 낼 수 있어야 한다. 때문에 **식별자는 메모리 주소를 기억하고 있다.**
- 즉, 식별자는 값이 저장되어 있는 메모리 주소와 매핑 관계를 맺으면서, 이때 맵핑된 정보도 메모리에 저장된다.
- 변수, 함수, 클래스 등의 이름은 모두 식별자로 칭한다.

# Variable Declaration

- 값을 저장하기 위한 메모리 공간을 확보(allocate) 하고, 변수 이름과 확보된 메모리 공간의 주소를 연결(name binding) 해서 값을 저장할 수 있게 준비하는 작업이다.
- **변수 선언에 의해 확보된 메모리 공간은 확보가 해제되기 전가지 누구도 확보된 메모리 공간을 사용할 수 없도록 보호된다.**
- 변수를 선언 할 때는 **var, let, const** 라는 키워드를 사용한다.

<aside>
var 키워드의 문제점
1. 블록 레벨 스코프를 지원하지 않고, 함수 레벨 스코프를 지원하기 때문에 의도치 않게 전역 변수가 선언되어 심각한 부작용 발생 할 수 있음.
2. var 키워드로 선언된 변수는 const 키워드와는 달리 어떤 영역에서든 값이 다른 값으로 초기화 될 수 있기 때문에, 의도치 않은 값이 도출 될 수 있다.
3. var Hosting 때문에 Global Scope에 변수 혹은 함수로 선언 할 경우 항상 위로 올라가버린다.

</aside>

- let, const 키워드는 ES6 버전부터 지원된 키워드이다.
- let 키워드는 선언된 스코프 내부에서만 호출된다. 그렇기 때문에 어떤 스코프에서도 값이 변할 수 있는 var 키워드보다 안전하게 사용 할 수 있다.

하지만 let 키워드는 특정 브라우저에서 호환이 되지 않는 경우가 있는데, 이는 Babel 을 통해 ES5, ES4 로 배포하는 것이 좋다. (IE)
- 변수 선언이 소스코드가 한 줄 씩 순차적으로 실행되는 시점, 즉 런타임이 아니라 그 이전 단계에서 먼저 실행(Variable Hoisting) 되어 메모리에 해당 변수가 할당된다.
따라서 source 변수는 출력코드보다 먼저 메모리에 할당되기 때문에, 참조에러가 발생하는 것이 아닌 undefined 를 출력하게 된다.
    
    ```jsx
    console.log(source); // undefined
    var source; // 변수 선언문
    ```
    
- 자바스크립트 엔진은 소스코드를 순차적으로 실행하기 전, 소스코드의 평가 과정을 거쳐 실행하기 위한 준비를 한다.
이러한 평가 과정에서 소스코드에 작성된 변수 선언을 포함한 모든 선언문을 소스코드에서 찾아내 먼저 실행한다.
그 이후, 모든 선언문을 제외하고 소스코드를 한 줄 씩 순차적으로 실행한다.

## Variable Hoisting

- 변수 선언문이 코드의 선두로 끌어 올려진 것처럼 동작하는 자바스크립트 고유의 특징을 의미한다.
- 변수 뿐만 아니라 let, const, function, function*, class 등의 키워드를 사용하는 모든 식별자는 호이스팅 과정을 거친다.
- 모든 선언문은 런타임 이전 단계에서 먼저 실행되기 때문이다.

# Assignment

- 값의 할당은 할당 연산자(=) 를 사용한다.
- 변수 선언은 소스코드가 순차적으로 실행되는 시점인 런타임 이전에 먼저 실행되지만 값의 할당은 소스코드가 순차적으로 실행되는 시점인 런타임에 실행된다.
    
    ```jsx
    // case 1
    // Variable Hoisting 에 의해 변수 선언문을 먼저 읽고 메모리에 올린다.
    console.log(score); // undefined
    
    var score = 'Hello World!'; // 값을 할당
    
    console.log(score); // Hello World 출력
    
    // case 2 > ReferenceError > source_2 를 선언한 변수가 없기 때문에
    console.log(score);
    
    score = 'Love myself';
    
    console.log(score);
    
    // case 3
    // Variable Hoisting 에 의해 가장 먼저 메모리에 올라간다.
    console.log(score); // 50 출력
    
    score = 100; // 100으로 초기화
    console.log(score); // 50 출력
    
    // Variable Hoisting 에 의해 가장 먼저 50으로 메모리에 할당되고,
    // 순차적으로 100으로 초기화된 후,
    // 다시 50으로 초기화됨
    var score = 50;
    
    console.log(score); // 50
    ```