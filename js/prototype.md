# Prototype

생성자 함수로 유사한 객체를 여러번 만들 수 있다.

```jsx
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.sayHi = function () {
        console.log("hi");
    };
}

p1.sayHi === p2.sayHi;
```

위 코드는 메모리를 불필요하게 잡아 먹는다.

sayHi 함수는 Person 생성자 함수로 만든 모든 객체가 동일한 동작을 하는데, 매 객체마다 함수를 생성하기 때문.

이는 프로토타입으로 해결할 수 있다.

## 프로토타입

> **object that provides shared properties for other objects ([Ecmascript 2015](https://262.ecma-international.org/6.0/#sec-terms-and-definitions-prototype))**

객체마다 [[Prototype]] 이라는 내부 슬롯으로 자신의 프로토타입을 참조하고 있다.

해당 [[Prototype]] 에 접근하기 위해서는 Object.getPrototypeOf() 혹은 \_\_proto\_\_를 사용한다.

\_\_proto\_\_는 많은 브라우저가 사용하고 있어서 legacy feature로 추가해준 것이기에 표준이라 보기에는 어렵다.

\_\_proto\_\_보다는 표준인 Object.getPrototypeOf()를 사용하는게 좋아보임.

## 프로토타입 예시

방금 Person 생성자 함수를 프로토타입을 이용해서 구현함.

```jsx
function Person(name, age) {
    this.name = name;
    this.age = age;
}
Person.prototype.sayHi = function () {
    console.log("hi");
};

p1.sayHi === p2.sayHi;
```

이상한 점이 하나 있음. prototype은 [[Prototype]]에 저장되어 있고, \_\_proto\_\_로 접근한다고 했는데 prototype은 또 뭐임?

## [[Prototype]] vs .prototype

[[Prototype]] 은 위에서 설명을 한 것처럼 **자신의 프로토타입**을 참조하는 속성이라고 했다. 그리고 해당 객체에 접근할 수 있는 프로퍼티도 \_\_proto\_\_라고 했다. 그런데 .prototype은 또 뭘까?

.prototype은 함수에만 있는 속성이다.(일반 객체에는 없다.)

함수를 생성자 함수로 동작시킬 때, 객체를 반환한다고 했다. 이 때, 반환하는 객체의 프로토타입으로 미리 지정해두었던 함수.prototype이 반환하는 객체의 프로토타입으로 정해진다.

### 함수.prototype.constructor

함수에는 prototype이라는 속성이 있다고 했다. prototype도 결국에는 객체이고 속성값을 가진다. 그 중 constructor라는 속성이 있는데 이는 .prototype과 연관된 함수를 가리키고 있다.

그래서 함수와 함수.prototype은 서로를 참조하는 순환참조 구조를 가지고 있다.

```jsx
function func() {}

func.prototype.constructor === func;
```

### Question. 아래의 객체들에 포함될 속성을 골라주세요.

```jsx
function func() {
}

const objGenByFunc = new func();

주어진 속성 값 = 1) [[Prototype]], 2) __proto__, 3) prototype 4) constructor

func = 1, 3
func.prototype = 1, 4
objGenByFunc = 1
```

<details>
<summary>정답</summary>
<p>
    func = 1, 3<br/>
    func.prototype = 1, 4 <br/>
    objGenByFunc = 1<br/>
    모든 객체는 [[Prototype]] 속성만 가지고 __proto__라는 프로퍼티는 Object.prototype에 존재하는 접근자 프로퍼티이다.<br/>
    <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/553e05a9-f80a-49da-b8e9-d28a922dde36/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20211215%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211215T071600Z&X-Amz-Expires=86400&X-Amz-Signature=3cb7eae184d2e8def920ada789150585d42a743262158a027c09438aaa4404eb&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" />
</p>
</details>

## 프로토타입 체인

자바스크립트는 특정 객체의 프로퍼티나 메소드에 접근하려고 할 때, 해당 객체에 접근하려는 프로퍼티 또는 메소드가 없다면 [[Prototype]]이 가리키는 링크를 따라 프로토타입 객체의 프로퍼티나 메소드를 차례대로 검색한다.

```jsx
const student = {
    name: "kukucorn",
    age: 20,
};

// Object.prototype.hasOwnProperty()
console.log(student.hasOwnProperty("name")); // true
```

프로토타입 체인의 종점은 Object.prototype 이다. 그러면 Object.prototype의 프로토타입은 뭘까? null

### \_\_proto\_\_ vs Object.getPrototype()

둘의 동작 결과는 다르지 않다.

하지만, \_\_proto\_\_는 Object.prototype의 접근자 프로퍼티 이기 때문에 Object.create(null)로 객체를 생성했을 때, 문제가 될 수 있다.

```jsx
const obj = Object.create(null);

obj.__proto__;
Object.getPrototypeOf(obj);
```

### (추가) mixin

[https://javascript.info/mixins](https://javascript.info/mixins)

### 참고자료

[https://poiemaweb.com/js-prototype](https://poiemaweb.com/js-prototype)

[https://evan-moon.github.io/2019/10/23/js-prototype/](https://evan-moon.github.io/2019/10/23/js-prototype/)

[https://stackoverflow.com/questions/38740610/object-getprototypeof-vs-prototype/38972040](https://stackoverflow.com/questions/38740610/object-getprototypeof-vs-prototype/38972040)
