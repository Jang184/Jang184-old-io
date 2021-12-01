---
title: DynamoDB 사용하기 (2)
category: serverless
tag: 
    - TIL
    - AWS
---

### NoSQL Workbench로 데이터 넣기

CRUD를 간단하게 수행 할 수 있다.

## Condition

### attribute not exists

```js
def create_put_item_input():
    return {
        "TableName": "test-table",
        "Item": {
            "id": {"S":"b_CHaJOZyLDj"}, 
            "history": {"L": [{"M": {"date": {"S":"21-12-02"},"view": {"N":"100"},"play": {"N":"50"}}}]}
        },
        "ConditionExpression": "attribute_not_exists(#419a0)",
        "ExpressionAttributeNames": {"#419a0":"id"}
    }
```
## Update Expression

### if_not_exists

```js


```

