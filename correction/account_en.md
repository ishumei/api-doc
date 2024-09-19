---
title: Shumei Account Correction Interface Documentation
sidebar_label: Shumei Account Correction Interface Documentation

hide_title: true
description: 
keywords:
- Correction Documentation
---

# Shumei Account Correction Interface API Documentation

---

All rights reserved, reproduction prohibited

---


## Explanation of Terms Related to the Document

**Mandatory Parameters**: Parameters that must be provided by the user when using the Shumei interface service, otherwise the service will not function properly;

**Strongly Recommended**: Parameters that are strongly recommended to be provided by the user when using the Shumei interface service, which can significantly improve recognition accuracy;

**Recommended**: Parameters that are recommended to be provided by the user when using the Shumei interface service, which can improve recognition accuracy in specific scenarios.

## Customer Configuration Information

| Parameter Name | Value | Description                                      |
| -------------- | ----- | ------------------------------------------------ |
| accessKey      |       | Authentication code for Shumei API service, required when calling Shumei API |

## Account Correction Interface Definition

**Interface Description**

This interface provides the account correction function of the Skynet service.

**Request URL:** http://api-web.fengkongcloud.com/account/feedback/v2

**Character Encoding:** UTF-8

**Request Method:** POST

**Suggested Timeout Duration:** 1s

**Request Parameters**

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Request Parameter Name** | **Type** | **Parameter Description**                  | **Transmission Description** | **Remarks**                                                     |
| -------------------------- | -------- | ------------------------------------------ | ---------------------------- | --------------------------------------------------------------- |
| accessKey                  | string   | Used for authentication, provided by Shumei when the account service is activated | Mandatory Parameter          | Assigned by Shumei                                              |
| tokenId                    | string   | User account identifier                    | Mandatory Parameter          |                                                                 |
| type                       | string   | Correction type                            | Mandatory Parameter          | Optional values: error: false positive miss: false negative     |
| reason                     | string   | Reason for false positive or false negative | Mandatory Parameter          | False positive: highValueUser: high-level/paid user normalBehavior: no abnormal behavior normalContent: no abnormal content other: other False negative: riskBehavior: abnormal behavior riskContent: abnormal content other: other |
| appId                      | []string | Application                                | Optional                     |                                                                 |

**Return Parameters**

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Return Result Parameter Name** | **Parameter Type** | **Parameter Description** | **Mandatory Return** | **Remarks** |
| -------------------------------- | ------------------ | ------------------------- | -------------------- | ----------- |
| code                             | string             | Return code               | Yes                  |             |
| message                          | string             | Description of return code | Yes                  |             |

The list of return codes is as follows:

| **code** | **message** |
| -------- | ----------- |
| 1100     | Success     |
| 1101     | QPS Exceeded |

**Example**

Request Example:
```
{  
"accessKey":"GZAWATIeVzpq79fFbNc1",  
"reason":"normalBehavior",  
"type":"error",  
"tokenId":"900019-12"  
}
```
Return Example:
```
{  
"code": 1100,  
"message": "Success"  
}
```