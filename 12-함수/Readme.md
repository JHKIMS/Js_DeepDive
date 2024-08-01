# 함수

일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것.

프로그래밍 언어의 함수도 입력을 받아서 출력을 내보냄.

함수 내부로 입력을 전달받는 변수를 매개변수, 출력을 반환값, 입력을 인수.
```js
/**
    x, y : 매개변수
    x+y : 반환값
    2,5 : 인수
    --함수 정의--
*/
function add(x, y){  // 함수 정의
  return x+y;
}

add(2,5); // 함수 호출
```

### 함수를 사용하는 이유
`코드의 재사용`
실행 시점을 개발자가 결정 가능.
함수는 몇 번이든 호출 가능.

`유지보수 편의성과 코드의 신뢰성`
코드의 중복을 억제하고 재사용성을 높이는 함수.

`함수 네이밍 중요성`
적절한 함수 이름은 함수 내부 코드를 이해하지 않고도 함수의 역할을 파악할 수 있다.<br>
→ 코드의 가독성이 좋아짐.

### 함수 리터럴
🙄 리터럴 짚고 넘어가기.
: 개발자 컴퓨터 간의 약속.
사람이 이해할 수 있는 문자 또는 약속된 기호를 사용해 값을 생성하는 표기 방식.

JavaScript 함수는 객체 타입의 값.
함수 리터럴은 function 키워드, 함수 이름, 매개변수 목록, 함수 몸체로 구성된다.
```js
var f = function add(x,y){
  return x+y;
}
```
함수 리터럴도 평가되어 값을 생성.<br>
→ 이 값은 객체이며, 결국 함수는 객체.

**👉 But.**
일반 객체와는 다르다.
일반 객체는 호출 X,
함수는 호출 O.

### 함수 정의
함수를 호출하기 이전에 인수를 전달받을 매개변수와 실행할 문들, 그리고 반환할 값을 지정하는 것.

**함수 선언문**
```js
function add(x,y){
  return x+y;
}
```

함수 리터럴은 이름을 생략할 수 있지만, 함수 선언문은 함수 이름을 생략할 수 없다.

함수 선언문은 표현식이 아닌 문이다.<br>
함수 선언문이 만약 표현식이라면 완료 값 undefined 대신 표현식이 평가되어 생성된 함수가 출력되어야함.

```js
function add(x,y){
  return x+y;
}
// → undefined
```

함수 선언문은 함수 이름을 생략할 수 없다는 점을 제외하면 함수 리터럴과 형태가 동일하다.

기명 함수 리터럴은 중의적인 코드.
함수 이름이 있는 함수 리터럴을 단독으로 사용하면 함수 선언문으로 해석.

함수 리터럴을 변수에 할당하거나 피연산자로 사용하면 함수 리터럴 표현식으로 해석.

두 가지다 함수가 생성되는 것은 동일.
함수를 생성하는 내부 동작은 다음과 같은 차이가 있다.
```js
function foo(){ console.log('foo'); }
foo();

(function bar() { console.log('bar');});
bar(); // Reference Error 발생.
```

함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자이다.
`foo`는 함수 선언문으로 해석.

`bar`는 함수 리터럴 표현식으로 해석.<br>
→ 문맥에 따라, 그룹 연산자의 피연산자는 값으로 평가될 수 있는 표현식이 되어야 한다.

foo(👉 함수 선언문)는 호출이 가능하고 , bar(👉 함수 표현식)가 호출이 불가능한 이유

함수 리터럴에서 함수 이름은 함수 몸체 내에서만 참조할 수 있는 식별자.<br>
함수 몸체 외부에서는 함수 이름으로 함수를 참조할 수 없으므로,
함수 몸체 외부에서는 함수 이름으로 함수를 호출할 수 없다는 의미.
![](https://velog.velcdn.com/images/next-react/post/e0330c2d-c375-4cc1-9d53-9e1cf27c0648/image.png)


위 예제에서는 foo를 선언한 적도 없고 할당한 적도 없다.<br>
그럼 뭐지? → foo는 자바스크립트 엔진이 암묵적으로 생성한 식별자이다.<br>
자바스크립트 엔진은 생성된 함수를 호출하기 위해 `함수 이름과 동일한 이름의 식별자를 암묵적으로 생성`하고,
거기에 함수 객체를 할당한다.<br>
![](https://velog.velcdn.com/images/next-react/post/2e1a55d2-9e05-4f64-b382-3d206a948269/image.png)

함수는 함수 이름으로 호출하는 것이 아닌 함수 객체를 가리키는 식별자로 호출.<br>
![](https://velog.velcdn.com/images/next-react/post/e2d00750-7764-4880-b6f9-9650b49e50f6/image.png)

**함수 표현식**<br>
자바스크립트의 함수는 값처럼 변수에 할당할 수도 있고, 프로퍼티 값이 될 수도 있으며 배열의 요소가 될 수도 있다.<br>
이처럼 값의 성질을 갖는 객체를 일급 객체라고 한다.<br>
→ 자바스크립트의 함수는 일급 객체이다.(함수를 값처럼 자유롭게 사용 가능)

함수 리터럴로 생성한 함수 객체를 변수에 할당할 수 있는데,<br>
이러한 함수 정의 방식을 `함수 표현식`이라고 한다.
```js
var add = function(x,y){
  return x+y;
}

console.log(add(2,5));
```
함수 리터럴의 함수 이름은 생략이 가능하다.<br>
함수 표현식의 함수 리터럴은 함수 이름을 생략하는 것이 일반적이다.

함수를 호출할 때는 함수 이름이 아닌 `함수 객체를 가리키는 식별자를 사용`...함수 이름은 함수 몸체 내부에서만 유효.
```js
var add = function foo(x,y){
  return x+y;
}
console.log(add(2,5)); // OK.
console.log(foo(2,5)); // ReferenceError발생.
```
함수 선언문은 `표현식이 아닌 문`이고 함수 표현식은 `표현식인 문`

> **🤖 GPT**
> - Q. 함수 리터럴 같은 경우 보통 변수에 할당하기 때문에 함수 이름을 생략하는 것이 좋을까?<br>
> 
> - 일반적인 경우 - 많은 콜백 함수와 이벤트 핸들러에 널리 사용됨.<br>
이름을 붙이는 경우 - 재귀 호출이 필요한 경우.<br>
> 
> 정리 <br>
> : 익명 함수는 일반적으로 간결하고 콜백 함수 or 일회성 함수에 적합.<br>
> : 명명된 함수 표현식은 디버깅이 용이하고, 재귀 호출이 필요한 경우에 유용.<br>
> : 함수 선언은 함수가 코드의 어디서든 호출될 수 있게 하려는 경우에 적합.

**함수 생성 시점과 함수 호이스팅**
함수 선언문 또한 코드의 선두로 끌어 올려진 것처럼 동작한다.<br>
→ 즉, 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행된다.

> **⛔️ 변수 호이스팅과 미묘한 차이가 있다.**
: var 키워드로 선언된 변수는 undefined로 초기화.<br>
함수 선언문을 통해 암묵적으로 생성된 식별자는 함수 객체로 초기화.<br>
var 키워드를 사용한 변수 선언문 이전에 변수를 참조하면 변수 호이스팅에 의해 undefined로 평가.<br>
But 함수 선언문으로 정의한 함수를 함수 선언문 이전에 호출하면 함수 호이스팅에 의해 호출 가능.

함수 선언문 보다 **⭐️함수 표현식⭐️** 사용을 권장한다.

### 함수 호출
**매개변수와 인수**
함수 외부에서 내부로 값을 전달할 필요가 있는 경우.

매개변수를 통해 인수를 전달한다.<br>
매개변수는 함수를 정의할 때 선언.<br>
인수는 함수를 호출할 때 지정.<br>
개수와 타입 제한X
![](https://velog.velcdn.com/images/next-react/post/109b703f-8a13-474e-8bdd-329412a07bf6/image.png)

매개 변수는 함수 내부에서만 참조 가능, 함수 외부에서 참조 X

✅ 매개변수의 개수와 인수의 개수가 일치하는지 체크하지 않음.<br>
매개변수의 수만큼 인수가 전달되지 않은 경우, 인수가 할당되지 않은 매개변수의 값은 undefined이다.

```js
function add(x,y){
  return x+y;
}
console.log(add(2)); // 2+undefined = NaN
```

**인수 확인**
```js
function add(x,y){
  return x+y;
}

console.log(add(2)); // NaN
console.log(add('a','b')); // ab
```

자바스크립트 함수는 매개변수와 인수의 개수가 일치하는지 확인X.<br>
동적 타입 언어 특성상 함수 매개변수의 타입을 사전에 지정할 수 없다.

자바스크립트는 함수를 정의할 때 적절한 인수가 전달되었는지 확인할 필요가 있다.
```js
function add(x,y){
  if(typeof x !== 'number' || typeof y !== 'number'){
    throw new TypeError('인수는 모두 숫자 값');
  }
  return x+y;
}
console.log(add(2)); // TypeError: 인수는 모두 숫자 값
console.log(add('a','b')); // TypeError: 인수는 모두 숫자 값
```

인수가 전달되지 않은 경우 단축 평가를 사용해 매개변수에 기본값 할당
```js
function add(a,b,c){
  a = a || 0;
  b = b || 0;
  c = c || 0;
  return a+b+c;
}
```

ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화 할 수 있음.
매개변수 전달하지 않은 경우 + undefined인 경우 기본값 할당.
```js
function add(a = 0, b = 0, c = 0){
  return a+b+c;
}
```

**매개변수의 최대 개수**
매개변수는 최대 3개 이상을 넘지 않는 것이 좋고,<br> 
만약 그 이상의 매개변수가 필요하다면 하나의 매개변수를 선언하고 객체를 인수로 전달하는 것이 좋다.

**❌Bad❌**
```js
function updateUser(id, name, email, age) {
  console.log(`ID: ${id}`);
  console.log(`Name: ${name}`);
  console.log(`Email: ${email}`);
  console.log(`Age: ${age}`);
}

updateUser(1, 'John Doe', 'john@example.com', 30);
```

**👍 Good**<br>![](https://velog.velcdn.com/images/next-react/post/70fced0d-0f8a-4a40-9ff9-34a2edc8c599/image.png)

**반환문**
함수는 return 키워드와 표현식으로 이뤄진 반환문을 사용해 실행 결과를 함수 외부로 반환할 수 있다.

함수 호출은 표현식.
함수 호출 표현식은 return 키워드가 반환한 표현식의 평가 결과.
즉, 반환값(return)으로 평가된다.

1번째 역할 - 함수의 실행을 중단하고 함수 몸체를 빠져나감.<br>
2번째 역할 - 반환문은 return 키워드 뒤에 오는 표현식을 평가해 반환.<br>
반환값으로 사용할 표현식을 명시적으로 지정하지 않으면 undefined 반환.

```js
function multiplu(x,y){
  return x+y;
}

var result = multiply(3, 5);
console.log(result); // 8;
```
```js
function foo() {
  return ;
}
console.log(foo()); // undefined
```
```js
function foo(){
}
console.log(foo()); // undefined
```

### 참조에 의한 전달과 외부 상태의 변경
`call by value`
값에 의한 호출, 전달.
함수 호출시 매개변수에 원시 값(primitive value)을 전달. - ex. primitive

`call by reference`
참조에 의한 호출, 전달.
함수 호출시 매개변수에 객체(object)을 전달. - ex. object

```js
function changeVal(primitive, obj){
     primitive += 100;
     obj.name = 'Kim';
     console.log('함수 :',primitive, obj.name) // 200, Kim
}

var num = 100;
var person = { name:'Lee' };

changeVal(num, person);

// 원시 값은 원본이 훼손X - 100
console.log(num); 

// 객체는 원본이 훼손된다. - {name: "Kim"}
console.log(person);
```

primitive
외부 상태, 즉 함수 외부에서 함수 몸체 내부로 전달한 원시 값의 원본을 변경하는 어떠한 부수 효과 발생하지 X

obj
참조 값이 복사되어 매개변수에 전달.<br>
함수 몸체에서 참조 값을 통해 객체를 변경할 경우 `원본이 훼손.`<br>
외부상태 - 함수 외부에서 함수 몸체 내부로 전달한 참조 값에 의해 `원본 객체가 변경되는 부수 효과 발생.`

![](https://velog.velcdn.com/images/next-react/post/f1f1eabc-888d-4cf9-b077-d0a27c22cb8e/image.png)

해결 방법
객체를 불변 객체(immutable object)로 만들어 사용.
- 객체의 복사본을 새롭게 생성하는 것은 비용(cost)이 원본 객체 규모에 따라 커질 수 있다.
- 하지만, `객체`를 마치 `원시 값처럼 변경 불가능한 값`으로 동작하게 만들 수 있다.
- 방법으로는 `깊은 복사(deep copy)` 를 통해 새로운 객체를 생성하고 재할당한다. → side effect X



### 다양한 함수의 형태
**즉시 실행 함수**
: 함수 정의와 동시에 즉시 호출되는 함수.<br>
단 한 번만 호출되며 다시 호출할 수 없다.

즉시 실행 함수는 `반드시 그룹 연산자(...)로 감싸야 한다.`
즉시 실행 함수는 `함수 이름이 없는 익명 함수를 사용하는 것이  일반적이다.`

```js
(function (){
    ///...
})();
```

**재귀 함수**
: 재귀 호출을 수행하는 함수.<br>
재귀 호출 → 함수가 자기 자신을 호출하는 것.

10부터 0까지 출력하는 함수 - 재귀함수 없이.
```js
function countdown(n){
  for(var i = n; i>=0; i--) console.log(i);
}

countdown(10);
```
**재귀함수를 이용하여(ft.반복문 없이)**<br>
![](https://velog.velcdn.com/images/next-react/post/8edf8ccd-bdf3-4d35-be20-5dcf2a5c9563/image.png)

**콜백 함수**
: 어떤 일을 반복 수행하는 repeat 함수를 정의.

```js
function repeat(n){
  for(var i=0; i<n; i++) console.log(i);
}

repeat(5); // 0 1 2 3 4

function repeat2(n){
  for(var i=0; i<n; i++){
    if(i%2) console.log(i);
  }
}

repeat2(5); // 1 3
```
repeat 함수는 console.log(i)에 강하게 의존하고 있어 다른 일을 할 수 없다.<br>
→ repeat 함수의 반복문 내부에서 다른 일을 하고 싶다면 함수를 새롭게 정의.(Like `repeat2`)

해결 방법 - 함수 <br>
함수의 변하지 않는 공통 로직은 미리 정의.<br>
경우에 따라 변경되는 로직은 추상화해서 함수 외부에서 함수 내부로 전달.

![](https://velog.velcdn.com/images/next-react/post/5acf8b09-9d1e-4a38-9e59-ad9f7e5a929c/image.png)<br>
repeat 함수는 경우에 따라 변경되는 일을 함수 f로 추상화하고 이를 외부에서 전달받음.<br>
로직의 일부분을 함수로 전달받아 수행하므로 더욱 유연한 구조를 가짐.

함수의 매개변수를 통해 다른 함수의 내부로 전달되는 함수를 `콜백 함수`라고 한다.

매개변수를 통해 함수의 외부에서 콜백 함수를 전달받은 함수를 `고차 함수`라고 한다.

고차 함수는 매개변수를 통해 전달받은 콜백 함수의 호출 시점을 결정해서 호출한다.

콜백 함수는 고차 함수에 의해 호출되며 이때 고차 함수는 필요에 따라 콜백 함수에 인수를 전달할 수 있다.


**순수 함수와 비순수 함수**<br>
순수 함수 - 부수 효과가 없는 함수(외부 상태 의존X, 변경X)<br>
비순수 함수 - 부수 효과가 있는 함수(외부 상태 의존O, 변경O)

**👍 순수 함수**
```js
function add(a, b) {
  return a + b;
}

// 호출 예시
console.log(add(2, 3)); // 항상 5를 출력
console.log(add(2, 3)); // 항상 5를 출력
console.log(add(2, 3)); // 항상 5를 출력
```

**🚫 비순수 함수**
: 코드의 복잡성을 증가시킨다.
비순수 함수를 최대한 줄이는 것이 부수 효과를 최대한 억제하는 것.<br>

```js
let count = 0;

function increment() {
    count += 1;
    return count;
}

// 호출 예시
console.log(increment()); // 1을 출력
console.log(increment()); // 2를 출력
console.log(increment()); // 3을 출력
```

함수형 프로그래밍의 방향<br>
순수 함수와 보조 함수의 조합을 통해 외부 상태를 변경하는 부수 효과를 최소화해서 불변성을 지향하는 프로그래밍 패러다임.
