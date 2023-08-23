# NextData Intelligent AudioStream Moderation API Interface Document


## Request parameters

### Request URL:

| Colony   | URL                                                           | Supported Language                             |
| ------ | ------------------------------------------------------------- | ---------------------------------------- |
| Singapore | `http://api-audiostream-xjp.fengkongcloud.com/audiostream/v4` | Chinese、English、Arabic |
| Silicon Valley   | `http://api-audiostream-gg.fengkongcloud.com/audiostream/v4`  | Chinese、English、Arabic |
| Shanghai   | `http://api-audiostream-sh.fengkongcloud.com/audiostream/v4`  | Chinese       |

### Request method:

`POST`

### Character encoding format:

`UTF-8`

### Recommended timeout length:

1s


### Request parameters:

It is placed in the HTTP Body in Json format. The specific parameters are as follows:

| **Request parameter name** | **Type**    | **Parameter Description**   | **Must be transferred in or not**               | **Detailed description**                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| -------------- | ----------- | -------------- | -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| accessKey      | string      | Interface authentication key       | Y                          | Provided by NextData                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| appId          | string      | Application ID       | Y                          | It is used to distinguish between applications. You need to contact the service of NextData to open. Please use the value transfer provided by NextData separately                                                                                                                                                                                                                                                                                                                                        |
| eventId        | string      | Event ID       | Y                          | You need to contact the service of NextData to open. Please use the value transfer provided by NextData separately |
| type           | string      | Types of risks to be detected | Y | Optional values:<br/> `POLITICS`<br/>`PORN`<br/>`AD`<br/>`MOAN`<br/>`ABUSE`<br/>`SING`<br/>`MINOR`<br/>The above types can be combined with underscores, such as:`TEXTRISK_FRUAD` |
| data | json_object | Requested data content | Y | Up to 1MB, [see data parameter for details](#data) |
| acceptLang | string | Returns the language type of interface information | Y | Optional language type of returned information:<br/>`zh`: Chinese<br/>`en`: English<br/>The default is `zh`. If you need to return results in English, this field is required |
| callback | string | Adress of callback | Y | this adress is used to get results of moderation，HTTP and HTTPS are supported |
<span id="data">The content of data is as follows:</span>

| **Request parameter name** | **Type**    | **Parameter Description**                                           | **Must be transferred in or not** | **Detailed description**                                                                                                                                                     |
| -------------- | ----------- | ------------------------------------------------------ | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| tokenId        | string      | UID                                           | Y            | It is recommended to use your company's user UID (encryptable) to generate the user account identification.                                                                        |
| btId           | string      | Unique identification                     | Y            | used to search data，up to 128  character                                                                                                                           |
| streamType     | string      | Type of audiostream                                                 | Y            | Optional values：<br/>`NORMAL`<br/>`ZEGO`<br/>`AGORA`<br/>`TRTC`                                                               |
| url            | string      | adress of audiostream or videostream                                             | N            | must be afferented when streamType is `NORMAL`                                                                                                                                |
| lang           | string      | Language of audio content to be tested                                         | Y            | Optional values：<br/>`zh`：Chinese<br/>`en`：English<br/>`ar`：Arabic<br/>The default value is zh.                                                                           |
| room | string | ID of livestream | N |  |
| role | string | role of anchor | N | Optional values:<br/>`ADMIN`<br/>`HOST`<br/>`SYSTEM`<br/>`USER` |
| returnAllText | int | switch of  return full result return | N | Optional values（default value is `0`）：<br/>`0`：return results of audio stream clips what riskLevel si no PASS<br/>`1`：return all results of audio stream clips |
| returnPreText | int | switch of return result of previous text | N | Optional values（default value is `0`）：<br/>`0`：not return result of previous text；<br/>`1`：return result of previous text； |
| returnPreAudio | int | switch of return result of previous audio clips URL | N | Optional values（default value is `0`）：<br/>`0`：not return；<br/>`1`：return； |
| returnFinishInfo | int | switch of audit status notice | N | Optional values（default value is `0`）：<br/>`0`：no inform<br/>`1`：inform |
| extra | json_object | Auxiliary parameters | N | Relevant information for auxiliary text detection, [see extra parameters](#extra) |
| liveTitle | string | title of liveroom | N |  |
| anchorName | string | name of anchor | N |  |


<span id="extra">The contents of each element of the extra array in data are as follows:</span>

| **Request parameter name** | **Type**    | **Parameter Description** | **Must be transferred in or not** | **Detailed description**                         |
| -------------- | ----------- | ------------ | ------------ | -------------------------------- |
| passThrough    | json_object | The pass-through field passed in by the customer     | N            | The content of this field will be returned along with the return value |

## Return results

It is placed in the HTTP Body in Json format. The specific parameters are as follows:

| **Request parameter name** | **Type** | **Parameter Description**       | **Must be returned or not** | **Detailed description**                                                                                            |
| ------------------ | ------------ | ------------------ | ------------ | --------------------------------------------------------------------------------------------------- |
| requestId          | string       | Request ID | Y           | The unique identifier of this request data is used for troubleshooting and effect optimization. It is strongly recommended to save it                                                                                         |
| code               | int          | Return code         | Y            | `1100`：Success<br/>`1901`：QPS overrun<br/>`1902`：Illegal parameter<br/>`1903`：Service failed<br/>`9101`：Operation without permission  |
| message            | string       | Return code description       | Y          | Corresponding to code,<br/>include: <br/>Success<br/>QPS overrun<br/>Illegal parameter<br/>Service failed<br/>Operation without permission                                                                                    |
| detail             | json_object  | Describe detailed information       | N           | Describe error code request, see detail parameter for details                                                           |

<span id="detail">The detail structure is as follows:</span>

| **Parameter Name** | **Parameter Type** | **Parameter Description** | **Required** | **Description**                                                                                                                                                                                                                                                           |
| ------------ | ------------ | ------------ | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| errorcode    | int          |              | N           | Status code<br/>1001: Duplicate streaming                                                                                                                                                                                                                                        |
| dupRequestId | string       |              | N           | Indicates the duplicated requestId<br/>When errorCode is 1001, which means duplicated streaming, the dupRequestId field will be returned.<br/>For example, if there is no response after the first request but the audio stream has actually started the review, without requestId, it is unable to actively close the review.<br/>You can request again, receive the information of duplicated streaming through the returned dupRequestId and call the close review interface with it. |

## Callback Results

When the audio stream stably connects and starts to receive monitoring from NextData, NextData will continuously call back recognition results to the client. The callback strategy is different according to the different values of returnAllText.

When returnAllText is 0, NextData will call back results to the client when it detects inappropriate content in the audio stream.

When returnAllText is 1, NextData will return recognition results of the latest 10 seconds to the client every 10 seconds.

The callback results are put in the HTTP Body in Json format, and the specific parameters are as follows:

| **Parameter Name**  | **Type**    | **Parameter Description**                   | **Required** | **Specification**                                                                                                                                                      |
| ----------- | ----------- | ------------------------------ | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| requestId   | string      | The unique identification of this request             | Y           |                                                                                                                                                               |
| btId        | string      | The unique identification of the audio                   | Y           |                                                                                                                                                               |
| code        | int         | Return code of the request                     | Y           | `1100`: Success<br/>`1901`: QPS exceeded<br/>`1902`: Invalid parameter<br/>`1903`: Service failed<br/>`9101`: No permission to operate. The fields other than message and requestId will only exist when code is 1100. |
| message     | string      | Description of the request return, corresponding to the request return code | Y           |                                                                                                                                                               |
| statCode     | int        | Review status                   | N            | <p>0: In review</p><p>1: Review ended</p>                                                                                                                                   |
| audioDetail | json_object | Information of the risky audio fragment               | N           | Returned only when code is 1100. See audioDetail parameter for details                                                                                                  |
| passThrough | json_object | Passthrough field                       | N           | The content of this field is the same as the value of passThrough in the extra field of the request parameter data.                                                                                                        |
| auxInfo     | json_object | Auxiliary information                       | N           |                                                                                                                                                          |

<span id="audioDetail">Among them, the audioDetail structure is as follows:</span>

| **Parameter Name**  | **Type**    | **Parameter Description**                   | **Required** | **Specification**                                                                                                                                         |
| --------------- | ----------- | ---------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| audioUrl        | string      | Audio segment address           | Y           |                                                                                                                                                  |
| riskLevel       | string      | Disposal suggestions for current events     | Y           | PASS: Pass <br/> REVIEW: Review <br/> REJECT: Reject                                                                                               |
| riskLabel1      | string      | Level 1 tag              | Y           | Each Level 1 tag is independent. Returns "normal" when riskLevel is PASS.                                                                                  |
| riskLabel2      | string      | Level 2 tag               | Y           | Level 2 tags belong to level 1 tags. It is empty when riskLevel is PASS.                                                                                                |
| riskLabel3      | string      | Level 3 tag               | Y           | Level 3 tags belong to level 2 tags. It is empty when riskLevel is PASS.                                                                                                |
| riskDescription | string      | Tag explanation              | Y           | Returns "Hit custom list" when hitting the user-defined list; Returns "normal" when riskLevel is PASS. The other situation is in the form of the Chinese name of Level 1 tag: Level 2 tag: Level 3 tag, only for human understanding of risk reasons as a reference, the program should not rely on the value of this parameter for logical processing |
| audioText       | string      | Result of audio transcription     | N           | When returnPreAudio is 1, it contains the text content of the previous illegal audio segment and the text content of the current illegal audio segment. <br/>When returnPreAudio is 0, it only contains the text content of the current illegal audio segment.        |
| preAudioUrl     | string      | Audio address of the previous audio segment | N           | When returnPreAudio is 1, it contains the audio address of the previous illegal audio segment. <br/> When returnPreAudio is 0, it is not returned.                                                  |
| riskDetail      | json_object | Risk detail information           | N          | Returned when code is 1100. See riskDetail parameter                                                                                       |
| auxInfo         | json_object | Other auxiliary information           | Y           | Returns auxiliary information such as timestamps. See auxInfo parameter                                                                                                |
| businessLabels  | json_array  | Audio business tags           | N           | Returns tags such as gender, tone, and whether to sing. See businessLabels parameter                                                                        |
| allLabels       | json_array  | Risk tags              | N           | All risk tags. See allLabels parameter                                                                                                    |
| riskSource      | int         | Risk source              | Y           | Risk source                                                                                                                                         |
| tokenProfileLabels  | json_array  | Account attribute tags           | N           | Only returned when the function is enabled. See tokenProfileLabels, tokenRiskLabels parameters                                                                        |
| tokenRiskLabels  | json_array  | Account risk tags          | N           | Only returned when the function is enabled. See tokenProfileLabels, tokenRiskLabels parameters                                                                        |
| speakers         | json_array | Speaker information for this audio segment     |N       | The uid and volume information of the speaker in this audio segment are collected once per second, and a segment does not exceed 10 times. <br/> This structure is an array with up to |

The auxInfo structure is as follows:：

| **Parameter Name**  | **Type**    | **Parameter Description**                   | **Required** | **Specification**                                                                                      |
| ----------------- | -------- | ---------------- | ------------ | ----------------------------------------------------------------------------------------------- |
| audioStartTime  | string      | Auxiliary parameter         | Y          | The start time (absolute time) of the violation content                                                                    |
| audioEndTime | string      | Auxiliary parameter         | Y           | The end time (absolute time) of the violation content                                                                    |
| beginProcessTime  | int      | Auxiliary parameter         | Y           | The time when processing started (13-digit timestamp)                                                                    |
| finishProcessTime | int      | Auxiliary parameter         | Y          | The time when processing ended (13-digit timestamp)                                                                    |
| userId            | int      | Agora user account identifier | N           | Only exists in the case of partitioning, the returned userId is the actual user ID in the room and has nothing to do with the uid parameter in the request.                     |
| strUserId         | string   | TRTC user account identifier | N           | User account identifier (only exists in the case of TRTC partitioning). The returned userId is the actual user ID in the room and has nothing to do with the uid parameter in the request. |
| room              | string   | Room number           | N           |                                                                                                 |
| seiInfo           | array    | SEI information          | N           | (Need to contact NextData to open)                                                                    |

<span id="riskDetail">Where, the riskDetail structure is as follows:</span>

| **Parameter Name**  | **Type**    | **Parameter Description**                   | **Required** | **Specification**                                                                             |
| ------------ | ---------- | ------------------------ | ------------ | --------------------------------------------------------------------------------------- |
| audioText    | string     | Result of audio transcription       | N           |                                                                                         |
| matchedLists | json_array | Client-defined list of hits | N           | Returned when the client-defined list is hit, otherwise it does not exist. See matchedLists parameter           |
| riskSegments | json_array | High-risk content segments           | N           | Exists in functions such as politics, terrorism, prohibited, competition, advertising law, See riskSegments parameter |

<span id="matchedLists">Where, the matchedLists structure is as follows:</span>

| **Parameter Name**  | **Type**    | **Parameter Description**                   | **Required** | **Specification**                              |
| ---------- | ---------- | ---------------------------- | ------------ | ---------------------------------------- |
| name       | string     | Client-defined list name             | Y           |                                          |
| words      | json_array | Sensitive word information | Y           | Counting starts from 0, See words parameter |

<span id="words">Where, the words structure is as follows:</span>

| **Parameter Name**  | **Type**    | **Parameter Description**                   | **Required** | **Specification**       |
| ---------- | --------- | -------------- | ------------ | --------------- |
| word       | string    | Sensitive word        | Y           |                 |
| position   | int_array | Position of sensitive word | Y           | Counting starts from 0 |

<span id="riskSegments">Where, the riskSegments structure is as follows:</span>

| **Parameter Name**  | **Type**    | **Parameter Description**                   | **Required** | **Specification**       |
| ---------- | --------- | ---------------------- | ------------ | --------------- |
| segment    | string    | High-risk content segment         | N           |                 |
| position   | int_array | Position of high-risk segment | N           | Counting starts from 0 |

Where, the businessLabels structure is as follows:

| **Parameter Name**  | **Type**    | **Parameter Description**                   | **Required** | **Specification**
| ------------------- | -------- | ------------ | ------------ | ------------ |
| businessLabel1      | string   | First-level business label | N           | First-level business label |
| businessLabel2      | string   | Second-level business label | N           | Second-level business label |
| businessLabel3      | string   | Third-level business label | N           | Third-level business label |
| businessDescription | string   | Chinese label description | N           | Business label description, only for reference when understanding risk reasons, the program should not rely on the value of this parameter for logic processing |

<span id="tokenRiskLabels">Where tokenProfileLabels, tokenRiskLabels have the following structure</span>

| **Parameter Name**  | **Type**    | **Parameter Description**                   | **Required** | **Specification**                               |
| ------------------- | -------- | ------------ | ------------ |----------------------------------------|
| label1      | string   | First-level label     | N           |                                        |
| label2      | string   | Second-level label     | N           |                                        |
| label3      | string   | Third-level label     | N           |                                        |
| description | string   | Label description     | N           | Account label description, only for reference when understanding risk reasons, the program should not rely on the value of this parameter for logic processing |
| timestamp   | int      | Timestamp when labeling | N           | 13-digit Unix timestamp, unit: milliseconds                       |

<span id="speakers">Where speakers is a two-dimensional array, and each element has the following structure</span>

| **Parameter Name**  | **Type**    | **Parameter Description**                   | **Required** | **Specification**                                |
| ------------------- | -------- | ------------ | ------------ |----------------------------------------|
| uid         | int   | Speaker UID     | Y           |                                        |
| volume      | int   | Volume size     | Y           |      The value range is [0, 255]                                  |

The structure of allLabels is as follows:

| **Parameter Name**  | **Type**    | **Parameter Description**                   | **Required** | **Specification**    |
| --------------- | -------- | ------------ | ------------ | ------------ |
| riskLabel1      | string   | First-level risk label | Y           | First-level risk label |
| riskLabel2      | string   | Second-level risk label | Y           | Second-level risk label |
| riskLabel3      | string   | Third-level risk label | Y           | Third-level risk label |
| riskDescription | string   | Risk reason     | Y           | Risk reason, only for reference when understanding risk reasons, the program should not rely on the value of this parameter for logic processing     |

The structure of the outermost auxInfo field is as follows:

| **Parameter Name**  | **Type**    | **Parameter Description**                   | **Required** | **Specification**                                                                                                                                         |
| --------------- | ----------- | ---------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| errorCode       | int         | Status code           | Y           | <p>Status code</p><p>3001: Stream URL access failed, for example, resource HTTP status code 404, 403</p><p>3002: Invalid stream data, for example, "Invalid data found when processing input"</p><p>3003: Stream does not exist, for example, zego returns error code 197612</p><p>3004: The stream did not return audio data</p><p>3005: The pull stream token  |
| streamTime       | int         | Stream review duration           | N           | The duration of review when the stream ends, representing the duration of review. If there is an interval review logic, it may not be consistent with the actual duration of the stream.  |

## Audio Stream Closure Notification API

### Interface Description

This interface is used by the client to notify the server that a particular audio stream has been closed.

### Request URL:

| Cluster   | URL                                                                  | Supported Product List                            |
| ------ | -------------------------------------------------------------------- | ---------------------------------------- |
| Singapore | `http://api-audiostream-xjp.fengkongcloud.com/finish_audiostream/v4` | Chinese audio streams<br/>English audio streams<br/>Arabic audio streams |
| Silicon Valley  | `http://api-audiostream-gg.fengkongcloud.com/finish_audiostream/v4`  | Chinese audio streams                               |
| Shanghai   | `http://api-audiostream-sh.fengkongcloud.com/finish_audiostream/v4`  | Chinese audio streams                               |

### Character Encoding:

`UTF-8`

### Request Method:

`POST`

### Suggested Timeout Duration:

1s

### Request Parameters:

Placed in the HTTP Body, in JSON format. The specific parameters are as follows:

| **Parameter Name**  | **Type**    | **Parameter Description**                   | **Required** | **Specification**                            |
| -------------- | -------- | ------------------ | ------------ | -------------------------------------- |
| accessKey      | string   | Company key          | Y            | Used for permission authentication, provided by NextData when opening the account service |
| requestId      | string   | Unique identifier for this request | Y            | requestId of the audio stream to be closed              |

## Return Parameters

Placed in the HTTP Body, in JSON format. The specific parameters are as follows:

| **Parameter Name**  | **Type**    | **Parameter Description**                   | **Required** | **Specification**                                                                                            |
| ------------------ | ------------ | ------------------------------ | ------------ | --------------------------------------------------------------------------------------------------- |
| requestId          | string       | Unique identifier for this request             | Y           | Request unique identifier                                                                                        |
| code               | int          | Request return code                     | Y           | 1100: Success<br/>1901: QPS exceeded<br/>1902: Invalid parameter<br/>1903: Service failure<br/>9101: Unauthorized operation |
| message            | string       | Request return description, corresponding to the request return code | Y           | Enumeration values: Success, the stream does not exist.                                                                           |

## Demo

### Sample request code：

```bash
curl -v 'http://api-audiostream-bj.fengkongcloud.com/audiostream/v4' -d '{
    "accessKey": "xxxxx",
    "appId": "default",
    "eventId": "default",
    "type": "PORN_AD_POLITICAL_GENDER_TIMBRE_ABUSE_SING_LANGUAGE",
    "callback": "xxxxx",
    "streamType": "NORMAL",
    "acceptLang": "en",
    "data": {
        "btId": "test1",
        "lang": "zh",
        "room": "room2",
        "url": "xxxxx",
        "returnAllText": 1,
        "returnPreText": 1,
        "returnPreAudio": 1,
        "tokenId": "2222"
    }
}'
```

### Sample return code：

```json
{
    "requestId":"b639042cbfe229359e672074762c5583_2",
    "btid":"1637847086612",
    "code":1100,
    "message":"Success",
    "audioDetail":{
        "audioTags":{
            "gender":{
                "label":"Male",
                "probability":99
            },
            "language":[
                {
                    "label":0,
                    "probability":99
                },
                {
                    "label":1,
                    "probability":0
                }
            ],
            "song":0,
            "timbre":[
                {
                    "label":"MiddleAged",
                    "probability":7
                },
                {
                    "label":"Youth",
                    "probability":34
                },
                {
                    "label":"OldMan",
                    "probability":58
                },
                {
                    "label":"LittleBoy",
                    "probability":0
                }
            ]
        },
        "audioText":"Yes Please， I want your help",
        "audioUrl":"https://xjp-voice-mp3-1251671073.cos.ap-beijing.myqcloud.com/MP3%2F20211125%2Fb639042cbfe229359e672074762c5583_2.mp3?q-sign-algorithm=sha1&q-ak=AKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW&q-sign-time=1637847114%3B1637847234&q-key-time=1637847114%3B1637847234&q-header-list=host&q-url-param-list=&q-signature=c0f9a66334bc00e1d92da40974756b9b4b9e7b26",
        "auxInfo":{
            "beginProcessTime":1637847113897,
            "finishProcessTime":1637847114514,
            "room":"test1"
        },
        "businessLabels":[
            {
                "businessDescription":"Minor:Minor:Minor",
                "businessLabel1":"Minor",
                "businessLabel2":"Minor",
                "businessLabel3":"Minor",
                "confidenceLevel":0,
                "probability":0
            }
        ],
        "preAudioUrl":"https://xjp-voice-mp3-1251671073.cos.ap-beijing.myqcloud.com/MP3%2F20211125%2Fb639042cbfe229359e672074762c5583_2_pre.mp3?q-sign-algorithm=sha1&q-ak=AKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW&q-sign-time=1637847114%3B1637847234&q-key-time=1637847114%3B1637847234&q-header-list=host&q-url-param-list=&q-signature=3a261ca2e46b32ec218be69a5802ec0e04c2c627",
        "riskDescription":"normal",
        "riskDetail":{
            "audioText":"Yes Please， I want your help"
        },
        "riskLabel1":"normal",
        "riskLabel2":"normal",
        "riskLabel3":"normal",
        "riskLevel":"REJECT",
        "speakers":[
            [
                {
                    "uid":2,
                    "volume":100
                },
                {
                    "uid":1,
                    "volume":255
                },
                {
                    "uid":3,
                    "volume":50
                }
            ],
            [
                {
                    "uid":2,
                    "volume":200
                },
                {
                    "uid":3,
                    "volume":50
                }
            ],
            [
                {
                    "uid":4,
                    "volume":255
                }
            ]
        ]
    },
    "requestParams":{
        "channel":"default",
        "returnAllText":1,
        "returnPreAudio":1,
        "returnPreText":1,
        "role":"USER",
        "room":"test1",
        "streamType":"DEFAULT",
        "tokenId":"test_audio_v4",
        "url":"http://dyscdnali1.douyucdn.cn/live/288016rlols5.flv?uuid=1637847086612"
    }
}
```

###  Sample request code of finish：

```bash
curl -v 'http://api-audiostream-bj.fengkongcloud.com/finish_audiostream/v4' -d '{
    "accessKey": "xxxxx",
    "requestId": "xxxxx"
}'
```

### Sample return code of finish ：

```json
{
    "code": 1100,
    "message": "Success",
    "requestId": " a78eef377079acc6cdec24967ecde722",
}
```

