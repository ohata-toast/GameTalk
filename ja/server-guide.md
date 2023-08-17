## Game > GameTalk > API v1.1ガイド

GameTalk Server APIはRESTful形式で次のようなAPIを提供します。

## Advance Notice

Server APIを使用するには次のような情報が必要です。

#### URL

APIを呼び出すためのURL(サーバーアドレス)は次のとおりです。該当アドレスはGameTalk Console画面でも確認できます。
> https://api-gametalk-back.nhncloudservice.com

![pre_server_url_v1.0](https://static.toastoven.net/prod_gametalk/server/pre_server_url_v1.0.0.png)

#### AppKey

AppKeyはGameTalk Consoleで確認できます。

![pre_server_appkey_v1.0](https://static.toastoven.net/prod_gametalk/server/pre_server_appkey_v1.0.0.png)

#### SecretKey

秘密鍵(secret key)はAPIへのアクセス制御方法で、GameTalk Consoleで確認できます。秘密鍵はServer APIを呼び出す時、HTTP Headerに必ず設定する必要があります。
> [参考]
> 秘密鍵が外部に漏れて誤った呼び出しが発生する場合は、**再作成**をクリックして新しい秘密鍵を作成した後、新たに作った秘密鍵を使用する必要があります。

![pre_server_secretkey_v1.0](https://static.toastoven.net/prod_gametalk/server/pre_server_secretkey_v1.0.0.png)

#### TransactionId

TransactionIdは、APIを呼び出すサーバーで内部的にAPIリクエストを管理できる機能です。呼び出しサーバーでHTTP HeaderにトランザクションIDを設定してAPIを呼び出すとGameTalkサーバーはレスポンスHTTP Headerおよびレスポンス結果のResponse Body Headerに該当TransactionIdを設定して結果を渡します。

## Common

#### HTTP Header

API呼び出し時にHTTP Headerに次の項目を設定する必要があります。

| Name | Required | Value |
| --- | --- | --- |
| Content-Type | mandatory | application/json; charset=UTF-8 |
| X-Secret-Key | mandatory | SecretKey説明参照 |
| X-GT-Transaction-Id | optional | TransactionId説明参照 |

#### API Response

すべてのAPIリクエストに対するレスポンスとして**HTTP 200 OK**を渡します。APIリクエストが成功したかどうかはResponse BodyのHeader項目を参照して判断できます。

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
| transactionId | String | APIリクエスト時にHTTP Headerに設定した値。<br>この値を渡さない場合、GameTalk内部的に作成された値を返す |
| isSuccessful | boolean | 成否 |
| resultCode | int | レスポンスコード<br>成功時は0、失敗時はエラーコードを返す |
| resultMessage | String | レスポンスメッセージ |

## Channel

#### Create Channel

チャンネルを作成します。

**[Method, URI]**

| Method | URI                                       |
| --- |-------------------------------------------|
| POST | /game-talk/v1.1/appkeys/{appKey}/channels |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN CloudプロジェクトAppKey |

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
| name | String | mandatory | チャンネル名                    |
| nickname | String | optional | チャンネルエイリアス                  |
| tagIdList | long[] | optional | チャンネルタグ                  |
| autoDelete | boolean | optional | チャンネル自動削除を使用するかどうか(デフォルト値はtrue) |

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
| result                 | Object        | チャンネル情報                        |
| result.appKey          | String        | プロジェクトアプリケーションキー(Appkey)           |
| result.id              | String        | チャンネルID                           |
| result.type            | String        | チャンネルタイプ<br/>PUBLIC(一般)            |
| result.name            | String        | チャンネル名                          |
| result.nickname        | String        | チャンネルエイリアス                        |
| result.autoDelete      | boolean       | チャンネル自動削除を使用するかどうか                 |
| result.deleted         | boolean       | チャンネル削除有無                       |
| result.lastMessageId   | Long          | 最後のメッセージID                      |
| result.subscriberCount | int           | チャンネル購読者数                       |
| result.tagList         | Array[Object] | チャンネルタグリスト                     |
| result.tagList[].id    | long          | チャンネルタグID                        |
| result.tagList[].name  | String        | チャンネルタグ名                       |
| result.regUser         | String        | 作成者                          |
| result.regDate         | Date          | 作成日時。日付と時間はISO 8601に従う。 |

#### Update Channel

チャンネルを修正します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| PUT | /game-talk/v1.1/appkeys/{appKey}/channels/{channelId} |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN CloudプロジェクトAppKey |
| channelId | String | チャンネルID |

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
| name | String | optional | チャンネル名                    |
| nickname | String | optional | チャンネルエイリアス                  |
| tagIdList | long[] | optional | チャンネルタグ                  |
| autoDelete | boolean | optional | チャンネル自動削除を使用するかどうか(デフォルト値はtrue) |

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

チャンネルを削除します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /game-talk/v1.1/appkeys/{appKey}/channels/{channelId} |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN CloudプロジェクトAppKey |
| channelId | String | チャンネルID |

**[Request Body]**

なし

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

チャンネルリストを照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.1/appkeys/{appKey}/channels |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN CloudプロジェクトAppKey |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| page | int | optional | ページ番号(デフォルト値は0) |
| size | int | mandatory | 1ページリスト数(最大100) |
| tagType | String | optional | タグ検索条件(デフォルト値はOR)<br/>OR, AND |
| tagIdList | String | optional | タグIDリスト<br/>各IDをカンマ(,)で区切る(1, 2, 3...) |

**[Request Body]**

なし

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
| pagingInfo | Object        | ページング情報 |
| pagingInfo.first | boolean       | 最初のページかどうか |
| pagingInfo.last | boolean       | 最後のページかどうか |
| pagingInfo.numberOfElements | int           | 照会されたページリスト数 |
| pagingInfo.page | int           | 現在ページ番号 |
| pagingInfo.size | int           | ページリスト数 |
| pagingInfo.totalElements | long          | 総リスト数 |
| pagingInfo.totalPages | int           | 総ページ数 |
| channelList | Array[Object] | チャンネルリスト情報 |
| channelList[].appKey | String        | プロジェクトアプリケーションキー(Appkey) |
| channelList[].id | String        | チャンネルID |
| channelList[].type | String        | チャンネルタイプ<br/>PUBLIC(一般) |
| channelList[].name | String        | チャンネル名 |
| channelList[].nickname | String        | チャンネルエイリアス |
| channelList[].autoDelete | boolean       | チャンネル自動削除を使用するかどうか |
| channelList[].deleted | boolean       | チャンネルが削除されているかどういか |
| channelList[].lastMessageId | Long          | 最後のメッセージID |
| channelList[].subscriberCount | int           | チャンネル購読者数 |
| channelList[].tagList | Array[Object] | チャンネルタグリスト |
| channelList[].tagList[].id | long          | チャンネルタグID |
| channelList[].tagList[].name | String        | チャンネルタグ名 |
| channelList[].regUser | String        | 作成者 |
| channelList[].regDate | Date          | 作成日時。日付と時間はISO 8601に従う。 |
| channelList[].modUser | String        | 修正者 |
| channelList[].modDate | Date          | 修正日時。日付と時間はISO 8601に従う。 |

#### Get Channel

チャンネルを照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.1/appkeys/{appKey}/channels/{channelId} |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN CloudプロジェクトAppKey |
| channelId | String | チャンネルID |

**[Request Parameter]**

なし

**[Request Body]**

なし

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
| result | Object        | チャンネルリスト情報 |
| result.appKey | String        | プロジェクトアプリケーションキー(Appkey) |
| result.id | String        | チャンネルID |
| result.type | String        | チャンネルタイプ<br/>PUBLIC(一般) |
| result.name | String        | チャンネル名 |
| result.nickname | String        | チャンネルエイリアス |
| result.autoDelete | boolean       | チャンネル自動削除を使用するかどうか |
| result.deleted | boolean       | チャンネルが削除されているかどうか |
| result.lastMessageId | Long          | 最後のメッセージID |
| result.subscriberCount | int           | チャンネル購読者数 |
| result.tagList | Array[Object] | チャンネルタグリスト |
| result.tagList[].id | long          | チャンネルタグID |
| result.tagList[].name | String        | チャンネルタグ名 |
| result.regUser | String        | 作成者 |
| result.regDate | Date          | 作成日時。日付と時間はISO 8601に従う。 |
| result.modUser | String        | 修正者 |
| result.modDate | Date          | 修正日時。日付と時間はISO 8601に従う。 |

#### Subscribe Channel

チャンネルを購読します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /game-talk/v1.1/appkeys/{appKey}/channels/{channelId}/subscribes |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN CloudプロジェクトAppKey |
| channelId | String | チャンネルID |

**[Request Body]**

```json
{
  "userId" : "String"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userId | String | mandatory | ユーザーID |

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

チャンネルの購読を解除します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /game-talk/v1.1/appkeys/{appKey}/channels/{channelId}/subscribes |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN CloudプロジェクトAppKey |
| channelId | String | チャンネルID |

**[Request Body]**

```json
{
  "userId" : "String"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| userId | String | mandatory | ユーザーID |

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

チャンネル購読者を照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.1/appkeys/{appKey}/channels/{channelId}/subscribes |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN CloudプロジェクトAppKey |
| channelId | String | チャンネルID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| page | int | optional | ページ番号(デフォルト値は0) |
| size | int | mandatory | 1ページリスト数(最大100) |

**[Request Body]**

なし

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
| pagingInfo | Object        | ページング情報                         |
| pagingInfo.first | boolean       | 最初のページかどうか                            |
| pagingInfo.last | boolean       | 最後のページかどうか                          |
| pagingInfo.numberOfElements | int           | 照会されたページリスト数                     |
| pagingInfo.page | int           | 現在ページ番号                         |
| pagingInfo.size | int           | ページリスト数                         |
| pagingInfo.totalElements | long          | 総リスト数                           |
| pagingInfo.totalPages | int           | 総ページ数                          |
| userList | Array[Object] | 購読者情報                            |
| userList[].appKey | String        | プロジェクトアプリケーションキー(Appkey)                |
| userList[].userId | String        | ユーザーID                                |
| userList[].nickname | String        | ユーザーニックネーム                            |
| userList[].languageCode | String        | 言語コード                             |
| userList[].valid | boolean       | ユーザー状態<br/>true(正常)、false(削除)        |
| userList[].lastLoginDate | Date          | 最後のログイン日時。日付と時間はISO 8601に従う。 |
| userList[].regDate | Date          | 購読日時。日付と時間はISO 8601に従う。      |

## Tag

#### Get Tag List

タグリストを照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.1/appkeys/{appKey}/tags |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN CloudプロジェクトAppKey |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| page | int | optional | ページ番号(デフォルト値は0) |
| size | int | mandatory | 1ページリスト数(最大100) |

**[Request Body]**

なし

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
| pagingInfo | Object | Paging情報 |
| pagingInfo.first | boolean | 最初のページかどうか |
| pagingInfo.last | boolean | 最後のページかどうか |
| pagingInfo.numberOfElements | int | 照会されたページリスト数 |
| pagingInfo.page | int | 現在ページの番号 |
| pagingInfo.size | int | ページリスト数 |
| pagingInfo.totalElements | long | リストの総数 |
| pagingInfo.totalPages | int  | 総ページ数 |
| tagList | Array[Object] | タグリスト情報 |
| tagList[].appKey | String | NHN CloudプロジェクトAppKey |
| tagList[].id | long | タグID |
| tagList[].name | String | タグ名 |
| tagList[].description | String | タグの説明 |
| tagList[].regUser | String | コンストラクタ |
| tagList[].regDate | Date | 作成日時。日付と時間はISO 8601に従う。|
| tagList[].modUser | String | 修正者 |
| tagList[].modDate | Date | 修正日時。日付と時間はISO 8601に従う。|

#### Get Tag

タグを照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.1/appkeys/{appKey}/tags/{tagId} |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN CloudプロジェクトAppKey |
| tagId | long | タグID |

**[Request Parameter]**

なし

**[Request Body]**

なし

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
| result | Object | タグ情報 |
| result.appKey | String | NHN CloudプロジェクトAppKey |
| result.id | long | タグID |
| result.name | String | タグ名 |
| result.description | String | タグの説明 |
| result.regUser | String | コンストラクタ |
| result.regDate | Date | 作成日時。日付と時間はISO 8601に従う。|
| result.modUser | String | 修正者 |
| result.modDate | Date | 修正日時。日付と時間はISO 8601に従う。|

## User

#### Get User

ユーザー情報を照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.1/appkeys/{appKey}/users/{userId} |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN CloudプロジェクトAppKey |
| userId | String | ユーザーID |

**[Request Parameter]**

なし

**[Request Body]**

なし

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
| user | Object  | ユーザー情報                             |
| user.appKey | String  | プロジェクトアプリケーションキー(Appkey)                |
| user.userId | String  | ユーザーID                                |
| user.nickname | String  | ユーザーニックネーム                            |
| user.languageCode | String  | 言語コード                             |
| user.valid | boolean | ユーザー状態<br/>true(正常)、false(削除)        |
| user.lastLoginDate | Date    | 最後のログイン日時。日付と時間はISO 8601に従う。 |
| user.regDate | Date    | 購読日時。日付と時間はISO 8601に従う。      |

#### Delete User Info

ユーザー情報を削除します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /game-talk/v1.1/appkeys/{appKey}/users/{userId} |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN CloudプロジェクトAppKey |
| userId | String | ユーザーID |

**[Request Parameter]**

なし

**[Request Body]**

なし

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

ユーザー情報を修正します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| PUT | /game-talk/v1.1/appkeys/{appKey}/users/{userId} |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN CloudプロジェクトAppKey |
| userId | String | ユーザーID |

**[Request Parameter]**

なし

**[Request Body]**

```json
{
  "languageCode" : "String",
  "nickname" : "String"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| languageCode | String | mandatory | 言語コード |
| nickname | String | mandatory | ユーザーニックネーム |

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
