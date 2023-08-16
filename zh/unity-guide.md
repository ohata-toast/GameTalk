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

### Spported Platforms

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

| Parameter | Required | Description |
| --- | --- | --- |
| isDebugMode | O | Whether to enable debug mode |

**Example**

```cs
public void SetDebugModeExample()
{
    GameTalk.SetDebugMode(true);
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

| Parameter | Required | Description |
| --- | --- | --- |
| error | O | GameTalkError object |

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

| GameTalkParams.Config | Required | Description |
| --- | --- | --- |
| appKey | O | Appkey automatically generated when GameTalk project is activated in the console |
| languageCode | O | The standard language code among multilingual translation target codes registered in the console |

**Callback**

| GameTalkCallback.GameTalkDelegate<GameTalkData.ServiceInfo> | Description |
| --- | --- |
| <GameTalkData.ServiceInfo> data | Callback Data |
| error | Error object |

**Callback Data**

| GameTalkData.ServiceInfo | Description |
| --- | --- |
| maxMessageLength | Maximum message length registered in the console |
| gameTalkState | GameTalk status (Refer to **GameTalkState.cs**)<br>- ACTIVATED: Enabled<br>- DEACTIVATED: Disabled<br>- DELETED: Deleted |

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
            Debug.Log(string.Format("Initialize succeeded. data:{0}", data));

            // If initialization is successful, you must register a handler to receive events using the AddEvent API.
            GameTalk.AddEvent((eventData, eventError) =>
            {
            });
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

Register a handler to receive the server evnets.

> <font color="red">**[Caution]**</font><br/>
> 
> Must be called before calling the MappingUserInfo API.
> If called later, messages received as events may be missed.

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void AddEvent(GameTalkCallback.GameTalkDelegate<GameTalkData.AddEvent> eventHandler)
```

**Parameter**

| GameTalkCallback.GameTalkDelegate<GameTalkData.AddEvent> | Description |
| --- | --- |
| <GameTalkData.AddEvent>data | Event data |
| error | Error object |

**Event data**

| GameTalkData.AddEvent | Description |
| --- | --- |
| type | Event type (see **GameTalkEventType.cs**)<br>- CHANGE_NETWORK_STATE: Receive an event when the network is interrupted or reconnected<br>- PUSH_MESSAGE: Called when a new message is received in the open channel you are subscribed to<br>  - Use EventDataParser's GetPushMessageData API to objectify and use data<br>- PUSH_TO_ALL_USERS: Called when announcement message sent to all is received.<br>  - Use the GetPushMessageData API of EventDataParser to objectify and use data<br>- PUSH_DELETE_USER: Called when canceling channel subscription information of a user using GameTalk through the console and server API.<br>  - When an event is received, the status value must be synchronized with the server by calling the GameTalk.DeleteUserInfo API after processing the game logic.<br>  - Since the user's subscription to all channels is canceled, all messages cannot be sent or received until the user subscribes to the channel again.<br>  - Use the GetPushDeleteUserData API of EventDataParser to objectify and use data |
| data | Data dependent on event type (see detailed data below) |
| data(CHANGE_NETWORK_STATE) | Network state (see **NetworkState.cs**)<br>- DISCONNECTED: Disconnected.<br>- RECONNECTED: Reconnected.<br>  - You must retrieve messages that were not received due to network problems (see Example). |
| data(PUSH_MESSAGE) | - channelId: Unique ID assigned when a channel is created<br>- channelType: Channel type (Refer to**ChannelType.cs**)<br>  - PUBLIC: Public<br>  - PRIVATE: Private<br>- messageId: Message ID<br>- messageType: message type (see** MessageType.cs**)<br>  - PUBLIC: Public<br>  - PRIVATE: Private<br>  - ADMIN: Administrator<br>  - ANNOUNCEMENT: Announcement message<br>  - SYSTEM: System<br>- contentType: Message data type (Refer to **MessageContentType.cs**)<br>  TEXT: Text<br>- senderType: Sender type ( Refer to **MessageSenderType.cs** )<br>  - USER: User<br>  - ADMIN: Operator<br>  - ANNOUNCEMENT: Operator<br>  - SYSTEM: System<br>- senderId: Sender ID<br>- senderNickname: Sender nickname<br>- languageCode: Message language code (see **LanguageCode.cs**)<br>- content: Message<br>- state: Message status ( Refer to **MessageState.cs**)<br>  - NORMAL: Normal message<br>  - FILTER: Messages filtered due to profane language<br>- deleted: Whether the message is deleted<br>- regDate: Message sent date |
| data(PUSH_TO_ALL_USERS) | In the PUSH_MESSAGE event data, only channelId and channelType are missing, all are the same |
| data(PUSH_DELETE_USER) |- userId: User ID |

**Example**

```cs
public void AddEventExample()
{
    GameTalk.AddEvent((eventData, error) =>
    {
        if (GameTalk.IsSucceeded(error) is true)
        {
            switch(eventData.type)
            {
                case GameTalkEventType.CHANGE_NETWORK_STATE:
                {
                    // Called when the network state changes.
                    // NetworkState.DISCONNECTED or NetworkState.RECONNECTED
                    Debug.Log(string.Format("Change networkState:{0}", eventData.data));

                    if (eventData.data.Equals(NetworkState.RECONNECTED) is true)
                    {
                        // Retrieve messages not received due to network problems.
                        CheckUnreceivedMessages(lastReceivedMessageId);
                    }

                    break;
                }
                case GameTalkEventType.PUSH_MESSAGE:
                {
                    // It is called when a new message is received on the subscribed open channel
                    // Use EventDataParser's GetEventData<GameTalkData.Message.PushMessage> API to objectify and use data.
                    Debug.Log(string.Format(
                        "An event has been received. data:{0}",
                        EventDataParser.GetEventData<GameTalkData.Message.PushMessage>(eventData.data)));
                    break;
                }
                case GameTalkEventType.PUSH_TO_ALL_USERS:
                {
                    // Called when the entire delivery notification message is received
                    // Use EventDataParser's GetEventData<GameTalkData.Message.PushToAllUsers> API to objectify and use data
                    Debug.Log(string.Format(
                        "An event has been received. data:{0}",
                        EventDataParser.GetEventData<GameTalkData.Message.PushToAllUsers>(eventData.data)));
                    break;
                }
                case GameTalkEventType.PUSH_DELETE_USER:
                {
                    // Called when the channel subscription information of the user using GameTalk is canceled through the console and server API
                    // Use EventDataParser's GetEventData<GameTalkData.PushDeleteUser> API to objectify and use data
                    Debug.Log(string.Format(
                        "PUSH_DELETE_USER event was received. data:{0}",
                        EventDataParser.GetEventData<GameTalkData.PushDeleteUser>(eventData.data)));

                    // You must call the DeleteUserInfo API to synchronize server and client state.
                    DeleteUserInfo();
                    break;
                }
            }
        }
        else
        {
            Debug.Log(string.Format("error:{0}", error));
        }
    });
}

private void CheckUnreceivedMessages(long lastReceivedMessageId)
{
    var param = new GameTalkParams.Message.GetMessage
    {
        // The ID of the last received message.
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
                // There are no unreceived messages.

                //-----------------------------
                // game processing logic
                //-----------------------------
                // Display the message received through the PUSH_MESSAGE event on the UI.
            }
            else
            {
                // There are unreceived messages.
                // If messages received through the PUSH_MESSAGE event are displayed on the UI, the order of messages may be out of order.
                // Hold the received message until its processing is finished.

                //-----------------------------
                // game processing logic
                //-----------------------------
                // 1. Archive the message received with the PUSH_MESSAGE event (UI display X).
                // 2. Display the messages retrieved by the GetMessage API on the game UI.
                // 3. Call the CheckUnreceivedMessages method recursively until there are no unreceived messages.
            }
        }
        else
        {
            Debug.Log(string.Format("GetMessage failed. error:{0}", error));
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
            Debug.Log(string.Format("DeleteUserInfo failed. error:{0}", error));
        }
    });
}
```

### MappingUserInfo

Map user credentials to GameTalk.
The mapped information serves as the GameTalk user identifier.

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

**Parameter**

| GameTalkParams.MappingUserInfo | Required | Description |
| --- | --- | --- |
| idPType | X | IdP (identity provider) type ( Refer to**IdPType.cs**)<br>- GAMEBASE: Gamebase<br>Enter IdPType.GAMEBASE when Gamebase is in use. |
| userId | O | User ID<br>Enter Gamebase User ID when Gamebase is in use |
| token | X | User authentication token<br>If using Gamebase authentication, enter the Gamebase authentication token (if the token is omitted, the server will return an error)<br>Can be omitted if Gamebase authentication is not used |
| nickname | X | User nickname |

**Callback**

| GameTalkCallback.GameTalkDelegate<GameTalkData.MappingUserInfo> | Description |
| --- | --- |
| <GameTalkData.MappingUserInfo>data | Callback Data |
| error | Error object |

**Callback Data**

| GameTalkData.MappingUserInfo | Description |
| --- | --- |
| user | **User Information**<br>- userId: User ID<br>- nickname: user nickname<br>- valid: user status<br>  - true: normal<br>  - false: deleted user<br>- regDate: User’s subscription date<br>- languageCode: user language code<br>- lastLoginDate: Last login date |

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
            Debug.Log(string.Format("MappingUserInfo succeeded. data:{0}", data));
        }
        else
        {
            Debug.Log(string.Format("MappingUserInfo failed. error:{0}", error));
        }
    });
}
```

### IsMappedUserInfo

Make sure users credentials are mapped to GameTalk.

**API**

Supported Platforms
<span style="color:#b60205">■</span> UNITY_EDITOR
<span style="color:#0e8a16">■</span> UNITY_ANDROID
<span style="color:#1d76db">■</span> UNITY_IOS

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

Disable the user credentials mapped to GameTalk.
All chat messages cannot be received until the GameTalk.MappingUserInfo API is called.

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void UnmappingUserInfo(GameTalkCallback.ErrorDelegate callback)
```

**Parameter**

| GameTalkCallback.ErrorDelegate | Description |
| --- | --- |
| error | Error object |

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
            Debug.Log(string.Format("UnmappingUserInfo failed. error:{0}", error));
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
    GameTalkParams.UpdateUserInfo param,
    GameTalkCallback.ErrorDelegate callback)
```

**Parameter**

| GameTalkParams.UpdateUserInfo | Required | Description |
| --- | --- | --- |
| languageCode | O | The standard language code among multilingual translation target codes registered in the console |
| nickname | X | User nickname |

**Callback**

| GameTalkCallback.ErrorDelegate | Description |
| --- | --- |
| error | Error object |

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
            Debug.Log(string.Format("UpdateUserInfo failed. error:{0}", error));
        }
    });
}
```

### DeleteUserInfo

Disable the subscription information on all channels for uers mapped to GameTalk.

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void DeleteUserInfo(GameTalkCallback.ErrorDelegate callback)
```

**Parameter**

| GameTalkCallback.ErrorDelegate | Description |
| --- | --- |
| error | Error object |

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
            Debug.Log(string.Format("DeleteUserInfo failed. error:{0}", error));
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

| GameTalkParams.Channel.GetChannelList | Required | Description |
| --- | --- | --- |
| page | X | Page index<br>- starting value is 0 |
| size | O | Page size<br>- A value between 1 and 100 |
| tagType | X | Tag search condition (see **TagType.cs**)<br>- Default: OR<br>- OR: Search for channels that contain at least one selected channel tag<br>- AND: Search for channels that contain all selected channel tags |
| tagIdList | X | Search tag list |

**Callback**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetChannelList> | Description |
| --- | --- |
| <GameTalkData.Channel.GetChannelList>data | Callback Data |
| error | Error object |

**Callback Data**

| GameTalkData.Channel.GetChannelList | Description |
| --- | --- |
| pagingInfo | **Paging information**<br>- first: Whether it is the first page<br>- last: Whether it is the last page<br>- numberOfElements: Number of channels on the current page<br>- page: page index<br>- size: Page size<br>- totalElements: The total number of channels<br>- totalPages: The total number of pages |
| channelList | **Channel List**<br>- id: Channel ID<br>- type: Channel type (Refer to**ChannelType.cs**)<br>  - PUBLIC: Public<br>  - PRIVATE: Private<br>- name: Channel name entered when creating a channel<br>- nickname: channel nickname<br>- subscriberCount: Number of channel subscribers<br>- lastMessageId: last message id<br>- autoDelete: whether to automatically delete the channel<br>- deleted: channel deleted status (if true, deleted channel)<br>- tagList: tag list<br>  - id: Tag ID<br>  - name: Tag name |

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
            Debug.Log(string.Format("GetChannelList succeeded. data:{0}", data));
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

| GameTalkParams.Channel.SubscribeChannel | Required | Description |
| --- | --- | --- |
| channelId | O | Unique ID given at channel creation |

**Callback**

| GameTalkCallback.ErrorDelegate | Description |
| --- | --- |
| error | Error object |

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

| GameTalkParams.Channel.UnsubscribeChannel | Required | Description |
| --- | --- | --- |
| channelId | O | Unique ID given at channel creation |

**Callback**

| GameTalkCallback.ErrorDelegate | Description |
| --- | --- |
| error | Error object |

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

| GameTalkParams.Channel.GetSubscriber | Required | Description |
| --- | --- | --- |
| channelId | O | Unique ID given at channel creation |
| page | X | Page index<br>- starting value is 0 |
| size | O | Page size<br>- A value between 1 and 100 |

**Callback**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetSubscriber> | Description |
| --- | --- |
| <GameTalkData.Channel.GetSubscriber>data | Callback Data |
| error | Error object |

**Callback Data**

| GameTalkData.Channel.GetSubscriber | Description |
| --- | --- |
| pagingInfo | **Paging information**<br>- first: Whether it is the first page<br>- last: Whether it is the last page<br>- numberOfElements: Number of channels on the current page<br>- page: page index<br>- size: Page size<br>- totalElements: The total number of channels<br>- totalPages: The total number of pages |
| userList | **User List** <br>- userId: User ID<br>- nickname: user nickname<br>- valid: user status<br>  - true: normal<br>  - false: deleted user<br>- regDate: User’s subscription date<br>- languageCode: user language code<br>- lastLoginDate: Last login date |

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

Retrieve the channel’s tag list.

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

**Parameter**

| GameTalkParams.Channel.GetTagList | Required | Description |
| --- | --- | --- |
| page | X | Page index<br>- starting value is 0 |
| size | O | Page size<br>- A value between 1 and 100 |

**Callback**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetTagList> | Description |
| --- | --- |
| <GameTalkData.Channel.GetTagList>data | Callback Data |
| error | Error object |

**Callback Data**

| GameTalkData.Channel.GetTagList | Description |
| --- | --- |
| pagingInfo | **Paging information**<br>- first: Whether it is the first page<br>- last: Whether it is the last page<br>- numberOfElements: Number of channels on the current page<br>- page: page index<br>- size: Page size<br>- totalElements: The total number of channels<br>- totalPages: The total number of pages |
| tagList | **Tag List** <br>- id: Tag ID<br>- name: Tag name |

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
            Debug.Log(string.Format("GetTagList succeeded. data:{0}", data));
        }
        else
        {
            Debug.Log(string.Format("GetTagList failed. error:{0}", error));
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

| GameTalkParams.Channel.GetSubscribedChannelList | Required | Description |
| --- | --- | --- |
| page | X | Page index<br>- starting value is 0 |
| size | O | Page size<br>- A value between 1 and 100 |

**Callback**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetSubscribedChannelList> | Description |
| --- | --- |
| <GameTalkData.Channel.GetSubscribedChannelList>data | Callback Data |
| error | Error object |

**Callback Data**

| GameTalkData.Channel.GetSubscribedChannelList | Description |
| --- | --- |
| pagingInfo | **Paging information**<br>- first: Whether it is the first page<br>- last: Whether it is the last page<br>- numberOfElements: Number of channels on the current page<br>- page: page index<br>- size: Page size<br>- totalElements: The total number of channels<br>- totalPages: The total number of pages |
| channelList | **Channel List**<br>- id: Channel ID<br>- type: Channel type (Refer to**ChannelType.cs**)<br>  - PUBLIC: Public<br>  - PRIVATE: Private<br>- name: Channel name entered when creating a channel<br>- nickname: channel nickname<br>- subscriberCount: Number of channel subscribers<br>- lastMessageId: last message id<br>- autoDelete: whether to automatically delete the channel<br>- deleted: channel deleted status (if true, deleted channel)<br>- tagList: tag list<br>  - iid: Tag ID<br>  - name: Tag name |

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
            Debug.Log(string.Format("GetSubscribedChannelList succeeded. data:{0}", data));
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

| GameTalkParams.Message.SendMessage | Required | Description |
| --- | --- | --- |
| channelId | O | Unique ID given at channel creation |
| contentType | O | Message data type (Refer to **MessageContentType.cs**)<br>- TEXT: Text |
| content | O | Message |

**Callback**

| GameTalkCallback.ErrorDelegate | Description |
| --- | --- |
| error | Error object |

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
            Debug.Log(string.Format("SendMessage failed. error:{0}", error));
        }
    });
}
```

### GetRecentlyMessage

Retrieves recent messages.

> <font color="red">**[Caution]**</font><br/>
> 
> You can view up to <span>50 messages<span>, including the latest messages.

**API**

Supported Platforms
<span style="color:#b60205">■</span> UNITY_EDITOR
<span style="color:#0e8a16">■</span> UNITY_ANDROID
<span style="color:#1d76db">■</span> UNITY_IOS

```cs
static void GetRecentlyMessage(
    GameTalkParams.Message.GetRecentlyMessage param,
    GameTalkCallback.GameTalkDelegate<GameTalkData.Message.GetRecentlyMessage> callback)
```

**Parameter**

| GameTalkParams.Message.GetRecentlyMessage | Required | Description |
| --- | --- | --- |
| channelId | O | Unique ID given at channel creation |
| count | O | Number of messages to retrieve<br>- A value between 1 and 50 |

**Callback**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Message.GetRecentlyMessage> | Description |
| --- | --- |
| <GameTalkData.Message.GetRecentlyMessage>data | Callback Data |
| error | Error object |

**Callback Data**

| GameTalkData.Message.GetRecentlyMessage | Description |
| --- | --- |
| count | Number of messages viewed |
| recentlyMessageList | **List of Recent Messages**<br>- channelId: Unique ID assigned when a channel is created<br>- channelType: Channel type (Refer to**ChannelType.cs**)<br>  - PUBLIC: Public<br>  - PRIVATE: Private<br>- messageId: Message ID<br>- messageType: message type (see **MessageType.cs**)<br>  - PUBLIC: Public<br>  - PRIVATE: Private<br>  - ADMIN: Administrator<br>  - ANNOUNCEMENT: Announcement message<br>  - SYSTEM: System<br>- contentType: Message data type (Refer to **MessageContentType.cs**)<br>  - TEXT: Text<br>- senderType: Sender type ( Refer to **MessageSenderType.cs** )<br>  - USER: User<br>  - ADMIN: Operator<br>  - ANNOUNCEMENT: Operator<br>  - SYSTEM: System<br>- senderId: Sender ID<br>- senderNickname: Sender nickname<br>- languageCode: Message language code (see **LanguageCode.cs**)<br>- content: Message<br>- state: Message status ( Refer to **MessageState.cs**)<br>  - NORMAL: Normal message<br>  - FILTER: Messages filtered due to profane language<br>- deleted: Whether the message is deleted<br>- regDate: Message sent date |

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
            Debug.Log(string.Format("GetRecentlyMessage succeeded. data:{0}", data));
        }
        else
        {
            Debug.Log(string.Format("GetRecentlyMessage failed. error:{0}", error));
        }
    });
}
```

### GetMessage

Retrieve messages based on a specific message ID.

> <font color="red">**[Caution]**</font><br/>
> 
> You can retrieve <span>up to 51 messages<span> based on a specific message ID.
If the value of include among the parameters is set to true, <span>a total of 51 messages<span> including the standard message will be retrieved.<span><span>
> 
> The sum of prevCount and nextCount must be greater than <span>1 and less than 50<span>.<span><span>

**API**

Supported Platforms
<span style="color:#b60205">■</span> UNITY_EDITOR
<span style="color:#0e8a16">■</span> UNITY_ANDROID
<span style="color:#1d76db">■</span> UNITY_IOS

```cs
static void GetMessage(
    GameTalkParams.Message.GetMessage param,
    GameTalkCallback.GameTalkDelegate<GameTalkData.Message.GetMessage> callback)
```

**Parameter**

| GameTalkParams.Message.GetMessage | Required | Description |
| --- | --- | --- |
| channelId | O | Unique ID given at channel creation |
| messageId | O | Base message id |
| prevCount | O | The number of previous messages to retrieve from the base message<br>- A value between 0 and 50 |
| nextCount | O | The number of next messages to retrieve from the base message<br>- A value between 0 and 50 |
| include | X | Whether to include base message<br>- Default: false |

**Callback**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Message.GetMessage> | Description |
| --- | --- |
| <GameTalkData.Message.GetMessage>data | Callback Data |
| error | Error object |

**Callback Data**

| GameTalkData.Message.GetMessage | Description |
| --- | --- |
| count | Number of messages viewed |
| baseMessage | **Base message (single object)**<br>- channelId: Unique ID assigned when a channel is created<br>- channelType: Channel type (Refer to**ChannelType.cs**)<br>  - PUBLIC: Public<br>  - PRIVATE: Private<br>- messageId: Message ID<br>- messageType: message type (see** MessageType.cs**)<br>  - PUBLIC: Public<br>  - PRIVATE: Private<br>  - ADMIN: Administrator<br>  - ANNOUNCEMENT: Announcement message<br>  - SYSTEM: System<br>- contentType: Message data type (Refer to **MessageContentType.cs**)<br>  - TEXT: Text<br>- senderType: Sender type ( Refer to **MessageSenderType.cs** )<br>  - USER: User<br>  - ADMIN: Operator<br>  - ANNOUNCEMENT: Operator<br>  - SYSTEM: System<br>- senderId: Sender ID<br>- senderNickname: Sender nickname<br>- languageCode: Message language code (see **LanguageCode.cs**)<br>- content: Message<br>- state: Message status ( Refer to **MessageState.cs**)<br>  - NORMAL: Normal message<br>  - FILTER: Messages filtered due to profane language<br>- deleted: Whether the message is deleted<br>- regDate: Message sent date |
| prevMessageList | An object with the same structure as baseMessage is passed as a list |
| nextMessageList | An object with the same structure as baseMessage is passed as a list |

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
            Debug.Log(string.Format("GetMessage succeeded. data:{0}", data));
        }
        else
        {
            Debug.Log(string.Format("GetMessage failed. error:{0}", error));
        }
    });
}
```

### ReportMessage

Report a specific message.

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

**Parameter**

| GameTalkParams.Message.ReportMessage | Required | Description |
| --- | --- | --- |
| messageId | O | Message ID to report |
| messageLanguageCode | O | Message language code to report |
| channelId | O | Unique ID given at channel creation |
| reporterNickname | X | Reporter Nickname |
| reason | X | Reason for Report |

**Callback**

| GameTalkCallback.ErrorDelegate | Description |
| --- | --- |
| error | Error object |

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