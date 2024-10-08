# Intelligent CAPTCHA Secondary Verification API Documentation

- - - - -

***All rights reserved. Reproduction prohibited***

- - - - -

* [Secondary Verification Interface](#sverifyInterface)
    + [Request Parameters](#requestParameter)
        - [Request URL](#requestUrl)
        - [Character Encoding Format](#requestEncode)
        - [Request Method](#requestMethod)
        - [Suggested Timeout Duration](#requestTimeout)
        - [Request Parameters](#requestParameters)
    + [Response](#response)
    + [Example](#example)
        - [Request Example](#requestExample)
        - [Response Example](#responseExample)

# <span id = "sverifyInterface">Secondary Verification Interface</span>

## <span id = "requestParameter">Request Parameters</span>

### <span id = "requestUrl">Request URL:</span>
| Cluster | URL | Supported Product List |
| --- | --- | --- |
| Beijing Cluster | `http://captcha-s.fengkongcloud.com/ca/v1/sverify` | Secondary Verification |
| Singapore Cluster | `http://captcha-xjp.fengkongcloud.com/ca/v1/sverify` | Secondary Verification |
| Virginia Cluster | `http://captcha-fjny.fengkongcloud.com/ca/v1/sverify` | Secondary Verification |

### <span id = "requestEncode">Character Encoding Format:</span>

`UTF-8`

### <span id = "requestMethod">Request Method:</span>

`POST`

### <span id = "requestTimeout">Suggested Timeout Duration:</span>

1s

### <span id = "requestParameters">Request Parameters:</span>

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| accessKey | string | Interface authentication key<br/>Used for permission authentication, provided by Shumei when opening account services or viewed in the relevant documents section in the upper right corner of the Shumei backend when logging in with the opening email | Required parameter | accessKey |
| data | json_object | Request data content | Required parameter | Request data content, up to 10MB, [see data parameters](#data) |

<span id = "data">Among them, the content of data is as follows:</span>

| **Request Parameter Name** | **Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| rid | string | Sliding CAPTCHA request identifier | Required parameter | Obtained by calling Shumei SDK |
| lastReq | string | The requestId of the previous Tianwang event before this verification interaction | Recommended parameter | Pass in the corresponding request ID of the previous CAPTCHA verification |
| ip | string | IP address | Required parameter | The client's public IPv4 address when sliding the CAPTCHA |
| tokenId | string | User account identifier, it is recommended to use your company's user UID (can be encrypted) to generate it yourself, used to identify the user's unique identity for risk control of behaviors such as flooding and advertising.<br/>In scenarios without user UID, it is recommended to use a unique data identifier to pass the value | Optional parameter | A string of up to 64 characters consisting of numbers, letters, underscores, and hyphens |
| deviceId | string | Shumei device identifier | Recommended parameter | The unique device identifier generated by Shumei's device fingerprint |
| expectedMode | string | Type of CAPTCHA | Recommended parameter | CAPTCHA types include:<br/> - slide: Sliding CAPTCHA<br/> - select: Text selection CAPTCHA<br/>- icon_select: Icon selection CAPTCHA<br/> - seq_select: Sequence selection CAPTCHA<br/> - spatial_select: Spatial reasoning CAPTCHA<br/> |
| expectedAppId | string | The application identifier (AppId: this identifier is the application identifier created in Shumei) expected to participate in the CAPTCHA verification | Recommended parameter | Return the application identifier that needs to verify the CAPTCHA verification result |

## <span id = "response">Response</span>

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name** | **Parameter Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| code | int | Return code | Yes |  `1100`: Success<br/>`1901`: QPS limit exceeded<br/>`1902`: Invalid parameters<br/>`1903`: Service failure<br/>`9101`: No permission to operate<br/><br/>Fields other than message and requestId will only exist when the code is 1100 |
| requestId | string | Request identifier | Yes | Unique request identifier, used for troubleshooting and subsequent effect optimization, strongly recommended to save |
| message | string | Return code description | Yes | Corresponding to code: Success QPS limit exceeded Invalid parameters Service failure Insufficient balance No permission to operate |
| riskLevel | string | Disposal suggestion | No | Possible return values:<br/>`PASS`: Normal, recommended to pass directly<br/>`REJECT`: Violation, recommended to intercept directly |
| score | int | Risk score of the current event, the higher the score, the greater the risk | No | [0, 1000] |
| detail | json_object | Risk detail information | No | [see detail result details](#detail) |

<span id = "detail">Among them, the detail result details are as follows:</span>

| **Parameter Name** | **Parameter Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| description | string | Risk description of the current event | Yes |  |
| descriptionV2 | string | Risk description of the current event | Yes |  |
| model | string | Rule identifier, the highest priority rule identifier hit | Yes |  |

## <span id = "example">Example:</span>

### <span id = "requestExample">Request Example:</span>

```json
{
    "accessKey":"****************",
    "data":{
        "lastReq":"****************"
        "expectedMode": "slide",
        "expectedAppId":"AppID",
        "rid":"************",
        "ip":"111.85.43.52"
    }
}
```

### <span id = "syncResponseExample">Synchronous Response Example:</span>

```json
{
    "code":1100,
    "detail":{
        "description":"Normal",
        "model":"M1000",
     },
    "message":"Success",
    "requestId":"585f346fd836f98403d72239f612b13a",
    "riskLevel":"PASS",
}

```