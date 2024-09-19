---
title: Cancel Correction Interface | Shumei
sidebar_label: Cancel Correction Interface

hide_title: true
description: 
keywords:
- Correction Document
---

# **Cancel Correction Interface**

---

All rights reserved, reproduction prohibited

---

## **Document Related Terminology**

Mandatory Parameter: Parameters that must be passed when using Shumei interface services, otherwise the interface service will not function properly;

Strongly Recommended: Parameters that are strongly recommended to be passed when using Shumei interface services, which can significantly improve recognition accuracy;

Optional: Parameters that are suggested to be passed when using Shumei interface services, which can improve recognition accuracy in specific scenarios.

## **Customer Configuration Information**

Request Header parameters are as follows:

| **Parameter Name** | **Type** | **Is Mandatory** | **Description**                        |
| ------------------ | -------- | ---------------- | -------------------------------------- |
| X-Accesskey        | string   | Mandatory        | Used for permission authentication, provided by Shumei when the account service is activated |

## **Text Correction Interface Definition**

Interface Description

This interface is used to provide the cancel correction function. It supports the cancellation of correction for request data.

Request URL: https://api-web.fengkongcloud.com/api/feedback/cancel

Character Encoding: UTF-8

Request Method: POST

Suggested Timeout Duration: 1s

Request Parameters

Placed in the HTTP Body, in Json format, the specific parameters are as follows:

| **Request Parameter Name** | **Type** | **Parameter Description**                                      | **Passing Instructions** | **Remarks** |
| -------------------------- | -------- | -------------------------------------------------------------- | ------------------------ | ----------- |
| requestId                  | string   | Unique identifier for text callback request, corresponding to the requestId in Shumei service return results | Mandatory Parameter      |             |
| serviceId                  | string   |                                                                | Mandatory Parameter      |             |

Placed in the HTTP Body, in Json format, the specific parameters are as follows:

| **Return Result Parameter Name** | **Parameter Type** | **Parameter Description** | **Is Mandatory Return** | **Remarks** |
| -------------------------------- | ------------------ | ------------------------- | ----------------------- | ----------- |
| code                             | string             | Return code               | Yes                     |             |
| message                          | string             | Description of return code details | Yes                     |             |

The list of code request return codes is as follows:

| **code** | **message** |
| -------- | ----------- |
| 1100     | Success     |
| 1101     | QPS Limit Exceeded |
| 1902     | xxxx        |

Example

Request Example:

~~~json
{
 "serviceId":"ACCOUNT_REGISTER",
 "requestId":"42c8d9a0b289ad0ed4c738e8bd4b1a2f"
}
~~~

Successful Return Example:

~~~json
{ 
"code": 1100, 
"message": "Success"
}
~~~

Error Return Example 1:

~~~json
{
"code": 1902,
"content": {},
"message": "Accesskey verification failed, please confirm whether the Accesskey is correct"
}
~~~

Error Return Example 2:

~~~json
{
"code": 1902,
"message": "The feedback record does not exist",
"content": {
"requestId": "e03c7fcfa38358cb12d9a49f0495f7b8"
}
}
~~~