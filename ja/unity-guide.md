## Game > GameTalk > Unity SDK使用ガイド

GameTalk SDK for Unity環境および使い方を説明します。

## Environments

### Unity

* 2018.4.0以上
    * .NET 4.x以上
* 下位バージョンのUnityサポートが必要な場合は[サポート](https://www.toast.com/kr/support/inquiry)にお問い合わせください。

> <font color="red">**[注意]**</font><br/>
>  
> 2019年8月1日からGoogle Playに公開される新規アプリは、64ビットアーキテクチャをサポートする必要があります。
> [Google Playポリシーおよび64ビットサポートUnityバージョンの確認](https://developer.android.com/games/optimize/64-bit#unity-developers)

### Android

* Android 4.4 (API 19)以上

### iOS

* iOS 11以上

### Supported Platforms

* UnityEditor
    * 一部機能のみサポートします。
* Android
* iOS

選択したプラットフォームでサポートしないGameTalk APIを呼び出すと、次のようなエラーがコールバックとして返され、コールバックがない場合にはWarningログが出力されます。

* GameTalkErrorCode.NOT_SUPPORTED_ANDROID
* GameTalkErrorCode.NOT_SUPPORTED_IOS

APIごとにサポートするプラットフォームは、以下のアイコンで区分します。

#### API

<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

## API

### SetDebugMode

* GameTalkは警告とエラーログだけを表示します。
* 開発の参考になるGameTalkログをオンにするにはGameTalk.SetDebugMode(true)を呼び出してください。

> <font color="red">**[注意]**</font><br/>
>  
> * GameTalkお問い合わせサポートが必要な場合には、必ずデバッグモードを有効にした後、ログを一緒に伝えてください。
> * ゲームを**リリース**する時は、必ずソースコードでSetDebugMode呼び出しを削除するか、パラメータをfalseに変更してビルドしてください。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void SetDebugMode(bool isDebugMode)
```

**引数**

| Parameter | 必須 | 説明 |
| --- | --- | --- |
| isDebugMode | O | デバッグモードが有効かどうか |

**Example**

```cs
public void SetDebugModeExample()
{
    GameTalk.SetDebugMode(true);
}
```

### IsSucceeded

GameTalkErrorを使用してAPIの成否を確認します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static bool IsSucceeded(GameTalkError error)
```

**引数**

| Parameter | 必須 | 説明 |
| --- | --- | --- |
| error | O | GameTalkErrorオブジェクト |

**Example**

```cs
public void IsSucceededExample(GameTalkError error)
{
    if (GameTalk.IsSucceeded(error) is true)
    {
        Debug.Log("succeeded.");
    }
    else
    {
        Debug.Log("failed.");
    }
}
```

### Initialize

GameTalk SDKを初期化します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void Initialize(
    GameTalkParams.Config config,
    GameTalkCallback.GameTalkDelegate<GameTalkData.ServiceInfo> callback)
```

**引数**

| GameTalkParams.Config | 必須 | 説明 |
| --- | --- | --- |
| appKey | O | コンソールでGameTalkプロジェクトを有効にすると自動作成されるアプリケーションキー(Appkey) |
| languageCode | O | コンソールに登録された多言語翻訳対象コードのうち、基準となる言語コード |

**コールバック**

| GameTalkCallback.GameTalkDelegate<GameTalkData.ServiceInfo> | 説明 |
| --- | --- |
| <GameTalkData.ServiceInfo> data | コールバックデータ |
| error | エラーオブジェクト |

**コールバックデータ**

| GameTalkData.ServiceInfo | 説明 |
| --- | --- |
| maxMessageLength | コンソールに登録された最大メッセージ長さ |
| gameTalkState | GameTalk状態(**GameTalkState.cs** 参照)<br>- ACTIVATED：有効<br>- DEACTIVATED：無効<br>- DELETED：削除 |

**Example**

```cs
public void InitializeExample()
{
    var param = new GameTalkParams.Config
    {
        appKey = "[GAMETALK_APP_KEY]",
        languageCode = LanguageCode.Korean
    };

    GameTalk.Initialize(param, (data, error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log(string.Format("Initialize succeeded. data：{0}", data));
        }
        else
        {
            Debug.Log(string.Format("Initialize failed. error:{0}", error));
        }
    });
}
```

### IsInitialized

GameTalk SDKの初期化状況を確認します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static bool IsInitialized()
```

**Example**

```cs
public void IsInitializedExample()
{
    if (GameTalk.IsInitialized() is true)
    {
        Debug.Log("Initialized.");
    }
    else
    {
        Debug.Log("Not initialized.");
    }
}
```

### AddEvent

サーバーイベントを受信するハンドラを登録します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void AddEvent(GameTalkCallback.GameTalkDelegate<GameTalkData.AddEvent> eventHandler)
```

**引数**

| GameTalkCallback.GameTalkDelegate<GameTalkData.AddEvent> | 説明 |
| --- | --- |
| <GameTalkData.AddEvent>data | イベントデータ |
| error | エラーオブジェクト |

**イベントデータ**

| GameTalkData.AddEvent | 説明 |
| --- | --- |
| type | イベントタイプ(**GameTalkEventType.cs** 参照)<br>- CHANGE_NETWORK_STATE：ネットワークが中断されたり、再接続されたときにイベントを受信<br>- PUSH_MESSAGE：購読中のオープンチャンネルに新しいメッセージが受信されると呼び出し<br>  - EventDataParserのGetPushMessageData APIを使用してdataをオブジェクト化して使用<br>- PUSH_TO_ALL_USERS：全体送信通知メッセージが受信されると呼び出し<br>  - EventDataParserのGetPushToAllUsersData APIを使用してdataをオブジェクト化して使用<br>- PUSH_DELETE_USER： GameTalkを利用中のユーザーのチャンネル購読情報をコンソールおよびserver APIを通じて解除すると呼び出し<br>  - イベントが受信されるとゲームロジック処理後、`GameTalk.DeleteUserInfo` APIを呼び出してサーバーと状態値を同期する必要があります。<br>  - ユーザーはすべてのチャンネルの購読状態が解除されるのでチャンネルを再購読するまではすべてのメッセージの送受信が不可能です。<br>  - EventDataParserのGetPushDeleteUserData APIを使用してdataをオブジェクト化して使用 |
| data | イベントタイプによって変わるデータ(以下詳細データ参照) |
| data(CHANGE_NETWORK_STATE) | ネットワーク状態(**NetworkState.cs**参照)<br>- DISCONNECTED：ネットワーク接続解除<br>- RECONNECTED：ネットワーク再接続<br>  - ネットワークの問題で受信できなかったメッセージを照会する必要があります(Example参照). |
| data(PUSH_MESSAGE) | - channelId：チャンネル作成時に付与された固有ID<br>- channelType：チャンネルタイプ(**ChannelType.cs**参照)<br>  - PUBLIC：公開<br>  - PRIVATE：非公開<br>- messageId：メッセージID<br>- messageType：メッセージタイプ(**MessageType.cs**参照)<br>  - PUBLIC：公開<br>  - PRIVATE：非公開<br>  - ADMIN：運営者<br>  - ANNOUNCEMENT：通知メッセージ<br>  - SYSTEM：システム<br>- contentType：メッセージデータ型(**MessageContentType.cs**参照)<br>  - TEXT：テキスト<br>- senderType：送信者タイプ(**MessageSenderType.cs**参照)<br>  - USER：ユーザー<br>  - ADMIN：運営者<br>  - ANNOUNCEMENT：運営者<br>  - SYSTEM：システム<br>- senderId：送信者ID<br>- senderNickname：送信者ニックネーム<br>- languageCode：メッセージ言語コード(**LanguageCode.cs**参照)<br>- content：メッセージ<br>- state：メッセージ状態(**MessageState.cs**参照)<br>  - NORMAL：正常メッセージ<br>  - FILTER：卑属語のためフィルタリングされたメッセージ<br>- deleted：メッセージが削除されているかどうか<br>- regDate：メッセージ送信日時 |
| data(PUSH_TO_ALL_USERS) | PUSH_MESSAGEイベントデータでchannelId、channelTypeだけなくて、すべて同じ |
| data(PUSH_DELETE_USER) | - userId：ユーザーID |

**Example**

```cs
public void AddEventExample()
{
    GameTalk.AddEvent((eventData, error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            switch (eventData.type)
            {
                case GameTalkEventType.CHANGE_NETWORK_STATE：
                {
                    // ネットワーク状態が変更されると呼び出されます。
                    // NetworkState.DISCONNECTED or NetworkState.RECONNECTED
                    Debug.Log(string.Format("Change networkState：{0}", eventData.data));

                    if (eventData.data.Equals(NetworkState.RECONNECTED) is true)
                    {
                        // ネットワーク問題で受信できなかったメッセージを照会します。
                        CheckUnreceivedMessages(lastReceivedMessageId);
                    }

                    break;
                }
                case GameTalkEventType.PUSH_MESSAGE：
                {
                    // It is called when a new message is received on the subscribed open channel
                    // Use EventDataParser's GetEventData<GameTalkData.Message.PushMessage> API to objectify and use data.
                    Debug.Log(string.Format(
                        "An event has been received. data：{0}",
                        EventDataParser.GetEventData<GameTalkData.Message.PushMessage>(eventData.data)));
                    break;
                }
                case GameTalkEventType.PUSH_TO_ALL_USERS：
                {
                    // 全体送信通知メッセージを受信したら呼び出し
                    // Use EventDataParser's GetEventData<GameTalkData.Message.PushToAllUsers> API to objectify and use data
                    Debug.Log(string.Format(
                        "An event has been received. data：{0}",
                        EventDataParser.GetEventData<GameTalkData.Message.PushToAllUsers>(eventData.data)));
                    break;
                }
                case GameTalkEventType.PUSH_DELETE_USER：
                {
                    // GameTalkを利用中のユーザーのチャンネル購読情報をコンソールおよびserver APIを通じて解除する場合に呼び出し
                    // Use EventDataParser's GetEventData<GameTalkData.PushDeleteUser> API to objectify and use data
                    Debug.Log(string.Format(
                        "PUSH_DELETE_USER event was received. data：{0}",
                        EventDataParser.GetEventData<GameTalkData.PushDeleteUser>(eventData.data)));

                    // サーバーとクライアント状態を同期するにはDeleteUserInfo APIを呼び出しする必要があります。
                    DeleteUserInfo();
                    break;
                }
            }
        }
        else
        {
            Debug.Log(string.Format("error：{0}", error));
        }
    });
}

private void CheckUnreceivedMessages(long lastReceivedMessageId)
{
    var param = new GameTalkParams.Message.GetMessage
    {
        // 最後に受信したメッセージのIDです。
        messageId = lastReceivedMessageId,
        nextCount = 50,
        channelId = "{CURRENT_CHANNEL_ID}"
    };

    GameTalk.Message.GetMessage(param, (message, error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            if (message.nextMessageList.Count == 0)
            {
                // 受信できなかったメッセージがありません。

                //------------------------------
                // ゲーム処理ロジック
                //------------------------------
                // PUSH_MESSAGEイベントで受信したメッセージをUIに表示します。
            }
            else
            {
                // 受信できなかったメッセージがあります。
                // PUSH_MESSAGEイベントで受信したメッセージをUIに表示する場合にはメッセージの順序が合わない問題が発生する可能性があるため
                // 該当処理が終わるまで受信したメッセージを保管します。

                //------------------------------
                // ゲーム処理ロジック
                //------------------------------
                // 1. PUSH_MESSAGEイベントで受信したメッセージを保管します(UI表示X)。
                // 2. ゲームUIにGetMessage APIで照会したメッセージを表示します。
                // 3. 受信できなかったメッセージがなくなるまでCheckUnreceivedMessagesメソッドを再帰的に呼び出しします。
            }
        }
        else
        {
            Debug.Log(string.Format("GetMessage failed. error：{0}", error));
        }
    });
}

private void DeleteUserInfo()
{
    GameTalk.DeleteUserInfo((error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log("DeleteUserInfo succeeded.");
        }
        else
        {
            Debug.Log(string.Format("DeleteUserInfo failed. error：{0}", error));
        }
    });
}
```

### RemoveEvent

登録されたハンドラを削除します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void RemoveEvent()
```

**Example**

```cs
public void RemoveEventExample()
{
    GameTalk.RemoveEvent();
}
```

### MappingUserInfo

ユーザー認証情報をGameTalkでマッピングします。
マッピングされた情報はGameTalkユーザー識別子として使用されます。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void MappingUserInfo(
    GameTalkParams.MappingUserInfo param,
    GameTalkCallback.GameTalkDelegate<GameTalkData.MappingUserInfo> callback)
```

**引数**

| GameTalkParams.MappingUserInfo | 必須 | 説明 |
| --- | --- | --- |
| idPType | X | IdP (identity provider)タイプ(**IdPType.cs** 参照)<br>- GAMEBASE： Gamebase<br>Gamebaseを使用中の場合はIdPType.GAMEBASEを入力 |
| userId | O | ユーザーID<br>Gamebaseを使用中の場合はGamebase User IDを入力 |
| token | X | ユーザー認証トークン<br>Gamebase認証を使用中の場合はGamebase認証トークンを入力(トークンが省略されるとサーバーでエラーを返す)<br>Gamebase認証を使用しない場合は省略可能 |
| nickname | X | ユーザーニックネーム |

**コールバック**

| GameTalkCallback.GameTalkDelegate<GameTalkData.MappingUserInfo> | 説明 |
| --- | --- |
| <GameTalkData.MappingUserInfo>data | コールバックデータ |
| error | エラーオブジェクト |

**コールバックデータ**

| GameTalkData.MappingUserInfo | 説明 |
| --- | --- |
| user | **ユーザー情報**<br>- userId：ユーザーID<br>- nickname：ユーザーニックネーム<br>- valid：ユーザー状態<br>  - true：正常<br>  - false：削除されたユーザー<br>- regDate：ユーザー加入日時<br>- languageCode：ユーザー言語コード<br>- lastLoginDate：最後のログイン日時 |

**Example**

```cs
public void MappingUserInfoExample()
{
    var param = new GameTalkParams.MappingUserInfo
    {
        idPType = IdPType.GAMEBASE,
        token = "[TOKEN]",
        userId = "[USER_ID]",
        nickname = "[NICKNAME]"
    };

    GameTalk.MappingUserInfo(param, (data, error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log(string.Format("MappingUserInfo succeeded. data：{0}", data));
        }
        else
        {
            Debug.Log(string.Format("MappingUserInfo failed. error：{0}", error));
        }
    });
}
```

### IsMappedUserInfo

ユーザー認証情報がGameTalkにマッピングされているか確認します。

**API**

Supported Platforms
<span style="color：#b60205">■</span> UNITY_EDITOR
<span style="color：#0e8a16">■</span> UNITY_ANDROID
<span style="color：#1d76db">■</span> UNITY_IOS

```cs
static bool IsMappedUserInfo()
```

**Example**

```cs
public void IsMappedUserInfoExample()
{
    if (GameTalk.IsMappedUserInfo() is true)
    {
        Debug.Log("UserInfo is mapped.");
    }
    else
    {
        Debug.Log("UserInfo is not mapped.");
    }
}
```

### UnmappingUserInfo

GameTalkにマッピングされたユーザー認証情報を解除します。
GameTalk.MappingUserInfo APIが呼び出されるまで、すべてのチャットメッセージを受信できません。

**API**

Supported Platforms
<span style="color：#B60205; font-size： 10pt">■</span> UNITY_EDITOR
<span style="color：#0E8A16; font-size： 10pt">■</span> UNITY_ANDROID
<span style="color：#1D76DB; font-size： 10pt">■</span> UNITY_IOS

```cs
static void UnmappingUserInfo(GameTalkCallback.ErrorDelegate callback)
```

**引数**

| GameTalkCallback.ErrorDelegate | 説明 |
| --- | --- |
| error | エラーオブジェクト |

**Example**

```cs
public void UnmappingUserInfoExample()
{
    GameTalk.UnmappingUserInfo((error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log("UnmappingUserInfo succeeded.");
        }
        else
        {
            Debug.Log(string.Format("UnmappingUserInfo failed. error：{0}", error));
        }
    });
}
```

### UpdateUserInfo

ユーザー情報を更新します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void UpdateUserInfo(
    GameTalkParams.UpdateUserInfo param,
    GameTalkCallback.ErrorDelegate callback)
```

**引数**

| GameTalkParams.UpdateUserInfo | 必須 | 説明 |
| --- | --- | --- |
| languageCode | O | コンソールに登録された多言語翻訳対象コードのうち、基準となる言語コード |
| nickname | X | ユーザーニックネーム |

**コールバック**

| GameTalkCallback.ErrorDelegate | 説明 |
| --- | --- |
| error | エラーオブジェクト |

**Example**

```cs
public void UpdateUserInfoExample()
{
    var param = new GameTalkParams.UpdateUserInfo
    {
        languageCode = LanguageCode.English,
        nickname = "[NICKNAME]"
    };

    GameTalk.UpdateUserInfo(param, (error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log("UpdateUserInfo succeeded.");
        }
        else
        {
            Debug.Log(string.Format("UpdateUserInfo failed. error：{0}", error));
        }
    });
}
```

### DeleteUserInfo

GameTalkにマッピングされたユーザーのすべてのチャンネルの購読情報を解除します。

**API**

Supported Platforms
<span style="color：#B60205; font-size： 10pt">■</span> UNITY_EDITOR
<span style="color：#0E8A16; font-size： 10pt">■</span> UNITY_ANDROID
<span style="color：#1D76DB; font-size： 10pt">■</span> UNITY_IOS

```cs
static void DeleteUserInfo(GameTalkCallback.ErrorDelegate callback)
```

**引数**

| GameTalkCallback.ErrorDelegate | 説明 |
| --- | --- |
| error | エラーオブジェクト |

**Example**

```cs
public void DeleteUserInfoExample()
{
    GameTalk.DeleteUserInfo((error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log("DeleteUserInfo succeeded.");
        }
        else
        {
            Debug.Log(string.Format("DeleteUserInfo failed. error：{0}", error));
        }
    });
}
```

### GetChannelList

チャンネル情報を照会します。


**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void GetChannelList(
    GameTalkParams.Channel.GetChannelList param,
    GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetChannelList> callback)
```

**引数**

| GameTalkParams.Channel.GetChannelList | 必須 | 説明 |
| --- | --- | --- |
| page | X | ページインデックス<br>- 開始値は0 |
| size | O | ページサイズ<br>- 1から100の間の値 |
| tagType | X | タグ検索条件(**TagType.cs** 参照)<br>- Default： OR<br>- OR：選択したチャンネルタグを一つでも含むチャンネルを検索<br>- AND：選択したチャンネルタグを全て含むチャンネルを検索 |
| tagIdList | X | 検索タグリスト |

**コールバック**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetChannelList> | 説明 |
| --- | --- |
| <GameTalkData.Channel.GetChannelList>data | コールバックデータ |
| error | エラーオブジェクト |

**コールバックデータ**

| GameTalkData.Channel.GetChannelList | 説明 |
| --- | --- |
| pagingInfo | **ページング情報**<br>- first：最初のページかどうか<br>- last：最後のページかどうか<br>- numberOfElements：現在ページのチャンネル数<br>- page：ページインデックス<br>- size：ページサイズ<br>- totalElements：総チャンネル数<br>- totalPages：総ページ数 |
| channelList | **チャンネルリスト**<br>- id：チャンネルID<br>- type：チャンネルタイプ(**ChannelType.cs** 参照)<br>  - PUBLIC：公開<br>  - PRIVATE：非公開<br>- name：チャンネル作成時に入力したチャンネル名<br>- nickname：チャンネルエイリアス<br>- subscriberCount：チャンネル購読者数<br>- lastMessageId：最後のメッセージID<br>- autoDelete：チャンネル自動削除を行うかどうか<br>- deleted：チャンネル削除状態(trueは削除されたチャンネル)<br>- tagList：タグリスト<br>    - <span style="color：#222222"></span>id：タグID<br>    - name：タグ名 |

**Example**

```cs
public void GetChannelListExample()
{
    var param = new GameTalkParams.Channel.GetChannelList
    {
        page = 0,
        size = 10,
        tagType = TagType.OR,
        tagIdList = new List<int>() { 1, 2 }
    };

    GameTalk.Channel.GetChannelList(param, (data, error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log(string.Format("GetChannelList succeeded. data：{0}", data));
        }
        else
        {
            Debug.Log(string.Format("GetChannelList failed. error：{0}", error));
        }
    });
}
```

### SubscribeChannel

チャンネルを購読します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void SubscribeChannel(
    GameTalkParams.Channel.SubscribeChannel param,
    GameTalkCallback.ErrorDelegate callback)
```

**引数**

| GameTalkParams.Channel.SubscribeChannel | 必須 | 説明 |
| --- | --- | --- |
| channelId | O | チャンネル作成時に付与された固有ID |

**コールバック**

| GameTalkCallback.ErrorDelegate | 説明 |
| --- | --- |
| error | エラーオブジェクト |

**Example**

```cs
public void SubscribeChannelExample()
{
    var param = new GameTalkParams.Channel.SubscribeChannel
    {
        channelId = "[CHANNEL_ID]"
    };

    GameTalk.Channel.SubscribeChannel(param, (error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log("SubscribeChannel succeeded.");
        }
        else
        {
            Debug.Log(string.Format("SubscribeChannel failed. error：{0}", error));
        }
    });
}
```

### UnsubscribeChannel

購読中のチャンネルを解除します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void UnsubscribeChannel(
    GameTalkParams.Channel.UnsubscribeChannel param,
    GameTalkCallback.ErrorDelegate callback)
```

**引数**

| GameTalkParams.Channel.UnsubscribeChannel | 必須 | 説明 |
| --- | --- | --- |
| channelId | O | チャンネル作成時に付与された固有ID |

**コールバック**

| GameTalkCallback.ErrorDelegate | 説明 |
| --- | --- |
| error | エラーオブジェクト |

**Example**

```cs
public void UnsubscribeChannelExample()
{
    var param = new GameTalkParams.Channel.UnsubscribeChannel
    {
        channelId = "[CHANNEL_ID]"
    };

    GameTalk.Channel.UnsubscribeChannel(param, (error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log("UnsubscribeChannel succeeded.");
        }
        else
        {
            Debug.Log(string.Format("UnsubscribeChannel failed. error：{0}", error));
        }
    });
}
```

### GetSubscriber

購読中のユーザーを照会します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void GetSubscriber(
    GameTalkParams.Channel.GetSubscriber param,
    GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetSubscriber> callback)
```

**引数**

| GameTalkParams.Channel.GetSubscriber | 必須 | 説明 |
| --- | --- | --- |
| channelId | O | チャンネル作成時に付与された固有ID |
| page | X | ページインデックス<br>- 開始値は0 |
| size | O | ページサイズ<br>- 1から100の間の値 |

**コールバック**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetSubscriber> | 説明 |
| --- | --- |
| <GameTalkData.Channel.GetSubscriber>data | コールバックデータ |
| error | エラーオブジェクト |

**コールバックデータ**

| GameTalkData.Channel.GetSubscriber | 説明 |
| --- | --- |
| pagingInfo | **ページング情報**<br>- first：最初のページかどうか<br>- last：最後のページかどうか<br>- numberOfElements：現在ページのチャンネル数<br>- page：ページインデックス<br>- size：ページサイズ<br>- totalElements：総チャンネル数<br>- totalPages：総ページ数 |
| userList | **ユーザーリスト** <br>- userId：ユーザーID<br>- nickname：ユーザーニックネーム<br>- valid：ユーザー状態<br>  - true：正常<br>  - false：削除されたユーザー<br>- regDate：ユーザー加入日時<br>- languageCode：ユーザー言語コード<br>- lastLoginDate：最終ログイン日時 |

**Example**

```cs
public void GetSubscriberExample()
{
    var param = new GameTalkParams.Channel.GetSubscriber
    {
        channelId = "[CHANNEL_ID]",
        page = 0,
        size = 10
    };

    GameTalk.Channel.GetSubscriber(param, (data, error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log(string.Format("GetSubscriber succeeded. data:{0}", data));
        }
        else
        {
            Debug.Log(string.Format("GetSubscriber failed. error:{0}", error));
        }
    });
}
```

### GetTagList

チャンネルのタグリストを照会します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void GetTagList(
    GameTalkParams.Channel.GetTagList param,
    GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetTagList> callback)
```

**引数**

| GameTalkParams.Channel.GetTagList | 必須 | 説明 |
| --- | --- | --- |
| page | X | ページインデックス<br>- 開始値は0 |
| size | O | ページサイズ<br>- 1から100の間の値 |

**コールバック**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetTagList> | 説明 |
| --- | --- |
| <GameTalkData.Channel.GetTagList>data | コールバックデータ |
| error | エラーオブジェクト |

**コールバックデータ**

| GameTalkData.Channel.GetTagList | 説明 |
| --- | --- |
| pagingInfo | **ページング情報**<br>- first：最初のページかどうか<br>- last：最後のページかどうか<br>- numberOfElements：現在ページのチャンネル数<br>- page：ページインデックス<br>- size：ページサイズ<br>- totalElements：総チャンネル数<br>- totalPages：総ページ数 |
| tagList | **タグリスト** <br>- id：タグID<br>- name：タグ名 |

**Example**

```cs
public void GetTagListExample()
{
    var param = new GameTalkParams.Channel.GetTagList
    {
        page = 0,
        size = 10
    };

    GameTalk.Channel.GetTagList(param, (data, error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log(string.Format("GetTagList succeeded. data：{0}", data));
        }
        else
        {
            Debug.Log(string.Format("GetTagList failed. error：{0}", error));
        }
    });
}
```

### GetSubscribedChannelList

購読中のチャンネルを照会します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void GetSubscribedChannelList(
    GameTalkParams.Channel.GetSubscribedChannelList param,
    GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetSubscribedChannelList> callback)
```

**引数**

| GameTalkParams.Channel.GetSubscribedChannelList | 必須 | 説明 |
| --- | --- | --- |
| page | X | ページインデックス<br>- 開始値は0 |
| size | O | ページサイズ<br>- 1から100の間の値 |

**コールバック**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetSubscribedChannelList> | 説明 |
| --- | --- |
| <GameTalkData.Channel.GetSubscribedChannelList>data | コールバックデータ |
| error | エラーオブジェクト |

**コールバックデータ**

| GameTalkData.Channel.GetSubscribedChannelList | 説明 |
| --- | --- |
| pagingInfo | **ページング情報**<br>- first：最初のページかどうか<br>- last：最後のページかどうか<br>- numberOfElements：現在ページのチャンネル数<br>- page：ページインデックス<br>- size：ページサイズ<br>- totalElements：総チャンネル数<br>- totalPages：総ページ数 |
| channelList | **チャンネルリスト**<br>- id：チャンネルID<br>- type：チャンネルタイプ(**ChannelType.cs** 参照)<br>  - PUBLIC：公開<br>  - PRIVATE：非公開<br>- name：チャンネル作成時に入力したチャンネル名<br>- nickname：チャンネルエイリアス<br>- subscriberCount：チャンネル購読者数<br>- lastMessageId：最後のメッセージID<br>- autoDelete：チャンネル自動削除を行うかどうか<br>- deleted：チャンネル削除状態(trueは削除されたチャンネル)<br>- tagList：タグリスト<br>    - <span style="color：#222222"></span>id：タグID<br>    - name：タグ名 |

**Example**

```cs
public void GetSubscribedChannelListExample()
{
    var param = new GameTalkParams.Channel.GetSubscribedChannelList
    {
        page = 0,
        size = 10
    };

    GameTalk.Channel.GetSubscribedChannelList(param, (data, error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log(string.Format("GetSubscribedChannelList succeeded. data：{0}", data));
        }
        else
        {
            Debug.Log(string.Format("GetSubscribedChannelList failed. error：{0}", error));
        }
    });
}
```

### SendMessage

メッセージを転送します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void SendMessage(
    GameTalkParams.Message.SendMessage param,
    GameTalkCallback.ErrorDelegate callback)
```

**引数**

| GameTalkParams.Message.SendMessage | 必須 | 説明 |
| --- | --- | --- |
| channelId | O | チャンネル作成時に付与された固有ID |
| contentType | O | メッセージデータ型(**MessageContentType.cs** 参照)<br>- TEXT：テキスト |
| content | O | メッセージ |

**コールバック**

| GameTalkCallback.ErrorDelegate | 説明 |
| --- | --- |
| error | エラーオブジェクト |

**Example**

```cs
public void SendMessageExample()
{
    var param = new GameTalkParams.Message.SendMessage
    {
        channelId = "[CHANNEL_ID]",
        contentType = MessageContentType.TEXT,
        content = "[MESSAGE]"
    };

    GameTalk.Message.SendMessage(param, (error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log(string.Format("SendMessage succeeded."));
        }
        else
        {
            Debug.Log(string.Format("SendMessage failed. error：{0}", error));
        }
    });
}
```

### GetRecentlyMessage

最新メッセージを照会します。

> <font color="red">**[注意]**</font><br/>
> 
> 最新メッセージを含めて<span>最大50個</span>のメッセージを照会できます。

**API**

Supported Platforms
<span style="color：#b60205">■</span> UNITY_EDITOR
<span style="color：#0e8a16">■</span> UNITY_ANDROID
<span style="color：#1d76db">■</span> UNITY_IOS

```cs
static void GetRecentlyMessage(
    GameTalkParams.Message.GetRecentlyMessage param,
    GameTalkCallback.GameTalkDelegate<GameTalkData.Message.GetRecentlyMessage> callback)
```

**引数**

| GameTalkParams.Message.GetRecentlyMessage | 必須 | 説明 |
| --- | --- | --- |
| channelId | O | チャンネル作成時に付与された固有ID |
| count | O | 照会するメッセージ数<br>- 1から50の間の値 |

**コールバック**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Message.GetRecentlyMessage> | 説明 |
| --- | --- |
| <GameTalkData.Message.GetRecentlyMessage>data | コールバックデータ |
| error | エラーオブジェクト |

**コールバックデータ**

| GameTalkData.Message.GetRecentlyMessage | 説明 |
| --- | --- |
| count | 照会されたメッセージ数 |
| recentlyMessageList | **最近のメッセージリスト**<br>- channelId：チャンネル作成時に付与された固有ID<br>- channelType：チャンネルタイプ(**ChannelType.cs**参照)<br>  - PUBLIC：公開<br>  - PRIVATE：非公開<br>- messageId：メッセージID<br>- messageType：メッセージタイプ(**MessageType.cs**参照)<br>  - PUBLIC：公開<br>  - PRIVATE：非公開<br>  - ADMIN：運営者<br>  - ANNOUNCEMENT：通知メッセージ<br>  - SYSTEM：システム<br>- contentType：メッセージデータ型(**MessageContentType.cs**参照)<br>  - TEXT：テキスト<br>- senderType：送信者タイプ(**MessageSenderType.cs**参照)<br>  - USER：ユーザー<br>  - ADMIN：運営者<br>  - ANNOUNCEMENT：運営者<br>  - SYSTEM：システム<br>- senderId：送信者ID<br>- senderNickname：送信者ニックネーム<br>- languageCode：メッセージ言語コード(**LanguageCode.cs**参照)<br>- content：メッセージ<br>- state：メッセージ状態(**MessageState.cs**参照)<br>  - NORMAL：正常メッセージ<br>  - FILTER：卑属語のためフィルタリングされたメッセージ<br>- deleted：メッセージが削除されているかどうか<br>- regDate：メッセージ送信日時 |

**Example**

```cs
public void GetRecentlyMessageExample()
{
    var param = new GameTalkParams.Message.GetRecentlyMessage
    {
        channelId = "[CHANNEL_ID]",
        count = 10
    };

    GameTalk.Message.GetRecentlyMessage(param, (data, error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log(string.Format("GetRecentlyMessage succeeded. data：{0}", data));
        }
        else
        {
            Debug.Log(string.Format("GetRecentlyMessage failed. error：{0}", error));
        }
    });
}
```

### GetMessage

特定メッセージIDを基準にメッセージを照会します。

> <font color="red">**[注意]**</font><br/>
> 
> 特定メッセージIDを基準に<span>最大51個</span>のメッセージを照会できます。
> 引数のうちincludeの値をtrueに設定する場合、基準となるメッセージを含めて<span>合計51個<span>のメッセージが照会されます。</span></span>
> 
> prevCountとnextCountの合計が<span>1以上50以下<span>でなければなりません。</span></span>

**API**

Supported Platforms
<span style="color：#b60205">■</span> UNITY_EDITOR
<span style="color：#0e8a16">■</span> UNITY_ANDROID
<span style="color：#1d76db">■</span> UNITY_IOS

```cs
static void GetMessage(
    GameTalkParams.Message.GetMessage param,
    GameTalkCallback.GameTalkDelegate<GameTalkData.Message.GetMessage> callback)
```

**引数**

| GameTalkParams.Message.GetMessage | 必須 | 説明 |
| --- | --- | --- |
| channelId | O | チャンネル作成時に付与された固有ID |
| messageId | O | 基準メッセージID |
| prevCount | O | 基準メッセージから照会する以前メッセージ数<br>- 0から50の間の値 |
| nextCount | O | 基準メッセージから照会する次のメッセージ数<br>- 0から50の間の値 |
| include | X | 基準メッセージを含めるかどうか<br>- Default： false |

**コールバック**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Message.GetMessage> | 説明 |
| --- | --- |
| <GameTalkData.Message.GetMessage>data | コールバックデータ |
| error | エラーオブジェクト |

**コールバックデータ**

| GameTalkData.Message.GetMessage | 説明 |
| --- | --- |
| count | 照会されたメッセージ数 |
| baseMessage | **基準メッセージ(単一オブジェクト)**<br>- channelId：チャンネル作成時に付与された固有ID<br>- channelType：チャンネルタイプ(**ChannelType.cs**参照)<br>  - PUBLIC：公開<br>  - PRIVATE：非公開<br>- messageId：メッセージID<br>- messageType：メッセージタイプ(**MessageType.cs**参照)<br>  - PUBLIC：公開<br>  - PRIVATE：非公開<br>  - ADMIN：運営者<br>  - ANNOUNCEMENT：通知メッセージ<br>  - SYSTEM：システム<br>- contentType：メッセージデータ型(**MessageContentType.cs**参照)<br>  - TEXT：テキスト<br>- senderType：送信者タイプ(**MessageSenderType.cs**参照)<br>  - USER：ユーザー<br>  - ADMIN：運営者<br>  - ANNOUNCEMENT：運営者<br>  - SYSTEM：システム<br>- senderId：送信者ID<br>- senderNickname：送信者ニックネーム<br>- languageCode：メッセージ言語コード(**LanguageCode.cs**参照)<br>- content：メッセージ<br>- state：メッセージ状態(**MessageState.cs**参照)<br>  - NORMAL：正常メッセージ<br>  - FILTER：卑属語のためフィルタリングされたメッセージ<br>- deleted：メッセージが削除されているかどうか<br>- regDate：メッセージ送信日時 |
| prevMessageList | baseMessageと同構造のオブジェクトがリストとして伝達 |
| nextMessageList | baseMessageと同じ同構造のオブジェクトがリストとして伝達 |

**Example**

```cs
public void GetMessageExample()
{
    var param = new GameTalkParams.Message.GetMessage
    {
        channelId = "[CHANNEL_ID]",
        messageId = 123456,
        prevCount = 10,
        nextCount = 10,
        include = true
    };

    GameTalk.Message.GetMessage(param, (data, error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log(string.Format("GetMessage succeeded. data：{0}", data));
        }
        else
        {
            Debug.Log(string.Format("GetMessage failed. error：{0}", error));
        }
    });
}
```

### ReportMessage

特定メッセージを通報します。

**API**

Supported Platforms
<span style="color:#b60205">■</span> UNITY_EDITOR
<span style="color:#0e8a16">■</span> UNITY_ANDROID
<span style="color:#1d76db">■</span> UNITY_IOS

```cs
static void ReportMessage(
    GameTalkParams.Message.ReportMessage param,
    GameTalkCallback.ErrorDelegate callback)
```

**引数**

| GameTalkParams.Message.ReportMessage | 必須 | 説明 |
| --- | --- | --- |
| messageId | O | 申告対象メッセージID |
| messageLanguageCode | O | 申告対象メッセージ言語コード |
| channelId | O | チャンネル作成時に付与された固有ID |
| reporterNickname | X | 通報者のニックネーム |
| reason | X | 通報理由 |

**コールバック**

| GameTalkCallback.ErrorDelegate | 説明 |
| --- | --- |
| error | エラーオブジェクト |

**Example**

```cs
public void ReportMessageExample()
{
    var param = new GameTalkParams.Message.ReportMessage
    {
        channelId = "[CHANNEL_ID]",
        messageLanguageCode = LanguageCode.Korean,
        messageId = 123456,
        reporterNickname = "[REPORTER_NICKNAME]",
        reason = "[REASON]"
    };

    GameTalk.Message.ReportMessage(param, (error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            Debug.Log(string.Format("ReportMessage succeeded."));
        }
        else
        {
            Debug.Log(string.Format("ReportMessage failed. error:{0}", error));
        }
    });
}
```
