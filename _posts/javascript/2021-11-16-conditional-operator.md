---
title: JS 삼항 조건 연산자
category: JS
tag: TIL
---

## 조건부 삼항 연산자

`if문`의 단축 형태로 쓰인다.

> 조건문 ? 참 : 거짓

맨 앞에 조건문이 들어가고 조건이 참이면 실행할 식이 `?`의 뒤에, 거짓이면 실행할 식이  `:`의 뒤에 들어간다.

```js
function getFee(isMember) {
    return isMember ? '$2.00' : '$10.00');
}
```

1) `isMember`엔 유저가 멤버인지 확인하는 로직이 있을 것이다. 리턴값으로 true 혹은 false를 반환한다.
2) isMember가 참이면 = member이면 2달러<br>
isMember가 거짓이면 = member가 아니면 10달러를 반환한다.

조건의 반환값이 false가 아닌 `null`, `NaN`, `0`, `""`, `undefined` 도 false 값처럼 사용할 수 있다.

## 예제

```js
let age = 26;
let beverage = (age >= 20) ? "Beer" : "Juice";

console.log(beverage) // "Beer"
```

```js
let greeting = person => {
    let name = person ? person.name : "stranger"
    return `Hello, ${name}`
}

console.log(greeting({name:"Juri"})); // "Hello, Juri"
console.log(greeting(null)); // "Hello, stranger"

```

## 연속 조건문 처리

`if ... else if ... else`와 같은 연속된 조건문을 처리할 수 있다.

```js
function example(...) {
    return condition1 ? value1
         : condition2 ? value2
         : condition3 ? value3
         : value4
}
```

```js
funtion example(...) {
    if (condition1) {
        return value1;
    } else if (condition2) {
        return value2
    } else if (condition3) {
        return value3;
    } else {
        return value4;
    }
}
```