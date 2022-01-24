# 数美智能视频文件识别产品API文档
- - - - -

***版权所有 翻版必究***

- - - - -

* [视频文件上传请求](#uploadV4)
    + [接口描述](#uploadV4.interafceDesc)
    + [请求URL](#uploadV4.requestUrl)
    + [请求方法](#uploadV4.requestMethod)
    + [字符编码](#uploadV4.requestEncode)
    + [建议超时时间](#uploadV4.requestTimeout)
    + [视频格式限制](#uploadV4.requestVideoFormat)
    + [视频大小限制](#uploadV4.requestVideoLength)
    + [视频时长限制](#uploadV4.requestDurationLength)
    + [请求参数](#uploadV4.requestParameters)
    * [返回参数](#uploadV4.responseParameters)
* [异步回调结果](#callbackV4)
    + [接口描述](#callbackV4.interafceDesc)
    + [请求方法](#callbackV4.callbackMethod)
    + [字符编码](#callbackV4.callbackEncode)
    + [建议超时时间](#callbackV4.callbackTimeout)
    + [支持协议](#callbackV4.callbackProtocol)
    + [回调策略](#callbackV4.callbackStrategy)
    + [回调参数](#callbackV4.callbackParameters)
* [查询视频结果请求](#queryV4)
    + [接口描述](#queryV4.interafceDesc)
    + [请求URL](#queryV4.requestUrl)
    + [请求方法](#queryV4.requestMethod)
    + [支持协议](#queryV4.requestProtocol)
    + [字符编码](#queryV4.requestEncode)
    + [建议超时时间](#queryV4.requestTimeout)
    + [请求参数](#queryV4.requestParameters)
    + [返回参数](#queryV4.responseParameters)
* [示例](#Demo)
    + [上传接口请求示例](#demo.requestUploadV4)
    + [上传接口返回示例](#demo.responseUploadV4)
    + [异步回调结果示例](#demo.callbackV4)

## <span id = "uploadV4">视频文件上传请求</span>

### <span id = "uploadV4.interafceDesc">接口描述</span>

该接口用于提交视频相关信息，自定义截帧频率等参数。识别结果需客户自行定期调用查询接口获取。

### <span id = "uploadV4.requestUrl">请求URL：</span>
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-video-bj.fengkongcloud.com/video/v4` | 中文视频文件 |
| 上海 | `http://api-video-sh.fengkongcloud.com/video/v4` | 中文视频文件 |
| 新加坡 | `http://api-video-xjp.fengkongcloud.com/video/v4` | 中文视频文件 |
| 硅谷 | `http://api-video-gg.fengkongcloud.com/video/v4` | 中文视频文件 |
| 印度 | `http://api-video-yd.fengkongcloud.com/video/v4` | 中文视频文件 |

### <span id = "uploadV4.requestMethod">请求方法：</span>

`POST` 

### <span id = "uploadV4.requestEncode">字符编码：</span>

`UTF-8`

### <span id = "uploadV4.requestTimeout">建议超时时间：</span>

7s

### <span id = "uploadV4.requestVideoFormat">视频格式限制：</span>

`AVI`、`FLV`、`MP4`、`MPG`、`WMV`、`MOV`、`WMA`、`RMVB`

### <span id = "uploadV4.requestVideoLength">视频大小限制：</span>

小于等于300MB

### <span id = "uploadV4.requestDurationLength">视频时长限制：</span>

小于等于2小时

### <span id = "uploadV4.requestParameters">请求参数：</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 公司密钥 | 必传参数 | 由数美提供，数美分配 |
| appId | string | 应用标识 | 必传参数 | 用于区分应用默认应用值：default |
| eventId | string | 事件标识 | 必传参数 | 用于区分场景数据，可选值：<br/>`video`:智能视频识别<br/>`default`:默认事件<br/>`live`:交友秀场<br/>`ecommerce`:电商直播<br/>`education`:教育场景<br/>`presision`:默认高准确<br/>`recall`:默认高召回 |
| imgType | string | 视频中的画面需要识别的监管类型，**和imgBusinessType至少传一个** | 非必传参数 | 监管一级标签<br/>可选值：<br/>`POLITICS`：涉政识别<br/>`VIOLENCE`：暴恐识别<br/>`BAN`：违禁识别<br/>`PORN`：色情识别<br/>`AD`：广告识别<br/>`SPAM`：机器灌水识别<br/>`OCR`：图片中的文字风险识别<br/>如果需要识别多个功能，通过下划线连接，如`AD_PORN_POLITICS`用于广告、色情和涉政组合识别 |
| audioType | string | 视频中的音频需要识别的监管类型 | 非必传参数 | 监管一级标签<br/>可选值：<br/>`POLITICS`：涉政识别<br/>`PORN`：色情识别<br/>`AD`：广告识别<br/>`MOAN`：娇喘识别<br/>`ABUSE`：辱骂识别<br/>`ANTHEN`：国歌识别<br/>`AUDIOPOLITICAL`：声音涉政<br/>`NONE`:不检测音频<br/>如需做组合识别，通过下划线连接即可，例如`POLITICAL_PORN_MOAN`用于广告、色情和涉政识别 |
| imgBusinessType | string | 视频中的画面需要识别的业务类型， **和imgType至少传一个** | 非必传参数 | 业务一级标签<br/>可选值：<br/>`SCREEN`：特殊画面识别<br/>`SCENCE`：场景画面识别<br/>`QR`：二维码识别<br/>`FACE`：人脸识别<br/>`QUALITY`：图像质量识别<br/>`MINOR`：未成年人识别<br/>`LOGO`：商企LOGO识别<br/>`BEAUTY`：颜值识别<br/>`FACECOMPARE`：人脸比对<br/>`OBJECT`：物品识别<br/>`STAR`：公众人物识别<br/>如需做组合识别，通过下划线连接即可，例如`QR_FACE_MINOR`用于二维码、人脸和未成年人识别 |
| audioBusinessType | String | 视频中的音频业务识别类型 | 非必传参数 | 业务一级标签<br/>可选值：<br/>`SING`：唱歌识别<br/>`LANGUAGE`：语种识别<br/>`MINOR`：未成年人识别<br/>`GENDER`：性别识别<br/>`TIMBRE`：音色识别，需要同时传入`GENDER`才能生效 |
| callback | string | 指定回调url地址 | 非必传参数 | 当该字段非空时，服务将根据该字段回调通知用户审核结果（支持`http`/`https`） |
| data | json\_object | 本次请求相关信息，最长1MB | 必传参数 | 最长1MB，其中[data内容如下](#uploadV4.requestParams.data) |

<span id = "uploadV4.requestParams.data">其中，data的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| btId | string | 视频唯一标识 | 必传参数 | 视频唯一标识，用于查询识别结果，最长64位 |
| url | string | 要检测的视频url地址 | 必传参数 | |
| tokenId | string | | 必传参数 | 客户端用户账号唯一标识，用于用户行为分析，建议传入用户UID； 最长40位 |
| detectFrequency | float | 截帧频率间隔，单位为秒 | 非必传参数 | 取值范围为0.5~60s；如不传递默认5s截帧一次 |
| ip | string | 客户端IP | 非必传参数 | 用于IP维度的用户行为分析，同时可用于比对数美IP黑库 |
| returnAllImg | int | | 非必传参数 | 选择返回视频截帧图片的等级：0：返回风险等级为非pass的图片1：返回所有风险等级的图片默认为0 |
| returnAllAudio | int | | 非必传参数 | 选择返回视频音频片段的等级：0：返回风险等级为非pass的音频片段1：返回所有风险等级的音频片段默认为0 |
| videoTitle | string | 视频名称 | 非必传参数 | 视频名称，用于后台界面展示 |
| extra | json_object | 扩展信息 | 非必传参数 | 详见[extra说明](#uploadV4.requestParams.data.extra) |

<span id="uploadV4.requestParams.data.extra">data 中，extra的内容如下</span>

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| passThrough | json\_object | 透传字段 | 非必传参数 | 该字段内容会随着回调结果一起原样返回 |

### <span id = "uploadV4.responseParameters">返回参数</span>：

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 本次请求的唯一标识 | 是 | 请求唯一标识 |
| code | int | 请求返回码 | 是 | 详见[接口响应码列表](#codeList) |
| message | string | 请求返回描述 | 是 | 详情描述如上 |
| btId | string | 唯一标识客户上传的视频 | 是 | 仅当code=1100时返回，与请求参数中的btId字段对应 |

## <span id = "callbackV4">异步回调结果</span>

### <span id = "callbackV4.interafceDesc">接口描述</span>

用户如果需要服务端主动对视频检测结果进行回调，则需要在请求参数中指定回调协议接口URL callback参数，服务端根据该参数在视频审核完成后，主动回调用户。


### <span id = "callbackV4.callbackMethod">请求方法：</span>

`POST` 

### <span id = "callbackV4.callbackEncode">字符编码：</span>

`UTF-8`

### <span id = "callbackV4.callbackTimeout">建议超时时间：</span>

5s

### <span id = "callbackV4.callbackProtocol">支持协议：</span>

`HTTP`或`HTTPS`

### <span id="callbackV4.callbackStrategy">回调策略：</span>

当用户收到推送结果，并返回HTTP状态码为200时，表示推送成功；否则系统将进行重试，最多20次推送。


### <span id = "callbackV4.callbackParameters">回调参数</span>：

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | 详见[接口响应码列表](#codeList) |
| message | string | 返回码详情描述 | 是 | |
| requestId | string | 请求唯一标识 | 是 | |
| btId | string | 视频唯一标识 | 是 | 最长64位 |
| riskLevel | string | 风险级别，code为1100时存在 | 否 | 返回值：<br/>`PASS`：正常内容，建议直接放行<br/>`REVIEW`：可疑内容，建议人工审核<br/>`REJECT`：违规内容，建议直接拦截 |
| frameDetail | json_array | 风险详情 | 否 | code为`1100`时存在，详见[frameDetail说明](#callbackV4.callbackParameters.frameDetail) |
| audioDetail | json_array | 音频片段信息 | 否 |code为`1100`时存在，详见[audioDetail说明](#callbackV4.callbackParameters.audioDetail)|
| auxInfo | json_object | 辅助信息 | 否 |code为`1100`时存在，详见[auxInfo说明](#callbackV4.callbackParameters.auxInfo)|
| tokenProfileLabels | json_array | 账号属性标签 | 否 |仅在开启功能时返回，详见[tokenProfileLabels说明](#callbackV4.callbackParameters.tokenProfileLabels)|
| tokenRiskLabels | json_array | 账号风险标签 | 否 |仅在开启功能时返回，详见[tokenRiskLabels说明](#callbackV4.callbackParameters.tokenRiskLabels)|
<span id = "callbackV4.callbackParameters.auxInfo">其中，auxInfo中的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| frameCount | int | 辅助信息 | 是 | 视频文件的截帧总数 |
| time | int | 辅助信息 | 是 | 视频时长 |
| passThrough | json_object | 透传字段 | 否 | 该字段内容与请求参数data中extra的passThrough的值相同 |

<span id = "callbackV4.callbackParameters.frameDetail">其中，frameDetail数组中每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| time | float | 截帧在视频文件中的时间，单位为秒 | 是 | 截帧图片相对视频文件的时间 |
| requestId | string | 当前截帧片段的唯一标识 | 是 | |
| imgUrl | string | 当前截帧的URL | 是 | |
| imgText | string | 截帧图片OCR文本内容 | 否 | 截帧图片OCR文字识别，识别类型包含OCR时会有 |
| riskLevel | string | 当前截帧的处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| riskLabel1 | string | 各个一级标签之间是并列的关系，当riskLevel为`PASS`时返回`normal` | 是 | 一级标签 |
| riskLabel2 | string | 二级标签归属于一级标签，当riskLevel为`PASS`时为空 | 是 | 二级标签 |
| riskLabel3 | string | 三级标签归属于二级标签，当riskLevel为`PASS`时为空 | 是 | 三级标签 |
| riskDescription | string | 标签解释 | 是 | 对于命中用户自定义名单时返回：`命中自定义名单`；当riskLevel为`PASS`时返回:`正常`；其他情况展现形式为一级标签：二级标签：三级标签的中文名 |
| riskDetail | json_object | 风险详情信息 | 是 | 详见[riskDetail说明](#callbackV4.callbackParameters.frameDetail.riskDetail) |
| allLabels | json_array | 全部的风险标签列表 | 是 | 全部的风险标签列表，详见[allLabels说明](#callbackV4.callbackParameters.frameDetail.allLabels) |
| businessLabels | json_array | 业务标签列表 | 否 | 传入imgBusinessType时返回，详见[businessLabels说明](#callbackV4.callbackParameters.frameDetail.businessLabels) |
| auxInfo | json_object | 辅助信息 | 是 | 一些辅助信息放在这里，详见[auxInfo说明](#callbackV4.callbackParameters.frameDetail.auxInfo) |

<span id = "callbackV4.callbackParameters.frameDetail.auxInfo">frameDetail中，auxInfo的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| qrContent | string | 截帧图片二维码链接识别 | 否 | 截帧图片二维码链接识别，如有需要可联系数美开启<br/>注意：开启该功能后，只有完整，可以正常识别到的二维码才会返回且imgType传值需要包含`AD` |
| similarity | float | 当前截帧图片和上一帧截帧图片的相似度 | 是 | 有图片则该字段就会返回，视频文件初始第一帧将比对纯黑背景图片 |

<span id = "callbackV4.callbackParameters.frameDetail.riskDetail">frameDetail中，riskDetail的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskSource | int | 风险来源 | 是 | 风险来源，可选值：<br/>`1000`：无风险<br/>`10001`：文本风险<br/>`1002`：视觉风险<br/>`1003`：音频风险 |
| faces | json_array | 人物信息 | 否 | 返回图片中涉政人物的名称及位置信息，详见[faces说明](#callbackV4.callbackParameters.frameDetail.riskDetail.faces) |
| objects | json_array | 标识信息 | 否 | 返回图片中标识或物品的名称及位置信息，详见[objects说明](#callbackV4.callbackParameters.frameDetail.riskDetail.objects) |
| ocrText | json_object | 文字信息 | 否 | 返回图片中文字相关信息，详见[ocrText说明](#callbackV4.callbackParameters.frameDetail.riskDetail.ocrText) |

<span id = "callbackV4.callbackParameters.frameDetail.riskDetail.faces">riskDetail中，faces数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 人物名称 | 否 | |
| location | int_array | 人物位置信息，四个值代表的是左上角的坐标和右下角的坐标。例如[207,522,340,567]，207代表的是左上角的x坐标，522代表左上角的y坐标，340代表的是右下角的x坐标，567代表的是右下角的y坐标 | 否 | 位置信息 |
| probability | float | 置信度，值越大，可信度越高 | 否 | 可选值在0～1之间 |

<span id = "callbackV4.callbackParameters.frameDetail.riskDetail.objects">riskDetail中，objects数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 标识或物品名称 | 否 ||
| location | int_array | 标识或物品位置，四个值代表的是左上角的坐标和右下角的坐标。例如[207,522,340,567]，207代表的是左上角的x坐标，522代表左上角的y坐标，340代表的是右下角的x坐标，567代表的是右下角的y坐标 | 否 |位置信息|
| probability | float | 置信度，值越大，可信度越高 | 否 |可选值在0～1之间|
<span id = "callbackV4.callbackParameters.frameDetail.riskDetail.ocrText">riskDetail中，ocrText数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| text | string | 图片中识别出的文字 | 否 | 识别出来所有文字内容 |
| matchedLists | json_array | 命中的客户自定义名单信息 | 否 | 仅在命中客户自定义名单时返回，详见[matchedLists说明](#callbackV4.callbackParameters.frameDetail.riskDetail.ocrText.matchedLists) |
| riskSegments | json_array | 高风险内容片段 | 否 | 在启用涉政、暴恐、违禁、广告等功能时存在，详见[riskSegments说明](#callbackV4.callbackParameters.frameDetail.riskDetail.ocrText.riskSegments) |

<span id = "callbackV4.callbackParameters.frameDetail.riskDetail.ocrText.matchedLists">ocrText中，matchedLists数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 客户自定义名单名称 | 否 | |
| words | json_array | 命中的这个名单中的敏感词信息 | 否 | 下标从0开始计数，详见[words说明](#callbackV4.callbackParameters.frameDetail.riskDetail.ocrText.matchedLists.words) |

<span id = "callbackV4.callbackParameters.frameDetail.riskDetail.ocrText.matchedLists.words">matchedLists中，words数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string | 敏感词 | 否 | |
| position | int_array | 敏感词所在位置 | 否 | 下标从0开始计数 |

<span id = "callbackV4.callbackParameters.frameDetail.riskDetail.ocrText.riskSegments">ocrText中，riskSegments每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segment | string | 高风险内容片段 | 否 | |
| position | int_array | 高风险内容片段所在位置 | 否 | 下标从0开始计数 |

<span id = "callbackV4.callbackParameters.frameDetail.allLabels">frameDetail中，allLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险描述 | 是 | 格式为&quot;一级风险标签：二级风险标签：三级风险标签&quot;的中文名称<br/>对于命中用户自定义名单时返回：`命中自定义名单` |
| riskLevel | string | 处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |

<span id = "callbackV4.callbackParameters.frameDetail.businessLabels">frameDetail中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |

<span id = "callbackV4.callbackParameters.audioDetail">其中，audioDetail数组中每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| audioStarttime | float | 音频片段发生时间 | 是 | |
| audioEndtime | float | 音频片段结束时间 | 是 | |
| audioUrl | string | 音频片段地址 | 是 | |
| audioText | string | 音转文文字 | 否 | 识别出文本会返回 |
| riskLevel | string | 当前事件的处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| riskLabel1 | string | 各个一级标签之间是并列的关系，riskLevel为`PASS`时返回`normal` | 是 | 一级标签 |
| riskLabel2 | string | 二级标签归属于一级标签，当riskLevel为`PASS`时为空 | 是 | 二级标签 |
| riskLabel3 | string | 三级标签归属于二级标签，当riskLevel为`PASS`时为空 | 是 | 三级标签 |
| riskDescription | string | 标签解释 | 是 | 格式为&quot;一级风险标签：二级风险标签：三级风险标签&quot;的中文名称<br/>对于命中用户自定义名单时返回：`命中自定义名单` |
| riskDetail | json_object | 风险详情信息 | 是 | 详见[riskDetail说明](#callbackV4.callbackParameters.audioDetail.riskDetail) |
| allLabels | json_array | 全部的风险标签列表 | 是 | 全部的风险标签列表，详见[allLabels说明](#callbackV4.callbackParameters.audioDetail.allLabels) |
| businessLabels | json_array | 业务标签列表 | 否 | 传入audioBusinessType时会返回，详见[businessLabels说明](#callbackV4.callbackParameters.audioDetail.businessLabels) |

<span id="callbackV4.callbackParameters.audioDetail.riskDetail">audioDetail中，riskDetail的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskSource | int | 风险来源 | 是 | 风险来源，可选值：<br/>`1000`：无风险<br/>`10001`：文本风险<br/>`1002`：视觉风险<br/>`1003`：音频风险 |
| audioText | string | 音频转译文本的结果 | 否 | |
| matchedLists | json_array | 命中的客户自定义名单信息 | 否 | 命中客户自定义名单时返回，其他时不存在，详见[matchedLists说明](#callbackV4.callbackParameters.audioDetail.riskDetail.matchedLists) |
| riskSegments | json_array | 高风险内容片段 | 否 | 在涉政、暴恐、违禁、竞品、广告法等功能的时候存在，详见[riskSegments说明](#callbackV4.callbackParameters.audioDetail.riskDetail.riskSegments) |

<span id="callbackV4.callbackParameters.audioDetail.riskDetail.matchedLists">riskDetail中，matchedLists的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 客户自定义名单名 | 否 | |
| words | json_array | 命中的这个名单中的敏感词信息 | 否 | 下标从0开始计数，详见[words说明](#callbackV4.callbackParameters.audioDetail.riskDetail.matchedLists.words) |

<span id="callbackV4.callbackParameters.audioDetail.riskDetail.matchedLists.words">matchedLists中，words的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string | 敏感词 | 否 | |
| position | int_array | 敏感词所在位置 | 否 | 下标从0开始计数 |

<span id="callbackV4.callbackParameters.audioDetail.riskDetail.riskSegments">riskDetail中，riskSegments的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segment | string | 高风险内容片段 | 否 | |
| position | int_array | 高风险内容片段所在位置 | 否 | 下标从0开始计数 |

<span id="callbackV4.callbackParameters.audioDetail.allLabels">audioDetail中，allLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险描述 | 是 | 格式为&quot;一级风险标签：二级风险标签：三级风险标签&quot;的中文名称<br/>对于命中用户自定义名单时返回：`命中自定义名单` |
| riskLevel | string | 处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |

<span id="callbackV4.callbackParameters.audioDetail.businessLabels">audioDetail中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |

<span id="callbackV4.callbackParameters.tokenProfileLabels">其中，tokenProfileLabels数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| label1 | string | 一级标签 | 否 | |
| label2 | string | 二级标签 | 否 | |
| label3 | string | 三级标签 | 否 | |
| description | string | 标签描述 | 否 | |
| timestamp | int | 打标签时间戳 | 否 | 13位Unix时间戳，单位：毫秒 |

<span id="callbackV4.callbackParameters.tokenRiskLabels">其中，tokenRiskLabels数组每个成员的具体字段同tokenProfileLabels</span>


## <span id = "queryV4">查询视频结果</span>

该接口用于客户主动查询视频文件识别结果，建议每30s进行一次查询。


### <span id = "queryV4.interafceDesc">接口描述</span>

该接口用于客户主动查询视频文件识别结果，建议每30s进行一次查询。

### <span id = "queryV4.requestUrl">请求URL：</span>
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-video-bj.fengkongcloud.com/video/query/v4` | 中文视频文件 |
| 上海 | `http://api-video-sh.fengkongcloud.com/video/query/v4` | 中文视频文件 |
| 新加坡 | `http://api-video-xjp.fengkongcloud.com/video/query/v4` | 中文视频文件 |
| 硅谷 | `http://api-video-gg.fengkongcloud.com/video/query/v4` | 中文视频文件 |
| 印度 | `http://api-video-yd.fengkongcloud.com/video/query/v4` | 中文视频文件 |

### <span id = "queryV4.requestMethod">请求方法：</span>

`POST` 

### <span id = "queryV4.requestProtocol">支持协议：</span>

`HTTP`或`HTTPS`

### <span id = "queryV4.requestEncode">字符编码：</span>

`UTF-8`

### <span id = "queryV4.requestTimeout">建议超时时间：</span>

1s

### <span id = "queryV4.requestParameters">请求参数：</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 用于权限认证，开通账号服务时由数美提供 | 必传参数 | |
| btId | string | 视频唯一标识，用于查询识别结果，最长64位 | 必传参数 | |

### <span id = "queryV4.responseParameters"> 返回参数</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | 详见[接口响应码列表](#codeList) |
| message | string | 返回码详情描述 | 是 | |
| requestId | string | 请求唯一标识 | 是 | |
| btId | string | 视频唯一标识 | 是 | 最长64位 |
| riskLevel | string | 风险级别，code为1100时存在 | 否 | 可能返回值：<br/>`PASS`：正常内容，建议直接放行<br/>`REVIEW`：可疑内容，建议人工审核<br/>`REJECT`：违规内容，建议直接拦截 |
| frameDetail | json_array | 风险详情 | 否 | code为`1100`时存在，详见[frameDetail](#queryV4.responseParameters.frameDetail) |
| audioDetail | json_array | 音频片段信息 | 否 | code为`1100`时存在，详见[audioDetail](#queryV4.responseParameters.audioDetail) |
| auxInfo | json_object | 辅助信息 | 是 | code为`1100`时存在，扩展辅助信息，详见[auxInfo说明](#queryV4.responseParameters.auxInfo) |
| tokenProfileLabels | json_array | 账号属性标签 | 否 | 仅在开启功能时返回，详见[tokenProfileLabels说明](#queryV4.responseParameters.tokenProfileLabels) |
| tokenRiskLabels | json_array | 账号风险标签 | 否 | 仅在开启功能时返回，详见[tokenRiskLabels说明](#queryV4.responseParameters.tokenRiskLabels) |

<span id="queryV4.responseParameters.auxInfo">其中，auxInfo数组中每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| frameCount | int | 辅助信息 | 是 | 视频文件的截帧总数 |
| time | int | 辅助信息 | 是 | 视频时长 |

<span id = "queryV4.responseParameters.frameDetail">其中，frameDetail数组中每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| time | float | 截帧在视频文件中的时间，单位为秒 | 是 | 截帧图片相对视频文件的时间 |
| requestId | string | 当前截帧片段的唯一标识 | 是 | |
| imgUrl | string | 当前截帧的URL | 是 | |
| imgText | string | 截帧图片OCR文本内容 | 否 | 截帧图片OCR文字识别，识别类型包含OCR时会有 |
| riskLevel | string | 当前截帧的处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| riskLabel1 | string | 各个一级标签之间是并列的关系，当riskLevel为`PASS`时返回`normal` | 是 | 一级标签 |
| riskLabel2 | string | 二级标签归属于一级标签，当riskLevel为`PASS`时为空 | 是 | 二级标签 |
| riskLabel3 | string | 三级标签归属于二级标签，当riskLevel为`PASS`时为空 | 是 | 三级标签 |
| riskDescription | string | 标签解释 | 是 | 对于命中用户自定义名单时返回：`命中自定义名单`；当riskLevel为`PASS`时返回:`正常`；其他情况展现形式为一级标签：二级标签：三级标签的中文名 |
| riskDetail | json_object | 风险详情信息 | 是 | 风险详情，详见[riskDetail说明](#queryV4.responseParameters.frameDetail.riskDetail) |
| allLabels | json_array | 全部的风险标签列表 | 是 | 全部的风险标签列表，详见[allLabels说明](#queryV4.responseParameters.frameDetail.allLabels) |
| businessLabels | json_array | 业务标签列表 | 否 | 传入imgBusinessType时返回，详见[businessLabels说明](#queryV4.responseParameters.frameDetail.businessLabels) |
| auxInfo | json_object | 辅助信息 | 是 | 一些辅助信息放在这里，详见[auxInfo说明](#queryV4.responseParameters.frameDetail.auxInfo) |

<span id="queryV4.responseParameters.frameDetail.auxInfo">frameDetail中，auxInfo的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| qrContent | string | 截帧图片二维码链接识别 | 否 | 截帧图片二维码链接识别，如有需要可联系数美开启<br/>注意：开启该功能后，只有完整，可以正常识别到的二维码才会返回且imgType传值需要包含`AD` |
| similarity | float | 当前截帧图片和上一帧截帧图片的相似度 | 是 | 有图片则该字段就会返回，视频文件初始第一帧将比对纯黑背景图片 |

<span id="queryV4.responseParameters.frameDetail.riskDetail">frameDetail中，riskDetail的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskSource | int | 风险来源 | 是 | 风险来源，可选值：<br/>`1000`：无风险<br/>`10001`：文本风险<br/>`1002`：视觉风险<br/>`1003`：音频风险 |
| faces | json_array | 人物信息 | 否 | 返回图片中涉政人物的名称及位置信息 |
| objects | json_array | 标识信息 | 否 | 返回图片中标识或物品的名称及位置信息 |
| ocrText | json_object | 文字信息 | 否 | 返回图片中文字相关信息 |

riskDetail中，faces数组每个成员的具体内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 人物名称 | 否 | |
| location | int_array | 人物位置信息，四个值代表的是左上角的坐标和右下角的坐标。例如[207,522,340,567]，207代表的是左上角的x坐标，522代表左上角的y坐标，340代表的是右下角的x坐标，567代表的是右下角的y坐标 | 否 | 位置信息 |
| probability | float | 置信度，值越大，可信度越高 | 否 | 可选值在0～1之间 |

riskDetail中，objects数组每个成员的具体内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 标识或物品名称 | 否 | |
| location | int_array | 标识或物品位置，四个值代表的是左上角的坐标和右下角的坐标。例如[207,522,340,567]，207代表的是左上角的x坐标，522代表左上角的y坐标，340代表的是右下角的x坐标，567代表的是右下角的y坐标 | 否 | 位置信息 |
| probability | float | 置信度，值越大，可信度越高 | 否 | 可选值在0～1之间 |

riskDetail中，ocrText数组每个成员的具体内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| text | string | 图片中识别出的文字 | 否 | 识别出来所有文字内容 |
| matchedLists | json_array | 命中的客户自定义名单信息 | 否 | 仅在命中客户自定义名单时返回 |
| riskSegments | json_array | 高风险内容片段 | 否 | 在启用涉政、暴恐、违禁、广告等功能时存在 |

ocrText中，matchedLists内每个元素详细内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 客户自定义名单名称 | 否 | |
| words | json_array | 命中的这个名单中的敏感词信息 | 否 | 下标从0开始计数 |

matchedLists中，words的每个元素详细内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string | 敏感词 | 否 | |
| position | int_array | 敏感词所在位置 | 否 | 下标从0开始计数 |

ocrText中，riskSegments的每个元素详细内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segment | string | 高风险内容片段 | 否 | |
| position | int_array | 高风险内容片段所在位置 | 否 | 下标从0开始计数 |

<span id="queryV4.responseParameters.frameDetail.allLabels">frameDetail，allLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险描述 | 是 | 格式为&quot;一级风险标签：二级风险标签：三级风险标签&quot;的中文名称<br/>对于命中用户自定义名单时返回：`命中自定义名单` |
| riskLevel | string | 处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |

<span id="queryV4.responseParameters.frameDetail.businessLabels">frameDetail中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |

<span id = "queryV4.responseParameters.audioDetail">其中，audioDetail数组中每个成员的具体内容如下：</span>


| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| audioStarttime | float | 音频片段发生时间 | 是 | |
| audioEndtime | float | 音频片段结束时间 | 是 | |
| audioUrl | string | 音频片段地址 | 是 | |
| audioText | string | 音转文文字 | 否 | 识别出文本会返回 |
| riskLevel | string | 当前事件的处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| riskLabel1 | string | 各个一级标签之间是并列的关系，riskLevel为`PASS`时返回`normal` | 是 | 一级标签 |
| riskLabel2 | string | 二级标签归属于一级标签，当riskLevel为`PASS`时为空 | 是 | 二级标签 |
| riskLabel3 | string | 三级标签归属于二级标签，当riskLevel为`PASS`时为空 | 是 | 三级标签 |
| riskDescription | string | 标签解释 | 是 | 格式为&quot;一级风险标签：二级风险标签：三级风险标签&quot;的中文名称<br/>对于命中用户自定义名单时返回：`命中自定义名单` |
| riskDetail | json_object | 风险详情信息 | 是 | 风险详情，详见[riskDetail说明](#queryV4.responseParameters.audioDetail.riskDetail) |
| allLabels | json_array | 全部的风险标签列表 | 是 | 全部的风险标签列表，详见[allLabels说明](#queryV4.responseParameters.audioDetail.allLabels) |
| businessLabels | json_array | 业务标签列表 | 否 | 传入audioBusinessType时会有，详见[businessLabels说明](#queryV4.responseParameters.audioDetail.businessLabels) |

<span id="queryV4.responseParameters.audioDetail.riskDetail">audioDetail中，riskDetail的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskSource | int | 风险来源 | 是 | 风险来源，可选值：<br/>`1000`：无风险<br/>`10001`：文本风险<br/>`1002`：视觉风险<br/>`1003`：音频风险 |
| audioText | string | 音频转译文本的结果 | 否 | |
| matchedLists | json_array | 命中的客户自定义名单信息 | 否 | 命中客户自定义名单时返回，其他时不存在，详见[matchedLists说明](#queryV4.responseParameters.audioDetail.riskDetail.matchedLists) |
| riskSegments | json_array | 高风险内容片段 | 否 | 在涉政、暴恐、违禁、竞品、广告法等功能的时候存在，详见[riskSegments说明](#queryV4.responseParameters.audioDetail.riskDetail.riskSegments) |

<span id="queryV4.responseParameters.audioDetail.riskDetail.matchedLists">riskDetail中，matchedLists数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 客户自定义名单名 | 否 | |
| words | json_array | 命中的这个名单中的敏感词信息 | 否 | 下标从0开始计数，详见[words说明](#queryV4.responseParameters.audioDetail.riskDetail.matchedLists.words) |

<span id="queryV4.responseParameters.audioDetail.riskDetail.matchedLists.words">matchedLists中，words的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string | 敏感词 | 否 | |
| position | int_array | 敏感词所在位置 | 否 | 下标从0开始计数 |

<span id="queryV4.responseParameters.audioDetail.riskDetail.riskSegments">riskDetail中，riskSegments数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segment | string | 高风险内容片段 | 否 | |
| position | int_array | 高风险内容片段所在位置 | 否 | 下标从0开始计数 |

<span id="queryV4.responseParameters.audioDetail.allLabels">audioDetail中，allLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险描述 | 是 | 格式为&quot;一级风险标签：二级风险标签：三级风险标签&quot;的中文名称<br/>对于命中用户自定义名单时返回：`命中自定义名单` |
| riskLevel | string | 处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |

<span id="queryV4.responseParameters.audioDetail.businessLabels">audioDetail中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |

<span id="queryV4.responseParameters.tokenProfileLabels">其中，tokenProfileLabels数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| label1 | string | 一级标签 | 否 | |
| label2 | string | 二级标签 | 否 | |
| label3 | string | 三级标签 | 否 | |
| description | string | 标签描述 | 否 | |
| timestamp | int | 打标签时间戳 | 否 | 13位Unix时间戳，单位：毫秒 |

<span id="queryV4.responseParameters.tokenRiskLabels">其中，tokenRiskLabels数组每个成员的具体字段同tokenProfileLabels</span>

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

### <span id = "demo.requestUploadV4">上传接口请求示例：</span>

```json
{
    "accessKey": "**********",
    "appId": "default",
    "btId": "1639824316368",
    "eventId": "video",
    "imgType": "POLITICS_VIOLENCE_BAN_PORN_MINOR_AD_SPAM_LOGO_STAR_OCR",
    "imgBusinessType": "SCREEN_SCENCE_QR_FACE_QUALITY_MINOR_LOGO_BEAUTY_FACECOMPARE",
    "audioType": "POLITICAL_PORN_AD_MOAN_ABUSE",
    "audioBusinessType": "SING_LANGUAGE_MINOR_GENDER_TIMBRE",
    "callback": "http://www.xxx.top/xxx",
    "data": {
        "btId": "1639824316368",
        "channel": "video",
        "detectFrequency": 3,
        "tokenId": "test",
        "ip":"123.171.34.3",
        "url": "http://oss.xxx.com/static/photo/117608703147396.mp4",
        "returnAllAudio": 1,
        "returnAllImg": 1,
        "extra": {
            "passThrough": {
                "passThrough1": "透传字段1",
                "passThrough2": "透传字段2",
                "passThrough3": "透传字段3"
            }
        }
    }
}
```


### <span id = "demo.responseUploadV4">上传接口返回示例：</span>

```json
{
    "code": 1100,
    "message": "成功",
    "requestId": "1639824316368",
    "btId": "1639824316368"
}
```

### <span id = "demo.callbackV4">异步回调结果示例：</span>

```json
{
    "code": 1100,
    "message": "成功",
    "requestId": "1639731708004",
    "btId": "btid_test",
    "frameDetail": [
        {
            "allLabels": [
                {
                    "riskDescription": "商企LOGO:LOGO:抖音",
                    "riskLabel1": "logo",
                    "riskLabel2": "logo",
                    "riskLabel3": "douyin",
                    "riskLevel": "REJECT"
                },
                {
                    "riskDescription": "涉政:负面事件:恶性刑事案件",
                    "riskLabel1": "politics",
                    "riskLabel2": "fumianshijian",
                    "riskLabel3": "exingxingshianjian",
                    "riskLevel": "REJECT"
                },
                {
                    "riskDescription": "涉政:政治象征:军装",
                    "riskLabel1": "politics",
                    "riskLabel2": "zhengzhixiangzheng",
                    "riskLabel3": "junzhuang",
                    "riskLevel": "REJECT"
                }
            ],
            "auxInfo": {
                "similarity": 0.61328125
            },
            "businessLabels": [
                {
                    "businessDescription": "商企LOGO:LOGO:抖音",
                    "businessLabel1": "logo",
                    "businessLabel2": "logo",
                    "businessLabel3": "douyin"
                },
                {
                    "businessDescription": "商企LOGO:LOGO:火山",
                    "businessLabel1": "logo",
                    "businessLabel2": "logo",
                    "businessLabel3": "huoshan"
                }
            ],
            "imgText": "抖音抖音号：xxx劳荣枝拒绝认罪当庭哭诉称自己是男友的性侵和赚钱工具爱害者家属：“希望她被干刀万别今天上午，荣技案开庭。法庭审理中，检察机关指控劳荣枝在南昌，温州、常州合肥四起犯罪事实，涉谦犯故意杀人、绑架、抢劫罪。",
            "imgUrl": "http://xxx.cos.ap-beijing.xxx.com/image202112171639004_v0.jpg",
            "requestId": "1639731708004_v0",
            "riskDescription": "涉政:负面事件:恶性刑事案件",
            "riskDetail": {
                "riskSource": 1001
            },
            "riskLabel1": "politics",
            "riskLabel2": "fumianshijian",
            "riskLabel3": "exingxingshianjian",
            "riskLevel": "REJECT",
            "time": 0
        },
        {
            "allLabels": [
                {
                    "riskDescription": "涉政:涉政:涉政",
                    "riskLabel1": "politics",
                    "riskLabel2": "shezheng",
                    "riskLabel3": "shezheng",
                    "riskLevel": "REJECT"
                },
                {
                    "riskDescription": "涉政:政治象征:军装",
                    "riskLabel1": "politics",
                    "riskLabel2": "zhengzhixiangzheng",
                    "riskLabel3": "junzhuang",
                    "riskLevel": "REJECT"
                },
                {
                    "riskDescription": "商企LOGO:LOGO:抖音",
                    "riskLabel1": "logo",
                    "riskLabel2": "logo",
                    "riskLabel3": "douyin",
                    "riskLevel": "REJECT"
                }
            ],
            "auxInfo": {
                "similarity": 0.87109375
            },
            "businessLabels": [
                {
                    "businessDescription": "商企LOGO:LOGO:抖音",
                    "businessLabel1": "logo",
                    "businessLabel2": "logo",
                    "businessLabel3": "douyin"
                },
                {
                    "businessDescription": "商企LOGO:LOGO:火山",
                    "businessLabel1": "logo",
                    "businessLabel2": "logo",
                    "businessLabel3": "huoshan"
                }
            ],
            "imgText": "抖音直播 抖音号：xxx劳荣枝拒绝认罪当庭哭诉称自己是男友的性侵和赚钱工具爱害者家属：“希望她被干刀万别12月21日上午南昌市中级人民法院今天上午，劳荣枝案开庭。法庭审理中，检察机关指控劳荣枝在南昌、温州、常州合肥四起犯罪事实，涉谦犯故意杀人、绑架、抢劫罪。",
            "imgUrl": "http://bj-video-xxx.cos.ap-beijing.xxx.com/image20211171639731708004_v3.jpg",
            "requestId": "1639731708004_v3",
            "riskDescription": "涉政:涉政:涉政",
            "riskDetail": {
                "ocrText": {
                    "matchedLists": [
                        {
                            "name": "测试01",
                            "words": [
                                {
                                    "position": [
                                        17,
                                        18,
                                        19
                                    ],
                                    "word": "111"
                                }
                            ]
                        }
                    ],
                    "text": "抖音直播 抖音号：xxx劳荣枝拒绝认罪当庭哭诉称自己是男友的性侵和赚钱工具爱害者家属：“希望她被干刀万别12月21日上午南昌市中级人民法院今天上午，劳荣枝案开庭。法庭审理中，检察机关指控劳荣枝在南昌、温州、常州合肥四起犯罪事实，涉谦犯故意杀人、绑架、抢劫罪。"
                },
                "riskSource": 1001
            },
            "riskLabel1": "politics",
            "riskLabel2": "shezheng",
            "riskLabel3": "shezheng",
            "riskLevel": "REJECT",
            "time": 3
        },
        {
            "allLabels": [
                {
                    "riskDescription": "商企LOGO:LOGO:抖音",
                    "riskLabel1": "logo",
                    "riskLabel2": "logo",
                    "riskLabel3": "douyin",
                    "riskLevel": "REJECT"
                },
                {
                    "riskDescription": "色情:性骚扰:重度性骚扰",
                    "riskLabel1": "porn",
                    "riskLabel2": "xingsaorao",
                    "riskLabel3": "zhongduxingsaorao",
                    "riskLevel": "REJECT"
                }
            ],
            "auxInfo": {
                "similarity": 0.625
            },
            "businessLabels": [
                {
                    "businessDescription": "商企LOGO:LOGO:抖音",
                    "businessLabel1": "logo",
                    "businessLabel2": "logo",
                    "businessLabel3": "douyin"
                },
                {
                    "businessDescription": "商企LOGO:LOGO:火山",
                    "businessLabel1": "logo",
                    "businessLabel2": "logo",
                    "businessLabel3": "huoshan"
                }
            ],
            "imgText": "直播  机荣荣枝拒绝认罪当庭哭诉称自己是男友的性侵和赚钱工具爱害者家属：“希望她被干刀万别9荣技法庭上多次为自己辩解，称20年来过着暗无天日的生话。她称自己會为法子英多次堕胎，常被酸打、。抖音抖音号：xxx",
            "imgUrl": "http://bj-video-xxx.cos.ap-beijing.xxx.com/image202112171639731708004_v6.jpg",
            "requestId": "1639731708004_v6",
            "riskDescription": "色情:性骚扰:重度性骚扰",
            "riskDetail": {
                "ocrText": {
                    "riskSegments": [
                        {
                            "position": [
                                23,
                                24
                            ],
                            "segment": "性侵"
                        }
                    ],
                    "text": "直播  机荣荣枝拒绝认罪当庭哭诉称自己是男友的性侵和赚钱工具爱害者家属：“希望她被干刀万别9荣技法庭上多次为自己辩解，称20年来过着暗无天日的生话。她称自己會为法子英多次堕胎，常被酸打、。抖音抖音号：xxx"
                },
                "riskSource": 1001
            },
            "riskLabel1": "porn",
            "riskLabel2": "xingsaorao",
            "riskLabel3": "zhongduxingsaorao",
            "riskLevel": "REJECT",
            "time": 6
        },
        {
            "allLabels": [
                {
                    "riskDescription": "商企LOGO:LOGO:抖音",
                    "riskLabel1": "logo",
                    "riskLabel2": "logo",
                    "riskLabel3": "douyin",
                    "riskLevel": "REJECT"
                },
                {
                    "riskDescription": "涉政:负面事件:恶性刑事案件",
                    "riskLabel1": "politics",
                    "riskLabel2": "fumianshijian",
                    "riskLabel3": "exingxingshianjian",
                    "riskLevel": "REJECT"
                }
            ],
            "auxInfo": {
                "similarity": 0.71484375
            },
            "businessLabels": [
                {
                    "businessDescription": "商企LOGO:LOGO:抖音",
                    "businessLabel1": "logo",
                    "businessLabel2": "logo",
                    "businessLabel3": "douyin"
                },
                {
                    "businessDescription": "商企LOGO:LOGO:火山",
                    "businessLabel1": "logo",
                    "businessLabel2": "logo",
                    "businessLabel3": "huoshan"
                }
            ],
            "imgText": "直播  礼劳荣枝拒绝认罪当庭哭诉称自己是男友的性侵和赚钱工具爱害者家属：“希望她被干刀万别我们的愿望是干刀万别被害人熊义弟媳：熊正英抖音抖音号：xxx",
            "imgUrl": "http://bj-video-xxx.cos.ap-beijing.xxx.com/image%2F20211217%2F1639731708004_v9.jpg",
            "requestId": "1639731708004_v9",
            "riskDescription": "涉政:负面事件:恶性刑事案件",
            "riskDetail": {
                "ocrText": {
                    "riskSegments": [
                        {
                            "position": [
                                5,
                                6,
                                7
                            ],
                            "segment": "劳荣枝"
                        }
                    ],
                    "text": "直播  礼劳荣枝拒绝认罪当庭哭诉称自己是男友的性侵和赚钱工具爱害者家属：“希望她被干刀万别我们的愿望是干刀万别被害人熊义弟媳：熊正英抖音抖音号：xxx"
                },
                "riskSource": 1001
            },
            "riskLabel1": "politics",
            "riskLabel2": "fumianshijian",
            "riskLabel3": "exingxingshianjian",
            "riskLevel": "REJECT",
            "time": 9
        },
        {
            "allLabels": [
                {
                    "riskDescription": "商企LOGO:LOGO:抖音",
                    "riskLabel1": "logo",
                    "riskLabel2": "logo",
                    "riskLabel3": "douyin",
                    "riskLevel": "REJECT"
                },
                {
                    "riskDescription": "色情:性骚扰:重度性骚扰",
                    "riskLabel1": "porn",
                    "riskLabel2": "xingsaorao",
                    "riskLabel3": "zhongduxingsaorao",
                    "riskLevel": "REJECT"
                }
            ],
            "auxInfo": {
                "similarity": 0.79296875
            },
            "businessLabels": [
                {
                    "businessDescription": "商企LOGO:LOGO:抖音",
                    "businessLabel1": "logo",
                    "businessLabel2": "logo",
                    "businessLabel3": "douyin"
                },
                {
                    "businessDescription": "商企LOGO:LOGO:火山",
                    "businessLabel1": "logo",
                    "businessLabel2": "logo",
                    "businessLabel3": "huoshan"
                }
            ],
            "imgText": "直播  荣技拒绝认罪当庭哭诉称自己是男友的性侵和赚钱工具爱害者家属：“希望她被干刀万别”此前据法子英供述1996年月为抢劫钱财劳荣技诱骗商人熊启义至出租屋事先躲藏在屋内的法子英将其杀害再将其胶解太恶毒了被害人熊后义弟媳：熊正英抖音抖音号：xxx",
            "imgUrl": "http://bj-video-xxx.cos.ap-beijing.xxx.com/image%2F20211217%2F1639731708004_v12.jpg",
            "requestId": "1639731708004_v12",
            "riskDescription": "色情:性骚扰:重度性骚扰",
            "riskDetail": {
                "ocrText": {
                    "riskSegments": [
                        {
                            "position": [
                                21,
                                22
                            ],
                            "segment": "性侵"
                        }
                    ],
                    "text": "直播  荣技拒绝认罪当庭哭诉称自己是男友的性侵和赚钱工具爱害者家属：“希望她被干刀万别”此前据法子英供述1996年月为抢劫钱财劳荣技诱骗商人熊启义至出租屋事先躲藏在屋内的法子英将其杀害再将其胶解太恶毒了被害人熊后义弟媳：熊正英抖音抖音号：xxx"
                },
                "riskSource": 1001
            },
            "riskLabel1": "porn",
            "riskLabel2": "xingsaorao",
            "riskLabel3": "zhongduxingsaorao",
            "riskLevel": "REJECT",
            "time": 12
        },
        {
            "allLabels": [
                {
                    "riskDescription": "色情:性骚扰:重度性骚扰",
                    "riskLabel1": "porn",
                    "riskLabel2": "xingsaorao",
                    "riskLabel3": "zhongduxingsaorao",
                    "riskLevel": "REJECT"
                },
                {
                    "riskDescription": "商企LOGO:LOGO:抖音",
                    "riskLabel1": "logo",
                    "riskLabel2": "logo",
                    "riskLabel3": "douyin",
                    "riskLevel": "REJECT"
                }
            ],
            "auxInfo": {
                "similarity": 0.79296875
            },
            "businessLabels": [
                {
                    "businessDescription": "商企LOGO:LOGO:抖音",
                    "businessLabel1": "logo",
                    "businessLabel2": "logo",
                    "businessLabel3": "douyin"
                },
                {
                    "businessDescription": "商企LOGO:LOGO:火山",
                    "businessLabel1": "logo",
                    "businessLabel2": "logo",
                    "businessLabel3": "huoshan"
                }
            ],
            "imgText": "直播  物哪啊广播电视台劳荣技拒绝认罪当庭哭诉称自己是男友的性侵和赚钱工具爱害者家属：“希望她被干刀万别”两人随后来到熊家劫财法子英将熊启义的妻女杀害女儿当时3岁连个三岁的小孩都不放过被害人熊义弟编：熊正英抖音抖音号：xxx",
            "imgUrl": "http://bj-video-xxx.cos.ap-beijing.xxx.com/image%2F20211217%2F1639731708004_v15.jpg",
            "requestId": "1639731708004_v15",
            "riskDescription": "色情:性骚扰:重度性骚扰",
            "riskDetail": {
                "riskSource": 1001
            },
            "riskLabel1": "porn",
            "riskLabel2": "xingsaorao",
            "riskLabel3": "zhongduxingsaorao",
            "riskLevel": "REJECT",
            "time": 15
        },
        {
            "allLabels": [
                {
                    "riskDescription": "商企LOGO:LOGO:抖音",
                    "riskLabel1": "logo",
                    "riskLabel2": "logo",
                    "riskLabel3": "douyin",
                    "riskLevel": "REJECT"
                }
            ],
            "auxInfo": {
                "similarity": 0.6328125
            },
            "businessLabels": [
                {
                    "businessDescription": "商企LOGO:LOGO:抖音",
                    "businessLabel1": "logo",
                    "businessLabel2": "logo",
                    "businessLabel3": "douyin"
                },
                {
                    "businessDescription": "商企LOGO:LOGO:火山",
                    "businessLabel1": "logo",
                    "businessLabel2": "logo",
                    "businessLabel3": "huoshan"
                }
            ],
            "imgText": "重播的聊来抖音 发现更多有趣创作者抖音号：xxx",
            "imgUrl": "http://bj-video-xxx.cos.ap-beijing.xxx.com/image%2F20211217%2F1639731708004_v18.jpg",
            "requestId": "1639731708004_v18",
            "riskDescription": "商企LOGO:LOGO:抖音",
            "riskDetail": {
                "riskSource": 1002
            },
            "riskLabel1": "logo",
            "riskLabel2": "logo",
            "riskLabel3": "douyin",
            "riskLevel": "REJECT",
            "time": 18
        }
    ],
    "audioDetail": [
        {
            "allLabels": [
                {
                    "riskDescription": "唱歌:唱歌:唱歌",
                    "riskLabel1": "sing",
                    "riskLabel2": "changge",
                    "riskLabel3": "changge",
                    "riskLevel": "REJECT"
                }
            ],
            "audioEndtime": 10,
            "audioStarttime": 0,
            "audioText": "把自己打过打过打过我们的夜晚",
            "audioUrl": "http://bj-video-xxx.cos.ap-beijing.xxx.com/audio%2F20211217%2F1639731708004_a0000.wav",
            "businessLabels": [
                {
                    "businessDescription": "唱歌:唱歌:唱歌",
                    "businessLabel1": "sing",
                    "businessLabel2": "changge",
                    "businessLabel3": "changge"
                },
                {
                    "businessDescription": "性别:男性:男性",
                    "businessLabel1": "gender",
                    "businessLabel2": "male",
                    "businessLabel3": "male"
                }
            ],
            "requestId": "1639731708004_a0000",
            "riskDescription": "唱歌:唱歌:唱歌",
            "riskDetail": {
                "riskSource": 1003
            },
            "riskLabel1": "sing",
            "riskLabel2": "changge",
            "riskLabel3": "changge",
            "riskLevel": "REJECT"
        },
        {
            "allLabels": [
                {
                    "riskDescription": "唱歌:唱歌:唱歌",
                    "riskLabel1": "sing",
                    "riskLabel2": "changge",
                    "riskLabel3": "changge",
                    "riskLevel": "REJECT"
                }
            ],
            "audioEndtime": 18.55275,
            "audioStarttime": 10,
            "audioText": "是千刀万太恶毒了，连个三岁的小孩子都犯不过，抖音",
            "audioUrl": "http://bj-video-xxx.cos.ap-beijing.xxx.com/audio%2F20211217%2F1639731708004_a0001.wav",
            "businessLabels": [
                {
                    "businessDescription": "性别:男性:男性",
                    "businessLabel1": "gender",
                    "businessLabel2": "male",
                    "businessLabel3": "male"
                },
                {
                    "businessDescription": "唱歌:唱歌:唱歌",
                    "businessLabel1": "sing",
                    "businessLabel2": "changge",
                    "businessLabel3": "changge"
                }
            ],
            "requestId": "1639731708004_a0001",
            "riskDescription": "唱歌:唱歌:唱歌",
            "riskDetail": {
                "riskSource": 1003
            },
            "riskLabel1": "sing",
            "riskLabel2": "changge",
            "riskLabel3": "changge",
            "riskLevel": "REJECT"
        }
    ],
    "riskLevel": "REJECT",
    "auxInfo": {
        "frameCount": 7,
        "time": 18
    }
}
```
