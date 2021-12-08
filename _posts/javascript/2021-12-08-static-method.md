---
title: 정적 클래스, 정적 메소드
category: JS
tag: TIL
---

## 정적 메소드

`prototype`이 아닌 클래스 함수 자체에 메소드를 설정할 수 있다. 이런 메소드를 정적 메소드 (static method)라고 한다. 즉, 정적 메소드는 인스턴스없이 클래스에서 바로 호출이 가능하고 이런 특성때문에 유틸리티 함수를 만드는 데 유용하게 사용된다.

```js
class Car {
    static start(){
        console.log(this === Car)
    }
}

Car.start(); //true
```

정적 메소드는 메소드를 `프로퍼티 형태`로 직접 할당하는 것과 비슷하다. start를 호출하면 클래스 생성자인 Car 자체가 된다.


정적 메소드는 어떤 특정한 객체가 아닌 클래스에 속한 함수를 구현하고자 할 때 사용된다. 하지만 정적 메소드는 인스턴스에서 호출할 수 없고 오직 클래스 생성자에서만 호출할 수 있다. (`new`없이 함수로써 호출할 때 의미가 있다.)

```js
class Feedback {
    constructor(data, secret){
        this.data = data
        this.secret = secret
    }
    static fromJson(data) {
        return new Feedback(data, secret)
    }
}

```

> 클래스의 인스턴스없이 호출 가능하며 인스턴스에서는 호출할 수 없다!
> 유틸리티 함수를 만드는 데 유용하게 사용된다!