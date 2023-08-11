## Game > GameTalk > Console User Guide

This document explains how to manage channels, members, and messages, and set languages, forbidden words, message sending restriction and authentication information by using the GameTalk console.

GameTalk console consists of the following menus.

- **Channel**: Manage channels that allow users to chat, channel tags, and announcement messages in GameTalk
- **Member**: Manage GameTalk users
- **Message**: View GameTalk messages and manage report messages
- **Settings**: Limitation on the number of characters, language settings, message sending restriction, forbidden word management,authentication management

## Channel
Channel is a connection window that enables chats among users in GameTalk.
Channels can be created in the console and all created channels have their own channel ID.
Users can subscribe to channels with an unique channel ID. Users can freely subscribe to and unsubscribe from multiple Channels.

## Channel List
You can retrieve/create/delete channels.

### Retrieve Channel List
![gametalk_channel_1_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_channel_1_230629.png)

#### (1) Search Conditions

Select Search Conditions

- **Channel Name**: Search by channel name entered when creating the Channel. Search for the Channel whose entered string contains the channel name.
- **Channel ID**: Search by the unique ID given during Channel creation. Search for the Channel whose entered string is matched with the Channel ID.
- **Channel Nickname**: Search by channel nickname entered in the Channel. Searches for channels whose channel nickname contains the entered string.

#### (2) Channel Tag
You can search by Channel Tag

- **AND**: Search for Channels that contain all selected Channel Tags.
- **OR**: Search for Channels that contain any of the selected Channel Tags.


#### (3) Number of Channel Users
Search for Channels that contain the number of subscribers to the Channel in the range entered.

#### (4) User ID
Search for the Channel that the user ID you entered is subscribed to.

#### (5) Channel Created Date
Search for Channels whose channel creation date is in the range you entered.

#### (6) Last Message
Search for channel whose delivery time of the last message passed to the Channel is within the range you entered.


### Channel Creation
![gametalk_channel_2_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_channel_2_230629.png)

#### (1) Channel Name
Enter the channel name to use.
Channel names cannot be duplicated, and all characters including emojis and special characters can be entered.
You can enter up to 50 characters.

#### (2) Channel Nickname
You can enter a channel nickname to use.
Channel nicknames can be duplicated, and all characters including emojis and special characters can be entered.
You can enter up to 50 characters.

#### (3) Channel Tag
You can add registered Channel Tag.
Tags added to Channels can be used to search for Channels.

Maximum 10 Channel Tags can be registered and the same Tag cannot be duplicated.

#### (4) Register Tag
You can register a tag.

![gametalk_channel_3_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_channel_3_230629.png)

#### (5) Auto Delete Channel
When the number of subscribers to a channel reaches 0, the channel is automatically deleted.
When the user unsubscribes from the channel, checks whether the channel has been deleted.

### Retrieve Channel Information
![gametalk_channel_4_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_channel_4_230629.png)

#### (1) Channel Name
Channel name you entered when creating the Channel.

#### (1) Channel ID
Channel’s own ID automatically granted when the Channel is created.

#### (3) Channel Nickname
Channel nicknames entered in Channel.

#### (4) Channel Tag
Channel Tag added when creating the Channel.

#### (5) Channel type
The type of channel. Currently only PUBLIC is supported.

#### (6) Auto Delete Channel
Whether to select the automatic deletion feature of a channel that is deleted when the number of subscribers to the channel reaches 0.

#### (7) Last Message Delivered Date
Time at which the last message was delivered to the current Channel.

#### (8) Modified By / Modified Date
Email of the user who modified the Channel information, the date and time of modification.

#### (9) Created By / Created Date
Email of the user who created the Channel, date and time of creation.

#### (10) Channel Member
The number of users who are currently subscribing to the Channel.

![gametalk_channel_5_230731](https://static.toastoven.net/prod_gametalk/console/gametalk_channel_5_230731.png)

You can search the channel's subscriber information.

#### (11) Chat
Live chat messages from the current channel are displayed.

#### (12) Administrator Message
You can send admin messages to the current channel.

#### (13) Modify

![gametalk_channel_6_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_channel_6_230629.png)

Click **Modify** to switch to a screen where you can modify channel information.
You can modify the **channel name, channel nickname, channel tag, and whether to automatically delete the channel**.

#### (14) Delete
Click **Delete** to delete the channel.
Once a channel is deleted, it is reverted.



## Channel Tag
You can retrieve/create/modify/delete Channel Tags.
Use Channel Tags to assist in Retrieve Channel and delivery of announcement messages.

For each Channel, users can register their own Channel Tags.
Channel Tags are not provided by default and users have to create and set the desired tags.

You can set maximum 10 Channel Tags per Channel and cannot set duplicate Channel Tags. Single tag can be used on multiple channels.

### Retrieve Tag List

![gametalk_tag_1_230731](https://static.toastoven.net/prod_gametalk/console/gametalk_tag_1_230731.png)

You can retrieve registered Tag.

#### (1) Search Tag
You can search tags by Tag name.
Search for Tags that contain the values you enter in Tag name.


### Register Tag
![gametalk_tag_2_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_tag_2_230629.png)

You can register a tag.

#### (1) Channel Tag name

Enter Tag name to use.

Channel Tag names can only contain alphanumeric and some allowed characters (_, -, :, .), and you can enter a maximum of 20 characters.

Tag names cannot be duplicated.

#### (2) Tag Description

Enter description of the Tag to use.

No characters are unable to be used and can enter a maximum of 100 characters.

#### (3) [+] Button
You can register additional tags by clicking on Button.

### Retrieve Tag Information

![gametalk_tag_3_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_tag_3_230629.png)

#### (1) Channel Tag name
Channel Tag name entered when registering Channel Tag.

#### (2) Tag Description
Tag Description entered when registering Channel Tag.

#### (3) Modified By / Modified Date
Email of the user who modified the Channel Tag, the date and time of modification.

#### (4) Created By / Created Date
Email of the user who created the Channel Tag, the date and time of modification.

#### (5) Modify
![gametalk_tag_4_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_tag_4_230629.png)
Can modify Channel Tag and Channel descriptions.

#### (6) Delete

You can delete Channel Tag.
Once Channel Tag is deleted, it cannot be reverted.


## Announcement Message 
You can send announcement message to desired Channel.
Announcement messages can be sent in multiple languages and translation is provided.
You can send announcement message immediately or can choose the time you want to send.


### Retrieve Announcement Messages

![gametalk_announcement_1_230731](https://static.toastoven.net/prod_gametalk/console/gametalk_announcement_1_230731.png)

#### (1) Send Type

- **Send All**: Search for announcement messages sent to all.
- **Channel Tag**: Search for Channel that sending type of announcement is Channel Tag and that matches the Tag search criteria you set.
- **Channel Name**: searches for Sent Announcements that sending type of announcement is the Channel Name and that contains the Channel value that you entered.


#### (2) Content

- **All**: search Announcements that contain any value entered in Announcement message and Announcement memo.
- **Announcement Message**: search Announcements that contain the value entered in the Announcement message.
- **Announcement Memo**: search Announcements that contain the value entered in the Announcement memo.


#### (3) Registered Date
- Search for registered announcement messages within the selected time period.

#### (4) Announcement Message Status

- **Ready**: message in a ready state fore the scheduled send time.
- **In Progress**: Sending announcement messages in progress.
- **Canceled**: User canceled the scheduled announcement message before it was sent.
- **Failed**: Announcement message failed to be sent.
- **Completed**: Announcement message successfully sent.


#### (5) Retrieval Result
Results of the retrieved announcement. Put the mouse cursor over the content to see the whole content.


### Send Announcement Message
![gametalk_announcement_2_230731](https://static.toastoven.net/prod_gametalk/console/gametalk_announcement_2_230731.png)

#### (1) Send Type
You can select the type of announcement message sent to the channel.

- **Send Immediately**: Send announcements immediately.
- **Scheduled Send**: Send announcements on the desired time. Select time and time zone to send announcements at the selected time of the selected time zone.
- **Repetitive Send**: Send announcement messages at the desired time by selecting the time and time zone, and by selecting the number of sendings and the sending interval.

![gametalk_announcement_3_230731](https://static.toastoven.net/prod_gametalk/console/gametalk_announcement_3_230731.png)

#### (2) Send To
You can select a channel to be an announcement target type.

- **Send All**: Send to all users to receive announcements.
- **Channel Tag**: Send announcements to users who subscribed to the Channel that meets the Channel Tag criteria.
- **Channel Name**: Send announcements to users who subscribed to the selected Channel.

#### (3) Announcement Message Content

- Announcement Content to send to users.
- Click **Translate to Selected Language** button to fill the contents of the remaining languages with the selected language.

#### (4) Announcement Message Preview
![gametalk_announcement_4_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_announcement_4_230629.png)

- Check and send the created announcement message.

### Retrieve Announcement Message in Detail

![gametalk_announcement_5_230731](https://static.toastoven.net/prod_gametalk/console/gametalk_announcement_5_230731.png)

You can view detailed information by selecting the searched announcement message list.
For announcement messages sent repeatedly, you can query the delivery status of each announcement message.

![gametalk_announcement_6_230731](https://static.toastoven.net/prod_gametalk/console/gametalk_announcement_6_230731.png)

#### Cancel Sending Announcement Messages
Announcement messages that have not been sent in Ready and In progress can be canceled.
Canceled announcement messages cannot be resent.

#### Copy Announcement Messages
You can create a new announcement message with the same input values as the registered announcement message.

---

## Member
Provides features to manage users, such as viewing information and access history of users who are using the chat service, and deleting users.


## View Member

You can check the information of users who are using the chat service, their access history, and the list of channels they are subscribed to.
User information can be searched by user ID and user nickname. Information of the same user as the entered value is searched.

![gametalk_member_1_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_member_1_230629.png)

### View Member
Retrieve member’s information who are using the chatting service.

#### (1) Search Conditions

Select Search Conditions

- **User ID**: Search by user ID. Searches for users whose input string matches the user ID.
- **Nickname**: Search by user's nickname. Searches for users whose nickname matches the input string.


### Retrieve Member Details

You can check the information of users who use the chat service, their access history, and the channels they are subscribed to.

![gametalk_member_2_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_member_2_230629.png)

#### (1) User ID
This is the ID of the user using the chat service.

#### (2) Nickname
This is the nickname of the user using the chat service.

#### (3) Language Code
This is the language code registered by the user using the chat service.

#### (4) Last Access Date
The date and time the user last used the chat service.

#### (4) User Created Date
The date and time the user first used the chat service.

#### (6) Access History

You can retrieve access history of a searched user.
You can retrieve 1 month of access history.
You can check the access date and time, nickname, language code, SDK version, device information, network type, etc.

#### (7) Subscribed Channels

You can search the channels subscribed to by the searched user.

#### (8) Delete Member

You can delete users. The user's access history and subscribed channels will be deleted.

---

## Message
Retrieves messages from the chat service.

### Retrieve Message List
![gametalk_message_1_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_message_1_230629.png)

#### (1) Search Conditions
Select Search Conditions

- **User ID**: Search by user ID. Searches for messages sent by users whose input string matches the user ID.
- **Nickname**: Search by user's nickname. Searches for messages sent by users whose nickname matches the input string.
- **Channel ID**: Search by the unique ID given during Channel creation. Search for messages of the Channel whose entered string is matched with the Channel ID.
- **Channel Name**: Search by channel name entered when creating the Channel. Search for messages of the Channel whose entered string contains the channel name.

#### (2) Message Creation Date
Search for messages with a message creation date that falls within the entered range.

#### (3) Include Messages of Deleted Channel
Search for messages, including messages from deleted channels.

## Report Message 
View message information reported by users using the chat service and delete report history.

### Retrieve Report Messages
![gametalk_message_2_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_message_2_230629.png)

#### (1) Search Conditions
Select Search Conditions

- **User ID**: Search by user ID. Searches for messages written by users whose input string matches the user ID.
- **Nickname**: Search by user's nickname. Searches for messages written by users whose nickname matches the input string.
- **Channel ID**: Search by the unique ID given during Channel creation. Search for report messages of the Channel whose entered string is matched with the Channel ID.
- **Channel Name**: Search by channel name entered when creating the Channel. Search for report messages of the Channel whose entered string contains the channel name.

#### (2) Message Creation Date
Search for report messages with a report message creation date that falls within the entered range.

#### (3) Include Messages of Deleted Channel
Search for report messages, including report messages from deleted channels.

### Retrieve Report Message Details
![gametalk_message_3_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_message_3_230629.png)

#### (1) Channel name (channel ID)
The channel name and channel ID of the report message.

#### (2) Channel type
The channel type of the report message.

#### (3) Deleted channel
Whether to delete the channel in the report message.

#### (4) Reported user ID (nickname)
User ID and nickname that sent the reported message.

#### (5) Reported message ID
Reported message ID

#### (6) Cumulative reports
The cumulative number of reports for reported messages.

#### (7) Report message
This is the content of the reported message. In the case of messages processed with forbidden words, if the original message remains, the original message can be searched.

#### (8) Message report history
You can view the information of the reported user.

#### (9) Delete report history
You can delete the report history.  

---

## Settings
Limitation of the number of characters, language of announcement message, message sending restriction, forbidden word, credentials management.

## General
![gametalk_common_setting_1_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_common_setting_1_230629.png)
### Message
You can do settings about message

#### Character Limit
You can limit the number of message characters that can be sent.
You can only enter numbers between 10 and 100.

#### Language Settings
You can set the languages supported by the chat service.
If you set the language, the language you set will be set as the default when sending notification messages.
The default language and supported language are set to Korean.

#### Restrict Message Sending
You can set message transmission limit before disabling GameTalk service(If transmission restrictions are not set, GameTalk cannot be disabled.).
Data is immediately deleted upon disabling the service, and deleted data cannot be recovered.
If message transmission is restricted, message transmission within the channel is not possible, **channel creation, tag creation, and new announcement message registration** are not possible.

## Manage Forbidden Word

You can manage forbidden words applying to message.

### Retrieve Forbidden Word List

![gametalk_banned_word_1_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_banned_word_1_230629.png)

### Register Forbidden Word
![gametalk_banned_word_2_230731](https://static.toastoven.net/prod_gametalk/console/gametalk_banned_word_2_230731.png)

#### (1) Name
Name of Forbidden Word

#### (2) Application Status
Whether the forbidden word is applied or not.

#### (3) Include Rule

- **Single (Exact)**: Only words that exactly match the words entered are to be banned.
- **Collective (Like)**: If it contains words entered based on spacing, it will be banned.

#### (4) Application Method

- **Replace with \***: Replace forbidden words with \*.
- **Block Message**: Block messages if they contain forbidden words.

#### (5) Forbidden Words
It is a forbidden word to apply.
It is separated with commas (,) and can be registered up to 5,000 byte and 1,000 units.
You cannot use forbidden words including commas. Forbidden words cannot be duplicated.

![gametalk_banned_word_3_230731](https://static.toastoven.net/prod_gametalk/console/gametalk_banned_word_3_230731.png)

#### (6) Upload
You can attach an Excel document. If there is any content previously entered, replace it with the contents of the uploaded file.

#### (7) Apply Defaults
You can apply Default Forbidden Word List provided by GameTalk.

#### (8) Save Excel
You can download the currently registered forbidden word list as an Excel file.

### Retrieve Forbidden Word List

![gametalk_banned_word_4_230731](https://static.toastoven.net/prod_gametalk/console/gametalk_banned_word_4_230731.png)

#### (1) Delete
You can delete a forbidden word.
Once forbidden word is deleted, it cannot be reverted.

#### (2) Modify

![gametalk_banned_word_5_230731](https://static.toastoven.net/prod_gametalk/console/gametalk_banned_word_5_230731.png)

You can modify the registered banned word.



## Authentication Information

You can view, create, modify, or delete login authentication information used in GameTalk.
If login authentication information is registered, only allowed users can use the chat through Gamebase authentication.

![gametalk_auth_1_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_auth_1_230629.png)

#### (1) APP ID
It is activated GameBase App ID.

#### (2) Secret Key
It is activated GameBase App Secret Key.

#### (3) Modify Authentication Information
You can edit authentication information App ID and Secret Key.

![gametalk_auth_2_230629](https://static.toastoven.net/prod_gametalk/console/gametalk_auth_2_230629.png)

#### (4) Delete Authentication Information
You can delete authentication information.

> **[Note]**<br />
>
> Currently only provides Game Base Credentials.

