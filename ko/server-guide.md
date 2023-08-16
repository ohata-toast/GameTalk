## Game > GameTalk > API v1.1 가이드

GameTalk Server API는 RESTful 형식으로 다음과 같은 API를 제공합니다.

## Advance Notice

Server API를 사용하려면 다음과 같은 정보들이 필요합니다.

#### URL

API를 호출하기 위한 URL(서버 주소)은 다음과 같습니다. 해당 주소는 GameTalk Console 화면에서도 확인할 수 있습니다.
> https://api-gametalk-back.nhncloudservice.com

![pre_server_url_v1.0](https://static.toastoven.net/prod_gametalk/server/pre_server_url_v1.0.0.png)

#### AppKey

AppKey는 GameTalk Console에서 확인할 수 있습니다.

![pre_server_appkey_v1.0](https://static.toastoven.net/prod_gametalk/server/pre_server_appkey_v1.0.0.png)

#### SecretKey

비밀 키(secret key)는 API에 대한 접근 제어 방안으로 GameTalk Console에서 확인할 수 있습니다. 비밀 키는 Server API를 호출할 때 HTTP Header에 필수로 설정해야 합니다.
> [참고]
> 비밀 키가 외부에 노출되어 잘못된 호출이 발생할 경우 **재생성**을 클릭해 새 비밀 키를 생성한 뒤 새로 만든 비밀 키를 사용해야 합니다.

![pre_server_secretkey_v1.0](https://static.toastoven.net/prod_gametalk/server/pre_server_secretkey_v1.0.0.png)

#### TransactionId

TransactionId는 API를 호출하는 서버에서 내부적으로 API 요청을 관리할 수 있는 기능입니다. 호출 서버에서 HTTP Header에 트랜잭션 ID를 설정하여 API를 호출하면 GameTalk 서버는 응답 HTTP Header 및 응답 결과의 Response Body Header에 해당 TransactionId를 설정하여 결과를 전달합니다.

## Common

#### HTTP Header

API 호출 시 HTTP Header에 다음 항목들을 설정해야 합니다.

| Name | Required | Value |
| --- | --- | --- |
| Content-Type | mandatory | application/json; charset=UTF-8 |
| X-Secret-Key | mandatory | SecretKey 설명 참고 |
| X-GT-Transaction-Id | optional | TransactionId 설명 참고 |

#### API Response

모든 API 요청에 대한 응답으로 **HTTP 200 OK**를 전달합니다. API 요청 성공 유무는 Response Body의 Header 항목을 참고하여 판단할 수 있습니다.

**[Request]**

```
Content-Type: application/json
X-GT-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
X-Secret-Key: IgsaAP
GET https://api-gametalk-back.nhncloudservice.com
```

**[Response]**

```
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
X-GT-Transaction-Id: 88a1ae42-6b1d-48c8-894e-54e97aca07fq
```

```json
{
    "header" : {
        "transactionId": "88a1ae42-6b1d-48c8-894e-54e97aca07fq",
        "isSuccessful" : true,
        "resultCode": 0,
        "resultMessage" : "SUCCESS"
    }
}
```

| Key | Type | Description |
| --- | --- | --- |
| transactionId | String | API 요청 시 HTTP Header에 설정한 값.<br>해당 값을 전달하지 않으면 GameTalk 내부적으로 생성된 값을 반환 |
| isSuccessful | boolean | 성공 여부 |
| resultCode | int | 응답 코드<br>성공 시 0, 실패 시 오류 코드 반환 |
| resultMessage | String | 응답 메시지 |

## Channel

#### Create Channel

채널을 생성합니다.

**[Method, URI]**

| Method | URI                                       |
| --- |-------------------------------------------|
| POST | /game-talk/v1.1/appkeys/{appKey}/channels |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud 프로젝트 AppKey |

**[Request Body]**

```json
{
  "name" : "String",
  "nickname" : "String",
  "tagIdList": [1],
  "autoDelete": true
}
```

| Name | Type | Required | Value                     |
| --- | --- | --- |---------------------------|
| name | String | mandatory | 채널명                       |
| nickname | String | optional | 채널 별칭                     |
| tagIdList | long[] | optional | 채널 태그                     |
| autoDelete | boolean | optional | 채널 자동 삭제 사용 여부(기본값은 true) |

**[Response Body]**

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "String",
    "transactionId": "String",
    "isSuccessful": true
  },
  "result": {
    "appKey": "String",
    "id": "String",
    "type": "String",
    "name": "String",
    "nickname": "String",
    "autoDelete": true,
    "deleted": false,
    "lastMessageId": 0,
    "subscriberCount": 0,
    "tagList": [
      {
        "id": 1,
        "name": "String"
      }
    ],
    "regUser": "String",
    "regDate": "2023-01-01T00:00:00+09:00"
  }
}
```

| Key                    | Type          | Description                     |
|------------------------|---------------|---------------------------------|
| result                 | Object        | 채널 정보                           |
| result.appKey          | String        | 프로젝트 앱키(Appkey)           |
| result.id              | String        | 채널 ID                           |
| result.type            | String        | 채널 타입<br/>PUBLIC(일반)            |
| result.name            | String        | 채널명                             |
| result.nickname        | String        | 채널 별칭                           |
| result.autoDelete      | boolean       | 채널 자동 삭제 사용 여부                  |
| result.deleted         | boolean       | 채널 삭제 여부                        |
| result.lastMessageId   | Long          | 마지막 메시지 ID                      |
| result.subscriberCount | int           | 채널 구독자 수                        |
| result.tagList         | Array[Object] | 채널 태그 목록                        |
| result.tagList[].id    | long          | 채널 태그 ID                        |
| result.tagList[].name  | String        | 채널 태그명                          |
| result.regUser         | String        | 생성자                             |
| result.regDate         | Date          | 생성 일시. 날짜와 시간은 ISO 8601 표준에 따름. |

#### Update Channel

채널을 수정합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| PUT | /game-talk/v1.1/appkeys/{appKey}/channels/{channelId} |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud 프로젝트 AppKey |
| channelId | String | 채널 ID |

**[Request Body]**

```json
{
  "name" : "String",
  "nickname" : "String",
  "tagIdList": [1],
  "autoDelete": true
}
```

| Name | Type | Required | Value                     |
| --- | --- | --- |---------------------------|
| name | String | optional | 채널명                       |
| nickname | String | optional | 채널 별칭                     |
| tagIdList | long[] | optional | 채널 태그                     |
| autoDelete | boolean | optional | 채널 자동 삭제 사용 여부(기본값은 true) |

**[Response Body]**

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "String",
    "transactionId": "String",
    "isSuccessful": true
  }
}
```

#### Delete Channel

채널을 삭제합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /game-talk/v1.1/appkeys/{appKey}/channels/{channelId} |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud 프로젝트 AppKey |
| channelId | String | 채널 ID |

**[Request Body]**

없음

**[Response Body]**

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "String",
    "transactionId": "String",
    "isSuccessful": true
  }
}
```

#### Get Channel List

채널 목록을 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.1/appkeys/{appKey}/channels |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud 프로젝트 AppKey |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| page | int | optional | 페이지 번호(기본값은 0) |
| size | int | mandatory | 한 페이지 목록 개수(최대 100) |
| tagType | String | optional | 태그 검색 조건(기본값은 OR)<br/>OR, AND |
| tagIdList | String | optional | 태그 ID 목록<br/>각 ID를 쉼표(,)로 구분(1, 2, 3...) |

**[Request Body]**

없음

**[Response Body]**

```json
{
  "header" : {
    "resultCode" : 0,
    "resultMessage" : "String",
    "transactionId" : "String",
    "isSuccessful" : true
  },
  "pagingInfo" : {
    "first" : true,
    "last" : true,
    "numberOfElements" : 0,
    "page" : 0,
    "size" : 0,
    "totalElements" : 0,
    "totalPages" : 0
  },
  "channelList" : [
    {
      "appKey" : "String",
      "id" : "String",
      "type" : "String",
      "name" : "String",
      "nickname" : "String",
      "autoDelete" : true,
      "deleted" : false,
      "lastMessageId" : 1282796687576789,
      "subscriberCount" : 0,
      "tagList" : [
        {
          "id" : 1,
          "name" : "String"
        }
      ],
      "regUser" : "String",
      "regDate" : "2023-01-01T00:00:00+09:00",
      "modUser": "String",
      "modDate": "2023-01-01T00:00:00+09:00"
    }
  ]
}
```

| Key | Type          | Description |
| --- |---------------| --- |
| pagingInfo | Object        | 페이징 정보 |
| pagingInfo.first | boolean       | 첫 페이지 여부 |
| pagingInfo.last | boolean       | 마지막 페이지 여부 |
| pagingInfo.numberOfElements | int           | 조회된 페이지 목록 개수 |
| pagingInfo.page | int           | 현재 페이지 번호 |
| pagingInfo.size | int           | 페이지 목록 개수 |
| pagingInfo.totalElements | long          | 총 목록 개수 |
| pagingInfo.totalPages | int           | 총 페이지 개수 |
| channelList | Array[Object] | 채널 목록 정보 |
| channelList[].appKey | String        | 프로젝트 앱키(Appkey) |
| channelList[].id | String        | 채널 ID |
| channelList[].type | String        | 채널 타입<br/>PUBLIC(일반) |
| channelList[].name | String        | 채널명 |
| channelList[].nickname | String        | 채널 별칭 |
| channelList[].autoDelete | boolean       | 채널 자동 삭제 사용 여부 |
| channelList[].deleted | boolean       | 채널 삭제 여부 |
| channelList[].lastMessageId | Long          | 마지막 메시지 ID |
| channelList[].subscriberCount | int           | 채널 구독자 수 |
| channelList[].tagList | Array[Object] | 채널 태그 목록 |
| channelList[].tagList[].id | long          | 채널 태그 ID |
| channelList[].tagList[].name | String        | 채널 태그명 |
| channelList[].regUser | String        | 생성자 |
| channelList[].regDate | Date          | 생성 일시. 날짜와 시간은 ISO 8601 표준에 따름. |
| channelList[].modUser | String        | 수정자 |
| channelList[].modDate | Date          | 수정 일시. 날짜와 시간은 ISO 8601 표준에 따름. |

#### Get Channel

채널을 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.1/appkeys/{appKey}/channels/{channelId} |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud 프로젝트 AppKey |
| channelId | String | 채널 ID |

**[Request Parameter]**

없음

**[Request Body]**

없음

**[Response Body]**

```json
{
  "header" : {
    "resultCode" : 0,
    "resultMessage" : "String",
    "transactionId" : "String",
    "isSuccessful" : true
  },
  "result" : {
    "appKey" : "String",
    "id" : "String",
    "type" : "String",
    "name" : "String",
    "nickname" : "String",
    "autoDelete" : true,
    "deleted" : false,
    "lastMessageId" : 1282796687576789,
    "subscriberCount" : 0,
    "tagList" : [
    {
      "id" : 1,
      "name" : "String"
    }
    ],
    "regUser" : "String",
    "regDate" : "2023-01-01T00:00:00+09:00",
    "modUser": "String",
    "modDate": "2023-01-01T00:00:00+09:00"
  }
}
```

| Key | Type          | Description |
| --- |---------------| --- |
| result | Object        | 채널 목록 정보 |
| result.appKey | String        | 프로젝트 앱키(Appkey) |
| result.id | String        | 채널 ID |
| result.type | String        | 채널 타입<br/>PUBLIC(일반) |
| result.name | String        | 채널명 |
| result.nickname | String        | 채널 별칭 |
| result.autoDelete | boolean       | 채널 자동 삭제 사용 여부 |
| result.deleted | boolean       | 채널 삭제 여부 |
| result.lastMessageId | Long          | 마지막 메시지 ID |
| result.subscriberCount | int           | 채널 구독자 수 |
| result.tagList | Array[Object] | 채널 태그 목록 |
| result.tagList[].id | long          | 채널 태그 ID |
| result.tagList[].name | String        | 채널 태그명 |
| result.regUser | String        | 생성자 |
| result.regDate | Date          | 생성 일시. 날짜와 시간은 ISO 8601 표준에 따름. |
| result.modUser | String        | 수정자 |
| result.modDate | Date          | 수정 일시. 날짜와 시간은 ISO 8601 표준에 따름. |

#### Subscribe Channel

채널을 구독합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /game-talk/v1.1/appkeys/{appKey}/channels/{channelId}/subscribes |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud 프로젝트 AppKey |
| channelId | String | 채널 ID |

**[Request Body]**

```json
{
  "userId" : "String"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userId | String | mandatory | 유저 ID |

**[Response Body]**

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "String",
    "transactionId": "String",
    "isSuccessful": true
  }
}
```

#### Unsubscribe Channel

채널 구독을 해제합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /game-talk/v1.1/appkeys/{appKey}/channels/{channelId}/subscribes |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud 프로젝트 AppKey |
| channelId | String | 채널 ID |

**[Request Body]**

```json
{
  "userId" : "String"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userId | String | mandatory | 유저 ID |

**[Response Body]**

```json
{
  "header": {
    "resultCode": 0,
    "resultMessage": "String",
    "transactionId": "String",
    "isSuccessful": true
  }
}
```

#### Get Subscriber

채널 구독자를 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.1/appkeys/{appKey}/channels/{channelId}/subscribes |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud 프로젝트 AppKey |
| channelId | String | 채널 ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| page | int | optional | 페이지 번호(기본값은 0) |
| size | int | mandatory | 한 페이지 목록 개수(최대 100) |

**[Request Body]**

없음

**[Response Body]**

```json
{
  "header" : {
    "resultCode" : 0,
    "resultMessage" : "String",
    "transactionId" : "String",
    "isSuccessful" : true
  },
  "pagingInfo" : {
    "first" : true,
    "last" : true,
    "numberOfElements" : 0,
    "page" : 0,
    "size" : 0,
    "totalElements" : 0,
    "totalPages" : 0
  },
  "userList": [
    {
      "appKey": "String",
      "userId": "String",
      "nickname": "String",
      "languageCode": "String",
      "valid": true,
      "lastLoginDate": "2023-01-01T00:00:00+09:00",
      "regDate": "2023-01-01T00:00:00+09:00"
    }
  ]
}
```

| Key | Type          | Description                          |
| --- |---------------|--------------------------------------|
| pagingInfo | Object        | 페이징 정보                            |
| pagingInfo.first | boolean       | 첫 페이지 여부                             |
| pagingInfo.last | boolean       | 마지막 페이지 여부                           |
| pagingInfo.numberOfElements | int           | 조회된 페이지 목록 개수                        |
| pagingInfo.page | int           | 현재 페이지 번호                            |
| pagingInfo.size | int           | 페이지 목록 개수                            |
| pagingInfo.totalElements | long          | 총 목록 개수                              |
| pagingInfo.totalPages | int           | 총 페이지 개수                             |
| userList | Array[Object] | 구독자 정보                               |
| userList[].appKey | String        | 프로젝트 앱키(Appkey)                |
| userList[].userId | String        | 유저 ID                                |
| userList[].nickname | String        | 유저 닉네임                               |
| userList[].languageCode | String        | 언어 코드                                |
| userList[].valid | boolean       | 유저 상태<br/>true(정상), false(삭제)        |
| userList[].lastLoginDate | Date          | 마지막 로그인 일시. 날짜와 시간은 ISO 8601 표준에 따름. |
| userList[].regDate | Date          | 구독 일시. 날짜와 시간은 ISO 8601 표준에 따름.      |

## Tag

#### Get Tag List

태그 목록을 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.1/appkeys/{appKey}/tags |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud 프로젝트 AppKey |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| page | int | optional | 페이지 번호(기본값은 0) |
| size | int | mandatory | 한 페이지 목록 개수(최대 100) |

**[Request Body]**

없음

**[Response Body]**

```json
{
  "header" : {
    "resultCode" : 0,
    "resultMessage" : "String",
    "transactionId" : "String",
    "isSuccessful" : true
  },
  "pagingInfo" : {
    "first" : true,
    "last" : true,
    "numberOfElements" : 0,
    "page" : 0,
    "size" : 0,
    "totalElements" : 0,
    "totalPages" : 0
  },
  "tagList": [
    {
      "appKey": "String",
      "id": 1,
      "name": "String",
      "description": "String",
      "regUser": "String",
      "regDate": "2023-01-01T00:00:00+09:00",
      "modUser": "String",
      "modDate": "2023-01-01T00:00:00+09:00"
    }
  ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| pagingInfo | Object | 페이징 정보 |
| pagingInfo.first | boolean | 첫 페이지 여부 |
| pagingInfo.last | boolean | 마지막 페이지 여부 |
| pagingInfo.numberOfElements | int | 조회된 페이지 목록 개수 |
| pagingInfo.page | int | 현재 페이지 번호 |
| pagingInfo.size | int | 페이지 목록 개수 |
| pagingInfo.totalElements | long | 총 목록 개수 |
| pagingInfo.totalPages | int  | 총 페이지 개수 |
| tagList | Array[Object] | 태그 목록 정보 |
| tagList[].appKey | String | NHN Cloud 프로젝트 AppKey |
| tagList[].id | long | 태그 ID |
| tagList[].name | String | 태그명 |
| tagList[].description | String | 태그 설명 |
| tagList[].regUser | String | 생성자 |
| tagList[].regDate | Date | 생성 일시. 날짜와 시간은 ISO 8601 표준에 따름. |
| tagList[].modUser | String | 수정자 |
| tagList[].modDate | Date | 수정 일시. 날짜와 시간은 ISO 8601 표준에 따름. |

#### Get Tag

태그를 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.1/appkeys/{appKey}/tags/{tagId} |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud 프로젝트 AppKey |
| tagId | long | 태그 ID |

**[Request Parameter]**

없음

**[Request Body]**

없음

**[Response Body]**

```json
{
  "header" : {
    "resultCode" : 0,
    "resultMessage" : "String",
    "transactionId" : "String",
    "isSuccessful" : true
  },
  "result": {
    "appKey": "String",
    "id": 1,
    "name": "String",
    "description": "String",
    "regUser": "String",
    "regDate": "2023-01-01T00:00:00+09:00",
    "modUser": "String",
    "modDate": "2023-01-01T00:00:00+09:00"
  }
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Object | 태그 정보 |
| result.appKey | String | NHN Cloud 프로젝트 AppKey |
| result.id | long | 태그 ID |
| result.name | String | 태그명 |
| result.description | String | 태그 설명 |
| result.regUser | String | 생성자 |
| result.regDate | Date | 생성 일시. 날짜와 시간은 ISO 8601 표준에 따름. |
| result.modUser | String | 수정자 |
| result.modDate | Date | 수정 일시. 날짜와 시간은 ISO 8601 표준에 따름. |

## User

#### Get User

사용자 정보를 조회합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.1/appkeys/{appKey}/users/{userId} |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud 프로젝트 AppKey |
| userId | String | 유저 ID |

**[Request Parameter]**

없음

**[Request Body]**

없음

**[Response Body]**

```json
{
  "header" : {
    "resultCode" : 0,
    "resultMessage" : "String",
    "transactionId" : "String",
    "isSuccessful" : true
  },
  "user": {
    "appKey": "String",
    "userId": "String",
    "nickname": "String",
    "languageCode": "String",
    "valid": true,
    "lastLoginDate": "2023-01-01T00:00:00+09:00",
    "regDate": "2023-01-01T00:00:00+09:00"
  }
}
```

| Key | Type    | Description                          |
| --- |---------|--------------------------------------|
| user | Object  | 유저 정보                                |
| user.appKey | String  | 프로젝트 앱키(Appkey)                |
| user.userId | String  | 유저 ID                                |
| user.nickname | String  | 유저 닉네임                               |
| user.languageCode | String  | 언어 코드                                |
| user.valid | boolean | 유저 상태<br/>true(정상), false(삭제)        |
| user.lastLoginDate | Date    | 마지막 로그인 일시. 날짜와 시간은 ISO 8601 표준에 따름. |
| user.regDate | Date    | 구독 일시. 날짜와 시간은 ISO 8601 표준에 따름.      |

#### Delete User Info

사용자 정보를 삭제합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /game-talk/v1.1/appkeys/{appKey}/users/{userId} |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud 프로젝트 AppKey |
| userId | String | 유저 ID |

**[Request Parameter]**

없음

**[Request Body]**

없음

**[Response Body]**

```json
{
  "header" : {
    "resultCode" : 0,
    "resultMessage" : "String",
    "transactionId" : "String",
    "isSuccessful" : true
  }
}
```

#### Update User

사용자 정보를 수정합니다.

**[Method, URI]**

| Method | URI |
| --- | --- |
| PUT | /game-talk/v1.1/appkeys/{appKey}/users/{userId} |

**[Request Header]**

공통 사항 확인

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud 프로젝트 AppKey |
| userId | String | 유저 ID |

**[Request Parameter]**

없음

**[Request Body]**

```json
{
  "languageCode" : "String",
  "nickname" : "String"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| languageCode | String | mandatory | 언어 코드 |
| nickname | String | mandatory | 유저 닉네임 |

**[Response Body]**

```json
{
  "header" : {
    "resultCode" : 0,
    "resultMessage" : "String",
    "transactionId" : "String",
    "isSuccessful" : true
  }
}
```
