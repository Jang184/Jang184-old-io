---
title: SQL 제약 (NOT NULL 등)
category: TIL
tag: SQL
---

## 제약

테이블에 제약을 설정해 저장할 데이터를 제한할 수 있다. 그 중 하나가 `NOT NULL`제약이다. `NOT NULL` 제약은 `NULL`값이 저장되지 않도록 제한한다. 이외에도 `Primary Key`제약이나 `외부참조(Foreign Key)`제약 등이 있다.

### 1. 테이블 작성시 제약 정의

`CREATE TABLE`을 통해 테이블을 작성할 때 열에 제약을 같이 정의한다. 물론 `ALTER TABLE`로 제약을 지정하거나 변경할 수도 있다. 

```
CREATE TABLE test (
     a INTEGER NOT NULL,
     b INTEGER NOT NULL UNIQUE,
     c VARCHAR(20)
     );
```

a열에는 `NOT NULL`제약이, b열에는 `NOT NULL`제약과 `UNIQUE` 제약이 걸려있다. 

```
CREATE TABLE test2 (
     no INTEGER NOT NULL,
     sub_no INTEGER NOT NULL,
     name VARCHAR(20),
     PRIMARY KEY (no, sub_no)
     );
```

 `Primary Key`제약과 같이 한 개의 제약으로 복수의 열에 제약을 설명하는 경우를 `테이블 제약`이라고 한다.

```
CREATE TABLE test2 (
    no INTEGER NOT NULL,
    sub_no INTEGER NOT NULL,
    name VARCHAR(20),
    CONSTRAINT pkey_sample PRIMARY KEY (no, sub_no)
    );
```

`CONSTRAINT` 를 사용해서 제약에 이름을 붙여 쉽게 관리할 수 있다.

### 2. 제약 추가

#### 열 제약 추가

```
ALTER TABLE test MODIFY c VARCHAR(30) NOT NULL;
```

`ALTER TABLE`을 사용해 열 정의에 제약을 추가한다. 기존 테이블을 변경할 경우엔 제약을 위반하는 데이터가 있는지 먼저 검사한다. 만약 위와 같이 기존 열에 `NOT NULL` 제약을 추가했을 때, 열에 `NULL`값이 존재하면 에러가 발생한다.

#### 테이블 제약 추가

```
ALTER TABLE test ADD CONSTRAINT pkey_sample2 PRIMARY KEY(a);
```

`ALTER TABLE`의 `ADD` 명령으로 테이블 제약을 추가할 수 있다. 위의 예시는 `Primary Key`제약을 추가하는 명령이다. PK키는 테이블에 하나만 추가할 수 있다. 이미 PK키가 설정된 테이블에 추가로 설정할 수 없다.

### 3. 제약 삭제

#### 열 제약 삭제

```
ALTER TABLE test MODIFY c VARCHAR(30);
```

열 제약을 추가할 때와 동일하게 열 정의를 변경한다.

#### 테이블 제약 삭제

```
ALTER TABLE test DROP CONSTRAINT pkey_sample2;
```

`ALTER TABLE`의 `DROP` 명령으로 테이블 제약을 삭제할 수 있다. 삭제할 땐 제약명을 지정한다. <u>단, PK키는 테이블당 하나만 설정할 수 있기 때문에 굳이 제약명을 지정하지 않아도 삭제할 수 있다.</u>