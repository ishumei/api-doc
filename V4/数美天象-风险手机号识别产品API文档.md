# 数美天象-风险手机号产品API接口说明文档

---

***版权所有 翻版必究***

---

* [天象风险识别](#riskDiscernInterface)
  + [调用时机](#requestParameter)
  + [具体接口](#requestInterface)
    - [请求URL](#requestUrl)
    - [字符编码格式](#requestEncode)
    - [请求方法](#requestMethod)
    - [建议超时时长](#requestTimeout)
    - [请求参数](#requestParameters)
  + [返回结果](#response)
  + [示例](#example)
    - [请求示例](#requestExample)
    - [返回示例](#responseExample)

# <span id = "riskDiscernInterface">天象风险识别</span>

## <span id = "requestParameter">调用时机</span>

在需要进行风险识别时调用本接口，例如：

1. 新用户注册后，进行风险画像查询，用于新人红包、注册礼物等的差异化投放；
2. 存量用户登录后，进行风险画像查询，用于日常活动运营的差异化投放；
3. 活动奖励下发前，进行风险画像查询，例如随机红包、抽奖等概率性活动；
4. 每日的例行查询，进行画像库或风控引擎的数据更新。

## <span id = "requestInterface">具体接口</span>

### <span id = "requestUrl">请求URL：</span>


| 集群   | URL                                                       | 支持产品列表 |
| -------- | ----------------------------------------------------------- | -------------- |
| 北京集群   | `http://api-tianxiang-bj.fengkongcloud.com/tianxiang/v4`  | 天象风险识别 |
| 新加坡集群 | `http://api-tianxiang-xjp.fengkongcloud.com/tianxiang/v4` | 天象风险识别 |
|法兰克福集群|`http://api-tianxiang-eur.fengkongcloud.com/tianxiang/v4`|天象风险识别 |
|弗吉尼亚集群|`http://api-tianxiang-fjny.fengkongcloud.com/tianxiang/v4`|天象风险识别 |

### <span id = "requestMethod">请求方法：</span>

`POST`

### <span id = "requestEncode">字符编码：</span>

`UTF-8`

### <span id = "requestTimeout">建议超时时间：</span>

1s

### <span id = "requestParameters">请求参数：</span>

放在HTTP Body中，采用Json格式，具体参数如下：


| **请求参数名** | **类型**    | **参数说明**                                                                                         | **是否必传** | **规范**                                        |
| ---------------- | ------------- | ------------------------------------------------------------------------------------------------------ | -------------- | ------------------------------------------------- |
| accessKey      | string      | 接口认证密钥<br>用于权限认证，开通账号服务时由数美提供或使用开通邮箱登录数美后台右上角相关文档处查看 | 必传参数     | 数美分配                                        |
| data           | json_object | 请求的数据内容                                                                                       | 必传参数     | 请求的数据内容，最长10MB，[详见data参数](#data) |

<span id = "data">其中，data的内容如下：</span>


| **请求参数名** | **类型** | **参数说明**                                                                                                                                    | **是否必传** | **规范**                                                 |
| ---------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- | ---------------------------------------------------------- |
| phone          | string   | 待查询的手机号                                          | 不能为空 | 该信息为客户要检测的手机号                               |

## <span id = "response">返回结果</span>
放在HTTP Body中，采用Json格式，具体参数如下：


| **参数名称**       | **参数类型** | **参数说明**   | **是否必返** | **规范**                                                                                                                                                                                                    |
| -------------------- | -------------- | ---------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| code               | int          | 返回码         | 是           | `1100`：成功<br>`1901`：QPS超限<br>`1902`：参数不合法<br>`1903`：服务失败<br>`1911`：图片下载失败<br>`9101`：无权限操作<br>`3000`：运营商错误<br>除message和requestId之外的字段，只有当code为1100时才会存在 |
| message            | string       | 返回码描述     | 是           | 和code对应：`成功 QPS超限 参数不合法 服务失败 余额不足 无权限操作`                                                                                                                                          |
| requestId          | string       | 请求标识       | 是           | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存                                                                                                                                                      |
| phonePrimaryInfo    | Json_object   | 手机号基础信息 | 否           | 见下面详情内容，仅在phone传入时返回（不需要开通）                                                                                                                                                           |
| phoneRiskLabels    | json_array   | 手机号风险标签 | 否           | 见下面详情内容，仅在phone传入时返回（不需要开通）                                                                                                                                                           |

其中
**1**）phoneRiskLabels的详情内容  

**2**）phoneRiskLabels的详情内容


| ***返回结果参数名*** | ***参数类型*** | ***参数说明***         | ***规范***                     |
| ---------------------- | ---------------- | ------------------------ | -------------------------------- |
| label1               | string         | 一级标签               | 展示手机号风险标签的一级标签。 |
| label2               | string         | 二级标签               | 展示手机号风险标签的二级标签。 |
| label3               | string         | 三级标签               | 展示手机号风险标签的三级标签。 |
| description          | string         | 风险描述               | 展示手机号风险标签的中文描述。 |
| timestamp            | int64          | 最近一次命中策略的时间 | 最近一次命中策略的时间         |
| detail               | json_object    | 证据描述               | 证据细节                       |


## <span id = "example">示例：</span>

### <span id = "requestExample">请求示例：</span>

```json
{
  "accessKey": "xxxxxxxxxxxxxxxxxxxxxxxxx",
  "data": {
    "phone": "ceshiphone"
  }
}
```

### <span id = "syncResponseExample">同步返回示例：</span>

```json
{
  "code": 1100,
  "message": "成功",
  "profileExist": 1,
  "requestId": "7a5445716f0581c2ab1d381a6af4d1b8",
  "phonePrimaryInfo": {
    "phone_city": "鞍山",
    "phone_operator": "移动",
    "phone_province": "辽宁"
  },
  "phoneRiskLabels": [
    {
      "description": "接码平台手机号:接码平台手机号:接码平台手机号",
      "label1": "sms_platform_phone",
      "label2": "sms_platform_phone",
      "label3": "sms_platform_phone",
      "timestamp": null
    },
    {
      "description": "物联网卡手机号:物联网卡手机号:物联网卡手机号",
      "label1": "iot_simcard_phone",
      "label2": "iot_simcard_phone",
      "label3": "iot_simcard_phone",
      "timestamp": null
    },
    {
      "description": "虚拟运营商手机号:虚拟运营商手机号:虚拟运营商手机号",
      "label1": "mvno_simcard_phone",
      "label2": "mvno_simcard_phone",
      "label3": "mvno_simcard_phone",
      "timestamp": null
    },
    {
      "description": "黑产手机号:黑产手机号:黑产手机号",
      "label1": "black_record_phone",
      "label2": "black_record_phone",
      "label3": "black_record_phone",
      "timestamp": null
    }
  ]
}
```
