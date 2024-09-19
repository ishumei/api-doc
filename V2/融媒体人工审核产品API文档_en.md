# **Shumei Media Content Review API Documentation**

- - - - -

***All rights reserved. Unauthorized reproduction is prohibited***

- - - - -



## **1. API Interface Description**

### 1.1 Request Parameters

**Request URL:**

Beijing: <http://api-audit-bj.fengkongcloud.com/audit/media/v1>

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

Among them, the content of data is as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Is Required** | **Remarks** |
| --- | --- | --- | --- | --- |
| tokenId | string | User account identifier | Required parameter |     |
| contents | json_object | Information of texts, images, audios, videos to be detected, each is an array type, see example for details | Required parameter | Detection data, up to 40 text contents when type is texts;  <br/>up to 100 image URLs when type is images;  <br/>up to 10 audio URLs when type is audios;  <br/>up to 10 video URLs when type is videos; |
| channel | string | Channel, used to distinguish different data review requirements | Optional parameter |     |
| passThrough | Json_object | Pass-through field, the content here will be passed to the callback interface, used by the business | Optional parameter |     |



### 1.2 Return Results

Placed in the HTTP Body, in Json format, specific parameters are as follows:

| **Parameter Name** | **Parameter Type** | **Parameter Description** | **Is Required** | **Remarks** |
| --- | --- | --- | --- | --- |
| code | int | Return code | Yes   | 1100: Success<br/>1901: QPS limit exceeded<br/>1902: Invalid parameter<br/>1903: Service failure<br/>9101: No permission to operate<br/> |
| message | string | Return code description | Yes   | Corresponding to code:<br/>Success<br/>QPS limit exceeded<br/>Invalid parameter<br/>Service failure<br/>No permission to operate |
| requestId | string | Request identifier | Yes   | Unique request identifier, used for troubleshooting and subsequent effect optimization, strongly recommended to save |

### 1.3 Example

**Request Example**
```json
{
    "accessKey": "",
    "appId": "default",
    "data": {
        "channel": "DEFAULT",
        "tokenId": "username123",
        "contents": {
            "texts": [
                "Add a friend qq12345",
                "Phone number 12345"
            ],
            "images": [
                "http://xxx.com/1.jpg",
                "http://xxx.com2.jpg"
            ],
            "audios": [
                "http://1.wav",
                "http://2.wav"
            ],
            "videos": [
                "http://1.mp4"
            ]
        },
        "passThrough": {

        }
    }
}
```  