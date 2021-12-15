# === vs ==

## ===

자바스크립트에서 ===는 엄격한 동등성을 테스트할 때 사용하는데, 이는 type과 value가 모두 같아야 한다는 의미이다.

```sql
5 === 5 // true
'hello world' === 'hello world' // true
true === true // true

77 === '77' // false (type이 다름)
'cat' === 'dog' // false (type은 같지만, value가 다름)
false === 0 // false (type이 다름)
```

## ==

==는 느슨한 동등성을 테스트할 때 사용한다. ==는 또한 type conversion을 하게되는데, type conversion이란 두 값을 비교하기 전에 서로 같은 type으로 변환하는 과정을 의미한다.

```sql
5 == '5' // true (type conversion 발생, '5' -> 5로 변환)

false == 0 // true (type conversion 발생, false -> 0으로 변환)

null == undefined // true
undefined == null // true
```

## ECMAScript 2015에 명시된 === 와 ==

[ECMAScript 2015 Language Specification - ECMA-262 6th Edition](https://262.ecma-international.org/6.0/#sec-abstract-equality-comparison)

## 까다로운 예시

### 'hello' !== new String('hello')

'hello'의 type은 string이고,
new String('hello')의 type은 object이다.
결과 : false

### NaN === NaN

결과 : false

### [123] == 123

1. [123] → '123' → 123 으로 변환이 이루어짐.
2. [123]이 primitive 값으로 변환되어야 함.
3. [123].valueOf()는 [123]으로 여전히 객체임.
4. [123].toString()은 '123'으로 primitive 값이므로 '123'으로 변환.
5. '123' == 123 의 비교가 다시 이루어짐.
6. String과 Number의 비교이므로 String이 숫자로 변환됨.

결과 : 123 == 123 은 true

## 결론

복잡한 loose equality(==)를 사용하기 보다는 비교적 간단하고 명확한 strict equality(===)를 사용하자.
