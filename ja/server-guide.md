## Game > GameTalk > API v1.0ガイド

GameTalk Server APIはRESTful形式で次のようなAPIを提供します。

## Advance Notice

Server APIを使用するには次のような情報が必要です。

#### URL

APIを呼び出すためのURL(サーバーアドレス)は次のとおりです。該当アドレスはGameTalk Console画面でも確認できます。
> https://api-gametalk-back.nhncloudservice.com

![pre_server_url_v1.0](https://static.toastoven.net/prod_gametalk/server/pre_server_url_v1.0.png)

#### AppKey

AppKeyはGameTalk Consoleで確認できます。

![pre_server_appkey_v1.0](https://static.toastoven.net/prod_gametalk/server/pre_server_appkey_v1.0.png)

#### SecretKey

秘密鍵(secret key)はAPIへのアクセス制御方法で、GameTalk Consoleで確認できます。秘密鍵はServer APIを呼び出す時、HTTP Headerに必ず設定する必要があります。
> [参考]
> 秘密鍵が外部に漏れて誤った呼び出しが発生する場合は、**再作成**をクリックして新しい秘密鍵を作成した後、新たに作った秘密鍵を使用する必要があります。

![pre_server_secretkey_v1.0](https://static.toastoven.net/prod_gametalk/server/pre_server_secretkey_v1.0.png)

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

| Method | URI |
| --- | --- |
| POST | /game-talk/v1.0/appkeys/{appKey}/channels |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN CloudプロジェクトAppKey |

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
| channelName | String | mandatory | チャンネル名 |
| channelTagIds | long[] | optional | チャンネルタグ |
| translation | boolean | optional | 翻訳を使用するかどうか<br/>true, false |

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
| result | Object | チャンネル情報 |
| result.appKey | String | NHN CloudプロジェクトAppKey |
| result.id | String | チャンネルID |
| result.type | String | チャンネルタイプ |
| result.name | String | チャンネル名 |
| result.regUser | String  | コンストラクタ |
| result.regDate | Date  | 作成日時。日付と時間はISO 8601に従う。 |
| result.translation | String | 翻訳を使用するかどうか<br/>Y(使用)、N(未使用) |
| result.lastMessageId | String | 最後のメッセージID |
| result.lastAdminMessageId | String | 最後の告知メッセージID |
| result.subscriberCount | int | チャンネル購読者数 |
| result.tagList | Array[Object] | チャンネルタグリスト |
| result.tagList[].id | long | チャンネルタグID |
| result.tagList[].name | String | チャンネルタグ名 |

#### Update Channel

チャンネルを修正します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| PUT | /game-talk/v1.0/appkeys/{appKey}/channels/{channelId} |

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
  "channelName" : "String",
  "channelTagIds": [1],
  "translation": false
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| channelName | String | optional | チャンネル名 |
| channelTagIds | long[] | optional | チャンネルタグ |
| translation | boolean | optional | 翻訳を使用するかどうか<br/>true, false |

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
| DELETE | /game-talk/v1.0/appkeys/{appKey}/channels/{channelId} |

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
| GET | /game-talk/v1.0/appkeys/{appKey}/channels |

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
| size | int | optional | 1ページリスト数(デフォルト値は10) |
| tagType | String | optional | タグ検索条件(デフォルト値はOR)<br/>OR, AND |
| tagList | String | optional | タグIDリスト<br/>各IDをカンマ(,)で区切る(1, 2, 3...) |

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
| pagingInfo | Object | Paging情報 |
| pagingInfo.first | boolean | 最初のページかどうか |
| pagingInfo.last | boolean | 最後のページかどうか |
| pagingInfo.numberOfElements | int | 照会されたページリスト数 |
| pagingInfo.page | int | 現在ページの番号 |
| pagingInfo.size | int | ページリスト数 |
| pagingInfo.totalElements | int | リストの総数 |
| pagingInfo.totalPages | int | 総ページ数 |
| channelList | Array[Object] | チャンネルリスト情報 |
| channelList[].appKey | String | NHN CloudプロジェクトAppKey |
| channelList[].id | String | チャンネルID |
| channelList[].type | String | チャンネルタイプ |
| channelList[].name | String | チャンネル名 |
| channelList[].regUser | String | コンストラクタ |
| channelList[].regDate | Date | 作成日時。日付と時間はISO 8601に従う。|
| channelList[].translation | String | 自動翻訳を使用するかどうか<br/>Y, N |
| channelList[].lastMessageId | String | 最後のメッセージID |
| channelList[].lastAdminMessageId | String | 最後の告知メッセージID |
| channelList[].subscriberCount | int | チャンネル購読者数 |
| channelList[].tagList | Array[Object] | チャンネルタグリスト |
| channelList[].tagList[].id | long | チャンネルタグID |
| channelList[].tagList[].name | String | チャンネルタグ名 |

#### Get Channel

チャンネルを照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.0/appkeys/{appKey}/channels/{channelId} |

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
| result | Object | チャンネルリスト情報 |
| result.appKey | String | NHN CloudプロジェクトAppKey |
| result.id | String | チャンネルID |
| result.type | String | チャンネルタイプ |
| result.name | String | チャンネル名 |
| result.regUser | String | コンストラクタ |
| result.regDate | Date | 作成日時。日付と時間はISO 8601に従う。|
| result.translation | String | 翻訳を使用するかどうか<br/>Y(使用)、N(未使用) |
| result.lastMessageId | String | 最後のメッセージID |
| result.lastAdminMessageId | String | 最後の告知メッセージID |
| result.subscriberCount | int | チャンネル購読者数 |
| result.tagList | Array[Object] | チャンネルタグリスト |
| result.tagList[].id | long | チャンネルタグID |
| result.tagList[].name | String | チャンネルタグ名 |

#### Subscribe Channel

チャンネルを購読します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| POST | /game-talk/v1.0/appkeys/{appKey}/channels/{channelId}/subscribes |

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
| DELETE | /game-talk/v1.0/appkeys/{appKey}/channels/{channelId}/subscribes |

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
| GET | /game-talk/v1.0/appkeys/{appKey}/channels/{channelId}/subscribes |

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
| size | int | optional | 1ページリスト数(デフォルト値は10) |

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
      "regDate": "2023-02-16T11:53:05+09:00",
      "languageCode": "String",
      "valid": "String"
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
| pagingInfo.totalElements | int | リストの総数 |
| pagingInfo.totalPages | int  | 総ページ数 |
| userList | Array[Object] | 購読者情報 |
| userList[].appKey | String | NHN CloudプロジェクトAppKey |
| userList[].userId | String | ユーザーID |
| userList[].regDate | Date | 購読日時。日付と時間はISO 8601に従う。|
| userList[].languageCode | String | 言語コード |
| userList[].valid | String | ユーザー状態<br/>Y(正常)、D(退会) |

## Tag

#### Get Tag List

タグリストを照会します。

**[Method, URI]**

| Method | URI |
| --- | --- |
| GET | /game-talk/v1.0/appkeys/{appKey}/tags |

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
| size | int | optional | 1ページリスト数(デフォルト値は10) |

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
      "regDate": "2023-02-20T16:25:30+09:00",
      "modUser": "String",
      "modDate": "2023-02-21T18:21:02+09:00"
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
| pagingInfo.totalElements | int | リストの総数 |
| pagingInfo.totalPages | int  | 総ページ数 |
| tagList | Array[Object] | タグリスト情報 |
| tagList[].appKey | String | NHN CloudプロジェクトAppKey |
| tagList[].id | String | タグID |
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
| GET | /game-talk/v1.0/appkeys/{appKey}/tags/{tagId} |

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
    "regDate": "2023-02-20T16:25:30+09:00",
    "modUser": "String",
    "modDate": "2023-02-21T18:21:02+09:00"
  }
}
```

| Key | Type | Description |
| --- | --- | --- |
| result | Object | タグ情報 |
| result.appKey | String | NHN CloudプロジェクトAppKey |
| result.id | String | タグID |
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
| GET | /game-talk/v1.0/appkeys/{appKey}/users/{userId} |

**[Request Header]**

共通事項確認

**[Path Variable]**

| Name | Type | Value |
| --- | --- | --- |
| appKey | String | NHN CloudプロジェクトAppKey |
| userId | String | ユーザーID |

**[Request Parameter]**

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| valid | String | mandatory | ユーザー状態<br/>Y(正常)、D(退会) |

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
    "languageCode": "String",
    "regDate": "2023-02-16T11:53:05+09:00",
    "lastLoginDate": "2023-02-16T17:47:12+09:00",
    "valid": "String"
  }
}
```

| Key | Type | Description |
| --- | --- | --- |
| user | Object | ユーザー情報 |
| user.appKey | String | NHN CloudプロジェクトAppKey |
| user.userId | String | ユーザーID |
| user.languageCode | String | 言語コード |
| user.regDate | Date | 作成日時。日付と時間はISO 8601に従う。|
| user.lastLoginDate | Date | 最後の接続日時。日付と時間はISO 8601に従う。|
| user.valid | String | ユーザー状態<br/>Y(正常)、D(退会) |

#### Withdraw

ユーザーを退会させます。

**[Method, URI]**

| Method | URI |
| --- | --- |
| DELETE | /game-talk/v1.0/appkeys/{appKey}/users/{userId} |

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
| PUT | /game-talk/v1.0/appkeys/{appKey}/users/{userId} |

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
  "languageCode" : "String"
}
```

| Name | Type | Required | Value |
| --- | --- | --- | --- |
| languageCode | String | mandatory | 言語コード |

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
