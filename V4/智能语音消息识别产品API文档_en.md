# Intelligent Voice Message Recognition Product API Documentation

- - - - -

***All rights reserved. Unauthorized reproduction is prohibited.***

- - - - -

- [Intelligent Voice Message Recognition Product API Documentation](#智能语音消息识别产品api文档)
  - [Voice Message Request](#语音消息请求)
    - [Request URL](#请求url)
    - [Character Encoding](#字符编码)
    - [Request Method](#请求方法)
    - [Suggested Timeout Duration](#建议超时时长)
    - [Audio Format Restrictions](#音频格式限制)
    - [Request Body Limitations](#请求体限制)
    - [Request Parameters](#请求参数)
    - [Synchronous Return Parameters](#同步返回参数)
  - [Examples](#示例)
    - [Upload Request Example](#上传请求示例)
    - [Synchronous Return Example](#同步返回示例)


## Voice Message Request

### Request URL

| Cluster | URL                                                     | Supported Product List       |
| ------- | ------------------------------------------------------- | ---------------------------- |
| Shanghai | `http://api-audio-sh.fengkongcloud.com/audiomessage/v4` | Chinese Audio File Sync Interface |

### Character Encoding

`UTF-8`

### Request Method

`POST`

### Suggested Timeout Duration

10s

### Audio Format Restrictions

`WAV`, `MP3`, `AAC`, `AMR`, `3GP`, `M4A`, `WMA`, `OGG`, `APE`, `FLAC`, `ALAC`, `WAVPACK`, `SILK_V3`, etc.

### Request Body Limitations

The total size of all request parameters must not exceed 18M.

### Request Parameters

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Request Parameter Name** | **Type**    | **Parameter Description** | **Required** | **Specification**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| :------------------------- | :---------- | :------------------------ | :----------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| accessKey                  | string      | Company Key               | Required     | Provided by Shumei                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| appId                      | string      | Application Identifier    | Required     | Used to distinguish applications, needs to contact Shumei service for activation, please use the value provided by Shumei separately                                                                                                                                                                                                                                                                                                                                                                                                                               |
| eventId                    | string      | Event Identifier          | Required     | Used to distinguish scene data, needs to contact Shumei service for activation, please use the value provided by Shumei separately                                                                                                                                                                                                                                                                                                                                                                                                                               |
| type                       | string      | Risk Type to Detect       | Required     | <p>AUDIOPOLITICAL: Leader Voiceprint Recognition</p><p>POLITY: Political Recognition</p><p>EROTIC: Erotic Recognition</p><p>ADVERT: Advertisement Recognition</p><p>ANTHEN: National Anthem Recognition</p><p>MOAN: Moan Recognition</p><p>DIRTY: Abuse Recognition</p><p>GENDER: Gender Recognition</p><p>TIMBRE: Timbre Recognition</p><p>SING: Singing Recognition</p><p>LANGUAGE: Language Recognition</p><p>BANEDAUDIO: Banned Songs</p><p>VOICE: Voice Attributes</p><p>AUDIOSCENE: Sound Scene</p><p>MINOR: Minor Recognition</p><p>For timbre, singing, and language recognition, GENDER is required</p><p>For combined recognition, connect with underscores, e.g., POLITY_EROTIC_MOAN for political, erotic, and moan recognition</p><p>Suggested input:<br/>POLITY_EROTIC_MOAN_ADVERT</p> |
| contentType                | string      | Format of Audio Content   | Required     | <p>Optional values:</p><p>URL: Audio content is a URL address;</p><p>RAW: Audio content is base64 encoded data</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| content                    | string      | Audio Content to Recognize | Required     | <p>Can be a URL address or base64 encoded data.</p><p>Base64 encoded data limit is 15M, only supports pcm, wav, mp3 formats, and pcm format data must use 16-bit little-endian encoding. Recommended to use pcm, wav format for transmission</p>                                                                                                                                                                                                                                                                                                                                                         |
| data                       | json object | Related Information for This Request | Required     | Maximum 1MB, [see data parameters](#data)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| btId                       | string      | Unique Identifier for Audio File | Required     | Uniquely identifies this audio file, convenient for matching callback results, up to 128 characters, must not be duplicated                                                                                                                                                                                                                                                                                                                                                                                                                                           |

Among them, the content of <span id="data">data</span> is as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification**                                                                                   |
| :------------------------- | :------- | :------------------------ | :----------- | :----------------------------------------------------------------------------------------- |
| tokenId                    | string   | User Account              | Optional     | Used for user behavior analysis, recommended to pass in user UID                                                          |
| formatInfo                 | string   | Audio Data Format         | Optional     | Must be present when audio content format is RAW, optional values: pcm, wav, mp3                                       |
| rate                       | int      | Audio Data Sampling Rate  | Optional     | Must be present when audio data format is pcm, range 8000-32000.                                        |
| track                      | int      | Audio Data Channel Count  | Optional     | <p>Must be present when audio data format is pcm, optional values:</p><p>1: Mono</p><p>2: Stereo</p>             |
| lang                       | string   | Audio Stream Language Type | Optional     | Optional values are as follows (default is zh):<br/>zh: Chinese<br/>en: English<br/>ar: Arabic                    |
| returnAllText              | int      | Return Level of Audio Segments | Optional     | <p>0: Return audio segments with risk level other than pass</p><p>1: Return all audio segments with risk levels</p><p>Default is 0</p> |

### Synchronous Return Parameters

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Return Result Parameter Name** | **Parameter Type** | **Parameter Description** | **Required** | **Specification**                                                                                                 |
| :------------------------------- | :----------------- | :------------------------ | :----------- | :------------------------------------------------------------------------------------------------------- |
| requestId                        | string             | Unique Identifier for This Request | Yes           |                                                                                                          |
| code                             | int                | Request Return Code       | Yes           | <p>1100: Success</p><p>1901: QPS Limit Exceeded</p><p>1902: Invalid Parameters</p><p>1903: Service Failure</p><p>9101: No Permission</p> |
| message                          | string             | Request Return Description, Corresponding to Request Return Code | Yes           |                                                                                                          |
| detail                           | json               | Request Return Detailed Data | No           | Must return in case of 1100                                                                                           |

detail content:

| **Parameter Name**  | **Type**    | **Parameter Description** | **Required** | **Specification**                                                                                                                                            |
| :------------------ | :---------- | :------------------------ | :----------- | :-------------------------------------------------------------------------------------------------------------------------------------------------- |
| riskLevel           | string      | Handling Suggestion for Current Event | Yes           | <p>Possible return values:<br/>PASS: Pass</p><p>REVIEW: Review</p><p>REJECT: Reject</p><p>Suggestion: Do not use the result directly during initial integration, adjust the interception scale, and use it after meeting expectations</p> |
| audioText           | string      | Transcription Result of the Entire Audio | Yes           |                                                                                                                                                     |
| audioTime           | int         | Duration of the Entire Audio | Yes           | In seconds                                                                                                                                              |
| audioDetail         | json_array  | Audio Segment Information | Yes           | Callback audio segment information, [see audioDetail parameters](#audioDetail)                                                                                             |
| auxInfo             | json_object | Auxiliary Information     | No           |                                                                                                                                                     |

Among them, the detailed content of <span id="audioDetail">audioDetail</span> is as follows:

| **Parameter Name**      | **Type**    | **Parameter Description** | **Required** | **Specification**                                                                 |
| :---------------------- | :---------- | :------------------------ | :----------- | :----------------------------------------------------------------------- |
| requestId               | string      | Unique Identifier for Audio Segment Request | Yes           |                                                                          |
| audioStarttime          | float       | Start Time of Audio Segment | Yes           | Relative to the start time of the audio, in seconds                                         |
| audioEndtime            | float       | End Time of Audio Segment   | Yes           | Relative to the start time of the audio, in seconds                                         |
| audioUrl                | string      | Audio Segment Link         | Yes           | In mp3 format                                                                  |
| riskLevel               | string      | Recognition Result of Audio Segment | Yes           | <p>Possible return values:<br/>PASS: Pass</p><p>REVIEW: Review</p><p>REJECT: Reject</p> |
| riskLabel1              | string      | Primary Risk Label         | Yes           |                                                                          |
| riskLabel2              | string      | Secondary Risk Label       | Yes           |                                                                          |
| riskLabel3              | string      | Tertiary Risk Label        | Yes           |                                                                          |
| riskDescription         | string      | Reason for Risk            | Yes           | For human understanding of the risk reason, do not rely on this parameter value for program logic processing           |
| riskDetail              | json_object | Risk Details               | No           | [See riskDetail parameters](#riskDetail)                                        |

Among them, the detailed content of <span id="riskDetail">riskDetail</span> is as follows:

| **Parameter Name**   | **Type**   | **Parameter Description** | **Required** | **Specification**                                                                                |
| :------------------- | :--------- | :------------------------ | :----------- | :-------------------------------------------------------------------------------------- |
| audioText            | string     | Transcription Result of Audio | No           |                                                                                         |
| matchedLists         | json_array | Information of Hit Custom Lists | No           | Returned when hitting custom lists, [see matchedLists parameters](#matchedLists)                         |
| riskSegments         | json_array | High-Risk Content Segments | No           | Present in political, terrorism, banned, competitive, advertising law functions, [see riskSegments parameters](#riskSegments) |

In riskDetail, the detailed content of <span id="matchedLists">matchedLists</span> is as follows:

| **Parameter Name** | **Type**   | **Parameter Description** | **Required** | **Specification**                |
| :----------------- | :--------- | :------------------------ | :----------- | :---------------------- |
| name               | string     | Custom List Name          | Yes           |                         |
| words              | json_array | Sensitive Words in This List | Yes           | [See words parameters](#words) |

In matchedLists, the detailed content of <span id="words">words</span> is as follows:

| **Parameter Name** | **Type**  | **Parameter Description** | **Required** | **Specification** |
| :----------------- | :-------- | :------------------------ | :----------- | :------- |
| word               | string    | Sensitive Word            | Yes           |          |
| position           | int_array | Position of Sensitive Word | Yes           |          |

In riskDetail, the detailed content of <span id="riskSegments">riskSegments</span> is as follows:

| **Parameter Name** | **Type**  | **Parameter Description** | **Required** | **Specification** |
| :----------------- | :-------- | :------------------------ | :----------- | :------- |
| segment            | string    | High-Risk Content Segment | No           |          |
| position           | int_array | Position of High-Risk Content Segment | No           |          |



## Examples

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
    "data": {
    		"returnAllText":1,
        "room": "general",
        "tokenId": "token-short"
        }
    }
}'
```

### Synchronous Return Example

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
                "audioUrl":"http://voice-stream.oss-cn-hangzhou.aliyuncs.com/POST_AUDIO%2F20221102%2F817c8509359500c898a762ffe93a582b_a0000.mp3",
                "riskLevel":"REJECT",
                "riskLabel1":"abuse",
                "riskLabel2":"buwenmingyongyu",
                "riskLabel3":"qingdubuwenmingyongyu",
                "riskDescription":"Abuse: Uncivilized Language: Mild Uncivilized Language",
                "riskDetail":{
                    "audioText":"超你今年十一月份十二月份你找个毛东啊我了下乡那就装了的下个箱子装了不不挂现在查整好你过两年都能你"
                }
            }
        ],
        "audioTags":{

        },
        "audioText":"超你今年十一月份十二月份你找个毛东啊我了下乡那就装了的下个箱子装了不不挂现在查整好你过两年都能你",
        "audioTime":10,
        "code":1100,
        "requestParams":{
            "channel":"TEST",
            "lang":"zh",
            "returnAllText":1,
            "tokenId":"test01"
        },
        "riskLevel":"REJECT"
    }
}
```