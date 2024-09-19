# Intelligent Number Authentication Product API Documentation

- - - - -

***All rights reserved. Reproduction is prohibited.***

- - - - -

Table of Contents

- [Intelligent Number Authentication Product API Documentation](#intelligent-number-authentication-product-api-documentation)
- [One-Click Login Interface](#one-click-login-interface)
  - [One-Click Login Request](#one-click-login-request)
    - [Request URL:](#request-url)
    - [Request Method:](#request-method)
    - [Character Encoding:](#character-encoding)
    - [Suggested Timeout:](#suggested-timeout)
    - [Request Parameters:](#request-parameters)
  - [Return Results](#return-results)
  - [Examples:](#examples)
    - [Request Example:](#request-example)
    - [Synchronous Return Example:](#synchronous-return-example)
- [Local Number Verification Interface](#local-number-verification-interface)
  - [Local Number Verification Request](#local-number-verification-request)
    - [Request URL:](#request-url-1)
    - [Request Method:](#request-method-1)
    - [Character Encoding:](#character-encoding-1)
    - [Suggested Timeout:](#suggested-timeout-1)
    - [Request Parameters:](#request-parameters-1)
  - [Return Results](#return-results-1)
  - [Examples:](#examples-1)
    - [Request Example:](#request-example-1)
    - [Synchronous Return Example:](#synchronous-return-example-1)

# One-Click Login Interface

## One-Click Login Request

### Request URL:
| Cluster | URL | Supported Product List |
| --- | --- | --- |
| Beijing | `http://api-oneclick-bj.fengkongcloud.com/oneclick/v1` | One-Click Login |

### Request Method:

`POST`

### Character Encoding:

`UTF-8`

### Suggested Timeout:

2s

### Request Parameters:

Placed in the HTTP Body, in Json format, specific parameters are as follows:
| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| accessKey | string | Interface authentication key<br/>Used for permission authentication, provided by Shumei when opening account service or viewed in the relevant document section at the top right corner of Shumei backend using the opened email login | Required parameter | accessKey |
| appId | string | Application identifier, used to distinguish different application data of the same company | Required parameter | Default application value: `default`<br/>Contact Shumei service for assistance when passing other values |
| data | json_object | Request data content | Required parameter | Request data content, up to 10MB, [see data parameter](#data) |

Among them, the content of data is as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| tokenId | string | User account identifier, it is recommended to use your company's user UID (can be encrypted) to generate it yourself, identifying the user's unique identity for dimension risk control such as flooding and advertising. If there is no user UID scenario, it is recommended to use a unique data identifier to pass the value | Optional parameter | A string of up to 64 characters consisting of numbers, letters, underscores, and hyphens |
| accessToken | string | Operator authorization code, used to obtain the mobile phone number from the operator | Required parameter | Obtained by calling Shumei SDK |
| os | string | Platform type identifier | Required parameter | Platform type<br/>Optional values:<br/>`android`<br/>`ios`<br/>`weapp`<br/>`h5` |
| deviceInfo  | string | Encrypted browser fingerprint, collected by weapp/h5 end SDK. android/ios can pass empty value | Required parameter | Obtained by calling Shumei SDK |

## Return Results

Placed in the HTTP Body, in Json format, specific parameters are as follows:

| **Parameter Name** | **Parameter Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| code | int | Return code | Yes |  `1100`: Success<br/>`1901`: QPS limit exceeded<br/>`1902`: Invalid parameter<br/>`1903`: Service failure<br/>`1911`: Image download failure<br/>`9101`: No permission to operate<br/>`3000`: Operator error<br/>Except for message and requestId, other fields only exist when the code is 1100 |
| message | string | Return code description | Yes | Corresponds to code: Success QPS limit exceeded Invalid parameter Service failure Insufficient balance No permission to operate |
| requestId | string | Request identifier | Yes | Unique request identifier, used for troubleshooting and subsequent effect optimization, strongly recommended to save |
| phoneRsa | string | Request result | Yes | Encrypted mobile phone number, encryption method: RSA encryption, customer uses private key to decrypt |

## Examples:

### Request Example:

```json
{
    "accessKey":"",
    "appId":"",
    "data":{
        "tokenId":"username123",
        "accessToken":"",
        "os":"android",
        "deviceInfo":""
    }
}
```

### Synchronous Return Example:

```json
{
    "code":1100,
    "message":"Success",
    "requestId":"69dbc1f81dc5c914b1f1b8a267fb9ec1",
    "phoneRsa":""
}
```

# Local Number Verification Interface

## Local Number Verification Request

### Request URL:
| Cluster | URL | Supported Product List |
| --- | --- | --- |
| Beijing | `http://api-phoneverify-bj.fengkongcloud.com/phoneverify/v1` | Local Number Verification |

### Request Method:

`POST`

### Character Encoding:

`UTF-8`

### Suggested Timeout:

2s

### Request Parameters:

Placed in the HTTP Body, in Json format, specific parameters are as follows:
| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| accessKey | string | Interface authentication key<br/>Used for permission authentication, provided by Shumei when opening account service or viewed in the relevant document section at the top right corner of Shumei backend using the opened email login | Required parameter | accessKey |
| appId | string | Application identifier, used to distinguish different application data of the same company | Required parameter | Default application value: `default`<br/>Contact Shumei service for assistance when passing other values |
| eventId | string | Event identifier | Required parameter | It is recommended to pass in the local number verification scenario for distinction, otherwise it is `default` |
| data | json_object | Request data content | Required parameter | Request data content, up to 10MB, [see data parameter](#data) |

Among them, the content of data is as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| lastReq | string | Preceding Skynet event request identifier | Optional parameter | Request requestId returned by the preceding Skynet event |
| tokenId | string | User account identifier, it is recommended to use your company's user UID (can be encrypted) to generate it yourself, identifying the user's unique identity for dimension risk control such as flooding and advertising. If there is no user UID scenario, it is recommended to use a unique data identifier to pass the value | Optional parameter | A string of up to 64 characters consisting of numbers, letters, underscores, and hyphens |
| accessToken | string | Operator authorization code, used to obtain the mobile phone number from the operator | Required parameter | Obtained by calling Shumei SDK |
| phoneRsa | string | Mobile phone number | Required parameter | Encrypted mobile phone number, used for subsequent verification with the operator whether it is a local number<br/>Encryption method: RSA encryption. Use public key encryption when requesting |
| os | string | Platform type identifier | Required parameter | Platform type<br/>Optional values:<br/>`android`<br/>`ios`<br/>`weapp`<br/>`h5` |
| deviceInfo  | string | Encrypted browser fingerprint, collected by weapp/h5 end SDK. android/ios can pass empty value | Required parameter | Obtained by calling Shumei SDK |

## Return Results

Placed in the HTTP Body, in Json format, specific parameters are as follows:

| **Parameter Name** | **Parameter Type** | **Parameter Description** | **Is Required** | **Specification** |
| --- | --- | --- | --- | --- |
| code | int | Return code | Yes |  `1100`: Success<br/>`1901`: QPS limit exceeded<br/>`1902`: Invalid parameter<br/>`1903`: Service failure<br/>`1911`: Image download failure<br/>`9101`: No permission to operate<br/>`3000`: Operator error<br/>Except for message and requestId, other fields only exist when the code is 1100 |
| message | string | Return code description | Yes | Corresponds to code: Success QPS limit exceeded Invalid parameter Service failure Insufficient balance No permission to operate |
| requestId | string | Request identifier | Yes | Unique request identifier, used for troubleshooting and subsequent effect optimization, strongly recommended to save |
| riskLevel | string | Verification result | Yes | Possible return values:<br/>`PASS`: Normal (operator returns authentication passed)<br/>`REVIEW`: Retry (operator returns unable to confirm)<br/>`REJECT`: Reject (operator returns authentication failed) |

## Examples:

### Request Example:

```json
{
    "accessKey":"",
    "appId":"",
    "eventId":"",
    "data":{
        "lastReq":"",
        "tokenId":"username123",
        "accessToken":"",
        "phoneRsa":"",
        "os":"android",
        "deviceInfo":""
    }
}
```

### Synchronous Return Example:

```json
{
    "code":1100,
    "message":"Success",
    "requestId":"69dbc1f81dc5c914b1f1b8a267fb9ec1",
    "verifyResult":3
}
```