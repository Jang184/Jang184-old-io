---
title: DynamoDB 사용하기
category: serverless
tag: 
    - TIL
    - AWS
---

## Docker로 로컬에서 DynamoDB 실행하기

```js
$ docker pull amazon/dynamodb-local

$ docker run -d -p 8000:8000 amazon/dynamodb-local
```

`Docker Hub`에서 DynamoDB 이미지를 가져와 컨테이너를 실행한다.

## AWS CLI 설정하기

```js
$ brew install awscli

$ aws configure
```
`AWS CLI`를 설치해준 후에 IAM 사용자의 정보(access key, secret access key)를 설정해준다.

## 연결 확인하기

```js
aws dynamodb list-tables --endpoint-url http://localhost:8000
```

```js
{
    "TableNames": []
}
```
위와 같이 테이블 리스트가 조회되면서 연결된 것을 확인할 수 있다.

[참고 : AWS 사용설명서](https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/cli-services-dynamodb.html)

## DynamoDB 테이블 생성

<p>원하는 데이터 형식 샘플</p>

```js
 [
     {
         "id" : "mawbpqv87po",
         "history" : [{
             "date" : "2021-11-29T00:00:00Z",
             "play_count" : 12,
             "view_count" : 109
         },
         {
             "date" : "2021-11-30",
             "play_count" : 52,
             "view_count" : 313
         }]
     },
     {
         "id" : "kAk03ZsKRXb",
         "history" : {
             "date" : "2021-11-29T00:00Z",
             "play_count" : 32,
             "view_count" : 530
         }
     },
     ...
 ]
```



```js
$ aws dynamodb create-table \
    --table-name test-table \
    --attribute-definitions \
        AttributeName=id,AttributeType=S \
        AttributeName=history,AttributeType=L \
    --key-schema \
        AttributeName=id,KeyType=HASH \
    --provisioned-throughput ReadCapacityUnits=1,WriteCapacityUnits=1 \
    --endpoint-url http://localhost:8000
```

`test`라는 이름의 테이블을 생성했다. 테이블의 속성으로 `id`와 `date`를 가진다. id는 `partition key`로 , date는 `sort key`로 설정했다. 완성된 테이블은 아니지만 이런 식으로 CLI에서 테이블을 작성할 수 있다는 것을 확인했다.

## 데이터 넣기

NoSQL Workbench를 사용하면 쉽게 데이터를 입력할 수 있다. 또는 AWS console에서도 쉽게 데이터를 입력할 수 있다. 

<div align=center><img src='https://user-images.githubusercontent.com/81026531/143883306-7e4419f3-a021-41c5-9a14-ea042666b153.png'></div>

NoSQL 특유의 nested 구조가 잘 이해되지 않아서 AWS console에서 테이블을 생성해보면서 이해했다. 



`list` `map` 타입의 데이터 구조를 섞어 사용해야해서 조건을 설정하기가 매우 헷갈렸다. 

```js

```