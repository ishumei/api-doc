Shumei Intelligent Voice Conversation Recognition API Interface Document ![](RackMultipart20230807-1-hn1vxm_html_1aad4b0c45090f7d.png)

**# Shumei Intelligent Voice Conversation Recognition API Interface Document**

**Provided by Beijing Shumei Times Technology Co., Ltd.**

**(All rights reserved, reproduction prohibited)**

Table of Contents

[Shumei Intelligent Voice Conversation Recognition](#_Toc12274551)API Interface Document 1

**[1.](#_Toc12274552)****Preparation Before Access **** 3**

[1.1](#_Toc12274553)Shumei Service Account Application 3

[1.2](#_Toc12274554)Channel Configuration Table 3

[1.3](#_Toc12274555)Receiving Shumei Service Account Information 3

**[2.](#_Toc12274556)****Intelligent Audio Filtering Service Interface Description **** 4**

[2.1](#_Toc12274557)Upload Voice Message File 4

[2.2](#_Toc12274558)Callback Audio Results 7

**[3.](#_Toc12274559)****Intelligent Audio Filtering Error Correction Interface Description **** 10**

[3.1](#_Toc12274560)Request Parameters 10

[3.2](#_Toc12274561)Return Parameters 11

[3.3](#_Toc12274562)Example 11

**[4. FAQ 12](#_Toc12274563)**

[4.1](#_Toc12274564)Interface Call Returns Parameter Error (1902) 12

[4.2](#_Toc12274565)Interface Call Returns No Permission to Operate (9101) 12

[4.3](#_Toc12274566)Interface Call Timeout Issue 12

# 1. Preparation Before Access

## 1.1 Shumei Service Account Application

The account manager has already contacted or visited your company in person and can directly provide the account opening and service-related information to the account manager.

The information required to open an account includes:

Company Full Name: xxxxxx

Company Abbreviation: xxxxxx

Contact Person Email: xxx@xxx.xxx

Contact Person Phone: 1xxxxxxxxxx

## 1.2 Channel Configuration Table

Shumei configures different channels based on the customer's different business scenarios and formulates targeted interception strategies. It also facilitates customers to filter and analyze data for different business scenarios. The corresponding table of business scenarios and channel values is as follows (supports customer customization):

| **Business Scenario** | **channel**** Value **|** Remarks** |
| --- | --- | --- |
| Group Chat Voice | GROUP\_CHAT | Voice messages in group chat |
| Private Chat Voice | MESSAGE | Voice messages in private chat |
| Recorded Voice | AUDIO\_PROFILE | Voice recording files |

## 1.3 Receiving Shumei Service Account Information

The Shumei account manager will open the corresponding Shumei account and service for you within 1 working day. Subsequently, the contact person's email will receive the following information:

| **Name** | **Specific Value** | **Description** |
| --- | --- | --- |
| accessKey | xxxxxx | Authentication code for Shumei API service, required when calling Shumei API |
| organization | xxxxxx | Unique identifier assigned by Shumei, required when calling SDK |
| Shumei Management Console Account | xxxxxx | Used to log in to the Shumei Management Console |
| Shumei Management Console Password | xxxxxx | Used to log in to the Shumei Management Console |
| Shumei Management Console Address | https://www.fengkongcloud.com | Used to log in to the Shumei Management Console |

# 2. Intelligent Voice Conversation Recognition Interface Description

## 2.1 Upload Voice Conversation File

**Interface Description**

This interface is used to submit information related to voice conversation files. The recognition results need to be obtained by the customer through the callback interface.

**Request**** URL ****：**

http://audio-api.fengkongcloud.com/v2/saas/anti\_fraud/conversation

**Character Encoding Format:** Use UTF-8 character set for encoding

**Request Method:** POST

**Suggested Timeout Duration** ：3s

**Request Parameters:**

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Parameter Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| accessKey | string | Y | Used for authentication, provided by Shumei when the account service is opened |
| type | string | Y | Type of violation recognition, optional values: DEFAULT: default valuePOLITICAL\_PORN\_MOANPORN: pornography recognitionAD: advertisement recognitionPOLITICAL: political recognitionMOAN: moan recognitionGENDER: gender recognitionFor combined recognition, connect with underscores, e.g., AD\_PORN\_POLITICAL for advertisement, pornography, and political recognition. |
| btId | string | Y | Unique identifier for the audio, used to query recognition results, within 40 characters. |
| data | json\_object | Y | Request data content, up to 1MB, detailed content see the table below |
| appId | string | N | Application identifier, used to distinguish different applications of the same company, the value of this parameter can be negotiated with Shumei service, if not passed, it is the default application |
| callback | string | Y | Callback http interface, the service will notify the user of the review results based on this field |
| callbackParam | json\_object | N | Callback pass-through field, the service will return the content of this field along with the audio result when sending the callback request |

The content of data is as follows:

| **Parameter Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| url | string | Y | URL address of the audio to be detected |
| channel | string | N | Data business scenario |

**Return Parameters**

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Parameter Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| code | int | Y | Return code |
| message | string | Y | Description of the return code |
| requestId | string | Y | Unique identifier for the request |
| btId | int | Y | Unique identifier for the audio uploaded by the customer, corresponding to the btId field in the request parameters |

The list of code and message is as follows:

| **code** | **message** |
| --- | --- |
| 1100 | Success |
| 1902 | Invalid parameter |
| 1903 | Service failure |
| 9100 | Insufficient balance |
| 9101 | No permission to operate |

**Example**

**Input Audio**** url ****Parameter Example**

{

"accessKey":"sdsfds",

"type":"DEFAULT",

"data":{

"url":"http://image.qinglv666.com/2018/11/05/152004399\_44183002.mp3",

"tokenId":"asdwef",

"returnAllText":true

},

"callback":"http://localhost:19989",

"appId":"asde",

"btId":"shesdwdddfr43f",

"callbackParam":{

"test1":1,

"test2":"qew",

"test3":true

}

}

**Return Example**

{

"code":1100,

"message":"Success",

"requestId":" a78eef377079acc6cdec24967ecde722",

"btId":"xxxx"

}

## 2.2 Callback Recognition Results

If the user needs the server to actively callback the audio detection results, the callback protocol interface URL callback parameter needs to be specified in the request parameters. The server will actively callback the user after the audio review is completed.

Supported Protocol:

HTTP

Character Encoding Format:

Requests use UTF-8 character set for encoding

Request Method:

POST

Timeout Duration:

5s

Push Strategy:

When the user receives the push result and returns the HTTP status code 200, it indicates a successful push; otherwise, the system will push up to 10 times.

Request Parameters:

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Parameter Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| code | int | Y | Return code |
| message | string | Y | Description of the return code |
| requestId | string | Y | Unique identifier for the request |
| btId | int | Y | Unique identifier for the audio |
| callbackParam | json\_object | N | Callback pass-through field |
| riskLevel | string | N | Risk level (exists when code is 1100)
Possible return values: PASS, REVIEW, REJECT
 PASS: Normal content, recommended to pass directly
 REVIEW: Suspicious content, recommended for manual review
 REJECT: Violation content, recommended to intercept directly |
| detail | json\_array | N | Risk details (exists when code is 1100) |

The specific parameters of each item in the detail array are as follows:

| **Parameter Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| startTime | int | Y | Start time of the audio segment in the audio, in milliseconds |
| endTime | int | Y | End time of the audio segment in the audio, in milliseconds |
| channelId | int | Y | Speaker identity of the segment |
| audioText | string | Y | Recognized text content of the segment |
| audioUrl | string | Y | Audio segment address |
| riskLevel | string | Y | Possible return values: REJECT, REVIEW
 REJECT: Violation content, recommended to intercept directlyREVIEW: Suspicious content, recommended for manual review |
| riskType | int | N | Risk type of the audio segment, possible values:
 0: Normal100: Political200: Pornography210: Abuse250: Moan300: Advertisement700: Blacklist900: Custom |
| description | string | Y | Description of the risk reason |

The list of code and message is as follows:

| **code** | **message** |
| --- | --- |
| 1100 | Success |
| 1101 | Processing |
| 1902 | Invalid parameter |
| 1903 | Service failure |
| 9100 | Insufficient balance |
| 9101 | No permission to operate |

**Return Example**

{

     "code":1100,

     "message":"Success",

     "requestId":"b891cf2d82e214de45df33fc2bea4875",

"riskLevel":"REJECT",

"btId":"xxxx",

"detail":[{

"audioStarttime":0,

"audioEndtime":4,

"channelId":0,

"description":"Political - Audio",

"riskLevel":"REJECT",

"audioText":"Falun Dafa is good",

"riskType":300

}]

}

# 3. Intelligent Audio Filtering Error Correction Interface Description

When customers find errors in the results while using Shumei services, they can use this interface to provide feedback on the errors to Shumei.

## 3.1 Request Parameters

**Request**** URL**

[https://www.fengkongcloud.com/ApiV2/Feedback/batchError](https://www.fengkongcloud.com/ApiV2/Feedback/batchError)

**Character Encoding**

Requests and return results use UTF-8 character set for encoding

**Request Method** ：

POST

Placed in the HTTP URL, with specific parameters as follows:

| **Field** | **Type** | **Description** | **Required** |
| --- | --- | --- | --- |
| accessKey | string | Used for authentication, provided by Shumei | Yes |

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Field** | **Type** | **Description** | **Required** |
| --- | --- | --- | --- |
| feedbackData | json\_array | Feedback data, an array of error correction data | Yes |
| serviceId | string | Service type, value: POST\_AUDIO | Yes |

Each element of the feedbackData array is error correction data, with the content as follows:

| **Field** | **Type** | **Description** | **Required** |
| --- | --- | --- | --- |
| organization | string | Company identifier | Yes |
| serviceId | string | Service type, value POST\_AUDIO | Yes |
| requestId | string | Serial number | Yes |
| btId | string | Unique business identifier, if the business does not have it, it is not required | No |
| timestamp | int | Request time of the serial number data, in milliseconds. | Yes |
| riskLevel | string | Risk levelPASS: PassREJECT: Reject | Yes |
| description | string | Risk reason remarks | Yes |

## 3.2 Return Parameters

Request successful, return HTTP STATUS 200, HTTP Body is an empty string;

Request failed, the return result is placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Field Name** | **Type** | **Description** | **Required** |
| --- | --- | --- | --- |
| code | string | Return code | Yes |
| message | string | Detailed description | Yes |

## 3.3 Example

curl -X POST -H "Content-Type: application/json" -d

'{{"organization":"xxxx","serviceId":"POST\_AUDIO","feedbackData":[{"organization":"xxx","serviceId":"POST\_AUDIO","requestId":"a2b8b5af8654072b2a76a4fb32f3fece","timestamp":1531734482000 ,"riskLevel":"REJECT","description":"Test Interface"}]}' https://www.fengkongcloud.com/ApiV2/Feedback/batchError?accessKey=xxxx

# 4. FAQ

## 4.1 Interface Call Returns Parameter Error (1902)

A: When calling the Shumei interface, if the code returns 1902 parameter invalid, it is generally because the format of the parameters entered by the customer is incorrect. The customer can analyze whether the request format is input according to the interface document, or provide the request data and return data to Shumei for analysis and resolution.

## 4.2 Interface Call Returns No Permission to Operate (9101)

A: When calling the Shumei interface, if the code returns 9101 no permission to operate, it is generally because the service that has not been opened is called. Confirm with the customer the service interface called and open the corresponding service.

## 4.3 Interface Call Timeout Issue

A: There are two common issues:

1) DNS issue:

When the customer calls the Shumei interface through the public network for testing, the customer's DNS resolves the domain name slowly, causing the first request to time out. It is recommended that the customer change the DNS. It is not recommended that the customer bind the domain name and IP in the host, as Shumei changes the interface IP, causing the interface to be inaccessible.

2) Network issue:

When the customer calls the Shumei interface through the public network, the public network latency is long, causing a small number of requests to time out. It is recommended that the customer ping different Shumei cluster networks and access the Shumei cluster with lower network latency.

12

**CONFIDENTIAL**