# 数美智能文本识别产品API接口文档

- - - - -

***版权所有 翻版必究***

- - - - -

* [请求参数](#requestParameter)
    + [请求URL](#requestUrl)
    + [字符编码格式](#requestEncode)
    + [请求方法](#requestMethod)
    + [建议超时时长](#requestTimeout)
    + [请求参数](#requestParameters)
* [返回结果](#response)
    + [返回结果](#responseParameters)
* [示例](#example)
    + [请求示例](#requestExample)
    + [返回示例](#responseExample)


## <span id = "requestParameter">请求参数</span>

### <span id = "requestUrl">请求URL：</span>


| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-text-bj.fengkongcloud.com/v2/saas/anti_fraud/text` | 中文文本 |
| 上海 | `http://api-text-sh.fengkongcloud.com/v2/saas/anti_fraud/text` | 中文文本 |
| 广州 | `http://api-text-gz.fengkongcloud.com/v2/saas/anti_fraud/text` | 中文文本 |
| 美国（弗吉尼亚） | `http://api-text-fjny.fengkongcloud.com/v2/saas/anti_fraud/text` | 中文文本 |
| 新加坡 | `http://api-text-xjp.fengkongcloud.com/v2/saas/anti_fraud/text` | 中文文本 |
| 印度 | `https://api-text-yd.fengkongcloud.com/v2/saas/anti_fraud/text` | 中文文本 |


### <span id = "requestEncode">字符编码格式：</span>

`UTF-8`字符集编码


### <span id = "requestMethod">请求方法：</span>

`POST`


### <span id = "requestTimeout">建议超时时长：</span>

1s


### <span id = "requestParameters">请求参数：</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥 | Y | 由数美提供 |
| appId | string | 应用标识 | Y | 用于区分应用，可选值如下：<br/>`default`：默认应用<br/>额外应用值需数美单独分配提供 |
| type | string | 检测的风险类型 | Y | 可选值：<br/>`ZHIBO`:直播<br/>`ECOM`:电商<br/>`GAME`:游戏<br/>`NEWS`:新闻资讯<br/>`FORUM`:论坛<br/>`SOCIAL`:社交<br/>`QQ`:QQ<br/>`NOVEL`:小说<br/>`DEFAULT`：默认值（包含：<br/>涉政、暴恐、违禁、色情、辱骂、广告、灌水、无意义、隐私、广告法、黑名单）<br/>`FRUAD`：网络诈骗<br/>`UNPOACH`：高价值用户防挖<br/> <br/>以上type可以下划线组合，如：`ZHIBO_DEFAULT_FRUAD` |
| businessType | string | 检测的业务类型 | N | 可选值：<br/>`MINOR`：未成年人 |
| data | json\_object | 请求的数据内容 | Y | 最长1MB, [详见data参数](#data) |


<span id = "data"> 其中，data的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| text | string | 需要检测的文本（200字内效果最佳） | Y | 若传递nickname字段，则会同时校验文本+昵称内容。<br/> |
| tokenId | string | 用户账号标识， 建议使用贵司用户UID（可加密）自行生成 , 标识用户唯一身份用作灌水和广告等行为维度风控。<br/>如无用户uid的场景建议使用唯一的数据标识传值 | Y | 由数字、字母、下划线、短杠组成的长度小于等于64位的字符串 |
| gender | int | 用户性别 | N | 可选值<br/>`0`:女性<br/>`1`:男性<br/> |
| channel | string | 业务场景 | N | 渠道表配置 |
| nickname | string | 用户昵称，强烈建议传递此参数，几乎所有平台的恶意用户都会通过昵称散播垃圾信息，存在涉政违禁和导流信息等风险 | N |  |
| ip | string | ip地址，该参数用于IP维度的用户行为分析，同时可用于比对数美IP黑库 | N |  |
| phone | string | 用户手机号，可用于比对数美手机号黑库 | N |  |
| deviceId | string | 数美设备标识，强烈建议传入该参数，数美设备指纹标识，用于用户行为分析。当恶意用户篡改mac、imei等设备信息时，使用deviceId能够发现和识别此类恶意行为，同时可用于比对数美设备指纹黑名单 | N |  |
| receiveTokenId | string | 接收者的tokenId，私聊场景必选 | N |  |
| level | int | 用户等级，针对不同等级的用户可配置不同拦截策略 | N |  |
| registerTime | int | 帐号注册时间，强烈建议传递此参数，新注册帐号的异常操作风险较高 | N |  |
| friendNum | int | 帐号好友数，社交场景强烈推荐传此参数，标识用户质量 | N |  |
| fansNum | int | 帐号粉丝数，直播/社区场景强烈推荐传此参数，标识用户质量 | N |  |
| isPremiumUser | int | 是否为优质（如付费）用户，配置不同等级，标识用户质量 | N | 可选值<br/>`0`:默认值<br/>`1`:优质账号<br/> |
| isTokenSeperate | int | 是否区分不同应用下的账号 | N | 可选值<br/>`0`:不区分<br/>`1`:区分<br/>默认值为0。<br/>取值为1时不同应用下的账号体系各自独立，账号相关的策略特征在不同应用下单独统计和生效。 |
| role | string | 用户角色，对不同角色可配置不同策略。 | N | 直播领域"ADMIN"表示房管，"HOST"表示主播，"SYSTEM"系统角色<br/>游戏领域"ADMIN"表示管理员，"USER"表示普通用户，缺失或者"USER"默认普通用户 |
| room | string | 直播间/游戏房间编号，可针对单个房间制定不同的策略 | N |  |
| topic | string | 讨论的话题编号，可为书评区编号、论坛帖子编号。 | N |  |
| nickocr | string | 头像OCR识别出的文本内容 | N |  |
| imei | string | 用户android设备唯一标识，相比tokenId和IP，imei和mac更难被更换，当恶意用户使用多个不同账户和IP进作恶时，通过imei和mac能够有效关联识别此类恶意行为，同时可用于比对数美设备黑名单。 | N |  |
| mac | string | 用户android设备唯一标识，相比tokenId和IP，imei和mac更难被更换，当恶意用户使用多个不同账户和IP进作恶时，通过imei和mac能够有效关联识别此类恶意行为，同时可用于比对数美设备黑名单。 | N |  |
| idfv | string | 用户iOS应用唯一标识，相比tokenId和IP，idfv不能被修改，当恶意用户使用多个不同账户和IP进行恶意行为时，使用idfv能够发现和识别此类恶意行为。 | N |  |
| idfa | string | 用户iOS应用唯一标识，相比tokenId和IP，idfv不能被修改，当恶意用户使用多个不同账户和IP进行恶意行为时，使用idfv能够发现和识别此类恶意行为。 | N |  |
| passThrough | json_object | 该字段内容同返回结果一起返回 | N |  |



## <span id = "response">返回结果</span>

### <span id = "responseParameters">返回结果</span>

<span id = "responseBody">放在HTTP Body中，采用Json格式，具体参数如下：</span>

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int| 返回码 | Y | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9100`：余额不足<br/>`9101`：无权限操作 |
| message | string | 返回码描述 | Y | 和code对应：<br/>成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>余额不足<br/>无权限操作 |
| requestId | string | 请求标识 | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存|
| score | int | 风险分数 | N | 取值范围[0,1000]，分数越高风险越大 |
| riskLevel | string | 处置建议 | N | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| status | int | 提示服务是否超时 | Y | 可能返回值：<br/>`0`：正常<br/>`501`：超时 |
| detail | string | 风险详情 | N | [详见detail参数](#detail) |
| businessLabels | json_array | 辅助信息 | Y | 命中的所有业务标签以及详细信息。[详见businessLabels参数](#businessLabels) |
| tokenProfileLabels | json_array | 辅助信息 | N | 属性账号类标签。[详见账号标签参数](#tokenProfileLabels) |
| tokenRiskLabels | json_array | 辅助信息 | N | 风险账号类标签。[详见账号标签参数](#tokenProfileLabels) |

<span id = "detail">其中detail字段如下：</span>

| 参数名称          | **类型**    | **是否必选** | **说明**                                                     |
| ----------------- | ----------- | ------------ | ------------------------------------------------------------ |
| riskType          | int         | Y            |                                                              |
| model             | string      | Y            | 规则标识，用来标识文本命中的策略规则。<br/>注：该参数为旧版API返回参数，兼容保留，后续版本将去除，请勿依赖此参数，仅供参考 |
| description       | string      | Y            | 策略规则风险原因描述<br/>注：该参数为旧版API返回参数，兼容保留，后续版本将去除，请勿依赖此参数，仅供参考 |
| descriptionV2     | string      | N            | 新版策略规则风险原因描述<br/>注：该参数为新版API返回参数，过渡阶段只有新策略才会返回 |
| isBlackToken      | string      | N            | 该账号被画像策略标记为高危账号，可能取值：<br/>`1`:高危账号   |
| hitPosition       | string      | N            | 命中的敏感词在文本中的位置，从0开始计数                      |
| filteredText      | string      | N            | 命中的敏感词被替换为*后的文本（该参数仅在命中敏感词时存在）  |
| matchedList       | string      | N            | 命中敏感词所在的名单名称（该参数仅在命中敏感词时存在）       |
| matchedItem       | string      | N            | 命中的具体敏感词（该参数仅在命中敏感词时存在）               |
| contactResult     | json_array  | N            | 联系方式识别结果，包含识别出的微信、微博、QQ、手机号的字符串类型和内容。详细信息见下表说明。[详见联系方式](#contactResult) |
| matchedDetail     | string      | N            | 命中的敏感词详细信息（需要和数美沟通开启相应策略）,可以反序列化为json_array。[详见matchedDetail](#matchedDetail) |
| passThrough       | json_object | N            | 该字段是客户传入透传字段                                     |
| sexy_risk_tokenid | float       | N            | 色情账号分，取值[0,1]                                        |
| contextProcessed  | bool        | Y            | true时说明该请求联系了上下文；<br/>false时说明该请求未关联上下文，如需该功能，可与数美协商 |
| contextText       | string      | Y            | 未开启联系上下文服务则只返回当前文本                         |

<span id = "contactResult">contactResult中的每一项内容：</span>

| **参数名称**| **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| contactType | int| 辅助信息 | N| 联系方式类型，可选值区间【0-3】，详情如下：<br/>`0`: 手机号 <br/>`1`: QQ号 <br/>`2`: 微信号 <br/>`3`: 微博号 |
| contactString | string | 辅助信息 | N| 联系方式串 |

<span id = "matchedDetail">其中，matchedDetail内容：</span>

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| listId         | string       |              | Y            | 返回码                                                       |
| matchedField   | string_array |              | N            | 标识昵称或文本内容命中了敏感词（该参数仅在命中敏感词时存在），可选值：<br/>text：文本命中敏感词<br/>nickname：昵称命中敏感词 |
| name           | string       |              | Y            | 命中敏感词所在的名单名称                                     |
| organization   | string       |              | N            | 命中名单所属的公司标识，其中“GLOBAL”为全局名单               |
| words          | string_array |              | N            | 命中的对应名单中的所有敏感词                                 |
| wordPostitions | json_array   |              | N            | 命中的对应名单中的所有敏感词及位置。[详见wordPostitions](#words) |

<span id = "words">wordPostitions中的每一项内容：</span>

| **参数名称** | **类型**| **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string| 辅助信息 | N| 命中的敏感词 |
| position | string | 辅助信息 | N| 敏感词所在位置 |

<span id = "businessLabels">其中，businessLabels的内容如下：</span>

| 参数名称            | 类型        | 参数说明                                                     | 是否必返 | 规范         |
| ------------------- | ----------- | ------------------------------------------------------------ | -------- | ------------ |
| businessLabel1      | string      | businessLabels不为空必返                                     | Y        | 一级业务标签 |
| businessLabel2      | string      | businessLabels不为空必返                                     | Y        | 二级业务标签 |
| businessLabel3      | string      | businessLabels不为空必返                                     | Y        | 三级业务标签 |
| businessDescription | string      | businessLabels不为空必返                                     | Y        | 标签描述     |
| probability         | float       | businessLabels不为空必返<br/>可选值在0～1之间，值越大，可信度越高 | Y        | 置信度       |
| businessDetail      | Json_object | businessLabels不为空必返                                     | Y        | 业务详情     |

<span id = "tokenProfileLabels">其中，tokenProfileLabels、tokenRiskLabels的内容如下：</span>

| 参数名称    | 类型   | 参数说明     | 是否必返 | 规范                       |
| ----------- | ------ | ------------ | -------- | -------------------------- |
| label1      | string | 一级标签     | 否       |                            |
| label2      | string | 二级标签     | 否       |                            |
| label3      | string | 三级标签     | 否       |                            |
| description | string | 标签描述     | 否       |                            |
| timestamp   | Int    | 打标签时间戳 | 否       | 13位Unix时间戳，单位：毫秒 |



## <span id = "example">示例</span>

### <span id = "requestExample">请求示例</span>

```json
{
    "accessKey":"*************",
    "appId":"default",
	"type":"ZHIBO",
	"businessType":"MINOR",
    "data":
    {
        "text":"我12岁了，你呢，我要去天安门看毛主席照片",
        "tokenId":"4567898765jhgfdsa",
        "channel":"text"
    }   
}
```

### <span id = "responseExample">返回示例</span>

```json
{
    "code":1100,
    "message":"\u6210\u529f",
    "requestId":"cfd98ce4510efd74e3a5182d2b752ee9",
    "score":990,
    "riskLevel":"REJECT",
    "detail":"{\"contactResult\":[{\"contactString\":\"\u4f60\u5462\",\"contactType\":3}],\"contextProcessed\":false,\"contextText\":\"\u621112\u5c81\u4e86\uff0c\u4f60\u5462\uff0c\u6211\u8981\u53bb\u5929\u5b89\u95e8\u770b\u6bdb\u4e3b\u5e2d\u7167\u7247\",\"description\":\"\u6d89\u653f\uff1a\u6d89\u653f\uff1a\u6d89\u653f\",\"descriptionV2\":\"\u6d89\u653f\uff1a\u6d89\u653f\uff1a\u6d89\u653f\",\"filteredText\":\"\u621112\u5c81\u4e86\uff0c\u4f60\u5462\uff0c\u6211\u8981\u53bb***\u770b***\u7167\u7247\",\"hitPosition\":\"16,17,18\",\"matchedDetail\":\"[{\\\"listId\\\":\\\"9f58f988212b33abe0aecfdaaf9569b2\\\",\\\"matchedFiled\\\":[\\\"text\\\"],\\\"name\\\":\\\"\u6d89\u653f_\u56fd\u5185\u9886\u5bfc\u4eba_\u5386\u4efb\u56fd\u7ea7\u9886\u5bfc\\\",\\\"organization\\\":\\\"GLOBAL\\\",\\\"wordPositions\\\":[{\\\"position\\\":\\\"16,17,18\\\",\\\"word\\\":\\\"\u6bdb\u4e3b\u5e2d\\\"}],\\\"words\\\":[\\\"\u6bdb\u4e3b\u5e2d\\\"]},{\\\"listId\\\":\\\"ec0d2507492be935a37b014eb28adf48\\\",\\\"matchedFiled\\\":[\\\"text\\\"],\\\"name\\\":\\\"\u6d89\u653f_\u6838\u5fc3\u9886\u5bfc_\u6bdb\u6cfd\u4e1c\u540c\u97f3\\\",\\\"organization\\\":\\\"GLOBAL\\\",\\\"wordPositions\\\":[{\\\"position\\\":\\\"16,17,18\\\",\\\"word\\\":\\\"\u6bdb\u4e3b\u5e2d\\\"}],\\\"words\\\":[\\\"\u6bdb\u4e3b\u5e2d\\\"]},{\\\"listId\\\":\\\"e9d10f80db083704aa139c69411dd9a8\\\",\\\"matchedFiled\\\":[\\\"text\\\"],\\\"name\\\":\\\"\u6d4b\u8bd5zyk\\\",\\\"organization\\\":\\\"RlokQwRlVjUrTUlkIqOg\\\",\\\"wordPositions\\\":[{\\\"position\\\":\\\"1,2\\\",\\\"word\\\":\\\"12\\\"},{\\\"position\\\":\\\"2\\\",\\\"word\\\":\\\"2\\\"}],\\\"words\\\":[\\\"12\\\",\\\"2\\\"]},{\\\"listId\\\":\\\"70cecfffebf31c2ac2b612cc3b6af142\\\",\\\"matchedFiled\\\":[\\\"text\\\"],\\\"name\\\":\\\"\u6d89\u653f\u8bcd\u5e933\\\",\\\"organization\\\":\\\"RlokQwRlVjUrTUlkIqOg\\\",\\\"wordPositions\\\":[{\\\"position\\\":\\\"12,13,14\\\",\\\"word\\\":\\\"\u5929\u5b89\u95e8\\\"},{\\\"position\\\":\\\"16,17,18\\\",\\\"word\\\":\\\"\u6bdb\u4e3b\u5e2d\\\"}],\\\"words\\\":[\\\"\u5929\u5b89\u95e8\\\",\\\"\u6bdb\u4e3b\u5e2d\\\"]}]\",\"matchedItem\":\"\u6bdb\u4e3b\u5e2d\",\"matchedList\":\"\u6d89\u653f_\u56fd\u5185\u9886\u5bfc\u4eba_\u5386\u4efb\u56fd\u7ea7\u9886\u5bfc\",\"model\":\"MA000001020001003\",\"riskType\":100,\"sexy_risk_tokenid\":0}",
	"status":0,
    "businessLabels":[
        {
            "businessDescription":"未成年人:未成年人:未成年人",
            "businessLabel1":"minor",
            "businessLabel2":"minor",
            "businessLabel3":"clear",
     		"probability":1,
            "businessDetail":{}
        }
    ]
}
```
