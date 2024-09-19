# Shumei Intelligent Video Stream Recognition Product API Documentation

## Video Stream Upload Request

### Interface Description

This interface is used to submit video stream identification and related information. After the stream is stabilized, the corresponding recognition results will be continuously called back to the specified callback address.

### Request URL:
| Cluster | URL | Supported Product List |
| --- | --- | --- |
| Shanghai | `http://api-videostream-sh.fengkongcloud.com/videostream/v4` | Chinese Video Stream |
| Singapore | `http://api-videostream-xjp.fengkongcloud.com/videostream/v4` | Chinese Video Stream<br/>English Video Stream<br/>Arabic Video Stream |
| Silicon Valley | `http://api-videostream-gg.fengkongcloud.com/videostream/v4` | Chinese Video Stream<br/>English Video Stream<br/>Arabic Video Stream |

### Request Method:

`POST`

### Supported Protocols

`HTTP` or `HTTPS`

### Character Encoding:

`UTF-8`

### Suggested Timeout:

7s

### Request Parameters:

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| accessKey | string | Company Key | Required | Assigned by Shumei |
| appId | string | Application Identifier | Required | Needs to contact Shumei service to activate, use the value provided by Shumei |
| eventId | string | Event Identifier | Required | Used to distinguish scene data, needs to contact Shumei service to activate, use the value provided by Shumei |
| imgType | string | Regulatory type of the image in the video that needs to be identified, **at least one of imgType or imgBusinessType must be provided** | Optional | Primary regulatory label<br/>Optional values:<br/>`POLITY`: Political identification<br/>`EROTIC`: Pornographic & sexy violation identification<br/>`VIOLENT`: Violent & prohibited identification<br/>`QRCODE`: QR code identification<br/>`ADVERT`: Advertisement identification<br/>`IMGTEXTRISK`: Image text violation identification<br/>If multiple functions need to be identified, connect them with an underscore, such as `POLITY_QRCODE_ADVERT` for political, QR code, and advertisement combined identification |
| audioType | string | Regulatory type of the audio in the video stream that needs to be identified, **at least one of audioType or audioBusinessType must be provided** | Optional | Primary regulatory label<br/>Optional values:<br/>`POLITY`: Political identification<br/>`EROTIC`: Pornographic identification<br/>`ADVERT`: Advertisement identification<br/>`DIRTY`: Abusive identification<br/>`ADLAW`: Advertisement law<br/>`MOAN`: Moan identification<br/>`AUDIOPOLITICAL`: Audio political<br/>`ANTHEN`: National anthem identification<br/>`NONE`: No audio detection<br/>For combined identification, connect them with an underscore, such as `POLITY_EROTIC` for political and pornographic identification |
| imgBusinessType | string | Business type of the image in the video that needs to be identified, **at least one of imgType or imgBusinessType must be provided** | Optional | Optional values refer to [imgBusinessType Optional Values List](#imgbusinesstype可选值列表)<br/>If multiple functions need to be identified, connect them with an underscore |
| audioBusinessType | string | Business type of the audio in the video stream that needs to be identified, **at least one of audioType or audioBusinessType must be provided** | Optional | Primary business label<br/>Optional values:<br/>`SING`: Singing identification<br/>`LANGUAGE`: Language identification<br/>`MINOR`: Minor identification<br/>`GENDER`: Gender identification<br/>`TIMBRE`: Timbre identification, must be provided with `GENDER` to be effective<br/>`APPNAME`: App name identification |
| imgCallback | string | Image callback address | Required | The detection results of the captured images in the video stream will be called back to the user through this address |
| audioCallback | string | Audio callback address | Optional | The detection results of the audio segments in the video stream will be called back to the user through this address; must be provided when audio needs to be identified |
| data | json_object | Request data content | Required | Maximum 1MB, see [data content below](#data) |

<span id="data">The content of data is as follows:</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| lang | string | Language | Required | Optional values are as follows:<br/>`zh`: Chinese<br/>`en`: English<br/>`ar`: Arabic<br/>Default value: `zh` |
| tokenId | string | Unique identifier of the client user account | Required | Used for user behavior analysis, it is recommended to pass in the user UID; maximum 40 characters |
| streamType | string | Video stream type | Required | Optional values are:<br/>`NORMAL`: Normal stream address, currently supports `rtmp`, `rtmps`, `hls`, `http`, `https` protocols, supports `flv`, `m3u8` formats<br/>`AGORA`: Agora review<br/>`TRTC`: Tencent review<br/>`ZEGO`: Zego review<br/>`VOLC`: Volcano engine review<br/>`ALI`: Alibaba Cloud review<br/>Note: When using the RTC SDK recording solution, additional recording fees may be incurred on the RTC side. Please consult the relevant RTC vendor for specific fees |
| agoraParam | json_object | Agora stream parameters | Optional | Parameters of the Agora stream to be detected (must be provided when streamType is `AGORA`), see [agoraParam description](#agoraParam) |
| trtcParam | json_object | Tencent stream parameters | Optional | Parameters of the TRTC stream to be detected (must be provided when streamType is `TRTC`), see [trtcParam description](#trtcParam) |
| zegoParam | json_object | Zego stream parameters | Optional | Parameters of the Zego stream to be detected (must be provided when streamType is `ZEGO`), see [zegoParam description](#zegoParam) |
| volcParam | json_object | Volcano stream parameters | Optional | Parameters of the Volcano stream to be detected (must be provided when streamType is `VOLC`), see [volcParam description](#volcParam) |
| aliParam | json_object | Alibaba stream parameters | Optional | Parameters of the Alibaba stream to be detected (must be provided when streamType is `ALI`), see [aliParam description](#aliParam) |
| url | string | URL address of the video to be detected | Optional | URL parameter of the stream to be detected (must be provided when streamType is NORMAL) |
| streamName | string | Video stream name | Optional | Used for background interface display, it is recommended to pass in |
| ip | string | Client IP | Optional | This parameter is used for user behavior analysis at the IP dimension and can also be used to compare with Shumei's IP blacklist |
| audioDetectStep | int | Audio review step length in the video stream | Optional | Unit is in pieces, the value range is 1-36 integers, 1 means skipping one 10S audio segment review, 2 means skipping two, and so on. If this function is not used, all audio content will be reviewed |
| returnAllImg | int | Users can choose to return images with different review results according to their needs | Optional | Optional values are as follows: (default value is `0`)<br/>`0`: Callback image review information with reject and review results<br/>`1`: Callback image review information with all results |
| returnAllText | int | Return the risk level of the audio segment recognition result | Optional | Optional values are as follows: (default value is `0`)<br/>`0`: Return audio segments and text content with risk levels other than pass<br/>`1`: Return audio segments and text content with all risk levels |
| returnPreText | int | If 1, return the text content of the audio segment for the previous 10 seconds and the current 10 seconds, totaling 20 seconds | Optional | Optional values are as follows: (default value is `0`)<br/>`1`: The content field of the return includes the text content of the previous minute of the violation audio<br/>`0`: The content field of the return only includes the text content of the violation audio segment |
| returnPreAudio | int | If 1, return the audio segment link for the previous 10 seconds and the current 10 seconds, totaling 20 seconds | Optional | Optional values are as follows: (default value is `0`)<br/>`1`: Return the audio link of the previous minute of the violation audio<br/>`0`: Only return the audio link of the violation segment |
| returnFinishInfo | int | If 1, return the end notification when the stream ends | Optional | Optional values are as follows: (default value is `0`)<br/>`1`: Send an end notification when the review ends<br/>`0`: Do not send an end notification when the review ends, see [End Stream Return Parameters](#审核结束回调参数) for detailed return parameters |
| detectFrequency | int | Frame capture frequency interval | Optional | Unit is in seconds, the value range is 1~60s, rounded down for decimals, less than 1 is treated as 1S, if not provided, the default is to capture a frame every 3s |
| detectStep | int | Video stream frame capture image detection step length | Optional | Each step of the captured image will only be detected once, the value is greater than or equal to 1. |
| room | string | Live room/game room number | Optional | Different strategies can be formulated for individual rooms; |
| imgBusinessDetectStep | int | Image business label detection step length | Optional | Each step will only detect imgBusinessType once, the value is greater than or equal to 1.<br/>Default value=1, which means all segments are reviewed for business labels. |
| extra | json_object | Extended information | Optional | See [extra description](#extra) |

<span id="extra">In data, the content of extra is as follows</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| passThrough | json_object | Pass-through field | Optional | The content of this field will be returned as is with the callback result |

<span id="agoraParam">The content of agoraParam is as follows:</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| appId | string | Application identifier provided by Agora | Required | |
| channel | string | Channel name provided by Agora | Required | |
| token | string | | Required | Users with high security requirements can use a token, see Agora documentation for token generation method: [https://docs.agora.io/cn/Recording/token](https://docs.agora.io/cn/Recording/token) |
| channelProfile | int | Channel mode for Agora recording | No | Optional values are as follows: (default value is `0`)<br/>`0`: Communication (default), common 1-to-1 single chat or group chat, any user in the channel can speak freely;<br/>`1`: Live broadcast, with two user roles: host and audience. |
| uid | int | User ID | Optional | 32-bit unsigned integer. When the token exists, the user ID used to generate the token must be provided. Note that the uid provided for server-side recording must not exist in the room |
| enableIntraRequest | bool | Whether to enable keyframe request | Optional | This parameter defaults to true, which can improve the audio and video experience under weak network conditions. If you want the recorded video in single stream mode to specify the playback position, set `enableIntraRequest` to false.<br/>false: Disable keyframe request, all streaming ends in the channel send a keyframe every 2 seconds. After disabling, the recorded video in single stream mode can specify the playback position.<br/>true: (default) The streaming end controls whether to enable keyframe request. After enabling, the recorded video file in single stream mode cannot specify the playback position. |
| enableH265Support | bool | Whether to support recording H.265 video stream | Optional | false: (default) Do not support recording H.265 video stream. Remote users in the channel cannot send H.265 video streams.<br/>true: Support recording H.265 video stream. |
| subscribeMode | string | Subscription mode | Optional | `AUTO`: Automatically subscribe to all streams in the room, the default behavior when subscribeMode is not set<br/>`UNTRUSTED`: Only subscribe to the user streams specified in the untrustedUserIdList, if the untrustedUserIdList is empty in this mode, it is an error because no streams can be subscribed<br/>`TRUSTED`: Only subscribe to the user streams outside the trustedUserIdList, if there are no users outside the trustedUserIdList in the room for a certain period, and the untrustedUserIdList is empty, Shumei will actively end the review. |
| trustedUserIdList | int_array | List of trusted users | Optional | Effective when subscribeMode is `TRUSTED`, cannot be empty, Shumei will not subscribe to the user streams specified in this list in the room<br/>Comma-separated UID array, such as `[1,2]`, up to 17 users |
| untrustedUserIdList | int_array | List of untrusted users | Optional | Effective when subscribeMode is `UNTRUSTED`, cannot be empty, Shumei only subscribes to the user streams specified in this list in the room<br/>Comma-separated UID array, such as `[1,2]`, up to 17 users |

Parameter usage instructions: If you need to review different users in the same room, please generate different uids each time you pass in to prevent the phenomenon of mutual kicking when reviewing different users in the same room; if you do not have the uid field (in the case of not using a token), you can consider passing in the corresponding subscription mode parameter: subscribeMode, when this field is not passed or passed as AUTO, automatically subscribe to all streams in the room, when this field is passed as TRUSTED, please pass in the corresponding trustedUserIdList, Shumei will not subscribe to the user streams specified in this list in the room, when this field is passed as UNTRUSTED, please pass in the corresponding untrustedUserIdList, Shumei only subscribes to the user streams specified in this list in the room, we will filter specific users for review based on this field subscribeMode and the corresponding list.

<span id="trtcParam">The content of trtcParam is as follows:</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| sdkAppId | int | Y | Required | sdkAppId provided by Tencent |
| demoSences | int | Y | Required | Recording type optional values:<br/>`2`: Split stream recording<br/>`4`: Mixed stream recording |
| userId | string | Y | Required | UserId assigned to the recording segment, limited to 32 bits, only allows (a-zA-Z), numbers (0-9), underscores, and hyphens |
| userSig | string | Y | Required | Verification signature corresponding to the recording userId, equivalent to a login password |
| roomId | int | Y | Optional | Room number, value range: [1-4294967294] roomId and strRoomId must pass one, if both have values, roomId is preferred Note: Currently, a room can only review up to 8 users |
| strRoomId | string | Y | Required | Room number value description: only allows (a-zA-Z), numbers (0-9), underscores, and hyphens, if you choose strRoomId, you need to note that if both strRoomId and roomId have values, roomId is preferred |

<span id="zegoParam">The content of data.zegoParam is as follows:</span>

| Request Parameter Name | Type | Parameter Description | Transmission Description | Specification |
| --- | --- | --- | --- | --- |
| tokenId | string | Zego authentication token | Required | Identity_token authentication information provided by zego, used for token login (a new token must be actively called from the zego interface each time a stream is opened)<br/>See [Audio and Video Stream Review Authentication Token](https://doc-zh.zego.im/article/15258) for the acquisition method |
| roomId     | string | Zego room number    | Required | Room number to be reviewed                                             |

<span id="volcParam">The content of data.volcParam is as follows:</span>

| Request Parameter Name | Type | Parameter Description | Transmission Description | Specification |
| --- | --- | --- | --- | --- |
| appId | string | Application identifier provided by Volcano | Required | 
| roomId | string | Room number | Required | 
| userId | string | UserId assigned to the recording end | Required | 
| token | string | Verification signature corresponding to the recording userId, equivalent to a login password | Required |

<span id="aliParam">The content of data.aliParam is as follows:</span>

| Request Parameter Name | Type   | Parameter Description                                                     | Transmission Description | Specification |
| ---------- | ------ | ------------------------------------------------------------ | -------- | ---- |
| room       | string | Room ID, must be consistent with the channel ID used to generate the token. The server pulls the stream and records it by room. Room is a unique identifier, the same room will not pull the stream repeatedly. | Required |      |
| userId     | string | Pull stream robot ID, must be consistent with the userId used to generate the token.              | Required |      |
| token      | string | Used for the pull stream end to join the channel, see the documentation for the generation method: https://help.aliyun.com/zh/live/user-guide/token-based-authentication, a new token must be generated for each upload review. | Required |      |

### Return Parameters

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Return Result Parameter Name** | **Parameter Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| requestId | string | Unique identifier for this request | Yes | Unique request identifier |
| code | int | Request return code | Yes | See [Interface Response Code List](#接口响应码列表) |
| message | string | Request return description, corresponding to the request return code | Yes | See [Interface Response Code List](#接口响应码列表) |
|