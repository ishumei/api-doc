
# 数美智能视频文件识别产品API文档
- - - - -

***版权所有 翻版必究***

- - - - -

* [视频文件上传请求](#uploadV2)
    + [接口描述](#uploadV2.interafceDesc)
    + [请求URL](#uploadV2.requestUrl)
    + [请求方法](#uploadV2.requestMethod)
    + [字符编码](#uploadV2.requestEncode)
    + [建议超时时间](#uploadV2.requestTimeout)
    + [视频格式限制](#uploadV2.requestVideoFormat)
    + [视频大小限制](#uploadV2.requestVideoLength)
    + [视频时长限制](#uploadV2.requestDurationLength)
    + [请求参数](#uploadV2.requestParameters)
    * [返回参数](#uploadV2.responseParameters)
* [异步回调结果](#callbackV2)
    + [接口描述](#callbackV2.interafceDesc)
    + [请求方法](#callbackV2.callbackMethod)
    + [字符编码](#callbackV2.callbackEncode)
    + [建议超时时间](#callbackV2.callbackTimeout)
    + [支持协议](#callbackV2.callbackProtocol)
    + [回调策略](#callbackV2.callbackStrategy)
    + [回调参数](#callbackV2.callbackParameters)
* [查询视频结果请求](#queryV2)
    + [接口描述](#queryV2.interafceDesc)
    + [请求URL](#queryV2.requestUrl)
    + [请求方法](#queryV2.requestMethod)
    + [支持协议](#queryV2.requestProtocol)
    + [字符编码](#queryV2.requestEncode)
    + [建议超时时间](#queryV2.requestTimeout)
    + [请求参数](#queryV2.requestParameters)
    + [返回参数](#queryV2.responseParameters)
* [示例](#Demo)
    + [上传接口请求示例](#demo.requestuploadV2)
    + [上传接口返回示例](#demo.responseuploadV2)
    + [异步回调结果示例](#demo.callbackV2)

## <span id = "uploadV2">视频文件上传请求</span>

### <span id = "uploadV2.interafceDesc">接口描述</span>

该接口用于提交视频相关信息，自定义截帧频率等参数。识别结果需客户自行定期调用查询接口获取。

### <span id = "uploadV2.requestUrl">请求URL：</span>
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-video-bj.fengkongcloud.com/v2/saas/anti_fraud/video` | 中文视频文件 |
| 上海 | `http://api-video-sh.fengkongcloud.com/v2/saas/anti_fraud/video` | 中文视频文件 |
| 新加坡 | `http://api-video-xjp.fengkongcloud.com/v2/saas/anti_fraud/video` | 中文视频文件 |
| 硅谷 | `http://api-video-gg.fengkongcloud.com/v2/saas/anti_fraud/video` | 中文视频文件 |
| 印度 | `http://api-video-yd.fengkongcloud.com/v2/saas/anti_fraud/video` | 中文视频文件 |

### <span id = "uploadV2.requestMethod">请求方法：</span>

`POST` 

### <span id = "uploadV2.requestEncode">字符编码：</span>

`UTF-8`

### <span id = "uploadV2.requestTimeout">建议超时时间：</span>

1s

### <span id = "uploadV2.requestVideoFormat">视频格式限制：</span>

`AVI`、`FLV`、`MP4`、`MPG`、`WMV`、`MOV`、`WMA`、`RMVB`

### <span id = "uploadV2.requestVideoLength">视频大小限制：</span>

小于等于300MB

### <span id = "uploadV2.requestDurationLength">视频时长限制：</span>

小于等于2小时

### <span id = "uploadV2.requestParameters">请求参数：</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 公司密钥 | 必传参数 | 由数美提供，数美分配 |
| appId | string | 应用标识 | 必传参数 | 用于区分应用默认应用值：default |
| btId | string | 视频唯一标识 | 必传参数 | 视频唯一标识，用于查询识别结果，最长64位 |
| imgType | string | 视频中的画面需要识别的监管类型，**和imgBusinessType至少传一个** | 非必传参数 | 监管一级标签<br/>可选值：<br/>`POLITICS`：涉政识别, 这里POLITICS实际识别内容为涉政人物和暴恐<br/>`PERSON`涉政人物识别<br/>`VIOLENCE`：暴恐识别<br/>`PORN`：色情识别<br/>`AD`：广告识别<br/>`OCR`：图片中的文字风险识别<br/>`BEHAVIOR`：不良场景识别,支持吸烟、喝酒、赌博、吸毒、避孕套和无意义画面<br/>如果需要识别多个功能，通过下划线连接，如`AD_PORN_POLITICS`用于广告、色情和涉政组合识别 |
| audioType | string | 视频中的音频需要识别的监管类型 | 非必传参数 | 监管一级标签<br/>可选值：<br/>`POLITICS`：涉政识别<br/>`PORN`：色情识别<br/>`AD`：广告识别<br/>`MOAN`：娇喘识别<br/>`ABUSE`：辱骂识别<br/>`ANTHEN`：国歌识别<br/>`AUDIOPOLITICAL`：声音涉政<br/>`NONE`:不检测音频<br/>如需做组合识别，通过下划线连接即可，例如`POLITICS_PORN_MOAN`用于广告、色情和涉政识别 |
| imgBusinessType | string | 视频中的画面需要识别的业务类型， **和imgType至少传一个** | 非必传参数 | 业务一级标签<br/>可选值：<br/>`SCREEN`：特殊画面识别<br/>`SCENCE`：场景画面识别<br/>`QR`：二维码识别<br/>`FACE`：人脸识别<br/>`QUALITY`：图像质量识别<br/>`MINOR`：未成年人识别<br/>`LOGO`：商企LOGO识别<br/>`BEAUTY`：颜值识别<br/>`OBJECT`：物品识别<br/>`STAR`：公众人物识别<br/>如需做组合识别，通过下划线连接即可，例如`QR_FACE_MINOR`用于二维码、人脸和未成年人识别 |
| audioBusinessType | String | 视频中的音频业务识别类型 | 非必传参数 | 业务一级标签<br/>可选值：<br/>`SING`：唱歌识别<br/>`LANGUAGE`：语种识别<br/>`MINOR`：未成年人识别<br/>`GENDER`：性别识别<br/>`TIMBRE`：音色识别，需要同时传入`GENDER`才能生效 |
| callback | string | 指定回调url地址 | 非必传参数 | 当该字段非空时，服务将根据该字段回调通知用户审核结果（支持`http`/`https`） |
| callbackParam | json_object | 回调透传字段 | 非必传参数 |  |
| data | json\_object | 本次请求相关信息，最长1MB | 必传参数 | 最长1MB，其中[data内容如下](#uploadV2.requestParams.data) |

<span id = "uploadV2.requestParams.data">其中，data的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| url | string | 要检测的视频url地址 | 必传参数 | |
| tokenId | string | | 必传参数 | 客户端用户账号唯一标识，用于用户行为分析，建议传入用户UID； 最长40位 |
| detectFrequency | float | 截帧频率间隔，单位为秒 | 非必传参数 | 取值范围为0.5~60s；如不传递默认5s截帧一次 |
| ip | string | 客户端IP | 非必传参数 | 用于IP维度的用户行为分析，同时可用于比对数美IP黑库 |
| retallImg | int | | 非必传参数 | 选择返回视频截帧图片的等级：0：返回风险等级为非pass的图片1：返回所有风险等级的图片默认为0 |
| retallAudio | int | | 非必传参数 | 选择返回视频音频片段的等级：0：返回风险等级为非pass的音频片段1：返回所有风险等级的音频片段默认为0 |
| videoName | string | 视频名称 | 非必传参数 | 视频名称，用于后台界面展示 |
| subtitle | string | 视频字幕 | 非必传参数 | 文本内容审核 |
| videoCover | string | 视频封面 | 非必传参数 | 视频封面图片审核 |
| channel | string | 客户渠道（不传为默认值） | 非必传参数 |[渠道配置表示例参考](#uploadV2.channel)  |

<span id="uploadV2.channel">data 中，channel的内容如下</span>

数美根据客户不同业务场景，配置不同的渠道（channel），制定针对性的拦截策略，同时也方便客户针对不同业务场景的数据进行筛选、分析。业务场景和渠道取值对应表如下（支持客户自定义）

| **业务场景** | **channel取值** | **传入说明** |
| --- | --- | --- |
| 短视频 | SHORT_VIDEO |时长在分钟级别的短片视频 |
| 群聊 | GROUP_CHAT |社交场景群聊发送的视频消息 |
| 私聊 | MESSAGE |社交场景私聊发送的视频消息 |
| 录播视频 | VIDEO |视频平台产生的视频文件 |

### <span id = "uploadV2.responseParameters">返回参数</span>：

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 本次请求的唯一标识 | 是 | 请求唯一标识 |
| code | int | 请求返回码 | 是 | 详见[接口响应码列表](#codeList) |
| message | string | 请求返回描述 | 是 | 详情描述如上 |
| btId | string | 唯一标识客户上传的视频 | 是 | 仅当code=1100时返回，与请求参数中的btId字段对应 |

## <span id = "callbackV2">异步回调结果</span>

### <span id = "callbackV2.interafceDesc">接口描述</span>

用户如果需要服务端主动对视频检测结果进行回调，则需要在请求参数中指定回调协议接口URL callback参数，服务端根据该参数在视频审核完成后，主动回调用户。


### <span id = "callbackV2.callbackMethod">请求方法：</span>

`POST` 

### <span id = "callbackV2.callbackEncode">字符编码：</span>

`UTF-8`

### <span id = "callbackV2.callbackTimeout">建议超时时间：</span>

5s

### <span id = "callbackV2.callbackProtocol">支持协议：</span>

`HTTP`或`HTTPS`

### <span id="callbackV2.callbackStrategy">回调策略：</span>

当用户收到推送结果，并返回HTTP状态码为200时，表示推送成功；否则系统将进行重试，最多20次推送。


### <span id = "callbackV2.callbackParameters">回调参数</span>：

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| checksum | string | 由accessKey + btId + result拼成字符串，通过SHA256算法生成。为防止篡改，可以按此算法生成字符串，与checksum做一次校验。 | 是 |  |
| result | string | 机器审核结果 | 是 | 详见[result字段说明](#callbackV2.result) |

<span id="callbackV2.result">result可反序列化为json结构，内容如下</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | 详见[接口响应码列表](#codeList) |
| message | string | 返回码详情描述 | 是 | |
| requestId | string | 请求唯一标识 | 是 | |
| btId | string | 视频唯一标识 | 是 | 最长64位 |
| riskLevel | string | 风险级别，code为1100时存在 | 否 | 返回值：<br/>`PASS`：正常内容，建议直接放行<br/>`REVIEW`：可疑内容，建议人工审核<br/>`REJECT`：违规内容，建议直接拦截 |
| labels | string | 风险标签（code为1100时存在）| 否|  |
| detail | json_array | 风险详情 | 否 | code为`1100`时存在，详见[detail说明](#callbackV2.callbackParameters.frameDetail) |
| addition | json_array | 音频片段信息 | 否 | code为`1100`时存在，详见[addition说明](#callbackV2.callbackParameters.audioDetail)|
| callbackParam | json_object | 回调透传字段 | 否 | 当提交请求中，callbackParam字段有值时存在 |
| auxInfo | json_object | 辅助信息 | 否 |code为`1100`时存在，详见[auxInfo说明](#callbackV2.callbackParameters.auxInfo)|
| tokenProfileLabels | json_array | 账号属性标签 | 否 |仅在开启功能时返回，详见[tokenProfileLabels说明](#callbackV2.callbackParameters.tokenProfileLabels)|
| tokenRiskLabels | json_array | 账号风险标签 | 否 |仅在开启功能时返回，详见[tokenRiskLabels说明](#callbackV2.callbackParameters.tokenRiskLabels)|

<span id = "callbackV2.callbackParameters.auxInfo">其中，auxInfo中的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| frameCount | int | 辅助信息 | 是 | 视频文件的截帧总数 |
| time | int | 视频时长 | 是 | 视频时长 |

<span id = "callbackV2.callbackParameters.frameDetail">其中，截帧图片detail数组中每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 截帧片段的requestId | 是 |  |
| time | float | 截帧在视频文件中的时间，单位为秒 | 是 | 截帧图片相对视频文件的时间 |
| similarity | float | 返回当前截帧图片和上一帧截帧图片的相似度 | 是 |注意：有图片则该字段就会返回，<br/>视频文件初始第一帧将比对纯黑背景图片 |
| imgUrl | string | 当前截帧的URL | 是 | |
| riskLevel | string | 当前截帧的处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| imgText | string | 截帧图片OCR文本内容 | 否 | 截帧图片OCR文字识别，识别类型包含OCR时会有 |
| qrContent | string | 截帧图片二维码链接识别，如有需要可联系数美开启| 否 | 注意：开启该功能后，只有完整，<br/>可以正常识别到的二维码才会返回<br/>（imgType传值需要包含AD） |
| riskType | int | 截帧图片风险类型 | 否 | 标识风险类型，可能取值：<br/>0：       正常<br/>100：涉政<br/>200：色情<br/>210：性感<br/>300：广告<br/>310：二维码<br/>320：水印<br/>400：暴恐<br/>500：违规<br/>510：不良场景<br/>520：未成年人<br/>530：人脸<br/>531：人像<br/>532：伪造人脸<br/>533：颜值<br/>535：公众人物<br/>540：物品<br/>541：动物<br/>542：植物<br/>550：场景<br/>560：行业违规<br/>570：画面属性<br/>700：黑名单<br/>710：白名单<br/>800：高危账号<br/>900：自定义 |
| riskSource | int | 风险来源 | 否 | 风险来源，可能取值：<br/>1000：无风险<br/>1001：文字风险 <br/>1002：视觉图片风险 <br/>1003：音频语音风险 |
| description | string | 风险原因 | 是 | |
| matchedItem | string | 图片文字命中的违规敏感词 | 否 |  |
| matchedList | string | 图片文字命中的违规名单 | 否 |  |
| polityName | string | 识别出的涉政人物名字（当TYPE传入PERSON或POLITICS时并检测到涉政人物时存在） | 否 |  |
| violenceLabel | string | 暴恐场景分类（当TYPE传入VIOENCE或POLITICS时并检测到暴恐场景时存在） | 否 |  |
| logos | string | 返回视频帧识别出来的logo结果<br/>如："logos": ["douyin"] | 否 |  |
| businessLabels | string | 识别出的业务标签 | 否 |传入imgBusinessType时返回，详见[businessLabels说明](#callbackV2.callbackParameters.frameDetail.businessLabels)   |

<span id = "callbackV2.callbackParameters.frameDetail.businessLabels">detail中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| probability         | float    | 置信度       | 是           | 可选值为0~1，值越大，可信度越高 |

<span id = "callbackV2.callbackParameters.addition">其中，音频片段addition中每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| audio_evidence | json_array | 音频片段详情数组 | 否 | 当请求参数retallAudio传值为1时<br/>数组中将包含正常内容片段的相关详情，否则只返回非PASS的片段的相关详情，详见[audio_evidence说明](#callbackV2.callbackParameters.audioEvidence) |
| subtitleDetail | json_array | 视频字幕存在时返回 | 否 | 视频字幕 |
| videoCoverDetail | json_array | 视频封面存在时返回 | 否 | 视频封面 |

<span id="callbackV2.callbackParameters.audioEvidence">addition中，audio_evidence的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| audioModel | string | 命中规则编号 | 是 |  |
| audioText | string | 返回音转文文字 | 否 | |
| audio_starttime | string | 违规音频发生时间 | 否 | |
| audio_endtime | string | 违规音频结束时间 | 否 | |
| audio_url | string | 音频片段地址 | 否 | |
| audio_matchedItem | string | 违规音频敏感词内容 | 否 | |
| description | string | 音频片段风险原因描述 | 是 | |
| requestId | string | 音频片段唯一标识 | 是 | 一般用户历史记录查询某一片段内容 |
| riskLevel | string | 处置建议 | 是 | 取值范围：<br/>PASS：正常 <br/>REVIEW：审核 <br/>REJECT：拒绝|
| riskType | int | 风险类型 | 是 | 标识风险类型，可能取值：<br/>0：正常<br/>100：涉政<br/>110: 暴恐<br/>200：色情<br/>210：辱骂<br/>250：娇喘<br/>300：广告<br/>400：灌水<br/>500：无意义<br/>600 : 违禁<br/>700：其他<br/>720：黑账号<br/>730：黑IP<br/>800：高危账号<br/>900：自定义 |
| riskSource | int  | 音频转译文本的结果 | 是 |风险来源，可能取值：<br/>1000：无风险 <br/>1001：文字 <br/>1002：视觉图片 <br/>1003：音频语音 |
| isSing | int  | 该条音频片段是否唱歌 | 否 | 取值范围：<br/>0:表示没有唱歌，<br/>1:表示唱歌。<br/>仅当type传入值包含SING时返回。 |
| language | json_array | 语种标签与概率值列表 | 否| 详见[language说明](#callbackV2.callbackParameters.audioDetail.language) |
| businessLabels | json_array | 识别出的业务标签 | 否 | 详见[businessLabels说明](#callbackV2.callbackParameters.audioDetail.businessLabels) |

<span id="callbackV2.callbackParameters.audioDetail.language">音频的audio_evidence中，language数组中每一项具体参数如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| label | int | 语种识别类别 | 是 |语种识别类别标识，可能取值：<br/>0:普通话<br/>1:英语<br/>2:粤语 |
| probability | int | 对应音色标签可能性大小，取值0-100，数值越高表示概率越大 | 是 | 取值范围[0,100] |

<span id="callbackV2.callbackParameters.audioDetail.businessLabels">audio_evidence中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |

<span id="callbackV2.callbackParameters.tokenProfileLabels">其中，tokenProfileLabels数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| label1 | string | 一级标签 | 否 | |
| label2 | string | 二级标签 | 否 | |
| label3 | string | 三级标签 | 否 | |
| description | string | 标签描述 | 否 | |
| timestamp | int | 打标签时间戳 | 否 | 13位Unix时间戳，单位：毫秒 |

<span id="callbackV2.callbackParameters.tokenRiskLabels">其中，tokenRiskLabels数组每个成员的具体字段同tokenProfileLabels</span>


## <span id = "queryV2">查询视频结果</span>

该接口用于客户主动查询视频文件识别结果，建议每30s进行一次查询。


### <span id = "queryV2.interafceDesc">接口描述</span>

该接口用于客户主动查询视频文件识别结果，建议每30s进行一次查询。

### <span id = "queryV2.requestUrl">请求URL：</span>
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-video-bj.fengkongcloud.com/v2/saas/anti_fraud/query_video` | 中文视频文件 |
| 上海 | `http://api-video-sh.fengkongcloud.com/v2/saas/anti_fraud/query_video` | 中文视频文件 |
| 新加坡 | `http://api-video-xjp.fengkongcloud.com/v2/saas/anti_fraud/query_video` | 中文视频文件 |
| 硅谷 | `http://api-video-gg.fengkongcloud.com/v2/saas/anti_fraud/query_video` | 中文视频文件 |
| 印度 | `http://api-video-yd.fengkongcloud.com/v2/saas/anti_fraud/query_video` | 中文视频文件 |

### <span id = "queryV2.requestMethod">请求方法：</span>

`POST` 

### <span id = "queryV2.requestProtocol">支持协议：</span>

`HTTP`或`HTTPS`

### <span id = "queryV2.requestEncode">字符编码：</span>

`UTF-8`

### <span id = "queryV2.requestTimeout">建议超时时间：</span>

1s

### <span id = "queryV2.requestParameters">请求参数：</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 用于权限认证，开通账号服务时由数美提供 | 必传参数 | |
| btId | string | 视频唯一标识，用于查询识别结果，最长64位 | 必传参数 | |

### <span id = "queryV2.responseParameters"> 返回参数</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | 详见[接口响应码列表](#codeList) |
| message | string | 返回码详情描述 | 是 | |
| requestId | string | 请求唯一标识 | 是 | |
| btId | string | 视频唯一标识 | 是 | 最长64位 |
| riskLevel | string | 风险级别，code为1100时存在 | 否 | 返回值：<br/>`PASS`：正常内容，建议直接放行<br/>`REVIEW`：可疑内容，建议人工审核<br/>`REJECT`：违规内容，建议直接拦截 |
| labels | string | 风险标签（code为1100时存在）| 否|  |
| detail | json_array | 风险详情 | 否 | code为`1100`时存在，详见[detail说明](#callbackV2.callbackParameters.query..frameDetail) |
| addition | json_array | 音频片段信息 | 否 | code为`1100`时存在，详见[addition说明](#callbackV2.callbackParameters.query.audioDetail)|
| tokenProfileLabels | json_array | 账号属性标签 | 否 |仅在开启功能时返回，详见[tokenProfileLabels说明](#callbackV2.callbackParameters..query.query.tokenProfileLabels)|
| tokenRiskLabels | json_array | 账号风险标签 | 否 |仅在开启功能时返回，详见[tokenRiskLabels说明](#callbackV2.callbackParameters.query.query.tokenRiskLabels)|

<span id = "queryV2.responseParameters.query.frameDetail">其中，detail中每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 截帧片段的requestId | 是 |  |
| time | float | 截帧在视频文件中的时间，单位为秒 | 是 | 截帧图片相对视频文件的时间 |
| similarity | float | 返回当前截帧图片和上一帧截帧图片的相似度 | 是 |注意：有图片则该字段就会返回，<br/>视频文件初始第一帧将比对纯黑背景图片 |
| imgUrl | string | 当前截帧的URL | 是 | |
| riskLevel | string | 当前截帧的处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| imgText | string | 截帧图片OCR文本内容 | 否 | 截帧图片OCR文字识别，识别类型包含OCR时会有 |
| qrContent | string | 截帧图片二维码链接识别，如有需要可联系数美开启| 否 | 注意：开启该功能后，只有完整，<br/>可以正常识别到的二维码才会返回<br/>（imgType传值需要包含AD） |
| riskType | int | 截帧图片风险类型 | 否 | 标识风险类型，可能取值：<br/>0：       正常<br/>100：涉政<br/>200：色情<br/>210：性感<br/>300：广告<br/>310：二维码<br/>320：水印<br/>400：暴恐<br/>500：违规<br/>510：不良场景<br/>520：未成年人<br/>530：人脸<br/>531：人像<br/>532：伪造人脸<br/>533：颜值<br/>535：公众人物<br/>540：物品<br/>541：动物<br/>542：植物<br/>550：场景<br/>560：行业违规<br/>570：画面属性<br/>700：黑名单<br/>710：白名单<br/>800：高危账号<br/>900：自定义 |
| riskSource | int | 风险来源 | 否 | 风险来源，可能取值：<br/>1000：无风险<br/>1001：文字风险 <br/>1002：视觉图片风险 <br/>1003：音频语音风险 |
| description | string | 风险原因 | 是 | |
| matchedItem | string | 图片文字命中的违规敏感词 | 否 |  |
| matchedList | string | 图片文字命中的违规名单 | 否 |  |
| polityName | string | 识别出的涉政人物名字（当TYPE传入PERSON或POLITICS时并检测到涉政人物时存在） | 否 |  |
| violenceLabel | string | 暴恐场景分类（当TYPE传入VIOENCE或POLITICS时并检测到暴恐场景时存在） | 否 |  |
| logos | string | 返回视频帧识别出来的logo结果<br/>如："logos": ["douyin"] | 否 |  |
| businessLabels | string | 识别出的业务标签 | 否 |传入imgBusinessType时返回，详见[businessLabels说明](#callbackV2.callbackParameters.frameDetail.query.businessLabels)   |


<span id = "callbackV2.callbackParameters.frameDetail.query.businessLabels">detail中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |

<span id = "callbackV2.callbackParameters.query.audioDetail">其中，音频片段addition中每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| audio_evidence | json_array | 音频片段详情数组 | 否 | 当请求参数retallAudio传值为1时<br/>数组中将包含正常内容片段的相关详情，否则只返回非PASS的片段的相关详情，详见[audio_evidence说明](#callbackV2.callbackParameters.query.audioEvidence) |
| subtitleDetail | json_array | 视频字幕存在时返回 | 否 | 视频字幕 |
| videoCoverDetail | json_array | 视频封面存在时返回 | 否 |视频封面  |

<span id="callbackV2.callbackParameters.query.audioEvidence">addition中，audio_evidence的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| audioModel | string | 命中规则编号 | 是 |  |
| audioText | string | 返回音转文文字 | 否 | |
| audio_starttime | string | 违规音频发生时间 | 否 | |
| audio_endtime | string | 违规音频结束时间 | 否 | |
| audio_url | string | 音频片段地址 | 否 | |
| audio_matchedItem | string | 违规音频敏感词内容 | 否 | |
| description | string | 音频片段风险原因描述 | 是 | |
| requestId | string | 音频片段唯一标识 | 是 | 一般用户历史记录查询某一片段内容 |
| riskLevel | string | 处置建议 | 是 | 取值范围：<br/>PASS：正常 <br/>REVIEW：审核 <br/>REJECT：拒绝|
| riskType | int | 风险类型 | 是 | 标识风险类型，可能取值：<br/>0：正常<br/>100：涉政<br/>110: 暴恐<br/>200：色情<br/>210：辱骂<br/>250：娇喘<br/>300：广告<br/>400：灌水<br/>500：无意义<br/>600 : 违禁<br/>700：其他<br/>720：黑账号<br/>730：黑IP<br/>800：高危账号<br/>900：自定义 |
| riskSource | int  | 音频转译文本的结果 | 是 |风险来源，可能取值：<br/>1000：无风险 <br/>1001：文字 <br/>1002：视觉图片 <br/>1003：音频语音 |
| isSing | int  | 该条音频片段是否唱歌 | 否 | 取值范围：<br/>0:表示没有唱歌，<br/>1:表示唱歌。<br/>仅当type传入值包含SING时返回。 |
| language | json_array | 语种标签与概率值列表 | 否| 详见[language说明](#callbackV2.callbackParameters.audioDetail.query.language) |
| businessLabels | json_array | 识别出的业务标签 | 否 | 详见[businessLabels说明](#callbackV2.callbackParameters.audioDetail.query.businessLabels) |

<span id="callbackV2.callbackParameters.audioDetail.query.language">音频的audio_evidence中，language数组中每一项具体参数如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| label | int | 语种识别类别 | 是 |语种识别类别标识，可能取值：<br/>0:普通话<br/>1:英语<br/>2:粤语 |
| probability | int | 对应音色标签可能性大小，取值0-100，数值越高表示概率越大 | 是 | 取值范围[0,100] |

<span id="callbackV2.callbackParameters.audioDetail.query.businessLabels">audio_evidence中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |

<span id="callbackV2.callbackParameters.query.tokenProfileLabels">其中，tokenProfileLabels数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| label1 | string | 一级标签 | 否 | |
| label2 | string | 二级标签 | 否 | |
| label3 | string | 三级标签 | 否 | |
| description | string | 标签描述 | 否 | |
| timestamp | int | 打标签时间戳 | 否 | 13位Unix时间戳，单位：毫秒 |

<span id="callbackV2.callbackParameters.query.tokenRiskLabels">其中，tokenRiskLabels数组每个成员的具体字段同tokenProfileLabels</span>


## 接口响应码列表

<span id="codeList">code请求返回码列表如下：</span>

| **code** | **message**      |
| -------- | ---------------- |
| 1100     | 成功             |
| 1901     | QPS超限          |
| 1902     | 参数不合法       |
| 1903     | 服务失败         |
| 1907     | 获取视频长度超时 |
| 9100     | 余额不足         |
| 9101     | 无权限操作       |


## <span id = "Demo">示例</span>

### <span id = "demo.requestuploadV2">上传接口请求示例：</span>

```json
{
    "accessKey": "**********",
    "appId": "default",
    "btId": "1639824316368",
    "imgType": "POLITICS_VIOLENCE_BAN_PORN_MINOR_AD_SPAM_LOGO_STAR_OCR",
    "imgBusinessType": "SCREEN_SCENCE_QR_FACE_QUALITY_MINOR_LOGO_BEAUTY",
    "audioType": "POLITICAL_PORN_AD_MOAN_ABUSE",
    "audioBusinessType": "SING_LANGUAGE_MINOR_GENDER_TIMBRE",
    "callback": "http://www.xxx.top/xxx",
    "data": {
        "channel": "video",
        "detectFrequency": 3,
        "tokenId": "test",
        "ip":"123.171.34.3",
        "url": "http://oss.xxx.com/static/photo/117608703147396.mp4",
        "retallAudio": 1,
        "retallImg": 1,
        "callbackParam": {
            "passThrough": {
                "passThrough1": "透传字段1",
                "passThrough2": "透传字段2",
                "passThrough3": "透传字段3"
            }
        }
    }
}
```


### <span id = "demo.responseuploadV2">上传接口返回示例：</span>

```json
{
    "code": 1100,
    "message": "成功",
    "requestId": "1639824316368",
    "btId": "1639824316368"
}
```

### <span id = "demo.callbackV2">异步回调结果示例：</span>

```json
{
    "checksum":"fcb7908131854c7bd451194a7d87f0d832bbd724bd2affbb70d75e109ed4c243",
    "result":"{\"code\":1100,\"message\":\"\\u6210\\u529f\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8\",\"btId\":\"515032468\",\"labels\":\"\\u97f3\\u9891\\u6587\\u5b57:\\u8272\\u60c5\\uff1a\\u6027\\u9a9a\\u6270\\uff1a\\u91cd\\u5ea6\\u6027\\u9a9a\\u6270-\\u97f3\\u9891 \\u6b63\\u5e38-\\u56fe\\u50cf \",\"detail\":[{\"description\":\"\\u6b63\\u5e38-\\u56fe\\u50cf\",\"imgUrl\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v0.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D2d0c13df3ea2235b4fc97dfdeb8ac2b6357f05a6\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_v0\",\"riskLevel\":\"PASS\",\"riskSource\":1000,\"riskType\":0,\"similarity\":0,\"time\":0},{\"description\":\"\\u6b63\\u5e38-\\u56fe\\u50cf\",\"imgText\":\"0D\\u5165\",\"imgUrl\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v5.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D1445219d6ffc3101c66defb8931c5d8e99be1e7c\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_v5\",\"riskLevel\":\"PASS\",\"riskSource\":1000,\"riskType\":0,\"similarity\":0.3828125,\"time\":5},{\"description\":\"\\u6b63\\u5e38-\\u56fe\\u50cf\",\"imgText\":\"LD\",\"imgUrl\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v10.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3Dd073b20f6472b10f324659c1d99f42a2b2b4ab8d\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_v10\",\"riskLevel\":\"PASS\",\"riskSource\":1000,\"riskType\":0,\"similarity\":0.6640625,\"time\":10},{\"description\":\"\\u6b63\\u5e38-\\u56fe\\u50cf\",\"imgText\":\"\\u5c71\",\"imgUrl\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v15.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D02fefdc10611061b450c1ec64e1342442e8206fe\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_v15\",\"riskLevel\":\"PASS\",\"riskSource\":1000,\"riskType\":0,\"similarity\":0.77734375,\"time\":15},{\"description\":\"\\u6b63\\u5e38-\\u56fe\\u50cf\",\"imgUrl\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v20.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D40829c647570c532d8faab28c36f0898757eaed3\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_v20\",\"riskLevel\":\"PASS\",\"riskSource\":1000,\"riskType\":0,\"similarity\":0.7421875,\"time\":20},{\"description\":\"\\u6b63\\u5e38-\\u56fe\\u50cf\",\"imgText\":\"#######\",\"imgUrl\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v25.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D64701f589de799dfed252e6a7b86d2db1764b255\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_v25\",\"riskLevel\":\"PASS\",\"riskSource\":1000,\"riskType\":0,\"similarity\":0.796875,\"time\":25},{\"description\":\"\\u6b63\\u5e38-\\u56fe\\u50cf\",\"imgUrl\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v30.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D97c04340256ec3394baa010f9becdc30d971669d\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_v30\",\"riskLevel\":\"PASS\",\"riskSource\":1000,\"riskType\":0,\"similarity\":0.7265625,\"time\":30}],\"auxInfo\":{\"frameCount\":7,\"time\":31},\"addition\":{\"audio_evidence\":[{\"audioModel\":\"MA000003002001000\",\"audioText\":\"\\u60f3\\u4f60\\u7b49\\u4e94\\u767e\\u5934\\u6211\\u8fd8\\u662f\\u4e00\\u6837\\u559c\\u6b22\\u4f60\\uff0c\\u81ea\\u6170\\uff0c\\u7684\",\"audio_endtime\":30,\"audio_starttime\":20,\"audio_url\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/audio%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_a0002.wav?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D95d7c542d6546f85d3801f06744991620806f982\",\"description\":\"\\u97f3\\u9891\\u6587\\u5b57:\\u8272\\u60c5\\uff1a\\u6027\\u9a9a\\u6270\\uff1a\\u91cd\\u5ea6\\u6027\\u9a9a\\u6270-\\u97f3\\u9891\",\"isSing\":0,\"language\":[{\"confidence\":0.0314832,\"label\":2},{\"confidence\":99.9603,\"label\":0},{\"confidence\":0.0366747,\"label\":1}],\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_a0002\",\"riskLevel\":\"REJECT\",\"riskSource\":1001,\"riskType\":200}]},\"riskLevel\":\"REJECT\"}"
    
}
```

### <span id = "demo.callbackV2.query">查询接口结果示例：</span>

```json

{
    "code":1100,
    "message":"成功",
    "requestId":"25321f666ac62c57e2d59ac47e815350",
    "btId":"515032468",
    "labels":"音频文字:色情：性骚扰：重度性骚扰-音频 正常-图像 ",
    "detail":[
        {
            "description":"正常-图像",
            "imgUrl":"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v0.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D2d0c13df3ea2235b4fc97dfdeb8ac2b6357f05a6",
            "requestId":"e7a59eebd431415e92684cb6151c4de8_v0",
            "riskLevel":"PASS",
            "riskSource":1000,
            "riskType":0,
            "similarity":0,
            "time":0
        },
        {
            "description":"正常-图像",
            "imgText":"0D入",
            "imgUrl":"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v5.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D1445219d6ffc3101c66defb8931c5d8e99be1e7c",
            "requestId":"e7a59eebd431415e92684cb6151c4de8_v5",
            "riskLevel":"PASS",
            "riskSource":1000,
            "riskType":0,
            "similarity":0.3828125,
            "time":5
        },
        {
            "description":"正常-图像",
            "imgText":"LD",
            "imgUrl":"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v10.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3Dd073b20f6472b10f324659c1d99f42a2b2b4ab8d",
            "requestId":"e7a59eebd431415e92684cb6151c4de8_v10",
            "riskLevel":"PASS",
            "riskSource":1000,
            "riskType":0,
            "similarity":0.6640625,
            "time":10
        },
        {
            "description":"正常-图像",
            "imgText":"山",
            "imgUrl":"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v15.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D02fefdc10611061b450c1ec64e1342442e8206fe",
            "requestId":"e7a59eebd431415e92684cb6151c4de8_v15",
            "riskLevel":"PASS",
            "riskSource":1000,
            "riskType":0,
            "similarity":0.77734375,
            "time":15
        },
        {
            "description":"正常-图像",
            "imgUrl":"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v20.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D40829c647570c532d8faab28c36f0898757eaed3",
            "requestId":"e7a59eebd431415e92684cb6151c4de8_v20",
            "riskLevel":"PASS",
            "riskSource":1000,
            "riskType":0,
            "similarity":0.7421875,
            "time":20
        },
        {
            "description":"正常-图像",
            "imgText":"#######",
            "imgUrl":"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v25.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D64701f589de799dfed252e6a7b86d2db1764b255",
            "requestId":"e7a59eebd431415e92684cb6151c4de8_v25",
            "riskLevel":"PASS",
            "riskSource":1000,
            "riskType":0,
            "similarity":0.796875,
            "time":25
        },
        {
            "description":"正常-图像",
            "imgUrl":"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v30.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D97c04340256ec3394baa010f9becdc30d971669d",
            "requestId":"e7a59eebd431415e92684cb6151c4de8_v30",
            "riskLevel":"PASS",
            "riskSource":1000,
            "riskType":0,
            "similarity":0.7265625,
            "time":30
        }
    ],
    "addition":{
        "audio_evidence":[
            {
                "audioModel":"MA000003002001000",
                "audioText":"想你等五百头我还是一样喜欢你，自慰，的",
                "audio_endtime":30,
                "audio_starttime":20,
                "audio_url":"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/audio%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_a0002.wav?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D95d7c542d6546f85d3801f06744991620806f982",
                "description":"音频文字:色情：性骚扰：重度性骚扰-音频",
                "isSing":0,
                "language":[
                    {
                        "confidence":0.0314832,
                        "label":2
                    },
                    {
                        "confidence":99.9603,
                        "label":0
                    },
                    {
                        "confidence":0.0366747,
                        "label":1
                    }
                ],
                "requestId":"e7a59eebd431415e92684cb6151c4de8_a0002",
                "riskLevel":"REJECT",
                "riskSource":1001,
                "riskType":200
            }
        ]
    },
    "riskLevel":"REJECT"
}

```