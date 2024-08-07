### 📌 19.11 직접 상속
#### Object.create에 의한 직접 상속
Object.create 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성한다.<br>
다른 객체 생성과 마찬가지로 `OrdinaryObjectCreate` 호출.<br>

첫 번째 매개변수 - 생성할 객체의 프로토타입으로 지정할 객체 전달.<br>
두 번째 매개변수(옵션) - 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체 전달.<br>

Object.create 메소드는 첫 번째 매개변수에 전달한 객체의 프로토타입 체인에 속하는 객체를 생성.<br>
→ 객체를 생성하면서 직접적으로 상속을 구현한다.<br>

이 메서드의 장점 3가지.
- new연산자가 없이도 객체 생성 가능.
- 프로토타입을 지정하면서 객체 생성 가능.
- 객체 리터럴에 의해 생성된 객체 상속받을 수 있음.

```js
// 프로토타입이 null인 객체를 생성. - 어떤 것도 부모로 가지지 않는 객체.
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null); // true

// Object.prototype을 부모로 가지는 객체.
obj = Object.create(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype);// true

obj = Object.create(Object.prototype, {
	x: { value: 1, writable: true, enumerable: true, configurable: true },
})
console.log(Object.getPrototypeOf(obj) === Object.prototype);// true

const myProto = { x: 10 };
obj = Object.create(myProto);
console.log(Object.getPrototypeOf(obj) === myProto);// true

function Person(name){
	this.name = name;
}

// obj = new Person('Lee')
obj = Object.create(Person.prototype);
console.log(Object.getPrototypeOf(obj) === Person.prototype); // true
```

**⛔️ ESLint에서는 Object.prototype의 빌트인 메서드를 객체가 직접 호출하는 것 권장X**<br>
다음 코드처럼 사용하는 것 권장X
```js
const obj = { a: 1 }

obj.hasOwnProperty('a');
```
모든 객체의 종점에는 언제나 `Object.prototype`이 존재한다는 점을 고려했을 때,<br>
직접 프로토타입 체인의 종점에 위치하는 객체를 생성하는 것은 안 좋은 방법이다.

**👍 간접적으로 다음과 같이 호출하는 것이 바람직하다.**
```js
const obj = Object.create(null);
obj.a = 1

console.log(Object.prototype.hasOwnProperty.call(obj, 'a')); // true
```


### 📌 19.12 정적 프로퍼티/메서드

생성자 함수로 `인스턴스를 생성하지 않아도 참조/호출 가능한` 프로퍼티/메서드.

```js
function Person(name){
	this.name = name;
};
Person.prototype.sayHello = function () {
	console.log(`Hi! My name is ${this.name}`);
};
Person.staticProp = `static prop`;

Person.staticMethod = function () {
	console.log('staticMethod');
}

const me = new Person('Kim');
Person.staticMethod();

// TypeError 발생.
me.staticMethod();
```
- 생성자 함수도 `객체`<br>
- 생성자 함수가 소유한 프로퍼티나 메서드를 정적 프로퍼티/메서드라고 함.<br>
  → 생성자 함수가 소유하고 있는 정적 프로퍼티/메서드는 `인스턴스에서 직접 참조/호출 불가능`
  : 코드의TypeError부분 `me.staticMethod()` 참고.<br>
  
![](https://velog.velcdn.com/images/next-react/post/f6cf5bb2-a27d-485b-b896-9fb939bc80fb/image.png)

### 📌 19.13 프로퍼티 존재 확인

#### in 연산자
객체 내에 특정 프로퍼티가 존재하는지 여부를 확인한다. → `boolean`
```js
/**
* key: 프로퍼티 키를 나타내는 문자열.
* object: 객체로 평가되는 표현식.
*/
key in Object
```

in 연산자는 확인 대상 객체의 프로퍼티뿐 아니라 `확인 대상 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인하므로 주의.`
```js
const Person = {
	name: 'Lee',
	address: 'Seoul'
}
/**
* 'name' in person - true
* 'age' in person - true
* 'toString' in person - true
*/
```
toString true가 나오는 이유.<br>
: in 연산자가 person 객체에 속한 프로토타입 체인 상에 존재하는 모든 프로토타입에서<br>
toString으로 프로퍼티를 검색했기 때문.

#### Reflect.has (ES6 도입)
in 연산자와 동일하게 동작한다.
```js
const person = {name: 'Lee'};

console.log(Reflect.has(person, 'name'));
console.log(Reflect.has(person, 'toString'));
```

#### Object.prototype.hasOwnProperty 메서드
인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우 true 반환
```js
console.log(person.hasOwnProperty('name')); // true
```

상속받은 프로토타입의 프로퍼티 키인 경우 false 반환.
```js
console.log(person.hasOwnProperty('toString')); // false
```

### 📌 19.14 프로퍼티 열거
#### for...in 문
객체의 모든 프로퍼티를 순회하며 열거할 필요가 있을 때 사용.
```js
const person = {
	name: 'Lee',
	address: 'Seoul',
}

for(const key in person){
	console.log(key + ': ' + person[key]);
}
```
person 객체에는 2개의 프로퍼티가 있다.<br>
객체를 2번 순회하면서 프로퍼티 키를 key 변수에 할당 후 코드 블록을 실행한다.<br>

for..in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 `[[Eumerable]]`의 값이 true인 프로퍼티를 순회하며 열거한다.
`[[Enumerable]]`값이 false면 프로퍼티 열거 불가능.

상속받은 프로퍼티는 제외하고<br>
객체 자신의 프로퍼티만 열거하려면 Object.prototype.hasOwnProperty 메서드를 사용하여<br>
객체 자신의 프로퍼티인지 확인해야 한다.<br>
```js
const person = {
  name: 'Kim',
  address: 'Seoul',
  __proto__: {age:20}
};

for(const key in person){
  if(!person.hasOwnProperty(key)) continue;
  console.log(key + ': ' + person[key]);
}
```


for...in 문은 프로퍼티를 열거할 때 순서를 보장하지 않는다.<br>
→ 대부분의 모던 브라우저는 순서를 보장하고 숫자인 프로퍼티 키에 대해서는 정렬을 한다.<br>

#### Object.keys/values/entries 메서드
객체 자신의 고유 프로퍼티만 열거하기 위해서는 for...in문을 사용하는 것보다<br>
Object.keys/values/entries 메서드를 사용하는 것을 권장한다.

**Es8** - Object.keys : 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환.
```js
const person = {
  name: 'Kim',
  address: 'Seoul',
  __proto__: {age:20}
};
console.log(Object.keys(person)) // ['name', 'address']
```

**Es8** - Object.entries: 객체 자신의 열거 가능한 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환.
```js
console.log(Object.entries(person)) // [['name', 'Kim'],['address', 'Seoul']]
```