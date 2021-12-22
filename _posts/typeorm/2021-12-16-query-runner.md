---
title: TypeORM QueryRunner
category: TypeORM
tag:
    - TIL
    - orm
    - database
---

## QueryRunner란?

connection이 설정되어야만 데이터베이스와 통신이 가능하다. TypeORM의 connection은 데이터베이스 커넥션을 설정하지 않지만 대신 `connection pool`(커넥션 풀)을 설정한다. 만약 데이터베이스 커넥션을 설정하길 원한다면 `Query Runner`을 이용하면 된다. query runner의 각 인스턴스는 독립된 데이터베이스 커넥션으로 존재한다. query runner을 사용하면 단일 데이터베이스 커넥션으로 쿼리를 제어할 수 있고 트랜잭션을 수동으로 제어할 수 있다.

### query runner 만들기

query runner의 인스턴스를 만들기 위해서는 먼저 커넥션 풀을 생성해야 한다. 커넥션이 생성되면 `createQueryRunner` 함수를 사용해 독립된 커넥션을 만든다.

```js
import { getConnection, QueryRunner } from "typeorm"

const connection: Connection = getConnection()

const queryRunner: QueryRunner = connection.createQueryRunner()
```

### query runner 사용하기

```js
import { getConnection, QueryRunner } from "typeorm"

const connection: Connection = getConnection()

const queryRunner: QueryRunner = connection.createQueryRunner()

await queryRunner.connect()
```
query runner 인스턴스를 만든 후 `connect()`를 통해 커넥션을 사용할 수 있다.


query runner은 독립된 데이터베이스 커넥션을 관리하기 위한 것이므로 재사용을 위해 사용이 끝나면 커넥션 풀로 돌려보낸다.


## Example

```js
import { getConnection, QueryRunner } from "typeorm"
import { User } from "../entity/User"

export class UserController {

    @Get("/users")
    getAll(): Promise<User[]> {
        const connection: Connection = getConnection()
        const queryRunner: QueryRunner = connection.createQueryRunner()
        await queryRunner.connect()

        const users = await queryRunner.manager.find(User)
        await queryRunner.release()
        
        return users
    }
}
```

## Transaction

단일 트랜잭션은 하나의 query runner에서만 설정할 수 있다. 수동으로 query runner 인스턴스를 생성하고 수동으로 트랜잭션 상태를 관리할 수 있다.

```js
import {getConnection} from "typeorm"

const connection = getConnection()
const queryRunner = connection.createQueryRunner()
await queryRunner.connect()

await queryRunner.query("SELECT * FROM users")

await queryRunner.startTransaction()

try{
    await queryRunner.manager.save(user1)
    await queryRunner.manager.save(user2)
    await queryRunner.manager.save(uesr3)

    await queryRunner.commitTransaction()
}catch(err){
    await queryRunner.rollbackTransaction()
}finally{
    await queryRunner.release()
}
```

- startTransaction : query runner 인스턴스 내부에서 트랜잭선을 시작한다.
- commitTransaction : query runner 인스턴스를 사용해 만든 모든 변화를 저장한다.
- rollbackTransaction : query runner 인스턴스를 사용해 만든 모든 변화를 되돌린다.