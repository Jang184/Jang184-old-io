---
title: SQL 집계함수 (COUNT 이외)
category: TIL
tag: SQL
---

<div align=center style='background-color:honeydew'>
<br>
<h1>COUNT 이외의 집계함수</h1>
<br>
</div>

## SUM으로 합계 구하기

|no|name|quantity|
|:--:|:--:|:-:|
|1|A|1|
|2|A|2|
|3|B|10|
|4|C|3|
|5|NULL|NULL|

<div border='1px solid red'>
```
SELECT SUM(quantity) FROM test_table;
```

|SUM(quantity)|
|:-:|
|16|
</div>
16은 quantity의 모든 열을 하나의 집합으로 보고 그 집합이 갖는 모든 수를 더한 결과이다. SUM은 수치형 집합만 지정할 수 있기때문에 문자열형으로 된 name 열은 지정할 수 없다. SUM도 COUNT와 마찬가지로 `NULL`값은 제외한 나머지 값으로 합계를 낸다.

## AVG로 평균 구하기

> SUM(quantity)/COUNT(quantity)

`AVG`로 계산한 평균값과 위의 연산으로 구한 값은 동일하다.

```
SELECT AVG(quantity) FROM test_table;
```

|AVG(quantity)|
|:-:|
|4.0000|

AVG도 NULL값을 제외한 값으로 평균값을 계산한다. 만약, NULL값을 0으로 간주하고 싶다면 `CASE`를 이용해서 NULL을 0으로 변환한 뒤 계산하면 된다.

```
SELECT AVG(CASE WHEN quantity IS NULL THEN 0 ELSE quantity END)
AS resultnull0;
```

|resultnull0|
|:--:|
|3.2000|

## MIN, MAX로 최소값, 최대값 구하기

|no|name|quantity|
|:--:|:--:|:-:|
|1|A|1|
|2|A|2|
|3|B|10|
|4|C|3|
|5|NULL|NULL|

위의 테이블에서 quantity의 최소값은 `1`, 최대값은 `10`이다. 

```
SELECT MIN(quantity), MAX(quantity)
```

|MIN(quantity)|MAX(quantity)|
|:--:|:--:|
|1|10|

수치형뿐만 아니라 문자열형과 날짜시간형에도 MIN과 MAX를 사용해 최소값과 최대값을 구할 수 있다. 또한, 다른 집계함수와 마찬가지로 NULL값은 무시한다.

```
SELECT MIN(name), MAX(name)
```

|MIN(name)|MAX(name)|
|:--:|:--:|
|A|C|