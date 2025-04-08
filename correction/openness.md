---
title: 对外开放接口手册 | 数美
sidebar_label: 对外开放接口手册

hide_title: true
description:
keywords:
  - 纠错文档
---

# 数美对外开放接口使用手册

---

版权所有，翻版必究

---

## 1. 文档目标

提供数美对外开放接口的说明

## 2. 接口说明

- 接口访问凭证 accessKey 是每次调用接口必带参数，要求放到 Request-Body 中
- Request-Body 中必须是标准的 Json 格式
- 请求及返回结果都使用 UTF-8 字符集进行编码

### 2.1. 查询账户余额

**请求 URL**
https://api-web-bj.fengkongcloud.com/saas/balance/v1

**请求方法**
POST

**输入参数**

| 字段      | 类型   | 说明                     | 是否必须 |
| --------- | ------ | ------------------------ | -------- |
| accessKey | string | 用于权限认证，由数美提供 | 是       |

**输入示例**

```json
{
  "accessKey": "xxxxx"
}
```

**输出参数**

| 字段    | 类型   | 说明                        | 是否必须 |
| ------- | ------ | --------------------------- | -------- |
| code    | int    | 状态码，1100 成功，其他失败 | 是       |
| message | string | 提示语                      | 是       |
| content | object | 数据对象                    | 是       |

content 中的子参数：

| 字段    | 类型  | 说明           | 是否必须 |
| ------- | ----- | -------------- | -------- |
| balance | float | 余额，单位：元 | 是       |

**输出示例**

```json
{
  "code": 1100,
  "message": "成功",
  "content": {
    "balance": 1357.65
  }
}
```

### 2.2. 查询账单明细

**请求 URL**
https://api-operation-bj.fengkongcloud.com/api/bill/get

**请求方法**
POST

**输入参数**

说明: 对外api使用Accesskey（参数key为X-Accesskey）方式验证，每次请求需在HTTP请求头Header中携带此参数。

请求Header参数如下:

| 参数名称    | 类型   | 是否必选 | 说明                                  |
| ----------- | ------ | -------- | ------------------------------------- |
| X-Accesskey | string | 是       | 用于权限认证,开通账号服务时由数美提供 |

请求其他参数放在HTTP Body中,具体参数如下:

| 参数名称     | 类型   | 是否必选 | 说明              |
| ------------ | ------ | -------- | ----------------- |
| organization | string | 是       | 公司标识          |
| date         | string | 是       | 时间 示例:2023-10 |



**输入示例**

```json
{
  "organization":"xxxxx",
  "date":"2023-10"
}
```

**输出参数**

| 参数名称 | 类型   | 说明                                                         |
| -------- | ------ | ------------------------------------------------------------ |
| code     | int    | 返回码<br />1100：成功<br />1902: QPS超限<br />1902: 参数不合法<br />1903: 服务异常 |
| message  | string | 返回码详情描述                                               |
| content  | object    | 账单数据列表                                                 |



content 字段如下所示:

| 参数名称         | 类型   | 说明                          |
| ---------------- | ------ | ----------------------------- |
| name             | string | 公司简称                      |
| fullName         | string | 公司全称                      |
| totalPrice       | float  | 总金额   精度为小数点后2位    |
| writeOffPrice    | float  | 核销金额   精度为小数点后2位  |
| couponPrice      | float  | 优惠券金额  精度为小数点后2位 |
| cashPrice        | float  | 现金金额  精度为小数点后2位   |
| monthlyStatement | object    | 月账单                        |

monthlyStatement 是一个对象，key-value结构，key是产品标识，value中的字段如下

| 参数名称        | 类型     | 说明                                                         |
| --------------- |--------| ------------------------------------------------------------ |
| requestCount    | object | 请求量<br />请求量为用户请求api的量级，一次请求记1。音频流的requestCount为截取的音频片段数量。 |
| requestDuration | object | 请求音频时长<br />请求音频时长为客户请求的音频类数据的总时长 |
| bill            | array  | 月账单明细                                                   |

requestCount字段如下所示

| 参数名称 | 类型   | 说明   |
| -------- | ------ | ------ |
| count    | int    | 请求量 |
| unit     | string | 单位   |

requestDuration字段如下所示

| 参数名称 | 类型   | 说明     |
| -------- | ------ | -------- |
| duration | int    | 请求时长 |
| unit     | string | 单位     |

bill 字段如下所示

| 参数名称       | 类型     | 说明                                                         |
| -------------- |--------| ------------------------------------------------------------ |
| date           | string | 日期:年-月                                                   |
| product        | string | 产品                                                         |
| appName        | string | 应用名称                                                     |
| functionName   | string | 功能                                                         |
| unitPrice      | float  | 单价  精度为小数点后6位                                      |
| priceUnit      | string | 计费单位                                                     |
| count          | float  | 调用量  精度为小数点后4位<br />调用量为调用模型的次数，是真实的计费量，和请求量有区别。例如图片会按照截帧图计费，一张图如果被截了2张截帧图，请求量计1，调用量计2。 |
| unit           | string | 调用量单位                                                   |
| price          | float  | 总金额  精度为小数点后2位                                    |
| dailyStatement | array  | 日账单                                                       |



dailyStatement字段如下所示

| 参数名称     | 类型   | 说明                        |
| ------------ | ------ | --------------------------- |
| date         | string | 日期:年-月-日               |
| product      | string | 产品                        |
| appName      | string | 应用名称                    |
| functionName | string | 功能                        |
| unitPrice    | float  | 单价  精度为小数点后6位     |
| priceUnit    | string | 计费单位                    |
| count        | float  | 调用量  精度为小数点后4位   |
| unit         | string | 调用量单位                  |
| price        | float  | 计费金额  精度为小数点后2位 |



**输出示例**

```json
{
    "code": 1100,
    "message": "success",
    "content": {
        "name": "xxxx",
        "fullName": "xxxxx",
        "totalPrice": 559.35,
        "writeOffPrice": 0,
        "couponPrice": 0,
        "cashPrice": 559.35,
        "monthlyStatement": {
            "POST_AUDIOSTREAM_EN": {
                "requestCount": {
                    "count": 5409487763,
                    "unit": "次"
                },
                "requestDuration": {
                    "duration": 5409487763,
                    "unit": "秒"
                },
                "bill": [
                    {
                        "date": "2023-12",
                        "product": "智能语义-智能音频流识别（英语）",
                        "appName": "默认应用",
                        "functionName": "娇喘",
                        "unitPrice": 0.01,
                        "priceUnit": "元/小时",
                        "count": 49339.3514,
                        "unit": "小时",
                        "price": 493.39,
                        "dailyStatement": [
                            {
                                "date": "2023-12-31",
                                "product": "智能语义-智能音频流识别（英语）",
                                "appName": "默认应用",
                                "functionName": "娇喘",
                                "unitPrice": 0.01,
                                "priceUnit": "元/小时",
                                "count": 49339.3514,
                                "unit": "小时",
                                "price": 493.39
                            }
                        ]
                    }
                ]
            },
            "POST_IMG": {
                "requestCount": {
                    "count": 157690,
                    "unit": "次"
                },
                "requestDuration": {
                    "duration": 0,
                    "unit": "秒"
                },
                "bill": [
                    {
                        "date": "2023-12",
                        "product": "智能视觉-智能图片识别",
                        "appName": "默认应用",
                        "functionName": "图片文字违规识别",
                        "unitPrice": 1.5,
                        "priceUnit": "元/万张",
                        "count": 15.7691,
                        "unit": "万张",
                        "price": 23.65,
                        "dailyStatement": [
                            {
                                "date": "2023-12-31",
                                "product": "智能视觉-智能图片识别",
                                "appName": "默认应用",
                                "functionName": "图片文字违规识别",
                                "unitPrice": 1.5,
                                "priceUnit": "元/万张",
                                "count": 15.7691,
                                "unit": "万张",
                                "price": 23.65
                            }
                        ]
                    }
                ]
            },
            "POST_VIDEO": {
                "requestCount": {
                    "count": 282049,
                    "unit": "次"
                },
                "requestDuration": {
                    "duration": 0,
                    "unit": "秒"
                },
                "bill": [
                    {
                        "date": "2023-12",
                        "product": "天净-智能视频文件识别",
                        "appName": "默认应用",
                        "functionName": "截帧图片色情&性感",
                        "unitPrice": 1.5,
                        "priceUnit": "元/万次",
                        "count": 28.2049,
                        "unit": "万次",
                        "price": 42.31,
                        "dailyStatement": [
                            {
                                "date": "2023-12-31",
                                "product": "天净-智能视频文件识别",
                                "appName": "默认应用",
                                "functionName": "截帧图片色情&性感",
                                "unitPrice": 1.5,
                                "priceUnit": "元/万次",
                                "count": 28.2049,
                                "unit": "万次",
                                "price": 42.31
                            }
                        ]
                    }
                ]
            }
        }
    }
}
```



### 2.3. 名单相关接口

数美内容识别服务支持自定义名单，通过名单结果查看、修改、增加、删减名单内容

> 建议 QPS < 20

#### 2.3.1. 名单列表

**请求 URL**
https://webapi.fengkongcloud.com/saas/listService/list/v1

**请求方法**
POST

**输入参数**

| 字段       | 类型         | 说明                                                  | 是否必须 |
| ---------- | ------------ | ----------------------------------------------------- | -------- |
| accessKey  | string       | 用于权限认证，由数美提供                              | 是       |
| type       | int          | 自定义类型：1，天网内置类型：4，文本、图片内置类型：5 | 是       |
| serviceId  | string       | 服务标识，取值见附录 3.5                              | 是       |
| checkItems | string_array | 匹配字段，详情见附录 3.3                              | 否       |
| riskLevel  | string       | 处置建议，取值见附录 3.6                              | 否       |
| offset     | int          | 偏移量，非负整数，默认为 0                            | 否       |
| count      | int          | 条目数，不大于 100 的正整数不传 count，默认值是：10   | 否       |

**输入示例**

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

**输出参数**

| 字段       | 类型         | 说明     | 是否必须 |
| ---------- | ------------ | -------- | -------- |
| totalCount | int          | 总条数   | 是       |
| contents   | object_array | 事件记录 | 是       |

contents 中的子参数：

| 字段        | 类型   | 说明                         | 是否必须 |
| ----------- | ------ | ---------------------------- | -------- |
| id          | string | 名单编号                     | 是       |
| listId      | String | 名单编号与 id 相同，兼容写法 | 是       |
| name        | string | 名单名字                     | 是       |
| owner       | string | 名单人                       | 是       |
| description | string | 描述                         | 是       |
| createTime  | int    | 名单创建时间，毫秒时间       | 是       |
| modifyTime  | int    | 名单修改时间                 | 是       |
| status      | int    | 启用状态                     | 是       |
| config      | object | 配置内容                     | 是       |
| priority    | int    | 优先级                       | 否       |
| topLevel    | int    | 是否置顶                     | 否       |
| itemCount   | Int    | 名单内敏感词个数             | 否       |

config 中的子参数：

| 字段          | 类型         | 说明                                               | 是否必须 |
| ------------- | ------------ | -------------------------------------------------- | -------- |
| action        | string       | 处理方法，取值见 附录 3.7                          | 是       |
| checkItems    | string_array | 匹配字段，取值见附录 3.3                           | 是       |
| operation     | string       | 匹配方式，取值见附录 3.8                           | 是       |
| segmentStatus | string       | 切词方式，取值：<br/>"0"：默认切词<br/>"1"：空格切词 | 否       |
| riskType      | int          | 风险原因，见附录 3.4                               | 否       |
| appId         | string       | 生效应用                                           | 否       |

**输出示例**

```json
{
  "contents": [
    {
      "actionCN": "拒绝",
      "appCN": "全部",
      "channelCN": "全部",
      "channelEventCN": "全部",
      "checkItemsCN": ["文本内容", "昵称"],
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
      "eventCN": "全部",
      "itemCount": 0,
      "listId": "0da5ad4f49c350ccdd79b94bf00d8a98",
      "modifyTimeCN": "06-28 15:42:30",
      "name": "更新后测试黑名单 3",
      "operationCN": "原文匹配",
      "owner": "0",
      "priority": 0,
      "riskTypeCN": "广告",
      "risklabelCN": "",
      "startTime": 315504000000,
      "status": 1,
      "topLevel": 0,
      "type": 1
    },
    {
      "actionCN": "拒绝",
      "appCN": "全部",
      "channelCN": "全部",
      "channelEventCN": "全部",
      "checkItemsCN": ["文本内容", "昵称"],
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
      "eventCN": "全部",
      "itemCount": 0,
      "listId": "ba231dc0759f599fdffce5464e344632",
      "modifyTimeCN": "06-28 15:33:13",
      "name": "更新后文本黑名单 2",
      "operationCN": "相等匹配",
      "owner": "0",
      "priority": 0,
      "riskTypeCN": "涉政",
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

#### 2.3.2. 新增名单

**请求 URL**
https://webapi.fengkongcloud.com/saas/listService/add/v1

**请求方法**
POST

**输入参数**

| 字段        | 类型   | 说明                      | 是否必须 |
| ----------- | ------ | ------------------------- | -------- |
| accessKey   | string | 用于权限认证，由数美提供  | 是       |
| listId      | string | 名单 Id，MD5 值，确保唯一 | 是       |
| name        | string | 名单名字，注意不能重复    | 是       |
| serviceId   | string | 服务标识，取值见附录 3.5  | 是       |
| description | string | 描述                      | 是       |
| type        | int    | 自定义名单：1             | 是       |
| config      | object | 配置内容                  | 是       |

config 中的子参数：

| 字段          | 类型         | 说明                                               | 是否必须 |
| ------------- | ------------ | -------------------------------------------------- | -------- |
| action        | string       | 处理方法，取值见附录 3.7                           | 是       |
| checkItems    | object_array | 匹配字段，取值见附录 3.3                           | 是       |
| operation     | string       | 匹配方式，取值见附录 3.8                           | 是       |
| segmentStatus | string       | 切词方式，取值：<br/>"0"：默认切词<br/>"1"：空格切词 | 是       |
| riskType      | int          | 风险原因，取值范围见附录 3.4，                     | 是       |
| appId         | string       | 生效应用                                           | 否       |
| eventId       | string       | 生效事件                                           | 否       |
| filter        | object       | 名单生效的条件                                     | 否       |

filter 中的子参数：

| 字段    | 类型   | 说明                                   | 是否必须 |
| ------- | ------ | -------------------------------------- | -------- |
| channel | string | 渠道生效范围，多个 \| 分割，如：aa\|bb | 否       |

**输入示例**

```json
{
  "name": "后端测试",
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

**输出参数**

| 字段    | 类型   | 说明                                 | 是否必须 |
| ------- | ------ | ------------------------------------ | -------- |
| code    | int    | 返回码，值为 1100 表示成功，其他失败 | 是       |
| message | string | 详细描述                             | 是       |

**输出示例**

```json
{
  "code": 1100,
  "message": "请求成功"
}
```

#### 2.3.3. 删除名单

**请求 URL**
https://webapi.fengkongcloud.com/saas/listService/delete/v1

**请求方法**
POST

**输入参数**

| 字段      | 类型   | 说明                     | 是否必须 |
| --------- | ------ | ------------------------ | -------- |
| accessKey | string | 用于权限认证，由数美提供 | 是       |
| ids       | array  | 要删除的名单号列表       | 是       |

**输入示例**

```json
{
  "accessKey": "xxxxxxxxxxxxxxx",
  "ids": ["424fd492cc4f6f0432cfb230a6ab7341"]
}
```

**输出参数**

| 字段    | 类型   | 说明                       | 是否必须 |
| ------- | ------ | -------------------------- | -------- |
| code    | int    | 返回码，1100 成功，其他失败 | 是       |
| message | string | 详细描述                   | 是       |

**输出示例**

```json
{
  "code": 1100,
  "message": "请求成功"
}
```

#### 2.3.4. 修改名单

**请求 URL**
https://webapi.fengkongcloud.com/saas/listService/update/v1

**请求方法**
POST

**输入参数**

| 字段        | 类型   | 说明                                      | 是否必须 |
| ----------- | ------ | ----------------------------------------- | -------- |
| accessKey   | string | 用于权限认证，由数美提供                  | 是       |
| listId      | string | 名单 Id，保证之前新增的时候指定的 id 一致 | 是       |
| name        | string | 名单名字                                  | 否       |
| description | string | 描述                                      | 否       |
| config      | object | 配置内容                                  | 否       |
| status      | int    | 启用状态，0：禁用，1：启用                | 否       |

config 中的子参数：

| 字段          | 类型         | 说明                                               | 是否必须 |
| ------------- | ------------ | -------------------------------------------------- | -------- |
| action        | string       | 处理方法，取值见附录 3.7                           | 是       |
| checkItems    | object_array | 匹配字段，取值见附录 3.3                           | 是       |
| operation     | string       | 匹配方式，取值见附录 3.8                           | 是       |
| segmentStatus | string       | 切词方式，取值：<br/>"0"：默认切词<br/>"1"：空格切词 | 是       |
| riskType      | int          | 风险原因，取值见附录 3.4                           | 是       |
| appId         | string       | 生效应用                                           | 否       |
| eventId       | string       | 生效事件                                           | 否       |
| filter        | object       | 名单生效的条件                                     | 否       |

filter 中的子参数：

| 字段    | 类型   | 说明                                   | 是否必须 |
| ------- | ------ | -------------------------------------- | -------- |
| channel | string | 渠道生效范围，多个 \| 分割，如：aa\|bb | 否       |

**输入示例**

```json
{
  "listId": "424fd492cc4f6f0432cfb230a6ab7341",
  "name": "sjx01",
  "status": 1,
  "type": 1,
  "description": "",
  "config": {
    "action": "REJECT",
    "appId": "",
    "checkItems": ["phone"],
    "ignoreStatus": "0",
    "operation": "equal"
  },
  "serviceId": "P_TIAN_WANG",
  "accessKey": "4Ky6AV4hE0pWLeG1bXNw"
}
```

**输出参数**

| 字段    | 类型   | 说明                        | 是否必须 |
| ------- | ------ | --------------------------- | -------- |
| code    | int    | 返回码，1100 成功，其他失败 | 是       |
| message | string | 详细描述                    | 是       |

**输出示例**

```json
{
  "code": 1100,
  "message": "请求成功"
}
```

#### 2.3.5. 名单内容列表

**请求 URL**
https://webapi.fengkongcloud.com/saas/listService/contentList/v1

**请求方法**
POST

**输入参数**

| 字段      | 类型   | 说明                               | 是否必须 |
| --------- | ------ | ---------------------------------- | -------- |
| accessKey | string | 用于权限认证，由数美提供           | 是       |
| listId    | strig  | 名单 md5 的 id                     | 是       |
| offset    | int    | 偏移量，默认值为 0，取值为非负整数 | 是       |
| count     | int    | 条目数，不大于 100 的正整数        | 是       |

**输入示例**

```json
{
  "count": 10,
  "offset": 0,
  "listId": "424fd492cc4f6f0432cfb230a6ab7341",
  "accessKey": "4Ky6AV4hE0pWLeG1bXNw"
}
```

**输出参数**

| 字段       | 类型         | 说明                | 是否必须 |
| ---------- | ------------ | ------------------- | -------- |
| code       | int          | 1100 成功，其他失败 | 是       |
| message    | string       | 提示语              | 是       |
| totalCount | int          | 总条数              | 否       |
| contents   | object_array | 事件记录            | 否       |

contents 中的子参数：

| 字段        | 类型   | 说明                          | 是否必须 |
| ----------- | ------ | ----------------------------- | -------- |
| content     | string | 内容                          | 是       |
| operateTime | string | 操作时间，如：”1544151689453” | 是       |
| remarks     | string | 备注                          | 否       |
| count       | int    | 命中次数                      | 否       |
| operator    | string | 操作人                        | 否       |

**输出示例**

```json
{
  "code": 1100,
  "message": "成功",
  "contents": [
    {
      "content": "dsf",
      "data": "dsf",
      "operateTime": "1575865083959",
      "remarks": "",
      "count": 0,
      "operator": "liu@ishumei.com"
    },
    {
      "content": "dd",
      "data": "dd",
      "operateTime": "1575864414464",
      "remarks": "",
      "count": 0,
      "operator": "liu@ishumei.com"
    }
  ],
  "totalCount": 2
}
```

#### 2.3.6. 新增名单内容

> 建议 QPS：同一名单 < 100；不同名单 < 20

**请求 URL**
https://webapi.fengkongcloud.com/saas/listService/contentAdd/v1

**请求方法**
POST

**输入参数**

| 字段       | 类型         | 说明                                      | 是否必须 |
| ---------- | ------------ | ----------------------------------------- | -------- |
| accessKey  | string       | 用于权限认证，由数美提供                  | 是       |
| listId     | string       | 名单 listId md5 值                        | 是       |
| contents   | object_array | 内容数组                                  | 是       |
| remarks    | object_array | 内容数组，备注信息，默认备注信息接口调用 | 否       |
| operator   | string       | 操作人，用于记录操作日志                  | 否       |
| serviceId  | string       | 服务标识，需与名单一致                    | 是       |
| checkItems | object_array | 匹配字段                                  | 是       |

**输入示例**

```json
{
  "accessKey": "xxx",
  "listId": "9d3bf5b1ab175bcce1dd73423b17b45a",
  "serviceId": "P_TIAN_WANG",
  "contents": ["test"],
  "remarks": ["test"],
  "operator": "XXX",
  "batchRemarks": 1
}
```

**输出参数**

| 字段    | 类型   | 说明                       | 是否必须 |
| ------- | ------ | -------------------------- | -------- |
| code    | int    | 返回码，1100 成功，其他失败 | 是       |
| message | string | 详细描述                   | 是       |

**输出示例**

```json
{
  "code": 1100,
  "message": "请求成功"
}
```

#### 2.3.7. 删除名单内容

> 建议 QPS：同一名单 < 100；不同名单 < 20

**请求 URL**
https://webapi.fengkongcloud.com/saas/listService/contentDelete/v1

**请求方法**
POST

**输入参数**

| 字段      | 类型         | 说明                         | 是否必须 |
| --------- | ------------ | ---------------------------- | -------- |
| accessKey | string       | 用于权限认证，由数美提供     | 是       |
| listId    | string       | 名单 listId md5 值           | 是       |
| contents  | object_array | 内容数组，非图片服务         | 是       |
| operator  | string       | 操作人，用于记录操作日志     | 否       |
| serviceId | string       | 服务标识，取值需要与名单一致 | 是       |

**输入示例**

```json
{
  "listId": "bc426e21a9f55bab787789c2d2a131f3",
  "accessKey": "4Ky6AV4hE0pWLeG1bXNw",
  "serviceId": "POST_TEXT",
  "operator": "xxx.@ishumei.com",
  "contents": ["测试"]
}
```

**输出参数**

| 字段    | 类型   | 说明                       | 是否必须 |
| ------- | ------ | -------------------------- | -------- |
| code    | int    | 返回码，1100 成功，其他失败 | 是       |
| message | string | 详细描述                   | 是       |

**输出示例**

```json
{
  "code": 1100,
  "message": "成功"
}
```

#### 2.3.8. 修改名单内容

**请求 URL**
https://webapi.fengkongcloud.com/saas/listService/contentUpdate/v1

**请求方法**
POST

**输入参数**

| 字段       | 类型   | 说明                       | 是否必须 |
| ---------- | ------ | -------------------------- | -------- |
| accessKey  | string | 用于权限认证，由数美提供   | 是       |
| listId     | string | 名单 listId md5 值         | 是       |
| newContent | string | 待更新内容                 | 是       |
| oldContent | string | 修改前内容                 | 是       |
| remark     | string | 备注，默认备注信息接口调用 | 否       |
| operator   | string | 操作人，用于记录操作日志   | 否       |
| serviceId  | string | 服务标识，取值与名单一致   | 是       |

**输入示例**

```json
{
  "accessKey": "4Ky6AV4hE0pWLeG1bXNw",
  "listId": "424fd492cc4f6f0432cfb230a8ab7341",
  "serviceId": "POST_IMG",
  "operator": "xxx.@ishumei.com",
  "oldContent": "123",
  "newContent": "456",
  "remark": ""
}

```

**输出参数**

| 字段    | 类型   | 说明                       | 是否必须 |
| ------- | ------ | -------------------------- | -------- |
| code    | int    | 返回码，1100 成功，其他失败 | 是       |
| message | string | 详细描述                   | 是       |

**输出示例**

```json
{
  "code": 1100,
  "message": "成功"
}

```

#### 2.3.9. 名单内容检索

**请求 URL**
https://webapi.fengkongcloud.com/saas/listService/contentSearch/v1

**请求方法**
POST

**输入参数**

| 字段      | 类型   | 说明                        | 是否必须 |
| --------- | ------ | --------------------------- | -------- |
| accessKey | string | 用于权限认证，由数美提供    | 是       |
| count     | int    | 条目数，不大于 100 的正整数 | 是       |
| offset    | int    | 偏移量，非负整数，默认为 0  | 是       |
| serviceId | string | 服务标识，取值与名单一致    | 是       |
| appId     | string | 生效应用                    | 否       |
| content   | string | 检索敏感词                  | 是       |

**输入示例**

```json
{
  "accessKey": "4Ky6AV4hE0pWLeG1bXNw",
  "serviceId": "POST_IMG",
  "offset": 0,
  "count": 2,
  "content": "test"
}

```

**输出参数**

| 字段       | 类型         | 说明                | 是否必须 |
| ---------- | ------------ | ------------------- | -------- |
| code       | int          | 1100 成功，其他失败 | 是       |
| message    | string       | 提示语              | 是       |
| totalCount | int          | 总条数              | 是       |
| contents   | object_array | 内容列表            | 是       |

contents 中的子参数：

| 字段            | 类型   | 说明        | 是否必须 |
| --------------- | ------ | ----------- | -------- |
| content         | string | 内容        | 是       |
| hitListId       | string | 名单 md5 id | 是       |
| hitConfigName   | string | 名单名称    | 是       |
| hitConfigConfig | object | 名单配置    | 是       |

**输出示例**

```json
{
  "code": 1100,
  "message": "成功",
  "contents": [
    {
      "content": "testtest",
      "hitItemId": null,
      "hitItemOwner": null,
      "hitItemCreateTime": null,
      "hitItemContent": "testtest",
      "hitListId": "585c0dbb2924f53bb90971423c2c577e",
      "action": null,
      "hitConfigName": "ocr 黑名单 11",
      "hitConfigOwner": "",
      "hitConfigConfig": {
        "action": "REJECT",
        "appId": "",
        "channelValid": "1",
        "checkItems": ["text"],
        "classDescription": "",
        "filter": [],
        "ignoreStatus": "0",
        "operation": "equal",
        "riskType": "100"
      },
      "hitConfigDescription": "",
      "hitConfigCreateTime": null,
      "hitConfigModifyTime": 0,
      "hitConfigType": 1,
      "hitConfigStartTime": 0,
      "hitConfigEndTime": 0,
      "hitConfigStatus": 1,
      "isGlobal": 0,
      "sort": 1
    }
  ],
  "totalCount": 2
}

```

## 3. 附录

### 3.1. 事件列表

| 服务 | 中文标识             | 中文解释     |
| ---- | -------------------- | ------------ |
| 天网 | POST_EVENT           | 业务事件     |
| ^    | ACCOUNT_LOGIN        | 登录         |
| ^    | ACCOUNT_REGISTER     | 注册         |
| ^    | ANTI_ROBOT_MARKETING | 羊毛党       |
| ^    | ANTI_ROBOT_SMS       | 短信保护通道 |
| ^    | SVERIFY_CAPTCHA      | 验证码       |
| 天净 | POST_TEXT            | 智能文本     |
| ^    | POST_IMG             | 智能图片     |
| ^    | POST_AUDIO           | 智能音频     |
| ^    | POST_AUDIOSTREAM     | 智能音频流   |
| ^    | POST_VIDEO           | 智能视频     |
| ^    | POST_VIDEOSTREAM     | 智能视频流   |
| ^    | POST_ARTICLE         | 智能网页     |

### 3.2. 名单返回码

| 类别     | 返回码 | 原因                     | 解决方式                                                   |
| -------- | ------ | ------------------------ | ---------------------------------------------------------- |
| 通用     | -1     | 参数错误                 | 检查传入参数，是否有漏传或丢包。                           |
| ^        | -11    | Config                   | 串错误 检查 config 串，是否传入了非法字段                  |
| 添加词条 | -19    | 名单不存在               | 检查 listId 以及名单名称 name                              |
| ^        | -20    | 名单已满                 | 删除部分无用词条                                           |
| ^        | -21    | 添加词条全部重复         | 传入的词条在名单中全部已经存在                             |
| ^        | -22    | 添加词条部分重复         | 传入的词条在名单中部分已经存在（未存在的部分会被成功添加） |
| ^        | -23    | 名单不存在，获取名单失败 | 检查传入的名单 id 是否正确，检查日志中的 SQL 语句是否正常  |
| ^        | -24    | 修改后名单写入数据库失败 | 检查数据库连接检查日志中的语句                             |
| 添加名单 | -25    | 名单已存在               | 更换名单 id 或名单组织号、服务 id、名称三者之一。          |
| ^        | -26    | 配置信息格式错误         | 检查入参格式                                               |
| ^        | -27    | 时间类型错误             | 检查时间类型                                               |
| ^        | -29    | 写入失败                 | 参考-24                                                    |
| 删除词条 | -31    | 传入为空                 | 检查传入的待删除词的数量                                   |
| ^        | -32    | 名单不存在，读取名单失败 | 检查传入名单 id 是否正确，检查 SQL 连接                    |
| ^        | -34    | 写入失败                 | 参考-29                                                    |
| 删除名单 | -35    | 传入为空                 | 检查传入的 id                                              |
| ^        | -39    | 写入失败                 | 参考-34                                                    |
| 修改词条 | -41    | 名单不存在               | 检查传入名单 id 是否正确                                   |
| ^        | -42    | 词条不存在               | 检查词条是否正确                                           |
| ^        | -44    | 写入失败                 | 参考-39                                                    |
| 修改名单 | -49    | 写入失败                 | 参考-44                                                    |
| 获取名单 | -51    | 读取失败                 | 参考-32-2                                                  |
| ^        | -52    | 名单不存在               | 检查传入的 id                                              |
| 检索文本 | -55    | 读取数据库失败           | 参考-51                                                    |
| 查找名单 | -61    | 读取失败                 | 参考-55                                                    |
| 查找词条 | -65    | 名单不存在               | 检查传入的 id                                              |
| ^        | -66    | 名单被破坏               | 检查该 id 对应的数据库 value                               |
| 其他     | -99    | 未知错误                 | -                                                          |

### 3.3. 匹配字段

| 标识           | 中文名称       |
| -------------- | -------------- |
| ip             | IP             |
| tokenId        | 账号           |
| deviceId       | 设备           |
| smid           | 服务端设备标识 |
| phone          | 手机号         |
| text           | 文本内容       |
| nickname       | 昵称           |
| img            | 图片内容       |
| img_md5        | 图片 MD5 值    |
| text_md5       | 文本 MD5 值    |
| origin_md5     | 原始 MD5 值    |
| rejectNames    | 涉政人脸       |
| reviewNames    | 疑似涉政人脸   |
| qr_content     | 二维码识别内容 |
| email          | 邮箱           |
| receiveTokenId | 接收者账号     |
| ipColumn       | IP 段（C 类）  |

### 3.4. 风险类型

> 不同产品之间有差异，具体参考历史记录和页面的可选范围

| 风险类型 | 中文解释     |
| -------- | ------------ |
| 0        | 正常         |
| 100      | 涉政         |
| 110      | 暴恐         |
| 200      | 色情         |
| 210      | 辱骂         |
| 250      | 娇喘         |
| 260      | 一号领导声纹 |
| 270      | 人声属性     |
| 280      | 违禁歌曲     |
| 300      | 广告         |
| 310      | 二维码       |
| 320      | 水印         |
| 340      | 网络诈骗     |
| 400      | 灌水         |
| 500      | 无意义       |
| 510      | 不良场景     |
| 520      | 未成年人     |
| 530      | 人脸         |
| 531      | 人像         |
| 533      | 颜值         |
| 534      | 人脸比对     |
| 535      | 公众人物     |
| 540      | 物品         |
| 541      | 动物         |
| 542      | 植物         |
| 550      | 场景         |
| 560      | 行业违规     |
| 570      | 画面属性     |
| 600      | 违禁         |
| 700      | 其他         |
| 710      | 白名单       |
| 720      | 黑账号       |
| 730      | 黑 IP        |
| 800      | 高危账号     |
| 900      | 自定义       |

### 3.5. serviceId

| 服务             | 标识                 |
| ---------------- | -------------------- |
| 智能文本识别     | POST_TEXT            |
| 智能图片识别     | POST_IMG             |
| 智能视频文件识别 | POST_VIDEO           |
| 智能音频文件识别 | POST_AUDIO           |
| 智能视频流识别   | POST_VIDEOSTREAM     |
| 智能音频流识别   | POST_AUDIOSTREAM     |
| 业务事件         | POST_EVENT           |
| 机器登录识别     | ACCOUNT_LOGIN        |
| 机器登录注册     | ACCOUNT_REGISTER     |
| 羊毛党防刷       | ANTI_ROBOT_MARKETING |
| 短信通道保护     | ANTI_ROBOT_SMS       |
| 智能验证码       | SVERIFY_CAPTCHA      |

### 3.6. 处置建议

| 标识           | 解释       |
| -------------- | ---------- |
| PASS           | 通过       |
| REVIEW         | 审核       |
| REJECT         | 拒绝       |
| VERIFY         | 二次验证   |
| SLIDER_CAPTCHA | 滑动验证码 |
| SELECT_CAPTCHA | 点选验证码 |
| IGNORE         | 忽略       |
| EXCLUDE        | 放行       |

### 3.7. action

| 标识         | 解释     |
| ------------ | -------- |
| PASS         | 通过     |
| REJECT       | 拒绝     |
| REVIEW       | 审核     |
| IGNORE       | 忽略     |
| EXCLUDE      | 放行     |
| REGEX_IGNORE | 正则忽略 |

### 3.8. operation

| 标识       | 解释     |
| ---------- | -------- |
| equal      | 相等匹配 |
| contain    | 原文匹配 |
| word       | 语义匹配 |
| variant    | 变体名单 |
| pinyin     | 同音名单 |
| like       | 相似名单 |
| image_hash | 相似匹配 |
| regex      | 正则匹配 |