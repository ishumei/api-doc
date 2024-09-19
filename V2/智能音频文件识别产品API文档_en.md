# Intelligent Audio File Recognition Product API Documentation

- - - - -

***All rights reserved. Unauthorized reproduction is prohibited.***

- - - - -
## Upload Audio Recognition Request

### Interface Description

This interface is used to submit audio recognition requests.

### Request URL:

| Cluster  | URL                                                          | <span id="language">Supported Languages</span> |
| -------- | ------------------------------------------------------------ | ---------------------------------------------- |
| Singapore | `http://api-audio-xjp.fengkongcloud.com/v2/saas/anti_fraud/audio` | Chinese, International                         |
| Silicon Valley | `http://api-audio-gg.fengkongcloud.com/v2/saas/anti_fraud/audio` | Chinese, International                         |
| Shanghai | `http://api-audio-sh.fengkongcloud.com/v2/saas/anti_fraud/audio` | Chinese, Arabic                                |

### Character Encoding Format

Use UTF-8 character set for encoding

### Request Method

POST

### Suggested Timeout Duration

3s

### Audio Format Restrictions

`WAV`, `MP3`, `AAC`, `AMR`, `3GP`, `M4A`, `WMA`, `OGG`, `APE`, `FLAC`, `ALAC`, `WAVPACK`, `SILK_V3`, etc.

### Request Body Limit

The total size of all request parameters should not exceed 18M

### Request Parameters

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name** | **Type**    | **Required** | **Description**                                               |
| :----------------- | :---------- | :----------- | :------------------------------------------------------------ |
| accessKey          | string      | Y            | Service key provided by Shumei when the account service is activated |
| type               | string      | Y            | <p>Type of violation to be recognized, optional values:</p><p>AUDIOPOLITICAL: Voiceprint recognition of the first leader</p><p>POLITY: Political recognition</p><p>ANTHEN: National anthem recognition</p><p>EROTIC: Erotic</p><p>DIRTY: Abusive recognition</p><p>ADVERT: Advertisement recognition</p><p>BAN: Prohibited recognition</p><p>VIOLENT: Violent and terrorist recognition</p><p>ADLAW: Advertisement law recognition</p><p>MOAN: Moan recognition</p><p>BANEDAUDIO: Prohibited songs</p><p>For combined recognition, connect with underscores, e.g.</p><p>POLITY_EROTIC_MOAN for political, erotic, and moan recognition.<br/>Suggested input:<br/>POLITY_EROTIC_MOAN_ADVERT</p> |
| businessType       | string      | N            | Recognition type, optional values:<br/>SING: Singing recognition<br/>LANGUAGE: Language recognition<br/>GENDER: Gender recognition<br/>TIMBRE: Timbre tag (must be passed with GENDER to be effective)<br/>VOICE: Voice attributes<br/>MINOR: Minor recognition<br/>AUDIOSCENE: Sound scene<br/>AGE: Age recognition<br/>For timbre, singing, and language recognition, GENDER must be passed<br/>Either type or businessType must be filled |
| appId              | string      | N            | Needs to contact Shumei for activation, please use the value provided by Shumei separately |
| btId               | string      | Y            | Unique identifier for the audio, truncated if it exceeds 128 characters, cannot be duplicated, otherwise parameter error will be prompted. |
| data               | json_object | Y            | Request data, up to 1MB, detailed content see the table below  |
| callback           | string      | N            | URL for asynchronous detection result callback notification, supports HTTP and HTTPS. If the field is empty, you must actively query the result through the query interface. |
| callbackParam      | json_object | N            | Pass-through field, optional when callback exists, the service will return the content of this field along with the audio result when sending the callback request |

*The content of data is as follows:*

| **Parameter Name** | **Type**    | **Required** | **Description**                                                                                                                                            |
| :----------------- | :---------- | :----------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| url                | string      | N            | URL address of the audio to be recognized, at least one of url and content must be provided                                                                 |
| lang               | string      | N            | Optional values are as follows (default is zh):<br/>zh: Chinese<br/>en: English<br/>ar: Arabic<br/>hi: Hindi<br/>es: Spanish<br/>fr: French<br/>ru: Russian<br/>pt: Portuguese<br/>id: Indonesian<br/>de: German<br/>ja: Japanese<br/>tr: Turkish<br/>vi: Vietnamese<br/>it: Italian<br/>th: Thai<br/>tl: Filipino<br/>ko: Korean<br/>ms: Malay<br/>[Supported languages for clusters see Request URL Supported Languages](#language), other than Chinese, other language types are internationalized |
| content            | string      | N            | Base64 encoded data of the audio to be recognized (up to 15M, only supports pcm, wav, mp3), pcm format data must use 16-bit little-endian encoding. It is recommended to use pcm, wav format for transmission. At least one of url and content must be provided, and only content is supported when both exist |
| formatInfo         | json_object | N            | Must exist when content exists, local voice file format information. The specific format in json is described below<br/>(In addition, if there are other specific audio format requirements, after communicating with Shumei, this field can be passed to indicate special decoding logic) |
| audioName          | string      | N            | Audio file name                                                                                                                                              |
| tokenId            | string      | N            | User account identifier                                                                                                                                      |
| channel            | string      | N            | Business scene name, see channel configuration table                                                                                                         |
| returnAllText      | bool        | N            | When the value is true, all audio segment recognition results are returned (one audio segment every 10 seconds); when the value is false, the recognition results of risk segments (riskLevel is REJECT or REVIEW) are returned. The default is false. |
| audioDetectStep    | int         | N            | Interval review step length, the value range is 1-36 integers, taking 1 means skipping the review of one 10S audio segment, taking 2 means skipping two, and so on. When this function is not used, the entire audio content is reviewed. When this function is enabled, it is recommended to enable returnAllText and use the ASR recognition results of each segment. |
| receiveTokenId     | string      | N            | TokenId of the message receiver in the private chat scene, a string of letters, numbers, underscores, and hyphens with a length of less than or equal to 64 characters |
| nickname           | string      | N            | User nickname                                                                                                                                               |
| timestamp          | int         | N            | Timestamp (in milliseconds)                                                                                                                                  |
| room               | string      | N            | Room number                                                                                                                                                  |
| deviceId           | string      | N            | Shumei device identifier                                                                                                                                     |
| dataId             | string      | N            | Data identifier                                                                                                                                              |
| ip                 | string      | N            | Public IP address of the user sending the audio, supports IPV4 or IPV6                                                                                       |
| level              | int         | N            | Optional values:<br/>`0`: Lowest level user, typically such as newly registered, completely inactive or level 0 users, etc.;<br/>`1`: Lower level user, typically such as low active or low level users, etc.;<br/>`2`: Medium level user, typically such as users with certain activity or medium level, etc.;<br/>`3`: Higher level user, typically such as high active or high level users, etc.;<br/>`4`: Highest level user, typically such as paid users, VIP users, etc. |
| gender             | string      | N            | User gender, optional values:<br/>male: male <br/>female: female |

*The content of formatInfo is as follows:*

| **Parameter Name** | **Type** | **Required** | **Description**                                                                 |
| :----------------- | :------- | :----------- | :------------------------------------------------------------------------------ |
| format             | string   | Y            | Voice data format, only supports (values) pcm, wav, mp3, opus-gin, strictly lowercase. |
| rate               | int      | N            | Voice data sampling rate, must exist when the voice data format is pcm, range limit 8000-32000. |
| track              | int      | N            | Number of voice data channels, must exist when the voice data format is pcm, supports mono (value 1) or stereo (value 2). |

### Return Parameters

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name** | **Type** | **Required** | **Description**                                         |
| :----------------- | :------- | :----------- | :------------------------------------------------------ |
| code               | int      | Y            | See the interface response code list                    |
| message            | string   | Y            | Description of the return code                          |
| requestId          | string   | Y            | Unique identifier for the request                       |
| btId               | string   | Y            | Unique identifier for the uploaded audio, corresponding to the btId field in the request parameters |

## Active Query Recognition Results

### Interface Description

This interface is used to query audio recognition results, supporting up to 10qps.

### Request URL

Shanghai Cluster:

http://api-audio-sh.fengkongcloud.com/v2/saas/anti_fraud/query_audio

Silicon Valley Cluster:

http://api-audio-gg.fengkongcloud.com/v2/saas/anti_fraud/query_audio

Singapore Cluster:

http://api-audio-xjp.fengkongcloud.com/v2/saas/anti_fraud/query_audio

### Character Encoding Format

Both request and return results use UTF-8 character set for encoding

### Request Method

POST

### Suggested Timeout Duration

1s

### Request Parameters

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name** | **Type** | **Required** | **Description**                           |
| :----------------- | :------- | :----------- | :---------------------------------------- |
| accessKey          | string   | Y            | Service key provided by Shumei when the account service is activated |
| btId               | string   | Y            | Unique identifier for the audio, used to query recognition results     |

### Return Parameters

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name**   | **Type**    | **Required** | **Description**                                                     |
| :------------------- | :---------- | :----------- | :------------------------------------------------------------------ |
| code                 | int         | Y            | See the interface response code list                                |
| message              | string      | Y            | Description of the return code                                      |
| requestId            | string      | Y            | Unique identifier for the request                                   |
| btId                 | string      | Y            | Unique identifier for the audio                                     |
| audioText            | string      | Y            | Transcription result of the entire audio                            |
| audioTime            | int         | N            | Duration of the entire audio, in seconds, exists when code is 1100   |
| labels               | string      | N            | Audio recognition result labels                                     |
| riskLevel            | string      | N            | <p>Recognition result, possible values:</p><p>PASS: Normal content<br/>REVIEW: Suspected violation content<br/>REJECT: Violation content</p> |
| detail               | json_array  | N            | Risk details                                                        |
| gender               | json_object | N            | Gender tags and probability values                                  |
| isSing               | int         | N            | <p>Indicates whether the audio file is singing, 0 means no singing, 1 means singing.</p><p>Only returned when type includes SING.</p> |
| language             | json_array  | N            | Language tags and probability values list                           |
| tags                 | json_array  | N            | Timbre tags and probability values list                             |
| businessLabels       | json_array  | N            | Business tags return (currently only supports MINOR, returns tag content when the strategy is hit, otherwise empty, historical legacy field, not recommended for use) |
| auxInfo              | json_object | N            | Auxiliary information                                               |
| tokenProfileLabels   | json_array  | N            | Account attribute tags, only returned when the function is enabled  |
| tokenRiskLabels      | json_array  | N            | Account risk tags, only returned when the function is enabled       |

*The specific parameters of each item in the detail array are as follows:*

| **Parameter Name**     | **Type**   | **Required** | **Description**                                                     |
| :--------------------- | :--------- | :----------- | :------------------------------------------------------------------ |
| requestId              | string     | Y            | Unique identifier for the risk audio segment request                |
| audioStarttime         | string     | Y            | Start time of the risk audio segment in the audio, in seconds       |
| audioEndtime           | string     | Y            | End time of the risk audio segment in the audio, in seconds         |
| audioModel             | string     | Y            | Rule identifier, the highest priority rule identifier hit (deprecated field, not recommended for use) |
| audioUrl               | string     | Y            | Address of the risk audio segment, in MP3 format                    |
| audioText              | string     | Y            | Transcription content of the audio segment                          |
| riskLevel              | string     | Y            | <p>Recognition result, possible values: <br/>REJECT: Violation content</p><p>REVIEW: Suspected violation content</p><p>PASS: Normal content</p> |
| businessLabels         | json_array | N            | Business tags return [see businessLabels parameter](#businessLabels) |
| riskType               | int        | N            | <p>Identifies the risk type, possible values:<br/>Risk type, not returned when silent, possible values:<br/>0: Normal</p><p>100: Political/National anthem</p><p>110: Violent and terrorist</p><p>200: Erotic</p><p>210: Abusive</p><p>250: Moan</p><p>260: Voiceprint of the first leader</p><p>270: Voice attributes</p><p>280: Prohibited songs</p><p>300: Advertisement</p><p>400: Watering</p><p>500: Meaningless</p><p>520: Minor</p><p>600: Prohibited</p><p>700: Other</p><p>720: Black account</p><p>730: Black IP</p><p>800: High-risk account</p><p>900: Custom</p> |
| audioMatchedItem       | string     | N            | Sensitive words that may appear in the audio                        |
| matchedList            | string     | N            | Name of the list where the sensitive words are hit                  |
| matchedDetail          | string     | N            | Details of all lists hit [see matchedDetail](#matchedDetail1)        |
| description            | string     | Y            | Description of the risk reason for the audio segment, only for human understanding of the risk reason, programs should not rely on the value of this parameter for logic processing |

*The structure of the gender parameter is as follows:*

| **Parameter Name** | **Type** | **Required** | **Description**                                              |
| :----------------- | :------- | :----------- | :----------------------------------------------------------- |
| label              | string   | Y            | <p>Gender tag name, possible values:</p><p>Male</p><p>Female</p> |
| confidence         | int      | Y            | Corresponding gender probability, value 0-100, the higher the value, the higher the probability. |

*The specific parameters of each item in the language array are as follows:*

| **Parameter Name** | **Type** | **Required** | **Description**                                                                     |
| :----------------- | :------- | :----------- | :---------------------------------------------------------------------------------- |
| label              | int      | Y            | <p>Language recognition category identifier, possible values:</p><p>0: Mandarin</p><p>1: English</p><p>2: Cantonese</p><p>3: Tibetan</p><p>4: Uyghur</p><p>5: Mongolian</p><p>6: Korean</p><p>-1: Other languages</p> |
| confidence         | int      | Y            | Corresponding language tag probability, value 0-100, the higher the value, the higher the probability. |

*The specific parameters of each item in the tags array are as follows:*

| **Parameter Name** | **Type** | **Required** | **Description**                                                                                                                                                              |
| :----------------- | :------- | :----------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| label              | string   | Y            | <p>Timbre tag category, possible values:</p><p>Uncle</p><p>Young</p><p>Shota</p><p>Old</p><p>Queen</p><p>Royal sister</p><p>Girl</p><p>Loli</p><p>Aunt</p><p>Male</p><p>Female</p><p>No voice</p> |
| confidence         | int      | Y            | Corresponding timbre tag probability, value 0-100, the higher the value, the higher the probability.                                                                                                             |

*The specific parameters of each item in the auxInfo array are as follows:*

| **Parameter Name** | **Type** | **Required** | **Description**                                 |
| :----------------- | :------- | :----------- | :---------------------------------------------- |
| errorCode          | int      | Y            |<p>Status code</p><p>2003: Audio download failed</p><p>2007: No valid data</p>|

*The specific parameters of each item in the tokenProfileLabels and tokenRiskLabels arrays are as follows:*

| **Parameter Name**    | **Type** | **Required** | **Description**                                 |
|:----------------------|:---------|:-------------|:------------------------------------------------|
| label1                | string   | N            | First-level tag                                 |
| label2                | string   | N            | Second-level tag                                |
| label3                | string   | N            | Third-level tag                                 |
| description           | string   | N            | Account tag description, only for human understanding of the risk reason, programs should not rely on the value of this parameter for logic processing |
| timestamp             | int      | N           