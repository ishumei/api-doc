# 数美智能文本识别产品API接口文档

## 请求参数

### 请求URL：

| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-text-bj.fengkongcloud.com/v2/saas/anti_fraud/text` | 中文文本 |
| 上海 | `http://api-text-sh.fengkongcloud.com/v2/saas/anti_fraud/text` | 中文文本 |
| 美国（弗吉尼亚） | `http://api-text-fjny.fengkongcloud.com/v2/saas/anti_fraud/text` | 中文文本 |
| 新加坡 | `http://api-text-xjp.fengkongcloud.com/v2/saas/anti_fraud/text` | 中文文本 |
| 印度 | `https://api-text-yd.fengkongcloud.com/v2/saas/anti_fraud/text` | 中文文本 |

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
| appId | string | 应用标识 | Y | 用于区分应用，需要联系数美开通，请使用数美单独提供的传值为准 |
| type | string | 检测的风险类型或者场景 | Y | 可选值：<br/>`ZHIBO`:直播<br/>`ECOM`:电商<br/>`GAME`:游戏<br/>`NEWS`:新闻资讯<br/>`FORUM`:论坛<br/>`SOCIAL`:社交<br/>`QQ`:QQ<br/>`NOVEL`:小说<br/>`TEXTRISK`：默认值（包含：<br/>涉政、暴恐、违禁、色情、辱骂、广告、隐私、广告法、无意义）<br/>`FRUAD`：网络诈骗<br/>`UNPOACH`：高价值用户防挖<br/> <br/>`TEXTMINOR`: 未成年人<br/>以上type可以下划线组合，如：`ZHIBO_TEXTRISK_FRUAD` |
| data | json\_object | 请求的数据内容 | Y | 最长1MB, [详见data参数](#data) |

 <span id="data">其中，data的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| text | string | 需要检测的文本 | Y | 文本字数上限1万字，超过1万字只截取前1万字进行识别<br/>若传递nickname字段，则会同时校验文本+昵称内容。 |
| relateText | string | 需要检测的关联文本 | N | 文本字数上限128字，超过128字只截取前128字进行识别。传入此字段会结合text一起检测。|
| tokenId | string | 用户账号标识， 建议使用贵司用户UID（可加密）自行生成 , 标识用户唯一身份用作灌水和广告等行为维度风控。<br/>如无用户uid的场景建议使用唯一的数据标识传值 | Y | 由数字、字母、下划线、短杠组成的长度小于等于64位的字符串 |
| gender | int | 用户性别 | N | 可选值<br/>`0`:女性<br/>`1`:男性<br/> |
| channel | string | 业务场景 | N | 渠道表配置 |
| nickname | string | 用户昵称，强烈建议传递此参数，几乎所有平台的恶意用户都会通过昵称散播垃圾信息，存在涉政违禁和导流信息等风险，长度限制150字符，超出部分会被截断 | N |  |
| ip | string | ip地址，该参数用于IP维度的用户行为分析，同时可用于比对数美IP黑库 | N | 发送该文本的的用户公网ipv4或ipv6地址 |
| deviceId | string | 数美设备标识，强烈建议传入该参数，数美设备指纹标识，用于用户行为分析。当恶意用户篡改mac、imei等设备信息时，使用deviceId能够发现和识别此类恶意行为，同时可用于比对数美设备指纹黑名单 | N |  |
| receiveTokenId | string | 接收者的tokenId，私聊场景必选 | N |  |
| level | int | 用户等级，针对不同等级的用户可配置不同拦截策略 | N | 可选值：<br/>`0`：最低级用户，典型如新注册、完全不活跃或等级为0的用户等;<br/>`1`：较低级用户，典型如低活跃或低等级用户等；<br/>`2`：中等级用户，典型如具备一定活跃或等级中等的用户等；<br/>`3`：较高级用户，典型如高活跃或高等级用户等；<br/>`4`：最高级用户，典型如付费用户、VIP用户等 |
| registerTime | int | 帐号注册时间，强烈建议传递此参数，新注册帐号的异常操作风险较高 | N |  |
| friendNum | int | 帐号好友数，社交场景强烈推荐传此参数，标识用户质量 | N |  |
| fansNum | int | 帐号粉丝数，直播/社区场景强烈推荐传此参数，标识用户质量 | N |  |
| isPremiumUser | int | 是否为优质（如付费）用户，配置不同等级，标识用户质量 | N | 可选值<br/>`0`:默认值<br/>`1`:优质账号<br/> |
| isTokenSeparate | int | 是否区分不同应用下的账号 | N | 可选值<br/>`0`:不区分<br/>`1`:区分<br/>默认值为0。<br/>取值为1时不同应用下的账号体系各自独立，账号相关的策略特征在不同应用下单独统计和生效。 |
| room | string | 直播间/游戏房间编号，可针对单个房间制定不同的策略 | N |  |
| topic | string | 讨论的话题编号，可为书评区编号、论坛帖子编号。 | N |  |
| nickocr | string | 头像OCR识别出的文本内容 | N |  |
| imei | string | 用户android设备唯一标识，相比tokenId和IP，imei和mac更难被更换，当恶意用户使用多个不同账户和IP进作恶时，通过imei和mac能够有效关联识别此类恶意行为，同时可用于比对数美设备黑名单。 | N |  |
| mac | string | 用户android设备唯一标识，相比tokenId和IP，imei和mac更难被更换，当恶意用户使用多个不同账户和IP进作恶时，通过imei和mac能够有效关联识别此类恶意行为，同时可用于比对数美设备黑名单。 | N |  |
| idfv | string | 用户iOS应用唯一标识，相比tokenId和IP，idfv不能被修改，当恶意用户使用多个不同账户和IP进行恶意行为时，使用idfv能够发现和识别此类恶意行为。 | N |  |
| idfa | string | 用户iOS应用唯一标识，相比tokenId和IP，idfv不能被修改，当恶意用户使用多个不同账户和IP进行恶意行为时，使用idfv能够发现和识别此类恶意行为。 | N |  |
| passThrough | json_object | 该字段内容同返回结果一起返回 | N |  |
| dataId | string | 数据标识 | N |  |

## 返回结果

### 返回结果

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int| 返回码 | Y | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1905`：字数超限<br/>`9101`：无权限操作 |
| message | string | 返回码描述 | Y | 和code对应：<br/>成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>字数超限<br/>无权限操作 |
| requestId | string | 请求标识 | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存|
| score | int | 风险分数 | N | 取值范围[0,1000]，分数越高风险越大 |
| riskLevel | string | 处置建议 | N | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| status | int | 提示服务是否超时 | Y | 可能返回值：<br/>`0`：正常<br/>`501`：超时 |
| detail | string | 风险详情 | N | [详见detail参数](#detail) |
| businessLabels | json_array | 辅助信息 | Y | 命中的所有业务标签以及详细信息。[详见businessLabels参数](#businessLabels) |
| tokenProfileLabels | json_array | 辅助信息 | N | 属性账号类标签。[详见账号标签参数](#tokenProfileLabels) |
| tokenRiskLabels | json_array | 辅助信息 | N | 风险账号类标签。[详见账号标签参数](#tokenProfileLabels) |
| unauthorizedType | string | 辅助信息 | N | 未授权的type |

<span id="detail">其中detail字段如下：</span>

| 参数名称          | **类型**    | **是否必选** | **说明**                                                     |
| ----------------- | ----------- | ------------ | ------------------------------------------------------------ |
| riskType          | int         | Y            | 风险类型：<br/>`0`：正常<br/>`100`：涉政<br/>`200`：色情<br/>`210`：辱骂<br/>`300`：广告<br/>`340`：网络诈骗<br/>`400`：灌水<br/>`500`：无意义<br/>`600`：违禁<br/>`700`：其他<br/>`720`：黑账号<br/>`730`：黑IP<br/>`800`：高危账号<br/>`900`：自定义 |
| model             | string      | Y            | 规则标识，用来标识文本命中的策略规则。<br/>注：该参数为旧版API返回参数，兼容保留，后续版本将去除，请勿依赖此参数，仅供参考 |
| description       | string      | Y            | 策略规则风险原因描述<br/>注：该参数为旧版API返回参数，兼容保留，后续版本将去除，请勿依赖此参数，仅供参考 |
| descriptionV2     | string      | N            | 新版策略规则风险原因描述<br/>注：该参数为新版API返回参数，过渡阶段只有新策略才会返回 |
| isBlackToken      | string      | N            | 该账号被画像策略标记为高危账号，可能取值：<br/>`1`:高危账号  |
| hitPosition       | string      | N            | 命中的敏感词在文本中的位置，从0开始计数                      |
| filteredText      | string      | N            | 风险片段被替换为*后的文本  |
| matchedList       | string      | N            | 命中敏感词所在的名单名称（该参数仅在命中敏感词时存在）       |
| matchedItem       | string      | N            | 命中的具体敏感词（该参数仅在命中敏感词时存在）               |
| contactResult     | json_array  | N            | 联系方式识别结果，包含识别出的微信、QQ、手机号的字符串类型和内容。详细信息见下表说明。[详见联系方式](#contactResult) |
| matchedDetail     | string      | N            | 命中的敏感词详细信息（需要和数美沟通开启相应策略）,可以反序列化为json_array。[详见matchedDetail](#matchedDetail) |
| passThrough       | json_object | N            | 该字段是客户传入透传字段                                     |
| sexy_risk_tokenid | float       | N            | 色情账号分，取值[0,1]                                        |
| contextProcessed  | bool        | Y            | true时说明该请求联系了上下文；<br/>false时说明该请求未关联上下文，如需该功能，可与数美协商 |
| contextText       | string      | Y            | 未开启联系上下文服务则只返回当前文本                         |

<span id="contactResult">contactResult中的每一项内容：</span>

| **参数名称**| **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| contactType | int| 辅助信息 | N| 联系方式类型，可选值区间【0-3】，详情如下：<br/>`0`: 手机号 <br/>`1`: QQ号 <br/>`2`: 微信号  |
| contactString | string | 辅助信息 | N| 联系方式串 |

<span id="matchedDetail">其中，matchedDetail内容：</span>

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| listId         | string       |              | Y            | 返回码                                                       |
| matchedFiled | string_array |              | N            | 标识昵称或文本内容命中了敏感词（该参数仅在命中敏感词时存在），可选值：<br/>text：文本命中敏感词<br/>nickname：昵称命中敏感词 |
| name           | string       |              | Y            | 命中敏感词所在的名单名称                                     |
| organization   | string       |              | N            | 命中名单所属的公司标识，其中“GLOBAL”为全局名单               |
| words          | string_array |              | N            | 命中的对应名单中的所有敏感词                                 |
| wordPositions | json_array   |              | N            | 命中的对应名单中的所有敏感词及位置。[详见wordPositions](#words) |

<span id="words">wordPositions中的每一项内容：</span>

| **参数名称** | **类型**| **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string| 辅助信息 | N| 命中的敏感词 |
| position | string | 辅助信息 | N| 敏感词所在位置 |

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

## 示例

### 请求示例

```json
{
    "accessKey":"*************",
    "appId":"default",
	"type":"ZHIBO",
	"businessType":"MINOR",
    "data":
    {
        "text":"加个好友吧 qq12345",
        "tokenId":"4567898765jhgfdsa",
        "channel":"text"
    }
}
```

### 返回示例

```json
{
    "businessLabels":[

    ],
    "code":1100,
    "detail":"{"contactResult":[{"contactString":"qq12345","contactType":2}],"contextProcessed":false,"contextText":"加个好友吧 qq12345","description":"广告：联系方式：联系方式","descriptionV2":"广告：联系方式：联系方式","filteredText":"加个好友吧 qq12345","matchedDetail":"[{\"listId\":\"e9d10f80db083704aa139c69411dd9a8\",\"matchedFiled\":[\"text\"],\"name\":\"测试zyk\",\"organization\":\"RlokQwRlVjUrTUlkIqOg\",\"wordPositions\":[{\"position\":\"8,9,10,11,12\",\"word\":\"12345\"},{\"position\":\"8,9,10\",\"word\":\"123\"},{\"position\":\"8,9,10,11\",\"word\":\"1234\"},{\"position\":\"10,11,12\",\"word\":\"345\"},{\"position\":\"9,10\",\"word\":\"23\"},{\"position\":\"8,9\",\"word\":\"12\"},{\"position\":\"9,10,11,12\",\"word\":\"2345\"},{\"position\":\"9,10,11\",\"word\":\"234\"},{\"position\":\"10,11\",\"word\":\"34\"},{\"position\":\"11,12\",\"word\":\"45\"}],\"words\":[\"12345\",\"123\",\"1234\",\"345\",\"23\",\"12\",\"2345\",\"234\",\"34\",\"45\"]}]","matchedItem":"12345,123,1234,345,23,12,2345,234,34,45","matchedList":"测试zyk","model":"MA000007015016003","riskType":300,"sexy_risk_tokenid":0,"tokenScore":4050}",
    "message":"成功",
    "requestId":"376c6607be790d70dc1cd40c61280ea0",
    "riskLevel":"REJECT",
    "score":650,
    "status":0
}
```
