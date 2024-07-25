### 타입 변환
`명시적 타입변환` or `타입 캐스팅` 은 개발자가 의도적으로 값의 타입을 변환하는 것

`암묵적 타입변환` or `타입 강제 변환`은 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 변환되는 것

두 가지 모두 원시 값을 직접 변경하는 것은 아니다.
`원시 값(primitive value)`은 변경 `불가능한 값(immutable value)`

> **🍴 암묵적 타입 변환 - 찍먹**<br>
기존 변수 값을 재할당하여 변경하는 것이 아님.<br>
자바스크립트 엔진은 표현식을 에러 없이 평가하기 위해 피연산자의 값을 암묵적 타입 변환해 새로운 타입의 값을 만들어 단 한 번 사용하고 버린다.

```js
var x = 10;

/**
 문자열 연결 연산자는 숫자 타입의 x의 값을 바탕으로 새로운 문자열을 생성한다.
x 변수의 숫자 값을 바탕으로 새로운 문자열 값 '10'을 생성
→ 그리고 이것으로 표현식 '10'+''을 평가.
→ But. x 변수의 값이 변경된 것은 아님.
*/
var str  = x + '';
console.log(typeof str, str) // string 10
console.log(typeof x, x) // number 10
```
---

### 암묵적 타입 변환
: 자바스크립트 엔진이 표현식을 평가할 때, 개발자의 의도와는 상관없이 `코드의 문맥을 고려해 암묵적으로` 데이터 타입을 강제 변환(암묵적 타입 변환) 하는 것.

**문자열 타입으로 변환**
`+` 연산자는 피연산자 중 하나 이상이 문자열이면 문자열 연결 연산자로 동작.

자바스크립트 엔진은 문자열 타입 아닌 값을 문자열 타입으로 암묵적 타입 변환을 수행할 때 다음과 같이 동작.

🎯 요약 : 문자열로 toString처럼 명시적이 아닌 암묵적 타입 변환이 가독성에 좋고 편하다면 `+''`을 사용하자. 
```js
// 숫자 타입
NaN + '';             // "NaN"
Infinity + ''         // "Infinity"

// null 타입
null + '';            // "null"

// undefined 타입
undefined + '';       // "undefined"

// 심벌 타입
(Symbol()) + '';      // TypeError: Cannot convert a Symbol value to a string

// 객체 타입
({}) + '';            // "[object Object]"
Math + '';            // "[object Math]"
[] + '';              // ""
[10, 20] + '';        // "10,20"
(function(){}_ + '';  // "function(){}"
Array + '';           // "function Array() { [native code] }"
```

**숫자 타입으로 변환**
```js
1 - '1' // 0
1 * '10' // 10
1 / 'one' // NaN

1 + +'1' // 2
1 + '1' // 11... 🙄 문자열로 처리해버림
```
자바스크립트 엔진은 산술 연산자 표현식을 평가하기 위해 산술 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환한다.
이때 피연산자를 숫자 타입으로 변환할 수 없는 경우 산술 연산 수행이 불가능 하여 결과값은 NaN이 된다.

문자열이 `+''`이라면 숫자는 `+`을 붙여서 타입을 변화해주면 된다.
```js
// 문자열 타입
+""; // 0
+"0"; // 0
+"1"; // 1
+"string"  // NaN
// 불리언 타입
+true; // 1
+false; // 0

// null 타입
+null; // 0

// undefined 타입
+undefined; // NaN

// 심벌 타입
+Symbol(); // TypeError: Cannot convert a Symbol value to a number

// 객체 타입
+{}; // NaN
+[]; // 0
+[10, 20]; // NaN
+function () {}; // NaN
```

빈 문자열(''), 빈 배열([]), null, false 는 0으로,
true는 1로 반환.

객체, 빈 배열이 아닌 배열, undefined는 반환되지 않아 NaN.<br>
🎯 Like This
```js
var score = 'test something'

let person = {
  name: "John Doe",
  age: 30,
  job: "Developer",
};
```
**불리언(Boolean) 타입으로 변환**
흔히 아는 불리언 타입`true`, `false`
falsy값만 조심하면 불리언 타입 변환은 비교적 안전하다.

**Falsy**값
- false
- undefined
- null
- 0, -0
- NaN
- ''(빈 문자열)

**Truthy**값
조건문에서 true로 평가되는 모든 값을 의미한다.(falsy값 제외 전부)
"hello"와 42는 truthy값.
```js
if("hello") console.log('Truthy Value')
if(42) console.log('Truthy Value2')
```

---
### 명시적 타입 변환
**문자열 타입으로 변환**

**숫자 타입으로 변환**

**불리언 타입으로 변환**

---
### 논리 연산자를 사용한 단축 평가
논리 연산의 결과를 결정하는 두 번째 피연산자, 즉 문자열 'Dog'을 그대로 반환.
결과적으로 두 번째 피연산자가 논리곱 연산자 표현식의 평가 결과를 결정하기 때문.
```js
'Cat' && 'Dog' // → 'Dog'
```

논리 연산의 결과를 결정한 첫 번째 피연산자, 즉 문자열 'Cat'을 그대로 반환.
```js
'Cat' || 'Dog' // → 'Cat'
```
단축 평가는 표현식을 평가하는 도중에 평가결과가 확정된 경우 → 나머지 평가 과정을 생략.

**단축 평가 규칙**
| 단축 평가 표현식  | 평가 결과 |
| ----------------- | --------- |
| true ll anything  | true      |
| false ll anything | anything  |
| true && anything  | anything  |
| false && anything | false     |

```js
// 논리합(||) 연산
"Cat" || "Dog"; // "Cat"
false || "Dog"; // "Dog"
"Cat" || false; // "Cat"

// 논리곱(&&) 연산
"Cat" && "Dog"; // "Dog"
false && "Dog"; // "false"
"Cat" && false; // "false"
```
논리곱(&&) 연산자_true평가 if문을 대체할 수 있다. → 가독성 좋음.
```js
var done = true;
var message = "";

// 조건문으로 값 할당
if (done) message = "Hmm";

// 논리 연산자(논리곱)으로 값 할당
meessage = done && "Awesome";
```

논리합(||) 연산자_false평가 if문을 대체할 수 있다.
```js
var done = false;
var message = "";

if(!done) message = '미완료';

message = done || '미완료';
```

**객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티 참조할 경우**<br>
: 객체를 가리키기를 기대하는 변수 값이 객체가 아닌 null 또는 undefined인 경우 객체의 프로퍼티를 참조하면 타입 에러 발생.<br> 
이런 경우 단축 평가를 사용해 예방할 수 있다.<br>
사실상 이렇게 처리하는 부분도 Optional Chaining을 사용하면 더 가독성 좋게 작성할 수 있음.
```js
var elem = null;
var value = elem && elem.value;
```
**함수 매개변수에 기본값을 설정할 때**
함수를 호출할 때 인수를 전달하지 않으면, 매개변수에는 undefined가 할당.
단축 평가를 사용해 매개변수 기본값을 설정하면 에러 방지 가능.
```js
// 인수를 전달하지 않을 경우
function getStringLength(str) {
  return str.length;
}
getStringLength(); // TypeError: Cannot read property 'length' of undefined

// 단축 평가를 사용한 매개변수의 기본값 설정
function getStringLength(str) {
  str = str || "";
  return str.length;
}
getStringLength(); // 0

// Es6의 매개변수 default parameter 설정
function getStringLength(str = "") {
  return str.length;
}
getStringLength(); // 0
```
---
### 옵셔널 체이닝 연산자
?. = 옵셔널 체이닝(optional chaining) 연산자
좌항의 피연산자가 null 또는 undefined인 경우 undefined반환.<br>
그렇지 않으면 우항의 프로퍼티 참조.

```js
var elem = null;
var value = elem?.value; // undefined
```

객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 안전하게 참조할 때 유용


**💡 논리곱(&&) 연산자 vs 옵셔널 체이닝 연산자**
```js
// 논리곱(&&) 연산자 = 좌항 피연산자가 Falsy값이면, 좌항 피연산자를 그대로 반환한다. (단, 0 또는 ''은 객체로 평가될 때도 있다.)
var str = ""; //
var length = str && str.length; // ''

// 옵셔널 체이닝 = 좌항 피연산자가 Falsy값이라도 null 또는 undefined 만 아니면, 우항의 프로퍼티를 참조한다.
var str = "";
var length = str?.length; // 0
```

### null 병합 연산자
?? = 병합 연산자(nullish coalescing) 연산자

좌항의 피연산자가 `null 또는 undefined` → 우항의 피연산자를 반환
그렇지 않은 경우 → 좌항의 프로퍼티를 참조.

변수에 기본값을 설정할 때 유용하다.

null 병합 연산자 ??는 좌항의 피연산자가 false로 평가되는 falsy값이라도 null 또는 undefined가 아니면 좌항의 피연산자를 그대로 반환한다.
```js
let value3 = NaN ?? "기본값";
console.log(value3); // NaN
```