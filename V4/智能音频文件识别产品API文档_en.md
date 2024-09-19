# Intelligent Audio File Recognition Product API Documentation

## Asynchronous Interface

### Request URL

| Cluster | URL                                              | Supported Languages                                 |
| ------- | ------------------------------------------------ | -------------------------------------------------- |
| Shanghai | `http://api-audio-sh.fengkongcloud.com/audio/v4` | Chinese, Arabic |
| Silicon Valley | `http://api-audio-gg.fengkongcloud.com/audio/v4` | Chinese, English, Arabic |
| Singapore | `http://api-audio-xjp.fengkongcloud.com/audio/v4` | Chinese, English, Arabic |

### Character Encoding

`UTF-8`

### Request Method

`POST`

### Suggested Timeout Duration

1s

### Audio Format Restrictions

`WAV`, `MP3`, `AAC`, `AMR`, `3GP`, `M4A`, `WMA`, `OGG`, `APE`, `FLAC`, `ALAC`, `WAVPACK`, `SILK_V3`, etc.

### Request Body Limit

The total size of all request parameters should not exceed 18M

### Request Parameters

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name** | **Type**    | **Description**         | **Required**               | **Specification**                                                     |
| :----------------- | :---------- | :---------------------- | :------------------------- | :-------------------------------------------------------------------- |
| accessKey          | string      | Company key             | Required                   | Provided by Shumei                                                   |
| appId              | string      | Application identifier  | Required                   | Used to distinguish applications, needs to be activated by contacting Shumei service, please use the value provided by Shumei |
| eventId            | string      | Event identifier        | Required                   | Used to distinguish scene data, needs to be activated by contacting Shumei service, please use the value provided by Shumei |
| type               | string      | Risk type to be detected | Either businesstype or type is required | <p>AUDIOPOLITICAL: Leader voiceprint recognition</p><p>POLITY: Political recognition</p><p>EROTIC: Erotic recognition</p><p>ADVERT: Advertisement recognition</p><p>ADLAW: Advertisement law recognition</p><p>BAN: Prohibited recognition</p><p>VIOLENT: Violent and terrorist recognition</p><p>ANTHEN: National anthem recognition</p><p>MOAN: Moan recognition</p><p>DIRTY: Abusive recognition</p><p>BANEDAUDIO: Prohibited songs</p><p>For combined recognition, connect with underscores, e.g., POLITY_EROTIC_MOAN for political, erotic, and moan recognition</p><p>Suggested value:<br/>POLITY_EROTIC_MOAN_ADVERT</p> |
| businessType       | string      | Business tag type to be detected | Either businesstype or type is required | Optional values:<br/>SING: Singing recognition<br/>LANGUAGE: Language recognition<br/>GENDER: Gender recognition<br/>TIMBRE: Timbre recognition <br/>VOICE: Voice attributes<br/>MINOR: Minor recognition<br/>AUDIOSCENE: Sound scene<br/>AGE: Age recognition<br/>For timbre, singing, and language recognition, GENDER is required<br/>Either type or businessType must be filled |
| contentType        | string      | Format of the audio content to be recognized | Required                   | <p>Optional values:</p><p>URL: The content to be recognized is an audio URL address;</p><p>RAW: The content to be recognized is base64 encoded audio data</p> |
| content            | string      | Audio content to be recognized | Required                   | <p>Can be a URL address or base64 encoded data.</p><p>Base64 encoded data is limited to 15M, only supports pcm, wav, mp3 formats, and pcm format data must use 16-bit little-endian encoding. It is recommended to use pcm, wav format for transmission</p> |
| data               | json object | Information related to this request | Required                   | Maximum 1MB, [see data parameters](#data)                               |
| btId               | string      | Unique identifier for the audio file | Required                   | Uniquely identifies this audio file, to facilitate matching the callback result, truncated if over 128 characters, must be unique |
| callback           | string      | Callback HTTP interface | Optional                   | When this field is not empty, the service will notify the user of the review result based on this field         |
| acceptLang         | string      | Language type of the returned tags | Optional                   | Choose the language type of the returned tags<br/>Optional values:<br/>zh: Chinese<br/>en: English<br/>Default is Chinese if not provided |

Among them, the content of <span id="data">data</span> is as follows:

| **Parameter Name** | **Type** | **Description**       | **Required** | **Specification**                                                                                   |
| :----------------- | :------- | :-------------------- | :----------- | :-------------------------------------------------------------------------------------------------- |
| tokenId            | string   | User account          | Optional     | Used for user behavior analysis, it is recommended to pass in the user UID                                                          |
| formatInfo         | string   | Audio data format     | Optional     | Must be present when the audio content format is RAW, optional values: pcm, wav, mp3                                       |
| rate               | int      | Audio data sampling rate | Optional     | Must be present when the audio data format is pcm, range 8000-32000.                                        |
| track              | int      | Number of audio channels | Optional     | <p>Must be present when the audio data format is pcm, optional values:</p><p>1: Mono</p><p>2: Stereo</p>             |
| returnAllText      | int      | Return level of audio segments | Optional     | Optional values are as follows (default is `0`):<br/> `0`: Return risk segment recognition results<br/> `1`: Return all segment recognition results<br/>This parameter is only used to control the return of segment recognition results, it does not affect the return of the overall recognition result.<br/>When "Return all segment recognition results" is selected, the segment recognition results include segments with riskLevel of PASS, REVIEW, and REJECT;<br/>When "Return risk segment recognition results" is selected, the segment recognition results only include segments with riskLevel of REVIEW and REJECT;<br/>Segment recognition results correspond to the audioDetail field in the callback or query response. |
| audioDetectStep | int | Interval review step length | Optional | Interval review step length, the value range is 1-36 integers, 1 means skipping one 10S audio segment review, 2 means skipping two, and so on. When this function is not used, the entire audio content is reviewed. When this function is enabled, it is recommended to enable returnAllText and use the ASR recognition results of each segment. |
| receiveTokenId | string | TokenId of the message receiver in private chat scenarios | N | A string composed of numbers, letters, underscores, and hyphens, with a length of less than or equal to 64 characters |
| lang           | string   | Audio language type     | Optional   | Optional values are as follows (default is zh):<br/>`zh`: Chinese<br/>`en`: English<br/>`ar`: Arabic<br/>`hi`: Hindi<br/>`es`: Spanish<br/>`fr`: French<br/>`ru`: Russian<br/>`pt`: Portuguese<br/>`id`: Indonesian<br/>`de`: German<br/>`ja`: Japanese<br/>`tr`: Turkish<br/>`vi`: Vietnamese<br/>`it`: Italian<br/>`th`: Thai<br/>`tl`: Filipino<br/>`ko`: Korean<br/>`ms`: Malay                    |
| deviceId        | string | Shumei device fingerprint identifier  | Optional    | The unique identifier generated by Shumei device fingerprint                                                               |
| room        | string | Room number  | Optional    | Room number, it is recommended to pass in                                                                |
| dataId | string | Data identifier | Optional | Data identifier |
| ip              | string | IPv4 or IPv6 address | Optional    | The public IP address of the user sending the audio                                                      |
| level | int | User level, different interception strategies can be configured for users of different levels | Optional | Optional values:<br/>`0`: Lowest level user, typically new registered, completely inactive or level 0 users, etc.;<br/>`1`: Lower level user, typically low active or low level users, etc.;<br/>`2`: Medium level user, typically users with certain activity or medium level, etc.;<br/>`3`: Higher level user, typically high active or high level users, etc.;<br/>`4`: Highest level user, typically paid users, VIP users, etc. |
| gender | string | User gender | Optional | Optional values:<br/>male: Male <br/>female: Female |
| extra | json object | Auxiliary parameters | Optional | [See extra parameters](#extra) |

<span id="extra">In data, the content of extra is as follows:</span>

| **Parameter Name** | **Type**    | **Description** | **Required** | **Specification**                                   |
| ------------------ | ----------- | --------------- | ------------ | -------------------------------------------------- |
| passThrough        | json_object | Pass-through field | N            | Pass-through field, all content under this field will be returned through the callback. |

### Return Parameters

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Return Result Parameter Name** | **Parameter Type** | **Description**                   | **Required** | **Specification**                                                     |
| :------------------------------- | :----------------- | :-------------------------------- | :----------- | :----------------------------------------------------------- |
| requestId                        | string             | Unique identifier of the request  | Yes           |                                                              |
| code                             | int                | Request return code               | Yes           | <p>1100: Success</p><p>1901: QPS limit exceeded</p><p>1902: Invalid parameters</p><p>1903: Service failure</p><p>1904: Download failure</p><p>1905: Decoding failure</p><p>9101: No permission to operate</p> |
| message                          | string             | Request return description, corresponding to the request return code | Yes           |                                                              |
| btId                             | string             | Unique identifier of the audio    | No           | Returned when code is 1100                                         |

## Active Query Results

### Request URL

| Cluster | URL                                                    | Supported Languages                                   |
| ------- | ------------------------------------------------------ | -------------------------------------------------- |
| Shanghai | `http://api-audio-sh.fengkongcloud.com/query_audio/v4` | Chinese               |
| Silicon Valley | `http://api-audio-gg.fengkongcloud.com/query_audio/v4` | Chinese, English, Arabic |
| Singapore | `http://api-audio-xjp.fengkongcloud.com/query_audio/v4` | Chinese, English, Arabic |

### Character Encoding

`UTF-8`

### Request Method

`POST`

### Suggested Timeout Duration

1s

### Request Parameters

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name** | **Type** | **Description**     | **Required** | **Specification**                               |
| :----------------- | :------- | :------------------ | :----------- | :--------------------------------------------- |
| accessKey          | string   | Company key         | Required     | Provided by Shumei                             |
| btId               | string   | Unique identifier for the audio file | Required     | Uniquely identifies this audio file, used to query recognition results |

### Return Parameters

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name** | **Type**    | **Description**                   | **Required** | **Specification**                                                     |
| :----------------- | :---------- | :-------------------------------- | :----------- | :----------------------------------------------------------- |
| requestId          | string      | Unique identifier of this request | Yes           |                                                              |
| btId               | string      | Unique identifier of the audio    | Yes           |                                                              |
| code               | int         | Request return code               | Yes           | <p>1100: Success</p><p>1101: Processing</p><p>1901: QPS limit exceeded</p><p>1902: Invalid parameters</p><p>1903: Service failure</p><p>1904: Download failure</p><p>1905: Decoding failure</p><p>9100: Insufficient balance</p><p>9101: No permission to operate</p><p>Except for message and requestId, other fields only exist when code is 1100</p> |
| message            | string      | Request return description, corresponding to the request return code | Yes           |                                                              |
| riskLevel          | string      | Handling suggestion for the current event | Yes           | <p>Possible return values:<br/>PASS: Pass</p><p>REVIEW: Review</p><p>REJECT: Reject</p><p>Suggestion: Do not use the result directly during the initial integration, adjust the interception scale, and use it after meeting expectations</p> |
| audioText          | string      | Transcription result of the entire audio | Yes           |                                                              |
| audioTime          | int         | Duration of the entire audio      | Yes           | In seconds                                                       |
| audioDetail        | json_array  | Audio segment information         | Yes           | Callback audio segment information, [see audioDetail parameters](#audioDetail)      |
| audioTags          | json_object | Audio tags                        | No           | Returns tags such as gender, timbre, whether singing, etc. Historical compatibility field, it is recommended to use businessLabels directly |
| requestParams      | json_object | Pass-through field                | Yes           | Returns all fields under data                                           |
| auxInfo            | json_object | Auxiliary information             | No           |                                                              |

<span id="audioDetail">The content of each element in audioDetail is as follows:</span>

| **Parameter Name** | **Type**    | **Description**     | **Required** | **Specification**                                                     |
| :----------------- | :---------- | :------------------ | :----------- | :----------------------------------------------------------- |
| audioStarttime     | float       | Start time of the audio segment | Yes           | Relative to the start time of the audio, in seconds                             |
| audioEndtime       | float       | End time of the audio segment   | Yes           | Relative to the start time of the audio, in seconds                             |
| audioUrl           | string      | Audio segment link              | Yes           | mp3 format                                                      |
| riskLevel          | string      | Recognition result of the audio segment | Yes           | <p>Possible return values:<br/>PASS: Pass</p><p>REVIEW: Review</p><p>REJECT: Reject</p> |
| businessLabels     | json_array  | Business tag returns            | No           | All business tags, [see businessLabels parameters](#businessLabels2)     |
| allLabels          | json_array  | Risk tag returns                | No           | All risk tags, [see allLabels parameters](#allLabels2)               |
| riskLabel1         | string      | Primary risk tag                | Yes           |                                                              |
| riskLabel2         | string      | Secondary risk tag              | Yes           |                                                              |
| riskLabel3         | string      | Tertiary risk tag               | Yes           |                                                              |
| riskDescription    | string      | Reason for risk                 | Yes           | Only for human understanding of the risk reason, programs should not rely on this parameter value for logic processing |
| riskDetail         | json_object | Risk details                    | No           | [See riskDetail parameters](#riskDetail)                            |

Among them, the detailed content of <span id="riskDetail">riskDetail</span> is as follows:

| **Parameter Name** | **Type**   | **Description**             | **Required** | **Specification**                                                                                  |
| :----------------- | :--------- | :-------------------------- | :----------- | :---------------------------------------------------------------------------------------- |
| audioText          | string     | Transcription result of the audio | No           |                                                                                           |
| matchedLists       | json_array | Information of the hit custom list | No           | Returned when hitting the custom list, [see matchedLists parameters](#matchedLists)                         |
| riskSegments       | json_array | High-risk content segments   | No           | Present in functions such as political, violent, prohibited, competitive, advertisement law, etc., [see riskSegments parameters](#riskSegments) |

In riskDetail, the detailed content of <span id="matchedLists">matchedLists</span> is as follows:

| **Parameter Name** | **Type**   | **Description**                 | **Required** | **Specification**                  |
| :----------------- | :--------- | :------------------------------ | :----------- | :------------------------ |
| name               | string     | Name of the custom list         | Yes           |                           |
| words              | json_array | Information of the sensitive words hit in this list | Yes           | [See words parameters](#words) |

In matchedLists, the detailed content of <span id="words">words</span> is as follows:

| **Parameter Name** | **Type**  | **Description**   | **Required** | **Specification** |
| :----------------- | :-------- | :---------------- | :----------- | :------- |
| word               | string    | Sensitive word    | Yes           |          |
| position           | int_array | Position of the sensitive word | Yes           |          |

In riskDetail, the detailed content of <span id="riskSegments">riskSegments</span> is as follows:

| **Parameter Name** | **Type**  | **Description**           | **Required** | **Specification** |
| :----------------- | :-------- | :------------------------ | :----------- | :------- |
| segment            | string    | High-risk content segment | No           |          |
| position           | int_array | Position of the high-risk content segment | No           |          |

Among them, the detailed content of <span id="audioTags">audioTags</span> is as follows:

| **Parameter Name** | **Type**    | **Description** | **Required** | **Specification**                                                                           |
| :----------------- | :---------- | :-------------- | :----------- | :--------------------------------------------------------------------------------- |
| gender             | json_object | Gender tag      | No           | Returned when type includes GENDER                                                         |
| timbre             | json_array  | Timbre tag      | No           | Returned when type includes TIMBRE                                                         |
| song               | int         | Singing tag     | No           | <p>Returned when type includes SING</p><p>Possible values:</p><p>0: No singing</p><p>1: Singing</p> |
| language           | json_object | Language recognition | No           | Returned when type includes LANGUAGE                                                         |

In audioTags, the detailed content of gender is as follows:

| **Parameter Name**  | **Type** | **Description**      