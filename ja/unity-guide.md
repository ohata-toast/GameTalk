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

**Parameter**

* bool isDebugMode：デバッグモードが有効かどうか

**Example**

```cs
public void SetDebugModeSample(bool isDebugMode)
{
    Gamebase.SetDebugMode(isDebugMode);
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

**Parameter**

* GameTalkError error: GameTalkErrorオブジェクト

**Example**

```cs
public void IsSucceeded(GameTalkError error)
{
    if (GameTalk.IsSucceeded(error) == true)
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

**Parameter**

* GameTalkParams.Config config
    * appKey：コンソールでGameTalkプロジェクト有効にする時に自動作成されるアプリケーションキー(Appkey)
    * languageCode：コンソールに登録された多言語翻訳対象コードのうち、基準となる言語コード
* GameTalkCallback.GameTalkDelegate<GameTalkData.ServiceInfo> callback
    * GameTalkData.ServiceInfo
        * maxMessageLength：コンソールに登録された最大メッセージ長
        * gameTalkState：GameTalk状態(**GameTalkState.cs**参照)
            * ACTIVATED：有効
            * DEACTIVATED：無効
            * DELETED：削除

**Example**

```cs
public void Initialize()
{
    var config = new GameTalkParams.Config
    {
        appKey = "appKey",
        languageCode = LanguageCode.Korean
    };
    
    GameTalk.Initialize(config, (data, error) =>
    {
        if (GameTalk.IsSucceeded(error) == true)
        {
            Debug.Log(string.Format("Initialize succeeded. maxMessageLength:{0}, gameTalkState:{1}", data.maxMessageLength, data.gameTalkState));
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
public void IsInitialized()
{
    if (GameTalk.IsInitialized() == true)
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

**Parameter**

* GameTalkCallback.GameTalkDelegate<GameTalkData.AddEvent> eventHandler
    * GameTalkData.AddEvent
        * type：イベントタイプ(**EventType.cs**参照)
            * PUSH_MESSAGE
                * 購読中のオープンチャンネルに新しいメッセージが受信した場合に呼び出し
                * EventDataParserのGetPushMessageData APIを使用してdataをオブジェクト化して使用
        * data：イベントタイプによって異なるデータ
            * PUSH_MESSAGE                
* イベントタイプ別データ
    * data(PUSH_MESSAGE)
        * messageInfoList
            * messageInfo
                * messageId：メッセージID
                * channelId：チャンネル作成時に付与された固有ID
                * senderType：送信者タイプ(**MessageSenderType.cs**参照)
                    * USER：一般ユーザー
                    * ADMIN：管理者
                    * SYSTEM：システム
                * senderId：送信者ID
                * senderNickname：送信者ニックネーム(ない場合はsenderIdで自動設定)
                * regDate：メッセージ送信日時
                * contentType：メッセージデータ型(**MessageContentType.cs**参照)
                    * TEXT：テキスト
                * messageList：送信者が入力したメッセージとLanguageCodeを基準に翻訳されたメッセージが一緒に伝達されます(LanguageCodeはInitialize、UpdateUserInfo APIで変更可能)。
                    * message
                        * content：メッセージ
                        * state：メッセージの状態(**MessageState.cs**参照)
                            * NORMAL：正常メッセージ
                            * FILTER：卑俗語によりフィルタリングされたメッセージ

**Example**

```cs
public void AddEvent()
{
    GameTalk.AddEvent((eventData, error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            switch (eventData.type)
            {
                case EventType.PUSH_MESSAGE:
                {
                    var pushMessageData = EventDataParser.GetPushMessageData(eventData.data);
                    var sb = new StringBuilder();
                    foreach (var messageInfo in pushMessageData.messageInfoList)
                    {
                        sb.AppendFormat("messageType:{0}, ", messageInfo.messageType); 
                        sb.AppendFormat("messageId:{0}, ", messageInfo.messageId); 
                        sb.AppendFormat("channelId:{0}, ", messageInfo.channelId); 
                        sb.AppendFormat("senderType:{0}, ", messageInfo.senderType); 
                        sb.AppendFormat("senderId:{0}, ", messageInfo.senderId); 
                        sb.AppendFormat("receiverId:{0}, ", messageInfo.receiverId); 
                        sb.AppendFormat("regDate:{0}, ", messageInfo.regDate); 
                        sb.AppendFormat("contentType:{0}, ", messageInfo.contentType);

                        foreach (var message in messageInfo.messageList)
                        {
                            sb.AppendFormat("content:{0}, ", message.content);
                            sb.AppendFormat("languageCode:{0}, ", message.languageCode);
                            sb.AppendFormat("state:{0}, ", message.state);
                        }
                        sb.AppendLine("==========");
                    }
                    
                    Debug.Log(string.Format("AddEvent succeeded. message:{0}", sb));
                }
            }
        }
        else
        {
            Debug.Log(string.Format("AddEvent failed. error:{0}", error));
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
public void RemoveEvent()
{
    GameTalk.RemoveEvent();
}
```

### Login

GameTalkログインを実行します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void Login(
    GameTalkParams.Auth.Login param,
    GameTalkCallback.GameTalkDelegate<GameTalkData.Auth.Login> callback)
```

**Parameter**

* GameTalkParams.Auth.Login param
    * idPType：IdP (identity provider)タイプ(**IdPType.cs**参照)
        * Gamebaseを使用中の場合はIdPType.GAMEBASEを入力
    * userId：ユーザーID
        * Gamebaseを使用中の場合はGamebase User IDを入力
    * token：ユーザー認証トークン
        * Gamebaseを使用中の場合はGamebase認証トークンを入力
* GameTalkCallback.GameTalkDelegate<GameTalkData.Auth.Login> callback
    * GameTalkData.Auth.Login
        * user
            * userId：ユーザーID
            * valid：ユーザー状態(**UserState.cs**参照)
                * Y：正常
                * D：削除されたユーザー
            * regDate：ユーザー加入日時
            * lastLoginDate：最終ログイン日時

**Example**

```cs
public void Login()
{
    var loginParams = new GameTalkParams.Auth.Login
    {
        idPType = IdPType.XX,
        token = "token",
        userId = "userId"
    };

    GameTalk.Auth.Login(loginParams, (data, error) =>
    {
        if (GameTalk.IsSucceeded(error) == true)
        {
            Debug.Log(string.Format("Login succeeded. userId:{0}", data.user.userId));
        }
        else
        {
            Debug.Log(string.Format("Login failed. error:{0}", error));
        }
    });
}
```

### Logout

GameTalkログアウトを実行します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void Logout(GameTalkCallback.ErrorDelegate callback)
```

**Parameter**

* GameTalkCallback.ErrorDelegate callback

**Example**

```cs
public void Logout()
{
    GameTalk.Auth.Logout((error) =>
    {
        if (GameTalk.IsSucceeded(error) == true)
        {
            Debug.Log("Logout succeeded.");
        }
        else
        {
            Debug.Log(string.Format("Logout failed. error:{0}", error));
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
    GameTalkParams.Auth.UpdateUserInfo param,
    GameTalkCallback.ErrorDelegate callback)
```

**Parameter**

* GameTalkParams.Auth.UpdateUserInfo param
    * languageCode：コンソールに登録された多言語翻訳対象コードのうち、基準となる言語コード
* GameTalkCallback.ErrorDelegate callback

**Example**

```cs
public void UpdateUserInfo()
{
    var updateUserInfoParams = new GameTalkParams.Auth.UpdateUserInfo
    {
        languageCode = LanguageCode.English
    };

    GameTalk.Auth.UpdateUserInfo(updateUserInfoParams, (error) =>
    {
        if (GameTalk.IsSucceeded(error) == true)
        {
            Debug.Log("UpdateUserInfo succeeded.");
        }
        else
        {
            Debug.Log(string.Format("UpdateUserInfo failed. error:{0}", error));
        }
    });
}
```

### Withdraw

ユーザー情報を削除します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void Withdraw(GameTalkCallback.ErrorDelegate callback)
```

**Parameter**

* GameTalkCallback.ErrorDelegate callback

**Example**

```cs
public void Withdraw()
{
    GameTalk.Auth.Withdraw((error) =>
    {
        if (GameTalk.IsSucceeded(error) == true)
        {
            Debug.Log("Withdraw succeeded.");
        }
        else
        {
            Debug.Log(string.Format("Withdraw failed. error:{0}", error));
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

**Parameter**

* GameTalkParams.Channel.GetChannelList param
    * page：ページインデックス(開始値は0)
    * size：ページサイズ
    * tagType：タグ検索条件(**TagType.cs**参照)
        * デフォルト値はOR
        * OR：選択したチャンネルタグを1つでも含むチャンネルを検索
        * AND：選択したチャンネルタグをすべて含むチャンネルを検索
    * tagList：検索タグリスト
* GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetChannelList> callback
    * GameTalkData.Channel.GetChannelList
        * pagingInfo
            * first：最初のページかどうか
            * last：最後のページかどうか
            * numberOfElements：現在ページのチャンネル数
            * page：ページインデックス(開始値は0)
            * size：ページサイズ
            * totalElements：総チャンネル数
            * totalPages：総ページ数
        * channelList
            * channelInfo
                * id：チャンネルID
                * type：チャンネルタイプ(**ChannelType.cs**参照)
                    * public：オープンチャンネル
                    * private：システムチャンネル、 1：1チャンネル
                * name：チャンネル作成時に入力したチャンネル名
                * subscriberCount：チャンネル購読者数
                * tagList
                    * tag
                        * id：タグID
                        * name：タグ名

**Example**

```cs
public void GetChannelList()
{
    var getChannelListParams = new GameTalkParams.Channel.GetChannelList
    {
        page = 0,
        size = 10,
        tagType = TagType.OR,
        tagList = new List<int> { 0, 1 }
    };

    GameTalk.Channel.GetChannelList(getChannelListParams, (data, error) =>
    {
        if (GameTalk.IsSucceeded(error) == true)
        {
            Debug.Log(string.Format("GetChannelList succeeded. channelList:{0}", data));
        }
        else
        {
            Debug.Log(string.Format("GetChannelList failed. error:{0}", error));
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

**Parameter**

* GameTalkParams.Channel.SubscribeChannel param
    * channelId：チャンネル作成時に付与された固有ID
* GameTalkCallback.ErrorDelegate callback

**Example**

```cs
public void SubscribeChannel()
{
    var subscribeChannelParams = new GameTalkParams.Channel.SubscribeChannel
    {
        channelId = "channelId"
    };

    GameTalk.Channel.SubscribeChannel(subscribeChannelParams, (error) =>
    {
        if (GameTalk.IsSucceeded(error) == true)
        {
            Debug.Log("SubscribeChannel succeeded.");
        }
        else
        {
            Debug.Log(string.Format("SubscribeChannel failed. error:{0}", error));
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

**Parameter**

* GameTalkParams.Channel.UnsubscribeChannel param
    * channelId：チャンネル作成時に付与された固有ID
* GameTalkCallback.ErrorDelegate callback


**Example**

```cs
public void UnsubscribeChannel()
{
    var subscribeChannelParams = new GameTalkParams.Channel.UnsubscribeChannel
    {
        channelId = "channelId"
    };

    GameTalk.Channel.UnsubscribeChannel(subscribeChannelParams, (error) =>
    {
        if (GameTalk.IsSucceeded(error) == true)
        {
            Debug.Log("UnsubscribeChannel succeeded.");
        }
        else
        {
            Debug.Log(string.Format("UnsubscribeChannel failed. error:{0}", error));
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

**Parameter**

* GameTalkParams.Channel.GetSubscriber param
    * channelId：チャンネル作成時に付与された固有ID
    * page：ページインデックス(開始値は0)
    * size：ページサイズ
* GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetSubscriber> callback
    * GameTalkData.Channel.GetSubscriber
        * pagingInfo
            * first：最初のページかどうか
            * last：最後のページかどうか
            * numberOfElements：現在ページのチャンネル数
            * page：ページインデックス(開始値は0)
            * size：ページサイズ
            * totalElements：総チャンネル数
            * totalPages：総ページ数
        * userList
            * user
                * id：ユーザーID

**Example**

```cs
public void GetSubscriber()
{
    var getSubscriberParams = new GameTalkParams.Channel.GetSubscriber
    {
        channelId = "channelId",
        page = 0,
        size = 10
    };

    GameTalk.Channel.GetSubscriber(getSubscriberParams, (data, error) =>
    {
        if (GameTalk.IsSucceeded(error) == true)
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

### GetChannelTagList

チャンネルのタグリストを照会します。

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void GetChannelTagList(
    GameTalkParams.Channel.GetChannelTagList param,
    GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetChannelTagList> callback)
```

**Parameter**

* GameTalkParams.Channel.GetChannelTagList param
    * page：ページインデックス(開始値は0)
    * size：ページサイズ
* GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetChannelTagList> callback
    * GameTalkData.Channel.GetChannelTagList
        * pagingInfo
            * first：最初のページかどうか
            * last：最後のページかどうか
            * numberOfElements：現在ページのチャンネル数
            * page：ページインデックス(開始値は0)
            * size：ページサイズ
            * totalElements：総チャンネル数
            * totalPages：総ページ数
        * tagList
            * tag
                * id：タグID
                * name：タグ名

**Example**

```cs
public void GetChannelTagList()
{
    var getChannelTagListParams = new GameTalkParams.Channel.GetChannelTagList
    {
        page = 0,
        size = 10
    };

    GameTalk.Channel.GetChannelTagList(getChannelTagListParams, (data, error) =>
    {
        if (GameTalk.IsSucceeded(error) == true)
        {
            Debug.Log(string.Format("GetChannelTagList succeeded. data:{0}", data));
        }
        else
        {
            Debug.Log(string.Format("GetChannelTagList failed. error:{0}", error));
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

**Parameter**

* GameTalkParams.Channel.GetSubscribedChannelList param
    * page：ページインデックス(開始値は0)
    * size：ページサイズ
* GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetSubscribedChannelList> callback)
    * GameTalkData.Channel.GetSubscribedChannelList
        * pagingInfo
            * first：最初のページかどうか
            * last：最後のページかどうか
            * numberOfElements：現在ページのチャンネル数
            * page：ページインデックス(開始値は0)
            * size：ページサイズ
            * totalElements：総チャンネル数
            * totalPages：総ページ数
        * channelList
            * channelInfo
                * id：チャンネルID
                * type：チャンネルタイプ(**ChannelType.cs**参照)
                    * public：オープンチャンネル
                    * private：システムチャンネル、 1：1チャンネル
                * name：チャンネル作成時に入力したチャンネル名
                * subscriberCount：チャンネル購読者数
                * tagList
                    * tag
                        * id：タグID
                        * name：タグ名

**Example**

```cs
public void GetSubscribedChannelList()
{
    var getSubscribedChannelListParams = new GameTalkParams.Channel.GetSubscribedChannelList
    {
        page = 0,
        size = 10
    };

    GameTalk.Channel.GetSubscribedChannelList(getSubscribedChannelListParams, (data, error) =>
    {
        if (GameTalk.IsSucceeded(error) == true)
        {
            Debug.Log(string.Format("GetSubscribedChannelList succeeded. channelList:{0}", data));
        }
        else
        {
            Debug.Log(string.Format("GetSubscribedChannelList failed. error:{0}", error));
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

**Parameter**

* GameTalkParams.Message.SendMessage param
    * senderNickname：送信者ニックネーム(ない場合はsenderIdで自動設定)
    * channelId：チャンネル作成時に付与された固有ID
    * contentType：メッセージデータ型(**MessageContentType.cs**参照)
        * TEXT：テキスト
    * content：メッセージ
* GameTalkCallback.ErrorDelegate callback

**Example**

```cs
public void SendPublicMessageToChannel()
{
    var sendMessageParams = new GameTalkParams.Message.SendMessage
    {
        channelId = "channelId",
        contentType = text,
        content = "message",
        languageCode = LanguageCode.Korean
    };

    GameTalk.Message.SendMessage(sendMessageParams, (error) =>
    {
        if (GameTalk.IsSucceeded(error) == true)
        {
            Debug.Log(string.Format("SendPublicMessageToChannel succeeded."));
        }
        else
        {
            Debug.Log(string.Format("SendPublicMessageToChannel failed. error:{0}", error));
        }
    });
}
```
