# Shumei Intelligent Video Stream Recognition Product API Documentation

## Video Stream Upload Request

### Interface Description

This interface is used to submit video stream identification and related information. After the stream is stabilized, the corresponding recognition results will be continuously called back to the specified callback address.

### Request URL:
| Cluster | URL | Supported Product List |
| --- | --- | --- |
| Shanghai | `http://api-videostream-sh.fengkongcloud.com/videostream/v5` | Chinese Video Stream |
| Singapore | `http://api-videostream-xjp.fengkongcloud.com/videostream/v5` | Chinese Video Stream<br/>English Video Stream<br/>Arabic Video Stream |
| Silicon Valley | `http://api-videostream-gg.fengkongcloud.com/videostream/v5` | Chinese Video Stream<br/>English Video Stream<br/>Arabic Video Stream |

### Request Method:

`POST`

### Supported Protocols

`HTTP` or `HTTPS`

### Character Encoding:

`UTF-8`

### Suggested Timeout:

5s

### Request Parameters:

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| accessKey | string | Company Key | Required | Assigned by Shumei |
| appId | string | Application Identifier | Required | Contact Shumei service to activate, use the value provided by Shumei |
| eventId | string | Event Identifier | Required | Used to distinguish scene data, contact Shumei service to activate, use the value provided by Shumei |
| imgType | string | Regulatory type of the video frame to be identified, **at least one of imgType or imgBusinessType must be provided** | Optional | Primary regulatory tags<br/>Optional values:<br/>`POLITY`: Political recognition<br/>`EROTIC`: Pornographic & sexy content recognition<br/>`VIOLENT`: Violent & prohibited content recognition<br/>`QRCODE`: QR code recognition<br/>`ADVERT`: Advertisement recognition<br/>`IMGTEXTRISK`: Image text violation recognition<br/>To recognize multiple functions, connect them with underscores, such as `POLITY_QRCODE_ADVERT` for political, QR code, and advertisement recognition |
| audioType | string | Regulatory type of the audio in the video stream to be identified, **at least one of audioType or audioBusinessType must be provided** | Optional | Primary regulatory tags<br/>Optional values:<br/>`POLITY`: Political recognition<br/>`EROTIC`: Pornographic recognition<br/>`ADVERT`: Advertisement recognition<br/>`DIRTY`: Abusive language recognition<br/>`ADLAW`: Advertisement law<br/>`MOAN`: Moaning recognition<br/>`AUDIOPOLITICAL`: Political audio recognition<br/>`ANTHEN`: National anthem recognition<br/>`NONE`: No audio detection<br/>For combined recognition, connect them with underscores, such as `POLITY_EROTIC` for political and pornographic recognition |
| imgBusinessType | string | Business type of the video frame to be identified, **at least one of imgType or imgBusinessType must be provided** | Optional | Refer to [imgBusinessType Optional Values List](#imgbusinesstype可选值列表)<br/>For multiple functions, connect them with underscores |
| audioBusinessType | string | Business type of the audio in the video stream to be identified, **at least one of audioType or audioBusinessType must be provided** | Optional | Primary business tags<br/>Optional values:<br/>`SING`: Singing recognition<br/>`LANGUAGE`: Language recognition<br/>`MINOR`: Minor recognition<br/>`GENDER`: Gender recognition<br/>`TIMBRE`: Timbre recognition, must be used with `GENDER`<br/>`APPNAME`: App name recognition |
| callback | string | Video callback address | Required | The detection results of video segments in the video stream will be called back to the user through this address |
| data | json_object | Request data content | Required | Maximum 1MB, see [data content below](#data) |

<span id="data">The content of data is as follows:</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| lang | string | Language | Required | Optional values:<br/>`zh`: Chinese<br/>`en`: English<br/>`ar`: Arabic<br/>Default value: `zh` |
| tokenId | string | Unique identifier of the client user account | Required | Used for user behavior analysis, it is recommended to pass in the user UID; maximum 40 characters |
| streamType | string | Video stream type | Required | Optional values:<br/>`NORMAL`: Normal stream address, currently supports `rtmp`, `rtmps`, `hls`, `http`, `https` protocols<br/>, currently only supports normal streams |
| url | string | URL address of the video to be detected | Optional | URL parameter of the stream to be detected (required when streamType is NORMAL) |
| streamName | string | Video stream name | Optional | Used for display on the backend interface, recommended to pass in |
| ip | string | Client IP | Optional | This parameter is used for user behavior analysis at the IP dimension and can also be used to compare with Shumei's IP blacklist |
| returnAllVideo | int | Users can choose to return video segments with different review results according to their needs **(here, the top-level judgment is non-PASS videodetail, which only needs to include non-pass frame images/audio of this video segment)** | Optional | Optional values: (default value is `0`)<br/>`0`: Callback review information of video segments with reject and review results<br/>`1`: Callback review information of all video segments |
| returnFinishInfo | int | When set to 1, a notification will be returned when the stream ends | Optional | Optional values: (default value is `0`)<br/>`1`: Send end notification when review ends<br/>`0`: Do not send end notification when review ends, see [End Stream Return Parameters](#审核结束回调参数) for detailed return parameters |
| detectFrequency | int | Frame capture frequency interval | Optional | Unit is seconds, range is 1~10s, rounded down for decimals, less than 1 is treated as 1s, default is 3s if not provided |
| room | string | Live room/game room number | Optional | Different strategies can be formulated for individual rooms; |
| extra | json_object | Additional information | Optional | See [extra description](#extra) |

<span id="extra">In data, the content of extra is as follows</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| passThrough | json_object | Pass-through field | Optional | The content of this field will be returned as is with the callback result |

### Return Parameters

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Return Result Parameter Name** | **Parameter Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| requestId | string | Unique identifier of this request | Yes | Unique request identifier |
| code | int | Request return code | Yes | See [Interface Response Code List](#接口响应码列表) |
| message | string | Request return description, corresponding to the request return code | Yes | See [Interface Response Code List](#接口响应码列表) |
| detail | json_object | Detailed description | No |  |

The structure of detail is as follows:

| **Parameter Name** | **Parameter Type** | **Parameter Description** | **Is Required** | **Specification** |
| ------------ | ------------ | ------------------------------------------------------------ | ------------ | ---------------- |
| errorCode    | int          | Status code | No | `1001`: Duplicate stream |
| dupRequestId | string       | Indicates the duplicate requestId<br/>When errorCode is 1001, indicating duplicate stream, the dupRequestId field will be returned<br/>For example, if the first request did not receive a return, but the audio stream has actually started to be reviewed, and there is no requestId to actively close the review<br/>You can request again, receive duplicate stream information, and call the close review interface with the returned dupRequestId | No |                  |

## Asynchronous Callback Results

### Interface Description

If the user needs the server to actively callback the video detection results, the callback protocol interface URL callback parameter needs to be specified in the request parameters. The server will actively callback the user after the video review is completed.

### Request Method:

`POST`

### Character Encoding:

`UTF-8`

### Suggested Timeout:

5s

### Callback Strategy

When the user receives the push result and returns the HTTP status code 200, it indicates a successful push; otherwise, the system will push up to 20 times.

### Callback Parameters

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name** | **Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| requestId | string | Unique identifier of this request | Yes | Unique request identifier |
| code | int | Request return code | Yes | See [Interface Response Code List](#接口响应码列表) |
| message | string | Request return description, corresponding to the request return code | Yes | See [Interface Response Code List](#接口响应码列表) |
| statCode | int | Callback status code | No |Status code correspondence:<br/>0: Review result callback<br/>1: Stream end result callback|
| riskLevel | string | Disposal suggestion for the current video segment | Yes |Returned when code equals 1100<br/>`PASS`: Normal content<br/>`REVIEW`: Suspicious content<br/>`REJECT`: Violation content|
| riskLabel1 | string | Each primary tag is parallel, when riskLevel is `PASS`, return `normal` | Yes |Primary tag|
| riskLabel2 | string | Secondary tags belong to primary tags, when riskLevel is `PASS`, it is empty | Yes |Secondary tag|
| riskLabel3 | string | Tertiary tags belong to secondary tags, when riskLevel is `PASS`, it is empty | Yes |Tertiary tag|
| videoDetail | json_object | Risk video segment information | No | Returned when there are risk segments or returnAllvideo=1, see [videoDetail description](#videoDetail) |
| tokenProfileLabels | json_array | Account attribute tags | No | Returned only when the function is enabled, see [tokenProfileLabels description](#tokenProfileLabels) |
| tokenRiskLabels | json_array | Account risk tags | No | Returned only when the function is enabled, see [tokenRiskLabels description](#tokenRiskLabels) |
| auxInfo | json_object | Auxiliary information | No | Returned when code equals `1100`, see [auxInfo description](#auxInfo) |

<span id="auxInfo">The specific content of auxInfo is as follows:</span>

| Parameter Name | Type | Parameter Description | Is Required | Specification |
| :---------- | :---------- | :--------------- | :------- | :----------------------------------------------------------- |
| passThrough | json_object | Pass-through field | No | The content of this field is the same as the value of passThrough in the extra of the request parameter data |
| frameCount  | int         | Number of frames captured in the video segment | Yes       | Number of frames captured in the returned video. When returnAllvideo=0, it is the number of risk frames, when returnAllvideo=1, it is the total number of frames |

<span id="videoDetail">The specific content of videoDetail is as follows:</span>

| Parameter Name | Type | Parameter Description | Is Required | Specification |
| :---------- | :--------- | :--------------------------- | :------- | :---------------------------------- |
| audioDetail | json_array | Audio segment information | Yes       |                                     |
| endTime     | float      | End time of the video segment (absolute time) | Yes       |                                     |
| frameDetail | json_array | Frame image information | Yes       | See [frameDetail description](#frameDetail) |
| startTime   | float      | Start time of the video segment (absolute time) | Yes       |                                     |
| videoUrl    | string     | URL of the current video segment | Yes       |                                     |

<span id="frameDetail">The specific content of each member in the frameDetail array is as follows:</span>

| **Parameter Name** | **Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| time | float | Time of the frame in the video file, in seconds | Yes | Time of the frame image relative to the video file |
| requestId | string | Unique identifier of the current frame segment | Yes |  |
| imgUrl | string | URL of the current frame | No | |
| imgText | string | OCR text content of the frame image | No | OCR text recognition of the frame image, returned only when ADVERT or IMGTEXTRISK is passed in |
| riskLevel | string | Disposal suggestion for the current frame | No | `PASS`: Normal content<br/>`REVIEW`: Suspicious content<br/>`REJECT`: Violation content |
| riskLabel1 | string | Each primary tag is parallel, when riskLevel is `PASS`, return `normal` | No | Primary tag |
| riskLabel2 | string | Secondary tags belong to primary tags, when riskLevel is `PASS`, it is empty | No | Secondary tag |
| riskLabel3 | string | Tertiary tags belong to secondary tags, when riskLevel is `PASS`, it is empty | No | Tertiary tag |
| riskDescription | string | Tag explanation | No | When hitting the user-defined list, return: `Hit user-defined list`; when riskLevel is `PASS`, return: `Normal`; other situations are displayed as the Chinese name of the primary tag: secondary tag: tertiary tag |
| riskDetail | json_object | Risk detail information | No | See [riskDetail description](#riskDetail) |
| allLabels | json_array | List of all risk tags | No | See [allLabels description](#allLabels) |
| businessLabels | json_array | Business tag list, returned when imgBusinessType is passed in | No | See [businessLabels description](#businessLabels) |

<span id="riskDetail">The content of riskDetail in frameDetail is as follows:</span>

| **Parameter Name** | **Type**    | **Parameter Description** | **Is Required** | **Specification**                                                     |
| ---------- | ----------- | ------------ | ------------ | ------------------------------------------------------------ |
| riskSource | int         | Risk source     | Yes           | Optional values:<br/>`1000`: No risk<br/>`1001`: Text risk<br/>`1002`: Visual risk<br/>`1003`: Audio risk |
| faces      | json_array  | Face information     | No           | Returns the name and location information of political figures in the image, see [faces description](#faces)  |
| face_num   | int         | Number of faces     | No           |                                                              |
| objects    | json_array  | Object information     | No           | Returns the name and location information of signs or objects in the image, see [objects description](#objects) |
| persons    | json_array  | Portrait information     | No           | Only when hitting multiple portraits, the array elements will have multiple, up to 10              |
| person_num | int         | Number of portraits     | No           |                                                              |
| ocrText    | json_object | Text information     | No           | Returns text-related information in the image, see [ocrText description](#ocrText)          |

<span id="faces">The specific content of each member in the faces array in riskDetail is as follows:</span>

| **Parameter Name**  | **Type**  | **Parameter Description** | **Is Required** | **Specification**                                                     |
| ----------- | --------- | ------------ | ------------ | ------------------------------------------------------------ |
| id          | string    | ID         | Yes           |                                                              |
| name        | string    | Name         | No           |                                                              |
| location    | int_array | Location coordinates     | No           | This array has four values, representing the coordinates of the upper left corner and the lower right corner respectively. For example, [207,522,340,567]<br/>207 represents the x-coordinate of the upper left corner<br/>522 represents the y-coordinate of the upper left corner<br/>340 represents the x-coordinate of the lower right corner<br/>567 represents the y-coordinate of the lower right corner |
| probability | float     | Confidence       | No           | Optional values are between 0 and 1, the larger the value, the higher the confidence                         |
| face_ratio  | float     | Face ratio     | No           |                                                              |

<span id="objects">The specific content of each member in the objects array in riskDetail is as follows:</span>

| **Parameter Name**  | **Type**  | **Parameter Description** | **Is Required** | **Specification**                                                     |
| ----------- | --------- | ------------ | ------------ | ------------------------------------------------------------ |
| id          | string    | ID         | Yes           |                                                              |
| name        | string    | Name         | No           |                                                              |
| location    | int_array | Location coordinates     | No           | This array has four values, representing the coordinates of the upper left corner and the lower right corner respectively. For example, [207,522,340,567]<br/>207 represents the x-coordinate of the upper left corner<br/>522 represents the y-coordinate of the upper left corner<br/>340 represents the x-coordinate of the lower right corner<br/>567 represents the y-coordinate of the lower right corner |
| probability | float     | Confidence       | No           | Optional values are between 0 and 1, the larger the value, the higher the confidence                         |
| qrContent   | string    | QR code information   | No           |                                                              |

The content of each element in the persons array in riskDetail is as follows:

| **Parameter Name**   | **Type**  | **Parameter Description** | **Is Required** | **Specification**                                                     |
| ------------ | --------- | ------------ | ------------ | ------------------------------------------------------------ |
| id           | string    | ID         | Yes           |                                                              |
| location     | int_array | Location coordinates     | No           | This array has four values, representing the coordinates of the upper left corner and the lower right corner respectively. For example, [207,522,340,567]<br/>207 represents the x-coordinate of the upper left corner<br/>522 represents the y-coordinate of the upper left corner<br/>340 represents the x-coordinate of the lower right corner<br/>567 represents the y-coordinate of the lower right corner |
| probability  | float     | Confidence       | No           | Optional values are between 0 and 1, the larger the value, the higher the confidence                         |
| person_ratio | float     | Portrait ratio     |