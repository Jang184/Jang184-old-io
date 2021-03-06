---
title: 서버리스 프레임워크
category: serverless
tag: TIL
---

서버리스 프로젝트에서 여러 기능을 하나의 큰 람다 기능으로 그룹화하거나 각 기능을 작은 기능으로 분리해 개발할 수 있다. 작은 기능으로 분리하는 경우, 각 기능에는 자신만의 고유한 구성과 종속성을 가지게 되므로 이런 수십 가지 기능의 배포를 관리하게 된다. 이 경우 배포 프로세스를 자동화하기는 어려울 수 있지만, 워크플로우에서 서버리스 프레임워크를 사용하면 작업이 쉬워진다. 배포 프로세스를 처리하는 것 외에도 프레임워크는 솔루션을 설계하고 다른 환경을 관리하는 데 도움이 되며 인프라의 버전을 관리하는 명확하고 간결한 문법을 제공한다.


## 서버리스 프레임워크 이해하기

프레임워크의 목적은 개발자가 생산성을 높일 수 있게 클라우드 자원을 사용하고 관리하는 방법을 단순화하는 데 있다. 프레임워크를 활용하면 새로운 프로젝트를 빠르게 시작하고 기능추가, 엔드포인트, 트리거를 추가하고 사용권한을 구성하는 등의 작업을 신속하게 할 수 있는 일련의 명령을 제공받을 수 있다. 

> 프레임워크는 코드 배포를 자동화하며 다양한 서비스와 통합하는 등의 프로젝트를 관리한다! 


### 서버리스 프레임워크의 입력으로 활용될 수 있는 것

- 통합 (Integration) : 여러 클라우드 서비스가 람다 함수를 트리거하는 방법과 내용을 설명
- 구성 (Configuration) : 람다 함수의 사용 권한 설정과 실행 환경의 제한 정의
- 플러그인 (Plugins) : 사용자 정의 코드를 이용한 프레임워크 기능 확장

### 입력 결과로 프레임워크가 제공하는 것

- 아키텍쳐 (Architecture) : 프로젝트를 일관성 있게 유지할 아키텍쳐 정의 지원
- 배포 (Deploy) : 코드 배포 자동화, 하나의 명령으로 언제든지 배포 가능
- 버저닝 (Versioning) : 인프라의 버전을 의미하는 코드 구성의 버저닝 지원


## 프로젝트 시작

새 프로젝트를 시작하면 `handler.js`파일과 `serverless.yml` 파일이 생성된다

### handler.js

hello라는 이름의 함수가 메인 항목으로 설정되며 `event`, `context`, `callback` 같은 세 개의 인수를 받는다. event는 입력 데이터, callback은 람다 실행이 완료된 후에 실행돼야 하는 함수다.

### serverless.yml

서비스를 생성할 때 aws-nodejs 인수를 사용해 서비스명과 function의 handler를 지정한다. main config file이라고 설명하고 있다.

> [참고: 서버리스 공식 홈페이지](https://www.serverless.com/framework/docs/)

기능을 제한하는 등 프로젝트의 설정 옵션을 이 파일에서 커스터마이징 할 수 있다. 메모리 크기를 제한하거나 timeout 속성을 설정할 수 있다.


## 프로젝트 배포

배포에 대한 설정은 serveless.yml 파일에서 수정할 수 있다.

```yml

provider:
    name: aws
    runtime: nodejs12.x
    stage: dev
    region: us-east-1

```

기본적으로 us-east-1로 지정된 region의 dev라는 이름의 stage에 함수를 배포한다. stage는 다른 환경을 시뮬레이션하는 데 사용된다. 예를 들어 API를 버전별로 생성하려면 개발용 버전과 운영용 버전을 만들어 사용할 수 있다. region은 람다 기능을 호스팅하는 데 사용되는 AWS 영역을 식별하는 데 사용된다. 

## 다른 AWS 자원에 접근하기

기본적으로 람다 함수는 권한 없이 실행된다. 다른 종류의 아마존 자원에 접근하려면 사용자는 접근 권한이 있어야 하며 서비스에 명시적으로 권한을 부여해야 한다. serveless.yml 파일의 provider 태그 아래에 명시한다.

# 웹사이트 호스팅

웹사이트의 정적 파일을 호스팅하는 서비스만 있으면 브라우저가 웹페이지를 렌더링하고 클라이언트단의 자바스크립트를 수행해 응답을 줄 것이다. 이 경우 프론트엔드 페이지를 호스팅하는 서비스는 아마존 S3다.

## S3에서 정적 파일 서비스

```js
버킷 생성 -> 
퍼블릭권한 관리 (읽기) 로 설정 -> 
웹 호스팅 활성화 -> 
버킷의 엔드포인트 주소 확인 -> 
정적파일 업로드 (로컬 폴더와 버킷 동기화 )
```

## 루트53 설정

```js
호스팅 영역 생성 -> DNS설정 -> 레코드셋 생성
```

## 클라우드프론트 설정

클라우드프론트 배포는 클라우드프론트가 정적 콘텐츠를 배포하기 위해서 루트 53과 연동하는 것을 가능하게 해준다. 기존에 루트53이 버킷으로 바로 연결된 것을 버킷이 아닌 클라우드프론트로 연결되게 하는 것이다.


기본적으로  클라우드프론트는 모든 파일을 24시간 동안 캐싱한다. 이 때문에 S3버킷에서 파일을 수정해도 클라우드프론트 배포를 통해서 제공되는 웹 화면에는 변화가 없다. 


### 캐싱 관리

1. 서버 : 캐시 무효화 요청을 수행한다
2. 클라이언트 : 내용을 변경할 때 모든 정적 파일에 접미사를 추가한다.