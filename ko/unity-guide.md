## Game > GameTalk > Unity SDK 사용 가이드

GameTalk SDK for Unity 환경 및 사용 방법을 설명합니다.

## Environments

### Unity

* 2018.4.0 이상
    * .NET 4.x 이상
* 하위 버전의 Unity 지원이 필요하면 [고객 센터](https://www.toast.com/kr/support/inquiry)로 문의해 주시기 바랍니다.

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

* GameTalkErrorCode.NOT_SUPPORTED_IOS
* GameTalkErrorCode.NOT_SUPPORTED_ANDROID

API 별 지원하는 플랫폼은 아래와 같은 아이콘으로 구분합니다.

#### API

<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

## API

### SetDebugMode

* GameTalk는 경고와 오류 로그만을 표시합니다.
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

**Parameter**

* isDebugMode 디버그 모드 활성화 여부

**Example**

```cs
public void SetDebugModeSample(bool isDebugMode)
{
    Gamebase.SetDebugMode(isDebugMode);
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

**Parameter**

* GameTalkError error
    * GameTalkError 객체

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

**Parameter**

* GameTalkParams.Config config
    * appKey: 콘솔에서 GameTalk 프로젝트 활성화 시, 자동 생성되는 앱 키(Appkey)
    * languageCode: 콘솔에 등록된 다국어 번역 대상 코드 중, 기준이 되는 언어 코드
* GameTalkCallback.GameTalkDelegate<GameTalkData.ServiceInfo> callback
    * GameTalkData.ServiceInfo
        * maxMessageLength: 콘솔에 등록된 최대 메시지 길이
        * gameTalkState: GameTalk 상태 (**GameTalkState.cs** 참조)
            * ACTIVATED: 활성화
            * DEACTIVATED: 비활성화
            * DELETED: 삭제

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

서버 이벤트를 수신할 핸들러를 등록합니다.

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
        * type: 이벤트 타입 (**EventType.cs** 참조)
            * PUSH_MESSAGE
                * 구독 중인 오픈 채널에 새로운 메시지가 수신되면 호출
                * EventDataParser의 GetPushMessageData API를 사용하여 data를 객체화하여 사용
        * data: 이벤트 타입에 따라 달라지는 데이터
            * PUSH_MESSAGE                
* 이벤트 타입 별 데이터
    * data(PUSH_MESSAGE)
        * messageInfoList
            * messageInfo
                * messageId: 메시지 아이디
                * channelId: 채널 생성 시 부여된 고유 ID
                * senderType: 송신자 타입 (**MessageSenderType.cs** 참조)
                    * USER: 일반 사용자
                    * ADMIN: 관리자
                    * SYSTEM: 시스템
                * senderId: 송신자 아이디
                * senderNickname: 송신자 닉네임 (없을 경우 senderId로 자동 설정)
                * regDate: 메시지 전송 일시
                * contentType: 메시지 데이터 타입 (**MessageContentType.cs** 참조)
                    * TEXT: 텍스트
                * messageList: 송신자가 입력한 메시지와 LanguageCode를 기준으로 번역된 메시지가 함께 전달됩니다. (LanguageCode는 Initialize, UpdateUserInfo API에서 변경 가능)
                    * message
                        * content: 메시지
                        * state: 메시지 상태 (**MessageState.cs** 참조)
                            * NORMAL: 정상 메시지
                            * FILTER: 비속어로 인해 필터링된 메시지

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

등록된 핸들러를 제거합니다.

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

GameTalk 로그인을 실행합니다.

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
    * idPType: IdP(identity provider) 타입 (**IdPType.cs** 참조)
        * Gamebase를 사용 중이라면 IdPType.GAMEBASE를 입력
    * userId: 사용자 ID
        * Gamebase를 사용 중이라면 Gamebase User ID를 입력
    * token: 사용자 인증 토큰
        * Gamebase를 사용 중이라면 Gamebase 인증 토큰을 입력
* GameTalkCallback.GameTalkDelegate<GameTalkData.Auth.Login> callback
    * GameTalkData.Auth.Login
        * user
            * userId: 사용자 ID
            * valid: 사용자 상태 (**UserState.cs** 참조)
                * Y: 정상
                * D: 삭제된 유저
            * regDate: 사용자 가입 일시
            * lastLoginDate: 마지막 로그인 일시

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

GameTalk 로그아웃을 실행합니다.

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

유저 정보를 갱신합니다.

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
    * languageCode: 콘솔에 등록된 다국어 번역 대상 코드 중, 기준이 되는 언어 코드
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

유저 정보를 삭제합니다.

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

**Parameter**

* GameTalkParams.Channel.GetChannelList param
    * page: 페이지 인덱스 (시작 값은 0)
    * size: 페이지 사이즈
    * tagType: 태그 검색 조건 (**TagType.cs** 참조)
        * 기본 값은 OR
        * OR: 선택한 채널 태그를 하나라도 포함한 채널을 검색
        * AND: 선택한 채널 태그를 모두 포함한 채널을 검색
    * tagList: 검색 태그 리스트
* GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetChannelList> callback
    * GameTalkData.Channel.GetChannelList
        * pagingInfo
            * first: 첫 페이지 여부
            * last: 마지막 페이지 여부
            * numberOfElements: 현재 페이지의 채널 수
            * page: 페이지 인덱스 (시작 값은 0)
            * size: 페이지 사이즈
            * totalElements: 총 채널 수
            * totalPages: 총 페이지 수
        * channelList
            * channelInfo
                * id: 채널 아이디
                * type: 채널 타입 (**ChannelType.cs** 참조)
                    * public: 오픈 채널
                    * private: 시스템 채널, 1:1 채널
                * name: 채널 생성 시 입력한 채널명
                * subscriberCount: 채널 구독자 수
                * tagList
                    * tag
                        * id: 태그 아이디
                        * name: 태그 명

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

**Parameter**

* GameTalkParams.Channel.SubscribeChannel param
    * channelId: 채널 생성 시 부여된 고유 ID
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

**Parameter**

* GameTalkParams.Channel.UnsubscribeChannel param
    * channelId: 채널 생성 시 부여된 고유 ID
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

**Parameter**

* GameTalkParams.Channel.GetSubscriber param
    * channelId: 채널 생성 시 부여된 고유 ID
    * page: 페이지 인덱스 (시작 값은 0)
    * size: 페이지 사이즈
* GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetSubscriber> callback
    * GameTalkData.Channel.GetSubscriber
        * pagingInfo
            * first: 첫 페이지 여부
            * last: 마지막 페이지 여부
            * numberOfElements: 현재 페이지의 채널 수
            * page: 페이지 인덱스 (시작 값은 0)
            * size: 페이지 사이즈
            * totalElements: 총 채널 수
            * totalPages: 총 페이지 수
        * userList
            * user
                * id: 유저 아이디

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

채널의 태그 리스트를 조회합니다.

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
    * page: 페이지 인덱스 (시작 값은 0)
    * size: 페이지 사이즈
* GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetChannelTagList> callback
    * GameTalkData.Channel.GetChannelTagList
        * pagingInfo
            * first: 첫 페이지 여부
            * last: 마지막 페이지 여부
            * numberOfElements: 현재 페이지의 채널 수
            * page: 페이지 인덱스 (시작 값은 0)
            * size: 페이지 사이즈
            * totalElements: 총 채널 수
            * totalPages: 총 페이지 수
        * tagList
            * tag
                * id: 태그 아이디
                * name: 태그 명

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

**Parameter**

* GameTalkParams.Channel.GetSubscribedChannelList param
    * page: 페이지 인덱스 (시작 값은 0)
    * size: 페이지 사이즈
* GameTalkCallback.GameTalkDelegate<GameTalkData.Channel.GetSubscribedChannelList> callback)
    * GameTalkData.Channel.GetSubscribedChannelList
        * pagingInfo
            * first: 첫 페이지 여부
            * last: 마지막 페이지 여부
            * numberOfElements: 현재 페이지의 채널 수
            * page: 페이지 인덱스 (시작 값은 0)
            * size: 페이지 사이즈
            * totalElements: 총 채널 수
            * totalPages: 총 페이지 수
        * channelList
            * channelInfo
                * id: 채널 아이디
                * type: 채널 타입 (**ChannelType.cs** 참조)
                    * public: 오픈 채널
                    * private: 시스템 채널, 1:1 채널
                * name: 채널 생성 시 입력한 채널명
                * subscriberCount: 채널 구독자 수
                * tagList
                    * tag
                        * id: 태그 아이디
                        * name: 태그 명

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

**Parameter**

* GameTalkParams.Message.SendMessage param
    * senderNickname: 송신자 닉네임 (없을 경우 senderId로 자동 설정)
    * channelId: 채널 생성 시 부여된 고유 ID
    * contentType: 메시지 데이터 타입 (**MessageContentType.cs** 참조)
        * TEXT: 텍스트
    * content: 메시지
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