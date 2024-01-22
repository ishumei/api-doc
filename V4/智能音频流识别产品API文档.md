# 智能音频流识别产品API文档


## 音频流上传请求

### 请求URL：

| 集群   | URL                                                           | <span id="language">支持语种</span> |
| ------ | ------------------------------------------------------------- | ---------------------------------------- |
| 新加坡 | `http://api-audiostream-xjp.fengkongcloud.com/audiostream/v4` | 中文、国际化 |
| 硅谷   | `http://api-audiostream-gg.fengkongcloud.com/audiostream/v4`  | 中文、国际化 |
| 上海   | `http://api-audiostream-sh.fengkongcloud.com/audiostream/v4`  | 中文       |

### 请求方法：

`POST`

### 字符编码：

`UTF-8`

### 建议超时时间：

1s

### 拉流重试机制

为防止网络异常导致的拉流失败问题发生，数美音频流服务设置了拉流失败后的重试机制，具体机制如下：

普通rtmp、http、hls流会重试12次，第一次间隔5秒，第二次间隔10秒，以此类推，最大间隔不超过60秒<br/>
通过声网SDK录制方式拉流，重试2次，间隔为0<br/>
通过即构SDK录制方式拉流，重试10次，每次重试间隔30秒

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型**    | **参数说明**   | **是否必传**               | **规范**                                                     |
| -------------- | ----------- | -------------- | -------------------------- | ------------------------------------------------------------ |
| accessKey      | string      | 公司密钥       | Y                          | 由数美提供                                                   |
| appId          | string      | 应用标识       | Y                          | 用于区分应用<br/>需要联系数美服务开通，请使用数美单独提供的传值为准<br/> |
| eventId        | string      | 事件标识       | Y                          | 区分场景数据<br/>需要联系数美服务开通，请使用数美单独提供的传值为准<br/> |
| type           | string      | 检测的风险类型 | businesstype和type必传其一 | 可选值：监管功能<br/>`POLITY`：涉政识别<br/>`EROTIC`：色情识别<br/>`ADVERT`：广告识别<br/>`MOAN`：娇喘识别<br/>`AUDIOPOLITICAL`：一号领导人声纹识别<br/>`ANTHEN`：国歌识别<br/>`DIRTY`：辱骂识别<br/>`ADLAW`：广告法识别<br/>`SING`：唱歌识别<br/>`MINOR`：未成年人识别<br/>`BANEDAUDIO`：违禁歌曲<br/>`VOICE`：人声属性（伪造人声）<br/>如需做组合识别，通过下划线连接即可，例如`POLITY_EROTIC_MOAN`<br/>涉政、色情和娇喘识别，涉政、色情、辱骂、广告识别指的是语义内容的风险检测 |
| businessType   | string      | 业务标签       | businesstype和type必传其一 | 可选值：业务标签的一、二、三级标签<br/>`GENDER`：性别识别<br/>`AGE`：年龄识别<br/>`TIMBRE`：音色识别<br/>`SING`：唱歌识别<br/>`LANGUAGE`：语种识别<br/>`VOICE`：人声属性<br/>`AUDIOSCENE`：声音场景 |
| data           | json_object | 请求的数据内容 | Y                          | 本次请求相关信息，最长1MB,[详见data参数](#data)              |
| callback       | string      | 回调地址       | Y                          | 异步检测结果回调通知您的URL，支持HTTP和HTTPS                 |

<span id="data">其中，data的内容如下：</span>

| **请求参数名** | **类型**    | **参数说明**                                           | **是否必传** | **规范**                                                                                                                                                     |
| -------------- | ----------- | ------------------------------------------------------ | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| tokenId        | string      | 用户账号标识                                           | Y            | 用于区分用户账号，建议传入用户ID                                                                                                                             |
| btId           | string      | 音频唯一标识                     | Y            | 用于查询指定音频，限长128位字符                                                                                                                              |
| streamType     | string      | 流类型                                                 | Y            | 可选值：<br/>`NORMAL`：普通流地址，目前支持rtmp、rtmps、hls、http、https协议,支持flv,m3u8等格式<br/>`ZEGO`：即构<br/>`AGORA`：声网 <br/>`TRTC`：腾讯录制<br/>`VOLC`：火山引擎录制<br/>`GIN`： 巨人录制<br/>注意：使用RTC的SDK录制方案的时候，会在RTC侧产生额外的录制费用，具体费用请咨询相关RTC厂商 |
| url            | string      | 直播流地址                                             | N            | 当streamType为`NORMAL`时必传                                                                                                                                 |
| lang           | string      | 音频流语言类型                                         | Y            | 可选值如下，（默认值为`zh`）：<br/>`zh`：中文<br/>`en`：英文<br/>`ar`：阿拉伯语<br/>`hi`：印地语<br/>`es`：西班牙语<br/>`fr`：法语<br/>`ru`：俄语<br/>`pt`：葡萄牙语<br/>`id`：印尼语<br/>`de`：德语<br/>`ja`：日语<br/>`tr`：土耳其语<br/>`vi`：越南语<br/>`it`：意大利语<br/>`th`：泰语<br/>`tl`：菲律宾语<br/>`ko`：韩语<br/>`ms`：马来语<br/>[集群支持语种详见 请求URL支持语种](#language)，除中文外其他语言类型为国际化 |
| zegoParam      | json_object | 要检测的流参数                                         | N            | 当streamType为`ZEGO`时必传，[详见zegoParam参数](#zegoParam)                                                                                                  |
| initDomain     | int         | 即构SDK初始化是否有设置隔离域名                        | N            | 当即构客户端init初始化支持隔离域名和随机userId该字段必传,可选值：<br/>`0`：默认版本<br/>`1`：仅支持客户端初始化有隔离域名<br/>`2`：支持客户端初始化有隔离域名和随机userId功能<br/>`3`：更新SDK，修复一些bug<br/>`4`：支持客户自定义传入SEI信息<br/>`5`：支持vad静音检测，token会有唯一性校验，每次上传鉴黄必须重新生成<br/>**推荐使用`5`进行接入；** 为兼容老客户使用，默认值为0 |
| trtcParam      | json_object | 腾讯录制参数（当streamType为TRTC时必传），详见扩展参数 | N            | 腾讯录制参数（当streamType为TRTC时必传），详见扩展参数                                                                                                       |
| agoraParam     | json_object | 要检测的声网流参数                                     | N            | 当streamType为`AGORA`时必传,[详见agoraParam参数](#agoraParam)                                                                                                |
| volcParam      | json_object | 要检测的火山流参数                                     | N            | 当streamType为`VOLC`时必传,[详见volcParam参数](#volcParam)                                                                                                |
| ginParam     | json_object | 要检测的巨人流参数                                     | N            | 当streamType为`GIN`时必传,[详见ginParam参数](#ginParam)                                                                                                |
| room           | string      | 直播房间号                                             | N            |                                                                                                                                                              |
| role           | string      | 用户角色                                               | N            | 用户角色对不同角色可配置不同策略。直播领域可取值如下（默认值`USER`普通用户）：<br/>`ADMIN`:房管<br/>`HOST`：主播<br/>`SYSTEM`：系统角色<br/>`USER`：普通用户 |
| returnAllText  | int         | 返回音频片段的等级                                     | N            | 可选值如下（默认为`0`）：<br/>`0`：返回风险等级为非pass的音频片段<br/>`1`：返回所有风险等级的音频片段<br/>建议传入1 （默认为0，在静音的情况下不会产生回调）             |
| returnPreText  | int         | 是否返回违规音频流片段的前文文字信息                   | N            | 可选值如下（默认值为`0`）：<br/>`0`：不返回违规片段前一个片段文字；<br/>`1`：返回违规片段前一个片段文字；                                                  |
| returnPreAudio | int         | 是否返回违规音频流片段的前文音频链接                   | N            | 可选值如下（默认值为`0`）：<br/>`0`：不返回违规片段前一个片段音频；<br/>`1`：返回违规片段前一个片段音频链接；                                              |
| returnFinishInfo | int     | 音频流结束回调通知 | N            | 可选值如下（默认值为`0`）：<br/>`0`：审核结束时不发送结束通知<br/>`1`：审核结束时发起结束通知，回调参数增加statCode状态码<br/>建议传入1（默认值为0，在流结束时不会产生回调）                                                                                                        |
| extra          | json_object | 辅助参数                                               | N            | 用于辅助音频检测的相关信息，[详见extra参数](#extra)                                                                                                          |
| liveTitle      | string      | 标题                                                   | N            | 房间标题，非必填参数，在客户开通人审服务传入                                                                                                                 |
| anchorName     | string      | 昵称                                                   | N            | 用户昵称，非必填参数，在客户开通人审服务传入 |
| audioDetectStep  | int     | 抽帧审核步长                                             | N          | 音频每个步长只会检测一次,取值范围1-36的整数，默认每个片段都审核（备注）<br/>举例：该参数设置为1，会审核第一个片段、第三个片段、第五个片段，以此类推。该参数设置为2，会审核第一个片段、第四个片段、第个七个片段，以此类推。                                                                                                           |

<span id="zegoParam">data中，zegoParam详细内容如下：</span>

| **请求参数名**  | **类型** | **参数说明**                                                 | **是否必传** | **规范**                                        |
| --------------- | -------- | ------------------------------------------------------------ | ------------ | ----------------------------------------------- |
| tokenId         | string   | zego提供的身份验证信息，获取zego的identify_token用于登录，生成方式详见zego文档：[https://doc-zh.zego.im/article/15258](https://doc-zh.zego.im/article/15258) 注意tokenId是唯一标识上传鉴黄每一次请求都需要重新生成新的 | Y            |                                                 |
| streamId        | string   | 音频流编号，唯一对应一路音频流，streamId与roomId至少传入其中之一。 | N            |                                                 |
| roomId          | string   | 房间编号，唯一对应一个房间。                                 |              |                                                 |
| isMixingEnabled | bool     | 录制模式<br/>true：合流，房间内所有用户合成一路流录制审核。此时如果streamId与roomId单独存在时则单独生效；但当streamId与roomId同时存在时，则以streamId为有效值。<br/>false：分流，房间内每个用户单独录制审核。此时roomId为必传，且以roomId为有效值。 | N            | 默认值为`true`<br/>`true`:合流<br/>`false`:分流 |

<span id="agoraParam">data中，agoraParam详细内容如下：</span>

| **请求参数名**  | **类型** | **参数说明**                                                 | **是否必传** | **规范**                                                |
| --------------- | -------- | ------------------------------------------------------------ | ------------ | ------------------------------------------------------- |
| appId           | string   | 声网提供的appId，注意与数美的appId区分开                     | Y           | 非数美的appId                                           |
| channel         | string   | 声网提供的频道名                                             | Y           |                                                         |
| token           | string   | 安全要求较高的用户可以使用 token进行认证，生成方式详见声网文档：[https://docs.agora.io/cn/Recording/token\_server?platform=CPP](https://docs.agora.io/cn/Recording/token\_server?platform=CPP) <br/>建议将token的有效期设置超过频道的持续时间，防止token失效导致无法拉流。当前声网支持的最大token有效期为24小时，因此当频道持续时间超过24小时的时候，需要处理token失效的问题。处理方法：在请求参数中设置开启音频流结束回调通知（设置returnFinishInfo为1）。当回调接收到审核结束通知（statCode为1），并且原因是由于拉流的token无效或过期（auxInfo下errorCode状态码返回3005），如果频道仍然存在并且需要继续审核，则生成新的token，将频道重新送审。 | N            |                                                         |
| uid             | int      | 用户 ID，当token存在时，必须提供生成token时所使用的用户ID。注意，此处需要区别实际房间中的用户uid，提供给服务端录制所用的uid不允许在房间中存在 | N            | 32 位无符号整数                                         |
| isMixingEnabled | bool     | 单流/合流录制<br/>合流是指一个直播房间一路流<br/>分流是指一个麦位一路流， | N            | 默认值为`true`<br/>`true`:合流<br/>`false`:分流         |
| channelProfile  | int      | 声网录制的频道模式<br/>通信，常见的 1 对 1 单聊或群聊，频道内任何用户可以自由说话<br/>直播，有两种用户角色: 主播和观众 | N            | 可选值如下（默认值为`0`）：<br/>`0`: 通信<br/>`1`: 直播 |
| subscribeMode  | string      | 订阅模式:<br/>`AUTO`: 自动订阅房间内的所有流，不设置subscribeMode时候的默认行为<br/>`UNTRUSTED`: 配合`untrustedUserIdList`只订阅该列表指定的用户流，仅在声网分流里生效<br/>`TRUSTED`: 配合`trustedUserIdList`只订阅该列表以外的用户流，仅在声网分流中生效 |||
| trustedUserIdList  | string_array      | 信任用户的列表，subscribeMode=TRUSTED时生效，允许为空，数美不会订阅房间内该列表指定的用户流。其中每个元素uid的范围应为uint32的范围，但类型为string。例如：["123","456"] |||
| untrustedUserIdList  | string_array      | 非信任用户的列表，subscribeMode=UNTRUSTED时生效，不允许为空，数美只订阅房间内该列表指定的用户流。其中每个元素uid的范围应为uint32的范围，但类型为string。例如：["123","456"] |||


<span id="ginParam">data中，ginParam详细内容如下：</span>

| **请求参数名**  | **类型** | **参数说明**                                                                                                                                                                                   | **是否必传** | **规范**                                                |
| --------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ------------------------------------------------------- |
| tokenId           | string   | 房间token，用于拉流端登陆房间，需要巨人提供	 | Y            |                                                         |
| roomId         | string   | 房间编号，唯一对应一个房间，服务端以房间为单位拉流录制		                                                                                                                                                                               | Y            |                                                         |
| isMixingEnabled | bool     | 单流/合流录制<br/>合流是指房间内所有用户合成一路流录制审核<br/>分流是指房间内每个用户单独录制审核                                                                                                                      | Y            | 默认值为`true`<br/>`true`:合流<br/>`false`:分流         |
| ip         | string   | 指定服务器ip		                                                                                                                                                                               | Y            |                                                         |
| port         | string   | 指定端口		                                                                                                                                                                               | Y            |                                                         |

其中 data.trtcParam内容如下

| 参数名称   | 类型   | **是否必传** | 说明                                                                                                                                     |
| ---------- | ------ | ------------ | ---------------------------------------------------------------------------------------------------------------------------------------- |
| sdkAppId   | int    | Y            | 腾讯提供的sdkAppId                                                                                                                       |
| demoSences | int    | Y            | 录制类型可选值：分流录制：2 合流录制：4                                                                                                  |
| userId     | string | Y            | 分配给录制端的userId，限制长度为32bit，只允许包含（a-zA-Z），数字(0-9)以及下划线和连词符                                                 |
| uid     | string | N            | 指定需要审核的用户ID，如果不传该参数，则默认拉取并审核房间中所有推流用户的流。如果需要审核同一房间内的一部分用户，**请使用不同的录制端userId和userSig分多次请求。 注意区分此参数和userId的区别**。                            |
| userSig    | string | Y            | 录制userId对应的验证签名，相当于登录密码                                                                                                 |
| roomId     | int    | Y            | 房间号码，取值范围：【1-4294967294】roomId与strRoomId必传一个，若两者都有值优先选用roomId                                                |
| strRoomId  | string | Y            | 房间号码 取值说明：只允许包含（a-zA-Z），数字(0-9)以及下划线和连词符若您选用strRoomId时，需注意strRoomId和roomId两者都有值优先选用roomId |

trtc流会根据上述`sdkAppId`、`userId`、`roomId`或`strRoomId`进行去重

<span id="volcParam">data中，volcParam详细内容如下：</span>

| **请求参数名**  | **类型** | **参数说明**                                                                                                                                                                                   | **是否必传** | **规范**                                                |
| --------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ------------------------------------------------------- |
| appId           | string   | 火山引擎提供的appId，注意与数美的appId区分开                                                                                                                                                       | Y            | 非数美的appId                                           |
| roomId          | string   | 房间号                                                                                                                                                                               | Y            |                                                         |
| token           | string   | 火山引擎token，详见：[https://www.volcengine.com/docs/6348/70121](https://www.volcengine.com/docs/6348/70121) | Y            |                                                         |
| userId          | string   | 分配给录制端的userId                                                 | Y            |                                          |
| subscribeMode          | string   | 订阅模式:<br/>AUTO: 自动订阅房间内的所有流，不设置subscribeMode时候的默认行为。<br/>UNTRUSTED: 配合untrustedUserIdList只订阅该列表指定的用户流，此种模式下如果untrustedUserIdList列表为空，参数错误，因为无法订阅任何流。<br/>TRUSTED: 配合trustedUserIdList只订阅该列表以外的用户流，此种模式下如果一定时间下没有untrustedUserIdList名单外的用户进入房间，数美将主动结束审核。                                                 | N            |                                          |
| trustedUserIdList          | string_array   | 信任用户的列表，subscribeMode=TRUSTED时生效，允许为空，数美不会订阅房间内该列表指定的用户流。                                                 | N            |                                          |
| untrustedUserIdList          | string_array   | 非信任用户的列表，subscribeMode=UNTRUSTED时生效，不允许为空，数美只订阅房间内该列表指定的用户流。                                                 | N            |                                          |

<span id="extra">data中，extra的内容如下：</span>

| **请求参数名** | **类型**    | **参数说明** | **是否必传** | **规范**                         |
| -------------- | ----------- | ------------ | ------------ | -------------------------------- |
| passThrough    | json_object | 透传字段     | N            | 该字段内容会随着回调结果一起返回 |

## 同步返回结果

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明**       | **是否必返** | **规范**                                                                                            |
| ------------------ | ------------ | ------------------ | ------------ | --------------------------------------------------------------------------------------------------- |
| requestId          | string       | 本次请求的唯一标识 | Y           | 请求唯一标识                                                                                        |
| code               | int          | 请求返回码         | Y            | `1100`：成功<br/>`1901`：QPS超限、流路数超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1904`：拉流失败<br/>`9101`：无权限操作 |
| message            | string       | 请求返回描述       | Y          | 和请求返回码对应                                                                                    |
| detail             | json_object  | 描述详细信息       | N           | 描述错误码请求，[详见detail参数](#detail)                                                           |

<span id="detail">其中，detail结构如下：</span>

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **说明**                                                                                                                                                                                                                                                           |
| ------------ | ------------ | ------------ | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| errorcode    | int          |              | N           | 状态码<br/>`1001`：重复推流                                                                                                                                                                                                                                        |
| dupRequestId | string       |              | N           | 表示重复的requestId<br/>当errorCode为1001，表示重复推流时，会返回dupRequestId字段<br/>例如当第一次请求的时候没有收到返回，但该音频流实际已经开始审核了，没有requestId无法主动关闭审核<br/>可以再次请求，收到重复推流的信息，通过返回的dupRequestId调用关闭审核接口 |

## 回调返回结果

当音频流稳定接入开始接受数美监测后，数美会持续回调识别结果给客户，回调策略根据returnAllText的取值不同而不同。

returnAllText为`0`时，监测到音频流有违规内容时回调结果给客户；

returnAllText为`1`时，每隔10秒返回一次最近10秒的识别结果给客户。

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名**  | **类型**    | **参数说明**                   | **是否必返** | **规范**                                                                                                                                                      |
| ----------- | ----------- | ------------------------------ | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| requestId   | string      | 本次请求的唯一标识             | Y           |                                                                                                                                                               |
| btId        | string      | 音频唯一标识                   | Y           |                                                                                                                                                               |
| code        | int         | 请求返回码                     | Y           | `1100`：成功<br/>`1901`：QPS超限、流路数超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1904`：拉流失败<br/>`9101`：无权限操作, message和requestId之外的字段，只有当code为1100时才会存在 |
| message     | string      | 请求返回描述，和请求返回码对应 | Y           |                                                                                                                                                               |
| statCode     | int        | 审核状态                   | N            | <p>0 ：审核中</p><p>1 ：审核结束</p>                                                                                                                                   |
| audioDetail | json_object | 风险音频片段信息               | N           | 当code等于`1100`时返回，[详见audioDetail参数](#audioDetail)                                                                                                   |
| auxInfo     | json_object | 辅助信息                       | N           |                                                                                                                                                          |

<span id="audioDetail">其中，audioDetail结构如下：</span>

| **参数名**      | **类型**    | **参数说明**           | **是否必返** | **规范**                                                                                                                                         |
| --------------- | ----------- | ---------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| audioUrl        | string      | 音频片段地址           | Y           |                                                                                                                                                  |
| riskLevel       | string      | 当前事件的处置建议     | Y           | `PASS`：通过<br/>`REVIEW`：审核<br/>`REJECT`：拒绝                                                                                               |
| riskLabel1      | string      | 一级标签               | Y           | 各个一级标签之间是并列的关系，当riskLevel为`PASS`时返回`normal`                                                                                  |
| riskLabel2      | string      | 二级标签               | Y           | 二级标签归属于一级标签，当riskLevel为`PASS`时为空                                                                                                |
| riskLabel3      | string      | 三级标签               | Y           | 三级标签归属于二级标签，当riskLevel为`PASS`时为空                                                                                                |
| riskDescription | string      | 标签解释               | Y           | 对于命中用户自定义名单时返回：`命中自定义名单`；<br/>当riskLevel为`PASS时返回`正常`；<br/>其他情况展现形式为一级标签：二级标签：三级标签的中文名，仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |
| audioText       | string      | 音频转译文本的结果     | N           | 当returnPreAudio值为`1`时，包含违规音频前一个片段文本内容和违规音频片段文本内容；<br/>当returnPreAudio值为`0`时，包含违规音频片段文本内容        |
| preAudioUrl     | string      | 前一个音频片段音频地址 | N           | 当returnPreAudio值为`1`时，返回的是当前片段与前一片段的20秒音频片段地址；<br/>returnPreAudio值为`0`时，不返回   |
| riskDetail      | json_object | 风险详情信息           | N          | 当code等于`1100`时返回，[详见riskDetail参数](#riskDetail)                                                                                        |
| auxInfo         | json_object | 其他辅助信息           | Y           | 返回时间戳等辅助信息，[详见auxInfo参数](#auxInfo)                                                                                                |
| businessLabels  | json_array  | 音频业务标签           | N           | 返回性别、音色、是否唱歌等标签，[详见businessLabels参数](#businessLabels)                                                                        |
| allLabels       | json_array  | 风险标签               | N           | 全部风险标签，[详见allLabels参数](#allLabels)                                                                                                    |
| riskSource      | int         | 风险来源               | Y           | 可选值：<br/>1000：无风险<br/>1001：文本风险<br/>1003：音频风险                                                                                            |
| tokenProfileLabels  | json_array  | 账号属性标签           | N           | 仅在开启功能时返回[详见tokenProfileLabels，tokenRiskLabels参数](#tokenRiskLabels)                                                                        |
| tokenRiskLabels  | json_array  | 账号风险标签           | N           | 仅在开启功能时返回[详见tokenProfileLabels，tokenRiskLabels参数](#tokenRiskLabels)                                                                        |
| speakers         | json_array | 该音频片段说话人信息     |N       | 该音频片段中说话人uid以及音量信息，每秒采集一次，一个片段不超过10次。<br/>该结构是个数组，最多10个元素，按照相对时间排序，每个元素也是一个数组，包含当前说话人uid和音量大小<br/>备注：目前仅在声网合流中生效 |
| vadCode          | int | 该音频片段的静音状态     |N       | 静音状态：<br/>`0` ：静音片段<br/>`1` ：非静音片段 |

其中，auxInfo结构如下：

| **参数名**        | **类型** | **参数说明**     | **是否必返** | **规范**                                                                                        |
| ----------------- | -------- | ---------------- | ------------ | ----------------------------------------------------------------------------------------------- |
| audioStartTime  | string      | 辅助参数         | Y          | 违规内容开始时间（绝对时间）                                                                    |
| audioEndTime | string      | 辅助参数         | Y           | 违规内容结束时间（绝对时间）                                                                    |
| beginProcessTime  | int      | 辅助参数         | Y           | 开始处理的时间（13位时间戳）                                                                    |
| finishProcessTime | int      | 辅助参数         | Y          | 结束处理的时间（13位时间戳）                                                                    |
| userId            | int      | 用户账号标识 | N           | AGORA分流情况下存在。返回的userId是实际房间中的用户id，与请求参数agoraParam中的uid无关。 |
| strUserId         | string   | 用户账号标识 | N           | TRTC、ZEGO、VOLC、GIN分流情况下存在。返回的strUserId是实际房间中的用户id。当流类型为TRTC分流，与请求参数trtcParam中的uid无关。 |
| room              | string   | 房间号           | N           |                                                                                                 |
| seiInfo           | array    | SEI信息          | N           | （需要联系数美开通）                                                                            |
| passThrough       | json_object | 透传字段       | N           | 该字段内容与请求参数data中extra的passThrough的值相同 |

<span id="riskDetail">其中，riskDetail结构如下：</span>

| **参数名**   | **类型**   | **参数说明**             | **是否必返** | **规范**                                                                                |
| ------------ | ---------- | ------------------------ | ------------ | --------------------------------------------------------------------------------------- |
| audioText    | string     | 音频转译文本的结果       | N           |                                                                                         |
| matchedLists | json_array | 命中的客户自定义名单信息 | N           | 命中客户自定义名单时返回，其他时不存在，[详见matchedLists参数](#matchedLists)           |
| riskSegments | json_array | 高风险内容片段           | N           | 在涉政、暴恐、违禁、竞品、广告法等功能的时候存在，[详见riskSegments参数](#riskSegments) |

<span id="matchedLists">其中，matchedLists结构如下：</span>

| **参数名** | **类型**   | **参数说明**                 | **是否必返** | **规范**                                 |
| ---------- | ---------- | ---------------------------- | ------------ | ---------------------------------------- |
| name       | string     | 客户自定义名单名             | Y           |                                          |
| words      | json_array | 命中的这个名单中的敏感词信息 | Y           | 下标从0开始计数，[详见words参数](#words) |

<span id="words">其中，words结构如下：</span>

| **参数名** | **类型**  | **参数说明**   | **是否必返** | **规范**        |
| ---------- | --------- | -------------- | ------------ | --------------- |
| word       | string    | 敏感词         | Y           |                 |
| position   | int_array | 敏感词所在位置 | Y           | 下标从0开始计数 |

<span id="riskSegments">其中，riskSegments结构如下：</span>

| **参数名** | **类型**  | **参数说明**           | **是否必返** | **规范**        |
| ---------- | --------- | ---------------------- | ------------ | --------------- |
| segment    | string    | 高风险内容片段         | N           |                 |
| position   | int_array | 高风险内容片段所在位置 | N           | 下标从0开始计数 |

其中，businessLabels结构如下：

| **参数名**          | **类型** | **参数说明** | **是否必返** | **规范**     |
| ------------------- | -------- | ------------ | ------------ | ------------ |
| businessLabel1      | string   | 一级业务标签 | N           | 一级业务标签 |
| businessLabel2      | string   | 二级业务标签 | N           | 二级业务标签 |
| businessLabel3      | string   | 三级业务标签 | N           | 三级业务标签 |
| businessDescription | string   | 中文标签描述 | N           | 业务标签描述，仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |

<span id="tokenRiskLabels">其中，tokenProfileLabels，tokenRiskLabels 结构如下</span>

| **参数名**          | **类型** | **参数说明** | **是否必返** | **规范**                                 |
| ------------------- | -------- | ------------ | ------------ |----------------------------------------|
| label1      | string   | 一级标签     | N           |                                        |
| label2      | string   | 二级标签     | N           |                                        |
| label3      | string   | 三级标签     | N           |                                        |
| description | string   | 标签描述     | N           | 账号标签描述，仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |
| timestamp   | int      | 打标签时间戳 | N           | 13位Unix时间戳，单位：毫秒                       |

<span id="speakers">其中，speakers 是个**二维数组**，每个元素的结构如下</span>

| **参数名**          | **类型** | **参数说明** | **是否必返** | **规范**                                 |
| ------------------- | -------- | ------------ | ------------ |----------------------------------------|
| uid         | int   | 说话人uid     | Y           |                                        |
| volume      | int   | 音量大小      | Y           |      取值范围为 [0,255]                                  |

<span id="allLabels">allLabels结构如下：</span>

| **参数名**      | **类型** | **参数说明** | **是否必返** | **规范**     |
| --------------- | -------- | ------------ | ------------ | ------------ |
| riskLabel1      | string   | 一级风险标签 | Y           | 一级风险标签 |
| riskLabel2      | string   | 二级风险标签 | Y           | 二级风险标签 |
| riskLabel3      | string   | 三级风险标签 | Y           | 三级风险标签 |
| riskDescription | string   | 风险原因     | Y           | 风险原因，仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理     |

其中最外层的auxInfo字段结构如下：

| **参数名**      | **类型**    | **参数说明**           | **是否必返** | **规范**                                                                                                                                         |
| --------------- | ----------- | ---------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| errorCode       | int         | 状态码           | Y           | <p>状态码</p><p>3001：流地址访问失败，例如资源HTTP状态码404、403</p><p>3002：流数据无效，例如“Invalid data found when processing input”</p><p>3003：流不存在，例如zego返回197612错误码</p><p>3004：流未返回音频数据</p><p>3005：拉流token无效或过期，建议使用新token重新开启审核，例如声网token过期或者trtc usersig无效</p>  |
| streamTime       | int         | 流审核时长           | N           | 流结束后最后一次返回，代表送审时长，如有间隔审核逻辑时，和流真实时长可能不一致  |

## 音频流关闭通知接口

### 接口描述

该接口用于客户端通知服务端某个音频流已关闭。

### 请求URL：

| 集群   | URL                                                                  | 支持产品列表                             |
| ------ | -------------------------------------------------------------------- | ---------------------------------------- |
| 新加坡 | `http://api-audiostream-xjp.fengkongcloud.com/finish_audiostream/v4` | 中文音频流<br/>英语音频流<br/>阿语音频流 |
| 硅谷   | `http://api-audiostream-gg.fengkongcloud.com/finish_audiostream/v4`  | 中文音频流                               |
| 上海   | `http://api-audiostream-sh.fengkongcloud.com/finish_audiostream/v4`  | 中文音频流                               |

### 字符编码：

`UTF-8`

### 请求方法：

`POST`

### 建议超时时长：

1s

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明**       | **是否必传** | **规范**                               |
| -------------- | -------- | ------------------ | ------------ | -------------------------------------- |
| accessKey      | string   | 公司密钥           | Y            | 用于权限认证，开通账号服务时由数美提供 |
| requestId      | string   | 本次请求的唯一标识 | Y            | 需要关闭音频流的requestId              |

## 返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明**                   | **是否必返** | **规范**                                                                                            |
| ------------------ | ------------ | ------------------------------ | ------------ | --------------------------------------------------------------------------------------------------- |
| requestId          | string       | 本次请求的唯一标识             | Y           | 请求唯一标识                                                                                        |
| code               | int          | 请求返回码                     | Y           | `1100`：成功<br/>`1901`：QPS超限、流路数超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1904`：拉流失败<br/>`9101`：无权限操作 |
| message            | string       | 请求返回描述，和请求返回码对应 | Y           | 枚举值：成功该路流不存在                                                                            |

## 示例

### 上传请求示例：

```bash
curl -v 'http://api-audiostream-sh.fengkongcloud.com/audiostream/v4' -d '{
    "accessKey": "xxxxx",
    "appId": "default",
    "eventId": "default",
    "type": "EROTIC_ADVERT_POLITY_DIRTY",
    "businessType":"GENDER_TIMBRE_SING_LANGUAGE",
    "callback": "xxxxx",
    "streamType": "NORMAL",
    "data": {
        "btId": "test1",
        "lang": "zh",
        "room": "room2",
        "url": "xxxxx",
        "returnAllText": 1,
        "returnPreText": 1,
        "returnPreAudio": 1,
        "tokenId": "2222"
    }
}'
```

### 同步返回示例：

```json
{
    "code":1100,
    "message":"成功",
    "requestId":"b639042cbfe229359e672074762c5583"
}
```


### 回调返回示例：

```json
{
    "requestId":"b639042cbfe229359e672074762c5583_2",
    "btid":"1637847086612",
    "code":1100,
    "message":"成功",
    "audioDetail":{
        "audioTags":{
            "gender":{
                "label":"男性",
                "probability":99
            },
            "language":[
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
                    "label":"大叔",
                    "probability":7
                },
                {
                    "label":"青年",
                    "probability":34
                },
                {
                    "label":"老年",
                    "probability":58
                },
                {
                    "label":"正太",
                    "probability":0
                }
            ]
        },
        "audioText":"那就不好打吗？所以所以他小龙让掉也是合情合理。还要看这条，先锋啊，下一个节奏点可能就是个先锋，但这个先锋的时候，苏宁其实是可以打正面团战了，谢谢毛主任一直都",
        "audioUrl":"https://bj-voice-mp3-1251671073.cos.ap-beijing.myqcloud.com/MP3%2F20211125%2Fb639042cbfe229359e672074762c5583_2.mp3?q-sign-algorithm=sha1&q-ak=AKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW&q-sign-time=1637847114%3B1637847234&q-key-time=1637847114%3B1637847234&q-header-list=host&q-url-param-list=&q-signature=c0f9a66334bc00e1d92da40974756b9b4b9e7b26",
        "auxInfo":{
            "beginProcessTime":1637847113897,
            "finishProcessTime":1637847114514,
            "room":"test1"
        },
        "businessLabels":[
            {
                "businessDescription":"未成年人:未成年人:未成年人",
                "businessLabel1":"minor",
                "businessLabel2":"weichengnianren",
                "businessLabel3":"weichengnianren",
                "confidenceLevel":0,
                "probability":0
            }
        ],
        "preAudioUrl":"https://bj-voice-mp3-1251671073.cos.ap-beijing.myqcloud.com/MP3%2F20211125%2Fb639042cbfe229359e672074762c5583_2_pre.mp3?q-sign-algorithm=sha1&q-ak=AKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW&q-sign-time=1637847114%3B1637847234&q-key-time=1637847114%3B1637847234&q-header-list=host&q-url-param-list=&q-signature=3a261ca2e46b32ec218be69a5802ec0e04c2c627",
        "riskDescription":"正常",
        "riskDetail":{
            "audioText":"那就不好打吗？所以所以他小龙让掉也是合情合理。还要看这条，先锋啊，下一个节奏点可能就是个先锋，但这个先锋的时候，苏宁其实是可以打正面团战了，谢谢毛主任一直都"
        },
        "riskLabel1":"normal",
        "riskLabel2":"normal",
        "riskLabel3":"normal",
        "riskLevel":"REJECT",
        "speakers":[
            [
                {
                    "uid":2,
                    "volume":100
                },
                {
                    "uid":1,
                    "volume":255
                },
                {
                    "uid":3,
                    "volume":50
                }
            ],
            [
                {
                    "uid":2,
                    "volume":200
                },
                {
                    "uid":3,
                    "volume":50
                }
            ],
            [
                {
                    "uid":4,
                    "volume":255
                }
            ]
        ]
    }
}
```

###  关流请求示例：

```bash
curl -v 'http://api-audiostream-sh.fengkongcloud.com/finish_audiostream/v4' -d '{
    "accessKey": "xxxxx",
    "requestId": "xxxxx"
}'
```

### 关流返回示例：

```json
{
    "code": 1100,
    "message": "成功",
    "requestId": " a78eef377079acc6cdec24967ecde722",
}
```

当关闭的流不存在时：

```json
{
    "code": 1909,
    "message": "该路流不存在",
    "requestId": " a78eef377079acc6cdec24967ecde722",
}
```


# FAQ

## 调用接口返回参数错误（1902）

答：调用数美接口时，code返回1902参数不合法，一般为客户输入的参数格式存在问题，客户可自行分析一下请求格式是否按照接口文档输入，或将请求的数据及返回数据反馈给数美分析解决。

## 调用接口返回无权限操作（9101）

答：调用数美接口时，code返回9101无权限操作，一般为调用了未开通的服务，沟通确认客户调用的服务接口，开通相应的服务。

## 调用接口超时问题

答：有如下两个常见问题：

1）DNS问题:

客户通过公网调用数美接口进行测试，客户DNS解析域名较慢，导致第一次请求超时，建议客户更换DNS，不建议客户在host中将域名和ip做绑定，数美更换接口IP导致无法请求接口。

2）网络问题:

客户通过公网调用数美接口，公网网络延迟较长，导致少量请求存在超时。可以建议客户ping数美不同的集群网络，建议客户接入网络延迟较低的数美集群。

## 数美接口支持哪些网络协议？

数美音频流测试接口支持http、https、RTMP、HLS、HDL(HTTP-FLV)、RTP等所有主流网络协议。
