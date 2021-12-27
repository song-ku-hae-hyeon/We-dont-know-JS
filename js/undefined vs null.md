# 값 아닌 값

### undefined와 null

- 자바스크립트에서는 undefined 타입과 null 타입이 존재한다. 이들은 각각 undefined와 null 값만을 갖는 타입이다. 즉 둘은 타입과 값이 같다. 둘 다 '빈 값' 또는 값이 아님을 의미한다. 하지만 둘은 엄연히 차이가 있다.

- null은 식별자가 아닌 특별한 키워드이므로 변수를 할당할 수 없다. 그러나 undefined는 식별자로 쓸 수 있어서 변수를 할당할 수 있다.

- 느슨한 모드에서는 전역 스코프에서도 undefined라는 식별자에 값을 할당할 수 있고, 심지어 모드에 상관없이 지역 변수를 생성할 수 있다.

```jsx
function foo() {
  'use strict';
  var undefined = 2;
  console.log(undefined); // 2
}
```

- 당연히 위와 같이 undefined 에 값을 할당하는 것은 매우 지양되는 패턴이다.

- undefined 는 void 연산자로도 얻을 수 있다. 표현식 void \_ 는 어떤 값이든 undefined로 만든다. (관례에 따라 void만으로 undefined 값을 나타내려면 void 0 이라고 쓴다. void 0, void 1, undefined 모두 같다)

### 참고

[You Don't know JS](https://github.com/getify/You-Dont-Know-JS)
