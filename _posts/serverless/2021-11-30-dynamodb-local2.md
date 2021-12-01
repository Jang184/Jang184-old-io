---
title: DynamoDB 사용하기 (2)
category: serverless
tag: 
    - TIL
    - AWS
---

## NoSQL Workbench로 데이터 넣기

CRUD를 간단하게 수행할 수 있다.
<img width="492" alt="스크린샷 2021-11-30 오후 8 23 47" src="https://user-images.githubusercontent.com/81026531/144225517-9c2c7419-e4f8-4f79-81f3-33da8fc3523f.png">


## Update Expression

### if_not_exists , list_append

```js
TableName: "test-table",
Key: { id: data.VIDEO_ID },
UpdateExpression: "SET history = list_append(if_not_exists(history, :empty_list),:p)",
ExpressionAttributeValues: {
    ":empty_list": [],
    ":p": [Item]
}
```

구글링을 하면서 꼬박 하루를 걸려 완성한 쿼리 . 아래와 같은 데이터 구조를 원했기 때문에 리스트 안에 데이터를 추가하기 위해 `list_append`를 사용했다. 하지만 기존에 ID가 존재하지 않으면 에러가 발생하기 때문에 만약 ID가 존재하지 않으면 데이터를 새로 생성할 필요가 있었다. 

1. ID 존재 -> list_append
2. ID 미존재 -> putItem

조건에 따라 케이스를 분리하기 위해서 `if not exists`와 `list_append`를 중첩해서 사용하여 해결햇다.

```js
[
     {
         "id" : "mawbpqv87po",
         "history" : [{
             "date" : "2021-11-29"
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
             "date" : "2021-11-29",
             "play_count" : 32,
             "view_count" : 530
         }
     },
     ...
 ]
```

하지만 예상치 못한 문제가 발생했다. `date`를 조건으로 쿼리를 탐색하는 로직을 작성할 예정이었는데 nested list의 데이터는 쿼리 기능을 지원하지 않는다고 한다. 데이터 설계를 다시 하든가 데이터 형식을 바꾸든가 해야 될 것 같다.



NoSQL을 처음 사용해보니 너무 자유도가 높아서 오히려 적응하기가 어렵다 ( ㅜ ㅜ )