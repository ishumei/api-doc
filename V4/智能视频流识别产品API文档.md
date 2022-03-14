
# 数美智能视频流识别产品API文档
- - - - -

***版权所有 翻版必究***

- - - - -

* [视频流上传请求](#uploadV4)
  
    + [接口描述](#uploadV4.interafceDesc)
    + [请求URL](#uploadV4.requestUrl)
    + [请求方法](#uploadV4.requestMethod)
    + [支持协议](#uploadV4.requestProtocol)
    + [字符编码](#uploadV4.requestEncode)
    + [建议超时时间](#uploadV4.requestTimeout)
    + [请求参数](#uploadV4.requestParameters)
    * [返回参数](#uploadV4.responseParameters)
* [异步回调结果](#callbackV4)
  
    + [接口描述](#callbackV4.interfaceDesc)
    + [请求方法](#callbackV4.callbackMethod)
    + [字符编码](#callbackV4.callbackEncode)
    + [建议超时时间](#callbackV4.callbackTimeout)
    + [回调策略](#callbackV4.callbackStrategy)
    + [回调参数](#callbackV4.callbackParameters)
* [视频流关闭请求](#closeV4)
  
    + [接口描述](#closeV4.interfaceDesc)
    + [请求URL](#closeV4.requestUrl)
    + [请求方法](#closeV4.requestMethod)
    + [支持协议](#closeV4.requestProtocol)
    + [字符编码](#closeV4.requestEncode)
    + [建议超时时间](#closeV4.requestTimeout)
    + [请求参数](#closeV4.requestParameters)
    + [返回参数](#closeV4.responseParameters)
* [示例](#demo)
  
    + [上传接口请求示例](#demo.requestUploadV4)
    + [上传接口返回示例](#demo.responseUploadV4)
    + [异步回调结果示例](#demo.callbackV4)
    + [关闭接口请求示例](#demo.requestCloseV4)
    + [关闭接口返回示例](#demo.responseCloseV4)

## <span id = "uploadV4">视频流上传请求</span>

### <span id = "uploadV4.interafceDesc">接口描述</span>

该接口用于提交视频流鉴定等相关信息，稳定拉流后将持续回调对应的识别结果至指定的callback地址。

### <span id = "uploadV4.requestUrl">请求URL：</span>
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-videostream-bj.fengkongcloud.com/videostream/v4` | 中文视频流 |
| 上海 | `http://api-videostream-sh.fengkongcloud.com/videostream/v4` | 中文视频流 |
| 新加坡 | `http://api-videostream-xjp.fengkongcloud.com/videostream/v4` | 中文视频流<br>英语视频流<br>阿语视频流 |
| 硅谷 | `http://api-videostream-gg.fengkongcloud.com/videostream/v4` | 中文视频流<br/>英语视频流<br/>阿语视频流 |
| 印度 | `http://api-videostream-yd.fengkongcloud.com/videostream/v4` | 中文视频流 |

### <span id = "uploadV4.requestMethod">请求方法：</span>

`POST` 

### <span id = "uploadV4.requestProtocol">支持协议</span>

`HTTP`或`HTTPS`

### <span id = "uploadV4.requestEncode">字符编码：</span>

`UTF-8`

### <span id = "uploadV4.requestTimeout">建议超时时间：</span>

7s

### <span id = "uploadV4.requestParameters">请求参数：</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 公司密钥 | 必传参数 | 数美分配 |
| appId | string | 应用标识 | 必传参数 | 该参数传递值可与数美协商 |
| eventId | string | 事件标识 | 必传参数 |  用于区分场景数据，可选值：<br>`video`:智能视频识别<br>`default`:默认事件<br>`live`:交友秀场<br>`ecommerce`:电商直播<br>`education`:教育场景<br>`presision`:默认高准确<br>`recall`:默认高召回 |
| imgType | string | 视频中的画面需要识别的监管类型，**和imgBusinessType至少传一个** | 非必传参数 | 监管一级标签<br>可选值：<br>`POLITICS`：涉政识别<br>`VIOLENCE`：暴恐识别<br>`BAN`：违禁识别<br>`PORN`：色情识别<br>`AD`：广告识别<br>`SPAM`：机器灌水识别<br>`OCR`：图片中的文字风险识别<br>如果需要识别多个功能，通过下划线连接，如`AD_PORN_POLITICS`用于广告、色情和涉政组合识别 |
| audioType | string | 视频流中的音频需要识别的监管类型 | 非必传参数 | 监管一级标签<br>可选值：<br>`POLITICAL`：涉政识别<br>`PORN`：色情识别<br>`AD`：广告识别<br>`MOAN`：娇喘识别<br>`NONE`:不检测音频<br>如需做组合识别，通过下划线连接即可，例如`POLITICAL_PORN_MOAN`用于广告、色情和涉政识别 |
| imgBusinessType | string | 视频中的画面需要识别的业务类型， **和imgType至少传一个** | 非必传参数 | 业务一级标签<br>可选值：<br>`SCREEN`：特殊画面识别<br>`SCENCE`：场景画面识别<br>`QR`：二维码识别<br>`FACE`：人脸识别<br>`QUALITY`：图像质量识别<br>`MINOR`：未成年人识别<br>`LOGO`：商企LOGO识别<br>`BEAUTY`：颜值识别<br>`FACECOMPARE`：人脸比对<br>`OBJECT`：物品识别<br>`STAR`：公众人物识别<br>如需做组合识别，通过下划线连接即可，例如`QR_FACE_MINOR`用于二维码、人脸和未成年人识别 |
| audioBusinessType | string | 视频流中的音频需要识别的业务类型 | 非必传参数 |  业务一级标签<br>可选值：<br>`SING`：唱歌识别<br>`LANGUAGE`：语种识别<br>`MINOR`：未成年人识别<br>`GENDER`：性别识别<br>`TIMBRE`：音色识别，需要同时传入`GENDER`才能生效 |
| imgCallback | string | 图片回调地址 | 必传参数 | 将视频流中截帧图片的检测结果通过该地址回调给用户 |
| audioCallback | string | 音频回调地址 | 非必传参数 | 将视频流中音频片段的检测结果通过该地址回调给用户；需要识别音频时必传 |
| data | json_object | 请求数据内容， | 必传参数 | 最长1MB，其中[data内容如下](#uploadV4.requestParameters.data) |

<span id = "uploadV4.requestParameters.data">其中，data的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| lang | string | 语种 | 必传参数 | 可选值如下：<br>`zh`：中文<br/>`en`：英语<br/>`ar`：阿语<br/>默认值：`zh` |
| tokenId | string | 客户端用户账号唯一标识 | 必传参数 | 用于用户行为分析，建议传入用户UID； 最长40位 |
| streamType | string | 视频流类型 | 必传参数 | 可选值为：<br>`NORMAL`：普通流地址，目前支持`rtmp`、`rtmps`、`hls`、`http`、`https`协议<br/>`AGORA`：声网审核<br>`TRTC`:腾讯审核<br>`ZEGO`：即构审核 |
| agoraParam | json_object | 声网流参数 | 非必传参数 | 要检测的声网流参数（当streamType为`AGORA`时必传），详见[agoraParam说明](#uploadV4.requestParameters.data.agoraParam) |
| trtcParam | json_object | 腾讯流参数 | 非必传参数 | 要检测的TRTC流参数（当streamType为`TRTC`时必传），详见[trtcParam说明](#uploadV4.requestParameters.data.trtcParam) |
| zegoParam | json_object | 即构流参数 | 非必传参数 | 要检测的即构流参数（当streamType为`ZEGO`时必传)，详见[zegoParam说明](#uploadV4.requestParameters.data.zegoParam) |
| url | string | 要检测的视频url地址 | 非必传参数 | 要检测的流地址url参数（当streamType为NORMAL时必传） |
| streamName | string | 视频流名称 | 非必传参数 | 用于后台界面展示，建议传入 |
| ip | string | 客户端IP | 非必传参数 | 该参数用于IP维度的用户行为分析，同时可用于比对数美IP黑库 |
| returnAllImg | int | 用户可根据需求选择返回不同审核结果的图片 | 非必传参数 | 可选值如下：（默认值为`0`）<br>`0`：回调reject、review结果的图片审核信息<br>`1`：回调所有结果的图片审核信息 |
| returnAllText | int | 返回音频流片段识别结果的风险等级 | 非必传参数 | 可选值如下：(默认值为`0`)<br>`0`：返回风险等级为非pass的音频片段与文本内容<br>`1`：返回所有风险等级的音频片段与文本内容 |
| returnPreText | int | | 非必传参数 | 可选值如下：（默认值为`false`）<br>`true`:返回的content字段包含违规音频前一分钟文本内容<br>`false`:返回的content字段只包含违规音频片段文本内容 |
| returnPreAudio | int | | 非必传参数 | 可选值如下：（默认值为`false`）<br>`true`:返回违规音频前一分钟音频链接<br>`false`:只返回违规片段音频链接 |
| detectFrequency | float | 截帧频率间隔 | 非必传参数 | 单位为秒，取值范围为0.5~60s；如不传递默认5s截帧一次 |
| detectStep | int | 视频流截帧图片检测步长 | 非必传参数 | 已截帧图片每个步长只会检测一次，取值大于等于1。 |
| room | string | 直播间/游戏房间编号 | 非必传参数 | 可针对单个房间制定不同的策略；（使用声网协议的用户建议传入） |
| retryTimes | int | 重试次数 | 非必传参数 | 视频流异常断开重试次数 |
| extra | json_object | 扩展信息 | 非必传参数 | 详见[extra说明](#uploadV4.requestParameters.data.extra) |

<span id="uploadV4.requestParameters.data.extra">data 中，extra的内容如下</span>

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| passThrough | json_object | 透传字段 | 非必传参数 | 该字段内容会随着回调结果一起原样返回 |

<span id="uploadV4.requestParameters.data.agoraParam">其中，agoraParam内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| appId | string | 声网提供的应用标识 | 必传参数 | |
| channel | string | 声网提供的频道名 | 必传参数 | |
| token | string | | 非必传参数 | 安全要求较高的用户可以使用 token，获取方式详见声网文档token生成方式：[https://docs.agora.io/cn/Recording/token](https://docs.agora.io/cn/Recording/token) |
| channelProfile | int | 声网录制的频道模式 | 否 | 可选值如下：（默认值为`0`）<br>`0`: 通信（默认）,即常见的 1 对 1 单聊或群聊，频道内任何用户可以自由说话；<br>`1`: 直播，有两种用户角色: 主播和观众。 |
| uid | int | 用户ID | 非必传参数 | 32位无符号整数。当channelKey存在时，必须提供生成channelKey时所使用的用户ID。注意，此处需要区别实际房间中的用户uid，提供给服务端录制所用的uid不允许在房间中存在 |

<span id="uploadV4.requestParameters.data.trtcParam">其中，trtcParam内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| sdkAppId | int | Y | 必传参数 | 腾讯提供的sdkAppId |
| demoSences | int | Y | 必传参数 | 录制类型可选值：<br>`2`:分流录制<br>`4`:合流录制 |
| userId | string | Y | 必传参数 | 分配给录制段的userId，限制长度为32bit，只允许包含（a-zA-Z），数字(0-9)以及下划线和连词符 |
| userSig | string | Y | 必传参数 | 录制userId对应的验证签名，相当于登录密码 |
| roomId | int | Y | 非必传参数 | 房间号码，取值范围：【1-4294967294】roomId与strRoomId必传一个，若两者都有值优先选用roomId 注意：目前一个房间最多只能审核8个用户|
| strRoomId | string | Y | 必传参数 | 房间号码取值说明：只允许包含（a-zA-Z），数字(0-9)以及下划线和连词符，若您选用strRoomId时，需注意strRoomId和roomId两者都有值，优先选用roomId |

<span id="uploadV4.requestParameters.data.zegoParam">其中，data.zegoParam内容如下：</span>

| 请求参数名 | 类型 | 参数说明 | 传入说明 | 规范 |
| --- | --- | --- | --- | --- |
| tokenId | string | Zego鉴权token | 必传参数 | zego提供的identity_token身份验证信息，用于token登陆（强烈建议每次开流主动调用zego接口获取新的token） |
| streamId | string | Zego流Id | 必传参数 | Zego的流ID |
| testEnv | bool | 是否使用zego测试环境 | 非必传参数 | 可选值如下：（默认值为`false`）<br>`true`:测试环境<br>`false`:正式环境 |

### <span id = "uploadV4.responseParameters">返回参数</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 本次请求的唯一标识 | 是 | 请求唯一标识 |
| code | int | 请求返回码 | 是 | 详见[接口响应码列表](#codeList) |
| message | string | 请求返回描述，和请求返回码对应 | 是 | 详见[接口响应码列表](#codeList) |

## <span id = "callbackV4">异步回调结果</span>

### <span id="callbackV4.interfaceDesc">接口描述</span>

用户如果需要服务端主动对视频检测结果进行回调，则需要在请求参数中指定回调协议接口URL callback参数，服务端根据该参数在视频审核完成后，主动回调用户。

### <span id = "callbackV4.callbackMethod">请求方法：</span>

`POST`

### <span id = "callbackV4.callbackEncode">字符编码：</span>

`UTF-8`

### <span id = "callbackV4.callbackTimeout">建议超时时间：</span>

5s

### <span id="callbackV4.callbackStrategy">回调策略</span>

当用户收到推送结果，并返回HTTP状态码为200时，表示推送成功；否则系统将进行最多20次推送。

### <span id = "callbackV4.callbackParameters">回调参数</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 本次请求的唯一标识 | 是 | 请求唯一标识 |
| code | int | 请求返回码 | 是 | 详见[接口响应码列表](#codeList) |
| message | string | 请求返回描述，和请求返回码对应 | 是 | 详见[接口响应码列表](#codeList) |
| contentType | int | 用来区分音频和图片回调，当code等于1100时返回 | 否 | 可能取值如下：<br>`1`：该回调为图片回调<br>`2`：该回调为音频回调 |
| frameDetail | json_object | 风险截帧信息 | 否 | 当code等于`1100`时返回，详见[frameDetail说明](#callbackV4.callbackParameters.frameDetail) |
| audioDetail | json_object | 风险音频片段信息 | 否 | 当code等于`1100`时返回，详见[audioDetail说明](#callbackV4.callbackParameters.audioDetail) |
| auxInfo | json_object | 辅助信息 | 否 | 当code等于`1100`时返回，详见[auxInfo说明](#callbackV4.callbackParameters.auxInfo) |

<span id="callbackV4.callbackParameters.auxInfo">其中，auxInfo中的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| passThrough | json_object | 透传字段 | 否 | 该字段内容与请求参数data中extra的passThrough的值相同 |

<span id="callbackV4.callbackParameters.frameDetail">其中，在图片回调时（contentType为`1`时），frameDetail每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| imgUrl | string | 当前截帧的URL | 是 | |
| riskLevel | string | 当前截帧的处置建议 | 是 | `PASS`：正常内容<br>`REVIEW`：可疑内容<br>`REJECT`：违规内容 |
| riskLabel1 | string | 各个一级标签之间是并列的关系，当riskLevel为`PASS`时返回`normal` | 是 | 一级标签 |
| riskLabel2 | string | 二级标签归属于一级标签，当riskLevel为`PASS`时为空 | 是 | 二级标签 |
| riskLabel3 | string | 三级标签归属于二级标签，当riskLevel为`PASS`时为空 | 是 | 三级标签 |
| riskDescription | string | 标签解释 | 是 | 对于命中用户自定义名单时返回：`命中自定义名单`；当riskLevel为`PASS`时返回:`正常`；其他情况展现形式为一级标签：二级标签：三级标签的中文名 |
| riskDetail | json_object | 风险详情信息 | 是 | 详见[riskDetail说明](#callbackV4.callbackParameters.frameDetail.riskDetail) |
| allLabels | json_array | 全部的风险标签列表 | 是 | 详见[allLabels说明](#callbackV4.callbackParameters.frameDetail.allLabels) |
| businessLabels | json_array | 业务标签列表，传入imgBusinessType时返回 | 否 | 详见[businessLabels说明](#callbackV4.callbackParameters.frameDetail.businessLabels) |
| auxInfo | json_object | 其他辅助信息 | 是 | 详见[auxInfo说明](#callbackV4.callbackParameters.frameDetail.auxInfo) |

<span id="callbackV4.callbackParameters.frameDetail.auxInfo">frameDetail中auxInfo的内容如下：</span>

| imgTime | float | 截帧图片发生时间 | 是 | 视频流截帧图片违规发生的时间（绝对时间） |
| --- | --- | --- | --- | --- |
| beginProcessTime | int | 辅助参数 | 是 | 开始处理的时间（13位时间戳） |
| finishProcessTime | int | 辅助参数 | 是 | 结束处理的时间（13位时间戳） |
| userId | int | 用户账号标识 | 否 | 仅分流情况下存在，返回的userId是实际房间中的用户id，与请求参数中的uid无关。 |
| strUserId | string | trtc流的用户id字段 | 否 | 分流的用户id（`TRTC`流才会有） |
| detectType | int | 用来区分截帧图片是否过了检测 | 否 | 可能取值如下：（仅当请求参数传了detectStep时才会返回该参数）<br>`1`：截帧图片过了检测<br>2：截帧图片没过检测 |
| room | string | 房间号 | 否 | |

<span id="callbackV4.callbackParameters.frameDetail.riskDetail">frameDetail中，riskDetail的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskSource | int | 风险来源 | 是 | 风险来源，可选值：<br>`1000`：无风险<br>`10001`：文本风险<br>`1002`：视觉风险<br>`1003`：音频风险 |
| faces | json_array | 人物信息 | 否 | 返回图片中涉政人物的名称及位置信息，详见[faces说明](#callbackV4.callbackParameters.frameDetail.riskDetail.faces) |
| objects | json_array | 标识信息 | 否 | 返回图片中标识或物品的名称及位置信息，详见[objects说明](#callbackV4.callbackParameters.frameDetail.riskDetail.objects) |
| ocrText | json_object | 文字信息 | 否 | 返回图片中文字相关信息，详见[ocrText说明](#callbackV4.callbackParameters.frameDetail.riskDetail.ocrText) |

<span id="callbackV4.callbackParameters.frameDetail.riskDetail.faces">riskDetail中，faces数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 人物名称 | 否 | |
| location | int_array | 人物位置信息，四个值代表的是左上角的坐标和右下角的坐标。例如[207,522,340,567]，207代表的是左上角的x坐标，522代表左上角的y坐标，340代表的是右下角的x坐标，567代表的是右下角的y坐标 | 否 | 位置信息 |

<span id="callbackV4.callbackParameters.frameDetail.riskDetail.objects">riskDetail中，objects数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 标识或物品名称 | 否 | |
| location | int_array | 标识或物品位置，四个值代表的是左上角的坐标和右下角的坐标。例如[207,522,340,567]，207代表的是左上角的x坐标，522代表左上角的y坐标，340代表的是右下角的x坐标，567代表的是右下角的y坐标 | 否 | 位置信息 |

<span id="callbackV4.callbackParameters.frameDetail.riskDetail.ocrText">riskDetail中，ocrText数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| text | string | 图片中识别出的文字 | 是 | 识别出来所有文字内容 |
| matchedLists | json_array | 命中的客户自定义名单信息 | 否 | 仅在命中客户自定义名单时返回，详见[matchedLists说明](#callbackV4.callbackParameters.frameDetail.riskDetail.ocrText.matchedLists) |
| riskSegments | json_array | 高风险内容片段 | 否 | 在启用涉政、暴恐、违禁、广告等功能时存在，详见[riskSegments说明](#callbackV4.callbackParameters.frameDetail.riskDetail.ocrText.riskSegments) |

<span id="callbackV4.callbackParameters.frameDetail.riskDetail.ocrText.matchedLists">ocrText中，matchedLists内每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 客户自定义名单名称 | 是 | |
| words | json_array | 命中的这个名单中的敏感词信息 | 是 | 下标从0开始计数，详见[words说明](#callbackV4.callbackParameters.frameDetail.riskDetail.ocrText.matchedLists.words) |

<span id="callbackV4.callbackParameters.frameDetail.riskDetail.ocrText.matchedLists.words">matchedLists中，words的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string | 敏感词 | 是 | |
| position | int_array | 敏感词所在位置 | 是 | 下标从0开始计数 |

<span id="callbackV4.callbackParameters.frameDetail.riskDetail.ocrText.riskSegments">ocrText中，riskSegments的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segment | string | 高风险内容片段 | 否 | |
| position | int_array | 高风险内容片段所在位置 | 否 | 下标从0开始计数 |

<span id="callbackV4.callbackParameters.frameDetail.allLabels">frmeDetail中，allLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险描述 | 是 | 格式为&quot;一级风险标签：二级风险标签：三级风险标签&quot;的中文名称<br>对于命中用户自定义名单时返回：`命中自定义名单` |
| riskLevel | string | 处置建议 | 是 | `PASS`：正常内容<br>`REVIEW`：可疑内容<br>`REJECT`：违规内容 |

<span id="callbackV4.callbackParameters.frameDetail.businessLabels">frmeDetail中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |

<span id="callbackV4.callbackParameters.audioDetail">其中，在音频回调时（contentType为2时），audioDetail每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| audioUrl | string | 音频片段地址 | 是 | |
| riskLevel | string | 当前事件的处置建议 | 是 | `PASS`：正常内容<br>`REVIEW`：可疑内容<br>`REJECT`：违规内容 |
| riskLabel1 | string | 各个一级标签之间是并列的关系，riskLevel为`PASS`时返回`normal` | 是 | 一级标签 |
| riskLabel2 | string | 二级标签归属于一级标签，当riskLevel为`PASS`时为空 | 是 | 二级标签 |
| riskLabel3 | string | 三级标签归属于二级标签，当riskLevel为`PASS`时为空 | 是 | 三级标签 |
| riskDescription | string | 标签解释 | 是 | 格式为&quot;一级风险标签：二级风险标签：三级风险标签&quot;的中文名称<br>对于命中用户自定义名单时返回：`命中自定义名单` |
| riskDetail | json_object | 风险详情信息 | 是 | 详见[riskDetail说明](#callbackV4.callbackParameters.audioDetail.riskDetail) |
| allLabels | json_array | 全部的风险标签列表 | 是 | 详见[allLabels说明](#callbackV4.callbackParameters.audioDetail.allLabels) |
| businessLabels | json_array | 业务标签列表，传入audioBusinessType时返回 | 否 | 详见[businessLabels说明](#callbackV4.callbackParameters.audioDetail.businessLabels) |
| auxInfo | json_object | 其他辅助信息 | 是 | 详见[auxInfo说明](#callbackV4.callbackParameters.audioDetail.auxInfo) |

<span id="callbackV4.callbackParameters.audioDetail.auxInfo">audioDetail中auxInfo的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| beginProcessTime | int | 辅助参数 | 是 | 开始处理的时间（13位时间戳） |
| finishProcessTime | int | 辅助参数 | 是 | 结束处理的时间（13位时间戳） |
| userId | int | 声网用户账号标识 | 否 |仅分流情况下存在，返回的userId是实际房间中的用户id，与请求参数中的uid无关。 |
| strUserId | string | trtc流的用户id字段 | 否 | 分流的用户id（`TRTC`流才会有） |
| room | string | 房间号 | 否 | |

<span id="callbackV4.callbackParameters.audioDetail.riskDetail">audioDetail中，riskDetail的详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskSource | int | 风险来源 | 是 | 风险来源，可选值：<br>`1000`：无风险<br>`10001`：文本风险<br>`1002`：视觉风险<br>`1003`：音频风险 |
| audioText | string | 音频转译文本的结果 | 否 | |
| matchedLists | json_array | 命中的客户自定义名单信息 | 否 | 命中客户自定义名单时返回，其他时不存在，详见[matchedLists说明](#callbackV4.callbackParameters.audioDetail.riskDetail.matchedLists) |
| riskSegments | json_array | 高风险内容片段 | 否 | 在涉政、暴恐、违禁、竞品、广告法等功能的时候存在，详见[riskSegments说明](#callbackV4.callbackParameters.audioDetail.riskDetail.riskSegments) |

<span id="callbackV4.callbackParameters.audioDetail.riskDetail.matchedLists">riskDetail中，matchedLists的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 客户自定义名单名 | 是 | |
| words | json_array | 命中的这个名单中的敏感词信息 | 是 | 下标从0开始计数，详见[words说明](#callbackV4.callbackParameters.audioDetail.riskDetail.matchedLists.words) |

<span id="callbackV4.callbackParameters.audioDetail.riskDetail.matchedLists.words">matchedLists中，words的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string | 敏感词 | 是 | |
| position | int_array | 敏感词所在位置 | 是 | 下标从0开始计数 |

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
| riskDescription | string | 风险描述 | 是 | 格式为&quot;一级风险标签：二级风险标签：三级风险标签&quot;的中文名称<br>对于命中用户自定义名单时返回：`命中自定义名单` |
| riskLevel | string | 处置建议 | 是 | `PASS`：正常内容<br>`REVIEW`：可疑内容<br>`REJECT`：违规内容 |

<span id="callbackV4.callbackParameters.audioDetail.businessLabels">audioDetail中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |


## <span id = "closeV4">视频流关闭接口</span>

### <span id = "closeV4.interfaceDesc">接口描述</span>

该接口用于客户端通知服务端某个视频流已关闭。

### <span id = "closeV4.requestUrl">请求URL：</span>

| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-videostream-bj.fengkongcloud.com/finish_videostream/v4` | 中文视频流 |
| 上海 | `http://api-videostream-sh.fengkongcloud.com/finish_videostream/v4` | 中文视频流 |
| 新加坡 | `http://api-videostream-xjp.fengkongcloud.com/finish_videostream/v4` | 中文视频流<br>英语视频流<br>阿语视频流 |
| 硅谷 | `http://api-videostream-gg.fengkongcloud.com/finish_videostream/v4` | 中文视频流<br/>英语视频流<br/>阿语视频流 |
| 印度 | `http://api-videostream-yd.fengkongcloud.com/finish_videostream/v4` | 中文视频流 |

### <span id = "closeV4.requestMethod">请求方法：</span>

`POST`

### <span id = "closeV4.requestProtocol">支持协议：</span>

`HTTP`或`HTTPS`

### <span id = "closeV4.requestEncode">字符编码：</span>

`UTF-8`

### <span id = "closeV4.requestTimeout">建议超时时间：</span>

1s

### <span id = "closeV4.requestParameters">请求参数：</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 公司密钥 | 必传参数 | 用于权限认证，开通账号服务时由数美提供 |
| requestId | string | 本次请求的唯一标识 | 必传参数 | 需要关闭视频流的requestId |

### <span id = "closeV4.responseParameters">返回参数</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 本次请求的唯一标识 | 是 | 请求唯一标识 |
| code | int | 请求返回码 | 是 | 请见[接口响应码列表](#codeList) |
| message | string | 请求返回描述，和请求返回码对应 | 是 | 请见[接口响应码列表](#codeList) |

## <span id="codeList">接口响应码列表</span>

<span id="closeV4.responseParameters.code">code请求返回码列表如下：</span>

| **code** | **message**      |
| -------- | ---------------- |
| 1100     | 成功             |
| 1901     | QPS超限          |
| 1902     | 参数不合法       |
| 1903     | 服务失败         |
| 1907     | 获取视频长度超时 |
| 9100     | 余额不足         |
| 9101     | 无权限操作       |

## <span id = "demo">示例</span>

### <span id = "demo.requestUploadV4">上传接口请求示例</span>

```json
{
    "accessKey": "*********",
    "appId": "defaulttest",
    "eventId": "VIDEOSTREAM",
    "imgType": "POLITICS_VIOLENCE_BAN_PORN_MINOR_AD_SPAM_LOGO_STAR",
    "imgBusinessType": "SCREEN_SCENCE_QR_FACE_QUALITY_MINOR_LOGO_BEAUTY_FACECOMPARE",
    "audioType": "POLITICAL_PORN_AD_MOAN",
    "audioBusinessType": "SING_LANGUAGE_MINOR_GENDER_TIMBRE",
    "imgCallback": "http://www.xxx.top/xxx",
    "audioCallback": "http://www.xxx.top/xxx",
    "data": {
        "streamType": "NORMAL",
        "tokenId": "123",
        "ip": "123.171.34.4",
        "detectFrequency": 10,
        "detectStep": 1,
        "room": "5e1854a6a0a79d0001a09bc3",
        "url": "http://rtmp.xxxx.cn/live/3637778raLSXdOdu.flv",
        "returnAllImg": 1,
        "returnAllText": 1,
        "returnPreText": 1,
        "returnPreAudio": 1,
        "retryTimes": 1,
        "lang": "zh",
        "extra": {
            "passThrough": {
                "passThrough1": "111",
                "passThrough2": "222",
                "passThrough3": "333"
            }
        }
    }
}
```

### <span id = "demo.responseUploadV4">上传接口返回示例：</span>

```json
{
    "code":1100,
    "message":"正常",
    "requestId":"d05f4270374ca516ce7aafc0139afd25"
}
```

### <span id = "demo.callbackV4">异步回调结果示例：</span>

```json
{
    "code": 1100,
    "contentType": 1,
    "message": "成功",
    "requestId": "1639825145166_vs130_1639825248361471656",
    "frameDetail": {
        "allLabels": [
            {
                "riskDescription": "涉政:核心领导:毛泽东",
                "riskLabel1": "politics",
                "riskLabel2": "hexinlingdao",
                "riskLabel3": "maozedong",
                "riskLevel": "REJECT"
            },
            {
                "riskDescription": "涉政:涉政:涉政",
                "riskLabel1": "politics",
                "riskLabel2": "shezheng",
                "riskLabel3": "shezheng",
                "riskLevel": "REJECT"
            }
        ],
        "auxInfo": {
            "beginProcessTime": 1639825248361,
            "detectType": 1,
            "finishProcessTime": 1639825248809,
            "imgTime": "2021-12-18 19:00:48.375",
            "room": "5e1854a6a0a79d0001a09bc3"
        },
        "businessLabels": [],
        "imgUrl": "http://bj.cos.ap-beijing.xxx.com/image/1639825145166_vs130_1639825248361471656.jpg",
        "riskDescription": "涉政:核心领导:毛泽东",
        "riskDetail": {
            "ocrText": {
                "text": "第四页（ban第五页（violence"
            },
            "riskSource": 1002
        },
        "riskLabel1": "politics",
        "riskLabel2": "hexinlingdao",
        "riskLabel3": "maozedong",
        "riskLevel": "REJECT"
    },
    "auxInfo": {
        "passThrough": {
            "passThrough1": "111",
            "passThrough2": "222",
            "passThrough3": "333"
        }
    }
}
```



### <span id = "demo.requestCloseV4">关闭接口请求示例：</span>

```json
{
    "accessKey": "4Ky6AV4hE0pWLeG1bXNw",
    "requestId": "1639825145166"
}
```

### <span id = "demo.responseCloseV4">关闭接口返回示例</span>

```json
{
    "code":1100,
    "message":"成功",
    "requestId":" a78eef377079acc6cdec24967ecde722"
}
```
