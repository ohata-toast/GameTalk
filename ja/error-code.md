## Game > GameTalk > Error Code

## Client SDK

| Platform           | Error                                    | Error Code | Description                              |
| ------------------ | ---------------------------------------- | ---------- | ---------------------------------------- |
| Android, iOS | NOT_INITIALIZED | 1 | GameTalk SDK is not initialized. |
| Android, iOS | NOT_LOGGED_IN | 2 | You are not logged in. |
| Android, iOS | ALREADY_INITIALIZED | 3 | GameTalk SDK is already initialized.  |
| Android, iOS | ALREADY_LOGGED_IN | 4 | You are already logged in. |
| Android, iOS | INVALID_PARAMETER | 5 | Invalid parameters. |
| Android, iOS | RESPONSE_TIMEOUT | 11 | No response due to unstable network status. |
| Android, iOS | NOT_SUPPORTED | 91 | This feature is not supported. |
| Android, iOS | NOT_SUPPORTED_ANDROID | 92 | This feature is not supported in Android. |
| Android, iOS | NOT_SUPPORTED_IOS | 93 | This feature is not supported in iOS. |
| Android, iOS | SOCKET_CONNECTION_FAILED | 101 | Socket failed to connect |
| Android, iOS | SOCKET_CONNECTION_TIMEOUT | 102 | A timeout occurred while connecting the socket. |
| Android, iOS | SERVER_INTERNAL_ERROR | 201 | Server error occurred. |
| Android, iOS | INVALID_SERVER_RESPONSE | 202 | Invalid response sent from the server. |
| Android, iOS | API_ID_IS_EMPTY | 203 | API ID missing from the server response. |
| Android, iOS | HEADER_IS_NULL | 204 | Header missing from the server response. |
| Android, iOS | TRANSACTION_ID_IS_EMPTY | 205 | Transaction ID missing from the server response. |
| Android, iOS | MESSAGE_LIMIT_EXCEEDED | 301 | The length of the message exceeds the limit.  |
| Android, iOS | UNKNOWN_ERROR | 999 | Unknown error occurred. |

## Server
| Module  | Error Code            | Description                              |
| ------- | --------------------- | ---------------------------------------- |
| Common  | -4000001<br/>-4000006 | API is called with an invalid parameter type. <br/>- Example) The parameter is declared as an `int` type, but the API is called as `string` type data. |
|         | -4000002<br/>-4000004<br>-4000005 | A required parameter is omitted or has no value. Or it is called with an invalid value. |
|         | -4000003              | A value undefined in the Request body is sent. |
|         | -4000007              | An API version that is no longer supported is called. |
|         | -4000008              | The parameter length exceeded. |
|         | -4010002              | An invalid AppKey is called. |
|         | -4010003              | An unauthenticated client called an API that requires authentication. |
|         | -4010004              | Called an invalid Secret key (secret key). |
|         | -4060001              | An invalid Content-Type is set in the HTTP header. |
|         | -4060002              | Called an API version not in use. |
|         | -4060003              | Called with an invalid API version or URI. |
|         | -4090001 ~ 4          | Error related to internal DB. |
|         | -4150001              | JSON data in an invalid format is sent. |
|         | -5000001 ~ 15         | An internal system error. |
| Front   | -4150102              | The request timed out. |
|         | -5000101<br/>-5000102<br/>-5040102 | An internal system error. |
| Back    | -4000201              | Unsupported IdP. |
|         | -4000202<br/>-4000203 | Unsupported API. |
|         | -4000204              | Unsupported LanguageCode. |
|         | -4010201              | Service basic information lookup error.  |
|         | -4010202              | An error related to authentication.  |
|         | -4040201              | Unregistered AppKey. |
|         | -4040202              | Unregistered channel. |
|         | -4040203              | Unregistered project. |
|         | -4040204              | Unregistered authentication information. |
|         | -4040205              | Deleted channel. |
|         | -4040206              | Unregistered message.|
|         | -4040207              | Deleted message.|
|         | -4040208              | Unregistered report message.|
|         | -4040209              | Unregistered user.|
|         | -4040210              | This channel is not subscribed to.|
|         | -4040211              | Unregistered tag.|
|         | -4120201              | Input limit exceeded.|
|         | -4120202              | Unreportable message.|
|         | -4120203              | Invalid channel name.|
|         | -4120204              | No message content.|
|         | -4120205              | Already subscribed channel.|
|         | -4120206              | Invalid tag name.|
|         | -4120207              | Message has been blocked.|
|         | -4120208              | An error related to authentication.|
|         | -4120209              | Already reported message.|
|         | -4120210              | Message sending is restricted to disabling the service.|
|         | -4120211              | Input value error.|
|         | -4290201              | Channel creation limit exceeded. |
|         | -4290202              | Tag creation limit exceeded. |
|         | -4290203              | Channel subscription limit exceeded. |
|         | -4290204              | Channel tag list exceeded. |
|         | -5000201 ~ 6<br/>-5040201<br/>-5040202<br/>-5100201 | This is an internal system error. If the error persists, please contact the Customer Center. |
