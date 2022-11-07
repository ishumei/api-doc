# 数美智能文本识别产品API接口文档


目录

- [数美智能文本识别产品API接口文档](#数美智能文本识别产品api接口文档)
  - [请求参数](#请求参数)
    - [请求URL：](#请求url)
    - [字符编码格式：](#字符编码格式)
    - [请求方法：](#请求方法)
    - [建议超时时长：](#建议超时时长)
    - [请求参数：](#请求参数-1)
  - [返回结果](#返回结果)
    - [返回结果](#返回结果-1)
  - [示例](#示例)
    - [请求示例](#请求示例)
    - [返回示例](#返回示例)

## 请求参数

### 请求URL：

| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-text-bj.fengkongcloud.com/text/v4` | 中文文本<br/> 国际化文本 |
| 上海 | `http://api-text-sh.fengkongcloud.com/text/v4` | 中文文本 |
| 广州 | `http://api-text-gz.fengkongcloud.com/text/v4` | 中文文本 |
| 美国（弗吉尼亚） | `http://api-text-fjny.fengkongcloud.com/text/v4` | 中文文本 <br/> 国际化文本 |
| 新加坡 | `http://api-text-xjp.fengkongcloud.com/text/v4` | 中文文本 <br/> 国际化文本 |

### 字符编码格式：

`UTF-8`字符集编码

### 请求方法：

`POST`

### 建议超时时长：

1s

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥 | Y | 由数美提供 |
| appId | string | 应用标识 | Y | 用于区分应用，需要联系数美服务开通，请使用数美单独提供的传值为准 |
| eventId | string | 事件标识 | Y | 需要联系数美服务开通，请使用数美单独提供的传值为准 |
| type | string | 检测的风险类型 | Y | 可选值：<br/>`TEXTRISK`：常规风险检测（包含：<br/>涉政、暴恐、违禁、色情、辱骂、广告、隐私、广告法）<br/>`FRUAD`：网络诈骗检测<br/>`UNPOACH`：高价值用户防挖检测<br/> <br/>`TEXTMINOR`: 未成年人检测<br/>以上type可以下划线组合，如：`TEXTRISK_FRUAD` |
| data | json\_object | 请求的数据内容 | Y | 最长1MB, [详见data参数](#data) |

 其中，data的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| text | string | 需要检测的文本 | Y | 当lang字段为zh或不传入lang字段时，文本字数上限1万字，超过1万字时会报错<br/>若传递nickname字段，则会同时校验文本+昵称内容。|
| tokenId | string | 用户账号标识， 建议使用贵司用户UID（可加密）自行生成 , 标识用户唯一身份用作灌水和广告等行为维度风控。<br/>如无用户uid的场景建议使用唯一的数据标识传值 | Y | 由数字、字母、下划线、短杠组成的长度小于等于64位的字符串 |
| lang | string | 待检测的文本内容语种 | N | 可选值和对应语种如下：<br/>`zh`：中文<br/>`en`：英文<br/>`ar`：阿拉伯语<br/>`hi`：印地语<br/>`es`：西班牙语<br/>`fr`：法语<br/>`ru`：俄语<br/>`pt`：葡萄牙语<br/>`id`：印尼语<br/>`de`：德语<br/>`ja`：日语<br/>`tr`：土耳其语<br/>`vi`：越南语<br/>`it`：意大利语<br/>`th`：泰语<br/>`tl`：菲律宾语<br/>`ko`：韩语<br/>`ms`：马来语<br/>`auto`：自动识别语种类型<br/>默认值zh，国内集群客户可不传或zh；海外文本内容如果不能区分语种建议取值auto，系统会自动检测语种类型 |
| nickname | string | 用户昵称 | N | 校验昵称内容风险 |
| ip | string | ip地址 | N | 发送该文本的的用户公网ipv4地址 |
| deviceId | string | 数美设备标识 | N | 数美设备指纹生成的设备唯一标识 |
| extra | json\_object | 辅助参数 | N | 用于辅助文本检测的相关信息,[详见extra参数](#extra) |

data 中 extra数组每个元素的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| receiveTokenId | string | 私聊场景下消息接收者的tokenId | N | 由数字、字母、下划线、短杠组成的长度小于等于64位的字符串，eventId值为`message`时必传 |
| topic | string | 可为话题编号、书评区编号、论坛帖子编号 | N | eventId值为`article`必传 |
| atId | string | 群聊场景下被@用户的tokenId | N | 由数字、字母、下划线、短杠组成的长度小于等于64位的字符串，eventId值为`groupChat`必传 |
| room | string | 直播间/游戏房间编号 | N | eventId值为`groupChat`时必传|
| level | int | 用户等级 | N | 可选值：<br/>`0`：最低级用户，典型如新注册、完全不活跃或等级为0的用户等;<br/>`1`：较低级用户，典型如低活跃或低等级用户等；<br/>`2`：中等级用户，典型如具备一定活跃或等级中等的用户等；<br/>`3`：较高级用户，典型如高活跃或高等级用户等；<br/>`4`：最高级用户，典型如付费用户、VIP用户等 |
| role | string | 用户角色 | N | 用于区分直播／游戏行业不同角色的用户，可选值（默认为普通用户）：<br/>`ADMIN`：房管／管理员<br/>`HOST`：主播<br/>`SYSTEM`：系统<br/>`USER`：普通用户 |
| sex | int | 性别 | N | 用于用户性别，可选值：<br/>`0`：男性<br/>`1`：女性<br/>`2`：性别不明 |
| isTokenSeparate| int | 应用体系，取值为1时不同应用下的账号体系各自独立，账号相关的策略特征在不同应用下单独统计和生效 | N | 区分不同应用下的账号,可取值（默认值为`0`）：<br/>`0`:不区分<br/>`1`:区分<br/> |
| passThrough | Json | 透传字段 | 非必传字段 | 该字段内容会随着返回值一起返回 |

## 返回结果

### 返回结果

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int| 返回码 | Y | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9100`：余额不足<br/>`9101`：无权限操作 |
| message | string | 返回码描述 | Y | 和code对应：<br/>成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>余额不足<br/>无权限操作 |
| requestId | string | 请求标识 | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存|
| riskLevel | string | 处置建议 | Y | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| riskLabel1| string | 一级风险标签 | Y | 一级风险标签，当riskLevel为`PASS`时返回`normal` |
| riskLabel2| string | 二级风险标签 | Y | 二级风险标签，当riskLevel为`PASS`时为空|
| riskLabel3| string | 三级风险标签 | Y | 三级风险标签，当riskLevel为`PASS`时为空|
| riskDescription | string | 风险原因 | Y | 当riskLevel为`PASS`时为"正常"|
| riskDetail | json_object| 风险详情 | Y | 风险详情，[详见riskDetail参数](#riskDetail)|
| tokenLabels | json object| 辅助信息 | Y | 账号风险画像标签信息见下面详情内容。[详见tokenLabels参数](#tokenLabels) |
| auxlnfo| json_object| 辅助信息 | Y | [详见auxlnfo参数](#auxlnfo)|
| allLabels | json_array | 辅助信息 | Y | 命中的所有风险标签以及详情信息。[详见allLabels参数](#allLabels) |
| businessLabels | json_array | 辅助信息 | Y | 命中的所有业务标签以及详细信息。[详见businessLabels参数](#businessLabels) |
| tokenProfileLabels | json_array | 辅助信息 | N | 属性账号类标签。[详见账号标签参数](#tokenProfileLabels) |
| tokenRiskLabels | json_array | 辅助信息 | N | 风险账号类标签。[详见账号标签参数](#tokenProfileLabels) |
| langResult | json_array | 语种信息 | N | 语种信息。[详见语种信息参数](#langResult) |

<span id="langResult">其中，语种信息langResult结构如下：</span>

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范**                                                     |
| ------------ | -------- | ------------ | ------------ | ------------------------------------------------------------ |
| detectedLang | string   | 语种识别结果 | N            | 当在国际化文本产品下传入lang的值为auto时返回该字段。值为标准语言代码表，例如："zh"、"en"、"ar"等 |

<span id="auxlnfo">其中auxInfo字段如下：</span>

| **参数名称**| **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| filteredText| string | 辅助信息 | N| 敏感词被替换为*后的文本（该参数仅在命中敏感词时存在） <br/>语境模型，联系方式相关风险不返回该字段 |
| passThrough | json_object | 透传字段 | 否 | 该字段内容与请求参数data中extra的passThrough的值相同。 |
| contactResult | json_array | 辅助信息 | N| 联系方式识别结果，包含识别出的微信、微博、QQ、手机号的字符串类型和内容。 [详见contactResult参数](#contactResult) |
| unauthorizedType | string | 辅助信息 | N | 未授权的type。 |

<span id="contactResult">auxInfo中，contactResult数组每个元素的内容如下：</span>

| **参数名称**| **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| contactType | int| 辅助信息 | N| 联系方式类型，可选值区间【0-3】，详情如下：<br/>`0`: 手机号 <br/>`1`: QQ号 <br/>`2`: 微信号 <br/>`3`: 微博号 |
| contactString | string | 辅助信息 | N| 联系方式串 |

<span id="riskDetail">其中，riskDetail的内容如下：</span>

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| matchedLists | json_array | 辅助信息 | N| 命中的客户自定义名单列表。[详见matchedLists参数](#matchedLists) |
| riskSegments | json_array | 辅助信息，高风险内容片段检测文本包含涉政、暴恐、违禁、广告法等风险内容的时候存在 | N| [详见riskSegments参数](#riskSegments) |

<span id="matchedLists">riskDetail中，matchedLists数组每个元素的内容如下：</span>

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范**|
| --- | --- | --- | --- | --- |
| name | string | 辅助信息 | N| 命中的名单名称|
| words| json_array | 辅助信息 | N| 命中的敏感词数组。[详见words参数](#words) |

<span id="words">matchedLists中，words数组每个元素的内容如下：</span>

| **参数名称** | **类型**| **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string| 辅助信息 | N| 命中的敏感词 |
| position | int_array | 辅助信息 | N| 敏感词所在位置 |

<span id="riskSegments">riskDetail中，riskSegments的内容如下：</span>

| **参数名称** | **类型**| **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segment| string| 辅助信息 | N| 高风险内容片段 |
| position | int_array | 辅助信息 | N| 高风险内容片段所在位置 |

<span id="tokenLabels">其中，tokenLabels的详情内容：</span>

| **参数名称** | **类型**| **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| UGC_account_risk | json_object | 辅助信息 | N| UGC内容相关风险。[详见UGC_account_risk参数](#UGC_account_risk) |

<span id="UGC_account_risk">tokenLabels中，UGC_account_risk的详情内容如下：</span>

| **参数名称**| **类型** | **参数说明** | **是否必返** | **规范**|
| --- | --- | --- | --- | --- |
| sexy_risk_tokenid | float| 辅助信息 | N| 色情账号风险分取值区间[0-1] |

<span id="allLabels">其中，allLabels的内容如下：</span>

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | allLabels不为空时必返 | Y | 一级风险标签 |
| riskLabel2 | string | allLabels不为空时必返 | Y | 二级风险标签|
| riskLabel3 | string | allLabels不为空时必返 | Y | 三级风险标签|
| riskDescription| string | allLabels不为空时必返 | Y | 风险原因|
| probability| float| 置信度 | Y | 可选值在0～1之间，值越大，可信度越高 注意：allLabels不为空时必返 |
| riskDetail | json_object| 风险详情 | Y | 格式与上层riskDetail结构相同 注意：allLabels不为空时必返 |
| riskLevel | string | 风险等级 | Y | 可能返回值：<br/>`REVIEW`：可疑<br/>`REJECT`：违规 |

<span id="businessLabels">其中，businessLabels的内容如下：</span>

| 参数名称            | 类型        | 参数说明                                                     | 是否必返 | 规范         |
| ------------------- | ----------- | ------------------------------------------------------------ | -------- | ------------ |
| businessLabel1      | string      | businessLabels不为空必返                                     | Y        | 一级业务标签 |
| businessLabel2      | string      | businessLabels不为空必返                                     | Y        | 二级业务标签 |
| businessLabel3      | string      | businessLabels不为空必返                                     | Y        | 三级业务标签 |
| businessDescription | string      | businessLabels不为空必返                                     | Y        | 标签描述     |
| probability         | float       | businessLabels不为空必返<br/>可选值在0～1之间，值越大，可信度越高 | Y        | 置信度       |
| businessDetail      | Json_object | businessLabels不为空必返                                     | Y        | 业务详情     |

<span id="tokenProfileLabels">其中，tokenProfileLabels、tokenRiskLabels的内容如下：</span>

| 参数名称    | 类型   | 参数说明     | 是否必返 | 规范                       |
| ----------- | ------ | ------------ | -------- | -------------------------- |
| label1      | string | 一级标签     | 否       |                            |
| label2      | string | 二级标签     | 否       |                            |
| label3      | string | 三级标签     | 否       |                            |
| description | string | 标签描述     | 否       |                            |
| timestamp   | Int    | 打标签时间戳 | 否       | 13位Unix时间戳，单位：毫秒 |

一级标签的内容如下：

| 一级标签 | 一级标识    | 类型     | 备注              |
| -------- | ----------- | -------- | ----------------- |
| 涉政     | politics    | 监管标签 | type值为TEXTRISK  |
| 暴恐     | violence    | 监管标签 | type值为TEXTRISK  |
| 色情     | porn        | 监管标签 | type值为TEXTRISK  |
| 违禁     | ban         | 监管标签 | type值为TEXTRISK  |
| 辱骂     | abuse       | 监管标签 | type值为TEXTRISK  |
| 广告法   | ad_law      | 监管标签 | type值为TEXTRISK  |
| 广告     | ad          | 监管标签 | type值为TEXTRISK  |
| 黑名单   | blacklist   | 监管标签 | type值为TEXTRISK  |
| 无意义   | meaningless | 监管标签 | type值为TEXTRISK  |
| 隐私     | privacy     | 监管标签 | type值为TEXTRISK  |
| 网络诈骗 | fraud       | 监管标签 | type值为FRUAD     |
| 未成年人 | minor       | 监管标签 | type值为TEXTMINOR |

## 示例

### 请求示例

```json
{
    "accessKey":"*************",
    "appId":"default",
    "eventId":"text",
	"type":"TEXTRISK",
    "data":
    {
        "text":"加个好友吧 qq12345",
        "tokenId":"4567898765jhgfdsa",
        "ip":"118.89.214.89",
        "deviceId":"*************",
        "nickname":"***********",
        "extra":
        {
            "topic":"12345",
            "atId":"username1",
            "room":"ceshi123",
            "receiveTokenId":"username2",
            "level":1,
            "role":"ADMIN"
    	}
    }
}
```

### 返回示例

```json
{
    "allLabels":[
        {
            "probability":1,
            "riskDescription":"涉政:涉政:涉政",
            "riskDetail":{

            },
            "riskLabel1":"politics",
            "riskLabel2":"shezheng",
            "riskLabel3":"shezheng",
            "riskLevel":"REVIEW"
        },
        {
            "probability":0.95559550232975,
            "riskDescription":"广告:加好友:加好友",
            "riskDetail":{

            },
            "riskLabel1":"ad",
            "riskLabel2":"jiahaoyou",
            "riskLabel3":"jiahaoyou",
            "riskLevel":"REJECT"
        },
        {
            "probability":1,
            "riskDescription":"广告:联系方式:联系方式",
            "riskDetail":{

            },
            "riskLabel1":"ad",
            "riskLabel2":"lianxifangshi",
            "riskLabel3":"lianxifangshi",
            "riskLevel":"REJECT"
        }
    ],
    "auxInfo":{
        "contactResult":[
            {
                "contactString":"qq12345",
                "contactType":2
            }
        ],
        "filteredText":"加个好友吧 qq12345"
    },
    "businessLabels":[

    ],
    "code":1100,
    "message":"成功",
    "requestId":"bb917ec5fa11fd02d226fb384968feb1",
    "riskDescription":"广告:联系方式:联系方式",
    "riskDetail":{

    },
    "riskLabel1":"ad",
    "riskLabel2":"lianxifangshi",
    "riskLabel3":"lianxifangshi",
    "riskLevel":"REJECT"
}
```
