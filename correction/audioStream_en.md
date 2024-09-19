---
title: Audio Stream (Clip) Error Correction API Documentation
sidebar_label: Audio Stream (Clip) Error Correction API Documentation

hide_title: true
description: 
keywords:
- Error Correction Documentation
---

# Shumei Audio Stream Clip Error Correction API Documentation

---

Copyright, All Rights Reserved

---


## Explanation of Terms Related to the Document

**Required Parameters**: Parameters that must be provided by the user when using the Shumei API service, otherwise the service will not function properly;

**Strongly Recommended**: Parameters that are strongly recommended to be provided by the user when using the Shumei API service, which can significantly improve recognition accuracy;

**Optional**: Parameters that are suggested to be provided by the user when using the Shumei API service, which can improve recognition accuracy in specific scenarios.

## Customer Configuration Information

Request Header parameters are as follows:

| **Parameter Name** | **Type** | **Required** | **Description**                        |
| ------------------ | -------- | ------------ | -------------------------------------- |
| X-Accesskey        | string   | Required     | Used for authentication, provided by Shumei when the account service is activated |

## Audio Stream Clip Error Correction API Definition

**API Description**

This API is used to provide error correction functionality for audio stream clips. It supports error correction for request data from the past 2 days.

**Request URL**: https://api-web.fengkongcloud.com/api/feedback/audiostream/clips/add

**Character Encoding**: UTF-8

**Request Method**: POST

**Suggested Timeout Duration**: 1s

**Request Parameters**

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name** | **Type** | **Description**                                                | **Required** | **Remarks**                                                                                       |
| ------------------ | -------- | -------------------------------------------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| requestId          | string   | Unique identifier for the audio stream clip callback request, corresponding to the requestId in the Shumei service return result | Required     |                                                                                                   |
| type               | string   | Error correction type                                          | Required     | Optional values: error: false positive miss: false negative                                       |
| riskType           | int      | Content risk type, refer to the table below                    | Optional     | False positive: not provided defaults to "normal" False negative: not provided defaults to "blacklist", some services without "blacklist" default to "custom" |
| timestamp          | int      | Default can correct data within 2 days, if correction time is more than 2 days, this parameter must be provided | Optional     | The time the record occurred, 13-digit timestamp                                                  |
| account            | string   | Account calling the API                                        | Optional     | e.g., test@ishumei.com                                                                            |
| remark             | string   | Customer error correction remarks, used for backend display    | Optional     |                                                                                                   |

The list of riskType is as follows:

| **Risk Type** | **Description** |
| ------------- | --------------- |
| 0             | Normal          |
| 100           | Political       |
| 250           | Moaning         |
| 260           | Leader's Voice  |
| 270           | Voice Attribute |
| 280           | Prohibited Song |
| 520           | Minor           |
| 900           | Custom          |

**Return Parameters**

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Return Parameter Name** | **Type** | **Description** | **Required** | **Remarks** |
| ------------------------- | -------- | --------------- | ------------ | ----------- |
| code                      | string   | Return code     | Yes          |             |
| message                   | string   | Return code description | Yes          |             |

The list of code return codes is as follows:

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