## Game > GameTalk > Console User Guide

This document explains how to manage channels, members and set languages, forbidden words, and authentication information by using the GameTalk console.

GameTalk console consists of the following menus. 

- **Channel**: Manage channels that allow users to chat, send and retrieve announcements in GameTalk
- **Member**: Manage GameTalk users
- **Settings**: Limitation on the number of characters, language settings, forbidden word management,authentication management

## Channel
Channel is a connection window that enables chats among users in GameTalk. 
Channels can be created in the console and all created channels have their own channel ID. 
Users can subscribe to channels with an unique channel ID. Users can freely subscribe to and unsubscribe from multiple Channels.

## Channel List
You can retrieve/create/delete channels.

### Retrieve Channel List
![gametalk_channel_1_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_channel_1_221213.png)


#### (1) Search Conditions

Select Search Conditions

- **Channel Name**: Search by channel name entered when creating the Channel. Search for the Channel whose entered string contains the channel name.
- **Channel ID**: Search by the unique ID given during Channel creation. Search for the Channel whose entered string is in the Channel ID.

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
![gametalk_channel_2_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_channel_2_221213.png)

#### (1) Channel Name
Enter the channel name to use. 
It can be duplicated and only used in Korean, English and special characters (-_:). 
Spaces are not allowed. You can enter up to 20 characters.


#### (2) Channel Tag
You can add registered Channel Tag. 
Tags added to Channels can be used to search for Channels.

Maximum 10 Channel Tags can be registered and the same Tag cannot be duplicated.

#### (3) Register Tag
You can register a tag. 

![gametalk_channel_3_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_channel_3_221213.png)

### Retrieve Channel Information
![gametalk_channel_4_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_channel_4_221213.png)

#### (1) Channel ID
Channel’s own ID automatically granted when the Channel is created.

#### (2) Channel Name
Channel name you entered when creating the Channel.

#### (3) Channel Tag
Channel Tag added when creating the Channel.

#### (4) Number of Channel users
The number of users who are currently subscribing to the Channel. 


#### (5) Last Message Delivered Date
Time at which the last message was delivered to the current Channel.

#### (6) Modified By / Modified Date
Email of the user who modified the Channel information, the date and time of modification.

#### (7) Created By / Created Date
Email of the user who created the Channel, date and time of creation.


#### (8) Modify
![gametalk_channel_5_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_channel_5_221213.png)

Proceed to the screen to modify channel information. 
You can modify Channel name and Channel Tag.


#### (9) Delete
You can delete Channel. 
Once Channel is deleted, it cannot be reverted.



## Channel Tag
You can retrieve/create/modify/delete Channel Tags. 
Use Channel Tags to assist in Retrieve Channel and delivery of announcement messages.

For each Channel, users can register their own Channel Tags. 
Channel Tags are not provided by default and users have to  create and set the desired tags.

You can set maximum 10 Channel Tags per Channel and cannot set duplicate Channel Tags. Single tag can be used on multiple channels.

### Retrieve Tag List

![gametalk_tag_1_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_tag_1_221213.png)

You can retrieve registered Tag.

#### (1) Search Tag
You can search tags by Tag name. 
Search for Tags that contain the values you enter in Tag name.


### Register Tag
![gametalk_tag_2_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_tag_2_221213.png)

You can register a tag. 

#### (1) Channel Tag name

Enter Tag name to use.

Channel Tag names can only contain alphabet uppercase/ lowercase and number and permissible characters (-_:). You can enter a maximum of 20 characters.

Tag names cannot be duplicated.

#### (2) Tag Description

Enter description of the Tag to use. 

No characters are unable to be used and can enter a maximum of 100 characters.

#### (3) [+] Button
You can register additional tags by clicking on Button.

### Retrieve Tag Information

![gametalk_tag_3_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_tag_3_221213.png)

#### (1) Channel Tag name
Channel Tag name entered when registering Channel Tag.

#### (2) Tag Description
Tag Description entered when registering Channel Tag.

#### (3) Modified By / Modified Date
Email of the user who modified the Channel Tag, the date and time of modification.

#### (4) Created By / Created Date
Email of the user who created the Channel Tag, the date and time of modification.

#### (5) Modify
![gametalk_tag_4_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_tag_4_221213.png) 
Can modify Channel Tag and Channel descriptions. 

#### (6) Delete

You can delete Channel Tag. 
Once Channel Tag is deleted, it cannot be reverted.


## Announcement
You can send announcement message to desired Channel. 
Announcement messages can be sent in multiple languages and translation is provided. 
You can send announcement message immediately or can choose the time you want to send.


### Retrieve Announcements
![gametalk_notice_1_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_notice_1_221219.png)

#### (1) Send Type

- **Send All**: Search for Send All Type in Send Type
- **Channel Tag**: Search for Channel that sending type of announcement is Channel Tag and that matches the Tag search criteria you set.
- **Channel Name**: searches for Sent Announcements that sending type of announcement is the Channel Name and that contains the Channel value that you entered.


#### (2) Content

- **All**: search Announcements that contain any value entered in Announcement message and Announcement memo.
- **Announcement Message**: search Announcements that contain the value entered in the Announcement message.
- **Announcement Memo**: search Announcements that contain the value entered in the Announcement memo.


#### (3) Send Time
- Search Announcements whose time sent in the Announcements falls within the time range you entered.

#### (4) Announcement Status

- **Waiting**: announcement is in a waiting state because the scheduled time hasn’t come yet.
- **Cancel**: User canceled the scheduled announcement before it was sent.
- **Failed**: Announcement has failed to be sent.
- **Completed**: Announcement was successfully sent.


#### (5) Retrieval Result
Results of the retrieved announcement. Put the mouse cursor over the content to see the whole content.


### Send Announcement
![gametalk_notice_2_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_notice_2_221213.png)

#### (1) Send Type
You can select a channel to be an announcement target type.

- **Send All**: Send to all users to receive announcements.
- **Channel Tag**: Send announcements to users who subscribed to the Channel that meets the Channel Tag criteria.
- **Channel Name**: Send announcements to users who subscribed to the selected Channel.


#### (2) Announcement Content

- Announcement Content to send to users. 
- Click **Translate to Selected Language** button to fill the contents of the remaining languages with the selected language.

#### (3) Send Time

- **Send Immediately**: Send announcements immediately.
- **Scheduled Send**: Send announcements on the desired time. Select time and time zone to send announcements at the selected time of the selected time zone.


#### (4) Preview Announcement.
![gametalk_notice_3_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_notice_3_221213.png)

- Check and send the created announcement.


### Retrieve Announcement in Detail
![gametalk_notice_4_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_notice_4_221213.png) 
You can retrieve detailed announcement by selecting retrieved announcements.

#### Modify Announcement
Announcements that have not yet been sent can be modified for the content, target and time of sending.

#### Delete Announcement
You can delete Announcement. 
Once Announcement is deleted, it cannot be reverted.

---

## Member
It provides managing features of users such as GameTalk user information, retrieval of access history and user deletion.


## View Member

You can check user information, connection history of users who logged in to GameTalk.

![gametalk_member_1_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_member_1_221213.png)

### View Member
Retrieve member’s information who are using the chatting service. 

You can retrieve member information by user ID. 
Member information that is the same as the value entered is to be retrieved.

#### (1) User ID
ID the user logged in. 


#### (2) Language Code
The language code the user used when logged in. 

#### (3) Last Logged In
The date and time that the user logged in last time. 

#### (4) User Created Date
The date and time the user first logged in. 


### Access History

You can retrieve access history of a searched member. 
You can retrieve 3 months of access history. 
You can check the access date and time, SDK version, device information, network type, etc.

### Retrieve Member information in Detail
![gametalk_member_2_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_member_2_221213.png)

You can select retrieved member and retrieve information in detail. 


---

## Settings
Limitation of the number of characters, language of announcement message, forbidden word, credentials management.

## General
![gametalk_common_setting_1_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_common_setting_1_221213.png)
### Message
You can do settings about message

#### Character Limit
You can limit the number of message characters that can be sent. 
You can only enter numbers between 10 and 100.

### Language Settings
You can set the languages supported by GameTalk. 
When setting language, set the language you set when sending announcement message to default value. 

## Manage Forbidden Word

You can manage forbidden words applying to message. 

### Retrieve Forbidden Word List
![gametalk_banned_word_1_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_banned_word_1_221213.png)

### Register Forbidden Word
![gametalk_banned_word_2_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_banned_word_2_221219.png)

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
You cannot use forbidden words including commas.

#### (6) Upload
You can attach an Excel document. If there is any content previously entered, replace it with the contents of the uploaded file.

#### (7) Apply Defaults
You can apply Default Forbidden Word List provided by GameTalk.

#### (8) Save Excel 
You can download the currently registered forbidden word list as an Excel file. 

### Retrieve Forbidden Word List 
![gametalk_banned_word_3_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_banned_word_3_221213.png)

#### (1) Delete
You can delete a forbidden word. 
Once forbidden word is deleted, it cannot be reverted.

#### (2) Modify
![gametalk_banned_word_4_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_banned_word_4_221213.png)

You can modify the registered banned word. 



## Authentication Information

It is login credentials used by GameTalk. 
Make the chat available only to users who are authorized with credentials.

![gametalk_auth_1_221213](https://static.toastoven.net/prod_gametalk/console/gametalk_auth_1_221213.png)

#### (1) APP ID
It is activated GameBase App ID. 

#### (2) Secret Key
It is activated GameBase App Secret Key. 


> **[Note]**<br />
>
> Currently only provides Game Base Credentials. 

