---
title: Image Correction API Documentation | Shumei
sidebar_label: Image Correction API Documentation

hide_title: true
description: 
keywords:
- Correction Documentation
---

# Shumei Image Correction API Documentation

---

All rights reserved, reproduction prohibited

---

## Terminology Explanation

**Required Parameters**: Parameters that must be provided when using Shumei API services, otherwise the service will not function properly;

**Strongly Recommended**: Parameters that are strongly recommended to be provided when using Shumei API services to significantly improve recognition accuracy;

**Optional**: Parameters that are suggested to be provided when using Shumei API services to improve recognition accuracy in specific scenarios.

## Client Configuration Information

Request Header parameters are as follows:

| **Parameter Name** | **Type** | **Required** | **Description**                        |
| ------------------ | -------- | ------------ | -------------------------------------- |
| X-Accesskey        | string   | Required     | Used for authentication, provided by Shumei when the account service is activated |

## Image Correction API Definition

**API Description**

This API is used to provide image service correction functionality. It supports correction of request data from the past 2 days.

**Request URL**: https://api-web.fengkongcloud.com/api/feedback/image/add

**Character Encoding**: UTF-8

**Request Method**: POST

**Suggested Timeout**: 1s

**Request Parameters**

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Parameter Name** | **Type** | **Description**                                                | **Required** | **Remarks**                                                                                     |
| ------------------ | -------- | -------------------------------------------------------------- | ------------ | ---------------------------------------------------------------------------------------------- |
| requestId          | string   | Unique identifier for the image callback request, corresponds to the requestId in Shumei service results | Required     |                                                                                                |
| type               | string   | Correction type                                                | Required     | Options: error: false positive miss: false negative                                             |
| riskType           | int      | Content risk type, refer to the table below                    | Optional     | False positive: not provided defaults to "normal" False negative: not provided defaults to "blacklist", some services without "blacklist" default to "custom" |
| timestamp          | int      | Default can correct data within 2 days, if correction time is more than 2 days, this parameter must be provided | Optional     | The time the record occurred, 13-digit timestamp                                                |
| account            | string   | Account calling the API                                        | Optional     | e.g., test@ishumei.com                                                                          |
| appId              | array    | Correct for specified applications, up to 3                    | Optional     | e.g., ["default", "test"]                                                                       |
| channel            | array    | Correct for specified channels, up to 3                        | Optional     |                                                                                                |
| remark             | string   | Customer correction remarks, used for backend display          | Optional     |                                                                                                |

The list of riskType is as follows:

| **Risk Type** | **Description** |
| ------------- | --------------- |
| 0             | Normal          |
| 100           | Political       |
| 200           | Pornography     |
| 210           | Sexy            |
| 300           | Advertisement   |
| 310           | QR Code         |
| 400           | Terrorism       |
| 500           | Violation       |
| 520           | Minor           |
| 570           | Image Attribute |
| 700           | Blacklist       |

**Response Parameters**

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Response Parameter Name** | **Type** | **Description** | **Required** | **Remarks** |
| --------------------------- | -------- | --------------- | ------------ | ----------- |
| code                        | string   | Return code     | Yes          |             |
| message                     | string   | Return code description | Yes          |             |

The list of return codes is as follows:

| **code** | **message** |
| -------- | ----------- |
| 1100     | Success     |
| 1101     | QPS Exceeded |
| 1902     | xxxx        |

**Example**

Request example:
```json
{
  "riskType": 100,
  "type": "miss",
  "requestId": "xxx_a0000"
}
```

Successful response example:
```json
{
  "code": 1100,
  "message": "Success"
}
```

Error response example 1:
```json
{
  "code": 1902,
  "content": {},
  "message": "Accesskey verification failed, please confirm if the Accesskey is correct"
}
```

Error response example 2:
```json
{
  "code": 1902,
  "message": "The feedback record does not exist",
  "content": {
    "requestId": "e03c7fcfa38358cb12d9a49f0495f7b8"
  }
}
```