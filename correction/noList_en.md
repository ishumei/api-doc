---
title: Tianjing General Error Correction Interface Documentation | Shumei
sidebar_label: Tianjing General Error Correction Interface Documentation

hide_title: true
description: 
keywords:
- Error Correction Documentation
---

# Shumei Tianjing General Error Correction API Interface Documentation

---

Copyright, All Rights Reserved

---

## Explanation of Terms Related to the Document

**Required Parameters**: Parameters that must be passed when using Shumei interface services, otherwise the interface service will not function properly;

**Strongly Recommended**: Parameters that are strongly recommended to be passed when using Shumei interface services, which can significantly improve recognition accuracy;

**Optional**: Parameters that are suggested to be passed when using Shumei interface services, which can improve recognition accuracy in specific scenarios.

## Customer Configuration Information

Request Header parameters are as follows:

| **Parameter Name** | **Type** | **Required** | **Description**                        |
| ------------------ | -------- | ------------ | -------------------------------------- |
| X-Accesskey        | string   | Required     | Used for permission authentication, provided by Shumei when the account service is activated |

## General Error Correction Interface Definition

**Interface Description**

This interface is used to provide error correction functions for all Tianjing services, supporting error correction for request data within the last 2 days.

**The data corrected by this interface does not enter the list**

**Request URL**: https://api-web.fengkongcloud.com/api/feedback/normal/add

**Character Encoding**: UTF-8

**Request Method**: POST

**Suggested Timeout Duration**: 1s

**Request Parameters**

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Request Parameter Name** | **Type** | **Parameter Description**                                      | **Passing Instructions** | **Remarks**                                                                                       |
| -------------------------- | -------- | -------------------------------------------------------------- | ------------------------ | ---------------------------------------------------------------------------------------------- |
| requestId                  | string   | Unique identifier for the callback request, corresponding to the requestId in the Shumei service return result | Required                |                                                                                                |
| serviceId                  | string   | The Tianjing service it belongs to                              | Required                |                                                                                                |
| type                       | string   | Error correction type                                           | Required                | Optional values: error: false positive miss: false negative                                     |
| riskType                   | int      | Content risk type, refer to the table below                     | Optional                | False positive: not passed, default handled as "normal" False negative: mandatory provided, default handled as "blacklist", some services without "blacklist" are handled as "custom" |
| timestamp                  | int      | Default can correct data within 2 days, if correction time is more than 2 days, this parameter must be passed | Optional                | The time the record occurred, 13-digit timestamp                                                |
| account                    | string   | Account calling the interface                                   | Optional                | e.g., test@ishumei.com                                                                           |
| appId                      | array    | Correcting for specified applications, up to 3                  | Optional                | e.g., ["default","test"]                                                                         |
| channel                    | array    | Correcting for specified channels, up to 3                      | Optional                |                                                                                                |
| remark                     | string   | Customer error correction remarks, used for backend display     | Optional                |                                                                                                |

Among them, the selectable content for riskType refers to the detailed documentation of other services (the selected risk type must correspond to the service)

The values for serviceId are as follows:

| **Service Identifier**     | **Service Name**       |
| -------------------------- | ---------------------- |
| POST_TEXT                  | Text                   |
| POST_IMG                   | Image                  |
| POST_AUDIOCLIPS            | Audio File Clips       |
| POST_AUDIOSTREAMCLIPS      | Audio Stream Clips     |
| POST_VIDEO_IMG             | Video File (Frame Capture) |
| POST_VIDEO_AUDIO           | Video File (Audio Clips) |
| POST_VIDEOSTREAM_IMG       | Video Stream (Frame Capture) |
| POST_VIDEOSTREAM_AUDIO     | Video Stream (Audio Clips) |

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
| 1101     | QPS Exceeded |
| 1902     | xxxx        |

**Example**

Request example:
```json
{
  "riskType": 100,
  "type": "miss",
  "serviceId": "POST_TEXT",
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
  "message": "Accesskey verification failed, please confirm whether the Accesskey is correct"
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