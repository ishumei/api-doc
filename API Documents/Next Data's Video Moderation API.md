# Next Data's Video Moderation API

## Video File Upload Request

### Interface Description

This interface is used to submit video-related information, customize frame capture frequency and other parameters. The recognition result requires the customer to regularly call the query interface to obtain.

### Request URL:
| Computer cluster | URL |
| --- | --- |
| Beijing | `http://api-video-bj.fengkongcloud.com/video/v4` |
| Shanghai | `http://api-video-sh.fengkongcloud.com/video/v4` |
| Singapore | `http://api-video-xjp.fengkongcloud.com/video/v4` |

### Request Method:

`POST`

### Character Encoding:

`UTF-8`

### Suggested Timeout:

7s

### Video Format Limitations:

`AVI`、`FLV`、`MP4`、`MPG`、`WMV`、`MOV`、`WMA`、`RMVB`、`m3u8`

### Video Size Limitations:

Less than or equal to 300MB

### Video Duration Limitations:

Less than or equal to 2 hours

### Request Parameters:

Placed in the HTTP Body, using Json format, specific parameters are as follows:

| ***Request Parameter*** | ***Type***  | ***Description***                                            | ***Required*** | ***Specification***                                          |
| ----------------------- | ----------- | ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| accessKey               | string      | Company key                                                  | Y              | Provided by NEXT DATA and allocated by NEXT DATA             |
| appId                   | string      | Application identifier                                       | Y              | Used to distinguish applications. Contact NEXT DATA for service activation. Please use the value provided by NEXT DATA separately |
| eventId                 | string      | Event identifier                                             | Y              | Used to distinguish scenario data. Contact NEXT DATA for service activation. Please use the value provided by NEXT DATA separately |
| imgType                 | string      | Regulatory types that need to be identified in the video, ***\*at least one of imgType and imgBusinessType needs to be passed\**** | N              | Level one label of regulation <br/>Optional values: <br/>`POLITY`: ***Political recognition<br/>***`EROTIC`: ***Pornographic and sexy violation recognition<br/>***`VIOLENT`***: Violence and prohibited recognition<br/>***`QRCODE`***: QR code recognition<br/>***`ADVERT`***: Advertisement recognition<br/>***`IMGTEXTRISK`: ***Recognition of inappropriate text in images<br/>***If multiple functions need to be recognized, connect them with underscores, such as **POLITY_QRCODE_ADVERT** for combination recognition of political, QR code, and advertisement |
| audioType               | string      | Regulatory types that need to be identified in the audio of the video | N              | Level one label of regulation <br/>Optional values: <br/>`POLITICS`***: Political recognition<br/>***`PORN`***: Pornographic recognition<br/>***`AD`***: Advertisement recognition<br/>***`MOAN`***: Moaning recognition<br/>***`ABUSE`***: Abuse recognition<br/>***`ANTHEN`***: National anthem recognition<br/>***`AUDIOPOLITICAL`***: Audio political<br/>***`NONE`: ***No audio detection<br/>***If combination recognition is required, connect them with underscores, such as **POLITICAL_PORN_MOAN** for advertisement, pornography, and political recognition |
| imgBusinessType         | string      | Business types that need to be identified in the video, ***\*at least one of imgType and imgBusinessType needs to be passed\**** | N              | Optional values refer to [imgBusinessType optional value list](#imgBusinessType optional value list) <br/>If multiple functions need to be recognized, connect them with underscores |
| audioBusinessType       | String      | Business recognition types of audio in the video             | N              | Level one label of business <br/>Optional values: <br/>`SING`***: Singing recognition<br/>***`LANGUAGE`***: Language recognition (Chinese, English, Cantonese, Tibetan, Uyghur, Korean, Mongolian, other)<br/>***`MINOR`: ***Minor recognition<br/>***`GENDER`: ***Gender recognition<br/>***`TIMBRE`: ***Tone recognition, <br/>***`GENDER` needs to be passed at the same time to take effect<br/>If multiple functions need to be recognized, connect them with underscores |
| callback                | string      | Specify the callback URL address                             | N              | When this field is not empty, the service will notify the user of the audit result based on this field callback (supports *http/https*) |
| data                    | json_object | Related information of this request, up to 1MB               | Y              | Up to 1MB, where [data content is as follows](#data)         |
| acceptLang              | string      | The language type of the label                               | Y              | The language type of the label<br/>Optional values:<br/>zh:Chinese<br/>en:English |

<span id="data">Where the content of data is as follows:</span>

| ***Request Parameter*** | ***Type***  | ***Description***                            | ***Input Requirement*** | ***Specification***                                          |
| ----------------------- | ----------- | -------------------------------------------- | ----------------------- | ------------------------------------------------------------ |
| btId                    | string      | Unique identifier of the video               | Y                       | Unique identifier of the video, used to query recognition results, up to 64 bits |
| url                     | string      | URL address of the video to be detected      | Y                       |                                                              |
| tokenId                 | string      |                                              | Y                       | Unique identifier of the client user account, used for user behavior analysis, it is recommended to pass in the user UID; up to 40 bits |
| lang                    | string      | Language type                                | N                       | Optional values: <br/>`zh`: Chinese<br/>`en`: English<br/>`ar`: Arabic<br/>Not passing defaults to Chinese detection |
| detectFrequency         | float       | Frame capture frequency interval, in seconds | N                       | Value range is 0.5~60s; if not passed, the default is to capture one frame every 5 seconds |
| advancedFrequency       | json_object | Advanced frame capture interval, in seconds  | N                       | Advanced frame capture settings, if this item is filled in, the default frame capture strategy is invalid <br/>Parameter configuration is as follows <br/>{"durationPoints":[300,600],"frequencies":[1,5,10]}<br/>Meaning:<br/>Video file duration ≤ 300s —— Choose 1s frame capture <br/>300s < video file duration ≤ 600s —— Choose 5s frame capture <br/>Video file duration > 600s —— Choose 10s frame capture |
| ip                      | string      | Client IP                                    | N                       | Used for user behavior analysis at the IP dimension, and can also be used to compare NEXT DATA‘s IP blacklists |
| audioDetectStep         | int         | Audio audit step in the video file           | N                       | Unit is a number, value range is 1-36 integers. Taking 1 means skipping an audio segment of 10 seconds for review, taking 2 means skipping two, and so on. When not using this feature, all audio content passes the review |
| returnAllImg            | int         |                                              | N                       | Select the level of video frame images to be returned: 0: return images with a risk level of non-pass; 1: return images of all risk levels. The default is 0 |
| returnAllAudio          | int         |                                              | N                       | Select the level of video audio segments to be returned: 0: return audio segments with a risk level of non-pass; 1: return audio segments of all risk levels. The default is 0 |
| videoTitle              | string      | Video name                                   | N                       | Video name, used for backend display                         |
| extra                   | json_object | Extended information                         | N                       | See [extra description](#extra)                              |

<span id="advancedFrequency">In the data, the content of advancedFrequency is as follows:</span>

| ***Request Parameter*** | ***Type*** | ***Description***                                            | ***Input Requirement*** | ***Specification***                                          |
| ----------------------- | ---------- | ------------------------------------------------------------ | ----------------------- | ------------------------------------------------------------ |
| durationPoints          | Object[]   | Segmentation of video duration intervals                     | N                       | Used to specify the duration interval supported by dynamic frame capture of the video file, with up to 5 arrays |
| frequencies             | Object[]   | Frame capture frequency corresponding to the duration interval of the video | N                       | The setting range can be from 0.5 to 60 seconds, and up to 6 arrays can be used. <br/>Explanation: The number of frequencies set in the array needs to be one more than the number of durationPoints arrays. An error will be returned if it is passed incorrectly or left empty (error code 1902). |

<span id="extra">In the data, the content of extra is as follows:</span>

| ***Request Parameter*** | ***Type***  | ***Description***  | ***Input Requirement*** | ***Specification***                                          |
| ----------------------- | ----------- | ------------------ | ----------------------- | ------------------------------------------------------------ |
| passThrough             | json_object | Pass-through field | N                       | The content of this field will be returned as is along with the callback result |

### **Return Parameters:**

Placed in HTTP Body, using JSON format, specific parameters are as follows:

| ***Returned Result Parameter*** | ***Parameter Type*** | ***Parameter Description***                           | ***Required Return*** | ***Specification***                                          |
| ------------------------------- | -------------------- | ----------------------------------------------------- | --------------------- | ------------------------------------------------------------ |
| requestId                       | string               | Unique identifier for this request                    | Y                     | Request unique identifier                                    |
| code                            | int                  | Request return code                                   | Y                     | See [Interface Response Code List](#Interface-Response-Code-List) |
| message                         | string               | Description of request return                         | Y                     | Details described above                                      |
| btId                            | string               | Unique identifier of the video uploaded by the client | Y                     | Only returned when code=1100, corresponding to the btId field in the request parameters |

## Asynchronous Callback Result

### Interface Description

If users need the server to actively callback the video detection results, they need to specify the callback protocol interface URL parameter in the request parameters. The server will actively callback the user after the video review is completed based on this parameter.

### Request Method:

`POST`

### Character Encoding:

`UTF-8`

### Recommended Timeout Time:

5s

### Supported Protocols:

`HTTP`或`HTTPS`

### Callback Policy:

When users receive the push results and return an HTTP status code of 200, it means that the push was successful. Otherwise, the callback fails, and the system will retry the push up to 20 times.

### Callback Parameters:

Placed in HTTP Body, using JSON format, specific parameters are as follows:

| ***Parameter Name*** | ***Type***  | ***Description***                    | ***Required*** | ***Specification***                                          |
| -------------------- | ----------- | ------------------------------------ | -------------- | ------------------------------------------------------------ |
| code                 | int         | Return code                          | Y              | See [Interface Response Code List](#interface-response-code-list) for details |
| message              | string      | Return code description              | Y              |                                                              |
| requestId            | string      | Request unique identifier            | Y              |                                                              |
| btId                 | string      | Video unique identifier              | Y              | Up to 64 characters                                          |
| riskLevel            | string      | Risk level, exists when code is 1100 | N              | Return value:<br/>`PASS`***: Normal content, recommended to be released directly<br/>***`REVIEW`***: Suspicious content, recommended for manual review<br/>***`REJECT`: ***Violation content, recommended to be blocked directly*** |
| frameDetail          | json_array  | Risk details                         | N              | Returned when there are risk segments or returnAllImg=1, see [frameDetail description](#frameDetail) for details |
| audioDetail          | json_array  | Audio segment information            | N              | Returned when there are risk segments or returnAllAudio=1, see [audioDetail description](#audioDetail) for details |
| auxInfo              | json_object | Auxiliary information                | N              | Exists when code is `1100`, see [auxInfo description](#auxInfo) for details |
| tokenProfileLabels   | json_array  | Account attribute labels             | N              | Returned only when the function is enabled, see [tokenProfileLabels description](#tokenProfileLabels) for details |
| tokenRiskLabels      | json_array  | Account risk labels                  | N              | Returned only when the function is enabled, see [tokenRiskLabels description](#tokenRiskLabels) for details |

<span id="auxInfo">Specific content of auxInfo:</span>

| ***Parameter Name*** | ***Type***  | ***Description***                                            | ***Required*** | ***Specification*** |
| -------------------- | ----------- | ------------------------------------------------------------ | -------------- | ------------------- |
| frameCount           | int         | Number of video frames returned. When returnAllImg=0, it is the number of risks. When returnAllImg=1, it is the total number | Y              |                     |
| time                 | int         | Video duration                                               | Y              |                     |
| passThrough          | json_object | Pass-through field, the content of which is the same as the value of passThrough in data.extra of the request parameters | N              |                     |

<span id="frameDetail">The specific contents of each member in the frameDetail array are as follows:</span>

| ***Parameter Name*** | ***Data Type*** | ***Parameter Description***                                  | ***Required*** | ***Specifications***                                         |
| -------------------- | --------------- | ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| time                 | float           | The time when the frame is captured in the video file, in seconds | Y              | The time when the frame image is captured relative to the video file |
| requestId            | string          | The unique identifier of the current frame segment           | Y              |                                                              |
| imgUrl               | string          | The URL of the current frame                                 | Y              |                                                              |
| imgText              | string          | OCR text content of the frame image                          | N              | OCR text recognition of the frame image, included in the recognition type |
| riskLevel            | string          | The disposal recommendation of the current frame             | Y              | `PASS`: normal content<br/>`REVIEW`: suspicious content<br/>`REJECT`: illegal content |
| riskLabel1           | string          | The first-level labels are parallel to each other. When the riskLevel is `PASS`, `normal` is returned. | Y              | First-level label                                            |
| riskLabel2           | string          | The second-level label belongs to the first-level label. It is empty when the riskLevel is `PASS`. | Y              | Second-level label                                           |
| riskLabel3           | string          | The third-level label belongs to the second-level label. It is empty when the riskLevel is `PASS`. | Y              | Third-level label                                            |
| riskDescription      | string          | Label interpretation                                         | Y              | When the user-defined list is hit, ***Hit the user-defined list*** is returned; when the riskLevel is `PASS`, `Normal` is returned; in other cases, the Chinese name of the first-level label: the second-level label: the third-level label is displayed |
| riskDetail           | json_object     | Risk detail information                                      | Y              | See [riskDetail description](#riskDetail)                    |
| allLabels            | json_array      | All risk label list                                          | Y              | All risk label list, see [allLabels description](#allLabels) |
| businessLabels       | json_array      | Business label list                                          | N              | Returned when imgBusinessType is passed in. See [businessLabels description](#businessLabels) |
| auxInfo              | json_object     | Auxiliary information                                        | Y              | Some auxiliary information is placed here, see [auxInfo description](#auxInfo3) |

<span id="auxInfo3">In frameDetail, the contents of auxInfo are as follows:</span>

| ***Parameter Name*** | ***Data Type*** | ***Parameter Description***                                  | ***Required*** | ***Specifications***                                         |
| -------------------- | --------------- | ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| qrContent            | string          | QR code link recognition of the frame image                  | N              | QR code link recognition of the frame image. Contact SUMERU to enable this feature. <br/>Note: After enabling this feature, only complete, normally recognizable QR codes will be returned, and the imgType parameter needs to contain ***AD***. |
| similarity           | float           | The similarity between the current frame image and the previous frame image | Y              | This field will be returned if there is an image, and the first frame of the video file will be compared with a pure black background image. |

<span id="riskDetail">The content of riskDetail in frameDetail is as follows:</span>

| ***Parameter Name*** | ***Type***  | ***Parameter Description*** | ***Mandatory*** | ***Specifications***                                         |
| -------------------- | ----------- | --------------------------- | --------------- | ------------------------------------------------------------ |
| riskSource           | int         | Risk source                 | Y               | Optional Values:<br/>`1000`: No risk<br/>`1001`: Text risk<br/>`1002`: Visual risk<br/>`1003`: Audio risk |
| faces                | json_array  | Face information            | N               | Returns the names and location information of the politically sensitive figures in the image, see [faces explanation](#faces) |
| face_num             | int         | Number of faces             | N               |                                                              |
| objects              | json_array  | Object information          | N               | Returns the names and location information of the identified objects in the image, see [objects explanation](#objects) |
| persons              | json_array  | Person information          | N               |                                                              |
| person_num           | int         | Number of persons           | N               |                                                              |
| ocrText              | json_object | Text information            | N               | Returns the text-related information in the image, see [ocrText explanation](#ocrText) |

<span id="faces">Each element of the faces array in riskDetail is as follows:</span>

| ***Parameter Name*** | ***Type*** | ***Parameter Description*** | ***Mandatory*** | ***Standard***                                               |
| -------------------- | ---------- | --------------------------- | --------------- | ------------------------------------------------------------ |
| id                   | string     | ID                          | Y               |                                                              |
| name                 | string     | Name                        | N               |                                                              |
| location             | int_array  | Location coordinates        | N               | This array has four values, representing the coordinates of the upper left corner and the lower right corner. For example, [207,522,340,567]<br/>207 represents the x-coordinate of the upper left corner<br/>522 represents the y-coordinate of the upper left corner<br/>340 represents the x-coordinate of the lower right corner<br/>567 represents the y-coordinate of the lower right corner |
| probability          | float      | Confidence                  | N               | Optional values are between 0 and 1. The larger the value, the higher the credibility |
| face_ratio           | float      | Face ratio                  | N               |                                                              |

<span id="objects">Each element of the objects array in riskDetail is as follows:</span>

| ***Parameter Name*** | ***Type*** | ***Parameter Description*** | ***Mandatory*** | ***Standards***                                              |
| -------------------- | ---------- | --------------------------- | --------------- | ------------------------------------------------------------ |
| id                   | string     | ID                          | Y               |                                                              |
| name                 | string     | Name                        | N               |                                                              |
| location             | int_array  | Location coordinates        | N               | This array has four values, representing the coordinates of the upper left corner and the lower right corner. For example, [207,522,340,567]<br/>207 represents the x-coordinate of the upper left corner<br/>522 represents the y-coordinate of the upper left corner<br/>340 represents the x-coordinate of the lower right corner<br/>567 represents the y-coordinate of the lower right corner |
| probability          | float      | Confidence                  | N               | Optional values are between 0 and 1. The larger the value, the higher the credibility |
| qrContent            | string     | QR code information         | N               |                                                              |

The content of each element in the persons array of riskDetail is as follows:

| ***Parameter Name*** | ***Type*** | ***Parameter Description***   | ***Mandatory or Optional*** | ***Standard***                                               |
| -------------------- | ---------- | ----------------------------- | --------------------------- | ------------------------------------------------------------ |
| id                   | string     | ID                            | Y                           |                                                              |
| location             | int_array  | Location coordinates          | N                           | The array contains four values, representing the coordinates of the upper left and lower right corners. For example, [207, 522, 340, 567]:<br/>207 represents the x-coordinate of the upper left corner<br/>522 represents the y-coordinate of the upper left corner<br/>340 represents the x-coordinate of the lower right corner<br/>567 represents the y-coordinate of the lower right corner |
| probability          | float      | Confidence score              | N                           | The value can be between 0 and 1, with higher values indicating higher confidence |
| person_ratio         | float      | Ratio of persons in the image | N                           |                                                              |

The content of each element in the ocrText array of riskDetail is as follows:

| ***Parameter Name*** | ***Type*** | ***Parameter Description***                                | ***Mandatory or Optional*** | ***Standard***                                               |
| -------------------- | ---------- | ---------------------------------------------------------- | --------------------------- | ------------------------------------------------------------ |
| text                 | string     | Recognized text in the image                               | N                           | All recognized text content                                  |
| matchedLists         | json_array | Information of the customer's custom list that was matched | N                           | Only returned when the customer's custom list is matched. See [matchedLists Description](#matchedLists) for details. |
| riskSegments         | json_array | High-risk content segments                                 | N                           | This exists when the functions of politics, violence, prohibited items, advertising, and others are enabled. See [riskSegments Description](#riskSegments) for details. |

In the ocrText array, the content of each element in the matchedLists array is as follows:

| ***Parameter Name*** | ***Type*** | ***Parameter Description***                                  | ***Required*** | ***Standard***                                               |
| -------------------- | ---------- | ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| name                 | string     | Name of the customer's custom list                           | N              |                                                              |
| words                | json_array | Information of the sensitive words in this list that were matched | N              | Counting starts from 0. See [words Description](#words) for details. |

In the matchedLists array, the content of each element in the words array is as follows:

| ***Parameter Name*** | ***Type*** | ***Parameter Description***    | ***Required*** | ***Standard***          |
| -------------------- | ---------- | ------------------------------ | -------------- | ----------------------- |
| word                 | string     | Sensitive word                 | N              |                         |
| position             | int_array  | Position of the sensitive word | N              | Counting starts from 0. |

For each element in riskDetail's persons array, the content is as follows:

| ***Parameter Name*** | ***Type*** | ***Description*** | ***Required*** | ***Standard***                                               |
| -------------------- | ---------- | ----------------- | -------------- | ------------------------------------------------------------ |
| id                   | string     | ID                | Y              |                                                              |
| location             | int_array  | Location          | N              | This array has four values, representing the coordinates of the upper-left and lower-right corners. For example, [207, 522, 340, 567]<br/>207 represents the x-coordinate of the upper-left corner<br/>522 represents the y-coordinate of the upper-left corner<br/>340 represents the x-coordinate of the lower-right corner<br/>567 represents the y-coordinate of the lower-right corner |
| probability          | float      | Confidence        | N              | The optional value is between 0 and 1, and the larger the value, the higher the confidence. |
| person_ratio         | float      | Person Ratio      | N              |                                                              |

In ocrText, the content of each member in the riskSegments array is as follows:

| ***Parameter Name*** | ***Type*** | ***Description*** | ***Required*** | ***Standard***          |
| -------------------- | ---------- | ----------------- | -------------- | ----------------------- |
| segment              | string     | Risky segment     | No             |                         |
| position             | int_array  | Position          | No             | Counting starts from 0. |

In frameDetail, the content of each member in the allLabels array is as follows:

| ***Parameter Name*** | ***Type***  | ***Description*** | ***Required*** | ***Standard***                                               |
| -------------------- | ----------- | ----------------- | -------------- | ------------------------------------------------------------ |
| riskLabel1           | string      | First level tag   | Y              | First level tag                                              |
| riskLabel2           | string      | Second level tag  | Y              | Second level tag                                             |
| riskLabel3           | string      | Third level tag   | Y              | Third level tag                                              |
| riskDescription      | string      | Risk description  | Y              | The Chinese name of the format "first-level risk tag: second-level risk tag: third-level risk tag" <br/>For returning custom lists hit: Hit custom list |
| riskLevel            | string      | Disposition       | Y              | `PASS`: normal content<br/>`REVIEW`***: suspicious content<br/>`REJECT`: violating content |
| probability          | float       | Confidence        | Y              | The optional value is between 0 and 1, and the larger the value, the higher the confidence. |
| riskDetail           | json_object | Risk detail       | Y              | The same as the riskDetail structure in frameDetail.         |

In frameDetail, the content of each member in the businessLabels array is as follows:

| ***Parameter Name*** | ***Type*** | ***Description*** | ***Required*** | ***Standard***                                               |
| -------------------- | ---------- | ----------------- | -------------- | ------------------------------------------------------------ |
| businessLabel1       | string     | First level tag   | Y              | First level tag                                              |
| businessLabel2       | string     | Second level tag  | Y              | Second level tag                                             |
| businessLabel3       | string     | Third level tag   | Y              | Third level tag                                              |
| businessDescription  | string     | Tag description   | Y              | The Chinese name of the format "first-level tag: second-level tag: third-level tag". |
| confidenceLevel      | int        | Confidence level  | Y              | The optional value is between 0 and 2, and the larger the value |

The content of businessDetail in businessLabels is as follows:

| ***Parameter Name*** | ***Type*** | ***Parameter Description*** | ***Required*** | ***Specification*** |
| -------------------- | ---------- | --------------------------- | -------------- | ------------------- |
| faces                | json_array | Face information            | N              |                     |
| face_num             | int        | Number of faces             | N              |                     |
| objects              | json_array | Object information          | N              |                     |
| persons              | Json_array | Person information          | N              |                     |
| person_num           | int        | Number of persons           | N              |                     |

Each element in the faces array in businessDetail is as follows:

| ***Parameter Name*** | ***Type*** | ***Parameter Description*** | ***Required*** | ***Specification***                                          |
| -------------------- | ---------- | --------------------------- | -------------- | ------------------------------------------------------------ |
| id                   | string     | ID number                   | Y              |                                                              |
| name                 | string     | Name                        | N              |                                                              |
| location             | int_array  | Location coordinates        | N              | The array has four values, representing the coordinates of the upper left corner and the lower right corner, for example, [207, 522, 340, 567]<br/>207 represents the x coordinate of the upper left corner<br/>522 represents the y coordinate of the upper left corner<br/>340 represents the x coordinate of the lower right corner<br/>567 represents the y coordinate of the lower right corner |
| probability          | float      | Confidence                  | N              | Optional value between 0 and 1, the larger the value, the higher the confidence |
| face_ratio           | float      | Face ratio                  | N              |                                                              |

Each element in the objects array in businessDetail is as follows:

| ***Parameter Name\**** | ***Type*** | ***Parameter Description*** | ***Required*** | ***Specification***                                          |
| ---------------------- | ---------- | --------------------------- | -------------- | ------------------------------------------------------------ |
| id                     | string     | ID number                   | Y              |                                                              |
| name                   | string     | Name                        | N              |                                                              |
| location               | int_array  | Location coordinates        | N              | The array has four values, representing the coordinates of the upper left corner and the lower right corner, for example, [207, 522, 340, 567]<br/>207 represents the x coordinate of the upper left corner<br/>522 represents the y coordinate of the upper left corner<br/>340 represents the x coordinate of the lower right corner<br/>567 represents the y coordinate of the lower right corner |
| probability            | float      | Confidence                  | N              | Optional value between 0 and 1, the larger the value, the higher the confidence |

Each element in the persons array in businessDetail is as follows:

| ***Parameter Name*** | ***Type*** | ***Parameter Description*** | ***Required*** | ***Specification***                                          |
| -------------------- | ---------- | --------------------------- | -------------- | ------------------------------------------------------------ |
| id                   | string     | ID number                   | Y              |                                                              |
| location             | int_array  | Location coordinates        | N              | The array has four values, representing the coordinates of the upper left corner and the lower right corner, for example, [207, 522, 340, 567]<br/>207 represents the x coordinate of the upper left corner<br/>522 represents the y coordinate of the upper left corner<br/>340 represents the x coordinate of the lower right corner<br/>567 represents the y coordinate of the lower right corner |
| probability          | float      | Confidence                  | N              |                                                              |

The detailed content of each member in the audioDetail array is as follows:

| ***Parameter*** | ***Type***  | ***Description***                                            | ***Required*** | ***Specification***                                          |
| --------------- | ----------- | ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| requestId       | string      | Unique identifier of the request                             | Y              |                                                              |
| audioStarttime  | float       | Start time of the audio segment                              | Y              |                                                              |
| audioEndtime    | float       | End time of the audio segment                                | Y              |                                                              |
| audioUrl        | string      | URL of the audio segment                                     | Y              |                                                              |
| audioText       | string      | Text transcribed from the audio                              | N              | Will be returned if recognized                               |
| riskLevel       | string      | Disposition recommendation for the current event             | Y              | `PASS`: Normal content<br/>`REVIEW`: Suspicious content<br/>`REJECT`: Violating content |
| riskLabel1      | string      | The first-level label, which is parallel to other first-level labels. Returns `normal` when riskLevel is `PASS` | Y              | First-level label                                            |
| riskLabel2      | string      | The second-level label, which belongs to the first-level label. Empty when riskLevel is `PASS` | Y              | Second-level label                                           |
| riskLabel3      | string      | The third-level label, which belongs to the second-level label. Empty when riskLevel is `PASS` | Y              | Third-level label                                            |
| riskDescription | string      | Label explanation                                            | Y              | Chinese name in the format of "first-level risk label: second-level risk label: third-level risk label"<br/>Returns ***\*Hit user-defined list\**** when hitting the user-defined list |
| riskDetail      | json_object | Detailed risk information                                    | Y              | See [riskDetail description](#riskDetail2)                   |
| allLabels       | json_array  | All risk label lists                                         | Y              | All risk label lists. See [allLabels description](#allLabels2) |
| businessLabels  | json_array  | Business label list                                          | N              | Returns when audioBusinessType is passed in. See [businessLabels description](#businessLabels2) |

In the audioDetail array, the detailed content of the riskDetail element is as follows:

| ***Parameter*** | ***Type*** | ***Description***                          | ***Required*** | ***Specification***                                          |
| --------------- | ---------- | ------------------------------------------ | -------------- | ------------------------------------------------------------ |
| riskSource      | int        | Risk source                                | Y              | Risk source, optional values:<br/>`1000`: No risk<br/>`1001`: Text risk<br/>`1002`: Visual risk<br/>`1003`: Audio risk |
| audioText       | string     | The result of transcription from the audio | N              |                                                              |
| matchedLists    | json_array | Hit customer-defined list information      | N              | Returns when hitting the customer-defined list, otherwise does not exist. See [matchedLists description](#matchedLists2) |
| riskSegments    | json_array | High-risk content segment                  | N              | Exists when using the political, violent, prohibited, competitor, advertising laws, and other functions. See [riskSegments description](#riskSegments2) |

The detailed content of each element in matchedLists in riskDetail is as follows:

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\****                          | ***\*Required\**** | ***\*Format\****                                             |
| ------------------- | -------------- | ---------------------------------------------- | ------------------ | ------------------------------------------------------------ |
| name                | string         | Custom name of the customer-defined list       | N                  |                                                              |
| words               | json_array     | Information of sensitive words in the hit list | N                  | Starting from 0, see words description (#words2) for details |

In the matchedLists array, the detailed content of each element in words is as follows:

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\****          | ***\*Required\**** | ***\*Format\**** |
| ------------------- | -------------- | ------------------------------ | ------------------ | ---------------- |
| word                | string         | Sensitive word                 | N                  |                  |
| position            | int_array      | Position of the sensitive word | N                  | Starting from 0  |

In riskDetail, the detailed content of each element in riskSegments is as follows:

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\****                     | ***\*Required\**** | ***\*Format\**** |
| ------------------- | -------------- | ----------------------------------------- | ------------------ | ---------------- |
| segment             | string         | High-risk content segment                 | N                  |                  |
| position            | int_array      | Position of the high-risk content segment | N                  | Starting from 0  |

In the audioDetail array, the content of each element in the allLabels array is as follows:

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\****      | ***\*Required\**** | ***\*Format\****                                             |
| ------------------- | -------------- | -------------------------- | ------------------ | ------------------------------------------------------------ |
| riskLabel1          | string         | First-level risk label     | Y                  | First-level risk label                                       |
| riskLabel2          | string         | Second-level risk label    | Y                  | Second-level risk label                                      |
| riskLabel3          | string         | Third-level risk label     | Y                  | Third-level risk label                                       |
| riskDescription     | string         | Risk description           | Y                  | Chinese name in the format of "first-level risk label: second-level risk label: third-level risk label"<br/>Returns Hit user-defined list when hitting the user-defined list |
| riskLevel           | string         | Disposition recommendation | Y                  | ***\*PASS\****: Normal content<br/>***\*REVIEW\****: Suspicious content<br/>***\*REJECT\****: Violating content |
| probability         | float          | Confidence level           | Y                  | Optional values range from 0 to 1. The larger the value, the higher the confidence level. |
| riskDetail          | json_object    | Risk detail                | Y                  | Same structure as riskDetail in audioDetail                  |

 

 

<span id="businessLabels2">For each member in the businessLabels array in audioDetail, the details are as follows:</span>

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\**** | ***\*Required\**** | ***\*Format\****                                             |
| ------------------- | -------------- | --------------------- | ------------------ | ------------------------------------------------------------ |
| businessLabel1      | string         | First-level label     | Y                  | First-level label                                            |
| businessLabel2      | string         | Second-level label    | Y                  | Second-level label                                           |
| businessLabel3      | string         | Third-level label     | Y                  | Third-level label                                            |
| businessDescription | string         | Label description     | Y                  | Chinese name in the format of "first-level label: second-level label: third-level label" |
| confidenceLevel     | int            | Confidence level      | Y                  | Optional values between 0 and 2. The higher the value, the higher the confidence level |
| probability         | float          | Confidence score      | Y                  | Optional values between 0 and 1. The higher the value, the higher the confidence score |
| businessDetail      | json_object    | Detailed information  | Y                  |                                                              |

<span id="tokenProfileLabels">For each member in the tokenProfileLabels array, the details are as follows:</span>

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\**** | ***\*Required\**** | ***\*Format\****                        |
| ------------------- | -------------- | --------------------- | ------------------ | --------------------------------------- |
| label1              | string         | First-level label     | N                  |                                         |
| label2              | string         | Second-level label    | N                  |                                         |
| label3              | string         | Third-level label     | N                  |                                         |
| description         | string         | Label description     | N                  |                                         |
| timestamp           | int            | Tagging timestamp     | N                  | 13-digit Unix timestamp in milliseconds |

<span id="tokenRiskLabels">The fields in the tokenRiskLabels array are the same as in tokenProfileLabels.</span>

Output in md format:

<span id="businessLabels2">For each member in the businessLabels array in audioDetail, the details are as follows:</span>

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\**** | ***\*Required\**** | ***\*Format\****                                             |
| ------------------- | -------------- | --------------------- | ------------------ | ------------------------------------------------------------ |
| businessLabel1      | string         | First-level label     | Y                  | First-level label                                            |
| businessLabel2      | string         | Second-level label    | Y                  | Second-level label                                           |
| businessLabel3      | string         | Third-level label     | Y                  | Third-level label                                            |
| businessDescription | string         | Label description     | Y                  | Chinese name in the format of "first-level label: second-level label: third-level label" |
| confidenceLevel     | int            | Confidence level      | Y                  | Optional values between 0 and 2. The higher the value, the higher the confidence level |
| probability         | float          | Confidence score      | Y                  | Optional values between 0 and 1. The higher the value, the higher the confidence score |
| businessDetail      | json_object    | Detailed information  | Y                  |                                                              |

<span id="tokenProfileLabels">For each member in the tokenProfileLabels array, the details are as follows:</span>

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\**** | ***\*Required\**** | ***\*Format\****                        |
| ------------------- | -------------- | --------------------- | ------------------ | --------------------------------------- |
| label1              | string         | First-level label     | N                  |                                         |
| label2              | string         | Second-level label    | N                  |                                         |
| label3              | string         | Third-level label     | N                  |                                         |
| description         | string         | Label description     | N                  |                                         |
| timestamp           | int            | Tagging timestamp     | N                  | 13-digit Unix timestamp in milliseconds |

<span id="tokenRiskLabels">The fields in the tokenRiskLabels array are the same as in tokenProfileLabels.</span>

## Query Video Results

This interface is used for customers to actively query the recognition results of video files, and it is recommended to query every 30 seconds.

### Interface Description

This interface is used for customers to actively query the recognition results of video files, and it is recommended to query every 30 seconds. The query supports results within the past three days.

### Request URL:
| Cluster | URL | Supported Product List |
| --- | --- | --- |
| Beijing | `http://api-video-bj.fengkongcloud.com/video/query/v4` | Video files |
| Shanghai | `http://api-video-sh.fengkongcloud.com/video/query/v4` | Video files |
| Singapore | `http://api-video-xjp.fengkongcloud.com/video/query/v4` | Video files |

### Request Method:

`POST`

### Supported Protocol:

`HTTP`or`HTTPS`

### Character Encoding:

`UTF-8`

### Recommended Timeout:

1s

### Request Parameters:

Put in the HTTP Body and use the Json format. The specific parameters are as follows:

| ***Parameter*** | ***Type*** | ***Parameter Description***                                  | ***Required*** | ***Specification***                                          |
| --------------- | ---------- | ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| accessKey       | string     | Used for permission authentication, provided by NextData when the account service is opened | Y              |                                                              |
| btId            | string     | Video unique identifier, used to query recognition results, up to 64 characters | Y              |                                                              |
| acceptLang      | string     | The language type of the label                               | Y              | The language type of the label<br/>Optional values:<br/>zh:Chinese<br/>en:English |

###  Callback Parameters:

Placed in HTTP Body, using JSON format, specific parameters are as follows:

| ***Parameter Name*** | ***Type***  | ***Description***                    | ***Required*** | ***Specification***                                          |
| -------------------- | ----------- | ------------------------------------ | -------------- | ------------------------------------------------------------ |
| code                 | int         | Return code                          | Y              | See [Interface Response Code List](#interface-response-code-list) for details |
| message              | string      | Return code description              | Y              |                                                              |
| requestId            | string      | Request unique identifier            | Y              |                                                              |
| btId                 | string      | Video unique identifier              | Y              | Up to 64 characters                                          |
| riskLevel            | string      | Risk level, exists when code is 1100 | N              | Return value:<br/>`PASS`***: Normal content, recommended to be released directly<br/>***`REVIEW`***: Suspicious content, recommended for manual review<br/>***`REJECT`: ***Violation content, recommended to be blocked directly*** |
| frameDetail          | json_array  | Risk details                         | N              | Returned when there are risk segments or returnAllImg=1, see [frameDetail description](#frameDetail) for details |
| audioDetail          | json_array  | Audio segment information            | N              | Returned when there are risk segments or returnAllAudio=1, see [audioDetail description](#audioDetail) for details |
| auxInfo              | json_object | Auxiliary information                | N              | Exists when code is `1100`, see [auxInfo description](#auxInfo) for details |
| tokenProfileLabels   | json_array  | Account attribute labels             | N              | Returned only when the function is enabled, see [tokenProfileLabels description](#tokenProfileLabels) for details |
| tokenRiskLabels      | json_array  | Account risk labels                  | N              | Returned only when the function is enabled, see [tokenRiskLabels description](#tokenRiskLabels) for details |

<span id="auxInfo">Specific content of auxInfo:</span>

| ***Parameter Name*** | ***Type***  | ***Description***                                            | ***Required*** | ***Specification*** |
| -------------------- | ----------- | ------------------------------------------------------------ | -------------- | ------------------- |
| frameCount           | int         | Number of video frames returned. When returnAllImg=0, it is the number of risks. When returnAllImg=1, it is the total number | Y              |                     |
| time                 | int         | Video duration                                               | Y              |                     |
| passThrough          | json_object | Pass-through field, the content of which is the same as the value of passThrough in data.extra of the request parameters | N              |                     |

<span id="frameDetail">The specific contents of each member in the frameDetail array are as follows:</span>

| ***Parameter Name*** | ***Data Type*** | ***Parameter Description***                                  | ***Required*** | ***Specifications***                                         |
| -------------------- | --------------- | ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| time                 | float           | The time when the frame is captured in the video file, in seconds | Y              | The time when the frame image is captured relative to the video file |
| requestId            | string          | The unique identifier of the current frame segment           | Y              |                                                              |
| imgUrl               | string          | The URL of the current frame                                 | Y              |                                                              |
| imgText              | string          | OCR text content of the frame image                          | N              | OCR text recognition of the frame image, included in the recognition type |
| riskLevel            | string          | The disposal recommendation of the current frame             | Y              | `PASS`: normal content<br/>`REVIEW`: suspicious content<br/>`REJECT`: illegal content |
| riskLabel1           | string          | The first-level labels are parallel to each other. When the riskLevel is `PASS`, `normal` is returned. | Y              | First-level label                                            |
| riskLabel2           | string          | The second-level label belongs to the first-level label. It is empty when the riskLevel is `PASS`. | Y              | Second-level label                                           |
| riskLabel3           | string          | The third-level label belongs to the second-level label. It is empty when the riskLevel is `PASS`. | Y              | Third-level label                                            |
| riskDescription      | string          | Label interpretation                                         | Y              | When the user-defined list is hit, ***Hit the user-defined list*** is returned; when the riskLevel is `PASS`, `Normal` is returned; in other cases, the Chinese name of the first-level label: the second-level label: the third-level label is displayed |
| riskDetail           | json_object     | Risk detail information                                      | Y              | See [riskDetail description](#riskDetail)                    |
| allLabels            | json_array      | All risk label list                                          | Y              | All risk label list, see [allLabels description](#allLabels) |
| businessLabels       | json_array      | Business label list                                          | N              | Returned when imgBusinessType is passed in. See [businessLabels description](#businessLabels) |
| auxInfo              | json_object     | Auxiliary information                                        | Y              | Some auxiliary information is placed here, see [auxInfo description](#auxInfo3) |

<span id="auxInfo3">In frameDetail, the contents of auxInfo are as follows:</span>

| ***Parameter Name*** | ***Data Type*** | ***Parameter Description***                                  | ***Required*** | ***Specifications***                                         |
| -------------------- | --------------- | ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| qrContent            | string          | QR code link recognition of the frame image                  | N              | QR code link recognition of the frame image. Contact SUMERU to enable this feature. <br/>Note: After enabling this feature, only complete, normally recognizable QR codes will be returned, and the imgType parameter needs to contain ***AD***. |
| similarity           | float           | The similarity between the current frame image and the previous frame image | Y              | This field will be returned if there is an image, and the first frame of the video file will be compared with a pure black background image. |

<span id="riskDetail">The content of riskDetail in frameDetail is as follows:</span>

| ***Parameter Name*** | ***Type***  | ***Parameter Description*** | ***Mandatory*** | ***Specifications***                                         |
| -------------------- | ----------- | --------------------------- | --------------- | ------------------------------------------------------------ |
| riskSource           | int         | Risk source                 | Y               | Optional Values:<br/>`1000`: No risk<br/>`1001`: Text risk<br/>`1002`: Visual risk<br/>`1003`: Audio risk |
| faces                | json_array  | Face information            | N               | Returns the names and location information of the politically sensitive figures in the image, see [faces explanation](#faces) |
| face_num             | int         | Number of faces             | N               |                                                              |
| objects              | json_array  | Object information          | N               | Returns the names and location information of the identified objects in the image, see [objects explanation](#objects) |
| persons              | json_array  | Person information          | N               |                                                              |
| person_num           | int         | Number of persons           | N               |                                                              |
| ocrText              | json_object | Text information            | N               | Returns the text-related information in the image, see [ocrText explanation](#ocrText) |

<span id="faces">Each element of the faces array in riskDetail is as follows:</span>

| ***Parameter Name*** | ***Type*** | ***Parameter Description*** | ***Mandatory*** | ***Standard***                                               |
| -------------------- | ---------- | --------------------------- | --------------- | ------------------------------------------------------------ |
| id                   | string     | ID                          | Y               |                                                              |
| name                 | string     | Name                        | N               |                                                              |
| location             | int_array  | Location coordinates        | N               | This array has four values, representing the coordinates of the upper left corner and the lower right corner. For example, [207,522,340,567]<br/>207 represents the x-coordinate of the upper left corner<br/>522 represents the y-coordinate of the upper left corner<br/>340 represents the x-coordinate of the lower right corner<br/>567 represents the y-coordinate of the lower right corner |
| probability          | float      | Confidence                  | N               | Optional values are between 0 and 1. The larger the value, the higher the credibility |
| face_ratio           | float      | Face ratio                  | N               |                                                              |

<span id="objects">Each element of the objects array in riskDetail is as follows:</span>

| ***Parameter Name*** | ***Type*** | ***Parameter Description*** | ***Mandatory*** | ***Standards***                                              |
| -------------------- | ---------- | --------------------------- | --------------- | ------------------------------------------------------------ |
| id                   | string     | ID                          | Y               |                                                              |
| name                 | string     | Name                        | N               |                                                              |
| location             | int_array  | Location coordinates        | N               | This array has four values, representing the coordinates of the upper left corner and the lower right corner. For example, [207,522,340,567]<br/>207 represents the x-coordinate of the upper left corner<br/>522 represents the y-coordinate of the upper left corner<br/>340 represents the x-coordinate of the lower right corner<br/>567 represents the y-coordinate of the lower right corner |
| probability          | float      | Confidence                  | N               | Optional values are between 0 and 1. The larger the value, the higher the credibility |
| qrContent            | string     | QR code information         | N               |                                                              |

The content of each element in the persons array of riskDetail is as follows:

| ***Parameter Name*** | ***Type*** | ***Parameter Description***   | ***Mandatory or Optional*** | ***Standard***                                               |
| -------------------- | ---------- | ----------------------------- | --------------------------- | ------------------------------------------------------------ |
| id                   | string     | ID                            | Y                           |                                                              |
| location             | int_array  | Location coordinates          | N                           | The array contains four values, representing the coordinates of the upper left and lower right corners. For example, [207, 522, 340, 567]:<br/>207 represents the x-coordinate of the upper left corner<br/>522 represents the y-coordinate of the upper left corner<br/>340 represents the x-coordinate of the lower right corner<br/>567 represents the y-coordinate of the lower right corner |
| probability          | float      | Confidence score              | N                           | The value can be between 0 and 1, with higher values indicating higher confidence |
| person_ratio         | float      | Ratio of persons in the image | N                           |                                                              |

The content of each element in the ocrText array of riskDetail is as follows:

| ***Parameter Name*** | ***Type*** | ***Parameter Description***                                | ***Mandatory or Optional*** | ***Standard***                                               |
| -------------------- | ---------- | ---------------------------------------------------------- | --------------------------- | ------------------------------------------------------------ |
| text                 | string     | Recognized text in the image                               | N                           | All recognized text content                                  |
| matchedLists         | json_array | Information of the customer's custom list that was matched | N                           | Only returned when the customer's custom list is matched. See [matchedLists Description](#matchedLists) for details. |
| riskSegments         | json_array | High-risk content segments                                 | N                           | This exists when the functions of politics, violence, prohibited items, advertising, and others are enabled. See [riskSegments Description](#riskSegments) for details. |

In the ocrText array, the content of each element in the matchedLists array is as follows:

| ***Parameter Name*** | ***Type*** | ***Parameter Description***                                  | ***Required*** | ***Standard***                                               |
| -------------------- | ---------- | ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| name                 | string     | Name of the customer's custom list                           | N              |                                                              |
| words                | json_array | Information of the sensitive words in this list that were matched | N              | Counting starts from 0. See [words Description](#words) for details. |

In the matchedLists array, the content of each element in the words array is as follows:

| ***Parameter Name*** | ***Type*** | ***Parameter Description***    | ***Required*** | ***Standard***          |
| -------------------- | ---------- | ------------------------------ | -------------- | ----------------------- |
| word                 | string     | Sensitive word                 | N              |                         |
| position             | int_array  | Position of the sensitive word | N              | Counting starts from 0. |

For each element in riskDetail's persons array, the content is as follows:

| ***Parameter Name*** | ***Type*** | ***Description*** | ***Required*** | ***Standard***                                               |
| -------------------- | ---------- | ----------------- | -------------- | ------------------------------------------------------------ |
| id                   | string     | ID                | Y              |                                                              |
| location             | int_array  | Location          | N              | This array has four values, representing the coordinates of the upper-left and lower-right corners. For example, [207, 522, 340, 567]<br/>207 represents the x-coordinate of the upper-left corner<br/>522 represents the y-coordinate of the upper-left corner<br/>340 represents the x-coordinate of the lower-right corner<br/>567 represents the y-coordinate of the lower-right corner |
| probability          | float      | Confidence        | N              | The optional value is between 0 and 1, and the larger the value, the higher the confidence. |
| person_ratio         | float      | Person Ratio      | N              |                                                              |

In ocrText, the content of each member in the riskSegments array is as follows:

| ***Parameter Name*** | ***Type*** | ***Description*** | ***Required*** | ***Standard***          |
| -------------------- | ---------- | ----------------- | -------------- | ----------------------- |
| segment              | string     | Risky segment     | No             |                         |
| position             | int_array  | Position          | No             | Counting starts from 0. |

In frameDetail, the content of each member in the allLabels array is as follows:

| ***Parameter Name*** | ***Type***  | ***Description*** | ***Required*** | ***Standard***                                               |
| -------------------- | ----------- | ----------------- | -------------- | ------------------------------------------------------------ |
| riskLabel1           | string      | First level tag   | Y              | First level tag                                              |
| riskLabel2           | string      | Second level tag  | Y              | Second level tag                                             |
| riskLabel3           | string      | Third level tag   | Y              | Third level tag                                              |
| riskDescription      | string      | Risk description  | Y              | The Chinese name of the format "first-level risk tag: second-level risk tag: third-level risk tag" <br/>For returning custom lists hit: Hit custom list |
| riskLevel            | string      | Disposition       | Y              | `PASS`: normal content<br/>`REVIEW`***: suspicious content<br/>`REJECT`: violating content |
| probability          | float       | Confidence        | Y              | The optional value is between 0 and 1, and the larger the value, the higher the confidence. |
| riskDetail           | json_object | Risk detail       | Y              | The same as the riskDetail structure in frameDetail.         |

In frameDetail, the content of each member in the businessLabels array is as follows:

| ***Parameter Name*** | ***Type*** | ***Description*** | ***Required*** | ***Standard***                                               |
| -------------------- | ---------- | ----------------- | -------------- | ------------------------------------------------------------ |
| businessLabel1       | string     | First level tag   | Y              | First level tag                                              |
| businessLabel2       | string     | Second level tag  | Y              | Second level tag                                             |
| businessLabel3       | string     | Third level tag   | Y              | Third level tag                                              |
| businessDescription  | string     | Tag description   | Y              | The Chinese name of the format "first-level tag: second-level tag: third-level tag". |
| confidenceLevel      | int        | Confidence level  | Y              | The optional value is between 0 and 2, and the larger the value |

The content of businessDetail in businessLabels is as follows:

| ***Parameter Name*** | ***Type*** | ***Parameter Description*** | ***Required*** | ***Specification*** |
| -------------------- | ---------- | --------------------------- | -------------- | ------------------- |
| faces                | json_array | Face information            | N              |                     |
| face_num             | int        | Number of faces             | N              |                     |
| objects              | json_array | Object information          | N              |                     |
| persons              | Json_array | Person information          | N              |                     |
| person_num           | int        | Number of persons           | N              |                     |

Each element in the faces array in businessDetail is as follows:

| ***Parameter Name*** | ***Type*** | ***Parameter Description*** | ***Required*** | ***Specification***                                          |
| -------------------- | ---------- | --------------------------- | -------------- | ------------------------------------------------------------ |
| id                   | string     | ID number                   | Y              |                                                              |
| name                 | string     | Name                        | N              |                                                              |
| location             | int_array  | Location coordinates        | N              | The array has four values, representing the coordinates of the upper left corner and the lower right corner, for example, [207, 522, 340, 567]<br/>207 represents the x coordinate of the upper left corner<br/>522 represents the y coordinate of the upper left corner<br/>340 represents the x coordinate of the lower right corner<br/>567 represents the y coordinate of the lower right corner |
| probability          | float      | Confidence                  | N              | Optional value between 0 and 1, the larger the value, the higher the confidence |
| face_ratio           | float      | Face ratio                  | N              |                                                              |

Each element in the objects array in businessDetail is as follows:

| ***Parameter Name\**** | ***Type*** | ***Parameter Description*** | ***Required*** | ***Specification***                                          |
| ---------------------- | ---------- | --------------------------- | -------------- | ------------------------------------------------------------ |
| id                     | string     | ID number                   | Y              |                                                              |
| name                   | string     | Name                        | N              |                                                              |
| location               | int_array  | Location coordinates        | N              | The array has four values, representing the coordinates of the upper left corner and the lower right corner, for example, [207, 522, 340, 567]<br/>207 represents the x coordinate of the upper left corner<br/>522 represents the y coordinate of the upper left corner<br/>340 represents the x coordinate of the lower right corner<br/>567 represents the y coordinate of the lower right corner |
| probability            | float      | Confidence                  | N              | Optional value between 0 and 1, the larger the value, the higher the confidence |

Each element in the persons array in businessDetail is as follows:

| ***Parameter Name*** | ***Type*** | ***Parameter Description*** | ***Required*** | ***Specification***                                          |
| -------------------- | ---------- | --------------------------- | -------------- | ------------------------------------------------------------ |
| id                   | string     | ID number                   | Y              |                                                              |
| location             | int_array  | Location coordinates        | N              | The array has four values, representing the coordinates of the upper left corner and the lower right corner, for example, [207, 522, 340, 567]<br/>207 represents the x coordinate of the upper left corner<br/>522 represents the y coordinate of the upper left corner<br/>340 represents the x coordinate of the lower right corner<br/>567 represents the y coordinate of the lower right corner |
| probability          | float      | Confidence                  | N              |                                                              |

The detailed content of each member in the audioDetail array is as follows:

| ***Parameter*** | ***Type***  | ***Description***                                            | ***Required*** | ***Specification***                                          |
| --------------- | ----------- | ------------------------------------------------------------ | -------------- | ------------------------------------------------------------ |
| requestId       | string      | Unique identifier of the request                             | Y              |                                                              |
| audioStarttime  | float       | Start time of the audio segment                              | Y              |                                                              |
| audioEndtime    | float       | End time of the audio segment                                | Y              |                                                              |
| audioUrl        | string      | URL of the audio segment                                     | Y              |                                                              |
| audioText       | string      | Text transcribed from the audio                              | N              | Will be returned if recognized                               |
| riskLevel       | string      | Disposition recommendation for the current event             | Y              | `PASS`: Normal content<br/>`REVIEW`: Suspicious content<br/>`REJECT`: Violating content |
| riskLabel1      | string      | The first-level label, which is parallel to other first-level labels. Returns `normal` when riskLevel is `PASS` | Y              | First-level label                                            |
| riskLabel2      | string      | The second-level label, which belongs to the first-level label. Empty when riskLevel is `PASS` | Y              | Second-level label                                           |
| riskLabel3      | string      | The third-level label, which belongs to the second-level label. Empty when riskLevel is `PASS` | Y              | Third-level label                                            |
| riskDescription | string      | Label explanation                                            | Y              | Chinese name in the format of "first-level risk label: second-level risk label: third-level risk label"<br/>Returns ***\*Hit user-defined list\**** when hitting the user-defined list |
| riskDetail      | json_object | Detailed risk information                                    | Y              | See [riskDetail description](#riskDetail2)                   |
| allLabels       | json_array  | All risk label lists                                         | Y              | All risk label lists. See [allLabels description](#allLabels2) |
| businessLabels  | json_array  | Business label list                                          | N              | Returns when audioBusinessType is passed in. See [businessLabels description](#businessLabels2) |

In the audioDetail array, the detailed content of the riskDetail element is as follows:

| ***Parameter*** | ***Type*** | ***Description***                          | ***Required*** | ***Specification***                                          |
| --------------- | ---------- | ------------------------------------------ | -------------- | ------------------------------------------------------------ |
| riskSource      | int        | Risk source                                | Y              | Risk source, optional values:<br/>`1000`: No risk<br/>`1001`: Text risk<br/>`1002`: Visual risk<br/>`1003`: Audio risk |
| audioText       | string     | The result of transcription from the audio | N              |                                                              |
| matchedLists    | json_array | Hit customer-defined list information      | N              | Returns when hitting the customer-defined list, otherwise does not exist. See [matchedLists description](#matchedLists2) |
| riskSegments    | json_array | High-risk content segment                  | N              | Exists when using the political, violent, prohibited, competitor, advertising laws, and other functions. See [riskSegments description](#riskSegments2) |

The detailed content of each element in matchedLists in riskDetail is as follows:

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\****                          | ***\*Required\**** | ***\*Format\****                                             |
| ------------------- | -------------- | ---------------------------------------------- | ------------------ | ------------------------------------------------------------ |
| name                | string         | Custom name of the customer-defined list       | N                  |                                                              |
| words               | json_array     | Information of sensitive words in the hit list | N                  | Starting from 0, see words description (#words2) for details |

In the matchedLists array, the detailed content of each element in words is as follows:

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\****          | ***\*Required\**** | ***\*Format\**** |
| ------------------- | -------------- | ------------------------------ | ------------------ | ---------------- |
| word                | string         | Sensitive word                 | N                  |                  |
| position            | int_array      | Position of the sensitive word | N                  | Starting from 0  |

In riskDetail, the detailed content of each element in riskSegments is as follows:

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\****                     | ***\*Required\**** | ***\*Format\**** |
| ------------------- | -------------- | ----------------------------------------- | ------------------ | ---------------- |
| segment             | string         | High-risk content segment                 | N                  |                  |
| position            | int_array      | Position of the high-risk content segment | N                  | Starting from 0  |

In the audioDetail array, the content of each element in the allLabels array is as follows:

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\****      | ***\*Required\**** | ***\*Format\****                                             |
| ------------------- | -------------- | -------------------------- | ------------------ | ------------------------------------------------------------ |
| riskLabel1          | string         | First-level risk label     | Y                  | First-level risk label                                       |
| riskLabel2          | string         | Second-level risk label    | Y                  | Second-level risk label                                      |
| riskLabel3          | string         | Third-level risk label     | Y                  | Third-level risk label                                       |
| riskDescription     | string         | Risk description           | Y                  | Chinese name in the format of "first-level risk label: second-level risk label: third-level risk label"<br/>Returns Hit user-defined list when hitting the user-defined list |
| riskLevel           | string         | Disposition recommendation | Y                  | ***\*PASS\****: Normal content<br/>***\*REVIEW\****: Suspicious content<br/>***\*REJECT\****: Violating content |
| probability         | float          | Confidence level           | Y                  | Optional values range from 0 to 1. The larger the value, the higher the confidence level. |
| riskDetail          | json_object    | Risk detail                | Y                  | Same structure as riskDetail in audioDetail                  |

 

 

<span id="businessLabels2">For each member in the businessLabels array in audioDetail, the details are as follows:</span>

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\**** | ***\*Required\**** | ***\*Format\****                                             |
| ------------------- | -------------- | --------------------- | ------------------ | ------------------------------------------------------------ |
| businessLabel1      | string         | First-level label     | Y                  | First-level label                                            |
| businessLabel2      | string         | Second-level label    | Y                  | Second-level label                                           |
| businessLabel3      | string         | Third-level label     | Y                  | Third-level label                                            |
| businessDescription | string         | Label description     | Y                  | Chinese name in the format of "first-level label: second-level label: third-level label" |
| confidenceLevel     | int            | Confidence level      | Y                  | Optional values between 0 and 2. The higher the value, the higher the confidence level |
| probability         | float          | Confidence score      | Y                  | Optional values between 0 and 1. The higher the value, the higher the confidence score |
| businessDetail      | json_object    | Detailed information  | Y                  |                                                              |

<span id="tokenProfileLabels">For each member in the tokenProfileLabels array, the details are as follows:</span>

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\**** | ***\*Required\**** | ***\*Format\****                        |
| ------------------- | -------------- | --------------------- | ------------------ | --------------------------------------- |
| label1              | string         | First-level label     | N                  |                                         |
| label2              | string         | Second-level label    | N                  |                                         |
| label3              | string         | Third-level label     | N                  |                                         |
| description         | string         | Label description     | N                  |                                         |
| timestamp           | int            | Tagging timestamp     | N                  | 13-digit Unix timestamp in milliseconds |

<span id="tokenRiskLabels">The fields in the tokenRiskLabels array are the same as in tokenProfileLabels.</span>

Output in md format:

<span id="businessLabels2">For each member in the businessLabels array in audioDetail, the details are as follows:</span>

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\**** | ***\*Required\**** | ***\*Format\****                                             |
| ------------------- | -------------- | --------------------- | ------------------ | ------------------------------------------------------------ |
| businessLabel1      | string         | First-level label     | Y                  | First-level label                                            |
| businessLabel2      | string         | Second-level label    | Y                  | Second-level label                                           |
| businessLabel3      | string         | Third-level label     | Y                  | Third-level label                                            |
| businessDescription | string         | Label description     | Y                  | Chinese name in the format of "first-level label: second-level label: third-level label" |
| confidenceLevel     | int            | Confidence level      | Y                  | Optional values between 0 and 2. The higher the value, the higher the confidence level |
| probability         | float          | Confidence score      | Y                  | Optional values between 0 and 1. The higher the value, the higher the confidence score |
| businessDetail      | json_object    | Detailed information  | Y                  |                                                              |

<span id="tokenProfileLabels">For each member in the tokenProfileLabels array, the details are as follows:</span>

| ***\*Parameter\**** | ***\*Type\**** | ***\*Description\**** | ***\*Required\**** | ***\*Format\****                        |
| ------------------- | -------------- | --------------------- | ------------------ | --------------------------------------- |
| label1              | string         | First-level label     | N                  |                                         |
| label2              | string         | Second-level label    | N                  |                                         |
| label3              | string         | Third-level label     | N                  |                                         |
| description         | string         | Label description     | N                  |                                         |
| timestamp           | int            | Tagging timestamp     | N                  | 13-digit Unix timestamp in milliseconds |

<span id="tokenRiskLabels">The fields in the tokenRiskLabels array are the same as in tokenProfileLabels.</span>

## imgBusinessType List

| ***Business Tag Recognition Type*** | ***Type Description***                                | ***Remarks***                                                |
| ----------------------------------- | ----------------------------------------------------- | ------------------------------------------------------------ |
| AGE                                 | Face - Age                                            | Can recognize minors                                         |
| GENDER                              | Face - Gender                                         |                                                              |
| BEAUTY                              | Face - Appearance                                     |                                                              |
| FACEDETECTION                       | Face - Face Detection                                 | Can recognize no face, real person, mask face, front face, side face, etc. |
| FAKEFACE                            | Face - Fake Face                                      |                                                              |
| RACE                                | Face - Race                                           | Can recognize black, white, and yellow races                 |
| PUBLICFIGURE                        | Characters - Public Figures                           | Can recognize well-known stars, internet celebrities, etc.   |
| TAINTEDSTAR                         | Characters - Disreputable Figures                     |                                                              |
| POSTURE                             | Portrait - Portrait Posture                           | Can recognize sitting posture, kneeling posture, etc.        |
| DRESS                               | Portrait - Portrait Dress                             | Can recognize JK, Hanfu, etc.                                |
| BODY                                | Human Body                                            | Can recognize hair, eyes, nose, etc.                         |
| PICTUREFORM                         | Picture Property - Picture Type                       | Can recognize animation, expression package, etc.            |
| PICTURESTRUCT                       | Picture Property - Picture Structure                  | Can recognize palace grid, plot diagram, etc.                |
| LOWVISION                           | Picture Property - Low-quality Picture                | Can recognize blur, smear, mosaic, etc.                      |
| LOWCONTNET                          | Picture Property - Low-quality Content                | Can recognize dense points and lines, insect-like density, etc. |
| LIVEPICTURE                         | Picture Property - Live Picture                       | Can recognize bed live broadcast, driving live broadcast, etc. |
| SCREENSHOT                          | Picture Property - APP Screenshot (Content Migration) | Can recognize Moments screenshots, chat screenshots, etc.    |
| FITNESS                             | Scene Theme - Fitness                                 |                                                              |
| CATE                                | Scene Theme - Food                                    |                                                              |
| MUSIC                               | Scene Theme - Music                                   |                                                              |
| SPORTS                              | Scene Theme - Sports                                  |                                                              |
| SCENERY                             | Scene Theme - Natural Scenery                         | Can recognize sky, sea, grassland, etc.                      |
| CITYVIEW                            | Scene Theme - Urban Scenery                           | Can recognize street view                                    |
| 3CPRODUCTSLOGO                      | LOGO - 3C Electronics Brand                           | Can recognize Huawei, Xiaomi, OPPO, etc.                     |
| SHOPPINGAPPSLOGO                    | LOGO - Shopping and Comparison Applications           | Can recognize Pinduoduo, etc.                                |
| RETOUCHAPPSLOGO                     | LOGO - Photo Editing Applications                     | Can recognize Quick Cut, Miaopai, etc.                       |
| SOCIALAPPSLOGO                      | LOGO - Social Communication Applications              | Can recognize Weibo, Xiaohongshu, etc.                       |
| PHOTOMATERIALLOGO                   | LOGO - Copyrighted Material Applications              | Can recognize CFP, etc.                                      |
| NEWSAPPSLOGO                        | LOGO - News Reading Applications                      | Can recognize Sina, Visual China, etc.                       |
| ENTERTAINMENTAPPSLOGO               | LOGO - Audio and Video Entertainment Applications     | Can recognize Douyin, Kuaishou, etc.                         |
| SPORTSLOGO                          | LOGO - Sports Events                                  | Can recognize Olympic logo                                   |
| APPARELLOGO                         | LOGO - Footwear and Clothing Brands                   | Can recognize Vans, H&M, etc.                                |
| ACCESSORIESLOGO                     | LOGO - Jewelry and Accessories Brands                 | Can recognize Audemars Piguet, Nomos, etc.                   |
| COSMETICSLOGO                       | LOGO - Cosmetics Brands                               | Can recognize LOTTE, EyesLipsFace, etc.                      |
| FOODLOGO                            | LOGO - Food Brands                                    | Can recognize Starbucks, LOTTE, etc.                         |

## Interface Response Code List

The request response code list is as follows:

| **code** | **message**              |
| -------- | ------------------------ |
| 1100     | Success                  |
| 1101     | Request is processing    |
| 1901     | Request qps over limit   |
| 1902     | Invalid parameters       |
| 1903     | Service failure          |
| 9100     | Insufficient balance     |
| 9101     | No permission to operate |

## Demo

### Demo of uploading interface request:

```json
{
    "accessKey": "**********",
    "appId": "default",
    "btId": "1639824316368",
    "eventId": "video",
    "imgType": "POLITICS_VIOLENCE_BAN_PORN_MINOR_AD_SPAM_LOGO_STAR_OCR",
    "imgBusinessType": "SCREEN_SCENCE_QR_FACE_QUALITY_MINOR_LOGO_BEAUTY_FACECOMPARE",
    "audioType": "POLITICAL_PORN_AD_MOAN_ABUSE",
    "audioBusinessType": "SING_LANGUAGE_MINOR_GENDER_TIMBRE",
    "callback": "http://www.xxx.top/xxx",
    "data": {
        "btId": "1639824316368",
        "channel": "video",
        "detectFrequency": 3,
	"advancedFrequency": {"durationPoints":[300,600],"frequencies":[1,5,10]},
        "tokenId": "test",
        "ip":"123.171.34.3",
        "url": "http://oss.xxx.com/static/photo/117608703147396.mp4",
        "returnAllAudio": 1,
        "returnAllImg": 1,
        "extra": {
            "passThrough": {
                "passThrough1": "",
                "passThrough2": "",
                "passThrough3": ""
            }
        }
    }
}
```

### Upload interface return demo:

```json
{
    "code": 1100,
    "message": "Success",
    "requestId": "1639824316368",
    "btId": "1639824316368"
}
```

### Demo of asynchronous callback results:

```json
{
	"code": 1100,
	"message": "Success",
	"requestId": "66fb85e3149bb9e13d6c72161cc6c6cf",
	"btId": "1666684506188",
	"frameDetail": [
		{
			"allLabels": [
				{
					"probability": 0.665125370025635,
					"riskDescription": "Politics:Politics:Politics",
					"riskDetail": {
						"ocrText": {
							"text": "2022/101/25 09:05"
						},
						"riskSource": 1002
					},
					"riskLabel1": "Politics",
					"riskLabel2": "Politics",
					"riskLabel3": "Politics",
					"riskLevel": "REJECT"
				}
			],
			"auxInfo": {
				"similarity": 0.4765625
			},
			"businessLabels": [
				{
					"businessDescription": "Face:FacePosture:FrontFace",
					"businessDetail": {},
					"businessLabel1": "Face",
					"businessLabel2": "FacePosture",
					"businessLabel3": "FrontFace",
					"confidenceLevel": 1,
					"probability": 0.450656906102068
				},
				{
					"businessDescription": "Face:FaceType:RealPeople",
					"businessDetail": {
						"face_num": 1,
						"faces": [
							{
								"face_ratio": 0.00227673095650971,
								"id": "f7bf8842f80a5a2192781064bd69e776",
								"location": [
									352,
									237,
									381,
									278
								],
								"name": "郭荣铿",
								"probability": 0.499512671029603
							}
						]
					},
					"businessLabel1": "Face",
					"businessLabel2": "FaceType",
					"businessLabel3": "RealPeople",
					"confidenceLevel": 2,
					"probability": 0.979977369308472
				}
			],
			"imgText": "2022/101/25 09:05",
			"imgUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/POST_VIDEO%2FPOST_VIDEO_IMG%2F20221025%2Fedaa113581ec1c18df7b44c86d36ae3b_v81.jpg?q-sign-algorithm=sha1&q-ak=AKIDg9LHyOYSAcmfHekZ6NN6XidHflbASUHn&q-sign-time=1666684548%3B1669276548&q-key-time=1666684548%3B1669276548&q-header-list=host&q-url-param-list=&q-signature=d7692c37694f1219092cbd3d7364481ab690d62e",
			"requestId": "edaa113581ec1c18df7b44c86d36ae3b_v81",
			"riskDescription": "Politics:Politics:Politics",
			"riskDetail": {
				"ocrText": {
					"text": "2022/101/25 09:05"
				},
				"riskSource": 1002
			},
			"riskLabel1": "Politics",
			"riskLabel2": "Politics",
			"riskLabel3": "Politics",
			"riskLevel": "REJECT",
			"time": 81
		},
		{
			"allLabels": [
				{
					"probability": 0.553634166717529,
					"riskDescription": "Politics:Politics:Politics",
					"riskDetail": {
						"ocrText": {
							"text": "Realnew 20210/2509:05"
						},
						"riskSource": 1002
					},
					"riskLabel1": "Politics",
					"riskLabel2": "Politics",
					"riskLabel3": "Politics",
					"riskLevel": "REJECT"
				}
			],
			"auxInfo": {
				"similarity": 0.95703125
			},
			"businessLabels": [
				{
					"businessDescription": "Face:FaceType:NoFaces",
					"businessDetail": {},
					"businessLabel1": "Face",
					"businessLabel2": "FaceType",
					"businessLabel3": "NoFaces",
					"confidenceLevel": 1,
					"probability": 0.457959338261496
				}
			],
			"imgText": "Realnew 20210/2509:05",
			"imgUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/POST_VIDEO%2FPOST_VIDEO_IMG%2F20221025%2Fedaa113581ec1c18df7b44c86d36ae3b_v82.jpg?q-sign-algorithm=sha1&q-ak=AKIDg9LHyOYSAcmfHekZ6NN6XidHflbASUHn&q-sign-time=1666684549%3B1669276549&q-key-time=1666684549%3B1669276549&q-header-list=host&q-url-param-list=&q-signature=2606d67861e62622926d9d7f10037d70f068ceb5",
			"requestId": "edaa113581ec1c18df7b44c86d36ae3b_v82",
			"riskDescription": "Politics:Politics:Politics",
			"riskDetail": {
				"ocrText": {
					"text": "Realnew 20210/2509:05"
				},
				"riskSource": 1002
			},
			"riskLabel1": "Politics",
			"riskLabel2": "Politics",
			"riskLabel3": "Politics",
			"riskLevel": "REJECT",
			"time": 82
		}
	],
	"audioDetail": [
		{
			"allLabels": [
				{
					"probability": 0.998463273048401,
					"riskDescription": "Abuse:UncivilizedWords:UncivilizedWords",
					"riskDetail": {
						"audioText": "Fuck you",
						"riskSource": 1001
					},
					"riskLabel1": "Abuse",
					"riskLabel2": "UncivilizedWords",
					"riskLabel3": "UncivilizedWords",
					"riskLevel": "REJECT"
				}
			],
			"audioEndtime": 20,
			"audioStarttime": 10,
			"audioText": "Fuck you",
			"audioUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/POST_VIDEO%2FPOST_VIDEO_AUDIO%2F20221025%2Fedaa113581ec1c18df7b44c86d36ae3b_a0001.wav?q-sign-algorithm=sha1&q-ak=AKIDg9LHyOYSAcmfHekZ6NN6XidHflbASUHn&q-sign-time=1666684511%3B1669276511&q-key-time=1666684511%3B1669276511&q-header-list=host&q-url-param-list=&q-signature=e87204b53077ddc763ddd2b7b5bd5e1382d4cc63",
			"businessLabels": [],
			"requestId": "edaa113581ec1c18df7b44c86d36ae3b_a0001",
			"riskDescription": "Abuse:UncivilizedWords:UncivilizedWords",
			"riskDetail": {
				"audioText": "Fuck you",
				"riskSource": 1001
			},
			"riskLabel1": "Abuse",
			"riskLabel2": "UncivilizedWords",
			"riskLabel3": "UncivilizedWords",
			"riskLevel": "REJECT"
		},
		{
			"allLabels": [
				{
					"probability": 0.857458027460472,
					"riskDescription": "Politics:Government:Government",
					"riskDetail": {
						"audioText": "Fuck Trump",
						"riskSource": 1001
					},
					"riskLabel1": "Politics",
					"riskLabel2": "Government",
					"riskLabel3": "Government",
					"riskLevel": "REJECT"
				}
			],
			"audioEndtime": 40,
			"audioStarttime": 30,
			"audioText": "Fuck Trump",
			"audioUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/POST_VIDEO%2FPOST_VIDEO_AUDIO%2F20221025%2Fedaa113581ec1c18df7b44c86d36ae3b_a0003.wav?q-sign-algorithm=sha1&q-ak=AKIDg9LHyOYSAcmfHekZ6NN6XidHflbASUHn&q-sign-time=1666684511%3B1669276511&q-key-time=1666684511%3B1669276511&q-header-list=host&q-url-param-list=&q-signature=fcf1b1275ca7dbafacaf06bd61cd05f5d612e9dc",
			"businessLabels": [],
			"requestId": "edaa113581ec1c18df7b44c86d36ae3b_a0003",
			"riskDescription": "Politics:Government:Government",
			"riskDetail": {
				"audioText": "Fuck Trump",
				"riskSource": 1001
			},
			"riskLabel1": "Politics",
			"riskLabel2": "Government",
			"riskLabel3": "Government",
			"riskLevel": "REJECT"
		}
	],
	"riskLevel": "REJECT",
	"auxInfo": {
		"frameCount": 2,
		"time": 85
		"passThrough": {
                "passThrough1": "",
                "passThrough2": "",
                "passThrough3": ""
		}
	}
}
```
