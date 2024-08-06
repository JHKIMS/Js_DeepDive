### 📌19.6 객체 생성 방식과 프로토타입의 결정

세부적인 객체 생성 방식의 차이는 있으나 `추상 연산 OrdinaryObjectCreate`에 의해 생성된다는 공통점이 있다.

- `OrdinaryObjectCreate`는 빈 객체를 생성.
- 객체에 추가할 프로퍼티 목록이 인수로 전달된 경우 프로퍼티를 객체에 추가
- 인수로 전달받은 프로토타입을 자신이 생성한 객체의 `[[Prototype]] `내부 슬롯에 할당.
- 생성한 객체를 반환한다.

프로토타입은 `추상 연산 OrdinaryObjectCreate`에 전달되는 인수에 의해 결정된다.<br>
인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정.

#### 객체 리터럴에 의해 생성된 객체의 프로토타입
객체 리터럴을 평가하여 객체를 생성할 때 `OrdinaryObjectCreate`을 호출.<br>
이 때 전달되는 `OrdinaryObjectCreate`에 전달되는 프로토타입은 Object.prototype.<br>
→ 객체의 리터럴에 의해 생성되는 객체의 프로토타입은Object.prototype이다.

```js
const obj = { x: 1 };
```
객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 프로토타입으로 갖게 되며, 이로써 Object.prototype을 상속받음.<br>
→ obj객체는 constructor 프로퍼티와 hasOwnProperty 메서드를 자신의 자산처럼 사용이 가능하다.

#### Object 생성자 함수에 의해 생성된 객체의 프로토타입
Object 생성자 함수를 호출하면 객체 리터럴과 마찬가지로 추상 연산 `OrdinaryObjectCreate`가 호출.<br>

Object 생성자 함수와 생성된 객체 사이에 연결이 만들어짐.<br>

![](https://velog.velcdn.com/images/next-react/post/167844d1-74d5-4f59-ab34-0143af4ebd6e/image.png)

→ Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 프로토타입으로 가지며, Object.prototype을 상속받는 것이 된다.

> 객체 생성 방식의 차이는 프로퍼티를 추가하는 방식에 있음.
> - 리터럴 : 객체 리터럴 내부에 프로퍼티를 추가.
> - Object 생성자 함수 : 일단 빈 객체를 생성하고 프로퍼티를 추가.

#### 생성자 함수에 의해 생성된 객체의 프로토타입
new 연산자와 함께 생성자 함수로 인스턴스를 생성하면 `OrdinaryObjectCreate`가 호출됨.

생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 **`prototype 프로퍼티에 바인딩되어 있는 객체.`**

```js
function Person(name){
	this.name = name;
}
const me = new Person('Kim');
```
![](https://velog.velcdn.com/images/next-react/post/b9434340-b6d9-4081-b935-f608b8b893d5/image.png)

사용자 정의 생성자 함수 Person과 더불어 생성된 프로토타입 Person.prototype 프로퍼티는 constructor뿐.

Person.prototype에 프로퍼티를 추가해서 , **`자식 객체가 상속받을 수 있도록 구현.`**
프로토타입 또한 객체.
: 프로퍼티 추가 삭제 가능

![](https://velog.velcdn.com/images/next-react/post/c9853aa8-59f6-43dc-88ae-c4f538f909ee/image.png)
```js
function Person(name){
	this.name = name;
}

Person.prototype.sayHelloi = function() {
	console.log(`say hello ${this.name}`);
}

const me = new Person('Lee');
const you = new Person('Kim');

me.sayHello(); // say hello Lee
you.sayHello(); // say hello Kim
```


### 📌19.7 프로토타입 체인
프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘.
```js
function Person(name){
	this.name = name;
}
Person.prototype.sayH = function(){
	console.log(`hi my name is ${this.name}`)
}
const me = new Person('Lee');
console.log(me.hasOwnProperty('name')); // true
```
me 객체는 Objcet.prototype의 메서드인 hasOwnProperty 호출 가능하다. Person뿐 아니라 Object도 상속받았다.
![](https://velog.velcdn.com/images/next-react/post/7e2ff390-8a45-47d0-8684-4f27b67f212c/image.png)

me 객체의 프로토타입 → Person.prototype의 프로토타입 → Object.prototype(프로토타입 최상위)<br>
**모든 객체는 Object.prototype을 상속받는다.**<br>
그런 Object.prototype의 프로토타입은 없다. null이다.

JavaScript는 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없으면<br>
→ 자신의 부모 역할을 하는 프로토타입의 **`⭐️ 프로퍼티를 순차적으로 검색.`**

**프로토타입 체인**<br>
자바스크립트 엔진은 프로토타입 체인을 따라 프로퍼티, 메서드 검색.<br>
객체 간의 상속 관계로 이뤄진 프로토타입의 계층적 구조에서 객체의 프로퍼티 검색.<br>
**상속과 프로퍼티 검색을 위한 메커니즘.**

**스코프 체인**<br>
자바스크립트 엔진은 함수의 중첩 관계로 이뤄진 스코프의 계층적 구조에서 식별자를 검색.<br>
**식별자 검색을 위한 메커니즘.**<br>

코드의 실행 순서
1. 스코프체인에서 'me' 식별자 검색.
2. 'me' 객체의 프로토타입 체인에서 hasOwnProperty 메서드 검색.
```js
me.hasOwnProperty('me')
```

**👉스코프 체인과 프로토타입 체인은 서로 협력하여 식별자와 프로퍼티를 검색하는데 사용된다.**

### 📌19.8 오버라이딩과 프로퍼티 섀도잉
```js
const Person = (function () {
	function Person(name){
		this.name = name;
	}
	Person.prototype.sayHello = function () {
		console.log('Hi test ${this.name}');
	}

	return Person;
}());

const me = new Person('Lee');

me.sayHello = function() {
	console.log(`Hey. My name is ${this.name}`);
};

me.sayHello();
```

![](https://velog.velcdn.com/images/next-react/post/d01216d4-e580-4115-b81e-ab1337bb18dc/image.png)

**오버라이딩(overridng)**
: 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식.

**프로퍼티 섀도잉(property shadowing)**
: 상속 관계에 의해 프로퍼티가 가려지는 현상(그림에서 Person.prototype의 sayHello가 가려짐)<br>
**✓ 오버라이딩이 일어나지 않으면 프로퍼티 섀도잉 현상도 일어나지 않는다.**<br>

**오버로딩(overloading)**
: 함수의 이름은 동일하지만,<br>
매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식.

프로토타입 프로퍼티 : 프로토타입이 소유한 프로퍼티<br>
인스턴스 프로퍼티 : 인스턴스가 소유한 프로퍼티<br>

하위 객체를 통해 프로토타입의 프로퍼티를 변경 또는 삭제는 불가능<br>
하위 객체를 통해 **`get액세스`** 는 허용, **`set액세스`** 는 혀용하지 않는다.<br>

삭제를 하기 위해서는 하위 객체를 통한 프로토타입 체인 접근이 아닌, 프로토타입에 직접 접근해야함.<br>
```js
Person.prototype.sayHello = function () {
	console.log(`Hey! My name is ${this.name}`);
}
me.sayHello();

delete Person.prototype.sayHello;
me.sayHello() // TypeError: me.sayHello is not a function(삭제됨)
```

### 📌19.10 instancof 연산자
좌변에 객체를 가리키는 식별자.<br>
우변에 생성자 함수를 가리키는 식별자를 피연산자로 받음.<br>
(우변의 피연산자가 함수가 아닌 경우 TypeError)

**우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true, 그게 아니면 false**

```js
function Person(name){
	this.name = name;
}
const me = new Person('Lee');
const parent = {};

Object.setPrototypeOf(me, parent);
// me의 prototype을 parent로 변경.

console.log(me instanceof Person); // false
console.log(me instanceof Object); // true
```

이 코드에서 `me instanceof Person(1번째 console.log)` 이  true가 나오려면 프로토타입으로 교체한 `parent`객체를 Person 생성자 함수의 prototype 프로퍼티에 바인딩하면 된다.
```js
// Person 생성자 함수에 parent을 추가.
Person.prototype = parent;

console.log(me instanceof Person); // true
```

`instanceof`연산자는 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수를 찾는 것이 아니라 **생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인하는 것.**<br>

`instanceof` 연산자를 함수로 표현하면 다음과 같다.<br>
```js
function isInstancof(instance, constructor){
	const prototype = Object.getPrototypeOf(instance);

	// 프로토타입 체인의 종점
	if(prototype === null) return false;

	// constructor.protype: 생성자 함수의 prototype 프로퍼티에 바인딩된 객체라면 true
	// isInstanceof(prototype, constructor): 프로토타입 체인 상의 상위 프로토타입으로 이동하여 확인.
	return prototype === constructor.prototype || isInstanceof(prototype, constructor)
}
```

constructor 프로퍼티와 생성자 함수 간의 연결 파괴<br>
객체의 프로토타입을 직접 변경하면(`me.__proto__` , `Object.setPrototypeOf`)<br>
객체의 `constructor 프로퍼티`가 원래의 생성자를 가리키지 않게 된다.<br>

그런데 instanceof는 영향을 받지 않는다.<br>
instanceof 연산자는 constructor 프로퍼티를 사용하지 않는다.<br>

instanceof는 객체의 프로토타입 체인을 따라간다.<br>
→ 객체의 constructor 프로퍼티가 바뀌어도, 프로토타입 체인이 유지되고 있다면 instanceof 연산자의 결과는 변하지 않는다.<br>
```js
function Person(name) {
    this.name = name;
}

const me = new Person('Lee');

console.log(me instanceof Person); // true

// 프로토타입을 변경하면
Object.setPrototypeOf(me, {});

console.log(me instanceof Person); // false

// `constructor` 프로퍼티를 변경하면
me.constructor = Array;

console.log(me instanceof Person); // true
console.log(me.constructor); // Array
```