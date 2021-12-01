---
title: Amazon EventBridge
category: serverless
tag:
    - TIL
---

[참고 : Serverless Docs ](https://www.serverless.com/framework/docs/providers/aws/events/event-bridge)<br>
[참고 : AWS Quick Starts](https://aws.amazon.com/ko/quickstart/eventbridge/aws-lambda/)

## Amazon EventBridge란?

> `Event Driven Architecture`을 구성하는데 도움을 주는 AWS의 서버리스 서비스!

이벤트를 받아 원하는 `lambda`로 보내주는 역할을 한다.


## AWS Lambda와 통합

<div align=center><img src='https://d1.awsstatic.com/partner-network/QuickStart/datasheets/eventbridge-blank-lambda-architecture-diagram.4a324655833386a7cb981e5f6c2ebc92c9504f43.png'></div>

`Event Rule`이 이벤트를 평가하고 일치하는 이벤트가 발생할 때 `Lambd 함수`를 호출하는 `EventBridge Event Bus`로 이벤트가 라우팅된다.



### Cron job 예약

AWS Lambda와 EventBridge를 함께 사용해 `Cron`을 사용한 예약 실행이 가능하다.

> cron(Minutes Hours Day-of-month Month Day-of-Week Year)

매일 자정 12:00는`cron(0 0 * * ? *)`로 표현할 수 있다.


cron뿐만 아니라 `rate`를 이용해 예약할 수도 있다. 