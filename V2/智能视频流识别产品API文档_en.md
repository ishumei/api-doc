# Shumei Intelligent Video Stream Recognition Product API Documentation

- - - - -

***All rights reserved. Unauthorized reproduction is prohibited***

- - - - -

## Video Stream Upload Request

### Interface Description

This interface is used to submit video stream identification and related information. After stable streaming, the corresponding recognition results will be continuously called back to the specified callback address.

### Request URL:
| Cluster | URL | Supported Product List |
| --- | --- | --- |
| Shanghai | `http://api-videostream-sh.fengkongcloud.com/v3/saas/anti_fraud/videostream` | Chinese Video Stream |
| Singapore | `http://api-videostream-xjp.fengkongcloud.com/v3/saas/anti_fraud/videostream` | Chinese Video Stream |
| Silicon Valley | `http://api-videostream-gg.fengkongcloud.com/v3/saas/anti_fraud/videostream` | Chinese Video Stream |

### Request Method:

`POST`

### Supported Protocols

`HTTP` or `HTTPS`

### Character Encoding:

`UTF-8`

### Suggested Timeout:

3s

### Request Parameters:

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| accessKey | string | Company key | Required parameter | Assigned by Shumei |
| appId | string | Application identifier | Required parameter | The value of this parameter can be negotiated with Shumei |
| imgType | string | The regulatory type of the image in the video that needs to be identified, **at least one of imgType or imgBusinessType must be passed** | Optional parameter | Primary regulatory label<br/>Optional values:<br/>`POLITICS`: Political recognition<br/>`PERSON`: Political figure recognition<br/>`VIOLENCE`: Terrorism recognition<br/>`PORN`: Pornography recognition<br/>`AD`: Advertisement recognition<br/>`OCR`: Text risk recognition in images<br/>`PORTRAIT`: Posture recognition<br/>`BUSINESSRISK`: Industry violation<br/>If multiple functions need to be identified, connect them with underscores, such as `AD_PORN_POLITICS` for advertisement, pornography, and political combination recognition |
| audioType | string | The regulatory type of the audio in the video stream that needs to be identified, **at least one of audioType or audioBusinessType must be passed** | Optional parameter | Primary regulatory label<br/>Optional values:<br/>`POLITICAL`: Political recognition<br/>`PORN`: Pornography recognition<br/>`AD`: Advertisement recognition<br/>`MOAN`: Moan recognition<br/>`SING`: Singing recognition<br/>`ANTHEN`: National anthem recognition<br/>`ABUSE`: Abuse recognition<br/>`LANGUAGE`: Language recognition<br/>`AUDIOPOLITICAL`: Political sound recognition<br/>`NONE`: Do not detect audio<br/>For combination recognition, connect them with underscores, such as `POLITICAL_PORN_MOAN` for political, pornography, and moan recognition |
| imgBusinessType | string | The business type of the image in the video that needs to be identified, **at least one of imgType or imgBusinessType must be passed** | Optional parameter | Optional values refer to [imgBusinessType Optional Values List](#imgbusinesstype可选值列表)<br/> |
| audioBusinessType | string | The business type of the audio in the video stream that needs to be identified, **at least one of audioType or audioBusinessType must be passed** | Optional parameter | Primary business label<br/>Optional values:<br/>`SING`: Singing recognition<br/>`LANGUAGE`: Language recognition<br/>`MINOR`: Minor recognition<br/>`GENDER`: Gender recognition<br/>`TIMBRE`: Timbre recognition, must be passed with `GENDER` to be effective<br/>`APPNAME`: App name recognition |
| imgCallback | string | Image callback address | Required parameter | The detection results of the frame images in the video stream will be called back to the user through this address |
| audioCallback | string | Audio callback address | Optional parameter | The detection results of the audio segments in the video stream will be called back to the user through this address; required when audio needs to be identified |
| data | json_object | Request data content | Required parameter | Maximum 1MB, see [data content below](#data) |

<span id="data">Among them, the content of data is as follows:</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| lang | string | Language | Required parameter | Optional values are as follows:<br/>`zh`: Chinese<br/>`en`: English<br/>`ar`: Arabic<br/>Default value: `zh` |
| tokenId | string | Unique identifier of the client user account | Required parameter | Used for user behavior analysis, it is recommended to pass in the user UID; maximum 40 characters |
| streamType | string | Video stream type | Required parameter | Optional values are:<br/>`NORMAL`: Normal stream address, currently supports `rtmp`, `rtmps`, `hls`, `http`, `https` protocols, supports `flv`, `m3u8` formats<br/>`AGORA`: Agora review<br/>`TRTC`: Tencent review<br/>`ZEGO`: Zego review<br/>`VOLC`: Volcano Engine review<br/>`ALI`: Alibaba Cloud review<br/>Note: When using the RTC SDK recording solution, additional recording fees may be incurred on the RTC side. Please consult the relevant RTC vendor for specific fees |
| agoraParam | json_object | Agora stream parameters | Optional parameter | Parameters of the Agora stream to be detected (required when streamType is `AGORA`), see [agoraParam description](#agoraParam) |
| trtcParam | json_object | Tencent stream parameters | Optional parameter | Parameters of the TRTC stream to be detected (required when streamType is `TRTC`), see [trtcParam description](#trtcParam) |
| zegoParam | json_object | Zego stream parameters | Optional parameter | Parameters of the Zego stream to be detected (required when streamType is `ZEGO`), see [zegoParam description](#zegoParam) |
| volcParam | json_object | Volcano stream parameters | Optional parameter | Parameters of the Volcano stream to be detected (required when streamType is `VOLC`), see [volcParam description](#volcParam) |
| aliParam | json_object | Alibaba stream parameters | Optional parameter | Parameters of the Alibaba stream to be detected (required when streamType is `ALI`), see [aliParam description](#aliParam) |
| url | string | URL of the video to be detected | Optional parameter | URL parameter of the stream to be detected (required when streamType is NORMAL) |
| streamName | string | Video stream name | Optional parameter | Used for display on the backend interface, it is recommended to pass in |
| ip | string | Client IP | Optional parameter | This parameter is used for user behavior analysis at the IP dimension and can also be used to compare with Shumei's IP blacklist |
| audioDetectStep | int | Audio review step length in the video stream | Optional parameter | The unit is pieces, the value range is 1-36 integers, 1 means skipping one 10S audio segment review, 2 means skipping two, and so on. If this function is not used, all audio content will be reviewed |
| returnAllImg | int | Users can choose to return images with different review results according to their needs | Optional parameter | Optional values are as follows (default value is `0`):<br/>`0`: Callback image review information with reject and review results<br/>`1`: Callback image review information with all results |
| returnAllText | bool | Return the risk level of the audio stream segment recognition result | Optional parameter | Optional values are as follows (default value is `false`):<br/>`false`: Return audio segments and text content with a risk level other than pass<br/>`true`: Return audio segments and text content with all risk levels |
| returnPreText | bool | If true, return the text content of the audio segment for the previous 10 seconds and the current 10 seconds, totaling 20 seconds | Optional parameter | Optional values are as follows (default value is `false`):<br/>`true`: The content field contains the text content of the previous 10 seconds of the violation audio<br/>`false`: The content field only contains the text content of the violation audio segment |
| returnPreAudio | bool | If true, return the audio segment link for the previous 10 seconds and the current 10 seconds, totaling 20 seconds | Optional parameter | Optional values are as follows (default value is `false`):<br/>`true`: Return the audio link of the previous 10 seconds of the violation audio<br/>`false`: Only return the audio link of the violation segment |
| returnFinishInfo | bool | If true, return the end notification when the stream ends | Optional parameter | Optional values are as follows (default value is `false`):<br/>`true`: Send an end notification when the review ends<br/>`false`: Do not send an end notification when the review ends, see [End Stream Return Parameters](#审核结束回调参数) for detailed return parameters |
| detectFrequency | int | Frame capture frequency interval | Optional parameter | The unit is seconds, the value range is 1~60s; if not passed, the default is to capture a frame every 3s |
| detectStep | int | Frame capture image detection step length in the video stream | Optional parameter | Each step of the captured image will only be detected once, the value is greater than or equal to 1. |
| channel | string | Channel identifier | Optional parameter | Users can choose different channels according to different business scenarios |
| room | string | Live room/game room number | Optional parameter | Different strategies can be formulated for a single room; |
| liveTitle | string | Live title | Optional parameter | Live title, generally used for human review required fields |
| liveCover | string | Live cover | Optional parameter | Live cover, generally used for human review required fields |
| anchorName | string | Anchor name | Optional parameter | Anchor name, generally used for human review required fields |
| imgBusinessDetectStep | int | Image business label detection step length | Optional parameter | Each step will only detect imgBusinessType once, the value is greater than or equal to 1.<br/>Default value=1, which means all segments are reviewed for business labels. |

<span id="agoraParam">Among them, the content of agoraParam is as follows:</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| appId | string | Application identifier provided by Agora | Required parameter | |
| channel | string | Channel name provided by Agora | Required parameter | |
| channelKey | string | | Optional parameter | Users with high security requirements can use ChannelKey,<br/>[For the generation method, see the Agora documentation ChannelKey generation method](https://docs.agora.io/cn/Interactive%20Broadcast/token_server?platform=All%20Platforms "Agora channelKey description address") |
| channelProfile | int | Channel mode for Agora recording | No | Optional values are as follows (default value is `0`):<br/>`0`: Communication (default), common 1-to-1 single chat or group chat,<br/>any user in the channel can speak freely;<br/>`1`: Live broadcast, with two user roles: host and audience. |
| uid | int | User ID | Optional parameter | 32-bit unsigned integer. When channelKey exists,<br/>the user ID used to generate the channelKey must be provided.<br/>Note that the uid provided to the server for recording is not allowed to exist in the room |
| enableIntraRequest | bool | Whether to enable keyframe request | Optional parameter | This parameter defaults to true, which can improve the audio and video experience in weak networks. If you want the recorded video in single stream mode to specify the playback position, set `enableIntraRequest` to false.<br/>false: Disable keyframe request, all streaming ends in the channel send a keyframe every 2 seconds. After disabling, the recorded video in single stream mode can specify the playback position.<br/>true: (default) The streaming end controls whether to enable keyframe request. After enabling, the recorded video file in single stream mode cannot specify the playback position. |
| enableH265Support | bool | Whether to support recording H.265 video stream | Optional parameter | false: (default) Do not support recording H.265 video stream. Remote users in the channel cannot send H.265 video streams.<br/>true: Support recording H.265 video stream. |
| subscribeMode | string | Subscription mode | Optional parameter | `AUTO`: Automatically subscribe to all streams in the room, the default behavior when subscribeMode is not set<br/>`UNTRUSTED`: Only subscribe to the user streams specified in the untrustedUserIdList, if the untrustedUserIdList is empty in this mode, the parameter is incorrect because no streams can be subscribed<br/>`TRUSTED`: Only subscribe to the user streams outside the trustedUserIdList, if there are no users outside the trustedUserIdList entering the room within a certain time, i.e., the untrustedUserIdList is empty, Shumei will actively end the review. |
| trustedUserIdList | int_array | List of trusted users | Optional parameter | Effective when subscribeMode is `TRUSTED`, cannot be empty, Shumei will not subscribe to the user streams specified in this list in the room<br/>Comma-separated UID array, such as `[1,2]`, with a maximum of 17 users |
| untrustedUserIdList | int_array | List of untrusted users | Optional parameter | Effective when subscribeMode is `UNTRUSTED`, cannot be empty, Shumei only subscribes to the user streams specified in this list in the room<br/>Comma-separated UID array, such as `[1,2]`, with a maximum of 17 users |

Parameter usage instructions: If you need to review different users in the same room, please generate different uids each time you pass in to prevent the phenomenon of mutual kicking when pulling streams for different users in the same room; if you do not have the uid field (in the case of not using a token), you can consider passing in the corresponding subscription mode parameter: subscribeMode, when this field is not passed or passed as AUTO, automatically subscribe to all streams in the room, when this field is passed as TRUSTED, please pass in the corresponding trustedUserIdList, Shumei will not subscribe to the user streams specified in this list in the room, when this field is passed as UNTRUSTED, please pass in the corresponding untrustedUserIdList, Shumei only subscribes to the user streams specified in this list in the room, we will filter specific users for review based on this field subscribeMode with the corresponding list.

<span id="trtcParam">Among them, the content of trtcParam is as follows:</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| sdkAppId | int | Y | Required parameter | sdkAppId provided by Tencent |
| demoSences | int | Y | Required parameter | Recording type optional values:<br/>`2`: Split stream recording<br/>`4`: Mixed stream recording |
| userId | string | Y | Required parameter | UserId assigned to the recording segment, limited to 32 bits, only allows (a-zA-Z), numbers (0-9), underscores, and hyphens |
| userSig | string | Y | Required parameter | Verification signature corresponding to the recording userId, equivalent to a login password |
| roomId | int | Y | Optional parameter | Room number, value range: [1-4294967294] roomId and strRoomId must pass one, if both have values, roomId is preferred, note: currently a room can only review up to 8 users |
| strRoomId | string | Y | Required parameter | Room number value description: only allows (a-zA-Z), numbers (0-9), underscores, and hyphens, if you choose strRoomId, note that if both strRoomId and roomId have values, roomId is preferred |

<span id="zegoParam">Among them, the content of data.zegoParam is as follows:</span>

| Request Parameter Name | Type | Parameter Description | Transmission Description | Specification |
| --- | --- | --- | --- | --- |
| tokenId | string | Zego authentication token | Required parameter | Identity_token authentication information provided by zego,<br/>used for token login (a new token must be actively called from the zego interface each time a stream is opened)<br/>For the acquisition method, see [Audio and Video Stream Review Authentication Token](https://doc-zh.zego.im/article/15258) |
| roomId | string | Zego room number | Required parameter | Room number to be reviewed |

<span id="volcParam">Among them, the content of data.volcParam is as follows:</span>

| Request Parameter Name | Type   | Parameter Description                                 | Transmission Description | Specification |
| ---------- | ------ | ---------------------------------------- | -------- | ---- |
| appId      | string | Application identifier provided by Volcano                       | Required parameter |      |
| roomId     | string | Room number                                   | Required parameter |      |
| userId     | string | UserId assigned to the recording end                     | Required parameter |      |
| token      | string | Verification signature corresponding to the recording userId, equivalent to a login password | Required parameter |      |

<span id="aliParam">Among them, the content of data.aliParam is as follows:</span>

| Request Parameter Name | Type   | Parameter Description                                                     | Transmission Description | Specification |
| ---------- | ------ | ------------------------------------------------------------ | -------- | ---- |
| room       | string | Room ID, which needs to be completely consistent with the channelID used to generate the token. The server pulls the stream for recording by room. The room is a unique identifier, and the same room will not be pulled repeatedly. | Required parameter |      |
| userId     | string | Pull stream robot ID, which needs to be completely consistent with the userId used to generate the token.              | Required parameter |      |
| token      | string | Used for the pull stream end to join the channel, for the generation method, see the document: https://help.aliyun.com/zh/live/user-guide/token-based-authentication, a new token needs to be regenerated for each upload review. | Required parameter |      |

### Return Parameters

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Return Result Parameter Name** | **Parameter Type** | **Parameter Description** | **Is it Required** | **Specification** |
| --- | --- | --- | --- | --- |
| requestId | string | Unique