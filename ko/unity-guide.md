## Game > GameTalk > Unity SDK 사용 가이드

GameTalk SDK for Unity 환경 및 사용 방법을 설명합니다.

## Environments

### Unity

* 2018.4.0 이상
    * .NET 4.x 이상
* 하위 버전의 Unity 지원이 필요하면 [고객 센터](https://www.toast.com/kr/support/inquiry)로 문의하십시오.

> <font color="red">**[주의]**</font><br/>
>  
> 2019년 8월 1일부터 Google Play에 게시되는 신규 앱에서는 64비트 아키텍처를 지원해야 합니다.
> [Google Play 정책 및 64비트 지원 Unity 버전 확인](https://developer.android.com/games/optimize/64-bit#unity-developers)

### Android

* Android 4.4 (API 19) 이상

### iOS

* iOS 11 이상

### Supported Platforms

* UnityEditor
    * 일부 기능만 지원합니다.
* Android
* iOS

선택한 플랫폼에서 지원하지 않는 GameTalk API를 호출하면 아래와 같은 오류가 콜백으로 반환되며 콜백이 없는 경우에는 Warning 로그가 출력됩니다.

* GameTalkErrorCode.NOT_SUPPORTED_ANDROID
* GameTalkErrorCode.NOT_SUPPORTED_IOS

API별 지원하는 플랫폼은 아래와 같은 아이콘으로 구분합니다.

#### API

<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

## API

### SetDebugMode

* GameTalk은 경고와 오류 로그만을 표시합니다.
* 개발에 참고할 수 있는 GameTalk 로그를 켜려면 GameTalk.SetDebugMode(true)를 호출하십시오.

> <font color="red">**[주의]**</font><br/>
>  
> * GameTalk 문의 지원이 필요한 경우에는 반드시 디버그 모드를 활성화한 후, 로그를 함께 전달해야 합니다.
> * 게임을 **릴리스**할 때는 반드시 소스 코드에서 SetDebugMode 호출을 제거하거나 파라미터를 false로 바꿔서 빌드하십시오.

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void SetDebugMode(bool isDebugMode)
```

**매개변수**

| Parameter | 필수 | 설명 |
| --- | --- | --- |
| isDebugMode | O | 디버그 모드 활성화 여부 |

**Example**

```cs
public void SetDebugModeExample()
{
    GameTalk.SetDebugMode(true);
}
```

### IsSucceeded

GameTalkError를 사용하여 API 성공 여부를 확인합니다.

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static bool IsSucceeded(GameTalkError error)
```

**매개변수**

| Parameter | 필수 | 설명 |
| --- | --- | --- |
| error | O | GameTalkError 객체 |

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

GameTalk SDK를 초기화합니다.

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

**매개변수**

| GameTalkParams.Config | 필수 | 설명 |
| --- | --- | --- |
| appKey | O | 콘솔에서 GameTalk 프로젝트 활성화 시 자동 생성되는 앱키(Appkey) |
| languageCode | O | 콘솔에 등록된 다국어 번역 대상 코드 중 기준이 되는 언어 코드 |

**콜백**

| GameTalkCallback.GameTalkDelegate<GameTalkData.ServiceInfo> | 설명 |
| --- | --- |
| <GameTalkData.ServiceInfo> data | 콜백 데이터 |
| error | 오류 객체 |

**콜백 데이터**

| GameTalkData.ServiceInfo | 설명 |
| --- | --- |
| maxMessageLength | 콘솔에 등록된 최대 메시지 길이 |
| gameTalkState | GameTalk 상태(**GameTalkState.cs** 참조)<br>- ACTIVATED: 활성화<br>- DEACTIVATED: 비활성화<br>- DELETED: 삭제 |

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

            // 초기화가 성공하면 AddEvent API를 이용하여 이벤트를 수신할 핸들러를 등록해야 합니다.
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

GameTalk SDK의 초기화 여부를 확인합니다.

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

서버 이벤트를 수신할 핸들러를 등록합니다.

> <font color="red">**[주의]**</font><br/>
> 
> MappingUserInfo API를 <span>호출하기 전<span>에 호출되어야 합니다.
> 이후에 호출되면, 이벤트로 수신되는 메시지가 누락될 수 있습니다.

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void AddEvent(GameTalkCallback.GameTalkDelegate<GameTalkData.AddEvent> eventHandler)
```

**매개변수**

| GameTalkCallback.GameTalkDelegate<GameTalkData.AddEvent> | 설명 |
| --- | --- |
| <GameTalkData.AddEvent>data | 이벤트 데이터 |
| error | 오류 객체 |

**이벤트 데이터**

| GameTalkData.AddEvent | 설명 |
| --- | --- |
| type | 이벤트 타입(**GameTalkEventType.cs** 참조)<br>- CHANGE_NETWORK_STATE: 네트워크가 중단되거나, 재연결되었을 때 이벤트를 수신<br>- PUSH_MESSAGE: 구독 중인 오픈 채널에 새로운 메시지가 수신되면 호출<br>  - EventDataParser의 GetPushMessageData API를 사용하여 data를 객체화하여 사용<br>- PUSH_TO_ALL_USERS: 전체 발송 알림 메시지가 수신되면 호출<br>  - EventDataParser의 GetPushToAllUsersData API를 사용하여 data를 객체화하여 사용<br>- PUSH_DELETE_USER: GameTalk을 이용 중인 사용자의 채널 구독 정보를 콘솔 및 server API를 통하여 해제할 경우 호출<br>  - 이벤트가 수신되면 게임 로직 처리 후 `GameTalk.DeleteUserInfo` API를 호출하여 서버와 상태 값을 동기화해야 합니다.<br>  - 사용자는 모든 채널의 구독 상태가 해지되므로 채널을 다시 구독하기 전까지는 모든 메시지 송수신이 불가능합니다.<br>  - EventDataParser의 GetPushDeleteUserData API를 사용하여 data를 객체화하여 사용 |
| data | 이벤트 타입에 따라 달라지는 데이터(아래 세부 데이터 참조) |
| data(CHANGE_NETWORK_STATE) | 네트워크 상태(**NetworkState.cs** 참조)<br>- DISCONNECTED: 네트워크 연결 해제<br>- RECONNECTED: 네트워크 재연결<br>  - 네트워크 문제로 수신하지 못한 메시지를 조회해야 합니다(Example 참조). |
| data(PUSH_MESSAGE) | - channelId: 채널 생성 시 부여된 고유 ID<br>- channelType: 채널 타입(**ChannelType.cs** 참조)<br>  - PUBLIC: 공개<br>  - PRIVATE: 비공개<br>- messageId: 메시지 아이디<br>- messageType: 메시지 타입(**MessageType.cs** 참조)<br>  - PUBLIC: 공개<br>  - PRIVATE: 비공개<br>  - ADMIN: 운영자<br>  - ANNOUNCEMENT: 알림 메시지<br>  - SYSTEM: 시스템<br>- contentType: 메시지 데이터 타입(**MessageContentType.cs** 참조)<br>  - TEXT: 텍스트<br>- senderType: 송신자 타입(**MessageSenderType.cs** 참조)<br>  - USER: 사용자<br>  - ADMIN: 운영자<br>  - ANNOUNCEMENT: 운영자<br>  - SYSTEM: 시스템<br>- senderId: 송신자 아이디<br>- senderNickname: 송신자 닉네임<br>- languageCode: 메시지 언어 코드(**LanguageCode.cs** 참조)<br>- content: 메시지<br>- state: 메시지 상태(**MessageState.cs** 참조)<br>  - NORMAL: 정상 메시지<br>  - FILTER: 비속어로 인해 필터링 된 메시지<br>- deleted: 메시지 삭제 여부<br>- regDate: 메시지 전송 일시 |
| data(PUSH_TO_ALL_USERS) | PUSH_MESSAGE 이벤트 데이터에서 channelId, channelType만 없고, 모두 동일 |
| data(PUSH_DELETE_USER) | - userId: 사용자 ID |

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
                case GameTalkEventType.CHANGE_NETWORK_STATE:
                {
                    // 네트워크 상태가 변경되면 호출됩니다.
                    // NetworkState.DISCONNECTED or NetworkState.RECONNECTED
                    Debug.Log(string.Format("Change networkState:{0}", eventData.data));

                    if (eventData.data.Equals(NetworkState.RECONNECTED) is true)
                    {
                        // 네트워크 문제로 수신하지 못한 메시지를 조회합니다.
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
                    // 전체 발송 알림 메시지가 수신되면 호출
                    // Use EventDataParser's GetEventData<GameTalkData.Message.PushToAllUsers> API to objectify and use data
                    Debug.Log(string.Format(
                        "An event has been received. data:{0}",
                        EventDataParser.GetEventData<GameTalkData.Message.PushToAllUsers>(eventData.data)));
                    break;
                }
                case GameTalkEventType.PUSH_DELETE_USER:
                {
                    // GameTalk을 이용 중인 사용자의 채널 구독 정보를 콘솔 및 server API를 통하여 해제할 경우 호출
                    // Use EventDataParser's GetEventData<GameTalkData.PushDeleteUser> API to objectify and use data
                    Debug.Log(string.Format(
                        "PUSH_DELETE_USER event was received. data:{0}",
                        EventDataParser.GetEventData<GameTalkData.PushDeleteUser>(eventData.data)));

                    // 서버와 클라이언트 상태를 동기화하려면 DeleteUserInfo API를 호출해야 합니다.
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
        // 마지막으로 수신한 메시지의 ID입니다.
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
                // 수신하지 못한 메시지가 없습니다.

                //------------------------------
                //  게임 처리 로직
                //------------------------------
                // PUSH_MESSAGE 이벤트로 수신된 메시지를 UI에 표시합니다.
            }
            else
            {
                // 수신하지 못한 메시지가 있습니다.
                // PUSH_MESSAGE 이벤트로 수신된 메시지를 UI에 표시할 경우에는 메시지의 순서가 맞지 않는 문제가 발생할 수 있으므로,
                // 해당 처리가 끝날 때까지 수신된 메시지를 보관합니다.

                //------------------------------
                //  게임 처리 로직
                //------------------------------
                // 1. PUSH_MESSAGE 이벤트로 수신된 메시지를 보관합니다(UI 표시 X).
                // 2. 게임 UI에 GetMessage API로 조회된 메시지들을 표시합니다.
                // 3. 수신하지 못한 메시지가 없을 때까지 CheckUnreceivedMessages 메서드를 재귀 호출합니다.
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

사용자 인증 정보를 GameTalk으로 매핑합니다.
매핑된 정보는 GameTalk 사용자 식별자로 사용됩니다.

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

**매개변수**

| GameTalkParams.MappingUserInfo | 필수 | 설명 |
| --- | --- | --- |
| idPType | X | IdP (identity provider) 타입(**IdPType.cs** 참조)<br>- GAMEBASE: Gamebase<br>Gamebase를 사용 중이라면 IdPType.GAMEBASE를 입력 |
| userId | O | 사용자 ID<br>Gamebase를 사용 중이라면 Gamebase User ID를 입력 |
| token | X | 사용자 인증 토큰<br>Gamebase 인증을 사용 중이라면 Gamebase 인증 토큰을 입력(토큰이 생략되면 서버에서 오류가 반환)<br>Gamebase 인증을 사용하지 않는다면 생략 가능 |
| nickname | X | 사용자 별명 |

**콜백**

| GameTalkCallback.GameTalkDelegate<GameTalkData.MappingUserInfo> | 설명 |
| --- | --- |
| <GameTalkData.MappingUserInfo>data | 콜백 데이터 |
| error | 오류 객체 |

**콜백 데이터**

| GameTalkData.MappingUserInfo | 설명 |
| --- | --- |
| user | **사용자 정보**<br>- userId: 사용자 ID<br>- nickname: 사용자 별명<br>- valid: 사용자 상태<br>  - true: 정상<br>  - false: 삭제된 유저<br>- regDate: 사용자 가입 일시<br>- languageCode: 사용자 언어 코드<br>- lastLoginDate: 마지막 로그인 일시 |

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

사용자 인증 정보가 GameTalk으로 매핑되어 있는지 확인합니다.

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

GameTalk으로 매핑된 사용자 인증 정보를 해제합니다.
GameTalk.MappingUserInfo API가 호출되기 전까지 모든 채팅 메시지를 수신할 수 없습니다.

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void UnmappingUserInfo(GameTalkCallback.ErrorDelegate callback)
```

**매개변수**

| GameTalkCallback.ErrorDelegate | 설명 |
| --- | --- |
| error | 오류 객체 |

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

유저 정보를 갱신합니다.

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

**매개변수**

| GameTalkParams.UpdateUserInfo | 필수 | 설명 |
| --- | --- | --- |
| languageCode | O | 콘솔에 등록된 다국어 번역 대상 코드 중 기준이 되는 언어 코드 |
| nickname | X | 사용자 별명 |

**콜백**

| GameTalkCallback.ErrorDelegate | 설명 |
| --- | --- |
| error | 오류 객체 |

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

GameTalk으로 매핑된 사용자의 모든 채널의 구독 정보를 해제합니다.

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void DeleteUserInfo(GameTalkCallback.ErrorDelegate callback)
```

**매개변수**

| GameTalkCallback.ErrorDelegate | 설명 |
| --- | --- |
| error | 오류 객체 |

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

채널 정보를 조회합니다.


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

**매개변수**

| GameTalkParams.Channel.GetChannelList | 필수 | 설명 |
| --- | --- | --- |
| page | X | 페이지 인덱스<br>- 시작 값은 0 |
| size | O | 페이지 사이즈<br>- 1부터 100 사이의 값 |
| tagType | X | 태그 검색 조건(**TagType.cs** 참조)<br>- Default: OR<br>- OR: 선택한 채널 태그를 하나라도 포함한 채널을 검색<br>- AND: 선택한 채널 태그를 모두 포함한 채널을 검색 |
| tagIdList | X | 검색 태그 리스트 |

**콜백**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetChannelList> | 설명 |
| --- | --- |
| <GameTalkData.Channel.GetChannelList>data | 콜백 데이터 |
| error | 오류 객체 |

**콜백 데이터**

| GameTalkData.Channel.GetChannelList | 설명 |
| --- | --- |
| pagingInfo | **페이징 정보**<br>- first: 첫 페이지 여부<br>- last: 마지막 페이지 여부<br>- numberOfElements: 현재 페이지의 채널 수<br>- page: 페이지 인덱스<br>- size: 페이지 사이즈<br>- totalElements: 총 채널 수<br>- totalPages: 총 페이지 수 |
| channelList | **채널 리스트**<br>- id: 채널 아이디<br>- type: 채널 타입(**ChannelType.cs** 참조)<br>  - PUBLIC: 공개<br>  - PRIVATE: 비공개<br>- name: 채널 생성 시 입력한 채널명<br>- nickname: 채널 별칭<br>- subscriberCount: 채널 구독자 수<br>- lastMessageId: 마지막 메시지 아이디<br>- autoDelete: 채널 자동삭제 여부<br>- deleted: 채널 삭제상태(true면 삭제된 채널)<br>- tagList: 태크 리스트<br>    - <span style="color:#222222"></span>id: 태그 아이디<br>    - name: 태그 명 |

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

채널을 구독합니다.

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

**매개변수**

| GameTalkParams.Channel.SubscribeChannel | 필수 | 설명 |
| --- | --- | --- |
| channelId | O | 채널 생성 시 부여된 고유 ID |

**콜백**

| GameTalkCallback.ErrorDelegate | 설명 |
| --- | --- |
| error | 오류 객체 |

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

구독 중인 채널을 해제합니다.

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

**매개변수**

| GameTalkParams.Channel.UnsubscribeChannel | 필수 | 설명 |
| --- | --- | --- |
| channelId | O | 채널 생성 시 부여된 고유 ID |

**콜백**

| GameTalkCallback.ErrorDelegate | 설명 |
| --- | --- |
| error | 오류 객체 |

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

구독 중인 유저를 조회합니다.

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

**매개변수**

| GameTalkParams.Channel.GetSubscriber | 필수 | 설명 |
| --- | --- | --- |
| channelId | O | 채널 생성 시 부여된 고유 ID |
| page | X | 페이지 인덱스<br>- 시작 값은 0 |
| size | O | 페이지 사이즈<br>- 1부터 100 사이의 값 |

**콜백**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetSubscriber> | 설명 |
| --- | --- |
| <GameTalkData.Channel.GetSubscriber>data | 콜백 데이터 |
| error | 오류 객체 |

**콜백 데이터**

| GameTalkData.Channel.GetSubscriber | 설명 |
| --- | --- |
| pagingInfo | **페이징 정보**<br>- first: 첫 페이지 여부<br>- last: 마지막 페이지 여부<br>- numberOfElements: 현재 페이지의 채널 수<br>- page: 페이지 인덱스<br>- size: 페이지 사이즈<br>- totalElements: 총 채널 수<br>- totalPages: 총 페이지 수 |
| userList | **사용자 리스트** <br>- userId: 사용자 ID<br>- nickname: 사용자 별명<br>- valid: 사용자 상태<br>  - true: 정상<br>  - false: 삭제된 유저<br>- regDate: 사용자 가입 일시<br>- languageCode: 사용자 언어 코드<br>- lastLoginDate: 마지막 로그인 일시 |

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

채널의 태그 리스트를 조회합니다.

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

**매개변수**

| GameTalkParams.Channel.GetTagList | 필수 | 설명 |
| --- | --- | --- |
| page | X | 페이지 인덱스<br>- 시작 값은 0 |
| size | O | 페이지 사이즈<br>- 1부터 100 사이의 값 |

**콜백**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetTagList> | 설명 |
| --- | --- |
| <GameTalkData.Channel.GetTagList>data | 콜백 데이터 |
| error | 오류 객체 |

**콜백 데이터**

| GameTalkData.Channel.GetTagList | 설명 |
| --- | --- |
| pagingInfo | **페이징 정보**<br>- first: 첫 페이지 여부<br>- last: 마지막 페이지 여부<br>- numberOfElements: 현재 페이지의 채널 수<br>- page: 페이지 인덱스<br>- size: 페이지 사이즈<br>- totalElements: 총 채널 수<br>- totalPages: 총 페이지 수 |
| tagList | **태그 리스트** <br>- id: 태그 아이디<br>- name: 태그 명 |

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

구독 중인 채널을 조회합니다.

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

**매개변수**

| GameTalkParams.Channel.GetSubscribedChannelList | 필수 | 설명 |
| --- | --- | --- |
| page | X | 페이지 인덱스<br>- 시작 값은 0 |
| size | O | 페이지 사이즈<br>- 1부터 100 사이의 값 |

**콜백**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetSubscribedChannelList> | 설명 |
| --- | --- |
| <GameTalkData.Channel.GetSubscribedChannelList>data | 콜백 데이터 |
| error | 오류 객체 |

**콜백 데이터**

| GameTalkData.Channel.GetSubscribedChannelList | 설명 |
| --- | --- |
| pagingInfo | **페이징 정보**<br>- first: 첫 페이지 여부<br>- last: 마지막 페이지 여부<br>- numberOfElements: 현재 페이지의 채널 수<br>- page: 페이지 인덱스<br>- size: 페이지 사이즈<br>- totalElements: 총 채널 수<br>- totalPages: 총 페이지 수 |
| channelList | **채널 리스트**<br>- id: 채널 아이디<br>- type: 채널 타입(**ChannelType.cs** 참조)<br>  - PUBLIC: 공개<br>  - PRIVATE: 비공개<br>- name: 채널 생성 시 입력한 채널명<br>- nickname: 채널 별칭<br>- subscriberCount: 채널 구독자 수<br>- lastMessageId: 마지막 메시지 아이디<br>- autoDelete: 채널 자동삭제 여부<br>- deleted: 채널 삭제상태(true면 삭제된 채널)<br>- tagList: 태크 리스트<br>    - <span style="color:#222222"></span>id: 태그 아이디<br>    - name: 태그 명 |

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

메시지를 전송합니다.

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

**매개변수**

| GameTalkParams.Message.SendMessage | 필수 | 설명 |
| --- | --- | --- |
| channelId | O | 채널 생성 시 부여된 고유 ID |
| contentType | O | 메시지 데이터 타입(**MessageContentType.cs** 참조)<br>- TEXT: 텍스트 |
| content | O | 메시지 |

**콜백**

| GameTalkCallback.ErrorDelegate | 설명 |
| --- | --- |
| error | 오류 객체 |

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

최신 메시지를 조회합니다.

> <font color="red">**[주의]**</font><br/>
> 
> 최신 메시지를 포함하여, <span>최대 50개</span>의 메시지를 조회할 수 있습니다.

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

**매개변수**

| GameTalkParams.Message.GetRecentlyMessage | 필수 | 설명 |
| --- | --- | --- |
| channelId | O | 채널 생성 시 부여된 고유 ID |
| count | O | 조회할 메시지 개수<br>- 1부터 50 사이의 값 |

**콜백**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Message.GetRecentlyMessage> | 설명 |
| --- | --- |
| <GameTalkData.Message.GetRecentlyMessage>data | 콜백 데이터 |
| error | 오류 객체 |

**콜백 데이터**

| GameTalkData.Message.GetRecentlyMessage | 설명 |
| --- | --- |
| count | 조회된 메시지 갯수 |
| recentlyMessageList | **최근 메시지 리스트**<br>- channelId: 채널 생성 시 부여된 고유 ID<br>- channelType: 채널 타입(**ChannelType.cs** 참조)<br>  - PUBLIC: 공개<br>  - PRIVATE: 비공개<br>- messageId: 메시지 아이디<br>- messageType: 메시지 타입(**MessageType.cs** 참조)<br>  - PUBLIC: 공개<br>  - PRIVATE: 비공개<br>  - ADMIN: 운영자<br>  - ANNOUNCEMENT: 알림 메시지<br>  - SYSTEM: 시스템<br>- contentType: 메시지 데이터 타입(**MessageContentType.cs** 참조)<br>  - TEXT: 텍스트<br>- senderType: 송신자 타입(**MessageSenderType.cs** 참조)<br>  - USER: 사용자<br>  - ADMIN: 운영자<br>  - ANNOUNCEMENT: 운영자<br>  - SYSTEM: 시스템<br>- senderId: 송신자 아이디<br>- senderNickname: 송신자 닉네임<br>- languageCode: 메시지 언어 코드(**LanguageCode.cs** 참조)<br>- content: 메시지<br>- state: 메시지 상태(**MessageState.cs** 참조)<br>  - NORMAL: 정상 메시지<br>  - FILTER: 비속어로 인해 필터링 된 메시지<br>- deleted: 메시지 삭제 여부<br>- regDate: 메시지 전송 일시 |

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

특정 메시지 ID를 기준으로 메시지를 조회합니다.

> <font color="red">**[주의]**</font><br/>
> 
> 특정 메시지 ID를 기준으로 <span>최대 51개</span>의 메시지를 조회할 수 있습니다.
> 매개변수 중 include의 값을 true로 설정할 경우, 기준이 되는 메시지를 포함하여 <span>총 51개<span>의 메시지가 조회됩니다.</span></span>
> 
> prevCount와 nextCount의 합이 <span>1이상 50이하<span>가 되어야 합니다.</span></span>

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

**매개변수**

| GameTalkParams.Message.GetMessage | 필수 | 설명 |
| --- | --- | --- |
| channelId | O | 채널 생성 시 부여된 고유 ID |
| messageId | O | 기준 메시지 아이디 |
| prevCount | O | 기준 메시지로부터 조회할 이전 메시지 개수<br>- 0부터 50 사이의 값 |
| nextCount | O | 기준 메시지로부터 조회할 다음 메시지 개수<br>- 0부터 50 사이의 값 |
| include | X | 기준 메시지 포함 여부<br>- Default: false |

**콜백**

| GameTalkCallback.GameTalkDelegate<GameTalkData.Message.GetMessage> | 설명 |
| --- | --- |
| <GameTalkData.Message.GetMessage>data | 콜백 데이터 |
| error | 오류 객체 |

**콜백 데이터**

| GameTalkData.Message.GetMessage | 설명 |
| --- | --- |
| count | 조회된 메시지 갯수 |
| baseMessage | **기준 메시지(단일 객체)**<br>- channelId: 채널 생성 시 부여된 고유 ID<br>- channelType: 채널 타입(**ChannelType.cs** 참조)<br>  - PUBLIC: 공개<br>  - PRIVATE: 비공개<br>- messageId: 메시지 아이디<br>- messageType: 메시지 타입(**MessageType.cs** 참조)<br>  - PUBLIC: 공개<br>  - PRIVATE: 비공개<br>  - ADMIN: 운영자<br>  - ANNOUNCEMENT: 알림 메시지<br>  - SYSTEM: 시스템<br>- contentType: 메시지 데이터 타입(**MessageContentType.cs** 참조)<br>  - TEXT: 텍스트<br>- senderType: 송신자 타입(**MessageSenderType.cs** 참조)<br>  - USER: 사용자<br>  - ADMIN: 운영자<br>  - ANNOUNCEMENT: 운영자<br>  - SYSTEM: 시스템<br>- senderId: 송신자 아이디<br>- senderNickname: 송신자 닉네임<br>- languageCode: 메시지 언어 코드(**LanguageCode.cs** 참조)<br>- content: 메시지<br>- state: 메시지 상태(**MessageState.cs** 참조)<br>  - NORMAL: 정상 메시지<br>  - FILTER: 비속어로 인해 필터링 된 메시지<br>- deleted: 메시지 삭제 여부<br>- regDate: 메시지 전송 일시 |
| prevMessageList | baseMessage와 동일한 구조의 객체가 리스트로 전달 |
| nextMessageList | baseMessage와 동일한 구조의 객체가 리스트로 전달 |

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

특정 메시지를 신고합니다.

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

**매개변수**

| GameTalkParams.Message.ReportMessage | 필수 | 설명 |
| --- | --- | --- |
| messageId | O | 신고 대상 메시지 아이디 |
| messageLanguageCode | O | 신고 대상 메시지 언어 코드 |
| channelId | O | 채널 생성 시 부여된 고유 ID |
| reporterNickname | X | 신고자 별명 |
| reason | X | 신고 사유 |

**콜백**

| GameTalkCallback.ErrorDelegate | 설명 |
| --- | --- |
| error | 오류 객체 |

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