---
title: Text Correction API Documentation | Shumei
sidebar_label: Text Correction API Documentation

hide_title: true
description: 
keywords:
- Correction Documentation
---

# Shumei Text Correction API Documentation

---

Copyright, all rights reserved

---

## Explanation of Terms Related to the Document

**Required Parameters**: Parameters that must be passed when using the Shumei API service, otherwise the service will not function properly;

**Strongly Recommended**: Parameters that are strongly recommended to be passed when using the Shumei API service, which can significantly improve recognition accuracy;

**Optional**: Parameters that are suggested to be passed when using the Shumei API service, which can improve recognition accuracy in specific scenarios.

## Customer Configuration Information

Request Header parameters are as follows:

| **Parameter Name** | **Type** | **Required** | **Description**                        |
| ------------------ | -------- | ------------ | -------------------------------------- |
| X-Accesskey        | string   | Required     | Used for authentication, provided by Shumei when the account service is activated |

## Text Correction API Definition

**API Description**

This API is used to provide text service correction functionality. It supports correction of request data from the past 2 days.

**Request URL**: https://api-web.fengkongcloud.com/api/feedback/text/add

**Character Encoding**: UTF-8

**Request Method**: POST

**Suggested Timeout**: 1s

**Request Parameters**

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Request Parameter Name** | **Type** | **Parameter Description**                                         | **Required** | **Remarks**                                                                                       |
| -------------------------- | -------- | ---------------------------------------------------------------- | ------------ | ---------------------------------------------------------------------------------------------- |
| requestId                  | string   | Unique identifier for the text callback request, corresponding to the requestId in the Shumei service return result | Required     |                                                                                                |
| type                       | string   | Correction type                                                  | Required     | Optional values: error: false positive miss: false negative                                     |
| riskType                   | int      | Content risk type, refer to the table below                      | Optional     | False positive: not passed, default handled as "normal" False negative: mandatory provided, default handled as "blacklist", some services without "blacklist" are handled as "custom" |
| timestamp                  | int      | Default can correct data within 2 days, if correction time is greater than 2 days, this parameter must be passed | Optional     | The time the record occurred, 13-digit timestamp                                                |
| account                    | string   | Account calling the API                                          | Optional     | e.g., test@ishumei.com                                                                           |
| appId                      | array    | Correct for specified applications, up to 3                      | Optional     | e.g., ["default","test"]                                                                         |
| channel                    | array    | Correct for specified channels, up to 3                          | Optional     |                                                                                                |
| remark                     | string   | Customer correction remarks, used for backend display            | Optional     |                                                                                                |

Among them, the list of riskType is as follows:

| **Risk Type** | **Description** |
| ------------- | --------------- |
| 0             | Normal          |
| 100           | Political       |
| 200           | Pornographic    |
| 210           | Abusive         |
| 300           | Advertisement   |
| 340           | Online Fraud    |
| 500           | Meaningless     |
| 600           | Prohibited      |
| 700           | Other           |
| 900           | Custom          |

**Return Parameters**

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Return Result Parameter Name** | **Parameter Type** | **Parameter Description** | **Required** | **Remarks** |
| -------------------------------- | ------------------ | ------------------------- | ------------ | ----------- |
| code                             | string             | Return code               | Yes          |             |
| message                          | string             | Return code description   | Yes          |             |

The list of request return codes is as follows:

| **code** | **message** |
| -------- | ----------- |
| 1100     | Success     |
| 1101     | QPS Exceeded|
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

Successful return example:
```json
{
  "code": 1100,
  "message": "Success"
}
```

Error return example 1:
```json
{
  "code": 1902,
  "content": {},
  "message": "Accesskey verification failed, please confirm if the Accesskey is correct"
}
```

Error return example 2:
```json
{
  "code": 1902,
  "message": "The feedback record does not exist",
  "content": {
    "requestId": "e03c7fcfa38358cb12d9a49f0495f7b8"
  }
}
```