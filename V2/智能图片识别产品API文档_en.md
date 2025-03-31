# Shumei Intelligent Image Recognition Product API Documentation

- - - - -

***All rights reserved. Unauthorized reproduction is prohibited.***

- - - - -
# Preparation Before Integration

### Shumei Service Account Application
The account manager has already established contact with your company or visited in person. You can directly provide the account opening and service-related information to the account manager.
The information required to open an account includes:

| Parameter |  |
| --- | --- |
| Full Company Name | xxxx |
| Company Abbreviation | xxxx |
| Contact Person's Email | xxxx |
| Contact Person's Phone | xxxx |

### Channel Configuration Table
Shumei configures different channels (channel) based on different business scenarios of customers, formulates targeted interception strategies, and also facilitates customers to filter and analyze data for different business scenarios. The corresponding table of business scenarios and channel values is as follows (supports customer customization):

| Business Scenario | Channel Value | Remarks |
| --- | --- | --- |
| Avatar | HEAD_IMG | User avatar |
| Album | IMGS | User album |
| Dynamic | DYNAMIC | Dynamic images on social platforms |
| Article Image | ARTICLE | Images in blogs or articles |
| Comment Image | COMMENT | Images in comments |
| Cover | COVER | Cover or background images in albums |
| Product Image | PRODUCT | Product images on e-commerce platforms |
| Group Chat Image | GROUP_CHAT | Image messages in group chats |
| Private Chat Image | MESSAGE | Image messages in private chats |
| Offline Test | OFFLINE_TEST | Dedicated for offline testing with portrait and behavior-related strategies turned off |

### Shumei Service Account Information Reception
The Shumei account manager will open the corresponding Shumei account and service for you within 1 working day. Subsequently, the contact person's email will receive the following information:

| Name | Specific Value | Description |
| --- | --- | --- |
| accessKey | xxxx | Authentication code for Shumei API service, required when calling Shumei API |
| organization | xxxx | Unique identifier assigned by Shumei for the enterprise, required when calling SDK |
| Shumei Management Console Account | xxxx | Used to log in to the Shumei management console |
| Shumei Management Console Password | xxxx | Used to log in to the Shumei management console |
| Shumei Management Console Address | https://console.ishumei.com | Used to log in to the Shumei management console |

# Intelligent Image Filtering Service Single Image API Integration Instructions

## Synchronous Single Request

### Request URL:
| Cluster | URL | Supported Product List |
| --- | --- | --- |
| Beijing | `http://api-img-bj.fengkongcloud.com/v2/saas/anti_fraud/img` | Image |
| Shanghai | `http://api-img-sh.fengkongcloud.com/v2/saas/anti_fraud/img` | Image |
| Singapore | `http://api-img-xjp.fengkongcloud.com/v2/saas/anti_fraud/img` | Image |
| India | `http://api-img-yd.fengkongcloud.com/v2/saas/anti_fraud/img` | Image |
| Silicon Valley | `http://api-img-gg.fengkongcloud.com//v2/saas/anti_fraud/img` | Image |

### Request Method:

`POST`

### Character Encoding:

`UTF-8`

### Suggested Timeout: 10s
Currently, the connection timeout for downloading images on the Shumei side is 2s, the read timeout is 3s, and the internal average processing time is around 500ms (the specific duration is related to the request type and image size). If the download fails, it will retry once. It is recommended that customers set the timeout to 7-10s.

### Request Parameters:

Placed in the HTTP Body in JSON format, the specific parameters are as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| accessKey | string | Interface authentication key<br/>Used for permission authentication, provided by Shumei when opening the account service or viewed in the relevant document section in the upper right corner of the Shumei backend after logging in with the opening email | Required | accessKey |
| type | string | Risk type to be detected | Required | Regulatory primary label<br/>Optional values:<br/>`POLITICS`: Political recognition<br/>`OCR`: OCR text recognition in images<br/>`AD`: Advertisement recognition<br/>`BEHAVIOR`: Bad scene recognition, supports smoking, drinking, gambling, drug use, condoms, and meaningless images<br/>`PERSON`: Political face recognition<br/>`VIOLENCE`: Terrorism recognition<br/>`PORN`: Pornography recognition<br/><br/>Multiple types are connected by underscores, such as `AD_PORN_POLITICS` for combined recognition of advertisements, pornography, and politics<br/>Recommended to pass: `POLITICS_PORN_AD_BEHAVIOR`<br/><br/>Note: Here `POLITICS` is actually equivalent to the following two types:<br/>`PERSON`: Political face recognition <br/>`VIOLENCE`: Terrorism recognition <br/> (One of this field and the `businessType` field must be passed) |
| businessType | string | Business label type | Optional | Business label<br/>Optional values: [See Appendix](#Appendix) If multiple recognition functions are needed, connect them with underscores. One of this field and the type must be passed |
| appId | string | Application identifier, used to distinguish different application data of the same company | Required | Need to contact Shumei to open, please use the value provided separately by Shumei |
| callback | string | Callback address, when this parameter is passed, the asynchronous callback logic is followed | Optional | Callback HTTP interface, when this field is not empty, the service will notify the user of the review result based on this field<br/>The address must be a standard URL of http or https |
| data | json_object | Request data content | Required | Request data content, the maximum length of data is 10MB, [see data parameters](#data) |
| callbackParam | json_object | Pass-through parameters | Optional |  |

<span id="data">Among them, the content of data is as follows:</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| tokenId | string | User account identifier, it is recommended to use your company's user UID (can be encrypted) to generate it yourself, identifying the user's unique identity for risk control of behaviors such as flooding and advertising. If there is no user UID scenario, it is recommended to use a unique data identifier to pass the value | Required | A string of numbers, letters, and hyphens with a length of less than or equal to 64 characters |
| img | string | The image to be detected, can use base64 encoded image data or image URL link. It is recommended to download the image from the CDN source station, and the source station should not be a single point<br/>Risk: If it is not downloaded from the source station, there may be image download failures, resulting in the inability to review | Required | Supported formats:<br/>`jpg`, `jpeg`, `png`, `webp`, `gif`, `tiff`, `tif`, `hief`<br/>It is recommended that the image pixel is not less than 256*256, currently supports images with resolutions between 20*20 and 6000*6000, the maximum image size is 10MB, and the maximum for asynchronous is 30M<br/>By default, long images are not split. If needed, please contact Shumei to open it. The billing is based on the actual number of frames captured. |
| btId | string | User-specified image identifier | Optional | When the callback exists, it will be returned to the user in the callback request, with a length limit of 30 characters |
| channel | string | Channel configuration | Optional | See the channel configuration table |
| registerTime | int | Account registration time | Optional | It is recommended to pass this parameter, as the risk of abnormal operations for newly registered accounts is higher, in milliseconds timestamp |
| friendNum | int | Number of account friends | Optional | Strongly recommended to pass this parameter in social scenarios, indicating user quality |
| fansNum | int | Number of account fans | Optional | Strongly recommended to pass this parameter in live/community scenarios, indicating user quality |
| isPremiumUser | int | Whether it is a premium (e.g., paid) user | Optional | Configure different levels to indicate user quality. 1 for premium users, 0 for default value |
| ip | string | Client IP | Optional | This parameter is used for user behavior analysis at the IP dimension and can also be used to compare with Shumei's IP blacklist |
| receiveTokenId | string | Receiver's tokenId | Optional | Receiver's tokenId, required in private chat scenarios |
| sex | int | User's gender | Optional | User's gender, optional values:<br/>0: Female<br/>1: Male |
| age | int | User's age | Optional | User's age, optional values:<br/>0: Youth (approximately 18-45 years old)<br/>1: Middle-aged (approximately 45-60 years old)<br/>2: Elderly (over 60 years old) |
| level | int | User level | Optional | User level, different interception strategies can be configured for users of different levels |
| role | string | User role | Optional | Different strategies can be configured for different roles.<br/>In the live broadcast field, "ADMIN" means room manager, "HOST" means host, "SYSTEM" means system role. In the game field, "ADMIN" means administrator, "USER" means ordinary user. Missing or "USER" defaults to ordinary user |
| topic | string | Discussion topic number | Optional | Can be the book review area number, forum post number |
| phone | string | User's phone number | Optional | User's phone number, can be used to compare with Shumei's phone number blacklist |
| deviceId | string | Shumei device fingerprint identifier | Optional | Strongly recommended to pass in, Shumei device fingerprint identifier, used for user behavior analysis. When malicious users tamper with mac, imei, and other device information, using deviceId can discover and identify such malicious behaviors, and can also be used to compare with Shumei's device fingerprint blacklist |
| imei | string | User's android device unique identifier | Optional | Compared with tokenId and IP, imei and mac are more difficult to replace. When malicious users use multiple different accounts and IPs to commit malicious acts, imei and mac can effectively associate and identify such malicious behaviors, and can also be used to compare with Shumei's device blacklist. |
| mac | string | User's android device unique identifier | Optional | Same as imei field |
| idfv | string | User's iOS application unique identifier | Optional | Compared with tokenId and IP, idfv cannot be modified. When malicious users use multiple different accounts and IPs to commit malicious acts, using idfv can discover and identify such malicious behaviors |
| idfa | string | User's iOS application unique identifier | Optional | Same as idfv field |
| maxFrame | int | Maximum number of frames captured for gif | Optional | Capture the number of frames for gifs and other dynamic images, the maximum is 20 frames, the default is 3 frames, and the billing is based on the actual number of frames captured. If the default is to capture 3 frames, it will be billed according to 3 frames. |
| interval | int | Gif frame capture frequency | Optional | Dedicated for GIF image detection, the default value is 1. Every interval images capture one for detection.<br/>When interval*maxFrame is less than the number of images contained in the image, the frame capture interval will be automatically modified to the number of images contained in the image/maxFrame to improve the overall detection effect. |
| isTokenSeparate | int | Whether to distinguish accounts under different applications | Optional | Whether to distinguish accounts under different applications, possible values:<br/>0: Do not distinguish<br/>1: Distinguish<br/>The default value is 0.<br/>When the value is 1, the account systems under different applications are independent, and the account-related strategy features are separately counted and take effect under different applications. |
| passThrough | json_object | Pass-through parameters | Optional | Customer pass-through fields, Shumei will not recognize and process this field internally, and it will be returned to the user with the result. It must be of json_object type |
| dataId | string | Customer-defined data Id | Optional | Can be used for Shumei SaaS backend retrieval |

## Synchronous Return Results

Placed in the HTTP Body in JSON format, the specific parameters are as follows:

| **Parameter Name** | **Parameter Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| code | int | Return code | Yes | [See the corresponding relationship between code and message](#code-message) |
| message | string | Return code description | Yes | Corresponds to code: Success/QPS limit exceeded/Invalid parameter/Service failure/No permission to operate |
| requestId | string | Request identifier | Yes | Unique identifier for the request, uniquely identifies this image review task |
| taskId | string | Task number | Yes |  |
| btId | string | User-uploaded image identifier | No | User-requested image identifier (exists when btId is passed in the request) |
| score | int | Risk score | No | Risk score (exists when callback is not present or empty and code is 1100) with a range of [0,1000], the higher the score, the greater the risk |
| riskLevel | string | Risk level | No | Risk level (exists when callback is not present or empty and code is 1100) possible return values: PASS, REVIEW, REJECT<br/>PASS: Normal content, recommended to pass directly<br/>REVIEW: Suspicious content, recommended for manual review<br/>REJECT: Violation content, recommended to intercept directly |
| status | int | Indicates whether the service timed out | Yes | Indicates whether the service timed out<br/>0: Normal<br/>501: Timeout |
| detail | json_object | Risk details | No | [See detail parameters](#detail) |
| businessLabels | json_array | Business labels | No | Business label results, only exist when the businessType parameter is not empty, [see businessLabels structure](#businessLabels) |
| tokenProfileLabels | json_array | Auxiliary information | No | Attribute account labels. [See account label parameters](#tokenProfileLabels) |
| tokenRiskLabels | json_array | Auxiliary information | No | Risk account labels. [See account label parameters](#tokenProfileLabels) |

<span id="code-message">Among them, the list of code and message is as follows:</span>

| **code** | **message** |
| --- | --- |
| 1100 | Success |
| 1901 | QPS limit exceeded |
| 1902 | Invalid parameter |
| 1903 | Service failure |
| 1905 | Unregulated content |
| 1911 | Download timeout |
| 9101 | No permission to operate |

<span id="detail">Among them, the structure of detail is as follows:</span>

| **Return Result Parameter Name** | **Parameter Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| riskType | int | Risk type | Yes | Indicates the risk type, [see riskType values](#vision-riskType) |
| riskSource | int | Risk result source | Yes | Indicates the source of the risk result:<br/>1000: No risk <br/>1001: Text risk <br/>1002: Visual image risk |
| model | string | Strategy rule identifier | Yes | Used to identify the hit strategy rule.<br/>Note: This parameter is an old API return parameter, retained for compatibility, and will be canceled in future versions. Please do not rely on this parameter, it is for reference only |
| description | string | Explanation of the intercepted risk reason | Yes | Only for understanding the risk reason as a reference, the program should not rely on the value of this parameter for logical processing |
| descriptionV2 | string | New strategy rule risk reason description | No | This parameter is a new API return parameter, and only new strategies will return it during the transition period |
| hits | string_array | Hit strategy set | No | Default is empty, needs to be opened in consultation with Shumei |
| text | string | OCR recognized text | No | OCR recognized text, the original_text and original_text_context fields return the same content as text, they are old fields and will be deprecated in the future, it is not recommended for customers to use them |
| matchedItem | string | Specific sensitive words hit | No | Specific sensitive words hit (this parameter only exists when sensitive words are hit), can be returned according to demand |
| matchedList | string | Name of the list where the sensitive words are hit | No | Name of the list where the sensitive words are hit (this parameter only exists when sensitive words are hit), can be returned according to demand |
| matchedDetail | string | Detailed information of the sensitive words hit | No | Detailed information of the sensitive words hit, can be deserialized into json_array, can be returned according to demand |
| qrcontent | string | QR code content information | No | QR code content information, can be returned according to demand, needs to be opened in consultation with Shumei |
| pornLabel | string | Pornography recognition label | No | Pornography recognition label, indicates the result of pornography recognition;<br/>After opening pornography recognition, you can choose whether to return this parameter according to demand;<br/>Optional values: "Pornography", "Sexy", "Normal" |
| pornRate | float | Probability of pornographic images | No | Probability of pornographic images, can be returned according to demand |
| sexyRate | float | Probability of sexy images | No | Probability of sexy images, can be returned according to demand |
| normalRate | float | Probability of normal images | No | Probability of normal images, can be returned according to demand |
| polityRate | float | Probability of the most similar political figure | No | Probability of the most similar political figure, can be returned according to demand |
| violenceLabel | string | Terrorism recognition label | No | Terrorism recognition label, indicates the result of terrorism recognition; after opening terrorism recognition, you can choose whether to return this parameter according to demand; optional values: "Riot scene", "National flag and emblem", "Military uniform", "Terrorist organization", "Guns and knives", "Bloody scene", "Game guns and knives", "China map", "Tank", "Candle", "Uniform", "Normal"  |
| rebelRate | float | Probability of riot scene | No | Probability of riot scene, can be returned according to demand |
| flagRate | float | Probability of national flag and emblem | No | Probability of national flag and emblem, can be returned according to demand |
