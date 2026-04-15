# 天枢产品API接口文档

### 请求URL：

| 集群 | URL |
| --- | --- |
| 北京 | `http://api-nexus-bj.fengkongcloud.com/nexus/v1` |

### 请求方法：

`POST`

### 字符编码：

`UTF-8`

### 建议超时时间：5s

<span id="request-params"></span>

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥<br/>用于权限认证，开通账号服务时由数美提供或使用开通邮箱登录数美后台右上角相关文档处查看 | 必传参数 |  |
| appId | string | 应用标识，用于区分相同公司的不同应用数据 | 必传参数 | 需要联系数美开通，请以数美单独提供的传值为准 |
| eventId | string | 事件标识 | 必传参数 | 需要联系数美服务开通，请使用数美单独提供的传值为准 |
| textType | string | 文本检测的风险类型 | textType、imgType、multimodalType必传其一 | [详见附录：textType](#appendix-textType) |
| imgType | string | 图片检测的风险类型 | textType、imgType、multimodalType必传其一 | [详见附录：imgType](#appendix-imgType) |
| multimodalType | string | 多模态检测的风险类型 | textType、imgType、multimodalType必传其一 | [详见附录：multimodalType](#appendix-multimodalType) |
| data | json_object | 请求的数据内容 | 必传参数 | 请求的数据内容，data字段长度最长5MB，[详见data参数](#data) |

<span id="data">其中，data的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| sessionId | string | 会话标识 | 必传参数 | 由数字、字母、下划线、短杠组成的长度小于等于32位的字符串 |
| role | string | 用户角色 | 必传参数 | 角色<br/>枚举值：<br/>user：用户<br/>assistant：机器人 |
| roundEnd | bool | 对话结束标识 | 非必传参数 | 默认值为false，当一轮assistant输出结束，设置为true |
| content | array | 检测内容 | 必传参数 | text/img对象数组，text单次限制384个字符 |

<span id="content">其中，content的内容如下：</span>

text对象

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| -------------- | -------- | ------------ | ------------ | -------- |
| text           | string   | 文本内容     | 非必传参数   |          |

img对象

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| -------------- | -------- | ------------ | ------------ | -------- |
| img            | string   | 图片内容     | 非必传参数   | 可使用base64编码的图片数据或者图片的url链接 |

## 同步返回结果

<span id="singleRet">放在HTTP Body中，采用Json格式，具体参数如下：</span>

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 |  `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/> |
| message | string | 返回码描述 | 是 | 和code对应：成功<br/>QPS超限<br/>参数不合法<br/>服务失败 |
| requestId | string | 请求标识 | 是 | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存 |
| riskLevel | string | 处置建议 | 是 | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| detail | json\_array | 详细结果 | 否 | 风险明细数组，结构见[detail参数](#detail) |

<span id="detail">其中detail数组的每个成员的内容如下：</span>

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范**                           |
| ------------------ | ------------ | ------------ | ------------ | ---------------------------------- |
| riskLabel1         | string       | 一级风险标签 | 是           | 一级风险标签                       |
| riskLabel2         | string       | 二级风险标签 | 是           | 二级风险标签                       |
| riskLabel3         | string       | 三级风险标签 | 是           | 三级风险标签                       |
| riskDescription    | string       | 风险原因     | 是           |                                    |
| riskDetail         | json_object  | 风险详情     | 是           | 详见riskDetail参数                 |
| allLabels          | json\_array  | 风险标签详情 | 是           | 返回命中的所有风险标签以及详情信息 |

<span id="riskDetail">其中，riskDetail结构如下：</span>

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskSegments | json\_array | 高风险片段内容，检测文本包含涉政、暴恐、违禁等风险内容的时候存在 | 否 |  |
| matchedLists | json\_array | 命中的客户自定义名单列表 | 否 |  |
| segmentText | string | 审核的文本片段内容 | 否 | riskSource为`1001`时返回 |
| riskSource | int | 标识资源哪里违规 | 是 | 标识风险结果的来源<br/>`1001`：文本风险<br/>`1002`：视觉图片风险<br/>`1004`：多模态风险<br/> |

matchedLists数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 命中的名单名称 | 否 | |
| words | json\_array | 命中的敏感词信息 | 否 | |

matchedLists中，words数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string | 命中的敏感词 | 否 | |
| position | int_array | 敏感词所在位置 | 否 | |

riskSegments的每个元素的详细内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segment | string | 高风险内容片段 | 否 | |
| position | int_array | 高风险内容片段所在位置 | 否 | |





<span id="sync-request-example"></span>

### 同步请求示例：

```json
{
    "accessKey": "xxxxxxxx",
    "appId": "xxx",
    "eventId": "xxx",
    "textType": "EROTIC",
    "data": {
        "sessionId": "sessionId001",
        "role": "assistant",
        "roundEnd": true,
        "content": [
            {
                "text": "你好我是大模型"
            },
            {
                "img": "https://example.com/demo.jpg"
            }
        ]
    }
}
```

<span id="sync-response-example"></span>

### 同步返回示例：

```json
{
    "requestId": "1264bbbc8d9dd29983c47547657eb936",
    "code": 1100,
    "message": "成功",
    "riskLevel": "REJECT",
    "detail": [
        {
            "riskLabel1": "porn",
            "riskLabel2": "sm",
            "riskLabel3": "sm",
            "riskDescription": "色情:SM:SM",
            "riskDetail": {
                "riskSource": 1001,
                "segmentText": "你好呀SM"
            },
            "allLabels": [
                {
                    "riskLabel1": "porn",
                    "riskLabel2": "sm",
                    "riskLabel3": "sm",
                    "riskDescription": "色情:SM:SM"
                }
            ]
        }
    ]
}
```

<span id="type-appendix"></span>

### 附录：Type可选值

<span id="appendix-textType">textType可选值如下：</span>

监管类：  
`POLITICAL`（涉政）  
`TERORISM`（暴恐）  
`PORNOGRAPHY`（色情）  
`PROHIBITED`（违禁）  
`DIRTY`（不文明用语）

隐私类：  
`BASICPROFILE`（个人基本资料）  
`IDENTITYINFO`（个人ID信息）  
`HEALTHINFO`（个人健康生理信息）  
`FINACIALINFO`（个人财产信息）  
`COMMUNICATIONINFO`（个人通信信息）  
`BEHAVIORINFO`（个人行为信息）

版权类：  
`IPCHARACTER`（版权角色）  
`BRAND`（品牌名称）  
`ELECTRONICS`（电子商品）  
`PUBLICFIGURE`（公众人物）  
`SPORTS`（体育赛事）  
`MUSIC`（歌曲名称）  
`ENTERTAINMENT`（影视名称）

指令攻击类：  
`DATAPRIVACYATTACK`（数据与隐私攻击指令）  
`RESOURCEATTACK`（资源与算力攻击指令）  
`INDUCEDOUTPUTATTACK`（诱导输出攻击指令）  
`ROLEPLAYATTACK`（角色扮演攻击指令）  
`CONSTRAINTATTACK`（约束限制攻击指令）  
`编码攻击指令`  
`逻辑推理指令`  
`权限攻击指令`

<span id="appendix-imgType">imgType可选值如下：</span>

监管类：  
`POLITICAL`（涉政）  
`TERORISM`（暴恐）  
`PROHIBITED`（违禁）  
`PORNOGRAPHY`（色情性感）

隐私类：  
`IDDOCUMENT`（个人身份证件）  
`BIOMETRICS`（个人生物识别）  
`HEATHINFO`（个人健康生理信息）  
`FINACIALINFO`（个人财产信息）

版权类：  
`LOGO`（LOGO）  
`CELEBRITY`（娱乐明星）  
`PUBLICFIGURE`（公众人物）  
`PRONSTAR`（色情明星）  
`EXCUTIVE`（企业高管）  
`GAMECHARCTER`（游戏角色）  
`ANIMECHARACTER`（动漫角色）  
`MEDIACHARCTER`（影视角色）  
`MEDIASCENE`（影视场景）

生成图/质量类：  
`AIHUMANQUALITY`（人物质量）  
`AIANIMALQUALITY`（动物质量）  
`AIOBJECTYAULITY`（物品质量）

<span id="appendix-multimodalType">multimodalType可选值如下：</span>

监管类：  
`POLITICAL`（涉政）  
`TERORISM`（暴恐）  
`PORNOGRAPHY`（色情）  
`PROHIBITED`（违禁）  
`DIRTY`（不文明用语）
