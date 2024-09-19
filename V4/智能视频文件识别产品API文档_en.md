# Shumei Intelligent Video File Recognition Product API Documentation

## Video File Upload Request

### Interface Description

This interface is used to submit video-related information, customize frame capture frequency, and other parameters. The recognition results need to be periodically retrieved by the customer through the query interface.

### Request URL:
| Cluster | URL |
| --- | --- |
| Beijing | `http://api-video-bj.fengkongcloud.com/video/v4` |
| Shanghai | `http://api-video-sh.fengkongcloud.com/video/v4` |
| Singapore | `http://api-video-xjp.fengkongcloud.com/video/v4` |
| Silicon Valley | `http://api-video-gg.fengkongcloud.com/video/v4` |

### Request Method:

`POST`

### Character Encoding:

`UTF-8`

### Suggested Timeout:

7s

### Video Format Restrictions:

`AVI`, `FLV`, `MP4`, `MPG`, `WMV`, `MOV`, `WMA`, `RMVB`, `m3u8`

### Video Size Limit:

Less than or equal to 300MB

### Video Duration Limit:

Less than or equal to 2 hours

### Request Parameters:

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| accessKey | string | Company key | Required parameter | Provided and assigned by Shumei |
| appId | string | Application identifier | Required parameter | Used to distinguish applications, needs to contact Shumei service for activation, use the value provided separately by Shumei |
| eventId | string | Event identifier | Required parameter | Used to distinguish scene data, needs to contact Shumei service for activation, use the value provided separately by Shumei |
| imgType | string | Regulatory type of the video frame to be recognized, **at least one of imgType or imgBusinessType must be provided** | Optional parameter | Primary regulatory label<br/>Optional values:<br/>`POLITY`: Political recognition<br/>`EROTIC`: Pornographic & sexy violation recognition<br/>`VIOLENT`: Violent & prohibited recognition<br/>`QRCODE`: QR code recognition<br/>`ADVERT`: Advertisement recognition<br/>`IMGTEXTRISK`: Image text violation recognition<br/>If multiple functions need to be recognized, connect them with an underscore, such as `POLITY_QRCODE_ADVERT` for political, QR code, and advertisement combined recognition |
| audioType | string | Regulatory type of the audio in the video to be recognized, **at least one of audioType or audioBusinessType must be provided** | Optional parameter | Primary regulatory label<br/>Optional values:<br/>`POLITY`: Political recognition<br/>`EROTIC`: Pornographic recognition<br/>`ADVERT`: Advertisement recognition<br/>`DIRTY`: Abusive recognition<br/>`ADLAW`: Advertisement law<br/>`MOAN`: Moaning recognition<br/>`AUDIOPOLITICAL`: Political audio recognition<br/>`ANTHEN`: National anthem recognition<br/>`NONE`: No audio detection<br/>For combined recognition, connect them with an underscore, such as `POLITY_EROTIC` for political and pornographic recognition |
| imgBusinessType | string | Business type of the video frame to be recognized, **at least one of imgType or imgBusinessType must be provided** | Optional parameter | Optional values refer to [imgBusinessType Optional Values List](#imgbusinesstype可选值列表)<br/>If multiple functions need to be recognized, connect them with an underscore |
| audioBusinessType | String | Business recognition type of the audio in the video, **at least one of audioType or audioBusinessType must be provided** | Optional parameter | Primary business label<br/>Optional values:<br/>`SING`: Singing recognition<br/>`LANGUAGE`: Language recognition (Chinese, English, Cantonese, Tibetan, Uighur, Korean, Mongolian, others)<br/>`MINOR`: Minor recognition<br/>`GENDER`: Gender recognition<br/>`TIMBRE`: Timbre recognition, must be used with `GENDER` to be effective<br/>If multiple functions need to be recognized, connect them with an underscore |
| callback | string | Specified callback URL address | Optional parameter | When this field is not empty, the service will notify the user of the review results based on this field (supports `http`/`https`) |
| data | json_object | Information related to this request, up to 1MB | Required parameter | Up to 1MB, see [data content below](#data) |

<span id="data">The content of data is as follows:</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| btId | string | Unique identifier of the video | Required parameter | Unique identifier of the video, used to query recognition results, up to 64 characters |
| url | string | URL address of the video to be detected | Required parameter | |
| tokenId | string | | Required parameter | Unique identifier of the client user account, used for user behavior analysis, it is recommended to pass in the user UID; up to 40 characters |
| lang | string | Language type | Optional parameter | Optional values:<br/>zh: Chinese<br/>en: English<br/>ar: Arabic<br/>If not provided, Chinese detection is performed by default |
| detectFrequency | int | Frame capture frequency interval, in seconds | Optional parameter | The value range is 1~60s; if not provided, the default is to capture a frame every 5s |
| advancedFrequency | json_object | Advanced frame capture interval, in seconds | Optional parameter | Advanced frame capture settings, if this item is filled in, the default frame capture strategy is invalid<br/>Parameter configuration is as follows<br/>{"durationPoints":[300,600],"frequencies":[1,5,10]}<br/>Meaning:<br/>Video file duration ≤ 300s —— use 1s per frame<br/>300s < Video file duration ≤ 600s —— use 5s per frame<br/>Video file duration > 600s —— use 10s per frame |
| ip | string | Client IP | Optional parameter | Used for user behavior analysis at the IP dimension, and can also be used to compare with Shumei's IP blacklist, supports ipv4 and ipv6 input |
| audioDetectStep | int | Audio review step length in the video file | Optional parameter | The unit is the number, the value range is 1-36 integers, 1 means skipping one 10S audio segment review, 2 means skipping two, and so on. If this function is not used, all audio content will be reviewed |
| returnAllImg | int | | Optional parameter | Choose the level of video frame images to return: 0: return images with risk levels other than pass; 1: return images of all risk levels. The default is 0 |
| returnAllAudio | int | | Optional parameter | Choose the level of video audio segments to return: 0: return audio segments with risk levels other than pass; 1: return audio segments of all risk levels. The default is 0 |
| videoTitle | string | Video name | Optional parameter | Video name, used for background interface display |
| extra | json_object | Extended information | Optional parameter | See [extra description](#extra) |
| dataId | string | Data identifier | Optional parameter |  |

<span id="advancedFrequency">The content of advancedFrequency in data is as follows</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| durationPoints | int_array | Video duration interval division | Optional parameter | Used to specify the duration interval of the video file that supports dynamic frame capture frequency, the array can have up to 5 values |
| frequencies | int_array | Frame capture frequency corresponding to the video duration interval | Optional parameter | The range can be set to 1~60 seconds, the array can have up to 6 values<br/>Note: The number of values set in the frequencies array needs to be one more than the number of values in the durationPoints array, if wrong or empty, an error 1902 will be returned |

<span id="extra">The content of extra in data is as follows</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| passThrough | json_object | Pass-through field | Optional parameter | The content of this field will be returned as is with the callback result |

### Return Parameters:

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Return Result Parameter Name** | **Parameter Type** | **Parameter Description** | **Is Required** | **Specification** |
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

### Supported Protocols:

`HTTP` or `HTTPS`

### Callback Strategy:

When the user receives the push result and returns the HTTP status code 200, it indicates a successful push; otherwise, the callback fails, and the system will retry up to 20 times.

### Callback Parameters:

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name** | **Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| code | int | Return code | Yes | See [Interface Response Code List](#接口响应码列表) |
| message | string | Return code detailed description | Yes | |
| requestId | string | Unique request identifier | Yes | |
| btId | string | Unique identifier of the video | Yes | Up to 64 characters |
| riskLevel | string | Risk level, exists when code is 1100 | No | Return values:<br/>`PASS`: Normal content, recommended to pass directly<br/>`REVIEW`: Suspicious content, recommended for manual review<br/>`REJECT`: Violation content, recommended to block directly |
| frameDetail | json_array | Risk details | No | Returned when there are risk segments or returnAllImg=1, see [frameDetail description](#frameDetail) |
| audioDetail | json_array | Audio segment information | No | Returned when there are risk segments or returnAllAudio=1, see [audioDetail description](#audioDetail) |
| auxInfo | json_object | Auxiliary information | No | Exists when code is `1100`, see [auxInfo description](#auxInfo) |
| tokenProfileLabels | json_array | Account attribute labels | No | Returned only when the function is enabled, see [tokenProfileLabels description](#tokenProfileLabels) |
| tokenRiskLabels | json_array | Account risk labels | No | Returned only when the function is enabled, see [tokenRiskLabels description](#tokenRiskLabels) |
<span id="auxInfo">The specific content of auxInfo is as follows:</span>

| **Parameter Name** | **Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| frameCount | int | Number of video frames returned. When returnAllImg=0, it is the number of risk frames, when returnAllImg=1, it is the total number of frames | Yes |  |
| billingAudioDuration | float | Duration of the audio in the reviewed video | Yes |  |
| billingImgNum | int | Number of video frames reviewed | Yes |  |
| time | int | Video duration | Yes |  |
| passThrough | json_object | Pass-through field, the content of this field is the same as the value of passThrough in the extra of the request parameter data | No |  |

<span id="frameDetail">The specific content of each member in the frameDetail array is as follows:</span>

| **Parameter Name** | **Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| time | float | Time of the frame in the video file, in seconds | Yes | Time of the frame relative to the video file |
| requestId | string | Unique identifier of the current frame segment | Yes | |
| imgUrl | string | URL of the current frame | Yes | |
| imgText | string | OCR text content of the frame image | No | OCR text recognition of the frame image, will be present when the recognition type includes OCR |
| riskLevel | string | Disposal suggestion for the current frame | Yes | `PASS`: Normal content<br/>`REVIEW`: Suspicious content<br/>`REJECT`: Violation content |
| riskLabel1 | string | The primary labels are parallel to each other, when riskLevel is `PASS`, it returns `normal` | Yes | Primary label |
| riskLabel2 | string | The secondary label belongs to the primary label, when riskLevel is `PASS`, it is empty | Yes | Secondary label |
| riskLabel3 | string | The tertiary label belongs to the secondary label, when riskLevel is `PASS`, it is empty | Yes | Tertiary label |
| riskDescription | string | Label explanation | Yes | When hitting the user-defined list, it returns: `Hit user-defined list`; when riskLevel is `PASS`, it returns: `Normal`; other situations are displayed as the Chinese name of the primary label: secondary label: tertiary label |
| riskDetail | json_object | Risk detail information | Yes | See [riskDetail description](#riskDetail) |
| allLabels | json_array | List of all risk labels | Yes | List of all risk labels, see [allLabels description](#allLabels) |
| businessLabels | json_array | Business label list | No | Returned when imgBusinessType is provided, see [businessLabels description](#businessLabels) |
| auxInfo | json_object | Auxiliary information | Yes | Some auxiliary information is placed here, see [auxInfo description](#auxInfo3) |

<span id="auxInfo3">The content of auxInfo in frameDetail is as follows:</span>

| **Parameter Name** | **Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| qrContent | string | QR code link recognition in the frame image | No | QR code link recognition in the frame image, can be enabled by contacting Shumei<br/>Note: After enabling this function, only complete and normally recognizable QR codes will be returned, and imgType must include `AD` |
| similarity | float | Similarity between the current frame image and the previous frame image | Yes | If there is an image, this field will be returned, the initial first frame of the video file will be compared with a pure black background image |

<span id="riskDetail">The content of riskDetail in frameDetail is as follows:</span>

| **Parameter Name** | **Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| riskSource | int | Risk source | Yes | Optional values:<br/>`1000`: No risk<br/>`1001`: Text risk<br/>`1002`: Visual risk<br/>`1003`: Audio risk |
| faces | json_array | Face information | No | Returns the name and location information of political figures in the image, see [faces description](#faces) |
| face_num | int | Number of faces | No |  |
| objects | json_array | Object information | No | Returns the name and location information of signs or objects in the image, see [objects description](#objects) |
| persons | json_array | Portrait information | No |  |
| person_num | int | Number of portraits | No |  |
| ocrText | json_object | Text information | No | Returns text-related information in the image, see [ocrText description](#ocrText) |

<span id="faces">The content of each element in the faces array in riskDetail is as follows:</span>

| **Parameter Name**  | **Type**  | **Parameter Description** | **Is Required** | **Specification**                                                     |
| ----------- | --------- | ------------ | ------------ | ------------------------------------------------------------ |
| id          | string    | Identifier         | Yes           |                                                              |
| name        | string    | Name         | No           |                                                              |
| location    | int_array | Location coordinates     | No           | This array has four values, representing the coordinates of the top-left corner and the bottom-right corner respectively. For example, [207,522,340,567]<br/>207 represents the x-coordinate of the top-left corner<br/>522 represents the y-coordinate of the top-left corner<br/>340 represents the x-coordinate of the bottom-right corner<br/>567 represents the y-coordinate of the bottom-right corner |
| probability | float     | Confidence       | No           | Optional values are between 0 and 1, the larger the value, the higher the confidence                         |
| face_ratio  | float     | Face ratio     | No           |                                                              |

<span id="objects">The content of each element in the objects array in riskDetail is as follows:</span>

| **Parameter Name**  | **Type**  | **Parameter Description** | **Is Required** | **Specification**                                                     |
| ----------- | --------- | ------------ | ------------ | ------------------------------------------------------------ |
| id          | string    | Identifier         | Yes           |                                                              |
| name        | string    | Name         | No           |                                                              |
| location    | int_array | Location coordinates     | No           | This array has four values, representing the coordinates of the top-left corner and the bottom-right corner respectively. For example, [207,522,340,567]<br/>207 represents the x-coordinate of the top-left corner<br/>522 represents the y-coordinate of the top-left corner<br/>340 represents the x-coordinate of the bottom-right corner<br/>567 represents the y-coordinate of the bottom-right corner |
| probability | float     | Confidence       | No           | Optional values are between 0 and 1, the larger the value, the higher the confidence                         |
| qrContent   | string    | QR code information   | No           |                                                              |

The content of each element in the persons array in riskDetail is as follows:

| **Parameter Name**   | **Type**  | **Parameter Description**