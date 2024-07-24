# 중요 연산자

### 비교 연산자
**==**(동등 비교)
: 좌항과 우항의 피연산자를 비교할 때 먼저 암묵적 타입 변환을 통해 타입을 일치시킨 후 같은 값인지 비교한다.

🙄 결과를 예측하는 것이 어렵다.
```js
'0' == '' // false
0 == '' // true
0 == '0' // true
false == '0' // true
```

`동등 비교(==) 연산자`는 예측하기 어려운 결과를 만들어낸다.<br>
동등 비교 연산자보다는 `일치 비교(===) 연산자`를 사용하는 것이 좋다.

**===**(일치 비교)
: 좌항과 우항의 피연산자가 타입도 같고, 값도 같은 경우에 한하여 true을 반환한다.<br>
암묵적 타입 변환을 하지 않는다.

⛔️ 일치 비교 연산자에서 주의할 것은 **NaN**.
**NaN**은 자신과 일치하지 않는 유일한 값.
```js
NaN === NaN; // false
```
**NaN**인지 조사하려면 빌트인 함수 Number.isNaN을 사용해야 한다.
```js
Number.isNaN(NaN); // true
Number.isNaN(10); // false
Number.isNaN(1+undefined); // true
```

**0, -0** 도 주의해야 한다.
이 2개를 비교하면 true을 반환한다.

💡 Object.is 메서드
ES6에서 도입된 Object.is 메서드는 예측 가능한 정확한 비교 결과를 반환하고, 그 외에는 === 과 동일하게 동작한다.
```js
+0 === -0; // true
Object.is(-0, +0); //false

NaN === NaN; // false
Object.is(NaN, NaN) // true
```

삼항연산자와 조건문
조건에 따라 어떤 값을 결정 - `삼항 연산자 표현식`<br>
조건에 따라 수행해야 할 문이 하나가 아니라 여러개다 - `if~else문`

기본 삼항 연산자
: score가 30이면 A, 그렇지 않으면 B
```js
const test = score === 30 ? A : B;
```
중첩 삼항 연산자
: score가 30일 때 A가 20인지 확인하고<br>
그러면 test에 C가 할당됨.<br>
그게 아니라면 test에 B가 할당됨.<br>
score가 30이 아니라면 그냥 바로 test에 B가 할당됨.<br>
...중첩은 가급적이면 사용하지 않는 것이 좋다.
이럴거면 그냥 `if~else문`으로...
```js
const test = score === 30? (A === 20 ? C : B) : B;
```

### typeof 연산자
피연산자의 데이터 타입을 문자열로 반환.(7가지 문자열)
`string`, `number`, `boolean`, `undefined`, `symbol`, `object`, `function`

🙄 `null`을 반환하는 경우는 없다.

> **⛔️ 자바스크립트의 버그**<br>
    ```js
    typeof null // object
    ```<br>
    null이 아닌 object을 반환한다.(자바스크립트의 버그)
    <br>
    null타입인지 확인할 때는 typeof가 아닌 일치 연산자(===)을 사용하자.<br>
    ```js
    const foo = null;
    ```<br>
    ```js 
    typeof foo === null; // →false
    ```<br>
    ```js
    foo === null; // →true
    ```

선언하지 않은 식별자를 typeof 연산자로 연산하면 ReferenceError가 아닌 `undefined`을 반환.
```js
typeof nothingdd; // →undefined
```

### 지수 연산자 (ES7에서 도입)
지수 연산자가 도입되기 이전에는 Math.pow을 사용.
```js
Math.pow(2, 2); // 4
Math.pow(2, 0); // 1
Math.pow(2, -2); // 0.25
```

좌항의 피연산자를 밑으로, 우항의 피연산자를 지수로 거듭 제곱하여 숫자 값을 반환.
```js
2 ** 2; // 4
2 ** 8; // 256
2 ** -2; // 0.25
```

음수를 거듭제곱의 밑으로 사용하려면 괄호로 묶어서 사용해야 한다.
```js
-5 ** 2;  // SyntaxError: Unary operator used immediately before exponentiation expression.
(-5) ** 2;  // 25
```

### 연산자의 부수 효과
대부분의 연산자는 다른 코드에 영향을 주지 않는다.

**증가/감소(++/--), 할당(=), delete 연산자**
이 3개의 연산자는 다른 코드에 영향을 주는 부수 효과가 있다.

**증가,감소(++/--) 예시**
```js
let a = 5;
console.log(++a); // 6 : a의 값을 먼저 증가시키고 그 값 반환.
console.log(a); // a의 값은 6으로 변경됨.

let b = 5;
console.log(c++); // 5, c의 현재 값을 반환하고 그 다음에 c값을 증가시킴.
console.log(c); // 6, c의 값이 6으로 증가함.

let count = 0;

function incrementAndPrint() {
  console.log(++count);
}

incrementAndPrint(); // 1
incrementAndPrint(); // 2
incrementAndPrint(); // 3
```

**할당(=) 예시**
```js
let globalVar = 1;

function modifyGlobalVar() {
  globalVar = 2; // 전역 변수를 변경하는 부수 효과
}

console.log(globalVar); // 1 : 함수 실행 전
modifyGlobalVar();
console.log(globalVar); // 2 : 함수 실행 후

let obj = { property: 10 };

function modifyObject(obj) {
  obj.property = 20; // 객체의 속성을 변경하는 부수 효과
}

console.log(obj.property); // 10
modifyObject(obj);
console.log(obj.property); // 20
```

**delete 예시**
```js
let person = {
  name: "DD",
  age: 30,
  job: "Front"
};

console.log(person); // { name: 'DD', age: 30, job: 'Front' }

delete person.job; // 객체의 job 속성을 삭제

console.log(person); // { name: 'DD', age: 30 }

console.log("job" in person); // false, job 속성이 삭제되었음을 확인
```