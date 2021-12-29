---
title: AWS S3 presigned url로 파일 업로드하기
category: serverless
tag:
    - aws
    - TIL
---

## presigned-url을 왜 쓸까?

### 사용자의 파일을 S3로 업로드하기

1. 사용자가 프론트로 파일 업로드
2. 프론트에서 백엔드 서버로 파일 전송
3. 백엔드 서버에서 파일을 S3로 업로드

이렇게 파일을 건네건네받아 S3로 전송하게 되면 파일 원본을 계속 주고 받아 리소스를 과도하게 사용하게 되고 효율이 떨어진다. 아마존은 프론트에서 바로 S3로 파일을 업로드할 수 있도록 `presigned url`을 발급해준다.

### presinged url을 사용해 사용자의 파일을 S3로 업로드하기

1. 사용자가 프론트로 파일 업로드
2. 프론트에서 백엔드서버로 presigned url 발급 요청
3. 백엔드 서버에서 presigned url을 발급받아 프론트로 반환
4. 프론트에서 사용자에게 받은 파일을 presigned url로 업로드

`presigned url`을 사용해 프론트에서 파일을 바로 S3로 업로드하면 파일을 이리저리 주고받지 않아도 되기때문에 리소스 절약은 물론 여러방면으로 효율적이다. presigned url은 사용기한을 정할 수 있어 특정 시간이 지나면 사용할 수 없게 된다. 또한, 발급할 때 S3 버킷 내 경로를 정하기 때문에 프론트에서 업로드할 때 따로 버킷 내 경로를 지정하지 않아도 된다.

```ts
import { S3 } from "aws-sdk"

const getPresignedUrl = () => {
    const s3: S3 = new S3()

    const url: string = s3.getSignedUrl("putObject", {
        Bucket: Bucket,
        Key: Key
    })

    return {
        statusCode: 200,
        body: JSON.stringify({
            url: url
        })
    }
}
```

프론트에서 받은 url로 파일을 전송하면 끝~! 

## S3 Event

보통 Lambda의 trigger로 `APIGateWay`를 사용한다. 예를 들어, API endpoint로 http 요청을 보내면 로직을 수행하고 response를 반환하는 싸이클이 있다. 이와 비슷하게 Lambda의 trigger로 S3를 사용할 수도 있다. 예를 들어, 특정 S3 버킷에 새로운 Object가 업로드되면 미리 지정해놓은 Lambda 함수를 실행한다.

프론트에서 S3로 파일을 업로드함과 동시에 DB에 경로를 저장해두기 위해 S3 trigger를 사용할 수 있다. S3에 Object가 업로드되면 해당 Object의 버킷 내 경로를 DB에 저장하도록 설정하면 된다. 

```yml
function:
    - updateDBFunction:
        handler: src/handler.updateDB
        timeout: 60
        events:
            - s3:
                bucket: BUCKET_NAME
                event: s3:ObjectCreated:*
            
```

serverless 를 사용할 땐 위의 코드와 같이 s3 trigger를 설정할 수 있다.