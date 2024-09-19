# Shumei Intelligent Text Recognition Product API Documentation

## Request Parameters

### Request URL:

| Cluster | URL | Supported Product List |
| --- | --- | --- |
| Beijing | `http://api-text-bj.fengkongcloud.com/v2/saas/anti_fraud/text` | Chinese Text |
| Shanghai | `http://api-text-sh.fengkongcloud.com/v2/saas/anti_fraud/text` | Chinese Text |
| USA (Virginia) | `http://api-text-fjny.fengkongcloud.com/v2/saas/anti_fraud/text` | Chinese Text |
| Singapore | `http://api-text-xjp.fengkongcloud.com/v2/saas/anti_fraud/text` | Chinese Text |
| India | `https://api-text-yd.fengkongcloud.com/v2/saas/anti_fraud/text` | Chinese Text |

### Character Encoding Format:

`UTF-8` character set encoding

### Request Method:

`POST`

### Suggested Timeout Duration:

1s

### Request Parameters:

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| accessKey | string | API authentication key | Y | Provided by Shumei |
| appId | string | Application identifier | Y | Used to distinguish applications, needs to contact Shumei to activate, please use the value provided separately by Shumei |
| type | string | Risk type or scenario to be detected | Y | Optional values:<br/>`ZHIBO`: Live broadcast<br/>`ECOM`: E-commerce<br/>`GAME`: Game<br/>`NEWS`: News<br/>`FORUM`: Forum<br/>`SOCIAL`: Social<br/>`QQ`: QQ<br/>`NOVEL`: Novel<br/>`TEXTRISK`: Default value (includes:<br/>political, terrorism, prohibited, pornographic, abusive, advertising, privacy, advertising law, meaningless)<br/>`FRUAD`: Online fraud<br/>`UNPOACH`: High-value user anti-poaching<br/> <br/>`TEXTMINOR`: Minors<br/>The above types can be combined with underscores, such as: `ZHIBO_TEXTRISK_FRUAD` |
| data | json_object | Request data content | Y | Maximum 1MB, [see data parameters](#data) |

 <span id="data">Among them, the content of data is as follows:</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| text | string | Text to be detected | Y | Text limit of 10,000 characters, only the first 10,000 characters will be recognized if exceeded<br/>If the nickname field is passed, the text + nickname content will be checked simultaneously. |
| tokenId | string | User account identifier, it is recommended to use your company's user UID (can be encrypted) to generate it yourself, to identify the user's unique identity for risk control of behaviors such as spamming and advertising.<br/>If there is no user UID scenario, it is recommended to use a unique data identifier to pass the value | Y | A string of up to 64 characters composed of numbers, letters, underscores, and hyphens |
| gender | int | User gender | N | Optional values<br/>`0`: Female<br/>`1`: Male<br/> |
| channel | string | Business scenario | N | Channel table configuration |
| nickname | string | User nickname, it is strongly recommended to pass this parameter, almost all malicious users on the platform will spread spam through nicknames, with risks such as political violations, prohibited content, and diversion information, length limit of 150 characters, the excess part will be truncated | N |  |
| ip | string | IP address, this parameter is used for user behavior analysis at the IP dimension, and can also be used to compare with Shumei's IP blacklist | N | The public IPv4 or IPv6 address of the user sending the text |
| deviceId | string | Shumei device identifier, it is strongly recommended to pass this parameter, Shumei device fingerprint identifier, used for user behavior analysis. When malicious users tamper with MAC, IMEI, and other device information, using deviceId can discover and identify such malicious behaviors, and can also be used to compare with Shumei's device fingerprint blacklist | N |  |
| receiveTokenId | string | Receiver's tokenId, required in private chat scenarios | N |  |
| level | int | User level, different interception strategies can be configured for users of different levels | N | Optional values:<br/>`0`: Lowest level user, typically new registrations, completely inactive or level 0 users, etc.;<br/>`1`: Lower level user, typically low active or low-level users, etc.;<br/>`2`: Medium level user, typically users with certain activity or medium level, etc.;<br/>`3`: Higher level user, typically high active or high-level users, etc.;<br/>`4`: Highest level user, typically paid users, VIP users, etc. |
| registerTime | int | Account registration time, it is strongly recommended to pass this parameter, the risk of abnormal operations for newly registered accounts is higher | N |  |
| friendNum | int | Number of account friends, it is strongly recommended to pass this parameter in social scenarios to indicate user quality | N |  |
| fansNum | int | Number of account fans, it is strongly recommended to pass this parameter in live broadcast/community scenarios to indicate user quality | N |  |
| isPremiumUser | int | Whether it is a premium (e.g., paid) user, configure different levels to indicate user quality | N | Optional values<br/>`0`: Default value<br/>`1`: Premium account<br/> |
| isTokenSeparate | int | Whether to distinguish accounts under different applications | N | Optional values<br/>`0`: Do not distinguish<br/>`1`: Distinguish<br/>The default value is 0.<br/>When the value is 1, the account systems under different applications are independent, and the account-related strategy features are separately counted and effective under different applications. |
| room | string | Live room/game room number, different strategies can be formulated for a single room | N |  |
| topic | string | Discussion topic number, can be the book review area number, forum post number. | N |  |
| nickocr | string | Text content recognized by avatar OCR | N |  |
| imei | string | Unique identifier of the user's Android device, compared to tokenId and IP, IMEI and MAC are more difficult to change. When malicious users use multiple different accounts and IPs to commit malicious acts, IMEI and MAC can effectively associate and identify such malicious behaviors, and can also be used to compare with Shumei's device blacklist. | N |  |
| mac | string | Unique identifier of the user's Android device, compared to tokenId and IP, IMEI and MAC are more difficult to change. When malicious users use multiple different accounts and IPs to commit malicious acts, IMEI and MAC can effectively associate and identify such malicious behaviors, and can also be used to compare with Shumei's device blacklist. | N |  |
| idfv | string | Unique identifier of the user's iOS application, compared to tokenId and IP, IDFV cannot be modified. When malicious users use multiple different accounts and IPs to commit malicious acts, using IDFV can discover and identify such malicious behaviors. | N |  |
| idfa | string | Unique identifier of the user's iOS application, compared to tokenId and IP, IDFV cannot be modified. When malicious users use multiple different accounts and IPs to commit malicious acts, using IDFV can discover and identify such malicious behaviors. | N |  |
| passThrough | json_object | This field content is returned together with the result | N |  |
| dataId | string | Data identifier | N |  |

## Return Results

### Return Results

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| code | int| Return code | Y | `1100`: Success<br/>`1901`: QPS limit exceeded<br/>`1902`: Invalid parameter<br/>`1903`: Service failure<br/>`1905`: Word limit exceeded<br/>`9101`: Unauthorized operation |
| message | string | Return code description | Y | Corresponds to code:<br/>Success<br/>QPS limit exceeded<br/>Invalid parameter<br/>Service failure<br/>Word limit exceeded<br/>Unauthorized operation |
| requestId | string | Request identifier | Y | The unique identifier of the request data, used for troubleshooting and performance optimization, it is strongly recommended to save |
| score | int | Risk score | N | Range [0,1000], the higher the score, the greater the risk |
| riskLevel | string | Disposal suggestion | N | Possible return values:<br/>`PASS`: Normal, recommended to pass directly<br/>`REVIEW`: Suspicious, recommended for manual review<br/>`REJECT`: Violation, recommended to intercept directly |
| status | int | Indicates whether the service timed out | Y | Possible return values:<br/>`0`: Normal<br/>`501`: Timeout |
| detail | string | Risk details | N | [See detail parameters](#detail) |
| businessLabels | json_array | Auxiliary information | Y | All business labels hit and detailed information. [See businessLabels parameters](#businessLabels) |
| tokenProfileLabels | json_array | Auxiliary information | N | Attribute account labels. [See account labels parameters](#tokenProfileLabels) |
| tokenRiskLabels | json_array | Auxiliary information | N | Risk account labels. [See account labels parameters](#tokenProfileLabels) |
| unauthorizedType | string | Auxiliary information | N | Unauthorized type |

<span id="detail">Among them, the detail field is as follows:</span>

| Parameter Name          | **Type**    | **Required** | **Description**                                                     |
| ----------------- | ----------- | ------------ | ------------------------------------------------------------ |
| riskType          | int         | Y            | Risk type:<br/>`0`: Normal<br/>`100`: Political<br/>`200`: Pornographic<br/>`210`: Abusive<br/>`300`: Advertising<br/>`340`: Online fraud<br/>`400`: Spamming<br/>`500`: Meaningless<br/>`600`: Prohibited<br/>`700`: Other<br/>`720`: Black account<br/>`730`: Black IP<br/>`800`: High-risk account<br/>`900`: Custom |
| model             | string      | Y            | Rule identifier, used to identify the strategy rule hit by the text.<br/>Note: This parameter is a return parameter of the old version of the API, retained for compatibility, will be removed in subsequent versions, please do not rely on this parameter, for reference only |
| description       | string      | Y            | Description of the strategy rule risk reason<br/>Note: This parameter is a return parameter of the old version of the API, retained for compatibility, will be removed in subsequent versions, please do not rely on this parameter, for reference only |
| descriptionV2     | string      | N            | Description of the new strategy rule risk reason<br/>Note: This parameter is a return parameter of the new version of the API, only new strategies will return during the transition period |
| isBlackToken      | string      | N            | This account is marked as a high-risk account by the portrait strategy, possible value:<br/>`1`: High-risk account  |
| hitPosition       | string      | N            | The position of the hit sensitive word in the text, starting from 0                      |
| filteredText      | string      | N            | The text after the risk segment is replaced with *  |
| matchedList       | string      | N            | The name of the list where the hit sensitive word is located (this parameter only exists when the sensitive word is hit)       |
| matchedItem       | string      | N            | The specific sensitive word hit (this parameter only exists when the sensitive word is hit)               |
| contactResult     | json_array  | N            | Contact recognition results, including the string type and content of the recognized WeChat, QQ, and phone numbers. See the table below for details. [See contactResult](#contactResult) |
| matchedDetail     | string      | N            | Detailed information of the hit sensitive word (requires communication with Shumei to enable the corresponding strategy), can be deserialized into json_array. [See matchedDetail](#matchedDetail) |
| passThrough       | json_object | N            | This field is the pass-through field passed by the customer                                     |
| sexy_risk_tokenid | float       | N            | Pornographic account score, range [0,1]                                        |
| contextProcessed  | bool        | Y            | true indicates that the request contacted the context;<br/>false indicates that the request did not contact the context, if this function is needed, it can be negotiated with Shumei |
| contextText       | string      | Y            | If the contact context service is not enabled, only the current text is returned                         |

<span id="contactResult">Each item in contactResult:</span>

| **Parameter Name**| **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| contactType | int| Auxiliary information | N| Contact type, optional values range [0-3], details are as follows:<br/>`0`: Phone number <br/>`1`: QQ number <br/>`2`: WeChat number  |
| contactString | string | Auxiliary information | N| Contact string |

<span id="matchedDetail">Among them, the content of matchedDetail:</span>

| **Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| listId         | string       |              | Y            | Return code                                                       |
| matchedFiled | string_array |              | N            | Indicates that the nickname or text content hit the sensitive word (this parameter only exists when the sensitive word is hit), optional values:<br/>text: Text hit the sensitive word<br/>nickname: Nickname hit the sensitive word |
| name           | string       |              | Y            | The name of the list where the hit sensitive word is located                                     |
| organization   | string       |              | N            | The company identifier to which the list belongs, where "GLOBAL" is the global list               |
| words          | string_array |              | N            | All sensitive words in the corresponding list that were hit                                 |
| wordPositions | json_array   |              | N            | All sensitive words and positions in the corresponding list that were hit. [See wordPositions](#words) |

<span id="words">Each item in wordPositions:</span>

| **Parameter Name** | **Type**| **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| word | string| Auxiliary information | N| Hit sensitive word |
| position | string | Auxiliary information | N| Position of the sensitive word |

<span id="businessLabels">Among them, the content of businessLabels is as follows:</span>

| Parameter Name            | Type        | Parameter Description                                                     | Required | Specification         |
| ------------------- | ----------- | ------------------------------------------------------------ | -------- | ------------ |
| businessLabel1      | string      | businessLabels is not empty, must return                                     | Y        | First-level business label |
| businessLabel2      | string      | businessLabels is not empty, must return                                     | Y        | Second-level business label |
| businessLabel3      | string      | businessLabels is not empty, must return                                     | Y        | Third-level business label |
| businessDescription | string      | businessLabels is not empty, must return                                     | Y        | Label description     |
| probability         | float       | businessLabels is not empty, must return<br/>Optional values between 0 and 1, the larger the value, the higher the confidence | Y        | Confidence       |
| businessDetail      | Json_object | businessLabels is not empty, must return                                     | Y        | Business details     |

<span id="tokenProfileLabels">Among them, the content of tokenProfileLabels, tokenRiskLabels is as follows:</span>

| Parameter Name    | Type   | Parameter Description     | Required | Specification                       |
| ----------- | ------ | ------------ | -------- | -------------------------- |
| label1      | string | First-level label     | No       |                            |
| label2      | string | Second-level label     | No       |                            |
| label3      | string | Third-level label     | No       |                            |
| description | string | Label description     | No       |                            |
| timestamp   | Int    | Labeling timestamp | No       | 13-digit Unix timestamp, unit: milliseconds |

## Example

### Request Example

```json
{
    "accessKey":"*************",
    "appId":"default",
	"type":"ZHIBO",
	"businessType":"MINOR",
    "data":
    {
        "text":"Add a friend qq12345",
        "tokenId":"4567898765jhgfdsa",
        "channel":"text"
    }
}
```

### Return Example

```json
{
    "businessLabels":[

    ],
    "code":1100,
    "detail":"{\"contactResult\":[{\"contactString\":\"qq12345\",\"contactType\":2}],\"contextProcessed\":false,\"contextText\":\"Add a friend qq12345\",\"description\":\"Advertising: Contact: Contact\",\"descriptionV2\":\"Advertising: Contact: Contact\",\"filteredText\":\"Add a friend qq12345\",\"matchedDetail\":\"[{\\\"listId\\\":\\\"e9d10f80db083704aa139c69411dd9a8\\\",\\\"matchedFiled\\\":[\\\"text\\\"],\\\"name\\\":\\\"Test zyk\\\",\\\"organization\\\":\\\"RlokQwRlVjUrTUlkIqOg\\\",\\\"wordPositions\\\":[{\\\"position\\\":\\\"8,9,10,11,12\\\",\\\"word\\\":\\\"12345\\\"},{\\\"position\\\":\\\"8,9,10\\\",\\\"word\\\":\\\"123\\\"},{\\\"position\\\":\\\"8,9,10,11\\\",\\\"word\\\":\\\"1234\\\"},{\\\"position\\\":\\\"10,11,12\\\",\\\"word\\\":\\\"345\\\"},{\\\"position\\\":\\\"9,10\\\",\\\"word\\\":\\\"23\\\"},{\\\"position\\\":\\\"8,9\\\",\\\"word\\\":\\\"12\\\"},{\\\"position\\\":\\\"9,10,11,12\\\",\\\"word\\\":\\\"2345\\\"},{