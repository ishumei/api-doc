# Intelligent Audio Moderation API Documentation

## Synchronous Interface

### Request URL

| Cluster | URL                                                     | Supported Languages        |
| ---- | ------------------------------------------------------- | -------------------- |
| Shanghai | `http://api-audio-sh.fengkongcloud.com/audiomessage/v4` | Chinese |

### Character Encoding

`UTF-8`

### Request Method

`POST`

### Recommended Timeout Duration

10s

### Audio Format Limitation

`WAV`、`MP3`、`AAC`、`AMR`、`3GP`、`M4A`、`WMA`、`OGG`、`APE`、`FLAC`、`ALAC`、`WAVPACK`、`SILK_V3`

### Audio Duration Limitation

Synchronous requests for a single audio should not exceed 60 seconds, otherwise an error will be reported. If you need to review audio data exceeding 60 seconds, please use the asynchronous interface.

### Request Body Limitation

18M

### Request Parameters

Place in the HTTP Body in JSON format. Specific parameters are as follows:

| **Request Parameter Name** | **Type**    | **Parameter Description**         | **Input Description** | **Specification**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| :------------- | :---------- | :------------------- | :----------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| accessKey      | string      | Company Secret Key   | Required    | Provided by NextData                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| appId          | string      | Application ID       | Required    | Used to differentiate applications, you need to contact NextData services to activate. Please use the value provided by NextData exclusively.                                                                                                                                                                                                                                                                                                                                                                                                         |
| eventId        | string      | Event ID             | Required    | Used to differentiate scenario data. You need to contact NextData services to activate. Please use the value provided by NextData exclusively.                                                                                                                                                                                                                                                                                                                                                                                                      |
| type           | string      | Risk Type to Detect  | Required    | <p>AUDIOPOLITICAL: Voiceprint recognition of the top leader</p><p>POLITY: Political recognition</p><p>EROTIC: Erotic content recognition</p><p>ADVERT: Advertisement recognition</p><p>ANTHEN: National anthem recognition</p><p>MOAN: Moaning recognition</p><p>DIRTY: Abusive language recognition</p><p>GENDER: Gender recognition</p><p>TIMBRE: Timbre recognition</p><p>SING: Singing recognition</p><p>LANGUAGE: Language type recognition</p><p>BANEDAUDIO: Banned song recognition</p><p>VOICE: Human voice attributes</p><p>AUDIOSCENE: Sound scene recognition</p><p>MINOR: Minor recognition</p><p>If recognizing timbre, singing, and language type, GENDER is required</p><p>For combined recognitions, connect with underscores, e.g., POLITY_EROTIC_MOAN for political, erotic, and moaning recognition</p><p>Recommended input:<br/>POLITY_EROTIC_MOAN_ADVERT</p> |
| contentType    | string      | Format of the Audio Content to Recognize | Required    | <p>Available options:</p><p>URL: The content is an audio URL;</p><p>RAW: The content is the base64 encoded data of the audio</p>                                                                                                                                                                                                                                                                                                                                                                                                                              |
| content        | string      | Audio Content to Recognize | Required    | <p>Can be a URL or base64 encoded data.</p><p>For base64 encoded data, the limit is 15M, only supports pcm, wav, mp3 formats, and pcm format data must be encoded in 16-bit little-endian. pcm and wav formats are recommended for transmission.</p>                                                                                                                                                                                                                                                                                                             |
| data           | json object | Information related to this request | Required    | Maximum 1MB, [see data parameter details](#data)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| acceptLang | string | Returns the language type of interface information | Required | Optional language type of returned information:<br/>`zh`: Chinese<br/>`en`: English<br/>The default is `zh`. If you need to return results in English, this field is required |
| btId           | string      | Unique Identifier for the Audio File | Required    | A unique identifier for this audio file to easily match the callback results, up to 128 bits, must not be repeated.|
<span id="data">data</span> content is as follows:

| **Request Parameter Name** | **Type** | **Parameter Description**                  | **Input Description** | **Specification**                                                     |
| :------------- | :------- | :---------------------------- | :----------- | :----------------------------------------------------------- |
| tokenId        | string   | User account                  | Optional     | Used for user behavior analysis, it is recommended to input the user UID |
| receiveTokenId | string   | TokenId of the message recipient in private chat | Optional | Comprised of numbers, letters, underscores, and hyphens with a length less than or equal to 64 characters |
| formatInfo     | string   | Audio data format             | Optional     | Must exist when the audio content format is RAW, available options: pcm, wav, mp3, opus-gin |
| rate           | int      | Audio data sample rate        | Optional     | Must exist when the audio data format is pcm, range limit from 8000 to 32000 |
| track          | int      | Number of audio data channels | Optional     | <p>Must exist when the audio data format is pcm, available options:</p><p>1: Mono</p><p>2: Stereo</p> |
| returnAllText  | int      | Level of the audio segment to return | Optional | Available options (default is `0`):<br/> `0`: Return risk segment identification results<br/> `1`: Return all segment identification results<br/> This parameter is only used to control the return of segment identification results and does not affect the return of overall identification results.<br/> When choosing "Return all segment identification results", the segment identification results include riskLevel as PASS, REVIEW, and REJECT;<br/> When choosing "Return risk segment identification results", the segment identification results only include riskLevel as REVIEW and REJECT;<br/> Segment identification results correspond to the audioDetail field in the response. |


### Return Parameters

Place in the HTTP Body in JSON format. Specific parameters are as follows:

| **Return Result Parameter Name** | **Parameter Type** | **Parameter Description**          | **Mandatory Return** | **Specification**                                                                                           |
| ------------------------------- | ------------------ | ---------------------------------- | -------------------- | ----------------------------------------------------------------------------------------------------------- |
| requestId                       | string             | Unique identifier for this request | Yes                  |                                                                                                           |
| code                            | int                | Request return code                | Yes                  | 1100: Success<br>1901: QPS exceeded<br>1902: Invalid parameter<br>1903: Service failure<br>9101: No permission to operate |
| message                         | string             | Description corresponding to the return code | Yes |                                                                                                           |
| detail                          | json               | Detailed return data for the request | No              | Must return under the 1100 situation |
### Detail Content:

| **Parameter Name** | **Type**      | **Parameter Description**          | **Mandatory Return** | **Specification**                                                                                                                                            |
| ------------------ | ------------- | ---------------------------------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| riskLevel          | string        | Recommended action for the current event | Yes                  | <p>Possible return values:<br/>PASS: Approved</p><p>REVIEW: Review</p><p>REJECT: Reject</p><p>Suggestion: Don't use the result directly during the initial integration, adjust the interception scale to meet expectations before using.</p> |
| audioText          | string        | Translated text result of the entire audio segment | Yes              |                                                                                                                                                              |
| audioTime          | int           | Duration of the entire audio       | Yes                  | In seconds                                                                                                                                                   |
| audioDetail        | json_array    | Audio segment information          | Yes                  | Callback audio segment information, [see audioDetail parameter details](#audioDetail)                                                                        |
| auxInfo            | json_object   | Auxiliary information              | No                   |                                                                                                                                                              |


<span id="audioDetail">audioDetail</span> detailed content is as follows:

| **Parameter Name** | **Type**      | **Parameter Description**               | **Mandatory Return** | **Specification**                                                                                                                                                                                                                                         |
| ------------------ | ------------- | --------------------------------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| requestId          | string        | Unique identifier for the audio segment request | Yes                  |                                                                                                                                                                                                                                                         |
| audioStarttime     | float         | Start time of the audio segment         | Yes                  | Distance from the start of the audio, in seconds                                                                                                                                                                                                         |
| audioEndtime       | float         | End time of the audio segment           | Yes                  | Distance from the start of the audio, in seconds                                                                                                                                                                                                         |
| audioUrl           | string        | Audio segment link                      | Yes                  | In mp3 format                                                                                                                                                                                                                                            |
| riskLevel          | string        | Identification result for the audio segment | Yes                  | <p>Possible return values:<br/>PASS: Approved</p><p>REVIEW: Review</p><p>REJECT: Reject</p>                                                                                                                                                              |
| riskLabel1         | string        | Primary risk label                      | Yes                  |                                                                                                                                                                                                                                                         |
| riskLabel2         | string        | Secondary risk label                    | Yes                  |                                                                                                                                                                                                                                                         |
| riskLabel3         | string        | Tertiary risk label                     | Yes                  |                                                                                                                                                                                                                                                         |
| riskDescription    | string        | Reason for the risk                     | Yes                  | Only for reference when understanding the reason for the risk, do not rely on the value of this parameter for logic processing                                                                                                                           |
| riskDetail         | json_object   | Risk details                            | No                   | [See riskDetail parameter details](#riskDetail)                                                                                                                                                                                                         |


<span id="riskDetail">riskDetail</span> detailed content is as follows:

| **Parameter Name** | **Type**      | **Parameter Description**                     | **Mandatory Return** | **Specification**                                                                                                                  |
| ------------------ | ------------- | ---------------------------------------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| audioText          | string        | Translated text result of the audio           | No                   |                                                                                                                                                                                             |
| riskSource         | int           | Risk source                                    | No                   | Possible values:<br/>1000: No risk<br/>1001: Text risk<br/>1003: Audio risk                                                                                                                 |
| matchedLists       | json_array    | Information on custom lists that were matched | No                   | Returned when a custom list is matched, [see matchedLists parameter details](#matchedLists)                                                                                                  |
| riskSegments       | json_array    | High-risk content segments                    | No                   | Exists for functions like politics, terrorism, banned content, competitors, advertising laws, etc., [see riskSegments parameter details](#riskSegments)                                       |

In riskDetail, <span id="matchedLists">matchedLists</span> detailed content is as follows:

| **Parameter Name** | **Type**      | **Parameter Description**                     | **Mandatory Return** | **Specification**                                                                                                                  |
| ------------------ | ------------- | ---------------------------------------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name               | string        | Name of the custom list set by the client     | Yes                  |                                                                                                                                                                                             |
| words              | json_array    | Information on sensitive words that were matched in this list | Yes              | [see words parameter details](#words)                                                                                                                                                         |

In matchedLists, <span id="words">words</span> detailed content is as follows:

| **Parameter Name** | **Type**      | **Parameter Description** | **Mandatory Return** | **Specification** |
| ------------------ | ------------- | ------------------------- | -------------------- | ----------------- |
| word               | string        | Sensitive word            | Yes                  |                   |
| position           | int_array     | Position of the sensitive word | Yes              |                   |

In riskDetail, <span id="riskSegments">riskSegments</span> detailed content is as follows:

| **Parameter Name** | **Type**      | **Parameter Description**           | **Mandatory Return** | **Specification** |
| ------------------ | ------------- | ----------------------------------- | -------------------- | ----------------- |
| segment            | string        | High-risk content segment           | No                   |                   |
| position           | int_array     | Position of the high-risk content segment | No               |                   |




## Example

### Upload Request Example


```bash
curl -v 'http://api-audio-bj.fengkongcloud.com/audiomessage/v4' -d '{
    "accessKey": "*************",
    "appId": "default",
    "eventId": "default",
    "type": "TIMBRE_POLITICAL_PORN",
    "btId": "test1",
    "contentType": "URL",
    "content": "*************",
    "acceptLang": "en",
    "data": {
    		"returnAllText":1,
        "room": "general",
        "tokenId": "token-short"
        }
    }
}'
```

### Synchronous Response Example


```json
{
    "code":1100,
    "message":"Success",
    "requestId":"817c8509359500c898a762ffe93a582b",
    "btId":"1667392054643",
    "detail":{
        "audioDetail":[
            {
                "requestId":"817c8509359500c898a762ffe93a582b_a0000",
                "audioStarttime":0,
                "audioEndtime":10,
                "audioUrl":"http://voice-stream.oss-cn-hangzhou.aliyuncs.com/POST_AUDIO%2F20221102%2F817c8509359500c898a762ffe93a582b_a0000.mp3?Expires=1669984055&amp;OSSAccessKeyId=LTAI4GCEw42chQY7RgPbGxhv&amp;Signature=pG4zurQ%2F%2F9laWauXo4mFfQoHrdE%3D",
                "riskLevel":"REJECT",
                "riskLabel1":"abuse",
                "riskLabel2":"buwenmingyongyu",
                "riskLabel3":"qingdubuwenmingyongyu",
                "riskDescription":"Abuse:UncivilizedWords:UncivilizedWordsSli",
                "riskDetail":{
                    "audioText":"you fool"
                }
            }
        ],
        "audioTags":{

        },
        "audioText":"you fool",
        "audioTime":10,
        "code":1100,
        "requestParams":{
            "channel":"TEST",
            "lang":"en",
            "returnAllText":1,
            "tokenId":"test01"
        },
        "riskLevel":"REJECT"
    }
}
```


## Asynchronous Interface

### Request URL

| Cluster  | URL                                                | Supported Languages                                 |
| -------- | -------------------------------------------------- | --------------------------------------------------- |
| Shanghai | `http://api-audio-sh.fengkongcloud.com/audio/v4`  | Chinese                                             |
| Silicon Valley | `http://api-audio-gg.fengkongcloud.com/audio/v4` | Chinese, English, Arabic                            |
| Singapore    | `http://api-audio-xjp.fengkongcloud.com/audio/v4` | Chinese, English, Arabic                            |

### Character Encoding

`UTF-8`

### Request Method

`POST`

### Recommended Timeout Duration

1s

### Audio Format Limitation

`WAV`、`MP3`、`AAC`、`AMR`、`3GP`、`M4A`、`WMA`、`OGG`、`APE`、`FLAC`、`ALAC`、`WAVPACK`、`SILK_V3`

### Request Body Limitation

18M

### Request Parameters

Place in the HTTP Body in JSON format. Specific parameters are as follows:

| **Request Parameter Name** | **Type**    | **Parameter Description**                 | **Input Description**               | **Specification**                                                                                                                     |
| :------------------------- | :---------- | :---------------------------------------- | :---------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------- |
| accessKey                  | string      | Company key                               | Mandatory                          | Provided by NextData                                                                                                            |
| appId                      | string      | Application identifier                    | Mandatory                          | Used to distinguish applications, please contact NextData for activation and use the value provided exclusively by NextData |
| eventId                    | string      | Event identifier                          | Mandatory                          | Used to distinguish scenario data, please contact NextData for activation and use the value provided exclusively by NextData |
| type                       | string      | Type of risk to be detected               | Either "businesstype" or "type" must be provided | <p>AUDIOPOLITICAL: Voiceprint recognition of the top leader</p><p>POLITY: Political recognition</p><p>EROTIC: Eroticism recognition</p><p>ADVERT: Advertisement recognition</p><p>ANTHEN: National anthem recognition</p><p>MOAN: Moaning recognition</p><p>DIRTY: Insult recognition</p><p>BANEDAUDIO: Banned song</p><p>For combined recognition, connect with underscores, e.g., POLITY_EROTIC_MOAN for political, erotic, and moaning recognition</p><p>Suggested input:<br/>POLITY_EROTIC_MOAN_ADVERT</p> |
| businessType               | string      | Type of business label to be detected     | Either "businesstype" or "type" must be provided | Optional values:<br/>SING: Singing recognition<br/>LANGUAGE: Language type recognition<br/>GENDER: Gender recognition<br/>TIMBRE: Timbre recognition<br/>VOICE: Human voice attributes<br/>MINOR: Minor recognition<br/>AUDIOSCENE: Sound scene<br/>AGE: Age recognition<br/>For timbre, singing, and language type recognition, GENDER must be provided<br/>Either "type" or "businessType" must be provided |
| contentType                | string      | Format of the audio content to be recognized | Mandatory                          | <p>Optional values:</p><p>URL: Content is an audio URL;</p><p>RAW: Content is the base64 encoded data of the audio</p>                 |
| content                    | string      | Audio content to be recognized            | Mandatory                          | <p>Can be a URL or base64 encoded data.</p><p>For base64 encoded data, the limit is 15M, and only supports pcm, wav, mp3 formats. For pcm format data, it must be encoded in 16-bit little-endian. It's recommended to use pcm or wav for transmission</p> |
| data                       | json object | Information related to this request       | Mandatory                          | Up to 1MB, [see data parameter details](#data)                                                                                         |
| btId                       | string      | Unique identifier for the audio file      | Mandatory                          | Unique identifier for this audio file, useful for correlating callback results, will be truncated if exceeding 128 characters, must be unique |
| callback                   | string      | Callback HTTP interface                   | Optional                           | When this field is not empty, the service will notify the user of the review results according to this field                           |


<span id="data">data</span> content is as follows:

| **Request Parameter Name** | **Type** | **Parameter Description**                | **Input Description**       | **Specification**                                                                                                                                                                                                                                      |
| :-------------------------| :------- | :--------------------------------------  | :------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| tokenId                   | string   | User account                             | Optional                   | Used for user behavior analysis, it's recommended to input the user UID                                                                                                                                                                                 |
| formatInfo                | string   | Audio data format                        | Optional                   | Must exist when the audio content format is RAW. Optional values: pcm, wav, mp3                                                                                                                                                                         |
| rate                      | int      | Audio data sample rate                   | Optional                   | Must exist when the audio data format is pcm, the range is limited from 8000 to 32000.                                                                                                                                                                  |
| track                     | int      | Number of audio data channels            | Optional                   | <p>Must exist when the audio data format is pcm. Optional values:</p><p>1: Mono</p><p>2: Stereo</p>                                                                                                                                                     |
| returnAllText             | int      | Level of returned audio segment          | Optional                   | Optional values (default is `0`):<br/>`0`: Return risk segment recognition result<br/>`1`: Return all segment recognition results<br/>This parameter only controls the return of segment recognition results and does not affect the return of the overall recognition result.<br/>When choosing "Return all segment recognition results", the segment recognition result includes riskLevel values of PASS, REVIEW, and REJECT;<br/>When choosing "Return risk segment recognition results", the segment recognition result only includes riskLevel values of REVIEW and REJECT;<br/>Segment recognition results correspond to the audioDetail field in the callback or query response. |
| audioDetectStep           | int      | Interval review step length              | Optional                   | Interval review step length, the value range is an integer from 1 to 36. Taking 1 means skipping the review of a 10-second audio segment, taking 2 means skipping two, and so on. When not using this function, all audio content is reviewed. When using this function, it is recommended to turn on returnAllText and use the ASR recognition result of each segment. |
| receiveTokenId            | string   | tokenId of the message recipient in private chat scenarios | Optional | Composed of numbers, letters, underscores, and hyphens with a length less than or equal to 64 characters                                                                                                                                                 |
| lang                      | string   | Audio language type                      | Optional                   | Optional values (default is "zh"):<br/>`zh`: Chinese<br/>`en`: English<br/>`ar`: Arabic<br/>`hi`: Hindi<br/>`es`: Spanish<br/>`fr`: French<br/>`ru`: Russian<br/>`pt`: Portuguese<br/>`id`: Indonesian<br/>`de`: German<br/>`ja`: Japanese<br/>`tr`: Turkish<br/>`vi`: Vietnamese<br/>`it`: Italian<br/>`th`: Thai<br/>`tl`: Filipino<br/>`ko`: Korean<br/>`ms`: Malay           |
| deviceId                  | string   | NextData device fingerprint identifier | Optional                   | Unique identifier generated by NextData device fingerprint                                                                                                                                                                                       |
| ip                        | string   | IPv4 address                             | Optional                   | Public IPv4 address of the user sending this audio                                                                                                                                                                                                     |


### Return Parameters

Place in the HTTP Body in JSON format. Specific parameters are as follows:

| **Parameter Name**    | **Type**   | **Parameter Description**           | **Is it mandatory?** | **Specification**                                                                                               |
| :-------------------- | :--------- | :---------------------------------- | :------------------- | :--------------------------------------------------------------------------------------------------------------- |
| requestId             | string     | Unique identifier for this request  | Yes                  |                                                                                                                  |
| code                  | int        | Request return code                 | Yes                  | <p>1100: Success</p><p>1901: QPS exceeded</p><p>1902: Invalid parameter</p><p>1903: Service failure</p><p>9101: Unauthorized operation</p> |
| message               | string     | Request return description, corresponding to the request return code | Yes |                                                                                                                  |


## Proactively Query Results

### Request URL

| Cluster    | URL                                                         | Supported Languages                                  |
| ---------- | ----------------------------------------------------------- | ---------------------------------------------------- |
| Shanghai   | `http://api-audio-sh.fengkongcloud.com/query_audio/v4`     | Chinese                                               |
| Silicon Valley | `http://api-audio-gg.fengkongcloud.com/query_audio/v4`    | Chinese, English, Arabic                              |
| Singapore  | `http://api-audio-xjp.fengkongcloud.com/query_audio/v4`    | Chinese, English, Arabic                              |


### Character Encoding

`UTF-8`

### Request Method

`POST`

### Recommended Timeout Duration

1s

### Request Parameters

Place in the HTTP Body in JSON format. Specific parameters are as follows:

| **Request Parameter Name** | **Type** | **Parameter Description**   | **Input Description** | **Specification**                                         |
| :------------------------ | :------- | :------------------------- | :-------------------- | :-------------------------------------------------------- |
| accessKey                 | string   | Company key                | Mandatory             | Provided by NextData                               |
| btId                      | string   | Unique identifier for the audio file | Mandatory             | Unique identifier for this audio file, used to query recognition results |

### Return Parameters

Place in the HTTP Body in JSON format. Specific parameters are as follows:

| **Parameter Name** | **Type**   | **Parameter Description**                 | **Is it mandatory?** | **Specification**                                                                                                                                                                     |
| :----------------- | :--------- | :---------------------------------------- | :------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| requestId          | string     | Unique identifier for this request        | Yes                  |                                                                                                                                                                                       |
| btId               | string     | Unique identifier for the audio           | Yes                  |                                                                                                                                                                                       |
| code               | int        | Request return code                       | Yes                  | <p>1100: Success</p><p>1101: Processing</p><p>1901: QPS exceeded</p><p>1902: Invalid parameter</p><p>1903: Decoding failure</p><p>9100: Insufficient balance</p><p>9101: Unauthorized operation</p><p>Except for message and requestId, other fields exist only when the code is 1100.</p> |
| message            | string     | Request return description corresponding to the return code | Yes |                                                                                                                                                                                       |
| riskLevel          | string     | Recommended disposition for the current event | Yes                  | <p>Possible return values:</p><p>PASS: Approved</p><p>REVIEW: Under Review</p><p>REJECT: Rejected</p><p>Suggestion: Don't use the results directly during initial integration. Adjust the interception scale and use it once it meets expectations.</p> |
| audioText          | string     | Translated text result of the entire audio | Yes                  |                                                                                                                                                                                       |
| audioTime          | int        | Duration of the entire audio               | Yes                  | In seconds                                                                                                                                                                            |
| audioDetail        | json_array | Audio segment information                 | Yes                  | Callback audio segment information, [see audioDetail parameter](#audioDetail)                                                                                                         |
| audioTags          | json_object | Audio tags                               | No                   | Returns tags such as gender, timbre, whether singing, etc.                                                                                                                           |
| auxInfo            | json_object | Auxiliary information                     | No                   |                                                                                                                                                                                       |


| **Parameter Name** | **Type**    | **Parameter Description**     | **Is it mandatory?** | **Specification**                                                                                                     |
| :----------------- | :---------- | :--------------------------- | :------------------- | :-------------------------------------------------------------------------------------------------------------------- |
| audioStarttime     | float       | Audio segment start time     | Yes                  | Relative distance from the beginning of the audio, in seconds                                                          |
| audioEndtime       | float       | Audio segment end time       | Yes                  | Relative distance from the beginning of the audio, in seconds                                                          |
| audioUrl           | string      | Audio segment link           | Yes                  | mp3 format                                                                                                            |
| riskLevel          | string      | Audio segment recognition result | Yes                  | <p>Possible return values:</p><p>PASS: Approved</p><p>REVIEW: Under Review</p><p>REJECT: Rejected</p>                 |
| businessLabels     | json_array  | Business label returns       | No                   | All business labels, [see businessLabels parameter](#businessLabels2)                                                 |
| allLabels          | json_array  | Risk label returns           | No                   | All risk labels, [see allLabels parameter](#allLabels2)                                                               |
| riskLabel1         | string      | Primary risk label           | Yes                  |                                                                                                                       |
| riskLabel2         | string      | Secondary risk label         | Yes                  |                                                                                                                       |
| riskLabel3         | string      | Tertiary risk label          | Yes                  |                                                                                                                       |
| riskDescription    | string      | Reason for risk              | Yes                  | For reference when understanding the reason for the risk, do not rely on this parameter's value for logical processing |
| riskDetail         | json_object | Risk details                 | No                   | [See riskDetail parameter](#riskDetail)                                                                               |



## RiskDetail Content

### **riskDetail** Detailed Content

| **Parameter Name** | **Type**   | **Parameter Description**             | **Is it mandatory?** | **Specification**                                                                                                                                 |
| :----------------- | :--------- | :----------------------------------- | :------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------ |
| audioText          | string     | Transcription result of the audio    | No                   |                                                                                                                                                   |
| matchedLists       | json_array | Hit customer-defined list information | No                   | Returned when a customer-defined list is hit, [see matchedLists parameter](#matchedLists)                                                         |
| riskSegments       | json_array | High-risk content segments           | No                   | Exists when features like politics, terrorism, prohibited items, competitors, advertising laws are involved, [see riskSegments parameter](#riskSegments) |

### Within **riskDetail**, **matchedLists** Detailed Content

| **Parameter Name** | **Type**   | **Parameter Description**                 | **Is it mandatory?** | **Specification**                      |
| :----------------- | :--------- | :--------------------------------------- | :------------------- | :------------------------------------ |
| name               | string     | Customer-defined list name               | Yes                  |                                       |
| words              | json_array | Sensitive words hit within this list     | Yes                  | [see words parameter](#words)         |

### Within **matchedLists**, **words** Detailed Content

| **Parameter Name** | **Type**  | **Parameter Description**   | **Is it mandatory?** | **Specification** |
| :----------------- | :-------- | :------------------------- | :------------------- | :--------------- |
| word               | string    | Sensitive word             | Yes                  |                  |
| position           | int_array | Position of the sensitive word | Yes                  |                  |

### Within **riskDetail**, **riskSegments** Detailed Content

| **Parameter Name** | **Type**  | **Parameter Description**           | **Is it mandatory?** | **Specification** |
| :----------------- | :-------- | :---------------------------------- | :------------------- | :--------------- |
| segment            | string    | High-risk content segment           | No                   |                  |
| position           | int_array | Position of the high-risk content   | No                   |                  |


## AudioTags Content

### **audioTags** Detailed Content

| **Parameter Name** | **Type**    | **Parameter Description** | **Is it mandatory?** | **Specification**                                                                                           |
| :----------------- | :---------- | :----------------------- | :------------------- | :--------------------------------------------------------------------------------------------------------- |
| gender             | json_object | Gender label             | No                   | Returned when `type` includes GENDER                                                                         |
| timbre             | json_array  | Timbre label             | No                   | Returned when `type` includes TIMBRE                                                                         |
| song               | int         | Singing label            | No                   | Returned when `type` includes SING<br/>Possible values:<br/>0: No singing<br/>1: Singing                     |
| language           | json_object | Language detection       | No                   | Returned when `type` includes LANGUAGE                                                                       |

### Within **audioTags**, **gender** Detailed Content

| **Parameter Name** | **Type** | **Parameter Description**  | **Is it mandatory?** | **Specification**                                                    |
| :----------------- | :------- | :------------------------ | :------------------- | :------------------------------------------------------------------- |
| label              | string   | Gender label name         | Yes                  | Possible values:<br/>Male<br/>Female                                 |
| probability        | int      | Corresponding gender probability | Yes          | Value range 0-100, the higher the value, the higher the probability  |

### Within **audioTags**, **timbre** Detailed Content

| **Parameter Name** | **Type** | **Parameter Description**       | **Is it mandatory?** | **Specification**                                                                                                               |
| :----------------- | :------- | :----------------------------- | :------------------- | :------------------------------------------------------------------------------------------------------------------------------ |
| label              | string   | Timbre label category          | Yes                  | Possible values:<br/>Uncle's voice, Youthful voice, Shota's voice, Elderly voice, Queen's voice, Older sister's voice, Girl's voice, Loli's voice, Aunt's voice, No human voice |
| probability        | int      | Corresponding timbre label probability | Yes     | Value range 0-100, the higher the value, the higher the probability                                                           |

### Within **audioTags**, **language** Detailed Content

| **Parameter Name** | **Type** | **Parameter Description**      | **Is it mandatory?** | **Specification**                                                                                    |
| :----------------- | :------- | :---------------------------- | :------------------- | :------------------------------------------------------------------------------------------------- |
| label              | int      | Language detection category ID | Yes                  | Possible values:<br/>0: Mandarin<br/>1: English<br/>2: Cantonese<br/>3: Tibetan<br/>4: Uighur<br/>5: Mongolian<br/>6: Korean<br/>-1: Other languages |
| probability        | int      | Corresponding language label probability | Yes     | Value range 0-100, the higher the value, the higher the probability                                 |

## AuxInfo Content

### **auxInfo** Detailed Content

| **Parameter Name** | **Type**   | **Parameter Description** | **Is it mandatory?** | **Specification**                                                                                   |
| :----------------- | :--------- | :----------------------- | :------------------- | :------------------------------------------------------------------------------------------------- |
| errorCode          | int        | Status code              | Yes                  | Status codes<br/>2003: Audio download failed<br/>2007: No valid data                                 |


## Asynchronous Callback Result

If users require the server to actively callback the audio detection results, the `callback` parameter needs to be passed in the `Request Parameters`. Once the server review is completed, it will actively callback the recognition results to the specified address.

### Supported Protocols

- HTTP 
- HTTPS

### Character Encoding Format

- Requests use UTF-8 character encoding.

### Request Method

- POST

### Recommended Timeout Duration

- 5s

### Push Strategy

When users receive the push result and return an HTTP status code of 200, it indicates a successful push. Otherwise, the system will attempt up to 12 pushes. The first failure will wait for 5 seconds, the second failure will wait for 10 seconds... gradually increasing up to 60 seconds.

### Callback Parameters


Place in the HTTP Body in JSON format. Specific parameters are as follows:

## Callback Parameters

| **Parameter Name**    | **Type**    | **Parameter Description**                                | **Mandatory** | **Specification**                                                                                                                                                                                             |
| :---------------------| :---------- | :------------------------------------------------------ | :------------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| requestId             | string      | Unique identifier for this request                       | Yes           |                                                                                                                                                                                                         |
| btId                  | string      | Unique identifier for the audio                          | Yes           |                                                                                                                                                                                                         |
| code                  | int         | Request return code                                      | Yes           | <p>1100: Success</p><p>1901: QPS exceeded</p><p>1902: Invalid parameters</p><p>1903: Service failure</p><p>9101: No permission for operation</p>Only fields apart from message and requestId exist when the code is 1100. |
| message               | string      | Request return description corresponding to the return code | Yes           |                                                                                                                                                                                                         |
| riskLevel             | string      | Recommendation for current event's handling              | Yes           | <p>Possible return values:</p><p>PASS: Approve</p><p>REVIEW: Review</p><p>REJECT: Deny</p>Recommendation: Do not use the result directly during the initial integration. Adjust the interception scale, and use once the desired results are achieved. |
| audioText             | string      | Transcription result of the entire audio                 | Yes           |                                                                                                                                                                                                         |
| audioTime             | int         | Duration of the entire audio                             | Yes           | Measured in seconds                                                                                                                                                                                     |
| audioDetail           | json_array  | Audio segment information                                | Yes           | Callback audio segment information, see [audioDetail parameter](#audioDetail2)                                                                                                                          |
| audioTags             | json_object | Audio tags                                               | No            | Returns tags such as gender, timbre, and whether singing                                                                                                                                                |
| requestParams         | json_object | Passthrough fields                                       | Yes           | Returns all fields under data                                                                                                                                                                           |
| businessLabels        | json_array  | Business label return                                    | No            | Returns business label content (currently only supports MINOR, returns label content if strategy is hit, otherwise empty, old field not recommended for use)                                           |
| auxInfo               | json_object | Auxiliary information                                    | No            |                                                                                                                                                                                                         |
| tokenProfileLabels    | json_array  | Account attribute labels                                 | No            | Only returned when the feature is enabled                                                                                                                                                              |
| tokenRiskLabels       | json_array  | Account risk labels                                      | No            | Only returned when the feature is enabled                                                                                                                                                              |


## Detailed content of <span id="audioDetail2">audioDetail</span>

| **Parameter Name** | **Type**    | **Parameter Description**                          | **Mandatory** | **Specification**                                                                                                                                                                 |
| :----------------- | :---------- | :------------------------------------------------- | :------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| requestId          | string      | Unique identifier for the audio segment request    | Yes           |                                                                                                                                                                                 |
| audioStarttime     | float       | Audio segment start time                           | Yes           | Time distance relative to the beginning of the audio, measured in seconds                                                                                                       |
| audioEndtime       | float       | Audio segment end time                             | Yes           | Time distance relative to the beginning of the audio, measured in seconds                                                                                                       |
| audioUrl           | string      | Audio segment link                                 | Yes           | In mp3 format                                                                                                                                                                   |
| riskLevel          | string      | Audio segment identification result                | Yes           | <p>Possible return values:</p><p>PASS: Approve</p><p>REVIEW: Review</p><p>REJECT: Deny</p>                                                                                      |
| businessLabels     | json_array  | Business label return                              | No            | All business labels, see [businessLabels parameter](#businessLabels2)                                                                                                           |
| allLabels          | json_array  | Risk label return                                  | No            | All risk labels, see [allLabels parameter](#allLabels2)                                                                                                                         |
| riskLabel1         | string      | Primary risk label                                 | Yes           |                                                                                                                                                                                 |
| riskLabel2         | string      | Secondary risk label                               | Yes           |                                                                                                                                                                                 |
| riskLabel3         | string      | Tertiary risk label                                | Yes           |                                                                                                                                                                                 |
| riskDescription    | string      | Reason for risk                                     | Yes           | For human understanding of the reason for risk as a reference, the program should not rely on the value of this parameter for logical processing                                |
| riskDetail         | json_object | Risk details                                        | No            | See [riskDetail parameter](#riskDetail2)                                                                                                                                       |



## Detailed content of <span id="riskDetail2">riskDetail</span>

| **Parameter Name** | **Type**   | **Parameter Description**                  | **Mandatory** | **Specification**                                                                                      |
| :----------------- | :--------- | :----------------------------------------- | :------------ | :----------------------------------------------------------------------------------------------------- |
| audioText          | string     | Translated text result of the audio        | No            |                                                                                                         |
| matchedLists       | json_array | Hits of custom list information by client  | No            | Returned when hitting the client's custom list, see [matchedLists parameter](#matchedLists2)            |
| riskSegments       | json_array | High-risk content segments                 | No            | Exists in functions like politics, terrorism, prohibition, competition products, advertising law, etc., see [riskSegments parameter](#riskSegments2) |

In riskDetail, detailed content of <span id="matchedLists2">matchedLists</span>:

| **Parameter Name** | **Type**   | **Parameter Description**           | **Mandatory** | **Specification**     |
| :----------------- | :--------- | :---------------------------------- | :------------ | :-------------------- |
| name               | string     | Client custom list name             | Yes           |                       |
| words              | json_array | Sensitive word information in this list | Yes           | See [words parameter](#words2) |

In matchedLists, detailed content of <span id="words2">words</span>:

| **Parameter Name** | **Type**  | **Parameter Description** | **Mandatory** | **Specification** |
| :----------------- | :-------- | :------------------------ | :------------ | :---------------- |
| word               | string    | Sensitive word            | Yes           |                   |
| position           | int_array | Position of the sensitive word | Yes           |                   |

In riskDetail, detailed content of <span id="riskSegments2">riskSegments</span>:

| **Parameter Name** | **Type**  | **Parameter Description**    | **Mandatory** | **Specification** |
| :----------------- | :-------- | :--------------------------- | :------------ | :---------------- |
| segment            | string    | High-risk content segment    | No            |                   |
| position           | int_array | Position of the high-risk content segment | No            |                   |

In <span id="audioTags">audioTags</span>, detailed content:

| **Parameter Name** | **Type**    | **Parameter Description** | **Mandatory** | **Specification**                                                                       |
| :----------------- | :---------- | :------------------------ | :------------ | :------------------------------------------------------------------------------------- |
| gender             | json_object | Gender label              | No            | Returned when type includes GENDER                                                     |
| timbre             | json_array  | Timbre label              | No            | Returned when type includes TIMBRE                                                     |
| song               | int         | Singing label             | No            | <p>Returned when type includes SING</p><p>Possible values:</p><p>0: No singing</p><p>1: Singing</p> |
| language           | json_object | Language recognition      | No            | Returned when type includes LANGUAGE                                                   |

In audioTags, detailed content of gender:

| **Parameter Name**  | **Type** | **Parameter Description**  | **Mandatory** | **Specification**                                      |
| :------------------ | :------- | :------------------------ | :------------ | :----------------------------------------------------- |
| label               | string   | Gender label name          | Yes           | <p>Possible values:</p><p>Male</p><p>Female</p>         |
| probability         | int      | Probability size of the corresponding gender | Yes           | Values from 0-100, higher values indicate higher probability |


In **audioTags**, detailed content for **timbre**:

| **Parameter Name** | **Type** | **Parameter Description**    | **Mandatory** | **Specification**                                                                                                                               |
| :----------------- | :------- | :-------------------------- | :------------ | :---------------------------------------------------------------------------------------------------------------------------------------------- |
| label              | string   | Timbre label type           | Yes           | <p>Possible values:</p><p>Uncle voice</p><p>Youth voice</p><p>Kid voice</p><p>Elderly voice</p><p>Queen voice</p><p>Mature woman voice</p><p>Young girl voice</p><p>Loli voice</p><p>Aunt voice</p> |
| probability        | int      | Probability size of the corresponding timbre label | Yes           | Values from 0-100, higher values indicate higher probability                                                                                    |

In **audioTags**, detailed content for **language**:

| **Parameter Name** | **Type** | **Parameter Description**    | **Mandatory** | **Specification**                                                   |
| :----------------- | :------- | :-------------------------- | :------------ | :------------------------------------------------------------------ |
| label              | int      | Language recognition type identifier | Yes           | <p>Possible values:</p><p>0: Mandarin</p><p>1: English</p><p>2: Cantonese</p> |
| probability        | int      | Probability size of the corresponding language label | Yes           | Values from 0-100, higher values indicate higher probability      |

For each item in the *<span id="businessLabels2">businessLabels</span>* array:

| **Parameter Name**     | **Type**    | **Parameter Description** | **Mandatory** | **Specification**                                     |
| :--------------------- | :---------- | :------------------------ | :------------ | :---------------------------------------------------- |
| businessLabel1         | string      | First-level label         | Yes           | First-level label                                     |
| businessLabel2         | string      | Second-level label        | Yes           | Second-level label                                    |
| businessLabel3         | string      | Third-level label         | Yes           | Third-level label                                     |
| businessDescription    | string      | Label description         | Yes           | Format is "First-level label:Second-level label:Third-level label" in Chinese |
| confidenceLevel        | int         | Confidence level          | No            | Optional values between 0～2, higher value indicates higher confidence        |
| probability            | float       | Confidence                | No            | Optional values between 0~1, higher value indicates higher confidence         |
| businessDetail         | json_object | Detailed information      | No            | Reserved field                                        |

For each item in the *<span id="allLabels2">allLabels</span>* array:

| **Parameter Name**     | **Type**    | **Parameter Description** | **Mandatory** | **Specification**                                                     |
| :--------------------- | :---------- | :------------------------ | :------------ | :-------------------------------------------------------------------- |
| riskLabel1             | string      | First-level risk label    | Yes           | First-level


For each item in the *tokenProfileLabels* and *tokenRiskLabels* arrays:

| **Parameter Name** | **Type** | **Parameter Description** | **Mandatory** | **Specification**                                                                                                 |
| :----------------- | :------- | :------------------------ | :------------ | :------------------------------------------------------------------------------------------------------------------ |
| label1             | string   | First-level label         | No            |                                                                                                                     |
| label2             | string   | Second-level label        | No            |                                                                                                                     |
| label3             | string   | Third-level label         | No            |                                                                                                                     |
| description        | string   | Label description         | No            | Label description for accounts, for human understanding only, not for program logic.                                |
| timestamp          | int      | Timestamp for labeling    | No            | 13-digit Unix timestamp in milliseconds.                                                                            |

For the *<span id="auxInfo">auxInfo</span>*:

| **Parameter Name** | **Type** | **Parameter Description** | **Mandatory** | **Specification**                                                                                                 |
| :----------------- | :------- | :------------------------ | :------------ | :------------------------------------------------------------------------------------------------------------------ |
| errorCode          | int      | Status code               | Yes           | <p>Status codes:</p><p>2003: Audio download failed</p><p>2007: No valid data available</p>                          |

## Example

### Upload Request Sample


```bash
curl -v 'http://api-audio-bj.fengkongcloud.com/audio/v4' -d '{
    "accessKey": "*************",
    "appId": "default",
    "eventId": "default",
    "type": "POLITY_EROTIC_MOAN_ADVERT_GENDER_TIMBRE_SING_LANGUAGE",
    "btId": "test1",
    "contentType": "URL",
    "content": "*************",
    "callback": "*************",
    "acceptLang": "en",
    "data": {
        "returnAllText": 1,
        "room": "general",
        "tokenId": "token-short"
        }
    }
}'

```

### Synchronous Response Sample

```json
{
    "code": 1100,
    "message": "Success",
    "requestId":" *************",
    "btId":"*************"
}
```


### Proactive Query Result Request Example

```bash
curl -v 'http://api-audio-bj.fengkongcloud.com/query_audio/v4' -d '{
    "accessKey": "*************",
    "btId": "*************"
}'
```

### Proactive Query Result Return Example

```json
{
    "requestId": "6a9cb980346dfea41111656a514e9109",
    "btId": "1604311839040",
    "code": 1100,
    "message": "normal",
    "riskLevel": "PASS",
    "audioDetail": [
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0000",
            "audioStarttime": 0,
            "audioEndtime": 10,
            "audioUrl": "https://voice-xjp-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0000.mp3",
            "businessLabels":[
                {
                    "businessDescription":"Sing:Sing:Sing",
                    "businessLabel1":"Sing",
                    "businessLabel2":"Sing",
                    "businessLabel3":"Sing",
                    "confidenceLevel":2,
                    "probability":0.858334402569294
                }
            ],
            "allLabels":[
                {
                    "probability":1,
                    "riskDescription":"Erotic:Harassment:HarassmentSev",
                    "riskDetail":{
                        "riskSource":1001
                    },
                    "riskLabel1":"Erotic",
                    "riskLabel2":"Harassment",
                    "riskLabel3":"HarassmentSev",
                    "riskLevel":"REJECT"
                }
            ],          
            "riskLevel": "PASS",
            "riskLabel1": "normal",
            "riskLabel2": "",
            "riskLabel3": "",
            "riskDescription": "normal",
            "riskDetail": {
                "audioText": ""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0001",
            "audioStarttime": 10,
            "audioEndtime": 20,
            "audioUrl": "https://voice-xjp-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0001.mp3",
            "riskLevel": "PASS",
            "riskLabel1": "normal",
            "riskLabel2": "",
            "riskLabel3": "",
            "riskDescription": "normal",
            "riskDetail": {
                "audioText": ""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0002",
            "audioStarttime": 20,
            "audioEndtime": 30,
            "audioUrl": "https://voice-xjp-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0002.mp3",
            "riskLevel": "PASS",
            "riskLabel1": "normal",
            "riskLabel2": "",
            "riskLabel3": "",
            "riskDescription": "normal",
            "riskDetail": {
                "audioText": ""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0003",
            "audioStarttime": 30,
            "audioEndtime": 40,
            "audioUrl": "https://voice-xjp-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0003.mp3",
            "riskLevel": "PASS",
            "riskLabel1": "normal",
            "riskLabel2": "",
            "riskLabel3": "",
            "riskDescription": "normal",
            "riskDetail": {
                "audioText": ""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0004",
            "audioStarttime": 40,
            "audioEndtime": 50,
            "audioUrl": "https://voice-xjp-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0004.mp3",
            "riskLevel": "PASS",
            "riskLabel1": "normal",
            "riskLabel2": "",
            "riskLabel3": "",
            "riskDescription": "normal",
            "riskDetail": {
                "audioText": ""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0005",
            "audioStarttime": 50,
            "audioEndtime": 60,
            "audioUrl": "https://voice-xjp-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0005.mp3",
            "riskLevel": "PASS",
            "riskLabel1": "normal",
            "riskLabel2": "",
            "riskLabel3": "",
            "riskDescription": "normal",
            "riskDetail": {
                "audioText": ""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0006",
            "audioStarttime": 60,
            "audioEndtime": 60,
            "audioUrl": "https://voice-xjp-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0006.mp3",
            "riskLevel": "PASS",
            "riskLabel1": "normal",
            "riskLabel2": "",
            "riskLabel3": "",
            "riskDescription": "normal"
        }
    ],
    "audioTags": {
        "gender": {
            "label": "Female",
            "probability": 95
        },
        "language": [
            {
                "confidence": 0,
                "label": 2
            },
            {
                "confidence": 99,
                "label": 0
            },
            {
                "confidence": 0,
                "label": 1
            }
        ],
        "song": 0,
        "timbre": [
            {
                "label": "Female",
                "probability": 95
            },
            {
                "label": "Queen",
                "probability": 12
            },
            {
                "label": "YoungLady",
                "probability": 37
            },
            {
                "label": "TeenageGirl",
                "probability": 56
            },
            {
                "label": "Aunty",
                "probability": 67
            },
            {
                "label": "LittleGirl",
                "probability": 24
            }
        ]
    }
}
```

### Callback Return Example

```json
{
    "requestId":"6a9cb980346dfea41111656a514e9109",
    "btId":"1604311839040",
    "code":1100,
    "message":"normal",
    "riskLevel":"PASS",
    "audioDetail":[
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0000",
            "audioStarttime":0,
            "audioEndtime":10,
            "audioUrl":"https://voice-xjp-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0000.mp3",
            "businessLabels":[
                {
                    "businessDescription":"Sing:Sing:Sing",
                    "businessDetail":{

                    },
                    "businessLabel1":"Sing",
                    "businessLabel2":"Sing",
                    "businessLabel3":"Sing",
                    "confidenceLevel":2,
                    "probability":0.858334402569294
                }
            ],       
            "allLabels":[
                {
                    "probability":1,
                    "riskDescription":"Erotic:Harassment:HarassmentSev",
                    "riskDetail":{
                        "riskSource":1001
                    },
                    "riskLabel1":"Erotic",
                    "riskLabel2":"Harassment",
                    "riskLabel3":"HarassmentSev",
                    "riskLevel":"REJECT"
                }
            ],           
            "riskLevel":"PASS",
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskDescription":"normal",
            "riskDetail":{
                "audioText":""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0001",
            "audioStarttime":10,
            "audioEndtime":20,
            "audioUrl":"https://voice-xjp-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0001.mp3",
            "riskLevel":"PASS",
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskDescription":"normal",
            "riskDetail":{
                "audioText":""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0002",
            "audioStarttime":20,
            "audioEndtime":30,
            "audioUrl":"https://voice-xjp-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0002.mp3",
            "riskLevel":"PASS",
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskDescription":"normal",
            "riskDetail":{
                "audioText":""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0003",
            "audioStarttime":30,
            "audioEndtime":40,
            "audioUrl":"https://voice-xjp-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0003.mp3",
            "riskLevel":"PASS",
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskDescription":"normal",
            "riskDetail":{
                "audioText":""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0004",
            "audioStarttime":40,
            "audioEndtime":50,
            "audioUrl":"https://voice-xjp-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0004.mp3",
            "riskLevel":"PASS",
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskDescription":"normal",
            "riskDetail":{
                "audioText":""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0005",
            "audioStarttime":50,
            "audioEndtime":60,
            "audioUrl":"https://voice-xjp-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0005.mp3",
            "riskLevel":"PASS",
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskDescription":"normal",
            "riskDetail":{
                "audioText":""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0006",
            "audioStarttime":60,
            "audioEndtime":60,
            "audioUrl":"https://voice-xjp-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0006.mp3",
            "riskLevel":"PASS",
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskDescription":"normal"
        }
    ],
    "audioTags": {
        "gender": {
            "label": "Female",
            "probability": 95
        },
        "language": [
            {
                "confidence": 0,
                "label": 2
            },
            {
                "confidence": 99,
                "label": 0
            },
            {
                "confidence": 0,
                "label": 1
            }
        ],
        "song": 0,
        "timbre": [
            {
                "label": "Female",
                "probability": 95
            },
            {
                "label": "Queen",
                "probability": 12
            },
            {
                "label": "YoungLady",
                "probability": 37
            },
            {
                "label": "TeenageGirl",
                "probability": 56
            },
            {
                "label": "Aunty",
                "probability": 67
            },
            {
                "label": "LittleGirl",
                "probability": 24
            }
        ]
    }
}
```
