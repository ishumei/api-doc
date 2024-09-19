---
title: External API Manual | Shumei
sidebar_label: External API Manual

hide_title: true
description: 
keywords:
- Error Correction Document
---

# Shumei External API Usage Manual

---

All rights reserved, reproduction prohibited

---

## 1. Document Objective

Provide instructions for Shumei's external API

## 2. API Description

- The access credential accessKey is a required parameter for each API call and must be placed in the Request-Body.
- The Request-Body must be in standard JSON format.
- Requests and responses are encoded using the UTF-8 character set.

### 2.1. Query Account Balance

**Request URL**
https://api-web-bj.fengkongcloud.com/saas/balance/v1

**Request Method**
POST

**Input Parameters**

| Field     | Type   | Description                | Required |
| --------- | ------ | -------------------------- | -------- |
| accessKey | string | Used for authentication, provided by Shumei | Yes     |

**Input Example**

```json
{
  "accessKey": "xxxxx"
}
```

**Output Parameters**

| Field   | Type   | Description                | Required |
| ------- | ------ | -------------------------- | -------- |
| code    | int    | Status code, 1100 for success, others for failure | Yes     |
| message | string | Message                    | Yes     |
| content | object | Data object                | Yes     |

Sub-parameters in content:

| Field   | Type  | Description       | Required |
| ------- | ----- | ----------------- | -------- |
| balance | float | Balance, unit: yuan | Yes     |

**Output Example**

```json
{
  "code": 1100,
  "message": "Success",
  "content": {
    "balance": 1357.65
  }
}
```

### 2.2. Query Monthly Bill Details

**Request URL**
https://api-web-bj.fengkongcloud.com/saas/monthBill/v1

**Request Method**
POST

**Input Parameters**

| Field      | Type   | Description                | Required |
| ---------- | ------ | -------------------------- | -------- |
| accessKey  | string | Used for authentication, provided by Shumei | Yes     |
| startMonth | string | Start month, e.g., 2019-10 | Yes     |
| endMonth   | string | End month, e.g., 2019-11   | Yes     |
| action     | string | Default value: list        | No       |

**Input Example**

```json
{
  "action": "list",
  "accessKey": "xxxxxx",
  "startMonth": "2019-09",
  "endMonth": "2019-10"
}
```

**Output Parameters**

| Field     | Type         | Description                | Required |
| --------- | ------------ | -------------------------- | -------- |
| code      | int          | Status code, 1100 for success, others for failure | Yes     |
| message   | string       | Message                    | Yes     |
| sumFee    | float        | Total consumption, unit: yuan | Yes     |
| contents  | object_array | Array                      | Yes     |

Sub-parameters in contents:

| Field       | Type         | Description                | Required |
| ----------- | ------------ | -------------------------- | -------- |
| month       | string       | Month, e.g., 2019-11       | Yes     |
| feeType     | string       | Billing type, query, type, annual, etc. | Yes     |
| fee         | float        | Consumption amount, unit: yuan, two decimal places | Yes     |
| productName | string       | Product name               | Yes     |
| appName     | string       | Application name, if not distinguished, value: - | Yes     |
| feeDetail   | object_array | Billing details            | Yes     |

Sub-parameters in feeDetail:

| Field  | Type   | Description                | Required |
| ------ | ------ | -------------------------- | -------- |
| price  | float  | Unit price, unit: yuan     | Yes     |
| count  | int    | Number of calls            | Yes     |
| type   | string | Function type, returned when billing method type | No       |

**Output Example**

```json
{
  "code": 1100,
  "message": "Success",
  "contents": [
    {
      "month": "2019-10",
      "feeType": "Annual",
      "fee": 0,
      "feeDetail": [
        {
          "price": 0,
          "count": 1
        }
      ],
      "productName": "Tianjing - Intelligent Text Recognition",
      "appName": "-"
    },
    {
      "month": "2019-10",
      "feeType": "Annual",
      "fee": 0,
      "feeDetail": [
        {
          "price": 0,
          "count": 1
        },
        {
          "price": 2,
          "count": 2,
          "type": "Voice Content"
        }
      ],
      "productName": "xxxxxxxx",
      "appName": "-"
    }
  ],
  "sumFee": 1.14
}
```

### 2.3. Query Daily Bill Details

**Request URL**
https://api-web-bj.fengkongcloud.com/saas/dayBill/v1

**Request Method**
POST

**Input Parameters**

| Field      | Type   | Description                | Required |
| ---------  | ------ | -------------------------- | -------- |
| accessKey  | string | Used for authentication, provided by Shumei | Yes     |
| startDay   | string | Start date, e.g., 2019-10-01 | Yes     |
| endDay     | string | End date, e.g., 2019-11-01 | Yes     |
| action     | string | Default value: list        | No       |
| appId      | string | Used to filter applications, if not provided, show all | No       |

**Input Example**

```json
{
  "action": "list",
  "accessKey": "xxxxxx",
  "startDay": "2019-09-01",
  "endDay": "2019-10-02",
  "appId": "default"
}
```

**Output Parameters**

| Field     | Type         | Description                | Required |
| --------- | ------------ | -------------------------- | -------- |
| code      | int          | Status code, 1100 for success, others for failure | Yes     |
| message   | string       | Message                    | Yes     |
| sumFee    | float        | Total consumption, unit: yuan | Yes     |
| contents  | object_array | Array                      | Yes     |

Sub-parameters in contents:

| Field       | Type         | Description                | Required |
| ----------- | ------------ | -------------------------- | -------- |
| day         | string       | Date, e.g., 2019-11-01     | Yes     |
| fee         | float        | Consumption amount, unit: yuan, two decimal places | Yes     |
| feeType     | string       | Billing type, query, type, annual, etc. | Yes     |
| productName | string       | Product name               | Yes     |
| appName     | string       | Application name, if not distinguished, value: - | Yes     |
| feeDetail   | object_array | Billing details            | Yes     |

Sub-parameters in feeDetail:

| Field  | Type   | Description                | Required |
| ------ | ------ | -------------------------- | -------- |
| price  | float  | Unit price, unit: yuan     | Yes     |
| count  | int    | Number of calls            | Yes     |
| type   | string | Function type, returned when billing method type | No       |

**Output Example**

```json
{
  "code": 1100,
  "message": "Success",
  "contents": [
    {
      "day": "2019-11-01",
      "feeType": "Annual",
      "fee": 0,
      "feeDetail": [
        {
          "price": 0,
          "count": 1
        }
      ],
      "productName": "Tianjing - Intelligent Text Recognition",
      "appName": "-"
    },
    {
      "day": "2019-11-01",
      "feeType": "Annual",
      "fee": 0,
      "feeDetail": [
        {
          "price": 0,
          "count": 2
        }
      ],
      "productName": "Tianwang - Registration and Login Protection",
      "appName": "-"
    },
    {
      "day": "2019-11-01",
      "feeType": "Type",
      "fee": 1.94,
      "feeDetail": [
        {
          "count": 3,
          "type": "Moaning",
          "price": 1
        },
        {
          "price": 2,
          "count": 3,
          "type": "Voice Content"
        },
        {
          "price": 0,
          "count": 3,
          "type": "Voice Tag"
        }
      ],
      "productName": "Tianjing - Intelligent Audio Recognition",
      "appName": "-"
    },
    {
      "day": "2019-11-01",
      "feeType": "Query",
      "fee": 1234,
      "feeDetail": [
        {
          "price": 123,
          "count": 2
        }
      ],
      "productName": "Tianjing - Intelligent Image Recognition",
      "appName": "-"
    }
  ],
  "sumFee": 57
}
```

### 2.4. List Related APIs

Shumei content recognition service supports custom lists, allowing for viewing, modifying, adding, and deleting list content.

> Recommended QPS < 20

#### 2.4.1. List of Lists

**Request URL**
https://webapi.fengkongcloud.com/saas/listService/list/v1

**Request Method**
POST

**Input Parameters**

| Field       | Type         | Description                | Required |
| ----------- | ------------ | -------------------------- | -------- |
| accessKey   | string       | Used for authentication, provided by Shumei | Yes     |
| type        | int          | Custom type: 1, Tianwang built-in type: 4, text, image built-in type: 5 | Yes     |
| serviceId   | string       | Service identifier, see Appendix 3.5 | Yes     |
| checkItems  | string_array | Matching fields, see Appendix 3.3 | No       |
| riskLevel   | string       | Handling suggestion, see Appendix 3.6 | No       |
| offset      | int          | Offset, non-negative integer, default is 0 | No       |
| count       | int          | Number of items, positive integer not greater than 100, default is 10 if not provided | No       |

**Input Example**

```json
{
  "accessKey": "4Ky6AV4hE0pWLeG1bXNw",
  "serviceId": "POST_TEXT",
  "count": 20,
  "offset": 0,
  "type": 1,
  "riskLevel": "REJECT",
  "checkItems": ["text", "nickname"]
}
```

**Output Parameters**

| Field       | Type         | Description                | Required |
| ----------- | ------------ | -------------------------- | -------- |
| totalCount  | int          | Total number of items      | Yes     |
| contents    | object_array | Event records              | Yes     |

Sub-parameters in contents:

| Field        | Type   | Description                | Required |
| ------------ | ------ | -------------------------- | -------- |
| id           | string | List ID                    | Yes     |
| listId       | String | List ID, same as id, compatible writing | Yes     |
| name         | string | List name                  | Yes     |
| owner        | string | List owner                 | Yes     |
| description  | string | Description                | Yes     |
| createTime   | int    | List creation time, in milliseconds | Yes     |
| modifyTime   | int    | List modification time     | Yes     |
| status       | int    | Enable status              | Yes     |
| config       | object | Configuration content      | Yes     |
| priority     | int    | Priority                   | No       |
| topLevel     | int    | Whether to pin to top      | No       |
| itemCount    | Int    | Number of sensitive words in the list | No       |

Sub-parameters in config:

| Field          | Type         | Description                | Required |
| -------------- | ------------ | -------------------------- | -------- |
| action         | string       | Handling method, see Appendix 3.7 | Yes     |
| checkItems     | string_array | Matching fields, see Appendix 3.3 | Yes     |
| operation      | string       | Matching method, see Appendix 3.8 | Yes     |
| segmentStatus  | string       | Word segmentation method, values: <br/>"0": default segmentation<br/>"1": space segmentation | No       |
| riskType       | int          | Risk reason, see Appendix 3.4 | No       |
| appId          | string       | Effective application       | No       |

**Output Example**

```json
{
  "contents": [
    {
      "actionCN": "Reject",
      "appCN": "All",
      "channelCN": "All",
      "channelEventCN": "All",
      "checkItemsCN": ["Text Content", "Nickname"],
      "config": {
        "action": "REJECT",
        "appId": "",
        "checkItems": ["text", "nickname"],
        "operation": "contain",
        "riskType": 300
      },
      "createTimeCN": "06-28 15:35:59",
      "description": "",
      "endTime": 315504000000,
      "eventCN": "All",
      "itemCount": 0,
      "listId": "0da5ad4f49c350ccdd79b94bf00d8a98",
      "modifyTimeCN": "06-28 15:42:30",
      "name": "Updated Test Blacklist 3",
      "operationCN": "Original Text Match",
      "owner": "0",
      "priority": 0,
      "riskTypeCN": "Advertisement",
      "risklabelCN": "",
      "startTime": 315504000000,
      "status": 1,
      "topLevel": 0,
      "type": 1
    },
    {
      "actionCN": "Reject",
      "appCN": "All",
      "channelCN": "All",
      "channelEventCN": "All",
      "checkItemsCN": ["Text Content", "Nickname"],
      "config": {
        "action": "REJECT",
        "appId": "",
        "checkItems": ["text", "nickname"],
        "operation": "equal",
        "riskType": 100,
        "segmentStatus": "0"
      },
      "createTimeCN": "06-28 15:33:13",
      "description": "",
      "endTime": 315504000000,
      "eventCN": "All",
      "itemCount": 0,
      "listId": "ba231dc0759f599fdffce5464e344632",
      "modifyTimeCN": "06-28 15:33:13",
      "name": "Updated Text Blacklist 2",
      "operationCN": "Equal Match",
      "owner": "0",
      "priority": 0,
      "riskTypeCN": "Political",
      "risklabelCN": "",
      "startTime": 315504000000,
      "status": 1,
      "topLevel": 0,
      "type": 1
    }
  ],
  "totalCount": 133
}
```

#### 2.4.2. Add List

**Request URL**
https://webapi.fengkongcloud.com/saas/listService/add/v1

**Request Method**
POST

**Input Parameters**

| Field        | Type   | Description                | Required |
| ------------ | ------ | -------------------------- | -------- |
| accessKey    | string | Used for authentication, provided by Shumei | Yes     |
| listId       | string | List ID, MD5 value, ensure uniqueness | Yes     |
| name         | string | List name, note that it cannot be duplicated | Yes     |
| serviceId    | string | Service identifier, see Appendix 3.5 | Yes     |
| description  | string | Description                | Yes     |
| type         | int    | Custom list: 1             | Yes     |
| config       | object | Configuration content      | Yes     |

Sub-parameters in config:

| Field          | Type         | Description                | Required |
| -------------- | ------------ | -------------------------- | -------- |
| action         | string       | Handling method, see Appendix 3.7 | Yes     |
| checkItems     | object_array | Matching fields, see Appendix 3.3 | Yes     |
| operation      | string       | Matching method, see Appendix 3.8 | Yes     |
| segmentStatus  | string       | Word segmentation method, values: <br/>"0": default segmentation<br/>"1": space segmentation | Yes     |
| riskType       | int          | Risk reason, see Appendix 3.4 | Yes     |
| appId          | string       | Effective application       | No       |
| eventId        | string       | Effective event             | No       |
| filter         | object       | Conditions for list effectiveness | No       |

Sub-parameters in filter:

| Field    | Type   | Description                | Required |
| -------- | ------ | -------------------------- | -------- |
| channel  | string | Channel effectiveness range, multiple separated by \|, e.g., aa\|bb | No       |

**Input Example**

```json
{
  "name": "Backend Test",
  "status": 1,
  "listId": "2b2bd7fab89ce92688417ba18ba5d458",
  "priority": 0,
  "config": {
    "checkItems": ["text"],
    "action": "PASS",
    "appId": "",
    "riskType": 710,
    "operation": "equal"
  },
  "contents": "",
  "contentRemarks": [],
  "subType": "text",
  "organization": "RlokQwRlVjUrTUlkIqOg",
  "product": "POST_AUDIO",
  "type": 1
}
```

**Output Parameters**

| Field    | Type   | Description                | Required |
| -------- | ------ | -------------------------- | -------- |
| code     | int    | Return code, 1100 for success, others for failure | Yes     |
| message  | string | Detailed description       | Yes     |

**Output