# Game > GameTalk > Unity SDK 사용 가이드

GameTalk SDK for Unity 환경 및 사용 방법을 설명합니다.

## Environments

### Unity

* 2018.4.0 이상
    * .NET 4.x 이상
* 하위 버전의 Unity 지원이 필요하면 [고객 센터](https://www.toast.com/kr/support/inquiry)로 문의해 주시기 바랍니다.

> <font color="red">**[주의]**</font><br/>
>  
> 2019년 8월 1일부터 Google Play에 게시되는 신규 앱에서는 64비트 아키텍처를 지원해야 합니다.<br/>
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

API별 지원하는 플랫폼은 아래와 같은 아이콘으로 구분합니다.

**API**

<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

## API

### Debug Mode

* GameTalk는 경고와 오류 로그만을 표시합니다.
* 개발에 참고할 수 있는 시스템 로그를 켜려면 GameTalk.SetDebugMode(true)를 호출하십시오.

> <font color="red">**[주의]**</font><br/>
>  
>  게임을 **릴리스**할 때는 반드시 소스 코드에서 SetDebugMode 호출을 제거하거나 파라미터를 false로 바꿔서 빌드하십시오.

**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void SetDebugMode(bool isDebugMode)
```

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

GameTalk SDK를 초기화 합니다.

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

* GameTalkParams.Config
    * appKey
        * 콜솔에서 GameTalk 프로젝트 활성화 시 자동 생성되는 앱 키(Appkey)입니다.
    * languageCode
        * 콘솔에 등록된 다국어 변역 대상 코드 중, 기준이 되는 언어코드입니다.
* GameTalkCallback.GameTalkDelegate<GameTalkData.ServiceInfo> callback
    * GameTalkData.ServiceInfo
        * maxMessageLength
            * 콘솔에 등록된 최대 메시지 길이입니다.
        * gameTalkState
            * GameTalk 상태입니다.
            * **GameTalkState**를 참고하십시오.
                * ACTIVATED
                    * 활성화되었습니다.
                * DEACTIVATED
                    * 비활성화되었습니다.
                * DELETED
                    * 삭제되었습니다.

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
        * type
            * **EventType**을 참고하십시오.
            * PUSH_MESSAGE
                * 구독 중인 오픈 채널에 새로운 메시지가 수신되면 호출됩니다.
                * EventDataParser의 GetPushMessageData API를 사용하여 data를 객체화하여 사용해야합니다.
        * data
            * 이벤트 타입에 따라 달라지는 데이터입니다.

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

* GameTalkParams.Auth.Login
    * idPType
        * **EventType**을 참고하십시오.
        * ID 제공자입니다.
            * Gamebase를 사용중이라면 IdPType.GAMEBASE를 입력하십시오.
    * userId
        * 사용자 ID입니다.
            * Gamebase를 사용중이라면 Gamebase User ID를 입력하십시오.
    * token
        * 사용자 인증 토큰입니다.
            * Gamebase를 사용중이라면 Gamebase 인증 토큰을 입력하십시오.
* GameTalkCallback.GameTalkDelegate<GameTalkData.Auth.Login> callback
    * GameTalkData.Auth.Login
        * user
            * userId
                * 사용자 ID입니다.
            * valid
                * **UserState**을 참고하십시오.
                * 사용자 상태입니다.
                    * Y
                        * 정상입니다.
                    * D
                        * 삭제된 유저입니다.
            * regDate
                * 사용자 가입 일시입니다.
            * lastLoginDate
                * 마지막 로그인 일시입니다.

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

* GameTalkParams.Auth.UpdateUserInfo
    * languageCode
        * 콘솔에 등록된 다국어 변역 대상 코드 중, 기준이 되는 언어코드입니다.
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








**API**

Supported Platforms
<span style="color:#B60205; font-size: 10pt">■</span> UNITY_EDITOR
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void RemoveEvent()
```

**Parameter**



**Example**

```cs

```