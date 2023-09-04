# 智能验证码二次验证API文档

- - - - -

***版权所有 翻版必究***

- - - - -

* [二次验证接口](#sverifyInterface)
    + [请求参数](#requestParameter)
        - [请求URL](#requestUrl)
        - [字符编码格式](#requestEncode)
        - [请求方法](#requestMethod)
        - [建议超时时长](#requestTimeout)
        - [请求参数](#requestParameters)
    + [返回结果](#response)
    + [示例](#example)
        - [请求示例](#requestExample)
        - [返回示例](#responseExample)

# <span id = "sverifyInterface">二次验证接口</span>

## <span id = "requestParameter">请求参数</span>

### <span id = "requestUrl">请求URL：</span>
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京集群 | `http://captcha-s.fengkongcloud.com/ca/v1/sverify` | 二次验证 |
| 新加坡集群 | `http://captcha-xjp.fengkongcloud.com/ca/v1/sverify` |二次验证 |
| 弗吉尼亚集群 | `http://captcha-fjny.fengkongcloud.com/ca/v1/sverify` |二次验证 |

### <span id = "requestEncode">字符编码格式：</span>

`UTF-8`

### <span id = "requestMethod">请求方法：</span>

`POST`

### <span id = "requestTimeout">建议超时时间：</span>

1s

### <span id = "requestParameters">请求参数：</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥<br/>用于权限认证，开通账号服务时由数美提供或使用开通邮箱登录数美后台右上角相关文档处查看 | 必传参数 | accessKey |
| data | json_object | 请求的数据内容 | 必传参数 | 请求的数据内容，最长10MB，[详见data参数](#data) |

<span id = "data">其中，data的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| rid | string | 滑动验证码请求标识 | 必传参数 | 调用数美sdk获取 |
| lastReq | string | 此次验证交互前一次的天网事件返回请求的requestId | 建议参数 | 传入对应此次验证码验证的前置的请求ID
| ip | string | ip地址 | 必传参数 | 用户滑动验证码时的客户端公网ipv4地址 |
| tokenId | string | 用户账号标识，建议使用贵司用户UID（可加密）自行生成 , 标识用户唯一身份用作灌水和广告等行为维度风控。<br/>如无用户uid的场景建议使用唯一的数据标识传值 | 非必传参数 | 由数字、字母、下划线、短杠组成的长度小于等于64位的字符串 |
| deviceId | string | 数美设备标识 | 建议参数 | 数美设备指纹生成的设备唯一标识 |
| expectedMode | string | 验证码的类型 | 建议参数 | 验证码类型包括: <br/> - slide: 滑动验证码<br/> - select: 文字点选验证码<br/>- icon_select: 图标点选验证码<br/> - seq_select: 语序点选验证码<br/> - spatial_select:空间推理验证码<br/> |
| expectedAppId | string | 预期验证码参与验证的应用标识（AppId：该标识为在数美创建的应用标识） | 建议参数 | 回传预期需要校验验证码验证结果的应用标识|


## <span id = "response">返回结果</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 |  `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9101`：无权限操作<br/><br/>除message和requestId之外的字段，只有当code为1100时才会存在 |
| requestId | string | 请求标识 | 是 | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存 |
| message | string | 返回码描述 | 是 | 和code对应：成功QPS超限参数不合法服务失败余额不足无权限操作 |
| riskLevel | string | 处置建议 | 否 | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REJECT`：违规，建议直接拦截 |
| score | int | 当前事件的风险评分，分数越高，风险越大 | 否 | [0, 1000] |
| detail | json_object | 风险详情信息 | 否 | [详见detail结果详情](#detail) |

<span id = "detail">其中，detail结果详情如下：</span>

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| description | string | 当前事件的风险描述 | 是 |  |
| descriptionV2 | string | 当前事件的风险描述 | 是 |  |
| model | string | 规则标识，命中的最高优先级规则标识 | 是 |  |

## <span id = "example">示例：</span>

### <span id = "requestExample">请求示例：</span>

```json
{
    "accessKey":"****************",
    "data":{
        "lastReq":"****************"
        "expectedMode": "slide",
        "expectedAppId":"AppID",
        "rid":"************",
        "ip":"111.85.43.52"
    }
}
```

### <span id = "syncResponseExample">同步返回示例：</span>

```json
{
    "code":1100,
    "detail":{
        "description":"正常",
        "model":"M1000",
     },
    "message":"成功",
    "requestId":"585f346fd836f98403d72239f612b13a",
    "riskLevel":"PASS",
}

```

