# 数美天象产品API说明——IP风险画像
---
***版权所有 翻版必究***
---

* [IP风险画像](#riskDiscernInterface)
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

# <span id = "riskDiscernInterface">风险IP画像</span>

## <span id = "requestParameter">调用时机</span>

在需要进行风险识别时调用本接口，例如：

1. 新用户注册后，进行风险画像查询，用于新人红包、注册礼物等的差异化投放；
2. 存量用户登录后，进行风险画像查询，用于日常活动运营的差异化投放；
3. 活动奖励下发前，进行风险画像查询，例如随机红包、抽奖等概率性活动；
4. 每日的例行查询，进行画像库或风控引擎的数据更新。

## <span id = "requestInterface">具体接口</span>

### <span id = "requestUrl">请求URL：</span>


| 集群         | URL                                                        | 支持产品列表 |
| -------------- | ------------------------------------------------------------ | -------------- |
| 北京集群     | `http://api-tianxiang-bj.fengkongcloud.com/tianxiang/v4`   | 天象风险识别 |
| 新加坡集群   | `http://api-tianxiang-xjp.fengkongcloud.com/tianxiang/v4`  | 天象风险识别 |
| 法兰克福集群 | `http://api-tianxiang-eur.fengkongcloud.com/tianxiang/v4`  | 天象风险识别 |
| 弗吉尼亚集群 | `http://api-tianxiang-fjny.fengkongcloud.com/tianxiang/v4` | 天象风险识别 |

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
| accessKey      | string      | 接口认证密钥<br/>用于权限认证，开通账号服务时由数美提供或使用开通邮箱登录数美后台右上角相关文档处查看 | 必传参数     | 数美分配                                        |
| data           | json_object | 请求的数据内容 | 必传参数     | 请求的数据内容，最长10MB，[详见data参数](#data) |

<span id = "data">其中，data的内容如下：</span>


| **请求参数名** | **类型** | **参数说明**   | **是否必传** | **规范**                   |
| ---------------- | ---------- | ---------------- | -------------- | ---------------------------- |
| ip | string | 当前业务事件发生时的客户端公网ipv4与ipv6地址 | 必传参数 | 非内网IP |


## <span id = "response">返回结果</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称**     | **参数类型** | **参数说明**   | **是否必返** | **规范**    |
| ------------------ | -------------- | ---------------- | -------------- | --------------
| code             | int          | 返回码         | 是           | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9101`：无权限操作<br/>  |
| message          | string       | 返回码描述     | 是           | 和code对应：`成功 QPS超限 参数不合法 服务失败 余额不足 无权限操作`|
| requestId        | string       | 请求标识       | 是           | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存|
| ipLabels         | json_object  | IP标签 | 否           | 见下面详情内容，仅在IP字段传入时返回。 |

其中ipLabels的详情内容为：

| **返回结果参数名** | **参数类型** | **是否必反** | **规范** |
| ---------------------- | ---------------- | ---------------- | ------------ |
| b_cgn        | int         | 否          | 运营商保留IP    |
| b_cmwap      | int         | 否          | 移动梦网IP     |
| risk_ip      | int            | 否          | 是否为风险IP，风险IP是指通过数美风控策略和模型检测到的风险IP |
| b_secdial    | int            | 否          | 是否为秒拨IP，秒拨IP是指利用宽带频繁拨号上网所获取的IP | 
| b_idc        | int            | 否          | 是否为机房IP，机房IP是指机房中心服务器使用的IP | 
| b_proxy      | int            | 否          | 是否为代理IP，代理IP是指代理网络用户去取得网络信息的中转IP | 
| ip_continent | string         | 否          | IP归属洲       |
| ip_country   | string         | 否          | IP归属国家     |
| ip_province  | string         | 否          | IP归属省份     |
| ip_city      | string         | 否          | IP归属城市     |
| ip_latitude  | float64        | 否          | IP归属纬度        |
| ip_longitude | float64        | 否          | IP归属经度        |
| ip_owner     | string         | 否          | IP运营商归属   |


## <span id = "example">示例：</span>

### <span id = "requestExample">请求示例：</span>

```json
{
  "accessKey": "xxxxxxxxxxxxxxxxxxxxxxxxx",
  "data": {
    "ip": "116.237.65.34"
  }
}
```

### <span id = "syncResponseExample">同步返回示例：</span>

```json
{
    "code": 1100,
    "detail": null,
    "devicePrimaryInfo": {},
    "deviceRiskLabels": [],
    "ipLabels": {
        "b_cgn": {
            "b_cgn": 0
        },
        "ip_city": {
            "ip_city": "深圳"
        },
        "ip_country": {
            "ip_country": "中国"
        },
        "ip_latitude": {
            "ip_latitude": 22.5427
        },
        "ip_longitude": {
            "ip_longitude": 114.01449
        },
        "ip_owner": {
            "ip_owner": "China Telecom"
        },
        "ip_province": {
            "ip_province": "广东"
        },
        "risk_ip": {
            "risk_ip": 0
        }
    },
    "message": "成功",
    "profileExist": 1,
    "requestId": "23e753672a69050d3dfb6db5d3259413",
    "tokenProfileLabels": [],
    "tokenRiskLabels": []
}
```
