# IIFE(Immediately-Invoked Function Expression)

즉시 실행하는 함수는 함수 표현식이어야한다.

(= 함수 선언문은 즉시 실행되지 않는다.)

## 함수 선언문

```jsx
// 일반적인 함수 선언 방식
function func() {}
```

## 함수 표현식

유연한 자바스크립트 언어의 특징(고차함수)을 활용

함수 선언문과 달리 함수 이름이 옵션이다.

```jsx
// 변수에 함수를 등록
const func = function () {};

// 콜백 함수로 사용
setTimeout(function () {}, 1000);
```

## 함수 선언문 vs 함수 표현식

함수를 선언하는 방식에는 차이가 거의 없다.

parser가 어떻게 해석하냐에 따라 함수 선언문이냐 함수 표현식이냐 구분할 수 있다.

일반적인 함수 선언 방식을 제외하고는 모두 함수 표현식으로 해석이 된다.

```jsx
// 모두 함수 표현식으로 해석되는 경우
// 1) 변수에 함수를 할당
const func = (function () {})(
    // 2) 괄호로 감쌈
    function () {}
);
// 3) 쉼표를 사용해 구분
0, function () {};
```

## IIFE의 2가지 형태

```jsx
// 1) 함수 표현식을 괄호로 감싸고 이를 호출한다.
(function () {})();

// 2) 괄호 안에 있는 함수 표현식을 바로 호출한다.
(function () {})();
```

자바스크립트 언어 개발에 참여한 더글라스 크락포드는 2번 방식이 에러를 덜 발생시키는 표현이라함.

## 화살표 함수를 IIFE로 사용

ES6에 등장한 화살표 함수를 사용해서 IIFE를 만들 수 있다.

```jsx
(() => console.log("hello IIFE"))(); // 'hello IIFE'
() => console.log("bye IIFE")(); // IIFE처럼 동작하지 않고 화살표 함수를 반환함.
```

화살표 함수를 사용하면 더글라스 크락포드가 선호한 방식을 사용할 수 없다.

명세에서는 함수를 호출하기 위해서는 호출대상이 MemberExpression이어야 한다.

하지만, 화살표함수는 AssignmentExpression의 한 종류 이기때문에 호출의 대상이 될 수 없다.

## IIFE의 사용 이유

IIFE는 보통 전역 스코프를 오염시키지 않기 위해서 사용한다.

### Anonymous Closures

```jsx
(function () {
    // ... all vars and functions are in this scope only
    // still maintains access to all globals
})();
```

함수 스코프를 따르기 때문에 함수의 실행이 끝나면 변수는 소멸된다.

### Global Import

```jsx
(function ($, YAHOO) {
    // now have access to globals jQuery (as $) and YAHOO in this code
})(jQuery, YAHOO);
```

자바스크립트는 implied globals가 발생해서 전역 스코프가 더러워지기 쉽다.(물론 var 대신 let, const를 사용하면 해결 가능) 하지만, 라이브러리를 사용하는 경우 전역 스코프에서 중복된 변수를 사용할 수 있다.

그래서 즉시 실행함수의 매개변수로 즉시 실행함수에서 전역변수처럼 사용할 수 있게 사용할 수 있다.

### Module Export

```jsx
let Counter = (function () {
    // private 변수
    let num = 0;

    return {
        increase() {
            return ++num;
        },
        decrease() {
            return --num;
        },
    };
})();
```

클로저를 활용해서 외부에 노출시킬 변수, 함수만 IIFE의 반환값으로 지정한다.

---

## 참고자료

[함수 표현식 vs 함수 선언식](https://joshua1988.github.io/web-development/javascript/function-expressions-vs-declarations/)

[함수는 선언된 위치에 따라 다르게 해석된다](https://stackoverflow.com/questions/1634268/explain-the-encapsulated-anonymous-function-syntax)

[자바스크립트 모듈 패턴](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html)
