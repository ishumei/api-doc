# 数美智能音频流识别产品API接口文档
- - - - -

***版权所有 翻版必究***

- - - - -

* [音频流上传请求](#requestParameter)
    + [请求URL](#requestUrl)
    + [字符编码格式](#requestEncode)
    + [请求方法](#requestMethod)
    + [建议超时时长](#requestTimeout)
    + [请求参数](#requestParameters)
* [同步返回结果](#syncResponseParameters)
* [回调返回结果](#callbackResponseParameters)
* [音频流关闭接口](#closeInterface)
    + [请求URL](#closeRequestUrl)
    + [字符编码格式](#closeRequestEncode)
    + [请求方法](#closeRequestMethod)
    + [建议超时时长](#closeRequestTimeout)
    + [请求参数](#closeRequestParameters)
* [音频流关闭返回结果](#closeResponse)
* [示例](#example)
    + [请求示例](#uploadRequestExample)
    + [返回示例](#callbackResponseExample)
    + [关流请求示例](#closeRequestExample)
    + [关流返回示例](#closeResponseExample)
* [Demo](#demo)


## <span id = "requestParameter">音频流上传请求</span>

### <span id = "requestUrl">请求URL：</span>
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 新加坡 | `http://api-audiostream-xjp.fengkongcloud.com/audiostream/v4` | 中文音频流<br>英语音频流<br>阿语音频流 |
| 硅谷 | `http://api-audiostream-gg.fengkongcloud.com/audiostream/v4` | 中文音频流<br>英语音频流<br>阿语音频流 |
| 上海 | `http://api-audiostream-sh.fengkongcloud.com/audiostream/v4` | 中文音频流 |

### <span id = "requestMethod">请求方法：</span>

`POST` 

### <span id = "requestEncode">字符编码：</span>

`UTF-8`

### <span id = "requestTimeout">建议超时时间：</span>

1s

### <span id = "requestParameters">请求参数：</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 公司密钥 | Y | 由数美提供 |
| appId | string | 应用标识 | Y | 用于区分应用，可选值如下：<br>`default`：默认应用<br>额外应用值需数美单独分配提供 |
| eventId | string | 事件标识 | Y | 区分场景数据可选值：<br>`audiobook`：有声书<br>`education`：教育音频<br>`game`：游戏语音房<br>`live`：秀场直播<br>`ecommerce`：电商直播<br>`voiceroom`：交友语音房<br>`private`：私密语音聊天<br>`default`：默认事件<br>`other`：其他 |
| type | string | 检测的风险类型 | businesstype和type必传其一 | 可选值：监管一级标签<br>`POLITICS`：涉政识别<br>`PORN`：色情识别<br>`AD`：广告识别<br>`MOAN`：娇喘识别<br>`ANTHEN`：国歌识别<br>`GENDER`：性别识别<br>`TIMBRE`：音色识别<br>`ABUSE`：辱骂识别<br>`SING`：唱歌识别<br>`LANGUAGE`：语种识别<br>`MINOR`：未成年人识别<br>如需识别音色，唱歌,语种GENDER必传如需做组合识别，通过下划线连接即可，例如`POLITICS_PORN_MOAN`涉政、色情和娇喘识别 |
| businessType | string | 业务标签 | businesstype和type必传其一 | 可选值：业务标签的一、二、三级标签<br>`GENDER`：性别识别<br>`TIMBRE`：音色识别<br>`SING`：唱歌识别<br>`LANGUAGE`：语种识别<br>`MINOR`：未成年人识别 |
| data | json_object | 请求的数据内容 | Y | 本次请求相关信息，最长1MB,[详见data参数](#data) |
| callback | string | 回调地址 | Y | 异步检测结果回调通知您的URL，支持HTTP和HTTPS |

<span id = "data">其中，data的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| tokenId | string | 用户账号标识 | Y | 用于区分用户账号，建议传入用户ID |
| btId | string | 应用ID，用于区分相同公司的不同应用 | Y | 用于查询指定音频，限长128位字符 |
| streamType | string | 流类型 | Y | 可选值：<br>`NORMAL`：普通流地址<br>`ZEGO`：即构<br>`AGORA`：声网 <br>`TRTC`：腾讯录制 |
| url | string | 直播流地址 | N | 当streamType为`NORMAL`时必传 |
| lang | string | 音频流语言类型 | Y | 可选值如下，（默认值为`zh`）：<br>`zh`：中文<br>`en`：英文<br>`ar`：阿拉伯语 |
| zegoParam | json_object | 要检测的流参数 | N | 当streamType为`ZEGO`时必传，[详见zegoParam参数](#zegoParam) |
| initDomain | int | 即构SDK初始化是否有设置隔离域名 | N | 当即构客户端init初始化支持隔离域名和随机userId该字段必传,可选值：<br>`1`：仅支持客户端初始化有隔离域名<br>`2`：支持客户端初始化有隔离域名和随机userId功能 |
| trtcParam | json_object | 腾讯录制参数（当streamType为TRTC时必传），详见扩展参数 | N | 腾讯录制参数（当streamType为TRTC时必传），详见扩展参数 |
| agoraParam | json_object | 要检测的声网流参数 | N | 当streamType为`AGORA`时必传,[详见agoraParam参数](#agoraParam) |
| room | string | 直播房间号 | N | |
| role | string | 用户角色 | N | 用户角色对不同角色可配置不同策略。直播领域可取值如下（默认值`USER`普通用户）：<br>`ADMIN`:房管<br>`HOST`：主播<br>`SYSTEM`：系统角色<br>`USER`：普通用户 |
| returnAllText | int | 返回音频片段的等级 | N | 可选值如下（默认为`0`）：<br>`0`：返回风险等级为非pass的音频片段<br>`1`：返回所有风险等级的音频片段 |
| returnPreText | int | 是否返回违规音频流片段的前文文字信息 | N | 可选值如下（默认值为`0`）：<br>`0`：不返回违规片段前一个片段文字；<br>`1`：返回违规片段前一分钟文字； |
| returnPreAudio | int | 是否返回违规音频流片段的前文音频链接 | N | 可选值如下（默认值为`0`）：<br>`0`：不返回违规片段前一个片段音频；<br>`1`：返回违规片段前一分钟音频链接； |
| extra | json_object | 辅助参数 | N | 用于辅助音频检测的相关信息，[详见extra参数](#extra) |
| liveTitle | string | 标题 | N | 房间标题，非必填参数，在客户开通人审服务传入 |
| anchorName | string | 昵称 | N | 用户昵称，非必填参数，在客户开通人审服务传入 |



<span id = "zegoParam">data中，zegoParam详细内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| tokenId | string | zego提供的身份验证信息，获取zego的identify_token用于登录，生成方式详见zego文档：[https://doc-zh.zego.im/zh/1345.html](https://doc-zh.zego.im/zh/1345.html)注意tokenId是唯一标识上传鉴黄每一次请求都需要重新生成新的 | N | |
| streamId | string | 用户设置的音频流编号，唯一对应一路音频流，streamId与roomId至少存在其中之一，如果streamId与roomId同时存在时，streamId有效；当streamId生效时，服务端以用户为单位拉流 | N |  |
| roomId | string | 用户设置的房间编号，唯一对应一个房间，streamId与roomId至少存在其中之一，如果streamId与roomId同时存在时，streamId有效；当roomId生效时，服务端以房间为单位拉流 | N | |
| testEnv | bool | 是否使用zego测试环境 | N | 默认值为`false`:<br>`true`：测试环境<br>`false`：正式环境 |

<span id = "agoraParam">data中，agoraParam详细内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| appId | string | 声网提供的appId，注意与数美的appId区分开 | N | 非数美的appId |
| channel | string | 声网提供的频道名 | N | |
| token | string | 安全要求较高的用户可以使用 Token进行认证，生成方式详见声网文档：[https://docs.agora.io/cn/Recording/token\_server?platform=CPP](https://docs.agora.io/cn/Recording/token\_server?platform=CPP) | N |  |
| uid | int | 用户 ID，当token存在时，必须提供生成token时所使用的用户ID。注意，此处需要区别实际房间中的用户uid，提供给服务端录制所用的uid不允许在房间中存在 | N | 32 位无符号整数 |
| isMixingEnabled | bool | 单流/合流录制<br>合流是指一个直播房间一路流<br>分流是指一个麦位一路流， | N | 默认值为`true`<br>`true`:合流<br>`false`:分流 |
| channelProfile | int | 声网录制的频道模式<br>通信，常见的 1 对 1 单聊或群聊，频道内任何用户可以自由说话<br>直播，有两种用户角色: 主播和观众 | N | 可选值如下（默认值为`0`）：<br>`0`: 通信<br>`1`: 直播 |

其中 data.trtcParam内容如下

| 参数名称   | 类型   | **是否必传** | 说明                                                         |
| ---------- | ------ | ------------ | ------------------------------------------------------------ |
| sdkAppId   | int    | Y            | 腾讯提供的sdkAppId                                           |
| demoSences | int    | Y            | 录制类型可选值：分流录制：2 合流录制：4                      |
| userId     | string | Y            | 分配给录制段的userId，限制长度为32bit，只允许包含（a-zA-Z），数字(0-9)以及下划线和连词符 |
| userSig    | string | Y            | 录制userId对应的验证签名，相当于登录密码                     |
| roomId     | int    | Y            | 房间号码，取值范围：【1-4294967294】roomId与strRoomId必传一个，若两者都有值优先选用roomId |
| strRoomId  | string | Y            | 房间号码 取值说明：只允许包含（a-zA-Z），数字(0-9)以及下划线和连词符若您选用strRoomId时，需注意strRoomId和roomId两者都有值优先选用roomId |

<span id = "extra">data中，extra的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| passThrough | json_object | 透传字段 | N | 该字段内容会随着回调结果一起返回 |

## <span id = "syncResponseParameters">同步返回结果</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 本次请求的唯一标识 | 是 | 请求唯一标识 |
| code | int | 请求返回码 | 是 | `1100`：成功<br>`1901`：QPS超限<br>`1902`：参数不合法<br>`1903`：服务失败<br>`9101`：无权限操作 |
| message | string | 请求返回描述 | 是 | 和请求返回码对应 |
| detail | json_object | 描述详细信息 | 否 | 描述错误码请求，[详见detail参数](#detail) |


<span id = "detail">其中，detail结构如下：</span>


| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **说明** |
| --- | --- | --- | --- | --- |
| errorCode | int | | 否 | 状态码<br>`1001`：重复推流 |

## <span id = "callbackResponseParameters">回调返回结果</span>

当音频流稳定接入开始接受数美监测后，数美会持续回调识别结果给客户，回调策略根据returnAllText的取值不同而不同。

returnAllText为`0`时，监测到音频流有违规内容时回调结果给客户；

returnAllText为`1`时，每隔10秒返回一次最近10秒的识别结果给客户。

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 本次请求的唯一标识 | 是 | |
| btId | string | 音频唯一标识 | 是 | |
| code | int | 请求返回码 | 是 | `1100`：成功<br>`1901`：QPS超限<br>`1902`：参数不合法<br>`1903`：服务失败<br>`9101`：无权限操作, message和requestId之外的字段，只有当code为1100时才会存在 |
| message | string | 请求返回描述，和请求返回码对应 | 是 | |
| audioDetail | json_object | 风险音频片段信息 | 否 | 当code等于`1100`时返回，[详见audioDetail参数](#audioDetail) |
| passThrough | json_object | 透传字段 | 否 | 该字段内容与请求参数data中extra的passThrough的值相同。 |

<span id = "audioDetail">其中，audioDetail结构如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| audioUrl | string | 音频片段地址 | 是 | |
| riskLevel | string | 当前事件的处置建议 | 是 | `PASS`：通过<br>`REVIEW`：审核<br>`REJECT`：拒绝 |
| riskLabel1 | string | 一级标签 | 是 | 各个一级标签之间是并列的关系，当riskLevel为`PASS`时返回`normal` |
| riskLabel2 | string | 二级标签 | 是 | 二级标签归属于一级标签，当riskLevel为`PASS`时为空 |
| riskLabel3 | string | 三级标签 | 是 | 三级标签归属于二级标签，当riskLevel为`PASS`时为空 |
| riskDescription | string | 标签解释 | 是 | 对于命中用户自定义名单时返回：`命中自定义名单`；<br>当riskLevel为`PASS时返回`正常`；<br>其他情况展现形式为一级标签：二级标签：三级标签的中文名 |
| audioText | string | 音频转译文本的结果 | 否 | 当returnPreAudio值为`1`时，包含违规音频前一个片段文本内容和违规音频片段文本内容；<br>当returnPreAudio值为`0`时，包含违规音频片段文本内容 |
| preAudioUrl | string | 前一个音频片段音频地址 | 否 | 当returnPreAudio值为`1`时，包含违规音频前一个片段音频地址；<br>returnPreAudio值为`0`时，不返回 |
| riskDetail | json_object | 风险详情信息 | 否 | 当code等于`1100`时返回，[详见riskDetail参数](#riskDetail)  |
| auxInfo | json_object | 其他辅助信息 | 是 | 返回时间戳等辅助信息，[详见auxInfo参数](#auxInfo) |
| businessLabels | json_array | 音频业务标签 | 否 | 返回性别、音色、是否唱歌等标签，[详见businessLabels参数](#businessLabels) |
| allLabels | json_array | 风险标签 | 否 | 全部风险标签，[详见allLabels参数](#allLabels) |
| riskSource | int | 风险来源 | 是 | 风险来源 |

<span id = "auxInfo">其中，auxInfo结构如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| beginProcessTime | int | 辅助参数 | 是 | 开始处理的时间（13位时间戳） |
| finishProcessTime | int | 辅助参数 | 是 | 结束处理的时间（13位时间戳） |
| userId | int | 声网用户账号标识 | 否 | 仅分流情况下存在，返回的userId是实际房间中的用户id，与请求参数中的uid无关。 |
| strUserId | string | trtc用户账号标识 | 否 | 用户账号标识（仅TRTC分流情况下存在）。返回的userId是实际房间中的用户id，与请求参数中的uid无关。 |
| room | string | 房间号 | 否 | |
| seiInfo | array | SEI信息 | 否 | （需要联系数美开通） |


<span id = "riskDetail">其中，riskDetail结构如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| audioText | string | 音频转译文本的结果 | 否 | |
| matchedLists | json_array | 命中的客户自定义名单信息 | 否 | 命中客户自定义名单时返回，其他时不存在，[详见matchedLists参数](#matchedLists) |
| riskSegments | json_array | 高风险内容片段 | 否 | 在涉政、暴恐、违禁、竞品、广告法等功能的时候存在，[详见riskSegments参数](#riskSegments) |

<span id = "matchedLists">其中，matchedLists结构如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 客户自定义名单名 | 是 | |
| words | json_array | 命中的这个名单中的敏感词信息 | 是 | 下标从0开始计数，[详见words参数](#words) |

<span id = "words">其中，words结构如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string | 敏感词 | 是 | |
| position | int_array | 敏感词所在位置 | 是 | 下标从0开始计数 |

<span id = "riskSegments">其中，riskSegments结构如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segment | string | 高风险内容片段 | 否 | |
| position | int_array | 高风险内容片段所在位置 | 否 | 下标从0开始计数 |

<span id = "businessLabels">其中，businessLabels结构如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级业务标签 | 否 | 一级业务标签 |
| businessLabel2 | string | 二级业务标签 | 否 | 二级业务标签 |
| businessLabel3 | string | 三级业务标签 | 否 | 三级业务标签 |
| businessDescription | string | 中文标签描述 | 否 | 业务标签描述 |

<span id = "allLabels">allLabels结构如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险原因 | 是 | 风险原因 |


## <span id = "closeInterface">音频流关闭通知接口</span>

**接口描述**

该接口用于客户端通知服务端某个音频流已关闭。

### <span id = "closeRequestUrl">请求URL：</span>
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 新加坡 | `http://api-audiostream-xjp.fengkongcloud.com/finish_audiostream/v4` | 中文音频流<br>英语音频流<br>阿语音频流 |
| 硅谷 | `http://api-audiostream-gg.fengkongcloud.com/finish_audiostream/v4` | 中文音频流 |
| 上海 | `http://api-audiostream-sh.fengkongcloud.com/finish_audiostream/v4` | 中文音频流 |


### <span id = "closeRequestEncode">字符编码：</span>

`UTF-8`


### <span id = "closeRequestMethod">请求方法：</span>

`POST`


### <span id = "closeRequestTimeout">建议超时时长：</span>

1s

### <span id = "closeRequestParameters">请求参数：</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 公司密钥 | Y | 用于权限认证，开通账号服务时由数美提供 |
| requestId | string | 本次请求的唯一标识 | Y | 需要关闭视频流的requestId |


## <span id = "closeResponse">返回参数</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 本次请求的唯一标识 | 是 | 请求唯一标识 |
| code | int | 请求返回码 | 是 | `1100`：成功<br>`1901`：QPS超限<br>`1902`：参数不合法<br>`1903`：服务失败<br>`9101`：无权限操作 |
| message | string | 请求返回描述，和请求返回码对应 | 是 | 枚举值：成功该路流不存在 |

## <span id = "example">示例</span>

### <span id = "uploadRequestExample">上传请求示例：</span>
```
curl -v 'http://api-audiostream-bj.fengkongcloud.com/audiostream/v4' -d '{"accessKey":"xxxxx","appId":"default","eventId":"default","type":"PORN_AD_POLITICAL_GENDER_TIMBRE_ABUSE_SING_LANGUAGE","callback":"xxxxx","streamType":"NORMAL","data":{"btId":"test1","lang":"zh","room":"room2","url":"xxxxx","returnAllText":1,"returnPreText":1,"returnPreAudio":1,"tokenId":"2222"}}'
```


### <span id = "callbackResponseExample">回调返回示例：</span>

```json
{
    "requestId":"db45ca0256775771262c7ca5419ca85f_0",
    "code":1100,
    "message":"成功",
    "btid":"1604285930313",
    "riskLevel":"PASS",
    "riskLabel1":"normal",
    "riskLabel2":"",
    "riskLabel3":"",
    "riskDescription":"正常",
    "riskDetail":{
        "audioEndtime":"2020-11-02 10:59:13",
        "audioStartTime":"2020-11-02 10:59:03",
        "audioText":"坚持爱汇仁牌六味地黄丸会人守护爱数学作业拍一下语文作业拍一下",
        "audioUrl":"http://voice-bj.bj.bcebos.com/20201102/db45ca0256775771262c7ca5419ca85f_0.mp3"
    },
    "businessLabels":{
        "gender":{
            "label":"女性",
            "probability":79
        },
        "language":[
            {
                "label":2,
                "probability":0
            },
            {
                "label":0,
                "probability":99
            },
            {
                "label":1,
                "probability":0
            }
        ],
        "song":0,
        "timbre":[
            {
                "label":"女王",
                "probability":14
            },
            {
                "label":"御姐",
                "probability":13
            },
            {
                "label":"少女",
                "probability":15
            },
            {
                "label":"大妈",
                "probability":11
            },
            {
                "label":"萝莉",
                "probability":44
            }
        ]
    },
    "requestParams":{
        "returnAllText":1,
        "returnPreAudio":1,
        "returnPreText":1,
        "room":"1604285930313",
        "tokenId":"2222",
        "url":"rtmp://58.200.131.2:1935/livetv/hunantv?10"
    }
}
```


###  <span id = "closeRequestExample">关流请求示例：</span>

```
curl -d'{"accessKey":"xxxxx", "requestId": "xxxxx"}' 'http://api-audiostream-bj.fengkongcloud.com/finish_audiostream/v4'
```


### <span id = "closeResponseExample">关流返回示例：</span>

```
{
"code":1100,
"message":"成功",
"requestId":" a78eef377079acc6cdec24967ecde722",
}

```


## <span id = "demo">Demo</span>

目前提供了 go、java、lua、nodes、php、python 的 demo，代码位置：[https://github.com/ishumei/api-demo/tree/master/v4](https://github.com/ishumei/api-demo/tree/master/v4)
