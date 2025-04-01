# DeepCleer Skynet - Channel Fake Traffic Identification

---

All rights reserved. Unauthorized reproduction is prohibited.

---

[Channel Fake Traffic Identification](#riskDiscernInterface)

+ [Event Interface](#requestInterface)
  + [Interface Definition](#requestInterface)
    - [Request URL](#requestUrl)
    - [Character Encoding Format](#requestEncode)
    - [Request Method](#requestMethod)
    - [Recommended Timeout Duration](#requestTimeout)
    - [Request Parameters](#requestParameters)
    - [Response](#response)
  + [Event List](#eventIdList)
  + [Event Detailed API](#data)
  + [Examples](#requestExample)
    - [Request Example](#requestExp)
    - [Response Example](#requestResponseExp)
+ [Query Interface](#query)
  + [Interface Definition](#query)
    - [Request URL](#queryUrl)
    - [Character Encoding Format](#queryEncode)
    - [Request Method](#queryMethod)
    - [Recommended Timeout Duration](#queryTimeout)
    - [Request Parameters](#queryParameters)
    - [Response](#queryResponse)
  + [Examples](#queryExample)
    - [Request Example](#queryExp)
    - [Response Example](#queryResponseExp)
    - [Encrypted Transport](#encryptedTransport)

# <span id = "riskDiscernInterface">Channel Fake Traffic Identification</span>

## <span id = "requestInterface">Specific Interface</span>

### <span id = "requestUrl">Request URL:</span>

| Cluster             | URL                                                 | Supported Product List |
| ------------------ | ----------------------------------------------------- | -------------- |
| Beijing             | `http://api-skynet-bj.fengkongcloud.com/v4/event`   | Skynet Risk Identification |
| USA (Virginia) | `http://api-skynet-fjny.fengkongcloud.com/v4/event` | Skynet Risk Identification |
| Singapore           | `http://api-skynet-xjp.fengkongcloud.com/v4/event`  | Skynet Risk Identification |
| Europe (Frankfurt) | `http://api-skynet-eur.fengkongcloud.com/v4/event`  | Skynet Risk Identification |

### <span id = "requestMethod">Request Method:</span>

`POST`

### <span id = "requestEncode">Character Encoding:</span>

`UTF-8`

### <span id = "requestTimeout">Recommended Timeout Duration:</span>

1s

### <span id = "requestParameters">Request Parameters:</span>

Request body = General Request Parameters + Event Specific Parameters (placed under the data field)

#### <span id = "general">General Request Parameters:</span>

Placed in the HTTP Body, in JSON format, with specific parameters as follows:


| **Parameter Name** | **Type**    | **Description**                                                                                         | **Required** | **Specification**                                                                                     |
| -------------- | ------------- | ------------------------------------------------------------------------------------------------------ | -------------- | ---------------------------------------------------------------------------------------------- |
| accessKey    | string      | Interface authentication key<br/>Used for permission authentication, provided by DeepCleer when opening account services or viewed in the relevant document section at the top right corner of the DeepCleer backend when logging in with the opening email | Required     | Assigned by DeepCleer                                                                                     |
| appId        | string      | Application ID, used to distinguish different applications of the same company                                                                   | Required     | The value of this parameter can be negotiated with DeepCleer                                                                     |
| eventId      | string      | Event ID, used to identify the event type.                                                                           | Required     | Different events correspond to different strategies, and the input parameters for different events may have slight differences. Please refer to the detailed parameter description for each event |
| data         | json_object | Request data content, event-specific parameters can be passed under data                                            | Required     | Request data content, maximum 10MB, [see data parameters](#data)                                              |

<span id = "data">The content of data is as follows:</span>

| **Parameter Name**    | **Type** | **Description**                                                                                                                                    | **Required** | **Specification**                                                                                                                                                                                                                                                                                                         |
| ----------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| tokenId         | string   | User account identifier, it is recommended to use your company's user UID (can be encrypted) to generate it yourself, identifying the user's unique identity for risk control in dimensions such as flooding and advertising. If there is no user UID scenario, it is recommended to use a unique data identifier | Required | This ID is the user's unique identifier and should be consistent with the tokenId of other DeepCleer interfaces                                                                                                                                                                                                                                                          |
| isTokenSeperate | int      | Whether to distinguish account systems. When different apps of the same company are accessed and the account systems are not unified, different users with the same account need to distinguish the account systems, `0` means no distinction, `1` means distinction.                     | Required     | Default is `0`, if passed as `1`, the token will be automatically processed as appid_token to distinguish different account systems                                                                                                                                                                                                                                         |
| ip              | string   | The client's public IPv4 address when the current business event occurs                                                                                                          | Required     | Non-internal IP                                                                                                                                                                                                                                                                                                         |
| timestamp       | int64    | The timestamp when the current business event occurs, in milliseconds (ms)                                                                                                    | Required     |                                                                                                                                                                                                                                                                                                                  |
| deviceId        | string   | DeepCleer device fingerprint identifier, generated by DeepCleer SDK                                                                                                                 | Strongly Recommended     | DeepCleer device fingerprint identifier, used for user behavior analysis.<br/>Note:<br/>1. For versions before integrating DeepCleer SDK, if deviceId cannot be obtained, pass an empty string "";<br/>2. Directly transmit the original field obtained by getDeviceId without decryption                                                               |
| os              | string   | Application end operating system type                                                                                                                              | Strongly Recommended     | Optional values: `android、harmony、ios、weapp、web、aliapp：Alipay Mini-Program Engine、ttapp：Douyin Mini-Program、tmapp：Tmall Mini-Program`                                                                                                                                                                                                                                                                               |
| appVersion      | string   | Application version number                                                                                                                                      | Strongly Recommended     | Pass the current version number of the application or service being used, in the format of 3 dots and 4 segments of numbers, with each segment having a maximum of 4 digits, for example, `3.2.1.1234`. If less than 4 segments, add 0, for example, `2.1.5` should be passed as `2.1.5.0`. If more than 4 segments, take the first 4 segments, for example, `2.1.5.1.1` should be passed as `2.1.5.1`.                                                                                                                            |
| activityId      | string   | Marketing activity ID                                                                                                                                      | Strongly Recommended     | It is recommended to pass the corresponding marketing activity identifier ID                                                                                                                                                                                                                                                                                       |
| activityType | string | Activity type, mainly divided into offline activities and online activities. Different strategies can be called according to different activity types | Recommended | Optional values: online_activity, offline_activity. online_activity refers to online activities, such as online check-in; offline_activity refers to offline activities, such as ground promotion and new user acquisition activities; |
| userAgent           | string   | User agent                                                                                                                                          | Strongly Recommended     |                     |
| countryCode     | string   | Country code of the mobile user                                                                                                                              | Strongly Recommended     | For mobile phone numbers in mainland China, fill in `0086`; for mobile phone numbers outside mainland China, fill in the country code, for example: USA fill in `0001`, Andorra `0376`, Bahamas `1242`                                                                                                                                                                                                         |
| phoneMd5        | string   | MD5 of the mobile phone number, 32-bit lowercase encrypted string                                                                                                                         | Recommended         |                                                                                                                                                                                                                                                                                                                  |
| phoneSha256     | string   | SHA256 encrypted string of the mobile phone number                                                                                                                            | Recommended         |                                                                                                                                                                                                                                                                                                                  |
| newCountryCode | string      | Country Code                                                                       | Recommended   | A four digit string. If the parameter was not passed in, the default value is China country code 0086                                                                                                                                                                                                       |
|role|string|Role|Strongly Recommended|Optional values (strict validation values can only be empty, `ADMIN`, `HOST`); `HOST` represents the host, and for the host, the rules for multiple openings, PC simulators, and the threshold of the association frequency strategy will be relaxed to ensure the normal experience and use of the host.|
| level           | int      | User level, different interception strategies can be configured for users of different levels                                                                                                  | Strongly Recommended     | Optional values: `0,1,2,3,4` corresponding to:<br/>`0`: lowest level user, typically new registered, completely inactive or level 0 users, etc.<br/>`1`: lower level user, typically low active or low level users, etc.<br/>`2`: medium level user, typically users with certain activity or medium level, etc.<br/>`3`: higher level user, typically high active or high level users, etc.<br/>`4`: highest level user, typically paying users, VIP users, users who can be considered to be let go, etc. |
| vdata           | json     | Micro-behavior data input field                                                                                                                              | Recommended         |                                                                                                                                                                                                                                                                                                                  |
| extra           | json     | User-defined data                                                                                                                                  | Recommended         | If not necessary, do not pass                                                                                                                                                                                                                                                                                                   |
| passThrough     | json     | User-defined pass-through field                                                                                                                              | Recommended         | If not necessary, do not pass                                                                                                                                                                                                                                                                                                   |

### <span id = "response">Response</span>

Placed in the HTTP Body, in JSON format, with specific parameters as follows:


| **Parameter Name**       | **Type**    | **Description**             | **Required** | **Specification**                                                                                                                                                                                                           |
| -------------------- | ------------- | -------------------------- | -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| code               | int         | Return code                   | Yes           | `1100`: Success<br/>`1901`: QPS limit exceeded<br/>`1902`: Invalid parameter<br/>`1903`: Service failure<br/>`1911`: Image download failure<br/>`9101`: Unauthorized operation<br/>`3000`: Operator error<br/>Except for message and requestId, other fields only exist when the code is 1100 |
| message            | string      | Return code description               | Yes           | Corresponds to the code: `Success QPS limit exceeded Invalid parameter Service failure Insufficient balance Unauthorized operation`                                                                                                                                                 |
| requestId          | string      | Request identifier                 | Yes           | Unique request identifier, used for troubleshooting and subsequent effect optimization, strongly recommended to save                                                                                                                                                             |
| riskevel           | string      | Handling suggestion for the current event       | Yes           | `PASS`: Pass<br/>`REVIEW`: Review<br/>`REJECT`: Reject<br/> `VERIFY`: Verify                                                                                                                                              |
| detail             | json_object | Risk detail information, see below for details | Yes           | See "detail result details" table                                                                                                                                                                                           |
| tokenProfileLabels | json_array  | Account attribute labels             | No           | See details below, only returned when the service is enabled                                                                                                                                                                                 |
| tokenRiskLabels    | json_array  | Account risk labels             | No           | See details below, only returned when the service is enabled                                                                                                                                                                                 |

Among them

1) Details of the detail result


| **Return Result Parameter Name** | **Parameter Type** | **Description**                             | **Specification**                                                                                                                                                                                                  |
| -------------------- | -------------- | ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| description        | string       | Risk description of the current event                       |                                                                                                                                                                                                           |
| model              | string       | Rule identifier, the highest priority rule identifier hit       |                                                                                                                                                                                                           |
| hits               | Json array   | All rule identifiers hit by the event                   | Each field in hits is an object, see "hits result details" for details                                                                                                                                                    |
| ip_country         | string       | IP country                             |                                                                                                                                                                                                           |
| ip_province        | string       | IP province                             |                                                                                                                                                                                                           |
| ip_city            | string       | IP city                             |                                                                                                                                                                                                           |
| verifyType         | string       | When the handling suggestion is `VERIFY`, return `verifyType` | `UPSMS`: Upstream SMS verification code<br/>`DOWNSMS`: Downstream SMS verification code<br/>`CAPTCHA`: Verification code (click or slide, etc.)<br/>`SEQUENCE`: Sequence verification (click or input)<br/>`SPATIAL`: Spatial logic reasoning<br/>`FACE`: Face detection<br/>`DELAY`: Delayed transaction |
| machineAccountRisk            | json_object       | Account historical blacklisting information                             | Only when the current account has been blacklisted in the account black library, the relevant transactions of the account will return this structure       |    
| smid | string | Device unique identifier | This field will be returned only when the deviceId field is passed and the configuration is enabled by contacting DeepCleer |

Among them, the details of the hits result are as follows:


| **Return Result Parameter Name** | **Parameter Type** | **Description**                             | **Specification**                                                                                                                                                                                                  |
| -------------------- | -------------- | ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| description        | string       | Risk description of the current event                       |                                                                                                                                                                                                           |
| model              | string       | Rule identifier                                 |                                                                                                                                                                                                           |
| riskLevel          | string       | Handling suggestion for the current event                       | `PASS`: Pass<br/>`REVIEW`: Review<br/>`REJECT`: Reject<br/>`VERIFY`: Verify                                                                                                                                      |
| verifyType         | string       | When the handling suggestion is `VERIFY`, return `verifyType` | `UPSMS`: Upstream SMS verification code<br/>`DOWNSMS`: Downstream SMS verification code<br/>`CAPTCHA`: Verification code (click or slide, etc.)<br/>`SEQUENCE`: Sequence verification (click or input)<br/>`SPATIAL`: Spatial logic reasoning<br/>`FACE`: Face detection<br/>`DELAY`: Delayed transaction |

Among them, the details of machineAccountRisk are as follows:


| **Return Result Parameter Name** | **Parameter Type** | **Description**         | **Specification**                   |
| -------------------- | -------------- | ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| tokenSampleLastTs        | int64       | Recent blacklisting time, in milliseconds                       |  For example: "tokenSampleLastTs": 1683026613000                                                                                                                                                                                                         |
| tokenSampleDesc              | string       | Description of historical blacklisting strategy                                 |  For example: "tokenSampleDesc": "High-risk device: IP abnormal aggregation"                                                                                                                                                                                                         |

Among them, the details of the label return fields

1) Details of tokenProfileLabels


| **Return Result Parameter Name** | **Parameter Type** | **Description**           | **Specification**                     |
| -------------------- | -------------- | ------------------------ | ------------------------------ |
| label1             | string       | Primary label               | Display the primary label of the account attribute label. |
| label2             | string       | Secondary label               | Display the secondary label of the account attribute label. |
| label3             | string       | Tertiary label               | Display the tertiary label of the account attribute label. |
| description        | string       | Risk description               | Display the Chinese description of the account attribute label. |
| timestamp          | int64        | Time of the most recent strategy hit | Time of the most recent strategy hit       |
| detail             | json_object  | Evidence description               | Evidence details                     |

2) Details of tokenRiskLabels


| **Return Result Parameter Name** | **Parameter Type** | **Description**           | **Specification**                     |
| -------------------- | -------------- | ------------------------ | ------------------------------ |
| label1             | string       | Primary label               | Display the primary label of the account risk label. |
| label2             | string       | Secondary label               | Display the secondary label of the account risk label. |
| label3             | string       | Tertiary label               | Display the tertiary label of the account risk label. |
| description        | string       | Risk description               | Display the Chinese description of the account risk label. |
| timestamp          | int64        | Time of the most recent strategy hit | Time of the most recent strategy hit       |
| detail             | json_object  | Evidence description               | Evidence details                     |

## <span id="eventIdList">Event List</span>


| **Industry** | **Scenario** | **Event** | **eventId** | **Specification**              |
| ---------- | ---------- | ---------- | ------------- | ----------------------- |
| General | Arbitrage - Channel Traffic Anti-Cheating | Activation                   | activation    |[Parameter Details](#activation)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | First Active               | firstActive   |[Parameter Details](#firstActive)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Register                   | register      |[Parameter Details](#register)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Guest Register               | guestRegister |[Parameter Details](#guestRegister)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Login                   | login         |[Parameter Details](#login)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Place Physical Goods Order         | order         |[Parameter Details](#order)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Place Virtual Goods Order (Recharge) | virtualOrder  |[Parameter Details](#virtualOrder)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Place Service Order             | serviceOrder  |[Parameter Details](#serviceOrder)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Withdraw                   | withdraw      |[Parameter Details](#withdraw)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Browse                   | browse        |[Parameter Details](#browse)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Like                   | like          |[Parameter Details](#like)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Collect                   | collect       |[Parameter Details](#collect)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Share                   | share         |[Parameter Details](#share)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Follow                   | follow        |[Parameter Details](#follow)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Sign In                   | signIn        |[Parameter Details](#signIn)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Do Task                 | task          |[Parameter Details](#task)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Enter Room               | enterRoom     |[Parameter Details](#enterRoom)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Comment                   | comment       |[Parameter Details](#comment)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Subscribe                   | subscribe     |[Parameter Details](#subscribe)    |
| General | Arbitrage - Channel Traffic Anti-Cheating | Payment                   | payment       |[Parameter Details](#payment)    |

## <span id="data"> Event Detailed API</span>

#### <span id="activation"> Activation </span>


| **Parameter Name