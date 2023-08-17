## Game > GameTalk > 릴리스 노트 > Unity

### 1.1.0(2023. 08. 17.)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gametalk/v1.1.0/GameTalkSDK_Unity.zip)

* 기능 추가
    * 메시지 신고 API가 추가되었습니다.
    * 기준이 되는 메시지의 ID로 다수의 메시지를 조회하는 API가 추가되었습니다.
    * 최근 메시지를 가져오는 API가 추가되었습니다. 
    * 신규 이벤트 타입이 추가되었습니다.
        * pushToAllUsers: 전체 발송 알림 메시지가 수신되면 호출됩니다.
        * pushDeleteUser: GameTalk을 이용 중인 사용자의 채널 구독 정보를 콘솔 및 server API를 통하여 해제할 경우 호출됩니다.
* 변경
    * 내부 로직 개선
        
### 1.0.0(2022. 12. 27.)

* 신규 상품 출시
    * 손쉽게 게임 내 채팅 기능을 구현할 수 있는 서비스입니다. GameTalk 서비스를 이용해 실시간 채팅, 길드 채팅 등 다양한 채팅 환경을 게임 내에 구현할 수 있습니다.