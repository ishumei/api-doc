# 智能音频流识别产品API文档

# 1. 接入前准备

## 1.1 数美服务账号申请

客户经理已提前与贵公司建立联系或当面拜访，可直接将开通账号及服务相关信息提供至客户经理。

开通账号所需信息包括：

公司全称：xxxxxx

公司简称：xxxxxx

接口人邮箱：xxx@xxx.xxx

接口人手机：1xxxxxxxxxx

## 1.2 渠道配置表

数美根据客户不同业务场景，配置不同的渠道（channel），制定针对性的拦截策略，同时也方便客户针对不同业务场景的数据进行筛选、分析。需要联系数美服务开通，请使用数美单独提供的传值为准。

## 1.2 数美服务账号信息接收

数美客户经理会在1个工作日内为您开通相应数美账号及服务，随后接口人邮箱会收到如下信息：

| **名称**         | **具体值**                    | **说明**                                    |
| :--------------- | :---------------------------- | :------------------------------------------ |
| accessKey        | xxxxxx                        | 数美API服务的认证码，调用数美API时需要传入  |
| organization     | xxxxxx                        | 数美分配的企业唯一标识码，调用SDK时需要传入 |
| 数美管理后台账号 | xxxxxx                        | 用于登陆数美管理后台                        |
| 数美管理后台密码 | xxxxxx                        | 用于登陆数美管理后台                        |
| 数美管理后台地址 | https://console.ishumei.com | 用于登陆数美管理后台                        |

# 2. 智能音频流过滤服务接口说明

数美智能音频流过滤服务方案提供音频流内容检测和音频流关闭通知接口。

## 2.1 音频流检测请求

### 接口描述

该接口用于提交音频流相关信息，接口会实时检测音频流中是否出现违规内容，并通过回调把违规信息发送给客户指定的url。

### 请求URL

上海集群：

http://api-audiostream-sh.fengkongcloud.com/v2/saas/anti_fraud/audiostream

硅谷集群：

http://api-audiostream-gg.fengkongcloud.com/v2/saas/anti_fraud/audiostream

新加坡:

http://api-audiostream-xjp.fengkongcloud.com/v2/saas/anti_fraud/audiostream

### 字符编码格式

请求及返回结果都使用UTF-8字符集进行编码

### 请求方法

POST

### 建议超时时长

3s

### 通用请求参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型**    | **是否必选** | **说明**                                                     |
| :----------- | :---------- | :----------- | :----------------------------------------------------------- |
| accessKey    | string      | Y            | 服务密钥，开通账号服务时由数美提供                           |
| type         | string      | N            | <p>识别类型，可选值：</p><p>EROTIC：色情识别<br/>DIRTY: 辱骂识别</p><p>ADVERT：广告识别</p><p>AUDIOPOLITICAL：一号领导人声纹识别</p><p>POLITY：涉政识别</p><p>MOAN：娇喘识别</p><p>ANTHEN：国歌识别</p><p>MINOR：未成年人识别</p><p>BANEDAUDIO：违禁歌曲</p><p>如需做组合识别，通过下划线连接即可，例</p><p>如 POLITY_EROTIC_MOAN_ADVERT 用于广告、色情和涉政,娇喘识别。</p><p>type和 businessType 必须填其一</p> |
| businessType | string      | N            | <p>识别类型，可选值：</p><p>SING：唱歌</p><p>AGE：年龄</p><p>LANGUAGE：语种</p><p>GENDER：性别</p><p>TIMBRE：音色</p><p>VOICE：人声属性</p><p>AUDIOSCENE：声音场景</p><p>type和 businessType 必须填其一</p> |
| btId         | string      | Y            | 音频唯一标识，用于查询指定音频，限长128位字符长度            |
| appId        | string      | N            | <p>应用标识</p><p>用于区分相同公司的不同应用，需要联系数美开通，请以数美单独提供的传值为准</p> |
| callback     | string      | Y            | 异步检测结果回调通知您的URL，支持HTTP和HTTPS                 |
| data         | json_object | Y            | 请求数据内容，最长1MB                                        |

其中，data的内容如下：

| **参数名称**  | **类型**    | **是否必选** | **说明**                                                                                                  |
| :------------ | :---------- | :----------- | :-------------------------------------------------------------------------------------------------------- |
| streamType    | string      | Y            | <p>流类型,可选择：<br/>普通流：NORMAL，目前支持rtmp、rtmps、hls、http、https协议,支持flv,m3u8等格式<br/>声网录制：AGORA<br/>即构录制：ZEGO<br/>腾讯录制：TRTC<br/>火山引擎录制：VOLC<br/>巨人录制：GIN<br/><p>注意：使用RTC的SDK录制方案的时候，会在RTC侧产生额外的录制费用，具体费用请咨询相关RTC厂商</p> |
| url           | string      | Y            | 要检测的音频流url地址（当streamType为NORMAL时必传）                                                       |
| agoraParam    | json_object | Y            | 声网录制参数（当streamType为AGORA时必传），详见扩展参数                                                   |
| ginParam    | json_object | Y            | 巨人录制参数（当streamType为GIN时必传），详见扩展参数                                                   |
| zegoParam     | json_object | Y            | 即构录制参数（当streamType为ZEGO时必传），详见扩展参数                                                    |
| trtcParam     | json_object | Y            | 腾讯录制参数（当streamType为TRTC时必传），详见扩展参数                                                    |
| volcParam     | json_object | Y            | 火山引擎录制参数（当streamType为VOLC时必传），详见扩展参数                                                    |
| tokenId       | string      | Y            | 客户端用户账号唯一标识，                                                                                  |
| channel       | string      | Y            | 见渠道配置表                                                                                              |
| lang          | string      | N            | 可选值如下，（默认值为zh）：<br/>zh：中文<br/>en：英文<br/>ar：阿拉伯语                                                                                                     |
| extra | json_object | N | 辅助参数 [详见extra参数](#extra) |

### 扩展请求参数

放在data下，其中具体参数如下：

| **参数名称**     | **类型** | **是否必选** | **说明**                                                     |
| :--------------- | :------- | :----------- | :----------------------------------------------------------- |
| room             | string   | N            | 房间号，强烈建议传入                                         |
| role             | string   | N            | <p>用户角色</p><p>对不同角色可配置不同策略。</p><p>直播领域可取值：</p><p>房管：ADMIN</p><p>主播：HOST</p><p>系统角色：SYSTEM</p><p>游戏领域可取值：</p><p>管理员：ADMIN</p><p>普通用户：USER</p><p>默认值：普通用户</p> |
| returnAllText    | bool     | N            | <p>取值为true时返回全量的音频流片段识别结果和文本内容；</p><p>取值为false时返回riskLevel为非pass的音频流片段识别结果和文本内容，默认是false</p><p>建议传入true （默认为false，在静音的情况下不会产生回调）</p> |
| returnPreText    | bool     | N            | <p>值为true时，返回的content字段包含违规音频前一个片段10秒文本内容；</p><p>值为false时，返回的content字段只包含违规音频片段文本内容，默认值为false(对于TRTC流该功能无效，当客户使用间隔审核功能时，即使returnPreAudio是true情况下，也不返回该字段）</p><p></p> |
| returnPreAudio   | bool     | N            | <p>值为true，返回违规音频前一个片段10秒链接；值为false时，只返回违规片段音频链接。默认值为false(对于TRTC流该功能无效)，</p><p>当客户使用间隔审核功能时，即使returnPreText为true情况下，也只返回当前片段文本，不返回前一个片段的文本。</p> |
| returnFinishInfo | bool     | N            | <p>音频流结束回调通知</p><p>可选值（默认为false）：<br/>false：审核结束时不发送结束通知</p><p>true：审核结束时发起结束通知，回调参数增加statCode状态码</p><p>建议传入true（默认为false，在流结束时不会产生回调）</p> |
| initDomain       | int      | N            | 当即构客户端init初始化支持隔离域名和随机userId该字段必传,可选值：<br/>`0`：默认版本<br/>`1`：仅支持客户端初始化有隔离域名<br/>`2`：支持客户端初始化有隔离域名和随机userId功能<br/>`3`：更新SDK，修复一些bug<br/>`4`：支持客户自定义传入SEI信息<br/>`5`：支持vad静音检测，token会有唯一性校验，每次上传鉴黄必须重新生成<br/>**推荐使用`5`进行接入；** 为兼容老客户使用，默认值为0 |
| audioDetectStep  | int      | N            | 音频每个步长只会检测一次,取值范围1-36的整数，默认每个片段都审核（备注）<br/>举例：该参数设置为1，会审核第一个片段、第三个片段、第五个片段，以此类推。该参数设置为2，会审核第一个片段、第四个片段、第个七个片段，以此类推。 |

其中data.agoraParam内容如下:

| **参数名称**    | **类型** | **是否必选** | **说明**                                                     |
| :-------------- | :------- | :----------- | :----------------------------------------------------------- |
| appId           | string   | Y            | 声网提供的appId，注意与数美的appId区分开                     |
| channel         | string   | Y            | 声网提供的频道名，注意与数美channel区分开。                  |
| token           | string   | N            | 安全要求较高的用户可以使用 token进行认证，生成方式详见声网文档： <https://docs.agora.io/cn/Interactive%20Broadcast/token_server?platform=All%20Platforms><br/>建议将token的有效期设置超过频道的持续时间，防止token失效导致无法拉流。当前声网支持的最大token有效期为24小时，因此当频道持续时间超过24小时的时候，需要处理token失效的问题。处理方法：在请求参数中设置开启音频流结束回调通知（设置returnFinishInfo为true）。当回调接收到审核结束通知（statCode为1），并且原因是由于拉流的token无效或过期（auxInfo下errorCode状态码返回3005），如果频道仍然存在并且需要继续审核，则生成新的token，将频道重新送审。 |
| uid             | int      | N            | 用户 ID，32 位无符号整数。当token存在时，必须提供生成token时所使用的用户ID。注意，此处需要区别实际房间中的用户uid，提供给服务端录制所用的uid不允许在房间中存在。 |
| isMixingEnabled | bool     | N            | <p>单流/合流录制，默认合流录制。</p><p>true:合流</p><p>false:分流</p><p>合流是指一个直播房间一路流，分流是指一个麦位一路流</p> |
| channelProfile  | int      | N            | <p>声网录制的频道模式，取值：</p><p>0: 通信（默认），即常见的 1 对 1 单聊或群聊，频道内任何用户可以自由说话；</p><p>1: 直播，有两种用户角色: 主播和观众。</p><p>默认以通信模式录制，即默认值为0。</p> |
| subscribeMode  | string      | N            | <p>订阅模式:</p><p>`AUTO`: 自动订阅房间内的所有流，不设置subscribeMode时候的默认行为</p><p>`UNTRUSTED`: 配合`untrustedUserIdList`只订阅该列表指定的用户流，仅在声网分流里生效</p><p>`TRUSTED`: 配合`trustedUserIdList`只订阅该列表以外的用户流，仅在声网分流中生效</p> |
| trustedUserIdList  | string_array      | N            | <p>信任用户的列表，subscribeMode=TRUSTED时生效，允许为空，数美不会订阅房间内该列表指定的用户流。其中每个元素uid的范围应为uint32的范围，但类型为string。例如：["123","456"]</p> |
| untrustedUserIdList  | string_array      | N            | <p>非信任用户的列表，subscribeMode=UNTRUSTED时生效，不允许为空，数美只订阅房间内该列表指定的用户流。其中每个元素uid的范围应为uint32的范围，但类型为string。例如：["123","456"]</p> |


其中data.ginParam内容如下:

| **参数名称**    | **类型** | **是否必选** | **说明**                                                                                                                                                                                                   |
| :-------------- | :------- | :----------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| tokenId           | string   | Y            | 房间token，用于拉流端登陆房间，需要巨人提供                                                                                                                                                                   |
| roomId         | string   | Y            | 房间编号，唯一对应一个房间，服务端以房间为单位拉流录制                                                                                                                                                                |
| isMixingEnabled | bool     | Y            | <p>录制模式，可能取值：</p><p>`false`：分流，房间内每个用户单独录制审核</p><p>`true`：合流，房间内所有用户合成一路流录制审核</p>                                                                             |
| ip  | string      | Y            | 指定服务器ip      |
| port   | string   | Y            | 指定端口                                                                     |

其中data.zegoParam内容如下

| **参数名称**    | **类型** | **是否必选** | **说明**                                                     |
| :-------------- | :------- | :----------- | :----------------------------------------------------------- |
| tokenId         | string   | Y            | zego提供的身份验证信息，用于token登陆                        |
| streamId        | string   | N            | 音频流编号，唯一对应一路音频流，streamId与roomId至少传入其中之一。 |
| roomId          | string   | N            | 房间编号，唯一对应一个房间。                                 |
| isMixingEnabled | bool     | N            | 录制模式，可能取值（不传默认为true）：<br/>true：合流，房间内所有用户合成一路流录制审核。此时如果streamId与roomId单独存在时则单独生效；但当streamId与roomId同时存在时，则以streamId为有效值。<br/>false：分流，房间内每个用户单独录制审核。此时roomId为必传，且以roomId为有效值。 |

其中 data.trtcParam内容如下

| **参数名称** | **类型** | **是否必选** | **说明**                                                                                                                                                   |
| :----------- | :------- | :----------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------- |
| sdkAppId     | int      | Y            | 腾讯提供的sdkAppId                                                                                                                                         |
| demoSences   | int      | Y            | <p>录制类型可选值：</p><p>分流录制：2<br/>合流录制：4</p>                                                                                                  |
| userId       | string   | Y            | 分配给录制端的userId，限制长度为32bit，只允许包含（a-zA-Z），数字(0-9)以及下划线和连词符                                                                   |
| userSig      | string   | Y            | 录制userId对应的验证签名，相当于登录密码                                                                                                                   |
| uid       | string   | N            | 指定需要审核的用户ID，如果不传该参数，则默认拉取并审核房间中所有推流用户的流。如果需要审核同一房间内的一部分用户，**请使用不同的录制端userId和userSig分多次请求。注意此参数和userId的区别**。                                            |
| roomId       | int      | Y            | <p>房间号码，取值范围：【1-4294967294】</p><p>roomId与strRoomId必传一个，若两者都有值优先选用roomId</p>                                                    |
| strRoomId    | string   | Y            | <p>房间号码<br/>取值说明：只允许包含（a-zA-Z），数字(0-9)以及下划线和连词符</p><p>若您选用strRoomId时，需注意strRoomId和roomId两者都有值优先选用roomId</p> |

trtc流会根据上述`sdkAppId`、`userId`、`roomId`或`strRoomId`进行去重

其中data.volcParam内容如下:

| **参数名称**    | **类型** | **是否必选** | **说明**                                                                                                                                                                                                   |
| :-------------- | :------- | :----------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| appId           | string   | Y            | 火山引擎提供的appId，注意与数美的appId区分开                                                                                                                                                                   |
| roomId         | string   | Y            | 房间号                                                                                                                                                                |
| token           | string   | Y            | 火山引擎token，详见：https://www.volcengine.com/docs/6348/70121 |
| userId            | string      | Y            | 分配给录制端的userId                                           |
| subscribeMode | string     | N            | <p>订阅模式:</p><p>AUTO: 自动订阅房间内的所有流，不设置subscribeMode时候的默认行为。</p><p>UNTRUSTED: 配合untrustedUserIdList只订阅该列表指定的用户流，此种模式下如果untrustedUserIdList列表为空，参数错误，因为无法订阅任何流。</p><p>TRUSTED: 配合trustedUserIdList只订阅该列表以外的用户流，此种模式下如果一定时间下没有untrustedUserIdList名单外的用户进入房间，数美将主动结束审核。</p>                                                                            |
| trustedUserIdList  | string_array      | N            | 信任用户的列表，subscribeMode=TRUSTED时生效，允许为空，数美不会订阅房间内该列表指定的用户流。      |
| untrustedUserIdList   | string_array   | N            | 非信任用户的列表，subscribeMode=UNTRUSTED时生效，不允许为空，数美只订阅房间内该列表指定的用户流。  |                     

<span id="extra">data中，extra的内容如下：</span>

| **请求参数名** | **类型**    | **是否必传** | **规范**                                 |
| -------------- | ----------- | ------------ | ---------------------------------------- |
| passThrough    | json_object | N            | 透传字段，该字段下所有内容会通过回调返回 |

### 返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型**    | **是否必选** | **说明**       |
| :----------- | :---------- | :----------- | :------------- |
| code         | int         | Y            | 返回码         |
| message      | string      | Y            | 返回码详情描述 |
| requestId    | string      | Y            | 请求唯一标识   |
| detail       | json_object | N            | 描述详细信息   |

detail结构如下：

| **参数名称** | **类型** | **是否必选** | **说明** |
| :----------- | :------- | :----------- | :------- |
| errorcode    | int      | Y            | 状态码   |
| dupRequestId | string       |  N|  表示重复的requestId<br/>当errorcode为1001，表示重复推流时，会返回dupRequestId字段<br/>例如当第一次请求的时候没有收到返回，但该音频流实际已经开始审核了，没有requestId无法主动关闭审核<br/>可以再次请求，收到重复推流的信息，通过返回的dupRequestId调用关闭审核接口 |


errorcode对应说明如下：

| **code** | **message** |
| :------- | :---------- |
| 1001     | 重复推流    |

### 回调策略

当用户收到推送结果，并返回HTTP状态码为200时，表示推送成功；否则

系统将进行最多12次推送。

### 请求方法

POST

### 字符编码格式

请求及返回结果都使用UTF-8字符集进行编码

### 回调参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型**    | **是否必选** | **说明**                                                                                                                                                                                   |
| :----------- | :---------- | :----------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| code         | int         | Y            | 返回码                                                                                                                                                                                     |
| message      | string      | Y            | 返回码详情描述                                                                                                                                                                             |
| requestId    | string      | Y            | 请求唯一标识                                                                                                                                                                               |
| score        | int         | N            | <p>风险分数（code 为 1100 且riskLevel=REJECT时存在）</p><p>取值范围[0,1000]，分数越高风险越大</p>                                                                                          |
| riskLevel    | string      | Y            | <p>风险级别（code 为 1100 时存在）</p><p>可能返回值：PASS，REVIEW，REJECT</p><p>PASS：正常内容，建议直接放行</p><p>REVIEW：可疑内容，建议人工审核</p><p>REJECT：违规内容，建议直接拦截</p> |
| statCode     | int         | N            | <p>审核状态：</p><p>0 ：审核中： </p><p>1 ：审核结束</p>                                                                                                                                   |
| detail       | json_object | Y            | 风险详情                                                                                                                                                                                   |
| auxInfo      | json_object | N            | 辅助信息                                                                                                                                                                                   |

其中，detail 的内容如下：

| **参数名称**      | **类型**    | **是否必选** | **说明**                                                                                                                                                                                                                                                                                                                                      |
| :---------------- | :---------- | :----------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| beginProcessTime  | int         | Y            | 开始处理的时间（13位时间戳）                                                                                                                                                                                                                                                                                                                  |
| finishProcessTime | int         | Y            | 结束处理的时间（13位时间戳）                                                                                                                                                                                                                                                                                                                  |
| audioUrl          | string      | Y            | <p>音频片段地址，returnAllText不传或为false时只返回违规音频片段地址，</p><p>returnAllText为true时返回所有音频片段地址</p>                                                                                                                                                                                                                     |
| preAudioUrl       | string      | N            | 返回的是当前片段与前一片段的20秒音频片段地址（该参数只有在请求参数中returnPreAudio是true情况下存在）                                                                                                                                                                |
| audio_endtime     | string      | Y            | 违规内容结束时间（绝对时间）                                                                                                                                                                                                                                                                                                                  |
| audio_starttime   | string      | Y            | 违规内容开始时间（绝对时间）                                                                                                                                                                                                                                                                                                                  |
| audioText         | string      | N            | 音频片段文本                                                                                                                                                                                                                                                                                                                                  |
| content           | string      | N            | returnPreText为true时返回违规内容前一个片段10秒文本和违规内容片段文本                                                                                                                                                                                                                                                                         |
| description       | string      | Y            | 策略规则风险原因描述<br/>注：该参数为旧版 API 返回参数，兼容保留，<br/>后续版本将去除，请勿依赖此参数，仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理                                                                                                                                                                                                                               |
| descriptionV2     | string      | Y            | 策略规则风险原因描述<br/>注：该参数为 API 返回参数<br/>请勿依赖此参数，仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理                                                                                                                                                                                                                                                               |
| matchedItem       | string      | N            | 命中的具体敏感词（该参数仅在命中敏感词时存在）                                                                                                                                                                                                                                                                                                |
| matchedList       | string      | N            | 命中敏感词所在的名单名称（该参数仅在命中<br/>敏感词时存在）                                                                                                                                                                                                                                                                                   |
| hits              | json_array  | Y            | 展示风险详情，请勿依赖此参数，仅供参考                                                                                                                                                                                                                                                                                                        |
| model             | string      | Y            | 规则标识，用来标识文本命中的策略规则。<br/>注：该参数为旧版 API 返回参数，兼容保留，<br/>后续版本将去除，请勿依赖此参数，仅供参考                                                                                                                                                                                                             |
| isSing            | int         | N            | type取值包含SING时存在，取值0表示检测不存在唱歌片段，取值1表示检测存在唱歌片段                                                                                                                                                                                                                                                                |
| requestParams     | json_object | Y            | 返回请求参数data中的所有字段                                                                                                                                                                                                           |
| riskType          | int         | Y            | <p>标识风险类型，可能取值:<br/>风险类型，静音时不返回，可能取值:<br/>0:正常</p><p>100:涉政/国歌</p><p>110:暴恐</p><p>200:色情</p><p>210:辱骂</p><p>250:娇喘</p><p>260:一号领导声纹</p><p>270:人声属性</p><p>280:违禁歌曲</p><p>300:广告</p><p>400:灌水</p><p>500:无意义</p><p>520:未成年人</p><p>600:违禁</p><p>700:其他</p><p>720:黑账号</p><p>730:黑IP</p><p>800:高危账号</p><p>900:自定义</p> |
| riskTypeDesc      | string      | N            | 风险原因描述                                                                                                                                                                                                                                                                                                                                  |
| room              | string      | Y            | 房间号                                                                                                                                                                                                                                                                                                                                        |
| userId            | int         | N            | AGORA分流情况下存在。返回的userId是实际房间中的用户id，与请求参数agoraParam中的uid无关。                                                   |
| strUserId         | string      | N            | TRTC、ZEGO、VOLC、GIN分流情况下存在。返回的strUserId是实际房间中的用户id。当流类型为TRTC分流，与请求参数trtcParam中的uid无关。 |
| vadCode           | int         | N            | <p>静音状态：</p><p>0 ：静音片段</p><p>1 ：非静音片段</p>                                                                                                                                                                                                                                                                                     |
| seiInfo           | array       | N            | <p>（需要联系数美开通）</p><p>展示流片段插入的SEI信息</p>                                                                                                                                                                                                                                                                                     |
| language          | json_array  | N            | 语种识别与概率值列表,在type下传入返回。                                                                                                                                                                                                                                                                                                       |
| minorLabel        | int         | N            | <p>当type传入MINOR且命中未成年人标签时，才会返回；</p><p>1：未成年人</p>                                                                                                                                                                                                                                                                      |
| businessLabels    | json_array  | Y            | 音频业务标签返回                                                                                                                                                                                                                                                                                                                             |
| tokenProfileLabels| json_array  | N            | 账号属性标签，仅在开启功能时返回                                                                                                                                                                                                                                                                                                                              |
| tokenRiskLabels   | json_array  | N            | 账号风险标签，仅在开启功能时返回                                                                                                                                                                                                                                                                                                                              |
| riskSource        | int         | Y            | <p>风险来源</p><p>1000：无风险</p><p>1001：文字</p><p>1003：音频</p>                                                                                                                                                                                                                                                                          |
| speakers          | json_array | N            | <p>该音频片段中说话人uid以及音量信息，每秒采集一次，一个片段不超过10次。</p><p>该结构是个数组，最多10个元素，按照相对时间排序，每个元素也是一个数组，包含当前说话人uid和音量大小</p><p>备注：目前仅在声网合流中生效</p>                                                                                                                                                  |

detail.language数组中每一项具体参数如下：

| 参数名称   | 类型 | 是否必选 | 说明                                                                         |
| :--------- | :--- | :------- | :--------------------------------------------------------------------------- |
| label      | int  | Y        | <p>语种识别类别标识，可能取值：</p><p>0:普通话</p><p>1:英语</p><p>2:粤语</p><p>3:藏语</p><p>4:维语</p><p>5:蒙语</p><p>6:朝鲜语</p><p>-1:其他语种</p> |
| confidence | int  | Y        | 对应语种标签可能性大小，取值0-100，数值越高表示概率越大。                    |
其中，businessLabels详细内容如下：

| 参数名              | 类型   | 参数说明     | 是否必返 | 备注         |
| :------------------ | :----- | :----------- | :------- | :----------- |
| businessLabel1      | string | 一级业务标签 | N       |              |
| businessLabel2      | string | 二级业务标签 | N       |              |
| businessLabel3      | string | 三级业务标签 | N       |              |
| businessDescription | string | 业务标签描述 | N       | 中文标签描述，仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |

其中，tokenProfileLabels，tokenRiskLabels 详细内容如下

| 参数名              | 类型   | 参数说明     | 是否必返 | 备注                                     |
|:-----------------| :----- | :----------- | :------- |:---------------------------------------|
| label1           | string   | 一级标签     | N           |                                        |
| label2           | string   | 二级标签     | N           |                                        |
| label3           | string   | 三级标签     | N           |                                        |
| description      | string   | 标签描述     | N           | 账号标签描述，仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |
| timestamp        | int      | 打标签时间戳 | N           | 13位Unix时间戳，单位：毫秒                       |

其中，auxInfo 的内容如下：

| **参数名称**      | **类型**    | **是否必选** | **说明**                                                                                                                                                                                                                                                                                                                                      |
| :---------------- | :---------- | :----------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| errorCode         | int         | Y           |<p>状态码</p><p>3001：流地址访问失败，例如资源HTTP状态码404、403</p><p>3002：流数据无效，例如“Invalid data found when processing input”</p><p>3003：流不存在，例如zego返回197612错误码</p><p>3004：流未返回音频数据</p><p>3005：拉流token无效或过期，建议使用新token重新开启审核，例如声网token过期或者trtc usersig无效</p>|
| streamTime       | int         | N           | 流结束后最后一次返回，代表送审时长，如有间隔审核逻辑时，和流真实时长可能不一致  |

其中，speakers 是个**二维数组**，每个元素的详细内容如下

| 参数名              | 类型   | 参数说明     | 是否必返 | 备注                                     |
|:-----------------| :----- | :----------- | :------- |:---------------------------------------|
| uid              | int      | 说话人uid   | Y           |                                        |
| volume           | int      | 音量大小     | Y           | 取值范围为 [0,255]                                       |

code和message的列表如下：

| **Code** | **message** |
| :------- | :---------- |
| 1100     | 成功        |
| 1901     | QPS超限、流路数超限  |
| 1902     | 参数不合法  |
| 1903     | 服务失败    |
| 1904     | 拉流失败    |
| 9100     | 余额不足    |
| 9101     | 无权限操作  |

### 示例

#### 请求示例

```bash
curl 'http://api-audiostream-bj.fengkongcloud.com/v2/saas/anti_fraud/audiostream' -d'{
    "accessKey": "",
    "type": "POLITY_EROTIC_MOAN_ADVERT",
    "appId": "default",
    "btid": "",
    "callback": "http://10.141.16.179:8900/",
    "data": {
        "streamType": "TRTC",
        "trtcParam": {
            "sdkAppId": 1400498247,
            "demoSences": 4,
            "userId": "12345",
            "userSig": "",
            "roomId": 517067780
        },
        "returnPreText": true,
        "returnPreAudio": true,
        "tokenId": "shumei-test",
        "channel": "",
        "returnAllText": true,
        "callbackParam": {
            "test1": 1,
            "test2": "qew",
            "test3": true
        },
    }
}'
```

#### 返回示例

```json
{
    "code":1100,
    "message":"成功",
    "requestId":" a78eef377079acc6cdec24967ecde722"
}
```

#### 回调接口返回的内容示例

```json
{
    "code": 1100,
    "message": "成功",
    "requestId": "a78eef377079acc6cdec24967ecde722_12345_0",
    "riskLevel": "REJECT",
    "detail": {
        "audioUrl": "[http://xxxx.mp3](http://xxxx.mp3/)",
        "preAudioUrl": "[http://prexxxx.mp3](http://prexxxx.mp3/)",
        "audio_endtime": "2018-09-18 17:54:31",
        "audio_starttime": "2018-09-18 17:54:21",
        "content": "啥鸡巴破地方啊，我发现我进传销了，兄弟们跟我过来当老板",
        "description": "色情内容",
        "matchedItem": "鸡巴",
        "matchedList": "色情",
        "model": "M1020_20",
        "requestParams":{
            "streamType": "TRTC",
            "trtcParam": {
                "sdkAppId": 1400498247,
                "demoSences": 4,
                "userId": "12345",
                "userSig": "",
                "roomId": 517067780
            },
            "returnPreText": true,
            "returnPreAudio": true,
            "tokenId": "shumei-test",
            "channel": "",
            "returnAllText": true,
            "callbackParam": {
                "test1": 1,
                "test2": "qew",
                "test3": true
            },
        },
        "riskType": 200,
        "riskTypeDesc": "色情",
        "room": "16037880",
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

## 2.2 音频流关闭通知接口

### 接口描述

该接口用于客户端通知服务端某个音频流已关闭。

### 请求URL

上海集群：

http://api-audiostream-sh.fengkongcloud.com/v2/saas/anti_fraud/finish_audiostream

硅谷集群：

http://api-audiostream-gg.fengkongcloud.com/v2/saas/anti_fraud/finish_audiostream

新加坡:

http://api-audiostream-xjp.fengkongcloud.com/v2/saas/anti_fraud/finish_audiostream

### 字符编码格式

请求及返回结果都使用UTF-8字符集进行编码

### 请求方法

POST

### 建议超时时长

1s

### 请求参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型** | **是否必选** | **说明**                               |
| :----------- | :------- | :----------- | :------------------------------------- |
| accessKey    | string   | Y            | 用于权限认证，开通账号服务时由数美提供 |
| requestId    | string   | Y            | 关闭的音频流的requestId                |

### 返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型** | **是否必选** | **说明**       |
| :----------- | :------- | :----------- | :------------- |
| code         | int      | Y            | 返回码         |
| message      | string   | Y            | 返回码详情描述 |
| requestId    | string   | Y            | 请求唯一标识   |

code和message的列表如下：

| **Code** | **message**               |
| :------- | :------------------------ |
| 1100     | 成功                      |
| 1901     | QPS超限、流路数超限         |
| 1902     | 参数不合法                |
| 1903     | 服务失败                  |
| 1904     | 拉流失败                  |
| 9100     | 余额不足                  |
| 9101     | 无权限操作或accessKey错误 |

### 示例

#### 请求示例

```bash
curl 'http://api-audiostream-sh.fengkongcloud.com/v2/saas/anti_fraud/finish_audiostream' -d'{
    "accessKey": "xxxxx",
    "requestId": "yyyy"
}'
```

#### 返回示例

```json
{
    "code":1100,
    "message":"成功",
    "requestId":" a78eef377079acc6cdec24967ecde722"
}
```

# 3. FAQ

## 3.1 调用接口返回参数错误（1902）

答：调用数美接口时，code返回1902参数不合法，一般为客户输入的参数格式存在问题，客户可自行分析一下请求格式是否按照接口文档输入，或将请求的数据及返回数据反馈给数美分析解决。

## 3.2 调用接口返回无权限操作（9101）

答：调用数美接口时，code返回9101无权限操作，一般为调用了未开通的服务，沟通确认客户调用的服务接口，开通相应的服务。

## 3.3 调用接口超时问题

答：有如下两个常见问题：

1）DNS问题:

客户通过公网调用数美接口进行测试，客户DNS解析域名较慢，导致第一次请求超时，建议客户更换DNS，不建议客户在host中将域名和ip做绑定，数美更换接口IP导致无法请求接口。

2）网络问题:

客户通过公网调用数美接口，公网网络延迟较长，导致少量请求存在超时。可以建议客户ping数美不同的集群网络，建议客户接入网络延迟较低的数美集群。

## 3.4 数美接口支持哪些网络协议？

数美音频流测试接口支持http、https、RTMP、HLS、HDL(HTTP-FLV)、RTP等所有主流网络协议。
