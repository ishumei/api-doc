# Shumei Intelligent Video File Recognition Product API Documentation

- - - - -

***All rights reserved. Unauthorized reproduction is prohibited.***

- - - - -
## Video File Upload Request

### Interface Description

This interface is used to submit video-related information, customize frame capture frequency, and other parameters. The recognition results need to be periodically queried by the customer through the query interface.

### Request URL:
| Cluster | URL | Supported Product List |
| --- | --- | --- |
| Beijing | `http://api-video-bj.fengkongcloud.com/v2/saas/anti_fraud/video` | Chinese Video Files |
| Shanghai | `http://api-video-sh.fengkongcloud.com/v2/saas/anti_fraud/video` | Chinese Video Files |
| Singapore | `http://api-video-xjp.fengkongcloud.com/v2/saas/anti_fraud/video` | Chinese Video Files |
| Silicon Valley | `http://api-video-gg.fengkongcloud.com/v2/saas/anti_fraud/video` | Chinese Video Files |

### Request Method:

`POST`

### Character Encoding:

`UTF-8`

### Suggested Timeout:

1s

### Video Format Restrictions:

`AVI`, `FLV`, `MP4`, `MPG`, `WMV`, `MOV`, `WMA`, `RMVB`, `m3u8`

### Video Size Limit:

Less than or equal to 300MB

### Video Duration Limit:

Less than or equal to 2 hours

### Request Parameters:

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| accessKey | string | Company key | Required parameter | Provided and assigned by Shumei |
| appId | string | Application identifier | Required parameter | Used to distinguish applications, needs to contact Shumei for activation, please use the value provided separately by Shumei |
| btId | string | Unique video identifier | Required parameter | Unique video identifier, used to query recognition results, up to 64 characters |
| imgType | string | Regulatory type of the video frames to be recognized, **at least one of imgType or imgBusinessType must be provided** | Optional parameter | Primary regulatory label<br/>Optional values:<br/>`POLITICS`: Political recognition, here POLITICS actually recognizes political figures and terrorism<br/>`PERSON`: Political figure recognition<br/>`VIOLENCE`: Terrorism recognition<br/>`PORN`: Pornography recognition<br/>`AD`: Advertisement recognition<br/>`QR`: QR code recognition<br/>`OCR`: Text risk recognition in images<br/>`BEHAVIOR`: Bad scene recognition, supports smoking, drinking, gambling, drug use, condoms, and meaningless frames<br/>If multiple functions need to be recognized, connect them with underscores, such as `AD_PORN_POLITICS` for combined recognition of advertisements, pornography, and politics |
| audioType | string | Regulatory type of the audio in the video, **at least one of audioType or audioBusinessType must be provided** | Optional parameter | Primary regulatory label<br/>Optional values:<br/>`POLITICS`: Political recognition<br/>`PORN`: Pornography recognition<br/>`AD`: Advertisement recognition<br/>`MOAN`: Moaning recognition<br/>`ABUSE`: Abuse recognition<br/>`ANTHEN`: National anthem recognition<br/>`AUDIOPOLITICAL`: Political audio<br/>`NONE`: Do not detect audio<br/>For combined recognition, connect them with underscores, such as `POLITICS_PORN_MOAN` for combined recognition of advertisements, pornography, and politics |
| imgBusinessType | string | Business type of the video frames to be recognized, **at least one of imgType or imgBusinessType must be provided** | Optional parameter | Optional values refer to [imgBusinessType Optional Values List](#imgbusinesstype可选值列表)<br/> |
| audioBusinessType | String | Business recognition type of the audio in the video, **at least one of audioType or audioBusinessType must be provided** | Optional parameter | Primary business label<br/>Optional values:<br/>`SING`: Singing recognition<br/>`LANGUAGE`: Language recognition (Chinese, English, Cantonese, Tibetan, Uyghur, Korean, Mongolian, others)<br/>`MINOR`: Minor recognition<br/>`GENDER`: Gender recognition<br/>`TIMBRE`: Timbre recognition, must be provided with `GENDER` to be effective |
| callback | string | Specified callback URL address | Optional parameter | When this field is not empty, the service will notify the user of the review results based on this field (supports `http`/`https`) |
| callbackParam | json_object | Callback pass-through field | Optional parameter |  |
| data | json\_object | Information related to this request, up to 1MB | Required parameter | Up to 1MB, [data content as follows](#data) |

<span id="data">Among them, the content of data is as follows:</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| url | string | URL address of the video to be detected | Required parameter | |
| tokenId | string | | Required parameter | Unique identifier of the client user account, used for user behavior analysis, it is recommended to pass in the user UID; up to 40 characters |
| lang | string | Language type | Optional parameter | Optional values:<br/>zh: Chinese<br/>en: English<br/>ar: Arabic<br/>If not provided, Chinese detection is performed by default |
| detectFrequency | int | Frame capture frequency interval, in seconds | Optional parameter | The value range is 1~60s; if not provided, the default is to capture a frame every 5s |
| advancedFrequency | json_object | Advanced frame capture interval, in seconds | Optional parameter | Advanced frame capture settings, if this item is filled in, the default frame capture strategy is invalid<br/>Parameter configuration is as follows<br/>{"durationPoints":[300,600],"frequencies":[1,5,10]}<br/>Meaning:<br/>Video file duration ≤ 300s —— use 1s per frame<br/>300s < Video file duration ≤ 600s —— use 5s per frame<br/>Video file duration > 600s —— use 10s per frame |
| ip | string | Client IP | Optional parameter | Used for user behavior analysis at the IP dimension, and can also be used to compare with Shumei's IP blacklist, supports ipv4 and ipv6 input |
| audioDetectStep | int | Audio review step length in the video file | Optional parameter | The unit is the number, the value range is 1-36 integers, 1 means skipping one 10S audio segment review, 2 means skipping two, and so on. If this function is not used, all audio content will be reviewed |
| retallImg | int | | Optional parameter | Choose the level of video frame capture images to return: 0: Return images with risk levels other than pass 1: Return images of all risk levels, default is 0 |
| retallAudio | int | | Optional parameter | Choose the level of video audio segments to return: 0: Return audio segments with risk levels other than pass 1: Return audio segments of all risk levels, default is 0 |
| videoName | string | Video name | Optional parameter | Video name, used for background interface display |
| subtitle | string | Video subtitles | Optional parameter | Text content review |
| videoCover | string | Video cover | Optional parameter | Video cover image review |
| channel | string | Customer channel (default value if not provided) | Optional parameter |[Channel configuration example reference](#channel)  |
| dataId | string | Data identifier | Optional parameter | |

<span id="advancedFrequency">In data, the content of advancedFrequency is as follows</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| durationPoints | int_array | Video duration interval segmentation | Optional parameter | Used to specify the duration interval of the video file that supports dynamic frame capture frequency, the array can have up to 5 items |
| frequencies | int_array | Frame capture frequency corresponding to the video duration interval | Optional parameter | The setting range is 1~60 seconds, the array can have up to 6 items<br/>Note: The number of items in the frequencies array needs to be one more than the number of items in the durationPoints array, if wrong or empty, an error 1902 will be returned |

<span id="channel">In data, the content of channel is as follows</span>

Shumei configures different channels (channel) according to different business scenarios of customers, formulates targeted interception strategies, and also facilitates customers to filter and analyze data for different business scenarios. The corresponding table of business scenarios and channel values is as follows (supports customer customization)

| **Business Scenario** | **channel Value** | **Transmission Description** |
| --- | --- | --- |
| Short Video | SHORT_VIDEO | Short videos with a duration of minutes |
| Group Chat | GROUP_CHAT | Video messages sent in social group chat scenarios |
| Private Chat | MESSAGE | Video messages sent in social private chat scenarios |
| Recorded Video | VIDEO | Video files generated by video platforms |

### Return Parameters:

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Return Result Parameter Name** | **Parameter Type** | **Parameter Description** | **Is it Required** | **Specification** |
| --- | --- | --- | --- | --- |
| requestId | string | Unique identifier of this request | Yes | Unique request identifier |
| code | int | Request return code | Yes | See [Interface Response Code List](#接口响应码列表) |
| message | string | Request return description | Yes | Detailed description as above |
| btId | string | Unique identifier of the video uploaded by the customer | Yes | Returned only when code=1100, corresponding to the btId field in the request parameters |

## Asynchronous Callback Results

### Interface Description

If the user needs the server to actively callback the video detection results, the callback protocol interface URL callback parameter needs to be specified in the request parameters. The server will actively callback the user after the video review is completed based on this parameter.

### Request Method:

`POST`

### Character Encoding:

`UTF-8`

### Suggested Timeout:

5s

### Supported Protocol:

`HTTP` or `HTTPS`

### Callback Strategy:

When the user receives the push result and returns the HTTP status code 200, it indicates that the push is successful; otherwise, the callback fails, and the system will retry up to 20 times.

### Callback Parameters:

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Parameter Name** | **Type** | **Parameter Description** | **Is it Required** | **Specification** |
| --- | --- | --- | --- | --- |
| checksum | string | A string formed by accessKey + btId + result, generated through the SHA256 algorithm. To prevent tampering, you can generate the string according to this algorithm and do a checksum verification. | Yes |  |
| result | string | Machine review result | Yes | See [result field description](#result) |

<span id="result">result can be deserialized into a json structure, with the content as follows</span>

| **Parameter Name** | **Type** | **Parameter Description** | **Is it Required** | **Specification** |
| --- | --- | --- | --- | --- |
| code | int | Return code | Yes | See [Interface Response Code List](#接口响应码列表) |
| message | string | Return code detailed description | Yes | |
| requestId | string | Unique request identifier | Yes | |
| btId | string | Unique video identifier | Yes | Up to 64 characters |
| riskLevel | string | Risk level, exists when code is 1100 | No | Return value:<br/>`PASS`: Normal content, recommended to pass directly<br/>`REVIEW`: Suspicious content, recommended for manual review<br/>`REJECT`: Violation content, recommended to intercept directly |
| labels | string | Risk labels (exists when code is 1100)| No|  |
| detail | json_array | Risk details | No | See [detail description](#frameDetail) |
| addition | json_object | Audio segment information | No | Exists when code is `1100`, see [addition description](#addition)|
| callbackParam | json_object | Callback pass-through field | No | Exists when the callbackParam field has a value in the submitted request |
| auxInfo | json_object | Auxiliary information | No | Exists when code is `1100`, see [auxInfo description](#auxInfo)|
| tokenProfileLabels | json_array | Account attribute labels | No |Returned only when the function is enabled, see [tokenProfileLabels description](#tokenProfileLabels)|
| tokenRiskLabels | json_array | Account risk labels | No |Returned only when the function is enabled, see [tokenRiskLabels description](#tokenRiskLabels)|

<span id="auxInfo">Among them, the specific content of auxInfo is as follows:</span>

| **Parameter Name** | **Type** | **Parameter Description** | **Is it Required** | **Specification** |
| --- | --- | --- | --- | --- |
| frameCount | int | Number of video frames returned. When retallImg=0, it is the number of risk frames, when retallImg=1, it is the total number of frames | Yes |  |
| billingAudioDuration | float | Duration of the audio in the reviewed video | Yes |  |
| billingImgNum | int | Number of video frames reviewed | Yes |  |
| time | int | Video duration | Yes |  |

<span id="frameDetail">Among them, the specific content of each member in the frame image detail array is as follows:</span>

| **Parameter Name** | **Type** | **Parameter Description** | **Is it Required** | **Specification** |
| --- | --- | --- | --- | --- |
| requestId | string | requestId of the frame segment | Yes |  |
| time | float | Time of the frame in the video file, in seconds | Yes | Time of the frame image relative to the video file |
| similarity | float | Similarity between the current frame image and the previous frame image | Yes |Note: If there is an image, this field will be returned,<br/>The initial first frame of the video file will be compared with a pure black background image |
| imgUrl | string | URL of the current frame | Yes | |
| riskLevel | string | Disposal suggestion for the current frame | Yes | `PASS`: Normal content<br/>`REVIEW`: Suspicious content<br/>`REJECT`: Violation content |
| imgText | string | OCR text content of the frame image | No | OCR text recognition of the frame image, will be available when the recognition type includes OCR |
| qrContent | string | QR code link recognition in the frame image, can be enabled by contacting Shumei| No | Note: After enabling this function, only complete,<br/>QR codes that can be normally recognized will be returned<br/>(imgType needs to include AD) |
| riskType | int | Risk type of the frame image | No | Identifies the risk type, possible values:<br/>0: Normal<br/>100: Political<br/>200: Pornography<br/>210: Sexy<br/>300: Advertisement<br/>310: QR code<br/>320: Watermark<br/>400: Terrorism<br/>500: Violation<br/>510: Bad scene<br/>520: Minor<br/>530: Face<br/>531: Portrait<br/>532: Fake face<br/>533: Appearance<br/>535: Public figure<br/>540: Object<br/>541: Animal<br/>542: Plant<br/>550: Scene<br/>560: Industry violation<br/>570: Image attribute<br/>700: Blacklist<br/>710: Whitelist<br/>800: High-risk account<br/>900: Custom |
| riskSource | int | Risk source | No | Risk source, possible values:<br/>1000: No risk<br/>1001: Text risk <br/>1002: Visual image risk <br/>1003: Audio voice risk |
| description | string | Reason for the risk | Yes | |
| matchedItem | string | Sensitive words hit by the text in the image | No |  |
| matchedList | string | Violation list hit by the text in the image | No |  |
| polityName | string | Name of the recognized political figure (exists when TYPE is PERSON or POLITICS and a political figure is detected) | No |  |
| violenceLabel | string | Classification of the terrorism scene (exists when TYPE is VIOENCE or POLITICS and a terrorism scene is detected) | No |  |
| logos | json_array | Logo results recognized in the video frame<br/>e.g.: "logos": ["douyin"] | No |  |
| businessLabels | json_array | Recognized business labels | No |Returned when imgBusinessType is provided, see [businessLabels description](#businessLabels)   |

<span id="businessLabels">In detail, the content of each member in the businessLabels array is as follows:</span>

| **Parameter Name** | **Type** | **Parameter Description** | **Is it Required** | **Specification** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | Primary label | Yes | Primary label |
| businessLabel2 | string | Secondary label | Yes | Secondary label |
| businessLabel3 | string | Tertiary label | Yes | Tertiary label |
| businessDescription | string | Label description | Yes | Format is "Primary label: Secondary label: Tertiary label" in Chinese |
| confidenceLevel | int | Confidence level | Yes | Optional values are between 0 and 2, the higher the value, the higher the confidence<br/> |
| probability         | float    | Confidence       | Yes           | Optional values are between 0 and 1, the higher the value, the higher the confidence |
| businessDetail | Json_object | Detailed information | Yes |  |

In businessLabels, the content of businessDetail is as follows:

| **Parameter Name** | **Type**   | **Parameter Description** | **Is it Required** | **Specification** |
| ---------- | ---------- | ------------ | ------------ | -------- |
| faces      | json_array | Face information     | No           |          |
| face_num   | int        | Number of faces     | No           |          |
| objects    | json_array | Object information     | No           |          |
| persons    | Json_array | Portrait information     | No           |          |
| person_num | int        | Number of portraits     | No           |          |

In businessDetail, the content of each element in the faces array is as follows:

| **Parameter Name**  | **Type**  | **Parameter Description** | **Is it Required** | **Specification**                                                     |
| ----------- | --------