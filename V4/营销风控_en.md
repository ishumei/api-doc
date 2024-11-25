# Shumei Skynet - Marketing Risk Control

---

All rights reserved. Unauthorized reproduction is prohibited.

---

[Marketing Risk Control](#riskDiscernInterface)
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
+ [Query Interface](#query)
    + [Interface Definition](#query)
        - [Request URL](#queryUrl)
        - [Character Encoding Format](#queryEncode)
        - [Request Method](#queryMethod)
        - [Suggested Timeout Duration](#queryTimeout)
        - [Request Parameters](#queryParameters)
        - [Response](#queryResponse)
    + [Examples](#queryExample)
        - [Request Example](#queryExp)
        - [Response Example](#queryResponseExp)
        - [Encrypted Transport](#encryptedTransport)


# <span id = "riskDiscernInterface">Marketing Risk Control</span>

## <span id = "requestInterface">Specific Interface</span>

### <span id = "requestUrl">Request URL:</span>


| Cluster             | URL                                                 | Supported Product List |
| ------------------ | ----------------------------------------------------- | -------------- |
| Beijing             | `http://api-skynet-bj.fengkongcloud.com/v4/event`   | Skynet Risk Identification |
| Virginia, USA | `http://api-skynet-fjny.fengkongcloud.com/v4/event` | Skynet Risk Identification |
| Singapore           | `http://api-skynet-xjp.fengkongcloud.com/v4/event`  | Skynet Risk Identification |
| Frankfurt, Europe | `http://api-skynet-eur.fengkongcloud.com/v4/event`  | Skynet Risk Identification |

### <span id = "requestMethod">Request Method:</span>

`POST`

### <span id = "requestEncode">Character Encoding:</span>

`UTF-8`

### <span id = "requestTimeout">Suggested Timeout Duration:</span>

1s

### <span id = "requestParameters">Request Parameters:</span>

Request body = General Request Parameters + Event Specific Parameters (placed under the data field)

#### <span id = "general">General Request Parameters:</span>

Placed in the HTTP Body in JSON format, specific parameters are as follows:


| **Parameter Name** | **Type**    | **Description**                                                                                         | **Required** | **Specification**                                                                                     |
| ---------------- | ------------- | ------------------------------------------------------------------------------------------------------ | -------------- | ---------------------------------------------------------------------------------------------- |
| accessKey      | string      | Interface authentication key<br/>Used for permission authentication, provided by Shumei when opening account services or viewed in the relevant document section in the upper right corner of the Shumei backend when logging in with the opening email | Required     | Assigned by Shumei                                                                                     |
| appId          | string      | Application ID, used to distinguish different applications of the same company                                                                   | Required     | This parameter value can be negotiated with Shumei.<br/>  When the company's app has multiple vest packages, there are two situations:<br/>1. Vest package accounts are interoperable, different vest package data is passed into different appIds, and tokenIds remain consistent<br/>2. Vest package accounts are independent, different vest package data is passed into different appIds, and tokenIds must be different                                                                   |
| eventId        | string      | Event ID, used to identify the event type.                                                                           | Required     | Different events correspond to different strategies, and the input parameters for different events may have slight differences. Please refer to the detailed parameter description for each event |
| data           | json_object | Request data content, event-specific parameters can be passed under data                                            | Required     | Request data content, maximum 10MB, [see data parameters](#data)                                           |

<span id = "data">Among them, the content of data is as follows:</span>


| **Parameter Name** | **Type**    | **Description**                                                                                         | **Required** | **Specification**                                                                                     |
| ----------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| tokenId         | string   | User account identifier, it is recommended to use your company's user UID (can be encrypted) to generate it yourself, used to identify the unique identity of the user for risk control of behaviors such as flooding and advertising. In scenarios without user UIDs, it is recommended to use a unique data identifier | Required | Note:<br/>1. The tokenId should be the user ID in the business system, and it must be ensured that the user ID value passed in for the same user in all events connected to Shumei is consistent and unique<br/>2. If there are multiple applications under the same company entity and accounts need to be distinguished by application, that is, the same user ID represents different users in different applications. It is recommended to prepend the application identifier to the user ID before passing it into the tokenId field to achieve the purpose of distinguishing account systems. <br/>3. If there is no need for distinction, directly pass the user ID into the tokenId field<br/>4. If passed incorrectly, the system will consider it as different accounts, and the behavior of the accounts cannot be associated, leading to inaccurate identification                                                                                                                                                                             |
| ip              | string   | The public IPv4 or IPv6 address of the client when the current business event occurs                                                                                                          | Required     | Non-internal IP                                                                                                                                                                                                                            |
| timestamp       | int64    | The timestamp when the current business event occurs, in milliseconds (ms)                                                                                                    | Required     |                                                                                                                                                                                                                                     |
| deviceId        | string   | Shumei device fingerprint identifier, generated by Shumei SDK                                                                                                                 | Strongly Recommended     | Shumei device fingerprint identifier, used for user behavior analysis.<br/>Note:<br/>1. For versions before integrating Shumei SDK, if the deviceId cannot be obtained, pass an empty string "";                                                          <br/>     2. Directly pass the original field obtained by getDeviceId without decryption |
| os              | string   | Application end operating system type                                                                                                                              | Strongly Recommended     | Optional values: `android、harmony、ios、weapp、web、aliapp：Alipay Mini-Program Engine、ttapp：Douyin Mini-Program、tmapp：Tmall Mini-Program`                                                                                                                                                                                                   |
| appVersion      | string   | Application version number                                                                                                                                      | Strongly Recommended     | Pass the current version number of the application or service being used, in the format of 3 dots and 4 segments of numbers, with each segment having a maximum of 4 digits, for example, `3.2.1.1234`. If less than 4 segments, pad with 0, for example, `2.1.5` should be passed as `2.1.5.0`. If more than 4 segments, take the first 4 segments, for example, `2.1.5.1.1` should be passed as `2.1.5.1`.                                                         |
| activityId      | string   | Marketing activity ID     | Strongly Recommended     | It is recommended to pass the corresponding marketing activity identifier ID      |
| activityType | string | Activity type, mainly divided into offline activities and online activities. Different strategies can be called based on different activity types | Recommended | Optional values: online_activity, offline_activity. online_activity refers to online activities, such as online check-ins; offline_activity refers to offline activities, such as ground promotion activities; |
| userAgent           | string   | User agent                                                                                                                                          | Strongly Recommended     |                                   |
| countryCode     | string   | Country code of the mobile user                                                                                                                              | Strongly Recommended     | For mobile numbers in mainland China, fill in `0086`; for mobile numbers outside mainland China, fill in the country code, for example: the United States is `0001`, Andorra is `0376`, the Bahamas is `1242`                                                                                                                            |
| phoneMd5        | string   | MD5 of the mobile number, 32-bit lowercase encrypted string     | Recommended         |               |
| phoneSha256     | string   | SHA256 encrypted string of the mobile number                                                                                                                            | Recommended         |                                                                                                                                                                                                                                     |
|role|string|Role|Strongly Recommended|Optional values (strict validation, values can only be empty, `ADMIN`, `HOST`); `HOST` represents the host, for the host, multi-opening, PC emulator rules, and associated frequency strategy thresholds will be relaxed to ensure the normal experience and use of the host.|
| level        | int     | User level, different interception strategies can be configured for different levels of users    | Strongly Recommended | Optional values: `0,1,2,3,4` corresponding to: <br/>`0`: lowest level user, typically new registered, completely inactive or level 0 users, etc.<br/>`1`: lower level user, typically low active or low level users, etc.<br/>`2`: medium level user, typically users with certain activity or medium level, etc.<br/>`3`: higher level user, typically high active or high level users, etc.<br/>`4`: highest level user, typically paying users, VIP users, users who can be considered to be let through, etc. |
| vdata           | json     | Micro-behavior data input field   | Recommended         |  If it exists, the value cannot be empty      |
| extra           | json     | User-defined data                                                                                                                                  | Recommended         | If not necessary, do not pass, if it exists, the value cannot be empty   |
| passThrough     | json     | User-defined pass-through field                                                                                                                              | Recommended         | If not necessary, do not pass, if it exists, the value cannot be empty|

### <span id = "response">Response</span>

Placed in the HTTP Body in JSON format, specific parameters are as follows:


| **Parameter Name** | **Type**    | **Description**                                                                                         | **Required** | **Specification**                                                                                     |
| -------------------- | -------------- | -------------------------- | -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| code               | int          | Return code                   | Yes           | `1100`: Success<br/>`1901`: QPS limit exceeded<br/>`1902`: Invalid parameters<br/>`1903`: Service failure<br/>`9101`: Unauthorized operation<br/>Fields other than message and requestId will only exist when the code is 1100 |
| message            | string       | Return code description               | Yes           | Corresponding to the code: `Success QPS limit exceeded Invalid parameters Service failure Unauthorized operation`                                                                                                                                                 |
| requestId          | string       | Request identifier                 | Yes           | Unique request identifier, used for troubleshooting and subsequent effect optimization, strongly recommended to save                                                                                                                                                             |
| riskLevel           | string       | Handling suggestion for the current event       | Yes           | `PASS`: Pass<br/>`REVIEW`: Review<br/>`REJECT`: Reject<br/> `VERIFY`: Verify                                                                                                                                            |
| detail             | json_object  | Risk detail information, see below for details | Yes           | See the "detail result details" table                                                                                                                                                                                           |
| tokenProfileLabels | json_array   | Account attribute labels             | No           | See below for details, only returned when the service is enabled                                                                                                                                                                                 |
| tokenRiskLabels    | json_array   | Account risk labels             | No           | See below for details, only returned when the service is enabled                                                                                                                                                                                 |

Among them

1) Details of the detail result


| **Return Result Parameter Name** | **Parameter Type** | **Description**         | **Specification**                   |
| -------------------- | -------------- | ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| description        | string       | Risk description of the current event                       |                                                                                                                                                                                                           |
| model              | string       | Rule identifier, identifier of the highest priority rule hit       |                                                                                                                                                                                                           |
| hits               | Json array   | All rule identifiers hit by the event                   | Each field in hits is an object, see "hits result details" for details                                                                                                                                                    |
| ip_country         | string       | IP country                             |                                                                                                                                                                                                           |
| ip_province        | string       | IP province                             |                                                                                                                                                                                                           |
| ip_city            | string       | IP city                             |                                                                                                                                                                                                           |
| verifyType         | string       | When the handling suggestion is `VERIFY`, return `verifyType` | `UPSMS`: Upstream SMS verification code<br/>`DOWNSMS`: Downstream SMS verification code<br/>`CAPTCHA`: Verification code (click or slide, etc.)<br/>`SEQUENCE`: Sequence verification (click or input)<br/>`SPATIAL`: Spatial logic reasoning<br/>`FACE`: Face detection<br/>`DELAY`: Delayed transaction |
| machineAccountRisk            | json_object       | Account historical blacklisting information                             | Only when the current account has been blacklisted in the account blacklist, the relevant transactions of the account will return this structure       | 
| smid | string | Device unique identifier | This field will be returned only when the deviceId field is passed and the configuration is enabled by contacting Shumei |

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
| tokenSampleLastTs        | int64       | Most recent blacklisting time, in milliseconds                       |  For example: "tokenSampleLastTs": 1683026613000                                                                                                                                                                                                         |
| tokenSampleDesc              | string       | Description of historical blacklisting strategy                                 |  For example: "tokenSampleDesc": "High-risk device: IP abnormal aggregation"                                                                                                                                                                                                         |


Among them, the details of the label return fields

1) Details of tokenProfileLabels


| **Return Result Parameter Name** | **Parameter Type** | **Description**         | **Specification**                   |
| ---------------------- | ---------------- | ------------------------ | ------------------------------ |
| label1               | string         | First-level label               | Display the first-level label of the account attribute label. |
| label2               | string         | Second-level label               | Display the second-level label of the account attribute label. |
| label3               | string         | Third-level label               | Display the third-level label of the account attribute label. |
| description          | string         | Risk description               | Display the Chinese description of the account attribute label. |
| timestamp            | int64          | Time of the most recent strategy hit | Time of the most recent strategy hit       |
| detail               | json_object    | Evidence description               | Evidence details                     |

2) Details of tokenRiskLabels


| **Return Result Parameter Name** | **Parameter Type** | **Description**         | **Specification**                   |
| ---------------------- | ---------------- | ------------------------ | ------------------------------ |
| label1               | string         | First-level label               | Display the first-level label of the account risk label. |
| label2               | string         | Second-level label               | Display the second-level label of the account risk label. |
| label3               | string         | Third-level label               | Display the third-level label of the account risk label. |
| description          | string         | Risk description               | Display the Chinese description of the account risk label. |
| timestamp            | int64          | Time of the most recent strategy hit | Time of the most recent strategy hit       |
| detail               | json_object    | Evidence description               | Evidence details                     |

## <span id="eventIdList">Event List</span>

| **Industry** |**Scenario** | **Event**       | **eventId**     |**Specification**    |
| ---------- | ----------------------------- | ----------------------------- | ------|----------- |
| General     | Profit - New User Acquisition                   | Initiate Invitation                    | invite          |[Parameter Details](#invite)          |
| General     | Profit - New User Acquisition                   | Accept Invitation                    | fission         |[Parameter Details](#fission)          |
| General     | Profit - Routine Check-in               | Check-in                        | signIn          |[Parameter Details](#signIn)          |
| General     | Profit - Other Tasks               | Do Task                      | task            |[Parameter Details](#task)          |
| General     | Profit - Assistance                   | Initiate Invitation                    | invite          |[Parameter Details](#invite)          |
| General     | Profit - Assistance                   | Assist                        | help            |[Parameter Details](#help)          |
| General     | General                        | Feedback                        | feedback        |[Parameter Details](#feedback)          |
| General     | General                        | Verification Result                    | verify          |[Parameter Details](#verify)          |
| General     | General                        | Issue Coupon                        | postCoupon      |[Parameter Details](#postCoupon)          |
| General     | General                        | Click                        | click           |[Parameter Details](#click)          |
| General     | General                        | Submit Order                    | submitOrder     |[Parameter Details](#submitOrder)          |
| General     | General                        | Underwriting                        | underwrite      |[Parameter Details](#underwrite)          |
| General     | General                        | Platform Reward Event                | platformAward   |[Parameter Details](#platformAward)          |
| General     | Profit - Incentive Video               | Ad Request Event                | adGet           |[Parameter Details](#adGet)          |
| Online Earning     | Profit - Incentive Video               | Ad Display Event                | adShow          |[Parameter Details](#adShow)          |
| Online Earning     | Profit - Incentive Video               | Issue Reward Event                | adReward        |[Parameter Details](#adReward)          |
| Online Earning     | Profit - Incentive Video               | Ad Click Event                | adClick         |[Parameter Details](#adClick)          |
| Online Earning     | Profit - Incentive Video               | Ad Disappearance Event                | adFinish        |[Parameter Details](#adFinish)          |
| Online Earning     | Profit