# Next Data's Video Stream Moderation API


## Video stream upload request

### Description of the interface

This interface is used to submit related information, such as video stream authentication, and continuously calls back the identification result to the specified callback address after the video stream is stably pulled.

### **Request URL:**
| Colony | URL |
| --- | --- |
| Shanghai (CN) | `http://api-videostream-sh.fengkongcloud.com/videostream/v4` |
| Singapore | `http://api-videostream-xjp.fengkongcloud.com/videostream/v4` |

### **Request method:**

`POST`

### Support protocol:

`HTTP` / `HTTPS`

### **Character encoding:**

`UTF-8`

### **Recommended timeout:**

7s

### **Request parameters:**

The parameters are placed in the HTTP Body in Json format. The specific parameters are as follows:

| **Request<br/>parameter<br/>name** | **Data type** | **Parameter Description** | **Must be<br/>transferred<br/>in or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| accessKey | string | Interface authentication key<br/>It is used for authority authentication | Y | When opening account service, it is provided by NextData or checked at the relevant documents in the upper right corner of the background of NextData by using the opening mailbox |
| appId | string | Application ID, used to distinguish different application data of the same company | Y | If you need to contact us to open, please refer to the value provided by NextData separately |
| eventId | string | identifier for the event | Y | It is used to distinguish scenario data. You need to contact NextData service to open it. Please use the transfer value provided by NextData separately |
| imgType | string | The type of supervision that needs to be identified for the images in the video stream，**This field and the`imgBusinessType` field must select an incoming** | N | **<u>Optional values of supervision level 1 label:</u>**<br/>***POLITY*** :Identify political figures, sensitive political events, etc<br/>***EROTIC*** :Identify NSFW, pornographic content, etc <br/>***VIOLENT*** :Identify violence, terrorists, drugs, guns, etc <br/>***QRCODE*** :Identify QR code<br/>***ADVERT*** :Identify spams, ads<br/>***IMGTEXTRISK*** :Identify OCR violations<br/>If more than one function needs to be identified, connect it by underline, such as **POLITY_QRCODE_ADVERT** which means to identify politics, QR code and spams at the same time<br/>*（Note: This field and the businessType field must select an incoming）*<br/>Political, pornographic and violent terrorism only include the detection of the violation of the picture itself. If it is necessary to identify the violation of the text in the picture, be sure to transfer in type IMGTEXTRISK |
| audioType | string | The type of supervision that needs to be identified for the audio in the video stream，**This field and the`audioBusinessType` field must select an incoming** | N | **<u>Optional values of supervision level 1 label:</u>**<br/>`POLITICAL`：Identify political risks in the voice<br/>`PORN`：Identify NSFW risks in the voice<br/>`AD`：Identify advertisement risks in the voice<br/>`MOAN`：Identify erotic moan risks in the voice<br/>`AUDIOPOLITICAL`：Identify the vioce of the president of China<br/>`ANTHEN`：Identify the sound of the Chinese national anthem<br/>`ABUSE`: Identify abuse risks in the voice<br/>`NONE`:Do not detect audio<br/>If more than one function needs to be identified, connect it by underline, such as **PORN_MOAN** |
| imgBusinessType | string | Types used for business and operations in image, **This field and the`imgType` field must select an incoming** | N | Optional values<br/>[imgBusinessType](#imgBusinessType)<br/> If multiple recognition functions are required, the field and type must be selected to be passed in |
| audioBusinessType | string | Types used for business and operations in audio, **This field and the`audioType` field must select an incoming** | N | **<u>Optional values of supervision level 1 label:</u>**<br/>`SING`：Identifying singing in audio<br/>`MINOR`：Identifying the voice of minors in audio<br/>`GENDER`：Identifying gender in audio<br/>`TIMBRE`：Recognize the timbre in audio, such as little boy's voice，Need to pass in `GENDER` simultaneously to take effect |
| imgCallback | string | Image callback address | Y | Callback the detection result of the captured image in the video stream to the user through this address |
| audioCallback | string | Audio callback address | N | Callback the detection results of audio clips in the video stream to the user through this address; Must be transmitted when audio recognition is required |
| data | json_object | Request data content | Y | The maximum length is 1MB, where the data content is as follows |
| acceptLang | string | Returns the language type of interface information | N | Optional language type of returned information:<br/>`zh`: Chinese<br/>`en`: English<br/>The default is `zh`. If you need to return results in English, this field is required |

<span id="data">The content of data is as follows:</span>

| **Request parameter name** | **Data type** | **Parameter Description** | **Must be transferred in or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| lang | string | Identify language for audio risk support | Y | Optional values:<br/>`zh`:Chinese<br/>`en`:English<br/>`ar`:Arabic<br/>Default:`zh` |
| tokenId | string | Unique ID of client user account | Y | 用于用户行为分析，建议传入用户UID； 最长40位 |
| streamType | string | Video stream type | Y | 可选值为：<br/>`NORMAL`：Normal stream address, currently supported`rtmp`、`rtmps`、`hls`、`http`、`https` agreement<br/>`AGORA`：Agora stream<br/>`TRTC`:TRTC stream<br/>`ZEGO`：ZEGO stream<br/>`VOLC`：VOLC stream<br/>*Note: When using the RTC SDK recording solution, additional recording fees may be incurred on the RTC side. Please consult the relevant RTC manufacturer for specific fees* |
| agoraParam | json_object | Agora stream parameters | N | The agora stream parameter to be detected (required when the streamType is`AGORA`),[agoraParam](#agoraParam) |
| trtcParam | json_object | TRTC stream parameters | N | The trtc stream parameter to be detected (required when the streamType is`TRTC`),[trtcParam](#trtcParam) |
| zegoParam | json_object | ZEGO stream parameters | N | The zego stream parameter to be detected (required when the streamType is`ZEGO`),[zegoParam](#zegoParam) |
| volcParam | json_object | VOLC stream parameters | N | The volc stream parameter to be detected (required when the streamType is`VOLC`),[volcParam](#volcParam) |
| url | string | Video stream URL address to be detected | N | The URL parameter of the stream address to be detected (required when streamType is NORMAL) |
| streamName | string | Stream Name | N | For platform interface display, it is recommended to pass in |
| ip | string | Client IP | N | This parameter is used for user behavior analysis in the IP dimension, and can also be used to compare the NextData's IP black library |
| audioDetectStep | int | Audit step size of audio in video streams | N | The unit is units, with a value range of 1-36 integers. Taking 1 means skipping one 10S audio clip review, taking 2 means skipping two, and so on. All audio content has been reviewed when not using this feature |
| returnAllImg | int | Users can choose whether to return images of different audit results based on their needs | N | The optional values are as follows: (default value is' 0 ')<br/>`0`: callback the image review information of the review and review results<br/>`1`: callback the image review information of all results |
| returnAllText | int | Return the risk level of audio stream segment recognition results | N | The optional values are as follows: (default value is' 0 ')<br/>`0`: returns audio clips and text content with a risk level of non pass<br/>`1`: returns audio clips and text content with all risk levels |
| returnPreText | int | 1 means returning the text content of the 20 second audio clip from the first 10 seconds and the current 10 seconds | N | The optional values are as follows: (default value is' 0 ')<br/>`1`: The returned content field contains the text content of the previous minute of the offending audio<br/>`0`: The returned content field only contains the text content of the offending audio segment |
| returnPreAudio | int | A value of 1 indicates that the audio clip link for the first 10 seconds and the current 10 seconds, totaling 20 seconds, is returned | N | The optional values are as follows: (default value is' 0 ')<br/>`1`: return the audio link one minute before the violation audio<br/>`0`: only return the audio link of the violation segment |
| returnFinishInfo | int | When it is 1, return the end notification when the stream ends | N | The optional values are as follows: (default value is' 0 ')<br/>`1`: Initiate end notification at the end of the audit<br/>`0`: Do not send end notification at the end of the audit. Please refer to [Stream end return parameter](#审核结束回调参数（returnFinishInfo为1时返回）：) for detailed return parameters |
| detectFrequency | float | Frame truncation frequency | N | The unit is seconds, and the value range is 1~60s. When encountering decimals, it is rounded down. If it is less than 1, it will be processed as 1s. If it is not transmitted, it will be truncated once in 3s by default |
| detectStep | int | Video stream frame capture image detection step size | N | The captured image will only be detected once per step, with a value greater than or equal to 1. |
| room | string | Live room/game room number | N | Different strategies can be developed for individual rooms |
| extra | json_object | Extended Information | N | For details, please refer to [extra](#extra) |

<span id="extra">In data, the content of extra is as follows</span>

| **Request parameter name** | **Data type** | **Parameter Description** | **Must be transferred in or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| passThrough | json_object | Pass-through field | N | The content of this field will be returned as is along with the callback result |

<span id="agoraParam">The content of agoraParam is as follows:</span>

| **Request parameter name** | **Data type** | **Parameter Description** | **Must be transferred in or not** | **Detailed description**                                     |
| --- | --- | --- | --- | --- |
| appId | string | Application identification provided by AGORA | Y | |
| channel | string | The channel name provided by AGORA | Y | |
| token | string | | Y | Users with high security requirements can use the token. For details about how to obtain the token, see the Token Generation Method in the AGORA document:[https://docs.agora.io/cn/Recording/token](https://docs.agora.io/cn/Recording/token) |
| channelProfile | int | The channel mode of AGORA recording | N | Optional values are as follows: (Default is' 0 ')<br/>`0`: Communication (default), that is, the common 1 to 1 single chat or group chat, where any user within the channel can speak freely<br/>`1`: Live streaming, there are two user roles: anchor and viewer |
| uid | int | User ID | N | 32-bit unsigned integer. When a token exists, the user ID used to generate the token must be provided. Note that the user uid in the actual room needs to be distinguished here. The **uid** provided for server recording is not allowed to exist in the room |
| subscribeMode | string | Subscription model | N | `AUTO`: Automatically subscribes to all streams in the room, with the default behavior when subscribeMode is not configured<br/>`UNTRUSTED`: In this mode, if the untrustedUserIdList list is empty, the parameter is incorrect because you cannot subscribe to any stream<br/>`TRUSTED`: In this mode, if no user from the untrustedUserIdList enters the room within a certain period of time, Digitme automatically ends the audit. |
| trustedUserIdList | int_array | A list of trusted users | N | subscribeMode takes effect when it is`TRUSTED`and is not allowed to be empty. NextData does not subscribe to user streams specified in the list in the room<br/>A comma-spliced UID array,such as `[1,2]`, the maximum number of users is 17 |
| untrustedUserIdList | int_array | A list of untrusted users | N | subscribeMode akes effect when it is`UNTRUSTED`and is not allowed to be empty. NextData does not subscribe to user streams specified in the list in the room<br/>A comma-spliced UID array,such as `[1,2]`, the maximum number of users is 17 |

**Parameter Usage Description:** If you need to audit different users in the same room, please generate different Uids each time you pass them in to prevent different users in the same room from kicking each other. If you don't have a uid field (in the case of not using tokens), you might consider passing in the appropriate subscription pattern parameter: subscribeMode: When the field is not transferred or is transferred as `AUTO`, all streams in the room are automatically subscribed. When the field is transferred as `TRUSTED`, you need to pass the corresponding `trustedUserIdList` to TrustedUseridList. Please pass in the corresponding `untrustedUserIdList`, NextData only subscribe to the user stream specified in the list in the room.  According to the `subscribeMode` of this field, a specific user will be screened with the corresponding list for moderation.

<span id="trtcParam">The content of trtcParam is as follows:</span>

| **Request parameter name** | **Data type** | **Must be transferred in or not** | **Detailed description**                                     |
| -------------------------- | ------------- | --------------------------------- | ------------------------------------------------------------ |
| SdkAppId                   | int           | Y                                 | sdkAppId provided by Tencent                                 |
| DemoSenses                 | int           | Y                                 | Record type Optional values:<br/>'2': Split recording<br/>'4': Merge recording |
| UserId                     | string        | Y                                 | The userId assigned to the recording segment, with a limit of 32 bits and only allowed to contain (a-zA-Z), numbers (0-9), as well as underscores and conjunctions |
| UserSig                    | string        | Y                                 | Record the verification signature corresponding to userId, which is equivalent to the login password |
| RoomId                     | int           | Y                                 | Room number, value range: [1-4294967294] RoomId and strRoomId must be passed one. If both have values, choose roomId first. Note: Currently, a room can only audit up to 8 users |
| StrRoomId                  | string        | Y                                 | Room Number Value Description: Only (a-zA-Z), numbers (0-9), underscores, and hyphens are allowed. If you choose strRoomId, please note that both strRoomId and roomId have values, and roomId is preferred |

<span id="zegoParam">The content of data.zegoParam is as follows:</span>

| **Request parameter name** | **Data type** | **Parameter Description** | **Must be transferred in or not** | **Detailed description**                                     |
| --- | --- | --- | --- | --- |
| tokenId | string | Zego token | Y | Identity provided by zego_ Token authentication information, used for token login (each session must actively call the zego interface to obtain a new token) |
| streamId | string | Zego stream Id | Y | Zego's stream ID |
| testEnv | bool | Whether to use the zego testing environment | N | The optional values are as follows: (default value is' false ')<br/>`true`: test environment<br/>` false`: formal environment |

<span id="volcParam">The content of data.volcParam is as follows:</span>

| **Request parameter name** | **Data type** | **Must be transferred in or not** | **Detailed description** |
| --- | --- | --- | --- |
| appId | string | Y ||
| roomId | string | Y ||
| userId | string | Y ||
| token | string | Y ||

### Return parameters

The parameters are placed in the HTTP Body in Json format. The specific parameters are as follows:

| **Request parameter name** | **Data type** | **Parameter Description**      | **Must be transferred in or not** | **Detailed description**              |
| --- | --- | --- | --- | --- |
| requestId | string | Request Unique Identification | Y | Request Unique Identification |
| code | int | Return code | Y | See [Interface Response Code List](#Interface Response Code List) |
| message | string | Request return description, corresponding to the Return code | Y | See [Interface Response Code List](#Interface Response Code List) |
| detail | json_object | Description Details | N |  |

The content of detail is as follows:

| **Request parameter name** | **Data type** | **Parameter Description**                                    | **Must be transferred in or not** | **Detailed description**     |
| -------------------------- | ------------- | ------------------------------------------------------------ | --------------------------------- | ---------------------------- |
| errorCode                  | int           | error code                                                   | N                                 | `1001`：Repetitive streaming |
| dupRequestId               | string        | If the errorCode is 1001 and the stream is pushed repeatedly, the dupRequestId field <br/> is returned. For example, the stream was not returned on the first request, but the stream has actually been reviewed. The audit interface cannot be actively closed without requestId. <br/> Can request again and close the audit interface by calling the returned dupRequestId | N                                 |                              |

## Result of asynchronous callback

### Interface description

If the user needs the server to proactively callback the video detection result, specify the URL callback parameter of the callback protocol interface in the request parameters. According to this parameter, the server can proactively callback the user after the video verification is complete.

### Request method:

`POST`

### Character encoding:

`UTF-8`

### **Recommended timeout:**

5s

### Callback strategy

When the user receives the push result and returns HTTP status code 200, the push is successful. Otherwise, the system will push a maximum of 20 times.

### Callback Parameters

The parameters are placed in the HTTP Body in Json format. The specific parameters are as follows:

| **Request parameter name** | **Data type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| -------------------------- | ------------- | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| requestId                  | string        | The unique identifier of this request                        | Y                           | The unique identifier of the request                         |
| code                       | int           | Request Return Code                                          | Y                           | Please refer to the [Interface Response Code List](#Interface Response Code List) for details |
| message                    | string        | Request return description, corresponding to the request return code | Y                           | For details, please refer to [Interface Response Code List](#Interface Response Code List) |
| statCode                   | int           | Callback Status Code                                         | N                           | Status Code Correspondence:<br/>0: Review Result Callback<br/>1: Flow End Result Callback |
| contentType                | int           | Used to distinguish between audio and image callbacks        | N                           | Possible values are as follows:<br/>'1': This callback is an image callback<br/>'2': This callback is an audio callback |
| frameDetail                | json_object   | Risk truncation information                                  | N                           | For details, please refer to [frameDetail](#frameDetail)     |
| audioDetail                | json_object   | Risk audio clip information                                  | N                           | Please refer to [audioDetail](#audioDetail)                  |
| tokenProfileLabels         | json_array    | Account attribute label                                      | N                           | Only returned when the function is enabled. Please refer to [tokenProfileLabels](#tokenProfileLabels) |
| tokenRiskLabels            | json_array    | Account risk label                                           | N                           | Only returned when the function is enabled, see  [tokenRiskLabels](#tokenRiskLabels) |

<span id="frameDetail">When contentType is `1`，the contents of each element of the frameDetail are as follows：</span>

| **Request parameter name** | **Data type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| --- | --- | --- | --- | --- |
| imgUrl | string | URL link for the current frame cut | N | |
| riskLevel | string | Suggestion for handling the current frame cut | N | Possible return values:<br/>`PASS`: Normal, direct release recommended<br/>`REVIEW`: Suspicious, manual review recommended<br/>`REJECT`: Violation, direct interception is recommended |
| riskLabel1 | string | Level 1 risk label | N | Returns `normal`when riskLevel is`PASS` |
| riskLabel2 | string | Level 2 risk label | N | Null when riskLevel is`PASS` |
| riskLabel3 | string | Level 3 risk label | N | Null when riskLevel is`PASS` |
| riskDescription | string | Risk description | N | If the user-defined list is matched, return: 'user-defined list is matched'; Returns: 'normal' when riskLevel is' PASS '; Other cases are displayed in the form of level-1 label: Level-2 label: level-3 label |
| riskDetail | json_object | Risk details | N | See details in [riskDetail](#riskDetail) |
| allLabels | json_array | Full risk labels list | N | See details in [allLabels](#allLabels) |
| businessLabels | json_array | List of business labels, returned when transferred imgBusinessType | N | See details in [businessLabels](#businessLabels) |
| auxInfo | json_object | Other auxiliary information | Y | See details in [auxInfo](#auxInfo2) |

<span id="auxInfo2"> The content of auxInfo in frameDetail are as follows:</span>

| **Request parameter name** | **Data type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| -------------------------- | ------------- | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| imgTime                    | float         | Time when the frame capture image occurred                   | N                           | Time when the video stream frame capture image violation occurred (absolute time) |
| beginProcessTime           | int           | Auxiliary parameter                                          | Y                           | Start processing time (13 bit timestamp)                     |
| finishProcessTime          | int           | Auxiliary parameter                                          | Y                           | End processing time (13 bit timestamp)                       |
| userId                     | int           | VoiceNet user account ID                                     | N                           | Only exists in the case of diversion. The returned userId is the user ID in the actual room, independent of the uid in the request parameters |
| strUserId                  | string        | User ID field for trtc/volc streams                          | N                           | User ID for diversion (only available for 'TRTC' and 'VOLC' streams) |
| detectType                 | int           | Used to distinguish whether the captured image has passed detection | N                           | Possible values are as follows: (This parameter is only returned when the request parameter has passed detectStep)<br/>'1': The captured image has passed detection<br/>2: The captured image has not been detected |
| room                       | string        | Room number                                                  | N                           |                                                              |
| similarityDedup            | int           | Auxiliary parameter                                          | N                           | Possible values are as follows: (Only when the similar frame de re push function takes effect, it is recommended to change the outer layer processing from reject/review to pass to return this parameter, otherwise it will not be returned.)<br/>1: The value is 1, and the similar frame de push function takes effect<br/> |

<span id="riskDetail">In frameDetail, riskDetail contains the following contents:</span>

| **Request parameter name** | **Data type** | **Parameter Description** | **Must be returned or not** | **Detailed description**                                     |
| -------------------------- | ------------- | ------------------------- | --------------------------- | ------------------------------------------------------------ |
| riskSource                 | int           | risk sources              | Y                           | Risk source, optional values:<br/>`1000`: No risk<br/>`1001`: Text risk<br/>`1002`: Visual risk<br/>`1003`: Audio risk |
| faces                      | json_array    | face information          | N                           | See [faces](#faces)                                          |
| face_num                   | int           | face number in image      | N                           |                                                              |
| objects                    | json_array    | object information        | N                           | See [objects](#objects)                                      |
| persons                    | json_array    | person information        | N                           | There will be multiple array elements, up to 10, only when hitting Label :Portrait - Multiple people |
| person_num                 | int           | person number in image    | N                           |                                                              |
| ocrText                    | json_object   | Image text information    | N                           | See [ocrText](#ocrText)                                      |

In riskDetail, the contents of each element of the faces array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| id                 | string             | Person number                                                | N                           | People in the same position in the picture have the same number under different labels.<br/>If the same person appears n times in the picture, assign n IDs |
| name               | string             | Character name                                               | N                           | Identifiable public figure name                              |
| location           | int_array          | The array has four values, representing the coordinates of the upper left corner and the lower right corner. for example<br/>[207,522,340,567]<br/>207 Represents the x coordinate of the upper left corner<br/>522 Represents the y coordinate of the upper left corner<br/>340 Represents the x coordinate of the lower right corner<br/>567 Represents the y coordinate of the lower right corner | N                           |                                                              |
| face_ratio         | float              | Proportion of faces                                          | N                           |                                                              |
| probability        | float              | Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | N                           | Floating point number between 0 and 1                        |

<span id="objects">In riskDetail, the contents of each element of the objects array are as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                          |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------- |
| id                 | string             | Number to ensure that the number of items in the same location is the same under different labels | N                           |                                                   |
| name               | string             | Identification name N                                        | N                           |                                                   |
| location           | int_array          | Identifies the location information. The array has four values, representing the coordinates of the upper left corner and the lower right corner. [207,522,340,567]<br/>207 Represents the x coordinate of the upper left corner<br/>522 Represents the y coordinate of the upper left corner<br/>340 Represents the x coordinate of the lower right corner<br/>567 Represents the y coordinate of the lower right corner | N                           |                                                   |
| probability        | float              | Degree of Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | N                           | Floating point number between 0 and 1             |
| qrContent          | string             | URL information of QR code                                   | N                           | Only returned when the QR code related tag is hit |

In riskDetail, the contents of each element of the persons array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**              |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------- |
| id                 | string             | Number, ensure that the same person has the same number under different labels. If the same person appears n times in the picture, assign n IDs | N                           |                                       |
| person_ratio       | string             | Proportion of portrait in the picture                        | N                           |                                       |
| location           | int_array          | Position coordinates of portrait                             | N                           |                                       |
| probability        | float              | Degree of Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | N                           | Floating point number between 0 and 1 |

<span id="ocrText">In riskDetail, the content of ocrText are as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------ |
| text               | string             | Recognized text                                              | Y                           |                          |
| matchedLists       | json\_array        | Hit customer custom list list                                | N                           |                          |
| riskSegments       | json\_array        | High-risk segment content exists when the detection picture contains risk content such as politics, terrorism, prohibition, advertising law, etc | N                           |                          |

<span id="matchedLists">In ocrText, the content of matchList are as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description**      | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | ------------------------------ | --------------------------- | ------------------------ |
| name               | string             | Hit list name                  | N                           |                          |
| words              | json\_array        | Hit sensitive word information | N                           |                          |

<span id="words">In matchedLists, the contents of each element of the words array are as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description**   | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | --------------------------- | --------------------------- | ------------------------ |
| word               | string             | Hit sensitive words         | N                           |                          |
| position           | int_array          | Location of sensitive words | N                           |                          |

<span id="riskSegments">In ocrText, the details of each element of riskSegments are as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description**               | **Must be returned or not** | **Detailed description**  |
| ------------------ | ------------------ | --------------------------------------- | --------------------------- | ------------------------- |
| segment            | string             | High-risk content fragments             | N                           |                           |
| position           | int_array          | Location of high-risk content fragments | N                           | The subscript starts at 0 |

<span id="allLabels">In frameDetail, the contents of each member of the allLabels array are as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                 |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | -------------------------------------------------------- |
| riskLabel1         | string             | Level 1 risk label                                           | Y                           |                                                          |
| riskLabel2         | string             | Level 2 risk label                                           | Y                           |                                                          |
| riskLabel3         | string             | Level 3 risk label                                           | Y                           |                                                          |
| riskDescription    | string             | Risk causes                                                  | Y                           |                                                          |
| riskLevel          | string             | Disposal suggestions                                         | Y                           |                                                          |
| probability        | float              | Degree of Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | Y                           |                                                          |
| riskDetail         | json_object        | See the riskDetail under result for risk details and field contents | Y                           | It's the same as the riskDetail structure in frameDetail |

<span id="businessLabels"><span id="allLabels">In frameDetail, the contents of each member of the businessLabels array are as follows:</span></span>

| **Request parameter name** | **Data type** | **Parameter Description** | **Must be returned or not** | **Detailed description**                                 |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | level 1 label | Y |  |
| businessLabel2 | string | level 2 label | Y |  |
| businessLabel3 | string | level 3 label | Y |                          |
| businessDescription | string | label description | Y |  |
| confidenceLevel | int | Level of Confidence, The optional value is between 0 and 2. The higher the value, the higher the confidence | Y |  |
| probability | float | Degree of Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | Y |  |
| businessDetail | Json_object |  | Y |  |

In businessLabels, the contents of each element of the businessDetail array are as follows:

| **Request parameter name** | **Data type** | **Parameter Description** | **Must be transferred in or not** | **Detailed description**                                     |
| -------------------------- | ------------- | ------------------------- | --------------------------------- | ------------------------------------------------------------ |
| faces                      | json_array    | face details in image     | N                                 |                                                              |
| face_num                   | int           | Face number               | N                                 |                                                              |
| objects                    | json_array    | object details in image   | N                                 |                                                              |
| persons                    | Json_array    | person details in image   | N                                 | There will be multiple array elements, up to 10, only when hitting Portrait - Multiple people |
| person_num                 | int           | person number             | N                                 |                                                              |

In businessDetail, the contents of each element of the faces array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| id                 | string             | Person number                                                | N                           | People in the same position in the picture have the same number under different labels.<br/>If the same person appears n times in the picture, assign n IDs |
| name               | string             | Character name                                               | N                           | Identifiable public figure name                              |
| location           | int_array          | The array has four values, representing the coordinates of the upper left corner and the lower right corner. for example<br/>[207,522,340,567]<br/>207 Represents the x coordinate of the upper left corner<br/>522 Represents the y coordinate of the upper left corner<br/>340 Represents the x coordinate of the lower right corner<br/>567 Represents the y coordinate of the lower right corner | N                           |                                                              |
| face_ratio         | float              | Proportion of faces                                          | N                           |                                                              |
| probability        | float              | Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | N                           | Floating point number between 0 and 1                        |

<span id="objects">In businessDetail, the contents of each element of the objects array are as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**              |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------- |
| id                 | string             | Number to ensure that the number of items in the same location is the same under different labels | N                           |                                       |
| name               | string             | Identification name N                                        | N                           |                                       |
| location           | int_array          | Identifies the location information. The array has four values, representing the coordinates of the upper left corner and the lower right corner. [207,522,340,567]<br/>207 Represents the x coordinate of the upper left corner<br/>522 Represents the y coordinate of the upper left corner<br/>340 Represents the x coordinate of the lower right corner<br/>567 Represents the y coordinate of the lower right corner | N                           |                                       |
| probability        | float              | Degree of Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | N                           | Floating point number between 0 and 1 |

In businessDetail, the contents of each element of the persons array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**              |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------- |
| id                 | string             | Number, ensure that the same person has the same number under different labels. If the same person appears n times in the picture, assign n IDs | N                           |                                       |
| person_ratio       | string             | Proportion of portrait in the picture                        | N                           |                                       |
| location           | int_array          | Position coordinates of portrait                             | N                           |                                       |
| probability        | float              | Degree of Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | N                           | Floating point number between 0 and 1 |

<span id="audioDetail">When contentType is `2`，the contents of each element of the audioDetail are as follows：</span>

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| --- | --- | --- | --- | --- |
| audioUrl | string | URL of the audio segment | Y | |
| riskLevel          | string             | Disposition recommendation for the current event             | Y                           | `PASS`: Normal content<br/>`REVIEW`: Suspicious content<br/>`REJECT`: Violating content |
| riskLabel1         | string             | The first-level label, which is parallel to other first-level labels. Returns `normal` when riskLevel is `PASS` | Y                           | First-level label                                            |
| riskLabel2         | string             | The second-level label, which belongs to the first-level label. Empty when riskLevel is `PASS` | Y                           | Second-level label                                           |
| riskLabel3         | string             | The third-level label, which belongs to the second-level label. Empty when riskLevel is `PASS` | Y                           | Third-level label                                            |
| riskDescription    | string             | Label explanation                                            | Y                           | Chinese name in the format of "first-level risk label: second-level risk label: third-level risk label"<br/>Returns ***\*Hit user-defined list\**** when hitting the user-defined list |
| riskDetail         | json_object        | Detailed risk information                                    | Y                           | See [riskDetail description](#riskDetail2)                   |
| allLabels          | json_array         | All risk label lists                                         | Y                           | All risk label lists. See [allLabels description](#allLabels2) |
| businessLabels     | json_array         | Business label list                                          | N                           | Returns when audioBusinessType is passed in. See [businessLabels description](#businessLabels2) |
| auxInfo | json_object | Other information | Y | See [auxInfo](#auxInfo3) |

<span id="auxInfo3">auxInfo in audioDetail is as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description**           | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ----------------------------------- | --------------------------- | ------------------------------------------------------------ |
| BeginProcessTime   | int                | Auxiliary parameter                 | Y                           | Start processing time (13 bit timestamp)                     |
| FinishProcessTime  | int                | Auxiliary parameter                 | Y                           | End processing time (13 bit timestamp)                       |
| Audio_ Starttime   | string             | auxiliary parameters                | Y                           | violation attribute start time (absolute time)               |
| Audio_ Endtime     | string             | Auxiliary parameters                | Y                           | Violation attribute end time (absolute time)                 |
| UserId             | int                | VoiceNet user account ID            | N                           | Only exists in the case of diversion. The returned userId is the user ID in the actual room, independent of the uid in the request parameters |
| StrUserId          | string             | User ID field for trtc/volc streams | N                           | User ID for diversion (only available for 'TRTC' and 'VOLC' streams) |
| PassThrough        | JSON_ Object       | transparent field                   | N                           | The attribute of this field is the same as the passThrough value of extra in the request parameter data |
| Room               | string             | Room number                         | N                           |                                                              |

<span id="riskDetail2">In audioDetail, the details of riskDetail are as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description**                  | **Must be returned or not** | **Detailed description**                                     |
| --- | --- | --- | --- | --- |
| riskSource         | int                | Risk source                                | Y                           | Risk source, optional values:<br/>`1000`: No risk<br/>`1001`: Text risk<br/>`1002`: Visual risk<br/>`1003`: Audio risk |
| audioText          | string             | The result of transcription from the audio | N                           |                                                              |
| matchedLists       | json_array         | Hit customer-defined list information      | N                           | Returns when hitting the customer-defined list, otherwise does not exist. See [matchedLists description](#matchedLists2) |
| riskSegments       | json_array         | High-risk content segment                  | N                           | Exists when using the political, violent, prohibited, competitor, advertising laws, and other functions. See [riskSegments description](#riskSegments2) |

The detailed content of each element in matchedLists in riskDetail is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                      | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ---------------------------------------------- | --------------------------- | ------------------------------------------------------------ |
| name               | string             | Custom name of the customer-defined list       | N                           |                                                              |
| words              | json_array         | Information of sensitive words in the hit list | N                           | Starting from 0, see words description (#words2) for details |

In the matchedLists array, the detailed content of each element in words is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**      | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | ------------------------------ | --------------------------- | ------------------------ |
| word               | string             | Sensitive word                 | N                           |                          |
| position           | int_array          | Position of the sensitive word | N                           | Starting from 0          |

In riskDetail, the detailed content of each element in riskSegments is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                 | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | ----------------------------------------- | --------------------------- | ------------------------ |
| segment            | string             | High-risk content segment                 | N                           |                          |
| position           | int_array          | Position of the high-risk content segment | N                           | Starting from 0          |

In the audioDetail array, the content of each element in the allLabels array is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**  | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | -------------------------- | --------------------------- | ------------------------------------------------------------ |
| riskLabel1         | string             | First-level risk label     | Y                           | First-level risk label                                       |
| riskLabel2         | string             | Second-level risk label    | Y                           | Second-level risk label                                      |
| riskLabel3         | string             | Third-level risk label     | Y                           | Third-level risk label                                       |
| riskDescription    | string             | Risk description           | Y                           | Chinese name in the format of "first-level risk label: second-level risk label: third-level risk label"<br/>Returns Hit user-defined list when hitting the user-defined list |
| riskLevel          | string             | Disposition recommendation | Y                           | ***\*PASS\****: Normal content<br/>***\*REVIEW\****: Suspicious content<br/>***\*REJECT\****: Violating content |
| probability        | float              | Confidence level           | Y                           | Optional values range from 0 to 1. The larger the value, the higher the confidence level. |
| riskDetail         | json_object        | Risk detail                | Y                           | Same structure as riskDetail in audioDetail                  |

<span id="businessLabels2">For each member in the businessLabels array in audioDetail, the details are as follows:</span>

| **Parameter name**  | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description**                                     |
| ------------------- | ------------------ | ------------------------- | --------------------------- | ------------------------------------------------------------ |
| businessLabel1      | string             | First-level label         | Y                           | First-level label                                            |
| businessLabel2      | string             | Second-level label        | Y                           | Second-level label                                           |
| businessLabel3      | string             | Third-level label         | Y                           | Third-level label                                            |
| businessDescription | string             | Label description         | Y                           | Chinese name in the format of "first-level label: second-level label: third-level label" |
| confidenceLevel     | int                | Confidence level          | Y                           | Optional values between 0 and 2. The higher the value, the higher the confidence level |
| probability         | float              | Confidence score          | Y                           | Optional values between 0 and 1. The higher the value, the higher the confidence score |
| businessDetail      | json_object        | Detailed information      | Y                           |                                                              |

<span id="tokenProfileLabels">For each member in the tokenProfileLabels array, the details are as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description**                |
| ------------------ | ------------------ | ------------------------- | --------------------------- | --------------------------------------- |
| label1             | string             | First-level label         | N                           |                                         |
| label2             | string             | Second-level label        | N                           |                                         |
| label3             | string             | Third-level label         | N                           |                                         |
| description        | string             | Label description         | N                           |                                         |
| timestamp          | int                | Tagging timestamp         | N                           | 13-digit Unix timestamp in milliseconds |

<span id="tokenRiskLabels">The tokenRiskLabels array has the same specific field as the tokenProfileLabels for each member</span>

### Audit end callback parameter（returnFinishInfo = 1）：

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| ----------------- | ----------- | -------------------------------------------- | ------------ | ------------------------------------------------------------ |
| code              | int         | Return code                         | Y           | See [Interface Response Code List](#Interface Response Code List) |
| message           | string      | Request return description, corresponding to the request return code | Y           | See [Interface Response Code List](#Interface Response Code List) |
| requestId         | string      | Request unique identification    | Y           |                                                              |
| statCode          | int         | Callback status code               | Y           | Callback status code, which exists when returnFinishInfo is true. Mapping between status codes: <br/>0: audit result callback <br/>1: stream end result callback <br/> When statCode=1, the following parameters exist |
| contentType       | int         | Used to distinguish between audio and picture callbacks. Returns when code equals 1100 | Y           | Possible values are as follows: <br/> '1' : The image callback <br/> '2' : the audio callback |
| riskLevel              | string         | Return the handling suggestion for the overall stream at the end of the callback | Y           | Return the handling suggestion for the overall stream at the end of the callback |
| pullStreamSuccess | bool        | Whether the pull stream succeeds | Y          | Possible values are as follows: <br/> 'true' : Pull-out succeeds <br/> 'false' : pull-out fails <br/> If no screenshots are obtained, pull-out fails |
| detail            | json_object |                                      | Y           | See [detail](#detail2) |
| auxinfo         | json_object | other information                  | Y           | See [auxinfo](#auxinfo2) |

<span id="detail2">The detail in the end callback is as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description**                        | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | ------------------------------------------------ | --------------------------- | ------------------------ |
| requestParams      | json_object        | Returns all fields in the request parameter data | Y                           |                          |

The auxInfo field structure is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description**                                     |
| --------------- | ----------- | ---------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| errorCode       | int         | 状态码           | Y           | <p> Status code </p><p>3001: The access to the stream address fails. For example, HTTP status codes 404 and 403</p><p>3002: The stream data is Invalid, for example, Invalid data found when processing input </p><p>3003: The stream does not exist. For example, zego returns 197612 error code </p><p>3004: The stream does not return audio data. </p><p>3005: The pull stream token is invalid or expired. |
| streamTime       | int         | 流审核时长           | N           | The last return after the end of the stream represents the time of sending for review. If there is interval review logic, it may not be consistent with the real time of the stream |

## Video stream shutdown interface

### Interface Description

This interface is used by the client to notify the server that a video stream is closed.

### Request URL:

| Computer cluster | URL | Support language |
| --- | --- | --- |
| Shanghai | `http://api-videostream-sh.fengkongcloud.com/finish_videostream/v4` | Chinese |
| Singapore | `http://api-videostream-xjp.fengkongcloud.com/finish_videostream/v4` | Chinese<br/>English<br/>Arabic |
| Silicon Valley | `http://api-videostream-gg.fengkongcloud.com/finish_videostream/v4` | Chinese<br/>English<br/>Arabic |
| India | `http://api-videostream-yd.fengkongcloud.com/finish_videostream/v4` | Chinese |

### Request Method:

`POST`

### Support protocol:

`HTTP`或`HTTPS`

### Character Encoding:

`UTF-8`

### Suggested Timeout:

1s

### Request Parameters:

It is stored in the HTTP Body in Json format. The specific parameters are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be transferred in or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| accessKey | string | Company key | Y | It is used for authority authentication. It is provided by NextData when opening account service |
| requestId | string | The unique identity of this request | Y | The requestId of the video stream needs to be turned off |

### Return Parameters:

It is stored in the HTTP Body in Json format. The specific parameters are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| --- | --- | --- | --- | --- |
| requestId | string | The unique identity of this request | Y |  |
| code | int | Return code | Y | See [Interface Response Code List](#Interface Response Code List) |
| message | string | Request return description, corresponding to the request return code | Y | See [Interface Response Code List](#Interface Response Code List) |

## imgBusinessType

| Business label identification type | Description                                                  |
| ---------------------------------- | ------------------------------------------------------------ |
| AGE                                | Identify the age of the person                               |
| GENDER                             | Identify the gender of the person                            |
| BEAUTY                             | Judge whether a person is good-looking or not                |
| RACE                               | Identify race                                                |
| FACEDETECTION                      | Such as recognition of real person, face mask, front face, side face, etc |
| FAKEFACE                           | Judge whether there is a disguised face in the target image  |
| FACECOMPARE                        | Compare the face similarity between two pictures             |
| PUBLICFIGURE                       | Identify famous Chinese stars, online celebrities, etc       |
| TAINTEDSTAR                        | Identify China's bad artists                                 |
| POSTURE                            | Such as recognizing sitting position, kneeling position, etc |
| DRESS                              | Such as identifying jk and hanfu                             |
| TEMPERAMENT                        | Identify a person's physical temperament                     |
| BODY                               | Such as recognizing hair, eyes, nose, etc                    |
| PICTUREFORM                        | Such as recognition of animation, expression package, etc    |
| PICTURESTRUCT                      | Identify low quality pictures such as grid diagram, bridge section diagram, etc |
| LOWVISION                          | Such as recognition blur, smear, mosaic, etc                 |
| LOWCONTNET                         | For example, identification points and lines are dense, insects are dense, etc |
| LIVEPICTURE                        | Identify live broadcast in bed and driving                   |
| SCREENSHOT                         | Such as screenshots of identifying circle of friends, chat, etc |
| FITNESS                            | Identify scenes with fitness equipment                       |
| CATE                               | Identify scenes with delicious food                          |
| MUSIC                              | Identify scenes with music elements                          |
| SPORTS                             | Identify scenes with sports elements                         |
| SCENERY                            | Identify scenes with natural scenery                         |
| CITYVIEW                           | Identify scenes with urban scenery                           |
| 3CPRODUCTSLOGO                     | Such as Sony, SanDisk, PHLIPS, etc                           |
| SOCIALAPPSLOGO                     | Such as UPLIVE, yalla, Twitter, etc                          |
| PHOTOMATERIALLOGO                  | Such as miaopai, etc                                         |
| NEWSAPPSLOGO                       | Such as Sina, etc                                            |
| ENTERTAINMENTAPPSLOGO              | Such as Kwai, TikTok, etc                                    |
| SPORTSLOGO                         | Identify the Olympic logo                                    |
| APPARELLOGO                        | Such as adidas, BALENCIAGA, Lamborghini, etc                 |
| ACCESSORIESLOGO                    | Such as Tiffany & Co, SWAROVSKI, etc                         |
| COSMETICSLOGO                      | Such as ESTEE LAUDER, AZZARO, etc                            |
| FOODLOGO                           | Such as Starbucks, Coca-Cola, etc                            |
| VEHICLE                            |                                                              |
| BUILDING                           |                                                              |
| TABLEWARE                          |                                                              |
| FOOD                               | Identify scenes with common foods                            |
| HOMEAPPLICATION                    |                                                              |
| OFFICESUPPLIES                     | Identify common office supplies such as printers             |
| FASHION                            | Identify scenes with common wear such as ring, earrings      |
| SPORTEQUIPMENT                     |                                                              |
| TOY                                |                                                              |
| MAKEUP                             |                                                              |
| DRUGS                              | Identify drugs such as traditional Chinese medicine          |
| PAINTING                           |                                                              |
| ELECTRONIC                         | Identify electronic products such as earphones and laptops   |
| MEDICALIMAGE                       | Identify NMR images                                          |
| FURNITURE                          | Identify common household items such as fireplaces, bathtubs |
| DAILYSUPPLIES                      | Identify common daily necessities such as books, mirrors, etc |
| CONSTELLATION                      | Identify things related to astrology and divination          |
| KITCHENWARE                        |                                                              |
| KEEPSAKE                           |                                                              |
| MAMMAL                             |                                                              |
| BIRDS                              |                                                              |
| REPTILE                            |                                                              |
| FISH                               |                                                              |
| ARTHROPOD                          |                                                              |
| COELENTERATE                       |                                                              |
| MOLLUSKS                           |                                                              |
| CRUSTACEAN                         |                                                              |
| PLANT                              |                                                              |
| SETTING                            | Identify various places such as gym                          |

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

## DEMO

### Demo of request:

```json
{
    "accessKey": "*********",
    "appId": "defaulttest",
    "eventId": "VIDEOSTREAM",
    "imgType": "POLITICS_VIOLENCE_BAN_PORN_MINOR_AD_SPAM_LOGO_STAR",
    "imgBusinessType": "SCREEN_SCENCE_QR_FACE_QUALITY_MINOR_LOGO_BEAUTY",
    "audioType": "POLITICAL_PORN_AD_MOAN",
    "audioBusinessType": "SING_LANGUAGE_MINOR_GENDER_TIMBRE",
    "imgCallback": "http://www.xxx.top/xxx",
    "audioCallback": "http://www.xxx.top/xxx",
    "acceptLang": "en",
    "data": {
        "streamType": "NORMAL",
        "tokenId": "123",
        "ip": "123.171.34.4",
        "detectFrequency": 10,
        "detectStep": 1,
        "room": "5e1854a6a0a79d0001a09bc3",
        "url": "http://rtmp.xxxx.cn/live/3637778raLSXdOdu.flv",
        "returnAllImg": 1,
        "returnAllText": 1,
        "returnPreText": 1,
        "returnPreAudio": 1,
        "lang": "en",
        "extra": {
            "passThrough": {
                "passThrough1": "111",
                "passThrough2": "222",
                "passThrough3": "333"
            }
        }
    }
}
```

### Demo of the uploaded interface:

```json
{
    "code":1100,
    "message":"Success",
    "requestId":"d05f4270374ca516ce7aafc0139afd25"
}
```

### Demo of asynchronous callback results:

contentType = 1
```json
{
    "code": 1100,
    "contentType": 1,
    "message": "Success",
    "requestId": "1639825145166_vs130_1639825248361471656",
    "frameDetail": {
        "allLabels": [
            {
                "riskDescription": "Erotic:Erotic:Erotic",
                "riskLabel1": "Erotic",
                "riskLabel2": "Erotic",
                "riskLabel3": "Erotic",
                "riskLevel": "REJECT"
            }
        ],
        "auxInfo": {
            "beginProcessTime": 1639825248361,
            "detectType": 1,
            "finishProcessTime": 1639825248809,
            "imgTime": "2021-12-18 19:00:48.375",
            "room": "5e1854a6a0a79d0001a09bc3"
        },
        "businessLabels": [],
        "imgUrl": "http://bj.cos.ap-beijing.xxx.com/image/1639825145166_vs130_1639825248361471656.jpg",
        "riskDescription": "Erotic:Erotic:Erotic",
        "riskDetail": {
            "ocrText": {
                "text": "Fuck U Mother"
            },
            "riskSource": 1002
        },
        "riskLabel1": "Erotic",
        "riskLabel2": "Erotic",
        "riskLabel3": "Erotic",
        "riskLevel": "REJECT"
    },
    "auxInfo": {
        "passThrough": {
            "passThrough1": "111",
            "passThrough2": "222",
            "passThrough3": "333"
        }
    }
}
```

contentType = 2
```json
{
    "requestId":"y28f8a4f1264085b321f12223wqed1121retestpvvvvv44321we12_3",
    "code":1100,
    "message":"Success",
    "contentType":2,
    "audioDetail":{
        "allLabels":[
            {
                "riskDescription":"Moan:Moan:Moan",
                "riskLabel1":"MoanMoanMoan",
                "riskLabel2":"MoanMoan",
                "riskLabel3":"Moan",
                "riskLevel":"REJECT"
            }
        ],
        "audioText":"Uhhhhhh",
        "audioUrl":"http://bj-voice-mp3-1251671073.cos.ap-beijing.myqcloud.com/POST_VIDEOSTREAM%2FPOST_VIDEOSTREAM_AUDIO%2FMP3%2F20221027%2Fy28f8a4f1264085b321f12223wqed1121retestpvvvvv44321we12_3.mp3?q-sign-algorithm=sha1&q-ak=AKIDg9LHyOYSAcmfHekZ6NN6XidHflbASUHn&q-sign-time=1666876123%3B1669468123&q-key-time=1666876123%3B1669468123&q-header-list=host&q-url-param-list=&q-signature=f32da45be186fd4a8ed063e499d3f4e0f4f5fc19",
        "auxInfo":{
            "audioEndTime":"2022-10-27 21:08:42",
            "audioStartTime":"2022-10-27 21:08:32",
            "beginProcessTime":1666876123332,
            "finishProcessTime":1666876123893,
            "room":"y1123413312ewe24sv2"
        },
        "businessLabels":[

        ],
        "content":"Uhhhh",
        "preAudioUrl":"http://bj-voice-mp3-1251671073.cos.ap-beijing.myqcloud.com/POST_VIDEOSTREAM%2FPOST_VIDEOSTREAM_AUDIO%2FMP3%2F20221027%2Fy28f8a4f1264085b321f12223wqed1121retestpvvvvv44321we12_3_pre.mp3?q-sign-algorithm=sha1&q-ak=AKIDg9LHyOYSAcmfHekZ6NN6XidHflbASUHn&q-sign-time=1666876123%3B1669468123&q-key-time=1666876123%3B1669468123&q-header-list=host&q-url-param-list=&q-signature=449fdcab8a3c11d5132f43f78c61e6663f5c08d6",
        "riskDescription":"Moan:Moan:Moan",
        "riskDetail":{
            "audioText":"Uhhhhhhh",
            "riskSource":1001
        },
        "riskLabel1":"Moan",
        "riskLabel2":"Moan",
        "riskLabel3":"Moan",
        "riskLevel":"REJECT"
    },
    "requestParams":{
        "dedup":"room",
        "eventId":"nickname",
        "isVideoNewStream":true,
        "returnAlltext":1,
        "returnFinishInfo":1,
        "returnPreAudio":1,
        "returnPreText":1,
        "role":"USER",
        "room":"y1123413312ewe24sv2",
        "streamType":"DEFAULT",
        "tokenId":"tokenzyp",
        "url":"rtmp://10.141.3.139:1936/live/stream"
    },
    "statCode":0
}
```

### Demo of closing an interface request:

```json
{
    "accessKey": "xxxxxxxx",
    "requestId": "1639825145166"
}
```

### Demo of closing an interface return:

```json
{
    "code":1100,
    "message":"Success",
    "requestId":" a78eef377079acc6cdec24967ecde722"
}
```
