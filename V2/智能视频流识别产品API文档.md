# 数美智能视频流识别产品API文档

- - - - -

***版权所有 翻版必究***

- - - - -

## 视频流上传请求

### 接口描述

该接口用于提交视频流鉴定等相关信息，稳定拉流后将持续回调对应的识别结果至指定的callback地址。

### 请求URL：
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 上海 | `http://api-videostream-sh.fengkongcloud.com/v3/saas/anti_fraud/videostream` | 中文视频流 |
| 新加坡 | `http://api-videostream-xjp.fengkongcloud.com/v3/saas/anti_fraud/videostream` | 中文视频流 |
| 硅谷 | `http://api-videostream-gg.fengkongcloud.com/v3/saas/anti_fraud/videostream` | 中文视频流 |
| 印度 | `http://api-videostream-yd.fengkongcloud.com/v3/saas/anti_fraud/videostream` | 中文视频流 |

### 请求方法：

`POST`

### 支持协议

`HTTP`或`HTTPS`

### 字符编码：

`UTF-8`

### 建议超时时间：

3s

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 公司密钥 | 必传参数 | 数美分配 |
| appId | string | 应用标识 | 必传参数 | 该参数传递值可与数美协商 |
| imgType | string | 视频中的画面需要识别的监管类型，**和imgBusinessType至少传一个** | 非必传参数 | 监管一级标签<br/>可选值：<br/>`POLITICS`：涉政识别<br/>`PERSON`：涉政人物识别<br/>`VIOLENCE`：暴恐识别<br/>`PORN`：色情识别<br/>`AD`：广告识别<br/>`OCR`：图片中的文字风险识别<br/>`PORTRAIT`：识别坐姿<br/>`BUSINESSRISK`：行业违规<br/>如果需要识别多个功能，通过下划线连接，如`AD_PORN_POLITICS`用于广告、色情和涉政组合识别 |
| audioType | string | 视频流中的音频需要识别的监管类型，**和audioBusinessType至少传一个** | 非必传参数 | 监管一级标签<br/>可选值：<br/>`POLITICAL`：涉政识别<br/>`PORN`：色情识别<br/>`AD`：广告识别<br/>`MOAN`：娇喘识别<br/>`SING`：唱歌识别<br/>`ANTHEN`：国歌识别<br/>`ABUSE`: 辱骂识别<br/>`LANGUAGE`：语种识别<br/>`AUDIOPOLITICAL`：声音涉政<br/>`NONE`:不检测音频<br/>如需做组合识别，通过下划线连接即可，例如`POLITICAL_PORN_MOAN`用于、色情和娇喘识别 |
| imgBusinessType | string | 视频中的画面需要识别的业务类型，**和imgType至少传一个** | 非必传参数 | 可选值参考[imgBusinessType可选值列表](#imgBusinessType可选值列表)<br/> |
| audioBusinessType | string | 视频流中的音频需要识别的业务类型，**和audioType至少传一个** | 非必传参数 | 业务一级标签<br/>可选值：<br/>`SING`：唱歌识别<br/>`LANGUAGE`：语种识别<br/>`MINOR`：未成年人识别<br/>`GENDER`：性别识别<br/>`TIMBRE`：音色识别，需要同时传入`GENDER`才能生效<br/>`APPNAME`：app名称识别 |
| imgCallback | string | 图片回调地址 | 必传参数 | 将视频流中截帧图片的检测结果通过该地址回调给用户 |
| audioCallback | string | 音频回调地址 | 非必传参数 | 将视频流中音频片段的检测结果通过该地址回调给用户；需要识别音频时必传 |
| data | json_object | 请求数据内容， | 必传参数 | 最长1MB，其中[data内容如下](#data) |

<span id="data">其中，data的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| tokenId | string | 客户端用户账号唯一标识 | 必传参数 | 用于用户行为分析，建议传入用户UID； 最长40位 |
| streamType | string | 视频流类型 | 必传参数 | 可选值为：<br/>`NORMAL`：普通流地址，目前支持`rtmp`、`rtmps`、`hls`、`http`、`https`协议<br/>`AGORA`：声网审核<br/>`TRTC`:腾讯审核<br/>`ZEGO`：即构审核 <br/>`VOLC`：火山引擎审核 <br/>注意：使用RTC的SDK录制方案的时候，可能会在RTC侧产生额外的录制费用，具体费用请咨询相关RTC厂商 |
| agoraParam | json_object | 声网流参数 | 非必传参数 | 要检测的声网流参数（当streamType为`AGORA`时必传），详见[agoraParam说明](#agoraParam) |
| trtcParam | json_object | 腾讯流参数 | 非必传参数 | 要检测的TRTC流参数（当streamType为`TRTC`时必传），详见[trtcParam说明](#trtcParam) |
| zegoParam | json_object | 即构流参数 | 非必传参数 | 要检测的即构流参数（当streamType为`ZEGO`时必传)，详见[zegoParam说明](#zegoParam) |
| volcParam | json_object | 火山流参数 | 非必传参数 | 要检测的火山流参数（当streamType为`VOLC`时必传)，详见[volcParam说明](#volcParam) |
| url | string | 要检测的视频url地址 | 非必传参数 | 要检测的流地址url参数（当streamType为NORMAL时必传） |
| streamName | string | 视频流名称 | 非必传参数 | 用于后台界面展示，建议传入 |
| ip | string | 客户端IP | 非必传参数 | 该参数用于IP维度的用户行为分析，同时可用于比对数美IP黑库 |
| audioDetectStep | int | 视频流中音频的审核步长 | 非必传参数 | 单位为个，取值范围为1-36整数，取1表示跳过一个10S的音频片段审核，取2表示跳过二个，以此类推。不使用该功能时音频内容全部过审 |
| returnAllImg | int | 用户可根据需求选择返回不同审核结果的图片 | 非必传参数 | 可选值如下：（默认值为`0`）<br/>`0`：回调reject、review结果的图片审核信息<br/>`1`：回调所有结果的图片审核信息 |
| returnAllText | bool | 返回音频流片段识别结果的风险等级 | 非必传参数 | 可选值如下：(默认值为`false`)<br/>`false`：返回风险等级为非pass的音频片段与文本内容<br/>`true`：返回所有风险等级的音频片段与文本内容 |
| returnPreText | bool | 为true表示返回前10秒和当前10秒共20秒音频片段的文本内容| 非必传参数 | 可选值如下：（默认值为`false`）<br/>`true`:返回的content字段包含违规音频前10秒文本内容<br/>`false`:返回的content字段只包含违规音频片段文本内容 |
| returnPreAudio | bool |  为true表示返回前10秒和当前10s共20秒的音频片段链接 | 非必传参数 | 可选值如下：（默认值为`false`）<br/>`true`:返回违规音频前10秒音频链接<br/>`false`:只返回违规片段音频链接 |
| returnFinishInfo | bool |为true时，流结束时返回结束通知 | 非必传参数 | 可选值如下：（默认值为`false`）<br/>`true`:审核结束时发起结束通知<br/>`false`:审核结束时不发送结束通知 ，详细返回参数见[结束流返回参数](#审核结束回调参数（returnFinishInfo为true时返回）：) |
| detectFrequency | float | 截帧频率间隔 | 非必传参数 | 单位为秒，取值范围为1~60s；如不传递默认3s截帧一次 |
| detectStep | int | 视频流截帧图片检测步长 | 非必传参数 | 已截帧图片每个步长只会检测一次，取值大于等于1。 |
| channel | string | 渠道标识 | 非必传参数 | 用户根据不同业务场景，选配不同的渠道 |
| room | string | 直播间/游戏房间编号 | 非必传参数 | 可针对单个房间制定不同的策略； |
| liveTitle | string | 直播标题 | 非必传参数 | 直播标题，一般用于人审需要字段|
| liveCover | string | 直播封面 | 非必传参数 | 直播封面，一般用于人审需要字段|
| anchorName | string | 主播名称 | 非必传参数 | 主播名称，一般用于人审需要字段|

<span id="agoraParam">其中，agoraParam内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| appId | string | 声网提供的应用标识 | 必传参数 | |
| channel | string | 声网提供的频道名 | 必传参数 |	|
| channelKey | string | | 非必传参数 | 安全要求较高的用户可以使用 ChannelKey，<br/>[获取方式详见声网文档ChannelKey生成方式](https://docs.agora.io/cn/Interactive%20Broadcast/token_server?platform=All%20Platforms "声网channelKey说明地址")|
| channelProfile | int | 声网录制的频道模式 | 否 | 可选值如下：（默认值为`0`）<br/>`0`: 通信（默认）,即常见的 1 对 1 单聊或群聊，<br/>频道内任何用户可以自由说话；<br/>`1`: 直播，有两种用户角色: 主播和观众。 |
| uid | int | 用户ID | 非必传参数 | 32位无符号整数。当channelKey存在时，<br/>必须提供生成channelKey时所使用的用户ID。<br/>注意，此处需要区别实际房间中的用户uid，<br/>提供给服务端录制所用的uid不允许在房间中存在 |
| subscribeMode | string | 订阅模式 | 非必传参数 | `AUTO`: 自动订阅房间内的所有流，不设置subscribeMode时候的默认行为<br/>`UNTRUSTED`: 配合untrustedUserIdList只订阅该列表指定的用户流，此种模式下如果untrustedUserIdList列表为空，参数错误，因为无法订阅任何流<br/>`TRUSTED`: 配合trustedUserIdList只订阅该列表以外的用户流，此种模式下如果一定时间下没有untrustedUserIdList名单外的用户进入房间，数美将主动结束审核。 |
| trustedUserIdList | int_array | 信任用户的列表 | 非必传参数 | subscribeMode为`TRUSTED`时生效，不允许为空，数美不会订阅房间内该列表指定的用户流<br/>逗号拼接的UID数组，如`[1,2]`，用户上限17个 |
| untrustedUserIdList | int_array | 非信任用户的列表 | 非必传参数 | subscribeMode为`UNTRUSTED`时生效，不允许为空，数美只订阅房间内该列表指定的用户流<br/>逗号拼接的UID数组，如`[1,2]`，用户上限17个 |

参数使用说明：若您需要对同一个房间中的不同用户进行审核，请您在每次传入时生成不同的uid，以防止审核同一房间内不同用户出现拉流互踢的现象；若您没有uid字段（不使用token的情况），您可以考虑传入相应的订阅模式参数：subscribeMode，当不传此字段或者传此字段为AUTO，自动订阅房间内的所有流，当传此字段为TRUSTED，请传入相应的trustedUserIdList，数美不会订阅房间内该列表指定的用户流，当传此字段为UNTRUSTED，请传入相应的untrustedUserIdList，数美只订阅房间内该列表指定的用户流，我们会根据此字段subscribeMode配合相应列表筛选特定用户进行审核。

<span id="trtcParam">其中，trtcParam内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| sdkAppId | int | Y | 必传参数 | 腾讯提供的sdkAppId |
| demoSences | int | Y | 必传参数 | 录制类型可选值：<br/>`2`:分流录制<br/>`4`:合流录制 |
| userId | string | Y | 必传参数 | 分配给录制段的userId，限制长度为32bit，只允许包含（a-zA-Z），数字(0-9)以及下划线和连词符 |
| userSig | string | Y | 必传参数 | 录制userId对应的验证签名，相当于登录密码 |
| roomId | int | Y | 非必传参数 | 房间号码，取值范围：【1-4294967294】roomId与strRoomId必传一个，若两者都有值优先选用roomId,注意：目前一个房间最多只能审核8个用户 |
| strRoomId | string | Y | 必传参数 | 房间号码取值说明：只允许包含（a-zA-Z），数字(0-9)以及下划线和连词符，若您选用strRoomId时，需注意strRoomId和roomId两者都有值，优先选用roomId |

<span id="zegoParam">其中，data.zegoParam内容如下：</span>

| 请求参数名 | 类型 | 参数说明 | 传入说明 | 规范 |
| --- | --- | --- | --- | --- |
| tokenId | string | Zego鉴权token | 必传参数 | zego提供的identity_token身份验证信息，<br/>用于token登陆（每次开流必须主动调用zego接口获取新的token） |
| streamId | string | Zego流Id | 必传参数 | Zego的流ID |
| testEnv | bool | 是否使用zego测试环境 | 非必传参数 | 可选值如下：（默认值为`false`）<br/>`true`:测试环境<br/>`false`:正式环境 |

<span id="volcParam">其中，data.volcParam内容如下：</span>

| 请求参数名 | 类型   | 参数说明                                 | 传入说明 | 规范 |
| ---------- | ------ | ---------------------------------------- | -------- | ---- |
| appId      | string | 火山提供的应用标识                       | 必传参数 |      |
| roomId     | string | 房间号                                   | 必传参数 |      |
| userId     | string | 分配给录制端的userId                     | 必传参数 |      |
| token      | string | 录制userId对应的验证签名，相当于登录密码 | 必传参数 |      |

### 返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 本次请求的唯一标识 | 是 | 请求唯一标识 |
| code | int | 请求返回码 | 是 | 详见[接口响应码列表](#接口响应码列表) |
| message | string | 请求返回描述，和请求返回码对应 | 是 | 详见[接口响应码列表](#接口响应码列表) |
| detail | Json_object | 描述详细信息 | 否 |  |

其中，detail结构如下：

| **参数名称** | **参数类型** | **参数说明**                                                 | **是否必返** | **规范**         |
| ------------ | ------------ | ------------------------------------------------------------ | ------------ | ---------------- |
| errorCode    | int          | 状态码                                                       | 否           | `1001`：重复推流 |
| dupRequestId | string       | 表示重复的requestId<br/>当errorCode为1001，表示重复推流时，会返回dupRequestId字段<br/>例如当第一次请求的时候没有收到返回，但该音频流实际已经开始审核了，没有requestId无法主动关闭审核<br/>可以再次请求，收到重复推流的信息，通过返回的dupRequestId调用关闭审核接口 | 否           |                  |

## 异步回调结果

### 接口描述

用户如果需要服务端主动对视频检测结果进行回调，则需要在请求参数中指定回调协议接口URL callback参数，服务端根据该参数在视频审核完成后，主动回调用户。

### 请求方法：

`POST`

### 字符编码：

`UTF-8`

### 建议超时时间：

5s

### 回调策略

当用户收到推送结果，并返回HTTP状态码为200时，表示推送成功；否则系统将进行最多20次推送。

### 回调参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 本次请求的唯一标识 | 是 | 请求唯一标识 |
| code | int | 请求返回码 | 是 | 详见[接口响应码列表](#接口响应码列表) |
| message | string | 请求返回描述，和请求返回码对应 | 是 | 详见[接口响应码列表](#接口响应码列表) |
| statCode | int | 回调状态码 | 否 |状态码对应关系：<br/>0 ：审核结果回调 <br/>1 ：流结束结果回调|
| riskLevel | string |风险级别（code为1100时存在），可能取值：PASS，REVIEW，REJECT | 否 | PASS：正常内容，建议直接放行<br/>REVIEW：可疑内容，建议人工审核<br/>REJECT：违规内容，建议直接拦截 |
| contentType | int | 用来区分音频和图片回调 | 否 | 可能取值如下：<br/>`1`：该回调为图片回调<br/>`2`：该回调为音频回调 |
| detail | json_object | 风险详情 | 否 | 视频流中截帧图片或者音频片段的风险详情，详见[detail说明](#frameDetail) |
| tokenProfileLabels | json_array | 账号属性标签 | 否 | 仅在开启功能时返回，详见[tokenProfileLabels说明](#tokenProfileLabels) |
| tokenRiskLabels | json_array | 账号风险标签 | 否 | 仅在开启功能时返回，详见[tokenRiskLabels说明](#tokenRiskLabels) |

<span id="frameDetail">其中，在图片回调时（contentType为`1`时），detail每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| imgUrl | string | 当前截帧的URL | 否 | |
| room | string | 房间号，当客户传入时返回该字段| 否 | |
| requestParams | json object | 返回请求参数data中的所有字段 | 是 | |
| beginProcessTime  | int | 开始处理时间（13位时间戳）| 是 | |
| finishProcessTime | int | 检测完成时间（13位时间戳）| 是 | |
| imgTime | string | 视频流截帧图片的时间（绝对时间）| 否 | |
| riskType | int | 风险类型| 否 |标识风险类型，可能取值：<br/>0：       正常<br/>100：涉政<br/>200：色情<br/>210：性感<br/>300：广告<br/>310：二维码<br/>320：水印<br/>400：暴恐<br/>500：违规<br/>510：不良场景<br/>520：未成年人<br/>530：人脸<br/>531：人像<br/>532：伪造人脸<br/>533：颜值<br/>535：公众人物<br/>540：物品<br/>541：动物<br/>542：植物<br/>550：场景<br/>560：行业违规<br/>570：画面属性<br/>700：黑名单<br/>710：白名单<br/>800：高危账号<br/>900：自定义|
| riskSource | int | 风险来源（图片还是图片中OCR）| 否 |风险来源，可能取值：<br/>1000：无风险<br/>1001：文字风险<br/>1002：视觉图片风险|
| model | string | 策略规则标识| 否 |用来标识命中的策略规则|
| descriptionV2 | string | 策略规则风险原因描述| 否 |中文描述|
| detectType | int | 区分截帧图片是否过了机审| 否|可取值为1和2（仅当请求参数传了detectStep时才会返回该参数）<br/>1：截帧图片过了检测<br/>2：截帧图片没过检测|
| similarity | float | 与上一张截帧图片的相似概率值| 否 |取值范围[0-1]，数值越接近1越相似|
| similarityDedup | int | 辅助参数 | 否 |可能取值如下：（仅当相似帧去重推审功能生效时，外层处置建议从reject/review变更成pass返回该参数，其他情况不返回该字段）<br/>1：值为1，相似帧去重推审功能生效<br/>|
| stillTime | int | 展示静止画面时间，单位秒| 否 |请求参数type传值包含BUSINESSRISK时返回|
| matchedDetail | string | 命中所有名单详情| 否 |命中名单时返回 |
| matchedItem | string | 命中的具体敏感词（该参数仅在命中敏感词时有效）| 否 |命中名单时返回|
| matchedList | string | 命中敏感词所在的名单名称（该参数仅在命中敏感词时存在）| 否 |命中名单时返回 |
| imgText | string | 视频中画面识别出的文字内容| 否 ||
| userId | int | 声网用户账号标识（仅分流情况下存在）| 否 |返回的userId是实际房间中的用户id，与请求参数中的uid无关|
| strUserId | string | TRTC流用户账号标识（仅分流情况下存在）| 否 |返回的strUserId是实际房间中的用户id|
|businessLabels | json_array |传了imgBusinessType时返回 | 否 | 详见[businessLabels说明](#businessLabels) |

截帧图片detail中，businessLabels数组的每个成员的内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| confidenceLevel | int | 置信等级 | 是 | 可选值在0～2之间，值越大，可信度越高<br/> |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |
| businessDetail | Json_object | 详细信息 | 是 |  |

<span id="businessLabels">businessLabels中，businessDetail的内容如下：</span>

| **参数名** | **类型**   | **参数说明** | **是否必返** | **规范** |
| ---------- | ---------- | ------------ | ------------ | -------- |
| faces      | json_array | 人脸信息     | 否           |          |
| face_num   | int        | 人脸数量     | 否           |          |
| objects    | json_array | 物品信息     | 否           |          |
| persons    | Json_array | 人像信息     | 否           |          |
| person_num | int        | 人像数量     | 否           |          |

businessDetail中，faces数组的每个元素的内容如下：

| **参数名**  | **类型**  | **参数说明** | **是否必返** | **规范**                                                     |
| ----------- | --------- | ------------ | ------------ | ------------------------------------------------------------ |
| id          | string    | 编号         | 是           |                                                              |
| name        | string    | 人名         | 否           |                                                              |
| location    | int_array | 位置坐标     | 否           | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 |
| probability | float     | 置信度       | 否           | 可选值在0～1之间，值越大，可信度越高<br/>                    |
| face_ratio  | float     | 人脸占比     | 否           |                                                              |

businessDetail中，objects数组的每个元素的内容如下：

| **参数名**  | **类型**  | **参数说明** | **是否必返** | **规范**                                                     |
| ----------- | --------- | ------------ | ------------ | ------------------------------------------------------------ |
| id          | string    | 编号         | 是           |                                                              |
| name        | string    | 名称         | 否           |                                                              |
| location    | int_array | 位置坐标     | 否           | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 |
| probability | float     | 置信度       | 否           | 可选值在0～1之间，值越大，可信度越高<br/>                    |

businessDetail中，persons数组的每个元素的内容如下：

| **参数名**   | **类型**  | **参数说明** | **是否必返** | **规范**                                                     |
| ------------ | --------- | ------------ | ------------ | ------------------------------------------------------------ |
| id           | string    | 编号         | 是           |                                                              |
| location     | int_array | 位置坐标     | 否           | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 |
| probability  | float     | 置信度       | 否           | 可选值在0～1之间，值越大，可信度越高<br/>                    |
| person_ratio | float     | 人像占比     | 否           |                                                              |

其中，在音频回调时（contentType为2时），音频片段detail每个成员的具体内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| audioUrl | string | 音频片段地址 | 是 | |
| vadCode  | int    | 是否静音片段 | 是 | 0 ：静音片段<br/>1 ：非静音片段|
| room     | string | 房间号       | 是 |  |
| requestParams     | json object | 返回请求参数data中的所有字段|是| |
| beginProcessTime  | int | 开始处理时间（13位时间戳）| 是 | |
| finishProcessTime | int | 检测完成时间（13位时间戳）| 是 | |
| audioStartTime  | string | 视频流中音频违规内容开始时间（绝对时间）| 是 | |
| audioEndTime | string | 视频流中音频违规内容结束时间（绝对时间）| 是 | |
| riskType | int | 视频中音频的标识风险类型| 是 |标识风险类型，可能取值：<br/>0：       正常<br/>100：涉政<br/>120：国歌<br/>200：色情<br/>210：辱骂<br/>250：娇喘<br/>300：广告<br/>700：黑名单<br/>900：自定义|
| riskSource | int | 风险来源（图片还是图片中OCR）| 否 |风险来源，可能取值：<br/>1000：无风险<br/>1001：文字风险<br/>1003：音频语音风险|
| model | string | 策略规则标识| 是 |用来标识命中的策略规则|
| descriptionV2 | string | 策略规则风险原因描述| 是 ||
| matchedDetail | string | 命中所有名单详情| 否 ||
| matchedItem | string | 命中的具体敏感词（该参数仅在命中敏感词时有效）| 否 ||
| matchedList | string | 命中敏感词所在的名单名称（该参数仅在命中敏感词时存在）| 否 ||
| isSing | int | 检测该片段是否为唱歌| 否 |type取值包含SING时存在，<br/>取值0表示检测不存在唱歌片段，<br/>取值1表示检测存在唱歌片段|
| language | json_array | 语种标签与概率值列表 | 否| 详见[language说明](#language) |
| audioText | string | 视频中音频识别出的文字内容 | 否 | |
| userId | int | 声网用户账号标识（仅分流情况下存在）| 否 |返回的userId是实际房间中的用户id，与请求参数中的uid无关|
| strUserId | string | TRTC流用户账号标识（仅分流情况下存在）| 否 |返回的strUserId是实际房间中的用户id|
|businessLabels | json_array |传了audioBusinessType时返回 | 否 | 详见[businessLabels说明](#audioBusinessLabels) |

<span id="audioBusinessLabels">音频的detail中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| confidenceLevel | int | 置信等级 | 是 | 可选值在0～2之间，值越大，可信度越高 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |
| businessDetail | Json_object | 详细信息 | 是 |  |

<span id="language">音频的detail中，language数组中每一项具体参数如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| label | int | 语种识别类别 | 是 |语种识别类别标识，可能取值：<br/>0:普通话<br/>1:英语<br/>2:粤语 |
| probability | int | 对应音色标签可能性大小，取值0-100，数值越高表示概率越大 | 是 | 取值范围[0,100] |

<span id="tokenProfileLabels">其中，tokenProfileLabels数组每个成员的具体内容如下：</span>

| **参数名**  | **类型** | **参数说明** | **是否必返** | **规范**                   |
| ----------- | -------- | ------------ | ------------ | -------------------------- |
| label1      | string   | 一级标签     | 否           |                            |
| label2      | string   | 二级标签     | 否           |                            |
| label3      | string   | 三级标签     | 否           |                            |
| description | string   | 标签描述     | 否           |                            |
| timestamp   | int      | 打标签时间戳 | 否           | 13位Unix时间戳，单位：毫秒 |

<span id="tokenRiskLabels">其中，tokenRiskLabels数组每个成员的具体字段同tokenProfileLabels</span>

### 审核结束回调参数（returnFinishInfo为true时返回）：

| **参数名**        | **类型**    | **参数说明**                                 | **是否必返** | **规范**                                                     |
| ----------------- | ----------- | -------------------------------------------- | ------------ | ------------------------------------------------------------ |
| code              | int         | 请求返回码                                   | 是           | 详见[接口响应码列表](#接口响应码列表)                              |
| message           | string      | 请求返回描述，和请求返回码对应               | 是           | 详见[接口响应码列表](#接口响应码列表)                              |
| requestId         | string      | 请求唯一标识                                 | 是           |                                                              |
| statCode          | int         | 回调状态码                                   | 是           | 回调状态码，当returnFinishInfo为true时存在。状态码对应关系：<br/>0 ：审核结果回调 <br/>1 ：流结束结果回调 <br/>当statCode=1时，如下参数存在 |
| contentType       | int         | ⽤来区分⾳频和图⽚回调，当code等于1100时返回 | 是           | 可能取值如下：<br/>`1`：该回调为图片回调<br/>`2`：该回调为音频回调 |
| riskLevel              | string         | 流风险处置建议                         | 是           | 回调结束时返回整体流的处置建议                             |
| pullStreamSuccess | bool        | 拉流是否成功                                 | 是           | 可能取值如下：<br/>`true`：拉流成功<br/>`false`：拉流失败<br/>如果一张截图都没有获取成功即认为拉流失败 |
| detail            | json_object | 结果详情                                     | 是           | 详见[detail说明](#FinishDetail) |
| auxinfo         | json_object | 辅助信息                                    | 是           | 详见[auxinfo说明](#auxinfo2) |

<span id="FinishDetail">结束回调中的detail内容如下：</span>

| **参数名**    | **类型**    | **参数说明**                 | **是否必返** | **规范** |
| ------------- | ----------- | ---------------------------- | ------------ | -------- |
| requestParams | json_object | 返回请求参数data中的所有字段 | 是           | 无       |

<span id="auxinfo2">其中auxInfo字段结构如下：</span>

| **参数名**      | **类型**    | **参数说明**           | **是否必返** | **规范**                                                                                                                                         |
| --------------- | ----------- | ---------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| errorCode       | int         | 状态码           | 是           | <p>状态码</p><p>3001：流地址访问失败，例如资源HTTP状态码404、403</p><p>3002：流数据无效，例如“Invalid data found when processing input”</p><p>3003：流不存在，例如zego返回197612错误码</p><p>3004：流未返回音频数据</p><p>3005：拉流token无效或过期，建议使用新token重新开启审核，例如声网token过期或者trtc usersig无效</p>  |
| streamTime       | int         | 流审核时长           | 否           | 流结束后最后一次返回，代表送审时长，如有间隔审核逻辑时，和流真实时长可能不一致  |

## 视频流关闭接口

### 接口描述

该接口用于客户端通知服务端某个视频流已关闭。

### 请求URL：

| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 上海 | `http://api-videostream-sh.fengkongcloud.com/v3/saas/anti_fraud/finish_videostream` | 中文视频流 |
| 新加坡 | `http://api-videostream-xjp.fengkongcloud.com/v3/saas/anti_fraud/finish_videostream` | 中文视频流 |
| 硅谷 | `http://api-videostream-gg.fengkongcloud.com/v3/saas/anti_fraud/finish_videostream` | 中文视频流 |
| 印度 | `http://api-videostream-yd.fengkongcloud.com/v3/saas/anti_fraud/finish_videostream` | 中文视频流 |

### 请求方法：

`POST`

### 支持协议：

`HTTP`或`HTTPS`

### 字符编码：

`UTF-8`

### 建议超时时间：

1s

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 公司密钥 | 必传参数 | 用于权限认证，开通账号服务时由数美提供 |
| requestId | string | 本次请求的唯一标识 | 必传参数 | 需要关闭视频流的requestId |

### 返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 本次请求的唯一标识 | 是 | 请求唯一标识 |
| code | int | 请求返回码 | 是 | 请见[接口响应码列表](#codeList) |
| message | string | 请求返回描述，和请求返回码对应 | 是 | 请见[接口响应码列表](#codeList) |

## imgBusinessType可选值列表

| 业务标签识别类型 | 类型说明 | 备注 |
| ------------- | ------------- | --------------- |
| AGE           | 人脸 - 年龄   | 可识别未成年人  |
| GENDER        | 人脸 -性别   |  |
| BEAUTY        | 人脸 - 颜值   |  |
| FACEDETECTION | 人脸-人脸检测 | 如识别无人脸、真人、口罩人脸、正脸、侧脸等 |
| FAKEFACE | 人脸 - 伪造人脸 |  |
| RACE | 人脸-人种 | 如黑种人、白种人、黄种人 |
| PUBLICFIGURE | 人物  - 公众人物 | 如识别知名明星、网红等 |
| TAINTEDSTAR | 人物 - 劣迹人物 |  |
| POSTURE | 人像-人像姿态 | 如识别坐姿、跪姿等 |
| DRESS | 人像 - 人像穿着 | 如识别jk、汉服等 |
| BODY | 人体 | 如识别头发、眼睛、鼻子等 |
| PICTUREFORM | 画面属性 - 画面类型 | 如识别动漫、表情包等 |
| PICTURESTRUCT | 画面属性-画面结构 | 如识别宫格图、桥段图等 |
| LOWVISION | 画面属性  - 画面低质 | 如识别模糊、涂抹、马赛克等 |
| LOWCONTNET | 画面属性 - 内容低质 | 如识别点线密集、虫类密集等 |
| LIVEPICTURE | 画面属性-直播画面 | 如识别床上直播、开车直播等 |
| SCREENSHOT | 画面属性 -  APP截图（内容搬运） | 如识别朋友圈截图、聊天截图等 |
| FITNESS | 场景主题-健身 |  |
| CATE | 场景主题-美食 |  |
| MUSIC | 场景主题-音乐 |  |
| SPORTS | 场景主题-体育 |  |
| SCENERY | 场景主题-自然风光 | 如识别天空、大海、草原等 |
| CITYVIEW | 场景主题-城市风光 | 如识别街景 |
| 3CPRODUCTSLOGO | LOGO - 3C电子类品牌 | 如识别华为、小米、OPPO等LOGO |
| SHOPPINGAPPSLOGO | LOGO - 购物比价类应用 | 如识别拼多多等LOGO |
| RETOUCHAPPSLOGO | LOGO - 拍摄美化类应用 | 如识别快剪辑、秒拍等LOGO |
| SOCIALAPPSLOGO | LOGO - 社交通讯类应用 | 如识别微博、小红书等LOGO |
| PHOTOMATERIALLOGO | LOGO - 素材版权类应用 | 如识别CFP等LOGO |
| NEWSAPPSLOGO | LOGO - 新闻阅读类应用 | 如识别新浪、视觉中国等LOGO |
| ENTERTAINMENTAPPSLOGO | LOGO - 影音娱乐类应用 | 如识别抖音、快手等LOGO |
| SPORTSLOGO | LOGO  - 体育赛事 | 如识别奥运会等LOGO |
| APPARELLOGO | LOGO - 鞋帽服饰类品牌 | 如识别VANS、H&M等LOGO |
| ACCESSORIESLOGO | LOGO - 饰品首饰类品牌 | 如识别AudemarsPiguet、Nomos等LOGO | 
| COSMETICSLOGO | LOGO - 化妆品类品牌 | 如识别LOTTE、EyesLipsFace等LOGO | 
| FOODLOGO | LOGO - 食品类品牌 | 如识别Starbucks、LOTTE等LOGO | 
| VEHICLE | 物品-交通工具 |  |
| BUILDING | 物品-建筑 |  |
| TABLEWARE | 物品-餐具 |  |
| FOOD | 物品-食物 |  |
| HOMEAPPLICATION | 物品-家用电器 |  |
| OFFICESUPPLIES | 物品-办公用品 |  |
| FASHION | 物品-穿着用品 |  |
| SPORTEQUIPMENT | 物品-运动器材 |  |
| TOY | 物品-玩具 |  |
| MAKEUP | 物品-化妆品 |  |
| DRUGS | 物品-药品 |  |
| PAINTING | 物品-绘画作品 |  |
| ELECTRONIC | 物品-电子产品 |  |
| MEDICALIMAGE | 物品-医疗影像 |  |
| FURNITURE | 物品-家居用品 |  |
| DAILYSUPPLIES | 物品-生活用品 |  |
| CONSTELLATION | 物品-星座占卜 |  |
| KITCHENWARE | 物品-厨房用品 |  |
| KEEPSAKE | 物品 - 纪念品 |  |
| MAMMAL | 动物-哺乳动物 |  |
| BIRDS | 动物 - 鸟类 |  |
| REPTILE | 动物-爬行动物 |  |
| FISH | 动物-鱼 |  |
| ARTHROPOD | 动物  - 节肢动物 |  |
| COELENTERATE | 动物  - 腔肠动物 |  |
| MOLLUSKS | 动物  - 软体动物 |  |
| CRUSTACEAN | 动物  - 甲壳动物 |  |
| PLANT | 植物 |  |
| SETTING | 场所 | 如识别卫生间、酒店、厨房等 |

## 接口响应码列表

code请求返回码列表如下：

| **code** | **message**      |
| -------- | ---------------- |
| 1100     | 成功             |
| 1901     | QPS超限          |
| 1902     | 参数不合法       |
| 1903     | 服务失败         |
| 1904     | 流路数超限       |
| 9100     | 余额不足         |
| 9101     | 无权限操作       |

## 示例

### 上传接口请求示例
**普通流**
```json
{
    "accessKey":"xxx",
    "audioCallback":"http://xxx",
    "audioType":"PORN_AD",
    "data":{
        "channel":"VIDEOSTREAM",
        "detectFrequency":3,
        "returnAllImg":1,
        "returnAllText":true,
        "returnPreAudio":true,
        "returnPreText":true,
        "room":"001",
        "streamType":"NORMAL",
        "tokenId":"1157893895",
        "url":"rtmp://xxxx"
    },
    "imgCallback":"http://xxxxx",
    "imgType":"DEFAULT"
}
```
**声网流**
```json
{
    "accessKey":"xxx",
    "audioCallback":"http://xxx/",
    "audioType":"AD_PORN",
    "data":{
        "agoraParam":{
            "appId":"xxx",
            "channel":"letdo",
            "channelKey":"xxxx",
            "uid":654321
        },
        "channel":"VIDEOSTREAM",
        "detectFrequency":3,
        "returnAllImg":1,
        "returnAllText":true,
        "returnPreAudio":true,
        "returnPreText":true,
        "room":"001",
        "streamName":"test1",
        "streamType":"AGORA",
        "tokenId":"1157893895"
    },
    "imgCallback":"http://xxxx",
    "imgType":"DEFAULT"
}
```
**即构流**
```json
{
    "accessKey":"xxx",
    "audioCallback":"http://xxx/",
    "audioType":"AD_PORN",
    "data":{
        "zegoParam":{
            "tokenId":"xxx",
            "streamId":"xxxx",
            "testEnv":654321
        },
        "channel":"VIDEOSTREAM",
        "detectFrequency":3,
        "returnAllImg":1,
        "returnAllText":true,
        "returnPreAudio":true,
        "returnPreText":true,
        "room":"001",
        "streamName":"test1",
        "streamType":"AGORA",
        "tokenId":"1157893895"
    },
    "imgCallback":"http://xxxx",
    "imgType":"DEFAULT"
}
```
**TRTC流**
```json
{
    "accessKey":"4Ky6AV4hE0pWLeG1bXNw}",
    "appId":"default",
    "imgType":"PORN_AD",
    "audioType":"NONE",
    "imgCallback":"http://10.0.20.208:8000/",
    "audioCallback":"http://10.0.20.208:8000/",
    "data":{
        "streamType":"TRTC",
        "tokenId":"test_videostream_v2",
        "trtcParam":{
            "sdkAppId":1400498247,
            "userId":"12345",
            "userSig":"eJyrVgrxCdYrSy1SslIy0jNQ0gHzM1NS80oy0zLBwoZGxiamUInilOzEgoLMFCUrQxMDAxNLCyMTc4hMakVBZlEqUNzU1NTIwMAAIlqSmQsSMzM2BWJLc0OoKZnpQHMtKiPNPStdtEs9w0P93Dx9sosc00MCLEJLAt39IoqK8isN-bxT3WP0k8rdsm2VagHNnDDN",
            "strRoomId":"1256732",
            "demoSences":4
        },
        "detectFrequency":5,
        "detectStep":1,
        "returnAllImg":1,
        "streamName":"test2",
        "returnAllText":true,
        "returnFinishInfo":true,
        "channel":"default"
    }
}
```

### 上传接口返回示例：

```json
{
    "code":1100,
    "message":"正常",
    "requestId":"d05f4270374ca516ce7aafc0139afd25"
}
```

### 异步回调结果示例：

**截帧图片回调**

```json
{
    "code":1100,
    "contentType":1,
    "message":"成功",
    "requestId":"e8f959af3e2498ec5dd89126465d124b_vs24_0000008",
    "riskLevel":"REJECT",
    "detail":{
        "descriptionV2":"正常",
        "imgText":"ocr123你好",
        "imgTime":"2019-09-20 14:53:25",
        "imgUrl":"http://video-bj.bj.bcebos.com/image/20190920/e8f959af3e2498ec5dd89126465d124b_vs24_0000008.jpg",
        "matchedDetail":"[{\"listId\":\"0b5d2ed695d40c0aa51e2b35fb51763d\",\"matchedFiled\":[\"text\"],\"name\":\"涉政测试\",\"organization\":\"RlokQwRlVjUrTUlkIqOg\",\"words\":[\"你好\"]}]",
        "matchedItem":"你好",
        "matchedList":"涉政测试",
        "model":"M02601",
        "polityName":"你好",
        "requestParams":{
            "appId":"default",
            "channel":"VIDEOSTREAM",
            "detectFrequency":3,
            "returnAllImg":1,
            "returnAllText":true,
            "returnPreAudio":true,
            "returnPreText":true,
            "room":"001",
            "streamType":"NORMAL",
            "tokenId":"1157893895",
            "url":"rtmp://58.200.131.2:1935/livetv/hunantv"
        },
        "riskType":100,
        "room":"001",
		 "userId":56432
    }
}

```

**音频片段回调**

```json
{
    "code":1100,
    "message":"成功",
    "requestId":"ba6e47e02f7b3d3ae6a48704f44c0da0_0",
    "riskLevel":"REJECT",
    "contentType":2,
    "detail":{
        "audioEndTime":"2019-09-20 15:27:35",
        "audioStartTime":"2019-09-20 15:27:25",
        "audioText":"老婆",
        "audioUrl":"http://voice-bj.bj.bcebos.com/20190920/ba6e47e02f7b3d3ae6a48704f44c0da0_0.mp3",
        "model":"M02101",
        "requestParams":{
            "agoraParam":{
                "appId":"2980dcb5f71b40e8bd83397ec2099bee",
                "channel":"letdo",
                "channelKey":"004b152978620e0072930edcfd862f116fab445e0f22980dcb5f71b40e8bd83397ec2099bee1568964445800000010000000000",
                "uid":654321
            },
            "appId":"",
            "audioName":"test1",
            "channel":"VIDEOSTREAM",
            "detectFrequency":3,
            "returnAllImg":1,
            "returnAllText":true,
            "returnPreAudio":true,
            "returnPreText":true,
            "room":"001",
            "streamName":"test1",
            "streamType":"AGORA",
            "tokenId":"1157893895"
        },
        "riskType":200,
        "room":"001"
    }
}
```

### 关闭接口请求示例：

```json
{
    "accessKey": "4Ky6AV4hE0pWLeG1bXNw",
    "requestId": "1639825145166"
}
```

### 关闭接口返回示例

```json
{
    "code":1100,
    "message":"成功",
    "requestId":" a78eef377079acc6cdec24967ecde722"
}
```
