# **Shumei Text Manual Review API Documentation**

- - - - -

***All rights reserved. Reproduction prohibited***

- - - - -

## **1. API Interface Description**

### 1.1 Request Parameters

**Request URL:**

Beijing: <http://api-audit-bj.fengkongcloud.com/audit/text/v1>

**Character Encoding Format:** UTF-8 character set encoding

**Request Method:** POST

**Suggested Timeout Duration:** 5s

**Request Parameters:**

Placed in the HTTP Body, in Json format, specific parameters are as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Remarks** |
| --- | --- | --- | --- | --- |
| accessKey | string | Interface authentication key | Required parameter | Provided by Shumei |
| appId | string | Application identifier | Optional parameter | Used to distinguish applications, need to contact Shumei to activate, please use the value provided separately by Shumei |
| data | json_object | Request data content | Required parameter | Request data content, maximum 10MB |
| result | json_object | Machine review result information | Optional parameter |  |

The content of data is as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Is it Required** | **Remarks** |
| --- | --- | --- | --- | --- |
| text | string | Text content to be reviewed | Required parameter | |
| tokenId | string | User account identifier | Required parameter | |
| channel | string | Channel, used to distinguish different data review requirements | Optional parameter | |
| passThrough | Json_object | Pass-through field, the content here will be passed through to the callback interface, used by the business | Optional parameter | |

The content of result is as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Is it Required** | **Remarks** |
| --- | --- | --- | --- | --- |
| riskLevel | string | Disposal suggestion | Required parameter | Take the disposal suggestion returned by the machine review |
| riskLabel1 | string | Primary risk label | Required parameter | Take the primary risk label returned by the machine review |
| riskLabel2 | string | Secondary risk label | Required parameter | Take the secondary risk label returned by the machine review |
| riskLabel3 | string | Tertiary risk label | Required parameter | Take the tertiary risk label returned by the machine review |
| riskDescription | string | Risk reason | Required parameter | Take the risk reason returned by the machine review |

### 1.2 Return Results

Placed in the HTTP Body, in Json format, specific parameters are as follows:

| **Parameter Name** | **Parameter Type** | **Parameter Description** | **Is it Required** | **Remarks** |
| --- | --- | --- | --- | --- |
| code | int | Return code | Yes | 1100: Success<br/>1901: QPS limit exceeded<br/>1902: Invalid parameter<br/>1903: Service failure<br/>9101: No permission to operate<br/> |
| message | string | Return code description | Yes | Corresponding to code:<br/>Success<br/>QPS limit exceeded<br/>Invalid parameter<br/>Service failure<br/>No permission to operate |
| requestId | string | Request identifier | Yes | Unique request identifier, used for troubleshooting and subsequent effect optimization, strongly recommended to save |

### 1.3 Example

**Request Example**
```json
{
    "accessKey": "xxxx",
    "appId": "default",
    "data": {
        "tokenId": "df6afc8badc44130a37677b700b67f30",
        "text": "Mr. A Dou",
        "channel": "default",
        "passThrough": {

        }
    },
    "result": {
        "riskLevel": "REJECT",
        "riskLabel1": "ad",
        "riskLabel2": "ad",
        "riskLabel3": "ad",
        "riskDescription": "Advertisement: Advertisement: Advertisement"
    }
}
```