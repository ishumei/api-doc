---
title: Callback Interface After Correction | Shenma
sidebar_label: Callback Interface After Correction

hide_title: true
description: 
keywords:
- Correction Document
---

# **Callback Interface After Correction**

---

All rights reserved, reproduction prohibited

---

## **Explanation of Document-Related Terms**

Mandatory parameters: Parameters that must be provided by the interface user when using Shenma interface services, otherwise the interface service will not function properly;

Strongly recommended: Parameters that are strongly recommended to be provided by the interface user when using Shenma interface services, which can significantly improve recognition accuracy;

Optional: Parameters that are suggested to be provided by the interface user when using Shenma interface services, which can improve recognition accuracy in specific scenarios.

## **Customer Configuration Information**

Before use, provide the URL address that needs to be called back to customer service for platform configuration.

## **Definition of Correction Callback Interface**

Interface Description

This interface is used to provide callback functionality after correction, supporting the provision of relevant main information of the correction to the customer.

Request URL: Provided by the customer, can be used after platform configuration

Character Encoding: UTF-8

Request Method: POST

Suggested Timeout Duration: 1s

### Request Parameters

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Parameter Name** | **Type**    | **Mandatory** | **Description**                |
| ------------------ | ----------- | ------------- | ------------------------------ |
| requestId          | string      | Yes           | Request serial number          |
| serviceId          | string      | Yes           | Specific service and corresponding information, see Appendix 1-1 |
| appId              | string      | Yes           | Application                    |
| channel            | string      | Yes           | Channel                        |
| result             | json_object | Yes           | Review log                     |
| feedback           | json_object | Yes           | Correction information         |

The content of result is as follows:

| **Parameter Name** | **Type** | **Mandatory** | **Description**                                    |
| ------------------ | -------- | ------------- | -------------------------------------------------- |
| riskLevel          | string   | Yes           | Machine review result PASS: pass, REJECT: fail, REVIEW: review |
| description        | string   | No            | Risk reason                                        |
| timestamp          | string   | Yes           | Review time                                        |
 

The content of feedback is as follows:

| **Parameter Name** | **Type** | **Mandatory** | **Description**                                                   |
| ------------------ | -------- | ------------- | ------------------------------------------------------------------ |
| content            | string   | Yes           | Correction content: text - text content, image - image address, audio - audio address, video - video address |
| tokenId            | string   | Yes           | User account                                                      |
| feedbackTime       | string   | Yes           | Correction time                                                   |
| caseType           | string   | Yes           | Correction type: error - false positive; miss - false negative    |
| caseLabel          | string   | Yes           | Label result                                                      |

### Return Parameters

Placed in the HTTP Body, in Json format, with specific parameters as follows:

| **Parameter Name** | **Type**   | **Mandatory** | **Description**                                                |
| ------------------ | ---------- | ------------- | -------------------------------------------------------------- |
| code               | int        | Yes           | Return code, 1100 for success, will determine if the request is successful; non-1100 is considered a failure |
| message            | string     | Yes           | |

## Appendix

| **Service Name**           | **Service Identifier**   |
| -------------------------- | ------------------------ |
| POST_TEXT                  | Text Service             |
| POST_IMG                   | Image Service            |
| POST_AUDIOCLIPS            | Audio File (Clips)       |
| POST_AUDIOSTREAMCLIPS      | Audio Stream (Clips)     |
| POST_VIDEO_IMG             | Video File (Frames)      |
| POST_VIDEO_AUDIO           | Video File (Audio Clips) |
| POST_VIDEOSTREAM_IMG       | Video Stream (Frames)    |
| POST_VIDEOSTREAM_AUDIO     | Video Stream (Audio Clips) |