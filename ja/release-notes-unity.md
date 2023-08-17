## Game > GameTalk > リリースノート > Unity

### 1.1.0(2023. 08. 17.)

[SDK Download](https://static.toastoven.net/toastcloud/sdk_download/gametalk/GameTalkSDK_Unity.zip)

* 機能追加
    * メッセージ通報APIが追加されました。
    * 基準となるメッセージのIDで複数のメッセージを照会するAPIが追加されました。
    * 最近のメッセージを取得するAPIが追加されました。 
    * 新規イベントタイプが追加されました。
        * pushToAllUsers；全体送信通知メッセージを受信すると呼び出されます。
        * pushDeleteUser：GameTalkを利用しているユーザーのチャンネル購読情報をコンソールとserver APIを通じて解除した場合に呼び出されます。
* 変更
    * 内部ロジック改善
        
### 1.0.0(2022. 12. 27.)

* 新規商品リリース
    * 簡単にゲーム内チャット機能を実装できるサービスです。GameTalkサービスを利用して、リアルタイムチャット、ギルドチャットなど様々なチャット環境をゲーム内に実装できます。
