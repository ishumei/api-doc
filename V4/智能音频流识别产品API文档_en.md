# Intelligent Audio Stream Recognition Product API Documentation


## Audio Stream Upload Request

### Request URL:

| Cluster   | URL                                                           | <span id="language">Supported Languages</span> |
| ------ | ------------------------------------------------------------- | ---------------------------------------- |
| Singapore | `http://api-audiostream-xjp.fengkongcloud.com/audiostream/v4` | Chinese, International |
| Silicon Valley   | `http://api-audiostream-gg.fengkongcloud.com/audiostream/v4`  | Chinese, International |
| Shanghai   | `http://api-audiostream-sh.fengkongcloud.com/audiostream/v4`  | Chinese, Arabic |

### Request Method:

`POST`

### Character Encoding:

`UTF-8`

### Suggested Timeout:

1s

### Stream Retry Mechanism

To prevent stream failure due to network anomalies, the Shumei audio stream service has a retry mechanism for stream failures. The specific mechanism is as follows:

Ordinary rtmp, http, hls streams will retry 12 times, with the first interval being 5 seconds, the second interval being 10 seconds, and so on, with a maximum interval of no more than 60 seconds.<br/>
For streams recorded via Agora SDK, retry 2 times with an interval of 0.<br/>
For streams recorded via Zego SDK, retry 10 times with an interval of 30 seconds each time.

### Request Parameters:

Placed in the HTTP Body in JSON format, the specific parameters are as follows:

| **Request Parameter Name** | **Type**    | **Parameter Description**       | **Required**               | **Specification**                                                     |
| -------------- | ----------- | ------------------ | -------------------------- | ------------------------------------------------------------ |
| accessKey      | string      | Company Key           | Y                          | Provided by Shumei                                                   |
| appId          | string      | Application Identifier           | Y                          | Used to distinguish applications<br/>Contact Shumei service to activate, use the value provided by Shumei<br/> |
| eventId        | string      | Event Identifier           | Y                          | Distinguish scene data<br/>Contact Shumei service to activate, use the value provided by Shumei<br/> |
| type           | string      | Risk Type to Detect     | Either businesstype or type is required | Optional values: Regulatory functions<br/>`POLITY`: Political recognition<br/>`EROTIC`: Erotic recognition<br/>`ADVERT`: Advertisement recognition<br/>`BAN`: Prohibited recognition<br/>`VIOLENT`: Violent and terrorist recognition<br/>`MOAN`: Moan recognition<br/>`AUDIOPOLITICAL`: Leader voiceprint recognition<br/>`ANTHEN`: National anthem recognition<br/>`DIRTY`: Abusive recognition<br/>`ADLAW`: Advertisement law recognition<br/>`SING`: Singing recognition<br/>`MINOR`: Minor recognition<br/>`BANEDAUDIO`: Prohibited songs<br/>`VOICE`: Human voice attributes (fake voice)<br/>For combined recognition, connect with underscores, e.g., `POLITY_EROTIC_MOAN`<br/>Political, erotic, and moan recognition, political, erotic, abusive, and advertisement recognition refer to semantic content risk detection |
| businessType   | string      | Business Tag           | Either businesstype or type is required | Optional values: Primary, secondary, and tertiary business tags<br/>`GENDER`: Gender recognition<br/>`AGE`: Age recognition<br/>`TIMBRE`: Timbre recognition<br/>`SING`: Singing recognition<br/>`LANGUAGE`: Language recognition<br/>`VOICE`: Human voice attributes<br/>`AUDIOSCENE`: Sound scene<br/>For timbre, singing, and language recognition, `GENDER` is required |
| data           | json_object | Request Data Content     | Y                          | Information related to this request, maximum 1MB, [see data parameters](#data)              |
| callback       | string      | Callback URL           | Y                          | Asynchronous detection result callback notification URL, supports HTTP and HTTPS                 |
| acceptLang     | string      | Return Label Language Type | N                          | Choose the language type for return labels<br/>Optional values:<br/>zh: Chinese<br/>en: English<br/>Default is Chinese if not provided |

<span id="data">The content of data is as follows:</span>

| **Request Parameter Name** | **Type**    | **Parameter Description**                                           | **Required** | **Specification**                                                                                                                                                     |
| -------------- | ----------- | ------------------------------------------------------ | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| tokenId        | string      | User Account Identifier                                           | Y            | Used to distinguish user accounts, it is recommended to pass in the user ID                                                                                                                             |
| btId           | string      | Unique Audio Identifier                     | Y            | Used to query specific audio, limited to 128 characters                                                                                                                              |
| streamType     | string      | Stream Type                                                 | Y            | Optional values:<br/>`NORMAL`: Ordinary stream address, currently supports rtmp, rtmps, hls, http, https protocols, supports flv, m3u8 formats<br/>`ZEGO`: Zego<br/>`AGORA`: Agora <br/>`TRTC`: Tencent recording<br/>`VOLC`: Volcano Engine recording<br/>`GIN`: Giant recording<br/>`ALI`: Ali recording<br/>Note: When using RTC SDK recording solutions, additional recording fees will be incurred on the RTC side, please consult the relevant RTC vendor for specific fees |
| url            | string      | Live Stream Address                                             | N            | Required when streamType is `NORMAL`                                                                                                                                 |
| lang           | string      | Audio Stream Language Type                                         | Y            | Optional values (default is `zh`):<br/>`zh`: Chinese<br/>`en`: English<br/>`ar`: Arabic<br/>`hi`: Hindi<br/>`es`: Spanish<br/>`fr`: French<br/>`ru`: Russian<br/>`pt`: Portuguese<br/>`id`: Indonesian<br/>`de`: German<br/>`ja`: Japanese<br/>`tr`: Turkish<br/>`vi`: Vietnamese<br/>`it`: Italian<br/>`th`: Thai<br/>`tl`: Filipino<br/>`ko`: Korean<br/>`ms`: Malay<br/>[Supported languages for clusters see Request URL Supported Languages](#language), other than Chinese, other language types are internationalized |
| zegoParam      | json_object | Stream Parameters to be Detected                                         | N            | Required when streamType is `ZEGO`, [see zegoParam parameters](#zegoParam)                                                                                                  |
| initDomain     | int         | Whether Zego SDK Initialization Sets Isolation Domain                        | N            | When the Zego client init initialization supports isolation domain and random userId, this field is required, optional values:<br/>`0`: Default version<br/>`1`: Only supports client initialization with isolation domain<br/>`2`: Supports client initialization with isolation domain and random userId function<br/>`3`: Update SDK, fix some bugs<br/>`4`: Support custom SEI information<br/>`5`: Support vad silence detection, token will have uniqueness check, each upload for review must regenerate<br/>`6`: In room dimension stream review mode, developers can control whether a stream needs to be reviewed<br/>**Recommended to use `6` for access;** For compatibility with old customers, the default value is 0 |
| trtcParam      | json_object | Tencent Recording Parameters (required when streamType is TRTC), see extended parameters | N            | Tencent recording parameters (required when streamType is TRTC), see extended parameters                                                                                                       |
| agoraParam     | json_object | Agora Stream Parameters to be Detected                                     | N            | Required when streamType is `AGORA`, [see agoraParam parameters](#agoraParam)                                                                                                |
| volcParam      | json_object | Volcano Stream Parameters to be Detected                                     | N            | Required when streamType is `VOLC`, [see volcParam parameters](#volcParam)                                                                                                |
| ginParam     | json_object | Giant Stream Parameters to be Detected                                     | N            | Required when streamType is `GIN`, [see ginParam parameters](#ginParam)                                                                                                |
| aliParam | json_object | Ali Stream Parameters to be Detected | N | Required when streamType is `ALI`, [see aliParam parameters](#aliParam) |
| room           | string      | Live Room Number                                             | N            |                                                                                                                                                              |
| returnAllText  | int         | Return Audio Segment Level                                     | N            | Optional values (default is `0`):<br/>`0`: Return audio segments with non-pass risk levels<br/>`1`: Return all audio segments with risk levels<br/>It is recommended to pass in 1 (default is 0, no callback will be generated in case of silence)             |
| returnPreText  | int         | Whether to Return the Previous Text Information of the Violation Audio Segment                   | N            | Optional values (default is `0`):<br/>`0`: Do not return the previous segment text of the violation segment;<br/>`1`: Return the previous segment text of the violation segment;                                                  |
| returnPreAudio | int         | Whether to Return the Previous Audio Link of the Violation Audio Segment                   | N            | Optional values (default is `0`):<br/>`0`: Do not return the previous segment audio of the violation segment;<br/>`1`: Return the previous segment audio link of the violation segment;                                              |
| returnFinishInfo | int     | Audio Stream End Callback Notification | N            | Optional values (default is `0`):<br/>`0`: Do not send end notification when review ends<br/>`1`: Send end notification when review ends, callback parameters add statCode status code<br/>It is recommended to pass in 1 (default is 0, no callback will be generated when the stream ends)                                                                                                        |
| extra          | json_object | Auxiliary Parameters                                               | N            | Related information for auxiliary audio detection, [see extra parameters](#extra)                                                                                                          |
| liveTitle      | string      | Title                                                   | N            | Room title, optional parameter, pass in when the customer activates human review service                                                                                                                 |
| anchorName     | string      | Nickname                                                   | N            | User nickname, optional parameter, pass in when the customer activates human review service |
| audioDetectStep  | int     | Frame-by-frame Review Step Length                                             | N          | Each step of the audio will only be detected once, the value range is an integer from 1 to 36, the default is to review each segment (note)<br/>Example: If this parameter is set to 1, the first segment, the third segment, the fifth segment will be reviewed, and so on. If this parameter is set to 2, the first segment, the fourth segment, and the seventh segment will be reviewed, and so on.                                                                                                           |
| receiveTokenId | string | TokenId of the Message Receiver in Private Chat Scenario | N | A string composed of numbers, letters, underscores, and hyphens with a length of less than or equal to 64 characters |
| deviceId | string | Shumei Device Identifier | N |  |
| ip | string | Public IP Address of the User Sending the Audio | N | Supports IPV4 or IPV6 |
| level | int | User Level, Different Interception Strategies Can Be Configured for Different Levels of Users | N | Optional values:<br/>`0`: Lowest level user, typically new registered, completely inactive or level 0 users, etc.;<br/>`1`: Lower level user, typically low active or low level users, etc.;<br/>`2`: Medium level user, typically users with certain activity or medium level users, etc.;<br/>`3`: Higher level user, typically high active or high level users, etc.;<br/>`4`: Highest level user, typically paid users, VIP users, etc. |
| gender | string | User Gender | N | Optional values:<br/>male male <br/>female female |

<span id="zegoParam">In data, the detailed content of zegoParam is as follows:</span>

| **Request Parameter Name**  | **Type** | **Parameter Description**                                                 | **Required** | **Specification**                                        |
| --------------- | -------- | ------------------------------------------------------------ | ------------ | ----------------------------------------------- |
| tokenId         | string   | Identity verification information provided by zego, obtain zego's identify_token for login, see zego documentation for generation method: [https://doc-zh.zego.im/article/15258](https://doc-zh.zego.im/article/15258). Note that tokenId uniquely identifies the review request, and a new one needs to be generated for each request. | Y            |                                                 |
| streamId        | string   | Audio stream number, uniquely corresponding to one audio stream, either streamId or roomId must be provided. | N            |                                                 |
| roomId          | string   | Room number, uniquely corresponding to one room.                                 |              |                                                 |
| isMixingEnabled | bool     | Recording mode<br/>true: Mixed stream, all users in the room are recorded and reviewed as one stream. In this case, if streamId and roomId exist separately, they will take effect separately; but if streamId and roomId exist simultaneously, streamId will be the effective value.<br/>false: Separate stream, each user in the room is recorded and reviewed separately. In this case, roomId is required, and roomId will be the effective value. At the same time, streamId cannot be provided. | N            | Default value is `true`<br/>`true`: Mixed stream<br/>`false`: Separate stream |

<span id="agoraParam">In data, the detailed content of agoraParam is as follows:</span>

| **Request Parameter Name**  | **Type** | **Parameter Description**                                                 | **Required** | **Specification**                                                |
| --------------- | -------- | ------------------------------------------------------------ | ------------ | ------------------------------------------------------- |
| appId           | string   | AppId provided by Agora, note the distinction from Shumei's appId                     | Y           | Not Shumei's appId                                           |
| channel         | string   | Channel name provided by Agora                                             | Y           |                                                         |
| token           | string   | Users with higher security requirements can use token for authentication, see Agora documentation for generation method: [https://docs.agora.io/cn/Recording/token\_server?platform=CPP](https://docs.agora.io/cn/Recording/token\_server?platform=CPP) <br/>It is recommended to set the token validity period to exceed the duration of the channel to prevent token expiration from causing stream failure. The maximum token validity period currently supported by Agora is 24 hours, so if the channel duration exceeds 24 hours, the token expiration issue needs to be handled. Solution: Set the audio stream end callback notification in the request parameters (set returnFinishInfo to 1). When the callback receives the review end notification (statCode is 1), and the reason is that the stream token is invalid or expired (errorCode status code under auxInfo returns 3005), if the channel still exists and needs to continue the review, generate a new token and resubmit the channel for review. | N            |                                                         |
| uid             | int      | User ID, when token exists, the user ID used to generate the token must be provided. Note that the uid provided for the recording server cannot exist in the actual room. | N            | 32-bit unsigned integer                                         |
| isMixingEnabled | bool     | Single stream/mixed stream recording<br/>Mixed stream refers to one stream for one live room<br/>Separate stream refers to one stream for one mic position, | N            | Default value is `true`<br/>`true`: Mixed stream<br/>`false`: Separate stream         |
| channelProfile  | int      | Channel mode for Agora recording<br/>Communication, common 1-to-1 single chat or group chat, any user in the channel can speak freely<br/>Live broadcast, with two user roles: host and audience | N            | Optional values (default value is `0`):<br/>`0`: Communication<br/>`1`: Live broadcast |
| subscribeMode  | string      | Subscription mode:<br/>`AUTO`: Automatically subscribe to all streams in the room, the default behavior when subscribeMode is not set<br/>`UNTRUSTED`: Only subscribe to the streams specified in the untrustedUserIdList, only effective in Agora separate stream<br/>`TRUSTED`: Only subscribe to the streams outside the trustedUserIdList, only effective in Agora separate stream |||
| trustedUserIdList  | string_array      | List of trusted users, effective when subscribeMode=TRUSTED, can be empty, Shumei will not subscribe to the streams specified in this list in the room. Each element's uid range should be within the uint32 range, but the type is string. For example: ["123","456"] |||
| untrustedUserIdList  | string_array      | List of untrusted users, effective when subscribeMode=UNTRUSTED, cannot be empty, Shumei will only subscribe to the streams specified in this list in the room. Each element's uid range should be within the uint32 range, but the type is string. For example: ["123","456"] |||


<span id="ginParam">In data, the detailed content of ginParam is as follows:</span>

| **Request Parameter Name**  | **Type** | **Parameter Description**                                                                                                                                                                                   | **Required** | **Specification**                                                |
| --------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ------------------------------------------------------- |
| tokenId           | string   | Room token, used for the stream end to log in to the room, provided by Giant	 | Y            |                                                         |
| roomId         | string   | Room number, uniquely corresponding to one room, the server pulls the stream and records by room unit		                                                                                                                                                                               | Y            |                                                         |
| isMixingEnabled | bool     | Single stream/mixed stream recording<br/>Mixed stream refers to all users in the room being recorded and reviewed as one stream<br/>Separate stream refers to each user in the room being recorded and reviewed separately                                                                                                                      | N           | Default value is `true`<br/>`true`: Mixed stream<br/>`false`: Separate stream         |
| ip         | string   | Specify server IP		                                                                                                                                                                               | Y            |                                                         |
| port         | string   | Specify port		                                                                                                                                                                               | Y            |                                                         |

<span id="aliParam">In data, the detailed content of aliParam is as follows:</span>

| **Request Parameter Name**  | **Type** | **Parameter Description**                                                 | **Required** | **Specification**                                        |
| --------------- | -------- | ------------------------------------------------------------ | ------------ | ----------------------------------------------- |
| token           | string   | Used for the stream end to join the channel, see the documentation for generation method: [Ali token authentication](https://help.aliyun.com/zh/live/user-guide/token-based-authentication), a new token needs to be generated for each upload review. | Y            |                                                 |
| room            | string   | Room ID, needs to be completely consistent with the channelID used to generate the token. The server pulls the stream and records by room unit. room is a unique identifier, the same room will not be pulled repeatedly. | Y            | Required parameter, non-empty string                            |
| user