# Shumei Intelligent Image Recognition Product API Documentation


# Synchronous Single Image Upload Interface

## Synchronous Single Request

### Request URL:
| Cluster | URL |
| --- | --- | 
| Beijing | `http://api-img-bj.fengkongcloud.com/image/v4` | 
| Shanghai | `http://api-img-sh.fengkongcloud.com/image/v4` | 
| Singapore | `http://api-img-xjp.fengkongcloud.com/image/v4` | 
| India | `http://api-img-yd.fengkongcloud.com/image/v4` | 
| Silicon Valley | `http://api-img-gg.fengkongcloud.com/image/v4` | 

### Request Method:

`POST`

### Character Encoding:

`UTF-8`

### Suggested Timeout: 10s
Currently, the connection timeout for downloading images on Shumei's side is 2s, the read timeout is 3s, and the internal average processing time is around 500ms (specific duration is related to request type and image size). If the download fails, it will retry once. It is recommended that customers set the timeout to 7-10s.

### Request Parameters:

Placed in the HTTP Body in JSON format, the specific parameters are as follows:

| **Parameter Name** | **Type** | **Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| accessKey | string | Interface authentication key<br/>Used for permission authentication, provided by Shumei when opening account services or viewed in the relevant documentation in the upper right corner of the Shumei backend when logging in with the opening email | Required |  |
| appId | string | Application identifier, used to distinguish different application data of the same company | Required | Need to contact Shumei to open, please use the value provided by Shumei |
| eventId | string | Event identifier | Required | Need to contact Shumei service to open, please use the value provided by Shumei |
| type | string | Risk type to be detected | Optional | Regulatory primary label Optional values:<br/>POLITY: Political recognition<br/>EROTIC: Pornographic & sexy violation recognition<br/>VIOLENT: Violent & prohibited recognition<br/>QRCODE: QR code recognition<br/>ADVERT: Advertisement recognition<br/>IMGTEXTRISK: Image text violation recognition<br/>If multiple functions need to be recognized, connect them with underscores, such as POLITY_QRCODE_ADVERT for political, QR code, and advertisement combined recognition<br/>(Either this field or the businessType field must be passed)<br/>Political, pornographic, and violent only include the violation detection of the image itself. If you need to recognize the violation content in the image text, be sure to pass the image text violation recognition function |
| businessType | string | Business label type | Optional | Business label<br/>Optional values: [See Appendix](#Appendix) If multiple recognition functions are needed, connect them with underscores. Either this field or the type field must be passed |
| data | json_object | Request data content | Required | Request data content, the length of the data field is up to 10MB, [see data parameters](#data) |
| callback | string | Callback request URL, passing callback indicates asynchronous callback logic, otherwise synchronous logic. Callback HTTP address field, when this field is not empty, the service will notify the user of the review result based on this field. The address must be a standard URL of HTTP or HTTPS | Optional | Asynchronous callback logic supports 30M images<br/>Synchronous supports 10M images<br/>Both asynchronous single and asynchronous batch need to call the query interface to check the results; The synchronous interface cannot call the query. If the callback is passed, the result will be called back to the corresponding server. If the callback is not passed, it will return synchronously |

<span id="data">Among them, the content of data is as follows:</span>

| **Parameter Name** | **Type** | **Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| tokenId | string | User account identifier, it is recommended to use your company's user UID (can be encrypted) to generate it yourself, identifying the user's unique identity for risk control of behaviors such as flooding and advertising. If there is no user UID scenario, it is recommended to use a unique data identifier to pass the value | Required | A string of up to 64 characters composed of numbers, letters, underscores, and hyphens |
| img | string | The image to be detected, can use base64 encoded image data or image URL link **It is recommended to download the image from the CDN source station, and the source station should not be a single point<br/>Risk: If it is not downloaded from the source station, there may be image download failures, resulting in the inability to review** | Required | Supported formats:<br/>`jpg`, `jpeg`, `png`, `webp`, `gif`, `tiff`, `tif`, `heif`<br/>It is recommended that the image pixel is not less than 256*256, currently supports images with resolutions between 20*20 and 6000*6000, the maximum image size is 10MB, and the maximum for asynchronous is 30M <br/>By default, long images are not split. If needed, please contact Shumei to open it. The billing is based on the actual number of frames captured.|
| imgCompareBase | string | The base image to be compared, exists when the Type field of the request parameter contains the label `FACECOMPARE`<br/>Can use base64 encoded image data or image URL link | Optional | Supported formats:<br/>`jpg`, `jpeg`, `png`, `webp`, `gif`, `tiff`, `tif`, `heif`<br/>It is recommended that the image pixel is not less than 256*256, the maximum image size is 10MB<br/><br/>The base image does not currently support long images and animated image formats |
| role | string | User role | Optional | User role, must be within the valid range to configure different strategies for different roles. (Default is `USER`)<br/>Live broadcast field values:<br/>`ADMIN`: Room manager<br/>`HOST`: Host<br/>`SYSTEM`: System role<br/>Game field values:<br/>`ADMIN`: Administrator<br/>`USER`: Ordinary user |
| ip | string | IP address | Optional | The public IPv4 address of the user sending the image |
| lang | string | Language type | Optional | When the request type contains IMGTEXTRISK, the corresponding detection language type can be specified, optional values:<br/>zh: Chinese<br/>en: English<br/>ar: Arabic<br/>The default is Chinese detection. Please note that passing in values outside the optional values is invalid |
| deviceId | string | Shumei device fingerprint identifier | Optional | The unique identifier generated by Shumei device fingerprint |
| maxFrame | int | Maximum frame number of gif images | Optional | The number of frames captured for gifs and other animated images, the maximum is 20 frames, the default is 3 frames, and the billing is based on the actual number of frames captured. If the default is to capture 3 frames, it will be billed according to 3 frames |
| interval | int | Detection interval of gif frame images | Optional | The default value is `1`, which means that each frame needs to be detected. The service will automatically adjust this value to ensure complete coverage of all frames |
| extra | json_object | Auxiliary parameters | Optional | Related information for auxiliary detection, [see extra parameters](#extra) |
| streamInfo | json_object | Similar frame review parameters | Optional | Related information for detecting similar frames, [see streamInfo parameters](#streamInfo), if you need to understand or use the similar frame function, please contact customer service |
| receiveTokenId | string | Receiver's tokenId | Optional | Receiver's tokenId, required in private chat scenarios |
| dataId | string | Customer custom data Id | Optional | Can be used for Shumei SaaS backend retrieval |

<span id="streamInfo">In data, the content of streamInfo is as follows:</span>

| **Parameter Name** | **Type** | **Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| similarDedup | bool | Whether to enable similar function | No |  |
| streamId | string | Pass-through parameter | No | Unique identifier id, required when similarDedup is true |
| timeWindow | int | Pass-through parameter | No | Time window, in seconds. Required when similarDedup is true  |
| frameTime | int | Pass-through parameter | No | Frame time, in ms, required when similarDedup is true |
| riskNum | int | Pass-through parameter | No | The number of similar frame images does not change. The quantity range is (1-5), the default is 1 |

<span id="extra">In data, the content of extra is as follows:</span>

| **Parameter Name** | **Type** | **Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| isIgnoreTls | bool | Auxiliary parameter to control whether to ignore CA certificate verification | No | Optional values (default is `false`):<br/>`true`: Ignore certificate trust<br/>`false`: Verify certificate |
| passThrough | json_object | Pass-through parameter | No | Customer pass-through field, Shumei will not recognize and process this field internally, and will return it to the user with the result. It must be of json_object type |
| isTokenSeparate | int | Whether to distinguish accounts under different applications | No | Whether to distinguish accounts under different applications, possible values:<br/>0: Do not distinguish<br/>1: Distinguish<br/>The default value is 0 |
| room | string | Live room number, it is recommended to pass in the room number for high-exposure group chat and other business scenarios | No |  |

## Synchronous Return Result

<span id="singleRet">Placed in the HTTP Body in JSON format, the specific parameters are as follows:</span>

| **Parameter Name** | **Type** | **Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| code | int | Return code | Yes |  `1100`: Success<br/>`1901`: QPS limit exceeded<br/>`1902`: Invalid parameter<br/>`1903`: Service failure<br/>`1911`: Image download failure<br/>`9101`: No permission to operate<br/> |
| message | string | Return code description | Yes | Corresponds to code: Success<br/>QPS limit exceeded<br/>Invalid parameter<br/>Service failure<br/>Image download failure<br/>No permission to operate |
| requestId | string | Request identifier | Yes | Unique request identifier, used for troubleshooting and subsequent effect optimization, strongly recommended to save |
| riskLevel | string | Disposal suggestion | Yes | Possible return values:<br/>`PASS`: Normal, recommended to pass directly<br/>`REVIEW`: Suspicious, recommended for manual review<br/>`REJECT`: Violation, recommended to intercept directly |
| riskLabel1 | string | Primary risk label | Yes | When riskLevel is `PASS`, return `normal` |
| riskLabel2 | string | Secondary risk label | Yes | When riskLevel is `PASS`, it is empty |
| riskLabel3 | string | Tertiary risk label | Yes | When riskLevel is `PASS`, it is empty |
| riskDescription | string | Risk reason | Yes | When riskLevel is `PASS`, it is `normal` |
| riskDetail | json_object | Risk details | Yes | [See riskDetail parameters](#riskDetail) |
| auxInfo | json_object | Other auxiliary information | Yes | [See auxInfo parameters](#auxInfo) |
| finalResult       | int  | Whether it is the final result | Yes |Value is 1, your company can directly use the returned result for disposal, distribution, and other downstream scenarios<br/>Value is 0, indicating that the result is a process result of Shumei's risk control, and it needs to be checked again by Shumei's human review before being returned to your company |
| resultType       | int  | Whether the current result is from machine review or human review |Yes|0: Machine review, 1: Human review |
| allLabels | json_array | Risk label details | Yes | Return all hit risk labels and detailed information |
| businessLabels | json_array | Business label details | No | When only recognition is needed, and there is no need to configure reject or review strategies, the result is returned here, [see businessLabels parameters](#businessLabels) |
| tokenProfileLabels | json_array | Auxiliary information | No | Attribute account labels. [See account label parameters](#tokenProfileLabels) |
| tokenRiskLabels | json_array | Auxiliary information | No | Risk account labels. [See account label parameters](#tokenProfileLabels) |
| tokenLabels    | json_object  | Account label information | No | See the details below, only returned when tokenId is passed and Shumei is contacted to open |
| disposal       | json_object  | Disposal and mapping results | No |Shumei can return according to your company's label system and identification; if the custom label system is not configured, this field will not be returned |

<span id="disposal">Among them, the structure of disposal is as follows:</span>

| **Return Result Parameter Name** | **Type** | **Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| riskLevel | string | Disposal suggestion | Yes |If your company has its own disposal rules, Shumei can configure and return the corresponding disposal suggestions according to your company's disposal logic; if the rule label is not mapped, the default disposal suggestion will be returned|
| riskLabel1 | string | Mapped primary risk label | Yes | Primary risk label, when Shumei's label is not mapped to a custom label, and the riskLevel under disposal is PASS, the value of riskLabel1 is normal |
| riskLabel2 | string | Mapped secondary risk label | Yes |Secondary risk label, when Shumei's label is not mapped to a custom label, and the riskLevel under disposal is PASS, the value of riskLabel2 is empty |
| riskLabel3 | string | Mapped tertiary risk label | Yes |Tertiary risk label, when Shumei's label is not mapped to a custom label, and the riskLevel under disposal is PASS, the value of riskLabel3 is empty |
| riskDescription | string | Mapped risk reason | Yes |When riskLevel is PASS, it is "normal" |
| riskDetail | json_object | Mapped risk details | Yes | [See riskDetail parameters](#riskDetail) |

<span id="riskDetail">Among them, the structure of riskDetail is as follows:</span>

| **Return Result Parameter Name** | **Type** | **Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| faces | json_array | Return the names and location information of political figures in the image | No |  |
| face_num | int | Number of faces | No |  |
| persons | json_array | Only when hitting the portrait-multiple people, the array elements will have multiple, up to 10 |No  | |
| person_num | int | Number of portraits | No | Only returned under the portrait-multiple people |
| objects | json_array | Return the location information of items or QR codes in the image | No | The array will only have one element |
| ocrText | json_object | Return information about the violation text in the image, exists when the type field of the request parameter contains `IMGTEXTRISK` and ADVERT | No | |
| riskSource | int | Identify where the resource is violating | Yes | Identify the source of the risk result<br/>`1000`: No risk<br/>`1001`: Text risk<br/>`1002`: Visual image risk |

In riskDetail, the content of each element in the faces array is as follows:

| **Return Result Parameter Name** | **Type** | **Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| id | string | Person number |  | The number of the same person in different labels at the same location in the image is the same.<br/>If the same person appears n times in the image, n IDs are assigned |
| name | string | Person name | No | The name of the public figure that can be recognized |
| location | int_array | Person location information, this array has four values, representing the coordinates of the upper left corner and the lower right corner respectively. For example, [207,522,340,567]<br/>207 represents the x-coordinate of the upper left corner<br/>522 represents the y-coordinate of the upper left corner<br/>340 represents the x-coordinate of the lower right corner<br/>567 represents the y-coordinate of the lower right corner | No |  |
| face_ratio | float | Face ratio | No | |
| probability | float | Confidence, optional values are between 0 and 1, the larger the value, the higher the confidence | No | A floating point number between 0 and 1 |

In riskDetail, the content of each element in the objects array is as follows:

| **Return Result Parameter Name** | **Type** | **Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| id | string | Number, ensuring that the number of items at the same location in different labels is the same | No |  |
| name | string | Identifier name | No | |
| location | int_array | Identifier location information, this array has four values, representing the coordinates of the upper left corner and the lower right corner respectively. For example, [207,522,340,567]<br/>207 represents the x-coordinate of the upper left corner<br/>522 represents the y-coordinate of the upper left corner<br/>340 represents the x-coordinate of the lower right corner<br/>567 represents the y-coordinate of the lower right corner  | No | |
| probability | float | Confidence, optional values are between 0 and 1, the larger the value, the higher the confidence | No | A floating point number between 0 and 1 |
| qrContent | string | URL information of the QR code | No | Only returned when hitting the QR code related label |

In riskDetail, the content of each element in the persons array is as follows:

| **Return Result Parameter Name** | **Type** | **Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| id | string | Number, ensuring that the number of the same person in different labels is the same. If the same person appears n times in the image, n IDs are assigned | No |  |
| person_ratio | string | The ratio of the portrait in the image | No | |
| location | int_array | Portrait location coordinates | No | |
| probability | float | Confidence, optional values are