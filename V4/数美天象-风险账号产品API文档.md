# 数美天象-风险账号识别产品API接口说明文档

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
|法兰克福集群|http://api-tianxiang-eur.fengkongcloud.com/tianxiang/v4|天象风险识别 |
|弗吉尼亚集群|http://api-tianxiang-fjny.fengkongcloud.com/tianxiang/v4|天象风险识别 |
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
| tokenId        | string   | 用户账号标识，建议使用贵司用户UID（可加密）自行生成 , 标识用户唯一身份用作灌水和广告等行为维度风控。如无用户uid的场景建议使用唯一的数据标识传值 | 不能为空 | 该ID为用户的唯一标识，且与其它数美接口的tokenId保持一致  |

## <span id = "response">返回结果</span>

放在HTTP Body中，采用Json格式，具体参数如下：


| **参数名称**       | **参数类型** | **参数说明**   | **是否必返** | **规范**                                                                                                                                                                                                    |
| -------------------- | -------------- | ---------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| code               | int          | 返回码         | 是           | `1100`：成功<br>`1901`：QPS超限<br>`1902`：参数不合法<br>`1903`：服务失败<br>`1911`：图片下载失败<br>`9101`：无权限操作<br>`3000`：运营商错误<br>除message和requestId之外的字段，只有当code为1100时才会存在 |
| message            | string       | 返回码描述     | 是           | 和code对应：`成功 QPS超限 参数不合法 服务失败 余额不足 无权限操作`                                                                                                                                          |
| requestId          | string       | 请求标识       | 是           | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存                                                                                                                                                      |
| tokenLabels        | json_object  | 账号标签信息   | 否           | 见下面详情内容，仅在tokenId传入且服务开通时返回                                                                                                                                                             |
| tokenProfileLabels | json_array   | 账号属性标签   | 否           | 见下面详情内容，仅在tokenId传入且服务开通时返回                                                                                                                                                             |
| tokenRiskLabels    | json_array   | 账号风险标签   | 否           | 见下面详情内容，仅在tokenId传入且服务开通时返回                                                                                                                                                             |

其中

**1）tokenLabels的详情内容**


| ***返回结果参数名*** | ***参数类型*** | ***参数说明***   | ***规范***                 |
| ---------------------- | ---------------- | ------------------ | ---------------------------- |
| machine_account_risk | json_object    | 机器控制相关风险 |                            |
| UGC_account_risk     | json_object    | UGC内容相关风险  |                            |
| scene_account_risk   | json_object    | 场景账号风险     | 特殊场景才可取到，如航司等 |

machine_account_risk的详情内容如下：


| ***返回结果参数名***              | ***参数类型*** | ***参数说明*** | ***规范***                           |
| ----------------------------------- | ---------------- | ---------------- | -------------------------------------- |
| b_machine_control_tokenid         | int            | 机器账号       | `0`：非机器控制账号`1`：机器控制账号 |
| b_machine_control_tokenid_last_ts | int            | 机器账号时间   |                                      |
| b_offer_wall_tokenid              | int            | 积分墙账号     | `0`：非积分墙账号`1`：积分墙账号     |
| b_offer_wall_tokenid_last_ts      | int            | 积分墙账号时间 |                                      |

UGC_account_risk的详情内容如下：


| ***返回结果参数名***             | ***参数类型*** | ***参数说明*** | ***规范***                             |
| ---------------------------------- | ---------------- | ---------------- | ---------------------------------------- |
| b_politics_risk_tokenid          | int            | 涉政风险       | `0`：暂未发现涉政风险`1`：存在涉政风险 |
| b_politics_risk_tokenid_last_ts  | int            | 涉政风险时间   |                                        |
| b_sexy_risk_tokenid              | int            | 色情风险       | `0`：暂未发现色情风险`1`：存在色情风险 |
| b_sexy_risk_tokenid_last_ts      | int            | 色情风险时间   |                                        |
| b_advertise_risk_tokenid         | int            | 广告风险       | `0`：暂未发现广告风险`1`：存在广告风险 |
| b_advertise_risk_tokenid_last_ts | int            | 广告风险时间   |                                        |

scene_account_risk的详情内容如下：


| ***返回结果参数名***        | ***参数类型*** | ***参数说明*** | ***规范***                           |
| ----------------------------- | ---------------- | ---------------- | -------------------------------------- |
| i_tout_risk_tokenid         | int            | 航司占座账号   | `0`：非航司占座账号`1`：航司占座账号 |
| i_tout_risk_tokenid_last_ts | int            | 航司占座时间   |                                      |


其中标签类返回字段

***1***）tokenProfileLabels的详情内容


| ***返回结果参数名*** | ***参数类型*** | ***参数说明***         | ***规范***                   |
| ---------------------- | ---------------- | ------------------------ | ------------------------------ |
| label1               | string         | 一级标签               | 展示账号属性标签的一级标签。 |
| label2               | string         | 二级标签               | 展示账号属性标签的二级标签。 |
| label3               | string         | 三级标签               | 展示账号属性标签的三级标签。 |
| description          | string         | 风险描述               | 展示账号属性标签的中文描述。 |
| timestamp            | int64          | 最近一次命中策略的时间 | 最近一次命中策略的时间       |
| detail               | json_object    | 证据描述               | 证据细节                     |

***2***）tokenRiskLabels的详情内容


| ***返回结果参数名*** | ***参数类型*** | ***参数说明***         | ***规范***                   |
| ---------------------- | ---------------- | ------------------------ | ------------------------------ |
| label1               | string         | 一级标签               | 展示账号风险标签的一级标签。 |
| label2               | string         | 二级标签               | 展示账号风险标签的二级标签。 |
| label3               | string         | 三级标签               | 展示账号风险标签的三级标签。 |
| description          | string         | 风险描述               | 展示账号风险标签的中文描述。 |
| timestamp            | int64          | 最近一次命中策略的时间 | 最近一次命中策略的时间       |
| detail               | json_object    | 证据描述               | 证据细节                     |

## <span id = "example">示例：</span>

### <span id = "requestExample">请求示例：</span>

```json
{
  "accessKey": "xxxxxxxxxxxxxxxxxxxxxxxxx",
  "data": {
    "deviceId": "ceshideviceId"
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
  "tokenLabels": {
    "machine_account_risk": {
      "b_machine_control_tokenid": 1,
      "b_machine_control_tokenid_last_ts": 1587711321232,
      "b_offer_wall_tokenid": 1,
      "b_offer_wall_tokenid_last_ts": 1587711321232
    },
    "UGC_account_risk": {
      "b_politics_risk_tokenid": 1,
      "b_politics_risk_tokenid_last_ts": 1587711321232,
      "b_sexy_risk_tokenid": 1,
      "b_sexy_risk_tokenid_last_ts": 1587711321232,
      "b_advertise_risk_tokenid": 1,
      "b_advertise_risk_tokenid_last_ts": 1587711321232
    },
    "scene_account_risk": {
      "i_tout_risk_tokenid": 1,
      "i_tout_risk_tokenid_last_ts": 1587711321232
    },
    "account_active_info": {
      "i_tokenid_first_active_timestamp": 1587711321232,
      "i_tokenid_active_days_7d": 5,
      "i_tokenid_active_days_4w": 5
    },
    "account_freq_info": {
      "i_tokenid_login_cnt_1d": 5,
      "i_tokenid_login_cnt_7d": 5
    },
    "account_relate_info": {
      "i_tokenid_relate_smid_cnt_1d": 5,
      "i_tokenid_relate_smid_cnt_7d": 5,
      "i_tokenid_relate_ip_city_cnt_1d": 5,
      "i_tokenid_relate_ip_city_cnt_7d": 5
    },
    "account_common_info": {
      "s_tokenid_relate_smid_info_map_4w": [
        {
          "smid": "xxxx1",
          "days": "3"
        },
        {
          "smid": "xxxx2",
          "days": "5"
        },
        {
          "smid": "xxxx3",
          "days": "10"
        }
      ],
      "s_tokenid_relate_ip_city_info_map_4w": [
        {
          "city": "北京",
          "days": "3"
        },
        {
          "city": "长沙",
          "days": "5"
        },
        {
          "city": "武汉",
          "days": "10"
        }
      ]
    }
  },
  "tokenProfileLabels": [
    {
      "label1": "age_gender",
      "lable2": "token_age",
      "label3": "minor_token",
      "description": "年龄性别:年龄:未成年人",
      "timestamp": 1634732525000,
      "detail": {
      }
    }
  ],
  "tokenRiskLabels": [
    {
      "label1": "risk_device_token",
      "label2": "b_cloud_token_device",
      "label3": "b_cloud_token_device",
      "description": "风险设备账号:云手机账号:云手机账号",
      "timestamp": 1634732525000,
      "detail": {
      }
    },
    {
      "label1": "risk_device_token",
      "label2": "b_hook_token",
      "label3": "b_hook_token",
      "description": "风险设备账号:hook设备:hook设备",
      "timestamp": 1634732525000,
      "detail": {
      }
    }
  ]
}
```
