
자바스크립트는 객체 기반의 프로그래밍 언어.
자바스크립트를 이루고 있는 거의 "모든 것"이 객체.
원시 타입(primitive type)의 값을 제외한 나머지 값들(함수, 배열, 정규 표현식 등)은 모두 객체.

### 📌 객체지향 프로그래밍
객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임

객체지향 프로그래밍은 실세계의 실체(사물, 개념)을 인식하는 철학적 사고를 프로그래밍에 접목하려는 시도에서 시작한다.<br>
실체는 특징이나 성질을 나타내는 속성을 가지고 있고, 이를 통해 실체를 인식하거나 구별할 수 있다.

사람에게는 다양한 속성이 있으나 우리가 구현하려는 프로그램에서의 사람의 '이름'과 '주소'라는 속성에만 관심이 있다고 가정.

**추상화**
이처럼 다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현하는 것.

'이름'과 '주소'라는 속성을 갖는 person이라는 객체를 자바스크립트로 표현하면 다음과 같다.
```js
const person = {
	name: 'Lee',
	address: 'Seoul',
}
```
`속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조를 객체`라 한다.<br>
객체지향 프로그래밍은 독립적인 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임이다.

반지름 - 원의 상태를 나타내는 데이터(state)(property)<br>
지름, 둘레를 구하는 것 - 동작(behavior)(method)

이러한 객체는 상태 데이터(프로퍼티)와 동작(메서드)을 하나의 논리적인 단위로 묶은 복합적인 자료구조.
```js
const circle = {
	radius: 5,
	
	// 원의 지름
	getDiameter(){
		return 2 * this.radius;
	},
	// 원의 둘레
	getPerimeter(){
		return 2 * Math.PI * this.radius;
	}
}	
```

### 📌 상속과 프로토타입
상속은 객체지향의 핵심.<br>
JavaScript는 **프로토타입 기반**으로 `상속을 구현하여 불필요한 중복을 제거.`<br>
중복을 제거하는 방법은 기존의 코드를 적극적으로 재사용.

`Circle 생성자 함수가 생성한 모든 인스턴스`는 자신의 프로토타입,<br>
즉 상위(부모) 객체 역할을 하는 `Circle prototype의 모든 프로퍼티와 메서드를 상속받는다.`


**❌ BadThings.**
```js
function Circle(radius){
  this.radius = radius;
  this.getArea = function() {
    return Math.PI * this.radius ** 2;
  };
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // false
```
Circle 생성자 함수는 인스턴스를 생성(circle1, circle2)할 때마다 `getArea 메서드`라는 메서드는 동일한 역할을 하는데 `불필요하게 중복 소유`
→ circle1.getArea === circle2.getArea `false`<br>
→ 메모리 낭비. 완전히 동일한 역할의 동일한 코드.<br>
![](https://velog.velcdn.com/images/next-react/post/91d7b077-0032-41ed-ac3c-c736ad39e2be/image.png)

**🤔 이러한 불필요한 부분을 없애기 위해서.**<br>
자바스크립트는 `프로토타입을 기반으로 상속을 구현`한다.<br><br>
**👍 AweSome.**
```js
function Circle(radius){
	this.radius = radius;
}

/* Circle 생성자 함수가 생성한 모든 인스턴스가 
getArea메서드를 공유해서 사용할 수 있도록 prototype에 추가했다.
**/
Circle.prototype.getArea = function() {
	return Math.PI * this.radius ** 2;
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);

/* Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체의 역할을 하는
프로토타입 Circle.prototype으로부터 getArea 메소드를 상속받는다.
→ Circle 생성자 함수가 생성하는 모든 인스턴스는 하나의 getArea 메소드를 공유.
**/
console.log(circle1.getArea === circle2.getArea) // true
```

`상속은 코드의 재사용`이란 관점에서 매우 유용하다.<br>
공통적으로 사용할 프로퍼티나 메서드를 프로토타입에 미리 구현해 두면 생성자 함수가 생성할 모든 인스턴스는 별도의 구현 없이 `부모 객체의 프로토타입의 자산을 공유하여 사용 가능.`


### 📌 프로토타입 객체
프로토타입은 객체지향 프로그래밍의 근간을 이루는 객체 간 `상속을 구현하기 위해 사용.`<br>
프로토타입을 상속받은 하위(자식) 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용가능.<br>

모든 객체는 [[Prototype]]이라는 내부 슬롯을 가진다.<br>
객체가 생성될 때 객체 생성 방식에 따라 프로토타입이 결정되고 [[Prototype]]에 저장된다.<br>

객체 리터럴에 의해 생성된 객체의 프로토타입은 Object.prototype<br>
생성자 함수에 의해 생성된 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체.

모든 객체는 하나의 프로토타입을 가지며.<br>
모든 프로토타입은 생성자 함수와 연결되잇다.<br>
객체와 프로토타입과 생성자 함수는 서로 연결되어 있다.<br>
![](https://velog.velcdn.com/images/next-react/post/0b71ea05-2a17-47e3-ae20-f46c08857b7b/image.png)


`__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입,<br>
즉 자신의 [[Prototype]] 내부 슬롯이 가리키는 프로토타입에 간접적으로 접근 가능.

프로토타입은 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근 가능<br>
↔️ 생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근 가능.

#### `__proto__` 접근자 프로퍼티<br>
모든 객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입<br>
즉, `[[Prototype]] 내부 슬롯에 간접적으로 접근`할 수 있다.

`__proto__` 는 접근자 프로퍼티.<br>
자바스크립트는 원칙적으로 내부 슬롯과 내부 메서드에 직접적으로 접근하거나 호출할 수 있는 방법을 제공하지 않는다.

일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단을 제공하기는 한다.<br>

> [[Prototype]] 직접 접근은 X
💡 `__proto__` 접근자 프로퍼티를 통해 `간접적`으로 [[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근할 수 있다.

접근자 프로퍼티(`__proto__`)는 자체적으로 값을 갖지 않고, <br>
다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수(accessor function) [[Get]], [[Set]] 프로퍼티 어트리뷰트로 구성된 프로퍼티이다.

1. `__proto__` 접근자 프로퍼티를 통해 프로토타입에 접근하면 내부적으로 `__proto__` 접근자 프로퍼티의 getter함수를 호출.
2. `__proto__` 접근자 프로퍼티를 통해 새로운 프로토타입을 할당하면 `__proto__` 접근자 프로퍼티의 setter 함수를 호출.
```js
const obj = {}
const parent = { x: 1 };

// 1. get __proto__가 호출됨. obj 객체의 프로토타입 취득
obj.__proto__; 
// 2. set __proto__가 호출됨. obj 객체의 프로토타입을 교체
obj.__proto__= parent; 

// obj.x의 값은 1
```

`__proto__` 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니다.<br>
`Object.prototype의 프로퍼티`이다.<br><br>
**🙄 모든 객체는 상속을 통해 `Object.prototype.__proto__` 접근자 프로퍼티를 사용할 수 있다.**<br>

#### `__proto__` 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유
[[Prototype]] 내부 슬롯의 값<br>
상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위함.<br><br>

프로토타입 체인은 `단방향 링크드 리스트`로 구현되어야함.<br>
순환 참조하는 프로토타입 체인이 만들어지면 프로토타입 체인 종점이 존재하지 않기 때문에 프로퍼티를 검색할 때 무한루프에 빠진다.<br>
→ 아무런 체크 없이 무조건적으로 프로토타입을 교체할 수 없도록 `__proto__` 접근자 프로퍼티를 통해 프로토타입에 접근하고 교체하도록 구현되어 있다.

다음 예시의 코드처럼 코드를 작성하면 안된다.<br><br>
**❌ BadThings.**
```js
const parent = {};
const child = {};

// child의 프로토타입을 parent로 설정
child.__proto__ = parent;
// parent의 프로토타입을 child로 설정
parent.__proto__ = child; // cyclic __proto__ 에러 발생
```
<br>

![](https://velog.velcdn.com/images/next-react/post/84ef8048-9c1d-41f2-96ce-8ac4ebc1f422/image.png)


#### `__proto__` 접근자 프로퍼티를 코드 내에서 직접 사용하는 것 권장X

모든 객체가 `__proto__` 접근자 프로퍼티를 사용할 수 있는 것은 아니다.<br>
직접 상속을 통해 다음과 같이 Object.prototype을 상속받지 않는 객체를 생성할 수도 있다.<br>
→ `__proto__` 접근자 프로퍼티 사용할 수 없음.<br>
`__proto__` 접근자 프로퍼티 대신 `프로토타입의 참조 혹은 할당`하고 싶은 경우에는 <br>
`Object.getPrototypeOf or setPrototypeOf 메소드`를 사용하는 것을 **권장**한다.

```js
const obj = Object.create(null);

console.log(Object.getPrototypeOf(obj));
```

`❌__proto__` 보다 `👍getPrototypeOf`, `👍setPrototypeOf`가 좋은 이유<br>
:: **💡 Es5에서는 setPrototypeOf을 지원하지 않는다.**
1. **명확성 및 가독성**:<br>
   •  Object.getPrototypeOf와 Object.setPrototypeOf는 명시적으로 프로토타입을 가져오고 설정하는 메서드입니다. 이들은 코드의 의도를 더 명확하게 전달합니다.<br>
   •  __proto__는 비공식적인 접근자 프로퍼티로, 특히 자바스크립트 초보자에게는 직관적이지 않을 수 있습니다.<br>

2. **호환성 및 표준화**:<br>
   •  __proto__는 오래된 브라우저에서 호환성 문제가 있을 수 있습니다. 특히 매우 구식 브라우저에서는 제대로 지원되지 않을 수 있습니다.<br>
   •  Object.getPrototypeOf와 Object.setPrototypeOf는 ES5(ECMAScript 5) 표준에서 도입된 메서드로, 최신 자바스크립트 표준을 준수하며 모든 최신 브라우저에서 안정적으로 지원됩니다.<br>

3.  **성능**:<br>
    •  일부 자바스크립트 엔진에서는 `__proto__`를 사용하는 것이 성능에 영향을 미칠 수 있습니다.<br>
    이는 엔진이 프로토타입 체인을 동적으로 변경해야 하기 때문입니다.<br>
    👉 자바스크립트 엔진 종류.
    구글 V8, 모질라(FireFox) SpiderMonkey, 애플 JavaScriptCore(Nitro)

• Object.getPrototypeOf와 Object.setPrototypeOf는 엔진이 최적화할 수 있는 더 명확한 API를 제공합니다.

4. **유지보수성**:<br>
   •  명시적인 메서드를 사용하면, 코드의 유지보수가 더 용이합니다. 다른 개발자가 코드를 이해하고 수정하는 것이 더 쉬워집니다.<br>
   •  `__proto__`를 사용하는 코드에서는, 잘못된 사용이나 오타로 인해 디버깅이 어려울 수 있습니다.<br>

#### 함수 객체의 prototype 프로퍼티
함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.

prototype 프로퍼티는 생성자 함수가 생성할 객체의 프로토타입을 가리킨다.<br>
생성자 함수로서 호출할 수 없는 함수.<br>
**non-constructor 인 `화살표 함수`와 ES6 메서드 축약 표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.**

일반 함수는 prototype 프로퍼티를 소유하지만 객체를 생성하지 않는 일반 함수의 prototype 프로퍼티는 아무런 의미가 없다.

모든 객체가 가지고 있는 `__proto__` 접근자 프로퍼티와<br>
함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다.<br>

`__proto__` 접근자 프로퍼티
: 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용.

prototype 프로퍼티
:  생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용.

```js
function Person(name) {
	this.name = name;
}

const me = new Person('Kim');

console.log(Person.prototype === me.__proto__); // true
```
![](https://velog.velcdn.com/images/next-react/post/cf977a0d-63f1-45e0-bfec-41b357f3cf8a/image.png)


#### 프로토타입의 constructor 프로퍼티와 생성자 함수
모든 프로토타입은 constructor 프로퍼티를 가진다.<br>
constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수.<br>
생성자 함수가 생성될 때 → 함수 객체가 생성될 때 이루어짐.

```js
function Person(name) {
	this.name = name;
}
// me 객체의 생성자 함수는 Person.
const me = new Person('Kim');
console.log(me.constructor === Person); // true
```
![](https://velog.velcdn.com/images/next-react/post/568a0036-8aa1-4a73-a786-50f99ed987d8/image.png)

me 객체에는 constructor 프로퍼티가 없다.<br>
me 객체의 프로토타입인 Person.prototype에는 constructor 프로퍼티가 있다.<br>
**me 객체는 Person.prototype.constructor 프로퍼티를 상속받아 사용하는 것.**

### 📌 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입
constructor 프로퍼티가 가리키는 생성자 함수는 `인스턴스를 생성한 생성자 함수`다.

```js
// obj 객체를 생성한 생성자 함수는 Object
const obj = new Object();
console.log(obj.constructor === Object); // true

function Person(name){
	this.name = name;
}
const me = new Person('Kim');
console.log(me.constructor === Person); // true
```

이 예제의 obj는 Object 생성자 함수가 아닌 객체 리터럴에 의해 생성된 객체.<br>
**obj** 객체는 `Object 생성자 함수와 constructor 프로퍼티로 연결`되어 있다.

Object 생성자 함수에 인수를 전달하지 않거나 undefined 또는 null을 인수로 전달하면서 호출하면 내부적으로 추상 연산 OrdinaryObjectCreate을 호출하여 Object.prototype을 프로토타입으로 갖는 빈 객체를 생성한다.
```js
const obj = {} // new Object()의 리터럴 표기법 {}

console.log(obj.constructor === Object) // true
```

> **추상 연산**<br>
> 추상 연산은 JavaScript의 내부 동작을 설명하는 단계별 가이드라인<br>
> **🙄 실제 코드로 존재X, 엔진이 어떻게 동작해야 하는지 정의**<br>
> const obj={} 같은 코드를 내부적으로 어떻게 처리하는지 설명할 때 사용


```js
// 추상 연산 OrdinaryObjectCreate을 호출하여 빈 객체를 생성
const obj1 = new Object(); // {}

// Object 생성자 함수로 빈 객체를 생성
const obj2 = new Object(123); // {123}
```

```js
function foo() {}
// 내부적으로 const foo = new Function()

console.log(foo.constructor === Function) // true
```
리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요함.<br>
**`리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 가짐.`**<br>
프로토타입은 생성자 함수와 더불어 생성되며 `prototype.constructor` 프로퍼티에 의해 연결되어 있다.<br>
**💡Function이면 무조건 prototype이 있다.**<br>

리터럴 표기법에 의해 생성된 객체는 생성자 함수에 의해 생성된 객체는 아니다.<br>
하지만 큰 틀에서 생각해 보면 `리터럴 표기법`으로 생성한 객체도 생성자 함수로 생성한 객체와 **`본질적인 면에서 큰 차이는 없다.`**

객체 리터럴 {} Object Object.prototype <br>
함수 리터럴 function FunctionFunction.prototype <br>
배열 리터럴 []  Array Array.prototype <br>
정규표현식 리터럴 / RegExp RegExp.prototype <br>

### 📌 프로토타입의 생성 시점
객체는 리터럴 표기법 또는 생성자 함수에 의해 생성되므로 **`모든 객체는 생성자 함수와 연결되어 있다.`**

**프로토타입은 생성자 함수가 생성되는 시점에 같이 생성된다.**

**생성자 함수는**
- 사용자가 직접 정의한 **`사용자 정의 생성자 함수,`**
- 자바스크립트가 기본 제공하는  **`빌트인 생성자 함수`** 로 구분 가능.

#### 사용자 정의 생성자 함수와 프로토타입 생성 시점

내부 메서드 [[Constructor]]를 갖는 함수 객체,<br>
즉 화살표 함수나 ES6 메서드 축약 표현으로 정의하지 않고 일반 함수(함수 선언문, 함수 표현식)로 정의한 함수 객체는 new연산자와 함께 생성자 함수로서 호출할 수 있다.

생성자 함수로서 호출할 수 있는 함수.<br>
**즉 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.**

생성자 함수로서 호출할 수 없는 함수.<br>
**즉 non-constructor는(ex. Arrow Function) 프로토타입이 생성되지 않는다.**
```js
const Person = name => {
	this.name = name;
}

console.log(Person.prototype); // undefined
```

#### 빌트인 생성자 함수와 프로토타입 생성 시점
Object, String, Number, Function, Array 같은 빌트인 생성자 함수도<br>
일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다.<br>

![](https://velog.velcdn.com/images/next-react/post/d55fc44f-21b5-4943-9416-9aa2376d546c/image.png)

이처럼 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재한다.<br>
**이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 `프로토타입은 생성된 객체의 [[Prototype]] 내부 슬롯에 할당` 된다.**<br>
이로써 생성된 객체는 프로토타입을 상속받는다.<br>

> **전역 객체**<br>
> 코드 실행 전 자바스크립트 엔진에 의해 생성되는 특수한 객체.<br>
> **클라이언트(브라우저) - window**<br>
> **서버 사이드 환경(Node.js) - global 객체**<br>
>
> **전역 객체**는<br>
> → 표준 빌트인 객체, 환경에 따른 호스트 객체, var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다.