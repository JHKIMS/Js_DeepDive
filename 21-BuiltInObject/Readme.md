# 빌트인 객체

### 📌 표준 빌트인 객체
Object, String, Number, Boolean, Symbol, Date, `Math`, RegExp, Array<br>
Map/Set, WeakMap/WeakSet, Function, Promise, `Reflect`, Proxy, `JSON`, Error<br>
등 40여 개의 표준 빌트인 객체를 제공.

Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.
생성자 함수 객체 : 프로토타입 메서드와 정적 메서드 제공.
생성자 함수가 아닌 객체 : 정적 메서드만 제공.

생성자 함수인 표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체다.


### 📌 원시값과 래퍼 객체

원시값은 객체가 아니므로 프로퍼티 or 메서드를 가질 수 없는데도 <br>
원시값인 문자열이 마치 객체처럼 동작.<br>
→ 마침표 표기법(or 대괄호 표기법)으로 접근하면 <br>
자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환시켜줌. 

**:: 이처럼 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 `래퍼 객체`라 한다.**
```js
const str = 'hello';

console.log(str.length);
console.log(str.toUpperCase());
```
문자열 래퍼 객체인 String 생성자 함수의 인스턴스는 String.prototype의 메서드를 상속받아 사용 가능.

래퍼 객체의 처리가 종료되면 래퍼 객체의 `[[StringData]]` 내부 슬롯에 할당된 원시값으로 원래의 상태로 되돌림.

래퍼 객체는 가비지 컬렉션 대상이 된다.
```js
const str = 'hello';

str.name = 'Lee';

console.log(str.name); // undefined

console.log(typeof str, str) // string hello
```
### 📌 전역 객체
어떤 객체에도 속하지 않은 최상위 객체이다.

브라우저 환경 - window(or self, this, frames)
Node.js 환경 - global

전역 객체는 표준 빌트인 객체, 호스트 객체, var 키워드로 선언한 전역 변수, 전역 함수를 프로퍼티로 갖는다.

전역 객체가 최상위 객체라는 것은 프로토타입 상속 관계상에서 최상위 객체라는 의미가 X.
어떤 객체의 프로퍼티도 아니며 객체의 계층적 구조상 표준 빌트인 객체와 호스트 객체를 프로퍼티로 소유한다는 것을 말함.

- 전역 객체는 개발자가 의도적으로 생성할 수 없다.(생성자 함수 제공X)
- 전역 객체의 프로퍼티를 참조할 때 window or global 생략 가능.

```js
window.parseInt('F', 16);
parent('F', 16); // window 생략 가능.
```
전역 객체는 Object, String, Number 같은 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
실행 환경(Node.js , 브라우저)에 따라 추가적으로 프로퍼티와 메서드를 가진다.

#### 빌트인 전역 프로퍼티
Infinity - 무한대를 나타내는 숫자값 Infinity를 갖는다.
NaN - 숫자가 아님을 나타내는 숫자값 NaN을 갖는다. Number.NaN 프로퍼티와 같다.
undefined - 원시 타입 undefined를 값으로 갖는다.
```js
console.log(3/0); // 양의 무한대 Infinity
console.log(-3/0); // 음의 무한대 Infinity

console.log(Number('xyz')); // NaN
console.log(1 * 'string'); // NaN

let foo;
console.log(foo);// undefined
```

#### 빌트인 전역 함수
❌ eval - 결론부터 말하면 쓰면 안됨. 보안, 성능 취약함, 자바스크립트 엔진 최적화X ❌

자바스크립트 코드를 나타내는 문자열을 인수로 받음.<br>
: 전달받은 문자열 코드가 표현식이면 eval은 문자열 코드를 런타임에 평가해 값을 생성.
표현식이 아니라 문이면 문자열 코드를 런타임에 실행.

**isFinite**<br>
전달받은 인수가 정상적인 유한수이면 true, 무한수이면 false 반환<br>
인수의 타입이 숫자가 아닌 경우, 수자로 타입 변환 후 검사 수행.
인수가 NaN으로 평가되는 값이라면 false 반환.
```js
isFinite(0);
isFinite('10');

isFinite(null);

isFinite(Infinity);

isFinite(NaN);
isFinite('Hello');
```

**isNaN**<br>
전달받은 인수가 NaN인지 검사하여 그 결과를 불리언 타입으로 반환.<br>
```js
isNaN(NaN) // true
isNaN(10); // false

isNaN('blabla'); // true
isNaN('10'); // false: '10' → 10
isNaN(''); // false: '' → 0
isNaN(' '); // false: ' ' → 0
```
**parseFloat**<br>
전달받은 문자열 인수를 부동 소수점 숫자, 즉 실수로 해석하여 반환.<br>
```js
parseFloat('3.14') // 3.14

// 공백으로 구분된 문자열은 첫 번째 문자열만 반환.
parseFloat('34 45 66') // 34

// 첫 번째 문자열을 숫자로 변환할 수 없다면 NaN을 반환.
parseFloat('He was 40'); // NaN

// 앞뒤 공백은 무시.
parseFloat(' 60 '); // 60
```
**parseInt**<br>
전달받은 문자열 인수를 정수로 해석하여 반환.

2번째 인수로 진법을 나타내는 기수 전달 가능.
→ 생략할 경우 10진수로 해석하여 반환.
→ 반환값은 언제나 10진수.
```js
// 문자열을 정수로 해석하여 반환.
parseInt('10') // 10

parseInt('10', 2) // 10을 2진수로 해석
```
기수를 지정하여 10진수 숫자 → 해당 기수의 문자열로 반환 필요 시 → Number.prototype.toString() 메서드 적용<br><br>
```js
const x = 15;

console.log(x.toString(2)); // '1111'
console.log(parseInt(x.toString(2), 2)); // 15

console.log(x.toString(8)); // '17'
console.log(parseInt(x.toString(8), 8)); // 15

console.log(x.toString(16)); // 'f'
console.log(parseInt(x.toString(16), 16)); // 15
```

**encodeURI / decodeURI**<br>
완전한 URI을 문자열로 전달받아 이스케이프 처리를 위해 인코딩한다.
URI는 인터넷에 있는 자원을 나타내는 유일한 주소.
![](https://velog.velcdn.com/images/next-react/post/087824ed-142f-4ab3-be51-6950ffecae93/image.png)



```js

```



#### ⛔️❌ 암묵적 전역
: 예기치 않은 동작을 초래하고, 디버깅이 어려우며, 코드의 가독성을 떨어뜨리기 때문에 조심해야함.

**❌BadThings❌**
```js
var x = 10;

function foo () {
    y = 20;
}
foo();

console.log(x+y); // 30
```
