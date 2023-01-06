## Game > GameTalk > Unity SDK Guide

This document explains GameTalk SDK for Unity environment and how to use it.

## Environments

### Unity

* 2018.4.0 or later
    * .NET 4.x or later
* Please contact [Customer Service](https://www.toast.com/kr/support/inquiry) if you need support for an earlier version of Unity.

> <font color="red">**[Caution]**</font><br/>
>  
> new apps published on Google Play since August 1, 2019 must support 64-bit architecture.
[Check Unity version supporting Google Play policy and 64 bit](https://developer.android.com/games/optimize/64-bit#unity-developers)

### Android

* Android 4.4 (API 19) or higher

### iOS

* iOS 11 or higher

### Supported Platforms

* UnityEditor
    * Support several features.
* Android
* iOS

If you call a GameTalk API that is not supported by the selected platform, an error below is returned as a callback. If there is no callback, a warning log is output.

* GameTalkErrorCode.NOT_SUPPORTED_ANDROID
* GameTalkErrorCode.NOT_SUPPORTED_IOS

Platforms supported for each API are separated by the following icons.

#### API

<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

## API

### SetDebugMode

* GameTalk only displays error logs such as warning.
* To turn on GameTalk logs for development reference, please call GameTalk.SetDebugMode(true).

> <font color="red">**[Caution]**</font><br/>
>  
> * If you need inquiry support for GameTalk, you must enable debug mode and send logs together.
> * Before **releasing** a game, make sure to delete SetDebugMode call from the source code or change the parameter to false before building.

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void SetDebugMode(bool isDebugMode)
```

**Parameter**

* bool isDebugMode: Whether the debug mode is enabled

**Example**

```cs
public void SetDebugModeSample(bool isDebugMode)
{
    Gamebase.SetDebugMode(isDebugMode);
}
```

### IsSucceeded

Use GameTalkError to check whether the API is succeeded.

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static bool IsSucceeded(GameTalkError error)
```

**Parameter**

* GameTalkError error: GameTalkError object

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

Initalize GameTalk SDK.

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
    * appKey: Appkey automatically generated when GameTalk project is activated in the console
    * languageCode: The standard language code among multilingual translation target codes registered in the console
* GameTalkCallback.GameTalkDelegate<GameTalkData.ServiceInfo> callback
    * GameTalkData.ServiceInfo
        * maxMessageLength: The largest message length registered in the conosle
        * gameTalkState: GameTalk status (Refer to**GameTalkState.cs** )
            * ACTIVATED: Enabled
            * DEACTIVATED: Disabled
            * DELETED: Deleted

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

Check whether the GameTalk SDK is initialized.

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

Register a handler to receive the server evnets.

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
        * type: Event type ( Refer to**EventType.cs** )
            * PUSH_MESSAGE
                * It is called when a new message is received on the subscribed open channel
                * Use EventDataParser's GetPushMessageData API to objectify and use data
        * Data that changes according to the event type
            * PUSH_MESSAGE                
* Data by event type
    * data(PUSH_MESSAGE)
        * messageInfoList
            * messageInfo
                * messageId: Message ID
                * channelId: Unique ID assigned when a channel is created
                * senderType: Sender type ( Refer to **MessageSenderType.cs** )
                    * USER: General user
                    * ADMIN: Administrator
                    * SYSTEM: System
                * senderId: Sender ID
                * senderNickname: Sender nicknme (If not, automatically set to senderId)
                * regDate: Message sent date
                * contentType: Message data type (Refer to **MessageContentType.cs**)
                    * TEXT: Text
                * messageList: The message entered by the sender and the translated message based on the LanguageCode are delivered together (LanguageCode can be changed in the Initialize and UpdateUserInfo APIs).
                    * message
                        * content: Message
                        * state: Message status ( Refer to **MessageState.cs**)
                            * NORMAL: Normal message
                            * FILTER: Messages filtered for profanity

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

Remove a registered handler.

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

Run GameTalk login.

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
    * idPType: IdP (identity provider) type ( Refer to**IdPType.cs**)
        * Enter IdPType.GAMEBASE when Gamebase is in use.
    * userId: User ID
        * Enter Gamebase User ID when Gamebase is in use
    * token: User authenticated token
        * Enter Gamebase-authenticated token when Gamebase is in use
* GameTalkCallback.GameTalkDelegate<GameTalkData.Auth.Login> callback
    * GameTalkData.Auth.Login
        * user
            * userId: User ID
            * valid: User Status ( Refer to**UserState.cs**)
                * Y: Normal
                * D: Deleted user
            * regDate: User’s subscription date
            * lastLoginDate: Last login date

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

Log out of GameTalk

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

Update user information.

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
    * languageCode: The standard language code among multilingual translation target codes registered in the console
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

Delete the user information.

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

Retrieve the channel information.


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
    * page: Page index (start value is 0)
    * size: Page size
    * tagType: Tag search condition ( Refer to **TagType.cs**)
        * The default is OR
        * OR: Search for channels that contain at least one selected channel tag
        * AND: Search for channels that contain all selected channel tags
    * tagList: Search tag list
* GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetChannelList> callback
    * GameTalkData.Channel.GetChannelList
        * pagingInfo
            * first: Whether it is the first page
            * last: Whether it is the last page
            * numberOfElements: Number of channels on the current page
            * page: Page index (start value is 0)
            * size: Page size
            * totalElements: The total number of channels
            * totalPages: The total number of pages
        * channelList
            * channelInfo
                * id: Channel ID
                * type: Channel type (Refer to**ChannelType.cs**)
                    * public: Open channel
                    * private: System channel, 1:1 channel
                * name: Channel name entered when creating a channel
                * subscriberCount: Number of channel subscribers
                * tagList
                    * tag
                        * id: Tag ID
                        * name: Tag name

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

Subscribe to a channel.

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
    * channelId: Unique ID assigned when a channel is created
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

Unsubscribe channels in subscription.

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
    * channelId: Unique ID assigned when a channel is created
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

Retrieve users in subscription.

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
    * channelId: Unique ID assigned when a channel is created
    * page: Page index (start value is 0)
    * size: Page size
* GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetSubscriber> callback
    * GameTalkData.Channel.GetSubscriber
        * pagingInfo
            * first: Whether it is the first page
            * last: Whether it is the last page
            * numberOfElements: Number of channels on the current page
            * page: Page index (start value is 0)
            * size: Page size
            * totalElements: The total number of channels
            * totalPages: The total number of pages
        * userList
            * user
                * id: User ID

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

Retrieve the channel’s tag list.

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
    * page: Page index (start value is 0)
    * size: Page size
* GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetChannelTagList> callback
    * GameTalkData.Channel.GetChannelTagList
        * pagingInfo
            * first: Whether it is the first page
            * last: Whether it is the last page
            * numberOfElements: Number of channels on the current page
            * page: Page index (start value is 0)
            * size: Page size
            * totalElements: The total number of channels
            * totalPages: The total number of pages
        * tagList
            * tag
                * id: Tag ID
                * name: Tag name

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

Retrieve channels in subscription.

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
    * page: Page index (start value is 0)
    * size: Page size
* GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetSubscribedChannelList> callback)
    * GameTalkData.Channel.GetSubscribedChannelList
        * pagingInfo
            * first: Whether it is the first page
            * last: Whether it is the last page
            * numberOfElements: Number of channels on the current page
            * page: Page index (start value is 0)
            * size: Page size
            * totalElements: The total number of channels
            * totalPages: The total number of pages
        * channelList
            * channelInfo
                * id: Channel ID
                * type: Channel type (Refer to**ChannelType.cs**)
                    * public: Open channel
                    * private: System channel, 1:1 channel
                * name: Channel name entered when creating a channel
                * subscriberCount: Number of channel subscribers
                * tagList
                    * tag
                        * id: Tag ID
                        * name: Tag name

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

Send a message.

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
    * senderNickname: Sender nicknme (If not, automatically set to senderId)
    * channelId: Unique ID assigned when a channel is created
    * contentType: Message data type (Refer to **MessageContentType.cs**)
        * TEXT: Text
    * content: Message
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