---
title: SQL 상관 서브쿼리
category: TIL
tag: CS
---

## EXISTS

`EXISTS`를 사용하면 서브쿼리가 반환하는 `결과값`의 존재여부를 확인할 수 있다. `EXISTS`가 반환된 행이 있는지 확인해보고 있으면 True, 없으면 False를 반환한다.

<p>예시</p>

test2 테이블에 test1 테이블의 no 열의 값과 같은 행이 있다면 `있음`이라는 값으로, 없으면 `없음`이라는 값으로 갱신하고 싶다.

<p>test1 테이블</p>

|no|a|
|:-:|:-:|
|1|NULL|
|2|NULL|
|3|NULL|
|4|NULL|
|5|NULL|

<p>test2 테이블</p>

|no2|
|:-:|
|3|
|5|

```
UPDATE test1 SET a='있음' WHERE
    EXISTS (SELECT * FROM test2 WHERE no2 = no);
```

<p>결과</p>

서브쿼리 부분이 UPDATE의 WHERE로 행을 검색할 때마다 차례로 실행된다. `no`가 3과 5일때만 (no2 = no) 서브쿼리가 행을 반환한다. 서브쿼리가 행을 반환하면 `True`를 돌려준다.

|no|a|
|:-:|:-:|
|1|NULL|
|2|NULL|
|3|있음|
|4|NULL|
|5|있음|

## NOT EXISTS

`없음`의 경우에는 행이 없을 때 `True`를 반환한다. 이 때는 `NOT EXISTS`를 사용한다.

```
UPDATE test1 SET a='없음' WHERE
    NOT EXISTS (SELECT * FROM test2 WHERE no2 = no);
```

<p>결과</p>

|no|a|
|:-:|:-:|
|1|없음|
|2|없음|
|3|있음|
|4|없음|
|5|있음|

## 상관 서브쿼리

서브쿼리 명령문 안에 중첩구조로 된 SELECT 명령이 존재한다.