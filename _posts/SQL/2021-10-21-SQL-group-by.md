---
title: SQL GROUP BY로 그룹화하기
category: TIL
tag: SQL
---

<div align=center style='background-color:lavender'><h1>GROUP BY</h1></div>

## GROUP BY로 그룹화하기

`GROUP BY`는 집계함수로 넘길 집합을 그룹으로 나눠준다.

|no|name|
|:--:|:--:|
|1|A|
|2|A|
|3|B|
|4|C|
|5|NULL|

```
SELECT name FROM test_table GROUP BY name;
```

|name|
|-|
|A|
|B|
|C|
|NULL|

위와 같이 지정된 컬럼의 값이 같은 행이 하나의 그룹으로 묶인다. `SELECT`가 name 열을 지정했으므로 name 열이 그룹화되어 반환된다. GROUP BY는 `DISTINCT`와 같이 중복을 제거하는 효과가 있다.

## DISTINCT와의 차이점

집계함수와 사용하지 않는 경우, 둘의 차이점은 없다. <br>
`GROUP BY`는 그룹화된 각 그룹이 하나의 집합으로 집계함수의 인수로 넘겨지게 한다.


|no|name|quantity|
|:--:|:--:|-|
|1|A|1|
|2|A|2|
|3|B|10|
|4|C|3|
|5|NULL|NULL|

```
SELECT name, COUNT(name), SUM(quantity)
FROM test_table GROUP BY name;
```
|name|COUNT(name)|SUM(quantity)|
|-|-|-|
|NULL|0|NULL|
|A|2|3|
|B|1|10|
|C|1|3|

A그룹에 속하는 두 개의 열이 한 그룹으로 집계함수의 인수로 넘겨진다.
<br>
예를 들어, 매출관련 데이터를 다룬다고 하자. 점포별, 상품별, 월별 등 특정 단위로 집계할 때 `GROUP BY`를 유용하게 쓸 수 있다.