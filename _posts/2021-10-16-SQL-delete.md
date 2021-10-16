---
title: SQL DELETE로 데이터 삭제/UPDATE로 데이터 갱신
category: TIL
tag: SQL
---

# ⚙️ <span style='color:darkseagreen'>DELETE로 데이터 삭제하기</span>

> DELETE FROM 테이블명 WHERE 조건

```
DELETE FROM test_table;
```
test 테이블의 모든 데이터가 삭제된다. `WHERE`을 사용해 삭제할 데이터를 지정할 수 있다. WHERE 뒤에 오는 조건과 일치하는 모든 행이 삭제된다.<br>
`ORDER BY`는 행을 삭제할 때 데이터를 정렬하는 것이 의미가 없기때문에 DELETE와 함께 사용할 수 없다.

# ⚙️ <span style='color:darkseagreen'>UPDATE로 데이터 갱신하기</span>

> UPDATE 테이블명 SET 컬럼=값, 컬럼=값,... WHERE 조건

`SET`을 사용해서 갱신할 컬럼과 값을 지정한다. 

```
UPDATE test_table 
SET result = 455
WHERE no=2;
```
갱신 전
|no|result|
|:--:|:--:|
|1|ABC|
|2|NULL|

갱신 후
|no|result|
|:--:|:--:|
|1|ABC|
|2|455|
