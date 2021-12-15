# 생성자 함수

## 객체를 생성하는 방법

### 1. 객체 리터럴

```js
const kukucorn = { age: 20 };
const insong = { age: 21 };
```

### 2. 내장 생성자

```js
let kukucorn = new Object(); // 안티패턴 Number, Boolean
kukucorn.age = 20;
```

### 3. 생성자 함수

```js
function Person(age) {
    this.age = age;
}

const kukucorn = new Person(20);
const insong = new Person(19);
```

객체 리터럴 {...}을 사용하면 객체를 쉽게 만들 수 있고 유효범위 판별 작업도 발생하지 않는다.

> 유효범위 판별 작업이란? 생성자 함수를 사용했다면, 지역 유효범위에 동일한 이름의 생성자가 있을 수 있기 때문에 생성자 함수를 호출한 위치에서부터 전역까지 인터프리터가 거슬러 올라가며 유효범위를 검색한다.

그러면, 만들기도 쉽고 유효범위 판별 작업도 발생하지 않는 객체 리터럴이 짱인가?

아니다, 유사한 객체를 여러 번 만들어야 할 때가 있을 수 있다. 이 때, 객체 리터럴을 사용하게 되면 비효율적이고 코드의 양이 늘어난다.

이 때, new 연산자와 생성자 함수를 이용하면 유사한 객체를 여러 번 만드는데 용이하다.

## 생성자 함수

생성자 함수와 일반 함수에 기술적인 차이는 없다.

다만, 관습적으로 생성자 함수의 첫 글자는 대문자로 시작한다.

또한, new 연산자를 함수 앞에 붙여 실행해야지만 생성자 함수로 동작한다.

## 생성자 함수 동작방식

new 연산자를 사용해서 함수를 실행할 때 내부적으로 어떻게 동작할까?

1. 빈 객체를 생성한다. {}
2. 함수의 prototype을 새로 생성한 객체(빈 객체)의 prototype으로 할당한다.
    - 함수의 prototype이 primitive type 이면, Object.prototype으로 할당한다.
3. 함수의 this에 새로 생성한 객체를 바인딩하고 호출한다. 전달된 파라미터가 있다면 같이 기술함.
4. 생성자 함수에서 반환하는 값이 없다면, 새로 생성한 객체(this)를 반환한다.
    - 만약 생성자 함수로 호출한 함수가 객체를 반환한다면 this 대신 해당 객체가 반환된다.
    - primitive type이 반환된다면 무시하고 this를 반환한다.

코드로 나타내면 아래와 같다.

```js
function C() {}

let obj = Object.createObject(C.prototype);
C.apply(obj, [arg1, arg2]);
```

## 생성자 함수의 반환값

생성자 함수를 new와 함께 호출하면 항상 객체를 반환한다.

반환되는 기본 값은 this로 참조되는 객체.

생성자 함수에서 아무런 프로퍼티나 메소드를 추가하지 않았다면 생성자 함수의 프로토타입만 상속된 빈 객체를 반환할 것임.

함수 내에서 return문을 쓰지 않더라도 암묵적으로 this를 반환한다.

만약 return을 명시적으로 사용하게 되면?? (2가지 경우)

1. this가 아닌 다른 객체를 반환

```js
function Person(age) {
    this.age = age;

    const that = {};

    return that;
}

new Person(20); // {}
```

2. 객체가 아닌 값을 반환

```js
function Person(age) {
    this.age = age;

    return age;
}

new Person(20); // 20 이 아닌 { age : 20 }가 반환
```

객체가 아닌 값을 반환하려 시도한다면 에러를 발생시키지는 않고 this에 참조된 객체를 반환한다.

### 생성자 함수를 일반 함수처럼 호출하면?

생성자 함수와 일반 함수는 기술적으로 차이가 없기 때문에 일반 함수처럼 호출할 수도 있다.

그러면 예상치 못한 상황이 발생할 수 있다.

```js
function Person(age) {
    this.age = age;
}

Person(20);

console.log(age); // 20
```

함수 호출에서 this는 전역객체를 가리키기 때문에 발생하는 문제.

## new를 강제하는 패턴

new.target 프로퍼티를 사용하면 함수가 new와 함께 호출되었는지 아닌지를 알 수 있다.

일반적인 방법으로 함수를 호출했다면 new.target은 undefined를 반환한다. 반면 new와 함께 호출한 경우 함수 자체를 반환해준다.

이를 활용해서 일반적인 방법으로 함수를 호출해도 new를 붙여 호출한 것과 같이 동작하도록 할 수 있다.

```js
function User(name) {
    if (!new.target) {
        return new User(name);
    }

    this.name = name;
}

User("kukucorn");
new User("kukucorn");
```

이렇게 new를 강제하는 패턴을 구현하기 위해 new.target을 사용하면 좀 더 유연하게 코드를 작성할 수 있다.

하지만 이러한 패턴이 좋은 것만은 아니다.

new를 생략해서 생성자 함수를 호출하게 되면 코드가 어떤일을 하는지 알기 어렵다. 그렇기 때문에 이러한 방법 보다는 new를 항상 명시적으로 함께 사용하는게 바람직하다.

## 참고 자료

[https://stackoverflow.com/questions/6750880/how-does-the-new-operator-work-in-javascript](https://stackoverflow.com/questions/6750880/how-does-the-new-operator-work-in-javascript)

[https://stackoverflow.com/questions/1646698/what-is-the-new-keyword-in-javascript](https://stackoverflow.com/questions/1646698/what-is-the-new-keyword-in-javascript)

[https://ko.javascript.info/constructor-new](https://ko.javascript.info/constructor-new)

[https://webclub.tistory.com/309](https://webclub.tistory.com/309)
