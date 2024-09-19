# Intelligent Audio Stream Recognition Product API Documentation

# 1. Preparation Before Access

## 1.1 Shumei Service Account Application

The account manager has already established contact with your company or visited in person. You can directly provide the account opening and service-related information to the account manager.

The information required to open an account includes:

Company Full Name: xxxxxx

Company Abbreviation: xxxxxx

Contact Person Email: xxx@xxx.xxx

Contact Person Phone: 1xxxxxxxxxx

## 1.2 Channel Configuration Table

Shumei configures different channels based on the customer's different business scenarios, formulates targeted interception strategies, and facilitates customers to filter and analyze data for different business scenarios. Please contact Shumei service to open it, and use the value provided separately by Shumei.

## 1.2 Receiving Shumei Service Account Information

The Shumei account manager will open the corresponding Shumei account and service for you within 1 working day. The contact person's email will receive the following information:

| **Name**             | **Specific Value**              | **Description**                                |
| :------------------- | :------------------------------ | :--------------------------------------------- |
| accessKey            | xxxxxx                          | Authentication code for Shumei API service, required when calling Shumei API |
| organization         | xxxxxx                          | Unique enterprise identifier assigned by Shumei, required when calling SDK |
| Shumei Management Console Account | xxxxxx                          | Used to log in to the Shumei management console |
| Shumei Management Console Password | xxxxxx                          | Used to log in to the Shumei management console |
| Shumei Management Console Address | https://console.ishumei.com | Used to log in to the Shumei management console |

# 2. Intelligent Audio Stream Filtering Service Interface Description

Shumei's intelligent audio stream filtering service solution provides audio stream content detection and audio stream closure notification interfaces.

## 2.1 Audio Stream Detection Request

### Interface Description

This interface is used to submit audio stream-related information. The interface will detect whether there is any illegal content in the audio stream in real-time and send the illegal information to the customer's specified URL through a callback.

### Request URL:

| Cluster  | URL                                                          | <span id="language">Supported Languages</span> |
| -------- | ------------------------------------------------------------ | ---------------------------------------------- |
| Singapore | `http://api-audiostream-xjp.fengkongcloud.com/v2/saas/anti_fraud/audiostream` | Chinese, English, Arabic                       |
| Silicon Valley | `http://api-audiostream-gg.fengkongcloud.com/v2/saas/anti_fraud/audiostream` | Chinese, English, Arabic                       |
| Shanghai | `http://api-audiostream-sh.fengkongcloud.com/v2/saas/anti_fraud/audiostream` | Chinese, Arabic                                |

### Character Encoding Format

Both request and return results use UTF-8 character set encoding.

### Request Method

POST

### Suggested Timeout Duration

3s

### General Request Parameters

Placed in the HTTP Body, using JSON format, the specific parameters are as follows:

| **Parameter Name** | **Type**    | **Required** | **Description**                                                     |
| :----------------- | :---------- | :----------- | :------------------------------------------------------------------ |
| accessKey          | string      | Y            | Service key, provided by Shumei when opening the account service    |
| type               | string      | N            | <p>Recognition type, optional values:</p><p>EROTIC: Erotic recognition<br/>DIRTY: Abusive recognition</p><p>ADVERT: Advertisement recognition</p><p>BAN: Prohibited recognition</p><p>VIOLENT: Violent and terrorist recognition</p><p>AUDIOPOLITICAL: Voiceprint recognition of the top leader</p><p>POLITY: Political recognition</p><p>ADLAW: Advertisement law recognition</p><p>MOAN: Moan recognition</p><p>ANTHEN: National anthem recognition</p><p>MINOR: Minor recognition</p><p>BANEDAUDIO: Prohibited songs</p><p>For combined recognition, connect with an underscore, e.g.</p><p>POLITY_EROTIC_MOAN_ADVERT for advertisement, erotic, political, and moan recognition.</p><p>Either type or businessType must be filled</p> |
| businessType       | string      | N            | <p>Recognition type, optional values:</p><p>SING: Singing</p><p>AGE: Age</p><p>LANGUAGE: Language</p><p>GENDER: Gender</p><p>TIMBRE: Timbre</p><p>VOICE: Voice attribute</p><p>AUDIOSCENE: Audio scene</p><p>For timbre, singing, and language recognition, GENDER must be passed</p><p>Either type or businessType must be filled</p> |
| btId               | string      | Y            | Unique identifier for the audio, used to query the specified audio, limited to 128 characters |
| appId              | string      | N            | <p>Application identifier</p><p>Used to distinguish different applications of the same company, needs to contact Shumei to open, please use the value provided separately by Shumei</p> |
| callback           | string      | Y            | Asynchronous detection result callback notification URL, supports HTTP and HTTPS |
| data               | json_object | Y            | Request data content, up to 1MB                                        |

Among them, the content of data is as follows:

| **Parameter Name**  | **Type**    | **Required** | **Description**                                                                                                  |
| :------------------ | :---------- | :----------- | :-------------------------------------------------------------------------------------------------------------- |
| streamType          | string      | Y            | Stream type, optional: <br/>Normal stream: NORMAL, currently supports rtmp, rtmps, hls, http, https protocols, supports flv, m3u8 formats<br/>Agora recording: AGORA<br/>Zego recording: ZEGO<br/>Tencent recording: TRTC<br/>Volcano Engine recording: VOLC<br/>Giant recording: GIN<br/>Alibaba recording: ALI<br/><p>Note: When using the RTC SDK recording solution, additional recording fees will be incurred on the RTC side. Please consult the relevant RTC vendor for specific fees.</p> |
| url                 | string      | Y            | The URL address of the audio stream to be detected (required when streamType is NORMAL)                                                       |
| agoraParam          | json_object | Y            | Agora recording parameters (required when streamType is AGORA), see extended parameters                                                   |
| ginParam            | json_object | Y            | Giant recording parameters (required when streamType is GIN), see extended parameters                                                   |
| zegoParam           | json_object | Y            | Zego recording parameters (required when streamType is ZEGO), see extended parameters                                                    |
| trtcParam           | json_object | Y            | Tencent recording parameters (required when streamType is TRTC), see extended parameters                                                    |
| volcParam           | json_object | Y            | Volcano Engine recording parameters (required when streamType is VOLC), see extended parameters                                                    |
| aliParam            | json_object | Y            | Alibaba recording parameters (required when streamType is ALI), see extended parameters |
| tokenId             | string      | Y            | Unique identifier for the client user account,                                                                                  |
| channel             | string      | Y            | See channel configuration table                                                                                              |
| receiveTokenId      | string      | N            | TokenId of the message receiver in a private chat scenario, a string of up to 64 characters composed of numbers, letters, underscores, and hyphens |
| deviceId            | string      | N            | Shumei device identifier |
| ip                  | string      | N            | Public IP address of the user sending the audio, supports IPV4 or IPV6 |
| level               | int         | N            | Optional values: <br/>`0`: Lowest level user, typically new registered, completely inactive, or level 0 users, etc.;<br/>`1`: Lower level user, typically low active or low level users, etc.;<br/>`2`: Medium level user, typically users with certain activity or medium level, etc.;<br/>`3`: Higher level user, typically high active or high level users, etc.;<br/>`4`: Highest level user, typically paid users, VIP users, etc. |
| gender              | string      | N            | User gender, optional values: <br/>male <br/>female <br/>ambiguity |
| lang                | string      | N            | Optional values are as follows (default is zh):<br/>zh: Chinese<br/>en: English<br/>ar: Arabic<br/>hi: Hindi<br/>es: Spanish<br/>fr: French<br/>ru: Russian<br/>pt: Portuguese<br/>id: Indonesian<br/>de: German<br/>ja: Japanese<br/>tr: Turkish<br/>vi: Vietnamese<br/>it: Italian<br/>th: Thai<br/>tl: Filipino<br/>ko: Korean<br/>ms: Malay |
| extra               | json_object | N            | Auxiliary parameters [see extra parameters](#extra) |

### Extended Request Parameters

Placed under data, the specific parameters are as follows:

| **Parameter Name**     | **Type** | **Required** | **Description**                                                     |
| :--------------------- | :------- | :----------- | :------------------------------------------------------------------ |
| room                   | string   | N            | Room number, strongly recommended to pass in                                         |
| returnAllText          | bool     | N            | <p>If the value is true, it returns the full audio stream segment recognition result and text content;</p><p>If the value is false, it returns the audio stream segment recognition result and text content with riskLevel other than pass, the default is false</p><p>It is recommended to pass in true (the default is false, no callback will be generated in the case of silence)</p> |
| returnPreText          | bool     | N            | <p>If the value is true, the content field contains the text content of the 10-second segment before the illegal audio;</p><p>If the value is false, the content field only contains the text content of the illegal audio segment, the default value is false (this function is invalid for TRTC streams, when the customer uses the interval review function, even if returnPreAudio is true, this field will not be returned)</p><p></p> |
| returnPreAudio         | bool     | N            | <p>If the value is true, it returns the 10-second link of the previous segment of the illegal audio; if the value is false, it only returns the link of the illegal segment audio. The default value is false (this function is invalid for TRTC streams),</p><p>When the customer uses the interval review function, even if returnPreText is true, it only returns the text of the current segment, not the text of the previous segment.</p> |
| returnFinishInfo       | bool     | N            | <p>Audio stream end callback notification</p><p>Optional values (default is false):<br/>false: No end notification is sent when the review ends</p><p>true: End notification is sent when the review ends, and the callback parameters increase the statCode status code</p><p>It is recommended to pass in true (the default is false, no callback will be generated when the stream ends)</p> |
| initDomain             | int      | N            | When the Zego client init initialization supports isolated domain names and random userId, this field must be passed, optional values:<br/>`0`: Default version<br/>`1`: Only supports client initialization with isolated domain names<br/>`2`: Supports client initialization with isolated domain names and random userId functions<br/>`3`: Update SDK, fix some bugs<br/>`4`: Supports custom SEI information<br/>`5`: Supports vad silence detection, token will have uniqueness check, each upload for review must regenerate<br/>`6`: In room dimension pull stream review mode, developers can control whether a certain stream needs to be reviewed<br/>**Recommended to use `6` for access;** For compatibility with old customers, the default value is 0 |
| audioDetectStep        | int      | N            | Each step of the audio will only be detected once, the value range is an integer from 1 to 36, the default is to review each segment (note)<br/>Example: If this parameter is set to 1, the first segment, the third segment, and the fifth segment will be reviewed, and so on. If this parameter is set to 2, the first segment, the fourth segment, and the seventh segment will be reviewed, and so on. |

The content of data.agoraParam is as follows:

| **Parameter Name**    | **Type** | **Required** | **Description**                                                     |
| :-------------------- | :------- | :----------- | :------------------------------------------------------------------ |
| appId                 | string   | Y            | AppId provided by Agora, note the distinction from Shumei's appId                     |
| channel               | string   | Y            | Channel name provided by Agora, note the distinction from Shumei's channel.                  |
| token                 | string   | N            | Users with higher security requirements can use a token for authentication, the generation method is detailed in the Agora documentation: <https://docs.agora.io/cn/Interactive%20Broadcast/token_server?platform=All%20Platforms><br/>It is recommended to set the token validity period to exceed the duration of the channel to prevent the token from expiring and causing the stream to be unable to be pulled. The maximum token validity period currently supported by Agora is 24 hours, so when the channel duration exceeds 24 hours, the token expiration issue needs to be handled. Solution: Set the audio stream end callback notification in the request parameters (set returnFinishInfo to true). When the callback receives the review end notification (statCode is 1), and the reason is that the token for pulling the stream is invalid or expired (errorCode status code under auxInfo returns 3005), if the channel still exists and needs to continue the review, generate a new token and resubmit the channel for review. |
| uid                   | int      | N            | User ID, 32-bit unsigned integer. When the token exists, the user ID used to generate the token must be provided. Note that the uid provided for the recording server is not allowed to exist in the room. |
| isMixingEnabled       | bool     | N            | <p>Single stream/mixed stream recording, default is mixed stream recording.</p><p>true: Mixed stream</p><p>false: Single stream</p><p>Mixed stream means one stream for one live room, single stream means one stream for one mic position</p> |
| channelProfile        | int      | N            | <p>Channel mode for Agora recording, values:</p><p>0: Communication (default), i.e., common 1-to-1 single chat or group chat, any user in the channel can speak freely;</p><p>1: Live broadcast, with two user roles: host and audience.</p><p>Default is communication mode recording, i.e., the default value is 0.</p> |
| subscribeMode         | string   | N            | <p>Subscription mode:</p><p>`AUTO`: Automatically subscribe to all streams in the room, the default behavior when subscribeMode is not set</p><p>`UNTRUSTED`: Only subscribe to the user streams specified in the `untrustedUserIdList`, only effective in Agora single stream</p><p>`TRUSTED`: Only subscribe to the user streams outside the `trustedUserIdList`, only effective in Agora single stream</p> |
| trustedUserIdList     | string_array | N            | <p>List of trusted users, effective when subscribeMode=TRUSTED, can be empty, Shumei will not subscribe to the user streams specified in this list in the room. Each element uid should be within the range of uint32, but the type is string. For example: ["123","456"]</p> |
| untrustedUserIdList   | string_array | N            | <p>List of untrusted users, effective when subscribeMode=UNTRUSTED, cannot be empty, Shumei will only subscribe to the user streams specified in this list in the room. Each element uid should be within the range of uint32, but the type is string. For example: ["123","456"]</p> |

The content of data.ginParam is as follows:

| **Parameter Name**    | **Type** | **Required** | **Description**                                                                                                                                                                                                   |
| :-------------------- | :------- | :----------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| tokenId               | string   | Y            | Room token, used for the pull stream end to log in to the room, needs to be provided by Giant                                                                                                                                                                   |
| roomId                | string   | Y            | Room number, uniquely corresponds to a room, the server pulls the stream and records it by room                                                                                                                                                                |
| isMixingEnabled       | bool     | Y            | <p>Recording mode, possible values:</p><p>`false`: Single stream, each user in the room is recorded and reviewed separately</p><p>`true`: Mixed stream, all users in the room are combined into one stream for recording and review</p>                                                                             |
| ip                    | string   | Y            | Designated server IP      |
| port                  | string   | Y            | Designated port                                                                     |

The content of data.zegoParam is as follows:

| **Parameter Name**    | **Type** | **Required** | **Description**                                                     |
| :-------------------- | :------- | :----------- | :------------------------------------------------------------------ |
| tokenId               | string   | Y            | Identity verification information provided by Zego, obtain the identify_token of Zego for login, the generation method is detailed in the Zego documentation: [https://doc-zh.zego.im/article/15258](https://doc-zh.zego.im/article/15258). Note that tokenId uniquely identifies the review request, and a new one needs to be generated for each request. |
| streamId              | string   | N            | Audio stream ID, uniquely corresponds to one audio stream, either streamId or roomId must be passed in. |
| roomId                | string   | N            | Room ID, uniquely corresponds to one room.                                 |
| isMixingEnabled       | bool     | N            | Recording mode, possible values (default is true):<br/>true: Mixed stream, all users in the room are combined into one stream for recording and review. In this case, if streamId and roomId exist separately, they will take effect separately; but when streamId and roomId exist at the same time, streamId will take precedence. <br/>false: Single stream, each user in the room is recorded and reviewed separately. In this case, roomId is required, and roomId will take precedence. At the same time, streamId cannot be passed in. |

The content of data.trtcParam