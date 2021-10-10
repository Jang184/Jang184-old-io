---
title: SQL ORDER BY로 정렬하기
category: TIL
tag: SQL
---
`ORDER BY`를 이용해 컬럼을 지정하고 정렬할 수 있다.

> SELECT 컬럼명 FROM 테이블명 WHERE 조건 ORDER BY 컬럼명1, 컬럼명2

### 1. 여러 개의 컬럼 정렬하기
   
<div align=center>test table</div>
|A|B|
|:--:|:--:|
|1|1|
|2|1|
|2|2|
|1|3|
|1|2|