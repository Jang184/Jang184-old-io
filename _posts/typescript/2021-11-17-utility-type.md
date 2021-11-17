---
title: TS 유틸리티 타입
category: TS
tag: TIL
---

> [참고 : 코딩앙마](https://youtu.be/IeXZo-JXJjc)

## keyof

```ts
interface User {
    id: number;
    name: string;
    age: number;
}

type UserKey = keyof User; 
```

keyof 은 인터페이스의 key값을 가리킨다. 위의 코드에서 UserKey 타입은 인터페이스 User의 키값인 `id`. `name`, `age`가 된다.

## Partial

```ts
let admin: Partial<User> = {
    id: 1,
    name: "Juri"
}
```

인터페이스 User를 부분적으로 가져온다. admin 객체 내에 `age`가 없어도 에러가 발생하지 않는다.

## Required

```ts
let admin2: Required<User> = {
    id: 2,
    name: "Jun",
    age: 26
}
```

인터페이스 User의 모든 키값을 갖고 있어야 한다.

## Readonly

```ts
let admin3: Readonly<User> = {
    id: 3,
    name: "mong",
    age: 1
}

admin3.age = 2 //error
```

객체의 인스턴스로 접근할 수 없다. 

## Record

1학년부터 4학년까지의 성적을 갖는 객체를 만들고 싶다.

\<예시>
```ts
const score = {
    "1": "A",
    "2": "C",
    "3": "B",
    "4": "D"
}
```

### 1. interface 

```ts
interface Score {
    "1":"A"|"B"|"C"|"D",
    "2":"A"|"B"|"C"|"D",
    "3":"A"|"B"|"C"|"D",
    "4":"A"|"B"|"C"|"D"
}
```
에러가 발생하진 않지만 중복되는 코드에 아찔하다.


### 2-1. Record (1)

> Record<K,T> 

K는 Key, T는 Type을 가리킨다.

```ts
const score: Record<"1"|"2"|"3"|"4","A"|"B"|"C"|"D" > = {
    "1": "A",
    "2": "C",
    "3": "B",
    "4": "D"
}
```
객체의 Key값으로 1,2,3,4 Type으로 A,B,C,D가 대입된다. 

### 2-2. Record (2)

타입을 미리 지정함으로 코드가 한결 깔끔하다.

```ts
type Grade = "1"|"2"|"3"|"4";
type Score = "A"|"B"|"C"|"D";

const score2: Record<Grade,Score> = {
    "1": "A",
    "2": "C",
    "3": "B",
    "4": "D"
}
```

### 3. Record 예제

```ts
interface User {
    id: number;
    name: string;
    age: number;
}

function isValid(user: User) {
    const result: Record<keyof User, boolean> = {
        id: user.id > 0,
        name: user.name !== '',
        age: user.age < 0
    };
    return result;
}
```

객체 result의 키값은 인터페이스 User의 키 값으로, 각 타입은 boolean으로 설정할 수 있다.

## Pick<T,K>

```ts
const test: Pick<User, "id" | "name"> = {
    id: 1,
    name: "Juri"
}
```

인터페이스 User의 값 중 "id"와 "name"만 지정했다. 객체 내에 "age"가 없어도 에러가 발생하지 않는다.

## Omit<T,K>

Omit은 객체의 특정 프로퍼티를 제외할 수 있다.

```ts
const test: Omit<User, "age"> = {
    id: 1,
    name: "Juri"
}
```

객체의 프로퍼티 중 "age"가 제외되어 객체 내에 없어도 에러가 발생하지 않는다.

## Exclude<T1,T2>

Omit과 비슷하지만 Exclude는 타입을 제외한다.

```ts
type T1 = string | number;
type T2 = Exclude<T1, number>;
```

타입 T2는 타입 T1에서 number 타입을 제외한 string 타입을 가리킨다.

## NonNullable

`null`과 `undefined`를 제거한다.

```ts
type T1 = string | null | undefined | void ;
type T2 = NonNullable<T1>
```

타입 T2를 확인해보면 null과 undefined가 제거되고 string과 void 타입을 가리키고 있다.

