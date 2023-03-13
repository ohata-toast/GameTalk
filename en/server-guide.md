## Game > GameTalk > API v1.0 Guide

The GameTalk Server API provides the following APIs in RESTful format.

## Advance Notice

To use the Server API, the following information is required.

#### URL

The URL (server address) to call the API is as follows. The address can also be in the GameTalk console.
> https://api-gametalk-back.nhncloudservice.com

![pre_server_url_v1.0](https://static.toastoven.net/prod_gametalk/server/pre_server_url_v1.0.png)

#### AppKey

AppKey can be found in the GameTalk console.

![pre_server_appkey_v1.0](https://static.toastoven.net/prod_gametalk/server/pre_server_appkey_v1.0.png)

#### SecretKey

Secret key serves as an access control measure to the API and can be found in the GameTalk console. The secret key must be set in the HTTP header when calling the Server API.
> [Note]
If an invalid call occurs because the secret key is exposed, click **Regenerate** to create and use a new secret key.

![pre_server_secretkey_v1.0](https://static.toastoven.net/prod_gametalk/server/pre_server_secretkey_v1.0.png)

#### TransactionId

TransactionId is a feature that allows the server calling the API to manage API calls internally. When the calling server calls the API by setting a transactionId in the HTTP header, the GameTalk server delivers the result by setting the corresponding transactionId in the Response HTTP header and the Response Body Header of the result.

## Common

#### HTTP Header

When calling the API, set the following items in the HTTP header.

| Name | Required | Value |
| --- | --- | --- |
| Content-Type | mandatory | application/json; charset=UTF-8 |
| X-Secret-Key | mandatory | See SecretKey description |
| X-GT-Transaction-Id | optional | See the TransactionId description |

#### API Response

Delivers an **HTTP 200 OK** in response to all API requests. You can determine if an API request was successful by looking at the header items in the response body.

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
| transactionId | String | Value set in the HTTP header when making an API request.<br>Returns a value generated internally by GameTalk if no such value is passed. |
| isSuccessful | boolean | Successful or not |
| resultCode | int | Response code<br>Returns 0 when successful, or an error code when failed |
| resultMessage | String | Response message |

## Channel

#### Create Channel

Creates a channel.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /game-talk/v1.0/appkeys/{appKey}/channels |

**[Request Header]**

Checks common items

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud Project AppKey |

**[Request Body]**

```json
{
  "channelName" : "String",
  "channelTagIds": [1],
  "translation": false
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| channelName | String | mandatory | Channel name |
| channelTagIds | long[] | optional | Channel Tag |
| translation | boolean | optional | Availability<br/>true, false |

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
    "regUser": "String",
    "regDate": "2023-02-23T07:31:52+09:00",
    "translation": "String",
    "lastMessageId": null,
    "lastAdminMessageId": null,
    "subscriberCount": 0,
    "tagList": [
      {
        "id": 1,
        "name": "String"
      }
    ]
  }
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Object | Channel information |
| result.appKey | String | NHN Cloud Project AppKey |
| result.id | String | Channel ID |
| result.type | String | Channel type |
| result.name | String | Channel name |
| result.regUser | String  | Creator  |
| result.regDate | Date  | Date of creation. Date and time compliant with ISO 8601.  |
| result.translation | String | Whether to use translation<br/>Y (use), N (not use) |
| result.lastMessageId | String | Last message ID |
| result.lastAdminMessageId | String | Last announcement message ID |
| result.subscriberCount | int | Number of channel subscribers |
| result.tagList | Array[Object] | Channel tag list |
| result.tagList[].id | long | Channel tag ID |
| result.tagList[].name | String | Channel tag name |

#### Update Channel

Updates a channel.

**[Method, URI]**

| Method | URI |
| --- | --- |
| PUT | /game-talk/v1.0/appkeys/{appKey}/channels/{channelId} |

**[Request Header]**

Checks common items

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud Project AppKey |
| channelId | String | Channel ID |

**[Request Body]**

```json
{
  "channelName" : "String",
  "channelTagIds": [1],
  "translation": false
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| channelName | String | optional | Channel name |
| channelTagIds | long[] | optional | Channel Tag |
| translation | boolean | optional | Whether to use translation<br/>true, false |

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

Deletes a channel.

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /game-talk/v1.0/appkeys/{appKey}/channels/{channelId} |

**[Request Header]**

Checks common items

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud Project AppKey |
| channelId | String | Channel ID |

**[Request Body]**

None

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

Retrieves the list of channels.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.0/appkeys/{appKey}/channels |

**[Request Header]**

Checks common items

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud Project AppKey |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| page | int | optional | Page number (default is 0) |
| size | int | optional | Number of lists on a page (default is 10) |
| tagType | String | optional | Tag search condition (default is OR)<br/>OR, AND |
| tagList | String | optional | Tag ID list<br/>Separate each ID with comma (1, 2, 3...) |

**[Request Body]**

None

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
      "regUser" : "String",
      "regDate" : "2022-10-25T20:54:45+09:00",
      "translation" : "String",
      "lastMessageId" : null,
      "lastAdminMessageId" : null,
      "subscriberCount" : 0,
      "tagList" : [
        {
          "id" : 1,
          "name" : "String"
        }
      ]
    }
  ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| pagingInfo | Object | Paging information |
| pagingInfo.first | boolean | First page exists or not |
| pagingInfo.last | boolean | Last page exists or not |
| pagingInfo.numberOfElements | int | Number of retrieved lists on the page |
| pagingInfo.page | int | Current page number |
| pagingInfo.size | int | Number of page lists |
| pagingInfo.totalElements | int | Number of total lists |
| pagingInfo.totalPages | int | Total number of pages |
| channelList | Array[Object] | Channel list information |
| channelList[].appKey | String | NHN Cloud Project AppKey |
| channelList[].id | String | Channel ID |
| channelList[].type | String | Channel type |
| channelList[].name | String | Channel name |
| channelList[].regUser | String | Creator |
| channelList[].regDate | Date | Date of creation. Date and time compliant with ISO 8601. |
| channelList[].translation | String | Whether to use auto translation<br/>Y, N |
| channelList[].lastMessageId | String | Last message ID |
| channelList[].lastAdminMessageId | String | Last announcement message ID |
| channelList[].subscriberCount | int | Number of channel subscribers |
| channelList[].tagList | Array[Object] | Channel tag list |
| channelList[].tagList[].id | long | Channel tag ID |
| channelList[].tagList[].name | String | Channel tag name |

#### Get Channel

Retrieves a channel.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.0/appkeys/{appKey}/channels/{channelId} |

**[Request Header]**

Checks common items

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud Project AppKey |
| channelId | String | Channel ID |

**[Request Parameter]**

None

**[Request Body]**

None

**[Response Body]**

```json
{
  "header" : {
    "resultCode" : 0,
    "resultMessage" : "String",
    "transactionId" : "String",
    "isSuccessful" : true
  },
  "result" : [
    {
      "appKey" : "String",
      "id" : "String",
      "type" : "String",
      "name" : "String",
      "regUser" : "String",
      "regDate" : "2022-10-25T20:54:45+09:00",
      "translation" : "String",
      "lastMessageId" : null,
      "lastAdminMessageId" : null,
      "subscriberCount" : 0,
      "tagList" : [
        {
          "id" : 1,
          "name" : "String"
        }
      ]
    }
  ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Object | Channel list information |
| result.appKey | String | NHN Cloud Project AppKey |
| result.id | String | Channel ID |
| result.type | String | Channel type |
| result.name | String | Channel name |
| result.regUser | String | Creator |
| result.regDate | Date | Date of creation. Date and time compliant with ISO 8601. |
| result.translation | String | Whether to use translation<br/>Y (use), N (not use) |
| result.lastMessageId | String | Last message ID |
| result.lastAdminMessageId | String | Last announcement message ID |
| result.subscriberCount | int | Number of channel subscribers |
| result.tagList | Array[Object] | Channel tag list |
| result.tagList[].id | long | Channel tag ID |
| result.tagList[].name | String | Channel tag name |

#### Subscribe Channel

Subscribes to a channel.

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /game-talk/v1.0/appkeys/{appKey}/channels/{channelId}/subscribes |

**[Request Header]**

Checks common items

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud Project AppKey |
| channelId | String | Channel ID |

**[Request Body]**

```json
{
  "userId" : "String"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userId | String | mandatory | User ID |

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

Unsubscribes a channel.

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /game-talk/v1.0/appkeys/{appKey}/channels/{channelId}/subscribes |

**[Request Header]**

Checks common items

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud Project AppKey |
| channelId | String | Channel ID |

**[Request Body]**

```json
{
  "userId" : "String"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userId | String | mandatory | User ID |

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

Retrieves a channel subscriber.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.0/appkeys/{appKey}/channels/{channelId}/subscribes |

**[Request Header]**

Checks common items

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud Project AppKey |
| channelId | String | Channel ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| page | int | optional | Page number (default is 0) |
| size | int | optional | Number of lists on a page (default is 10) |

**[Request Body]**

None

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
      "regDate": "2023-02-16T11:53:05+09:00",
      "languageCode": "String",
      "valid": "String"
    }
  ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| pagingInfo | Object | Paging information |
| pagingInfo.first | boolean | First page exists or not |
| pagingInfo.last | boolean | Last page exists or not |
| pagingInfo.numberOfElements | int | Number of retrieved lists on the page |
| pagingInfo.page | int | Current page number |
| pagingInfo.size | int | Number of page lists |
| pagingInfo.totalElements | int | Number of total lists |
| pagingInfo.totalPages | int  | Total number of pages |
| userList | Array[Object] | Subscriber information |
| userList[].appKey | String | NHN Cloud Project AppKey |
| userList[].userId | String | User ID |
| userList[].regDate | Date | Date of subscription. Date and time compliant with ISO 8601. |
| userList[].languageCode | String | Language code |
| userList[].valid | String | User status<br/>Y (normal), D(withdrawl) |

## Tag

#### Get Tag List

Retrieves the list of tags.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.0/appkeys/{appKey}/tags |

**[Request Header]**

Checks common items

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud Project AppKey |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| page | int | optional | Page number (default is 0) |
| size | int | optional | Number of lists on a page (default is 10) |

**[Request Body]**

None

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
      "regDate": "2023-02-20T16:25:30+09:00",
      "modUser": "String",
      "modDate": "2023-02-21T18:21:02+09:00"
    }
  ]
}
```

| Key | Type | Description |
| --- | --- | --- |
| pagingInfo | Object | Paging information |
| pagingInfo.first | boolean | First page exists or not |
| pagingInfo.last | boolean | Last page exists or not |
| pagingInfo.numberOfElements | int | Number of retrieved lists on the page |
| pagingInfo.page | int | Current page number |
| pagingInfo.size | int | Number of page lists |
| pagingInfo.totalElements | int | Number of total lists |
| pagingInfo.totalPages | int  | Total number of pages |
| tagList | Array[Object] | Tag list information |
| tagList[].appKey | String | NHN Cloud Project AppKey |
| tagList[].id | String | Tag ID |
| tagList[].name | String | Tag name |
| tagList[].description | String | Tag description |
| tagList[].regUser | String | Creator |
| tagList[].regDate | Date | Date of creation. Date and time compliant with ISO 8601. |
| tagList[].modUser | String | Modifier |
| tagList[].modDate | Date | Date of modification. Date and time compliant with ISO 8601. |

#### Get Tag

Retrieves a tag.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.0/appkeys/{appKey}/tags/{tagId} |

**[Request Header]**

Checks common items

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud Project AppKey |
| tagId | long | Tag ID |

**[Request Parameter]**

None

**[Request Body]**

None

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
    "regDate": "2023-02-20T16:25:30+09:00",
    "modUser": "String",
    "modDate": "2023-02-21T18:21:02+09:00"
  }
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Object | Tag information |
| result.appKey | String | NHN Cloud Project AppKey |
| result.id | String | Tag ID |
| result.name | String | Tag name |
| result.description | String | Tag description |
| result.regUser | String | Creator |
| result.regDate | Date | Date of creation. Date and time compliant with ISO 8601. |
| result.modUser | String | Modifier |
| result.modDate | Date | Date of modification. Date and time compliant with ISO 8601. |

## User

#### Get User

Retrieves user information.

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.0/appkeys/{appKey}/users/{userId} |

**[Request Header]**

Checks common items

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud Project AppKey |
| userId | String | User ID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| valid | String | mandatory | User status<br/>Y (normal), D(withdrawl) |

**[Request Body]**

None

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
    "languageCode": "String",
    "regDate": "2023-02-16T11:53:05+09:00",
    "lastLoginDate": "2023-02-16T17:47:12+09:00",
    "valid": "String"
  }
}
```

| Key | Type | Description |
| --- | --- | --- |
| user | Object | User information |
| user.appKey | String | NHN Cloud Project AppKey |
| user.userId | String | User ID |
| user.languageCode | String | Language code |
| user.regDate | Date | Date of creation. Date and time compliant with ISO 8601. |
| user.lastLoginDate | Date | Date of access. Date and time compliant with ISO 8601. |
| user.valid | String | User status<br/>Y (normal), D(withdrawl) |

#### Withdraw

Unsubscribes a user.

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /game-talk/v1.0/appkeys/{appKey}/users/{userId} |

**[Request Header]**

Checks common items

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud Project AppKey |
| userId | String | User ID |

**[Request Parameter]**

None

**[Request Body]**

None

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

Updates user information.

**[Method, URI]**

| Method | URI |
| --- | --- |
| PUT | /game-talk/v1.0/appkeys/{appKey}/users/{userId} |

**[Request Header]**

Checks common items

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN Cloud Project AppKey |
| userId | String | User ID |

**[Request Parameter]**

None

**[Request Body]**

```json
{
  "languageCode" : "String"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| languageCode | String | mandatory | Language code |

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