# Shumei Skynet - Game Task Gold Farming

---

All rights reserved. Unauthorized reproduction is prohibited.

---

[Game Task Gold Farming](#riskDiscernInterface)

+ [Event Interface](#requestInterface)
    + [Interface Definition](#requestInterface)
        - [Request URL](#requestUrl)
        - [Character Encoding Format](#requestEncode)
        - [Request Method](#requestMethod)
        - [Suggested Timeout Duration](#requestTimeout)
        - [Request Parameters](#requestParameters)
        - [Response](#response)
    + [Event List](#eventIdList)
    + [Event Detailed API](#data)
    + [Examples](#requestExample)
        - [Request Example](#requestExp)
        - [Response Example](#requestResponseExp)
        - [Encrypted Transport](#encryptedTransport)
+ [Appendix](#appendix)

# <span id = "riskDiscernInterface">Game Resource Trading</span>

## <span id = "requestInterface">Specific Interface</span>

### <span id = "requestUrl">Request URL:</span>

| Cluster             | URL                                                 | Supported Product List |
| ------------------ | -------------------------------------------------- | -------------- |
| Beijing             | `http://api-skynet-bj.fengkongcloud.com/v4/event`   | Skynet Risk Identification |
| USA (Virginia) | `http://api-skynet-fjny.fengkongcloud.com/v4/event` | Skynet Risk Identification |
| Singapore           | `http://api-skynet-xjp.fengkongcloud.com/v4/event`  | Skynet Risk Identification |
| Europe (Frankfurt) | `http://api-skynet-eur.fengkongcloud.com/v4/event`  | Skynet Risk Identification |

### <span id = "requestMethod">Request Method:</span>

`POST`

### <span id = "requestEncode">Character Encoding:</span>

`UTF-8`

### <span id = "requestTimeout">Suggested Timeout Duration:</span>

1s

### <span id = "requestParameters">Request Parameters:</span>

Request body = General Request Parameters + Event Specific Parameters (placed under the data field)

#### <span id = "general">General Request Parameters:</span>

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name** | **Type**    | **Description**                                                                                         | **Required** | **Specification**                                                                                     |
| ---------------- | ------------- | ------------------------------------------------------------------------------------------------------ | -------------- | ---------------------------------------------------------------------------------------------- |
| accessKey      | string      | Interface authentication key<br/>Used for permission authentication, provided by Shumei when opening account services or viewed in the relevant documentation section in the upper right corner of the Shumei backend when logging in with the opening email | Required     | Assigned by Shumei                                                                                     |
| appId          | string      | Application ID, used to distinguish different applications of the same company                                                                   | Required     | The value of this parameter can be negotiated with Shumei                                                                     |
| eventId        | string      | Event ID, used to identify the event type.                                                                           | Required     | Different events correspond to different strategies, and the input parameters for different events may have slight differences. Please refer to the detailed parameter description for each event |
| data           | json_object | Request data content, event-specific parameters can be passed under data                                            | Required     | Request data content, up to 10MB, [see data parameters](#data)                                              |

<span id = "data">The content of data is as follows:</span>

| **Parameter Name** | **Type**    | **Description**                                                                                         | **Required** | **Specification**                                                                                     |
| ----------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| tokenId         | string   | User account identifier, it is recommended to use your company's user UID (can be encrypted) to generate it yourself, identifying the user's unique identity for risk control of behaviors such as spamming and advertising. If there is no user UID scenario, it is recommended to use a unique data identifier | Required | This ID is the user's unique identifier and should be consistent with the tokenId of other Shumei interfaces<br/>**Note**: When the same company has multiple applications, there are two situations:<br/>`1) `If the same user ID across applications is used by the same person in business, pass the original user ID.<br/>`2)` If the same user ID across applications is used by different people in business, pass the concatenated value of the application ID and user ID to ensure that the passed tokenId value can uniquely identify the user, such as appId_tokenId.                                                                                                                                                                             |
| ip              | string   | The public IPv4 address of the client when the current business event occurs                                                                                                          | Required     | Non-internal IP                                                                                                                                                                                                                            |
| timestamp       | int64    | The timestamp when the current business event occurs, in milliseconds (ms)                                                                                                    | Required     |                                                                                                                                                                                                                                     |
| deviceId        | string   | Shumei device fingerprint identifier, generated by Shumei SDK                                                                                                                 | Strongly recommended     | Shumei device fingerprint identifier, used for user behavior analysis.<br/>Note:<br/>1. For versions before integrating Shumei SDK, if deviceId cannot be obtained, pass an empty string "";<br/>2. Directly pass the original field obtained by getDeviceId without decryption <br/>(requires embedding SDKs for android, ios, harmony, web, web SDK for official website pages, H5 pages)                                                          <br/> |
| activityType | string | Activity type, mainly divided into offline activities and online activities. Different strategies can be called according to different activity types | Recommended | Optional values: online_activity, offline_activity. online_activity refers to online activities, such as online check-ins; offline_activity refers to offline activities, such as ground promotion activities, etc.; |
| os              | string   | Application end operating system type                                                                                                                              | Strongly recommended     | Optional values: `android、harmony、ios、weapp、web、aliapp：Alipay Mini-Program Engine、ttapp：Douyin Mini-Program、tmapp：Tmall Mini-Program`;<br/>If the registration event comes from the official website registration, the os of the registration event is passed as "web"                                                                                                                                                                                                 |
| appVersion      | string   | Application version number                                                                                                                                      | Strongly recommended     | Pass the current version number of the application or service being used, in the format of 3 dots and 4 segments of numbers, with each segment of numbers up to 4 digits, such as `3.2.1.1234`. If less than 4 segments, fill with 0, such as `2.1.5` pass `2.1.5.0`. If more than 4 segments, take the first 4 segments, such as `2.1.5.1.1` pass `2.1.5.1`.                                                         |
| countryCode     | string   | Country code of the mobile user                                                                                                                              | Strongly recommended     | Mainland China mobile phone numbers fill in `0086`, non-mainland China mobile phone numbers fill in the country code, for example: USA fill in `0001`, Andorra `0376`, Bahamas `1242`                                                                                                                            |
| phoneMd5        | string   | MD5 of the phone number, 32-bit lowercase encrypted string                                                                                                                       | Recommended         |                                                                                                                                                                                                                                     |
| newCountryCode | string      | Country Code                                                                       | Recommended   | A four digit string. If the parameter was not passed in, the default value is China country code 0086                                                                                                                                                                                                       |
| level        | int     | User combat power level, not passed for registration events, needs to be passed for login, task completion, and virtual order events    | Strongly recommended | Can be discussed and defined<br/>Optional values: 0,1,2,3,4 corresponding to:<br/>0: lowest level user,<br/>1: lower level user,<br/>2: medium level user, typically users with certain activity or medium level, etc.<br/>3: higher level user, typically users with high activity or high level, etc.<br/>4: highest level user, typically high-paying users, VIP users, users who can be considered for exemption, etc.|
| passThrough     | json     | User-defined pass-through fields                                                                                                                              | Recommended         | If not necessary, do not pass                                                                                                                                                                                                                      |

### <span id = "response">Response</span>

Placed in the HTTP Body, in JSON format, with specific parameters as follows:


| **Parameter Name** | **Type**    | **Description**                                                                                         | **Required** | **Specification**                                                                                     |
| -------------------- | -------------- | -------------------------- | -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| code               | int          | Return code                   | Yes           | `1100`: Success<br/>`1901`: QPS limit exceeded<br/>`1902`: Invalid parameter<br/>`1903`: Service failure<br/>`9101`: No permission to operate<br/>Except for message and requestId, other fields only exist when the code is 1100 |
| message            | string       | Return code description               | Yes           | Corresponds to code: `Success QPS limit exceeded Invalid parameter Service failure Insufficient balance No permission to operate`                                                                                                                                                 |
| requestId          | string       | Request identifier                 | Yes           | Unique request identifier, used for troubleshooting and subsequent effect optimization, strongly recommended to save                                                                                                                                                             |
| riskLevel           | string       | Current event handling suggestion       | Yes           | `PASS`: Pass<br/>`REVIEW`: Review<br/>`REJECT`: Reject<br/> `VERIFY`: Verify                                                                                                                                            |
| detail             | json_object  | Risk detail information, see below for details | Yes           | See "detail result details" table                                                                                                                                                                                           |
| tokenProfileLabels | json_array   | Account attribute labels             | No           | See below for details, only returned when the service is enabled                                                                                                                                                                                 |
| tokenRiskLabels    | json_array   | Account risk labels             | No           | See below for details, only returned when the service is enabled                                                                                                                                                                                 |

Among them

1) Details of the detail result


| **Return Result Parameter Name** | **Parameter Type** | **Description**         | **Specification**                   |
| -------------------- | -------------- | ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| description        | string       | Risk description of the current event                       |                                                                                                                                                                                                           |
| model              | string       | Rule identifier, the highest priority rule identifier hit       |                                                                                                                                                                                                           |
| hits               | Json array   | All rule identifiers hit by the event                   | Each field in hits is an object, see "hits result details" for details                                                                                                                                                    |
| ip_country         | string       | IP country                             |                                                                                                                                                                                                           |
| ip_province        | string       | IP province                             |                                                                                                                                                                                                           |
| ip_city            | string       | IP city                             |                                                                                                                                                                                                           |
| verifyType         | string       | When the handling suggestion is `VERIFY`, return `verifyType` | `UPSMS`: Upstream SMS verification code<br/>`DOWNSMS`: Downstream SMS verification code<br/>`CAPTCHA`: Verification code (click or slide, etc.)<br/>`SEQUENCE`: Sequence verification (click or input)<br/>`SPATIAL`: Spatial logic reasoning<br/>`FACE`: Face detection<br/>`DELAY`: Delayed transaction |
| machineAccountRisk            | json       | Account historical blacklisting information                             | Only when the current account has been blacklisted, the relevant transaction of the account will return this structure       |  

Among them, the details of the hits result are as follows:


| **Return Result Parameter Name** | **Parameter Type** | **Description**         | **Specification**                   |
| -------------------- | -------------- | ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| description        | string       | Risk description of the current event                       |                                                                                                                                                                                                           |
| model              | string       | Rule identifier                                 |                                                                                                                                                                                                           |
| riskLevel          | string       | Handling suggestion for the current event                       | `PASS`: Pass<br/>`REVIEW`: Review<br/>`REJECT`: Reject<br/>`VERIFY`: Verify                                                                                                                                      |
| verifyType         | string       | When the handling suggestion is `VERIFY`, return `verifyType` | `UPSMS`: Upstream SMS verification code<br/>`DOWNSMS`: Downstream SMS verification code<br/>`CAPTCHA`: Verification code (click or slide, etc.)<br/>`SEQUENCE`: Sequence verification (click or input)<br/>`SPATIAL`: Spatial logic reasoning<br/>`FACE`: Face detection<br/>`DELAY`: Delayed transaction |

Among them, the details of machineAccountRisk are as follows:


| **Return Result Parameter Name** | **Parameter Type** | **Description**         | **Specification**                   |
| -------------------- | -------------- | ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| tokenSampleLastTs        | int64       | Last blacklisting time, in milliseconds                       |  For example: "tokenSampleLastTs": 1683026613000                                                                                                                                                                                                         |
| tokenSampleDesc              | string       | Description of historical blacklisting strategy                                 |  For example: "tokenSampleDesc": "High-risk device: IP abnormal aggregation"                                                                                                                                                                                                         |

Among them, the details of the label return fields

1) Details of tokenProfileLabels


| **Return Result Parameter Name** | **Parameter Type** | **Description**         | **Specification**                   |
| ---------------------- | ---------------- | ------------------------ | ------------------------------ |
| label1               | string         | Primary label               | Display the primary label of the account attribute label. |
| label2               | string         | Secondary label               | Display the secondary label of the account attribute label. |
| label3               | string         | Tertiary label               | Display the tertiary label of the account attribute label. |
| description          | string         | Risk description               | Display the Chinese description of the account attribute label. |
| timestamp            | int64          | Time of the last hit strategy | Time of the last hit strategy       |
| detail               | json_object    | Evidence description               | Evidence details                     |

2) Details of tokenRiskLabels


| **Return Result Parameter Name** | **Parameter Type** | **Description**         | **Specification**                   |
| ---------------------- | ---------------- | ------------------------ | ------------------------------ |
| label1               | string         | Primary label               | Display the primary label of the account risk label. |
| label2               | string         | Secondary label               | Display the secondary label of the account risk label. |
| label3               | string         | Tertiary label               | Display the tertiary label of the account risk label. |
| description          | string         | Risk description               | Display the Chinese description of the account risk label. |
| timestamp            | int64          | Time of the last hit strategy | Time of the last hit strategy       |
| detail               | json_object    | Evidence description               | Evidence details                     |

Among them, the details of the group account label (risk_group_token) are as follows:


| **Return Result Parameter Name** | **Parameter Type** | **Description**         |                  
| ---------------------- | ---------------- | ------------------------ | 
| groups               | json_array         | Multiple group information arrays, elements are group                |

**Group content is as follows:**

| **Parameter Name** | **Parameter Type** | **Description**         |                  
| ---------------------- | ---------------- | ------------------------ | 
| memberIds               | array         | Group member accounts                |
| memberCount               | int         | Group size, total number of accounts in this group                |
| groupId               | string         | Unique identifier of the group                |
| reason               | string         | Reason for forming the group                |
| ts               | string         | Time of labeling the group                |


## <span id="eventIdList">Event List</span>

| **Industry** | **Scenario**  | **Event** | **eventId**  | **Specification**                | **Timing and Usage**                                                                                                                                  |
|--------|---------|--------|--------------|-----------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| General     | General      | Registration     | register     | [Parameter Details](#register)     | Timing: When the user registers an account on the official website or in the game <br/>Usage: PASS: Normal<br/>REJECT: Tag and confirm whether it is a material account during the transaction, if confirmed, ban the account                                                                     |
| General     | General      | Login     | login        | [Parameter Details](#login)        | Timing: When the user logs in with a password, one-click login, or default login <br/>Usage: PASS: Normal<br/>REJECT: Tag and confirm whether it is a material account during the transaction, if confirmed, ban the account                                                                 |
| Game     | Profit-Task   | Task Completion    | gameTask     | [Parameter Details](#task)         | Timing: When the user obtains rewards through levels, battles, task completion, etc. <br/>**(Customers can control the frequency of statistics, such as calling the task completion event for each account every 10 minutes, and the transmission content is: the total amount of props and materials obtained by account A within 10 minutes)**<br/>Usage: Tag and confirm whether it is a material account during the transaction, if confirmed, ban the account or handle it as appropriate  |
| Game     | Profit-Resell Resources | Place Virtual Order  | virtualOrder | [Parameter Details](#virtualOrder) | Timing: When the buyer clicks the purchase button to confirm payment in the exchange, where tokenId is the buyer ID; <br/>When the user purchases official mall items for recharge<br/>Usage: PASS: Pass<br/>REJECT: Tag: Material account: Ban the account or handle it as appropriate; <br/>Material account/game currency account: Handle it as appropriate based on the divine weapon value, historical recharge amount, and other business data |



## <span id="data"> Event Detailed API</span>
#### <span id="register">Registration Event </span>


| **Parameter Name** | **Type**    | **Description**                                                                                         | **Required** | **Specification**                                                                                     |
| --------------------------- | ------------ | ---------------------------------------------------------------------------------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| [Basic Parameters](#data)             |  |               |      |                   Needs to meet the configuration requirements of basic parameters                                                                                                                                                                                    |
| type                  | string | Registration method: including mobile phone number one-click registration, mobile phone number verification code registration, third-party authorization, username and password registration | Required | Optional values:<br/>`phoneOnePass`: Mobile phone number one-click registration, mobile phone number verification code registration<br/>`signupPlatform`: Third-party authorization<br/>`userPassword`: Username and password registration      |
| hashPassword          | string | Encrypted user password                                                             | Strongly recommended | You can choose any encryption method for encryption, but you must ensure that the same password is unique and consistent after encryption.                                                                  |
| isPhoneExist          | int    | Whether the mobile phone number has been registered                                                       | Strongly recommended | Optional values: `0`, `1` If the registration method is mobile phone number registration, and the mobile phone number has been registered, pass `1`, otherwise pass `0`.                                                     |
| signupPlatform        | string | Third-party registration platform, if registered directly on this platform, it can be omitted                                 | Recommended     | Optional values: `qq, weibo, weixin, alipay, taobao, facebook, twitter `                                                                             |
| email                 | string | Registration email                                                                 | Recommended     |                                                                                                                                             |
| sex                   | string | User gender, if not available, do not pass                                                         | Recommended     | Optional values