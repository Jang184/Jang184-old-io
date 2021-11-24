---
title: TS builder pattern
category: TS
tag: TIL
---


[참고 : Let's Look at th Builder Pattern in Typescript](https://betterprogramming.pub/lets-look-at-the-builder-pattern-in-typescript-fb9cf202c04d)

## 들어가면서

builder pattern은 객체 생성 메커니즘을 다루는 디자인 패턴 중 하나다. 이는 패턴이 객체를 직접적으로 인스턴스화 하지 않고 객체를 생성하는데 사용된다는 의미이다.

---

- 애플리케이션에서 사용하는 인터페이스에 누락된 속성을 더하고자 한다. 인터페이스의 모든 속성은 필수이므로 인스턴스화되는 모든 위치에 값을 지정해야 한다. 그러나 애플리케이션의 특정 부분에만 값이 필요하다면 복잡한 객체를 모든 곳에 갖게 된다. 이 객체는 기본값이 바뀌면 취약해질 수 있다.

- 사용자가 변경을 요청해서 인터페이스에 하나의 값을 추가하려고 한다. 테스트에서 많은 복잡한 객체를 생성했고 이 모든 객체들이 추가한 하나의 값을 수행해야 한다. 테스트가 대다수 실패했고 프로젝트의 리더는 하나의 추가 속성이 개발팀에서 그렇게나 많은 시간을 소비해야하는 지 이해하지 못했다.

코드가 늘어날수록 유지보수하기 어려울뿐만 아니라 개발비용이 늘어나고 사용자들의 요청에 부응하기 어려워진다. 패턴을 적용해 당신의 개발자 인생을 더 윤택하게 만들어보자! 

## 예시 : The UserBuilder

> The builder pattern itself is used to separate the construction of a complex object from its representation.



`name`과 `age`를 속성으로 갖는 `User`가 있다. 이 객체는 인스턴스화하기 간단하다.
```ts

export interface User {
    name: string;
    age: number;
}
```

```ts
const user: User = {
    name: "Juri",
    age: 27
};
```

이 User를 표시하는 코드에 대한 테스트를 작성한다고 가정해보자.


예를 들어, 이름의 첫 글자가 대문자로 표시되는 지 확인하는 테스트를 작성하려고 한다. `age`속성은 필수이므로 임의의 age값으로 개체를 생성해야 한다. 하지만 `User`에 100개의 다른 속성이 있다고 하면 작성하는 모든 테스트에서 각 속성에 대한 값을 제공해야 한다.


`UserBuilder`클래스는 애플리케이션에서 사용되는 기본 속성을 가진 기본 User을 생성한다. 기본값은 딱 한번만 지정하면 된다. 만약 그 값들이 변하면 다시 기본값을 지정한다.


추가해야 하는 속성이 생기면 builder에 한번만 추가한다. builder을 사용하면 유지보수가 쉬워진다.

```ts
import {User} from "./user";

export class UserBuilder {
    private readonly _user: User;

    constructor() {
        this._user = {
            name: "",
            age: 0
        };
    }

    name(name: string): UserBuilder {
        this._user.age = age;
        return this;
    }

    build(): User {
        return this._user;
    }
}


```

## 

`UserBuilder`를 사용하면 객체를 인스턴스화할 때 어떤 속성이라도 생략할 수 있다. name에 대한 테스트를 작성할 때 아래와 같이 작성할 수 있다.

```ts
const userWithName: User = new UserBuilder()
    .name("Juri")
    .build();
```

`age`가 필요없으며 이외에 다른 속성들도 필요하지 않다. 테스트하고자 하는 속성만 작성하면 된다.


