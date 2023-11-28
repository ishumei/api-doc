# 数美智能视频文件识别产品API文档

- - - - -

***版权所有 翻版必究***

- - - - -
## 视频文件上传请求

### 接口描述

该接口用于提交视频相关信息，自定义截帧频率等参数。识别结果需客户自行定期调用查询接口获取。

### 请求URL：
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-video-bj.fengkongcloud.com/v2/saas/anti_fraud/video` | 中文视频文件 |
| 上海 | `http://api-video-sh.fengkongcloud.com/v2/saas/anti_fraud/video` | 中文视频文件 |
| 新加坡 | `http://api-video-xjp.fengkongcloud.com/v2/saas/anti_fraud/video` | 中文视频文件 |
| 硅谷 | `http://api-video-gg.fengkongcloud.com/v2/saas/anti_fraud/video` | 中文视频文件 |

### 请求方法：

`POST`

### 字符编码：

`UTF-8`

### 建议超时时间：

1s

### 视频格式限制：

`AVI`、`FLV`、`MP4`、`MPG`、`WMV`、`MOV`、`WMA`、`RMVB`、`m3u8`

### 视频大小限制：

小于等于300MB

### 视频时长限制：

小于等于2小时

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 公司密钥 | 必传参数 | 由数美提供，数美分配 |
| appId | string | 应用标识 | 必传参数 | 用于区分应用，需要联系数美开通，请使用数美单独提供的传值为准 |
| btId | string | 视频唯一标识 | 必传参数 | 视频唯一标识，用于查询识别结果，最长64位 |
| imgType | string | 视频中的画面需要识别的监管类型，**和imgBusinessType至少传一个** | 非必传参数 | 监管一级标签<br/>可选值：<br/>`POLITICS`：涉政识别, 这里POLITICS实际识别内容为涉政人物和暴恐<br/>`PERSON`涉政人物识别<br/>`VIOLENCE`：暴恐识别<br/>`PORN`：色情识别<br/>`AD`：广告识别<br/>`QR`：二维码识别<br/>`OCR`：图片中的文字风险识别<br/>`BEHAVIOR`：不良场景识别,支持吸烟、喝酒、赌博、吸毒、避孕套和无意义画面<br/>如果需要识别多个功能，通过下划线连接，如`AD_PORN_POLITICS`用于广告、色情和涉政组合识别 |
| audioType | string | 视频中的音频需要识别的监管类型， **和audioBusinessType至少传一个** | 非必传参数 | 监管一级标签<br/>可选值：<br/>`POLITICS`：涉政识别<br/>`PORN`：色情识别<br/>`AD`：广告识别<br/>`MOAN`：娇喘识别<br/>`ABUSE`：辱骂识别<br/>`ANTHEN`：国歌识别<br/>`AUDIOPOLITICAL`：声音涉政<br/>`NONE`:不检测音频<br/>如需做组合识别，通过下划线连接即可，例如`POLITICS_PORN_MOAN`用于广告、色情和涉政识别 |
| imgBusinessType | string | 视频中的画面需要识别的业务类型， **和imgType至少传一个** | 非必传参数 | 可选值参考[imgBusinessType可选值列表](#imgbusinesstype可选值列表)<br/> |
| audioBusinessType | String | 视频中的音频业务识别类型， **和audioType至少传一个** | 非必传参数 | 业务一级标签<br/>可选值：<br/>`SING`：唱歌识别<br/>`LANGUAGE`：语种识别（中文、英文、粤语、藏语、维吾尔语、朝鲜语、蒙语、其他）<br/>`MINOR`：未成年人识别<br/>`GENDER`：性别识别<br/>`TIMBRE`：音色识别，需要同时传入`GENDER`才能生效 |
| callback | string | 指定回调url地址 | 非必传参数 | 当该字段非空时，服务将根据该字段回调通知用户审核结果（支持`http`/`https`） |
| callbackParam | json_object | 回调透传字段 | 非必传参数 |  |
| data | json\_object | 本次请求相关信息，最长1MB | 必传参数 | 最长1MB，其中[data内容如下](#data) |

<span id="data">其中，data的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| url | string | 要检测的视频url地址 | 必传参数 | |
| tokenId | string | | 必传参数 | 客户端用户账号唯一标识，用于用户行为分析，建议传入用户UID； 最长40位 |
| lang | string | 语言类型 | 非必传参数 | 可选值：<br/>zh ：中文<br/>en ：英文<br/>ar ：阿拉伯语<br/>不传默认进行中文检测 |
| detectFrequency | int | 截帧频率间隔，单位为秒 | 非必传参数 | 取值范围为1~60s；如不传递默认5s截帧一次 |
| advancedFrequency | json_object | 高级截帧间隔，单位为秒 | 非必传参数 | 高级截帧设置，此项填写，默认截帧策略失效<br/>参数配置如下<br/>{"durationPoints":[300,600],"frequencies":[1,5,10]}<br/>含义为：<br/>视频文件时长≤300s ——选用1s一截帧<br/>300s<视频文件时长≤600s ——选用5s一截帧<br/>视频文件时长>600s ——选用10s一截帧 |
| ip | string | 客户端IP | 非必传参数 | 用于IP维度的用户行为分析，同时可用于比对数美IP黑库，支持ipv4和ipv6的传入 |
| audioDetectStep | int | 视频文件中的音频审核步长 | 非必传参数 | 单位为个，取值范围为1-36整数，取1表示跳过一个10S的音频片段审核，取2表示跳过二个，以此类推。不使用该功能时音频内容全部过审 |
| retallImg | int | | 非必传参数 | 选择返回视频截帧图片的等级：0：返回风险等级为非pass的图片1：返回所有风险等级的图片默认为0 |
| retallAudio | int | | 非必传参数 | 选择返回视频音频片段的等级：0：返回风险等级为非pass的音频片段1：返回所有风险等级的音频片段默认为0 |
| videoName | string | 视频名称 | 非必传参数 | 视频名称，用于后台界面展示 |
| subtitle | string | 视频字幕 | 非必传参数 | 文本内容审核 |
| videoCover | string | 视频封面 | 非必传参数 | 视频封面图片审核 |
| channel | string | 客户渠道（不传为默认值） | 非必传参数 |[渠道配置表示例参考](#channel)  |
| dataId | string | 数据标识 | 非必传参数 | |

<span id="advancedFrequency">data 中，advancedFrequency的内容如下</span>

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| durationPoints | int_array | 视频时长区间分割 | 非必传参数 | 用于规定视频文件支持动态截帧频率的时长区间，数组最多为5个 |
| frequencies | int_array | 视频时长区间对应的截帧频率 | 非必传参数 | 可设置范围为1~60秒，数组最多6个<br/>说明：frequencies数组设置的个数需要比durationPoints数组个数多1个，传错或传空报错返回1902 |

<span id="channel">data 中，channel的内容如下</span>

数美根据客户不同业务场景，配置不同的渠道（channel），制定针对性的拦截策略，同时也方便客户针对不同业务场景的数据进行筛选、分析。业务场景和渠道取值对应表如下（支持客户自定义）

| **业务场景** | **channel取值** | **传入说明** |
| --- | --- | --- |
| 短视频 | SHORT_VIDEO |时长在分钟级别的短片视频 |
| 群聊 | GROUP_CHAT |社交场景群聊发送的视频消息 |
| 私聊 | MESSAGE |社交场景私聊发送的视频消息 |
| 录播视频 | VIDEO |视频平台产生的视频文件 |

### 返回参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 本次请求的唯一标识 | 是 | 请求唯一标识 |
| code | int | 请求返回码 | 是 | 详见[接口响应码列表](#接口响应码列表) |
| message | string | 请求返回描述 | 是 | 详情描述如上 |
| btId | string | 唯一标识客户上传的视频 | 是 | 仅当code=1100时返回，与请求参数中的btId字段对应 |

## 异步回调结果

### 接口描述

用户如果需要服务端主动对视频检测结果进行回调，则需要在请求参数中指定回调协议接口URL callback参数，服务端根据该参数在视频审核完成后，主动回调用户。

### 请求方法：

`POST`

### 字符编码：

`UTF-8`

### 建议超时时间：

5s

### 支持协议：

`HTTP`或`HTTPS`

### 回调策略：

当用户收到推送结果，并返回HTTP状态码为200时，表示推送成功；否则回调失败，系统将进行重试，最多20次推送。

### 回调参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| checksum | string | 由accessKey + btId + result拼成字符串，通过SHA256算法生成。为防止篡改，可以按此算法生成字符串，与checksum做一次校验。 | 是 |  |
| result | string | 机器审核结果 | 是 | 详见[result字段说明](#result) |

<span id="result">result可反序列化为json结构，内容如下</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | 详见[接口响应码列表](#接口响应码列表) |
| message | string | 返回码详情描述 | 是 | |
| requestId | string | 请求唯一标识 | 是 | |
| btId | string | 视频唯一标识 | 是 | 最长64位 |
| riskLevel | string | 风险级别，code为1100时存在 | 否 | 返回值：<br/>`PASS`：正常内容，建议直接放行<br/>`REVIEW`：可疑内容，建议人工审核<br/>`REJECT`：违规内容，建议直接拦截 |
| labels | string | 风险标签（code为1100时存在）| 否|  |
| detail | json_array | 风险详情 | 否 | 详见[detail说明](#frameDetail) |
| addition | json_array | 音频片段信息 | 否 | code为`1100`时存在，详见[addition说明](#addition)|
| callbackParam | json_object | 回调透传字段 | 否 | 当提交请求中，callbackParam字段有值时存在 |
| auxInfo | json_object | 辅助信息 | 否 |code为`1100`时存在，详见[auxInfo说明](#auxInfo)|
| tokenProfileLabels | json_array | 账号属性标签 | 否 |仅在开启功能时返回，详见[tokenProfileLabels说明](#tokenProfileLabels)|
| tokenRiskLabels | json_array | 账号风险标签 | 否 |仅在开启功能时返回，详见[tokenRiskLabels说明](#tokenRiskLabels)|

<span id="auxInfo">其中，auxInfo中的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| frameCount | int | 返回的视频截帧数量。retallImg=0时为风险数量，retallImg=1时为全部数量 | 是 |  |
| billingAudioDuration | float | 审核的视频中音频的时长  | 是 |  |
| billingImgNum | int | 审核的视频截帧数量 | 是 |  |
| time | int | 视频时长 | 是 |  |

<span id="frameDetail">其中，截帧图片detail数组中每个成员的具体内容如下：</span>

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
| logos | json_array | 返回视频帧识别出来的logo结果<br/>如："logos": ["douyin"] | 否 |  |
| businessLabels | json_array | 识别出的业务标签 | 否 |传入imgBusinessType时返回，详见[businessLabels说明](#businessLabels)   |

<span id="businessLabels">detail中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| confidenceLevel | int | 置信等级 | 是 | 可选值在0～2之间，值越大，可信度越高<br/> |
| probability         | float    | 置信度       | 是           | 可选值为0~1，值越大，可信度越高 |
| businessDetail | Json_object | 详细信息 | 是 |  |

businessLabels中，businessDetail的内容如下：

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

<span id="addition">其中，addition每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| audio_evidence | json_array | 音频片段详情数组 | 否 | 当请求参数retallAudio传值为1时<br/>数组中将包含正常内容片段的相关详情，否则只返回非PASS的片段的相关详情，详见[audio_evidence说明](#audio_evidence) |
| subtitleDetail | json_array | 视频字幕存在时返回 | 否 | 视频字幕，详见[subtitleDetail](#subtitleDetail) |
| videoCoverDetail | json_array | 视频封面存在时返回 | 否 | 视频封面，详见[videoCoverDetail](#videoCoverDetail) |

<span id="audio_evidence">addition中，audio_evidence的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| audioModel | string | 命中规则编号 | 是 |  |
| audioText | string | 返回音转文文字 | 否 | |
| audio_starttime | float | 违规音频发生时间 | 否 | |
| audio_endtime | float | 违规音频结束时间 | 否 | |
| audio_url | string | 音频片段地址 | 否 | |
| audio_matchedItem | string | 违规音频敏感词内容 | 否 | |
| description | string | 音频片段风险原因描述 | 是 | |
| requestId | string | 音频片段唯一标识 | 是 | 一般用户历史记录查询某一片段内容 |
| riskLevel | string | 处置建议 | 是 | 取值范围：<br/>PASS：正常 <br/>REVIEW：审核 <br/>REJECT：拒绝|
| riskType | int | 风险类型 | 是 | 标识风险类型，可能取值：<br/>0：正常<br/>100：涉政/国歌<br/>110: 暴恐<br/>200：色情<br/>210：辱骂<br/>250：娇喘<br/>260：一号领导人声纹<br/>300：广告<br/>400：灌水<br/>500：无意义<br/>520：未成年人<br/>600 : 违禁<br/>700：其他<br/>720：黑账号<br/>730：黑IP<br/>800：高危账号<br/>900：自定义 |
| riskSource | int  | 音频转译文本的结果 | 是 |风险来源，可能取值：<br/>1000：无风险 <br/>1001：文字 <br/>1002：视觉图片 <br/>1003：音频语音 |
| isSing | int  | 该条音频片段是否唱歌 | 否 | 取值范围：<br/>0:表示没有唱歌，<br/>1:表示唱歌。<br/>仅当type传入值包含SING时返回。 |
| language | json_array | 语种标签与概率值列表 | 否| 详见[language说明](#language) |
| businessLabels | json_array | 识别出的业务标签 | 否 | 详见[businessLabels说明](#businessLabels) |

<span id="language">音频的audio_evidence中，language数组中每一项具体参数如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| label | int | 语种识别类别 | 是 |语种识别类别标识，可能取值：<br/>0:普通话<br/>1:英语<br/>2:粤语<br/>3:藏语<br/>4:蒙语<br/>5:维语<br/>6:朝鲜语<br/>7:其他<br/> |
| probability | int | 对应音色标签可能性大小，取值0-100，数值越高表示概率越大 | 是 | 取值范围[0,100] |

<span id="businessLabels">audio_evidence中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| confidenceLevel | int | 置信等级 | 是 | 可选值在0～2之间，值越大，可信度越高 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |
| businessDetail | Json_object | 详细信息 | 是 |  |

<span id="subtitleDetail">addition中，subtitleDetail的详细内容如下：</span>

| **参数名**  | **类型** | **参数说明**   | **是否必返** | **规范**                                                     |
| ----------- | -------- | -------------- | ------------ | ------------------------------------------------------------ |
| code        | int      | 返回码         | 是           | 详见[接口响应码列表](#接口响应码列表)                              |
| message     | string   | 返回码详情描述 | 是           |                                                              |
| description | string   | 风险原因       | 是           |                                                              |
| riskLevel   | string   | 处置建议       | 是           | 取值范围：<br/>PASS：正常 <br/>REVIEW：审核 <br/>REJECT：拒绝 |
| requestId   | string   | 字幕唯一标识   | 是           |                                                              |

<span id="videoCoverDetail">addition中，videoCoverDetail的详细内容同subtitleDetail</span>

<span id="tokenProfileLabels">其中，tokenProfileLabels数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| label1 | string | 一级标签 | 否 | |
| label2 | string | 二级标签 | 否 | |
| label3 | string | 三级标签 | 否 | |
| description | string | 标签描述 | 否 | |
| timestamp | int | 打标签时间戳 | 否 | 13位Unix时间戳，单位：毫秒 |

<span id="tokenRiskLabels">其中，tokenRiskLabels数组每个成员的具体字段同tokenProfileLabels</span>

## 查询视频结果

该接口用于客户主动查询视频文件识别结果，建议每30s进行一次查询。

### 接口描述

该接口用于客户主动查询视频文件识别结果，建议每30s进行一次查询。

### 请求URL：
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-video-bj.fengkongcloud.com/v2/saas/anti_fraud/query_video` | 中文视频文件 |
| 上海 | `http://api-video-sh.fengkongcloud.com/v2/saas/anti_fraud/query_video` | 中文视频文件 |
| 新加坡 | `http://api-video-xjp.fengkongcloud.com/v2/saas/anti_fraud/query_video` | 中文视频文件 |
| 硅谷 | `http://api-video-gg.fengkongcloud.com/v2/saas/anti_fraud/query_video` | 中文视频文件 |
| 印度 | `http://api-video-yd.fengkongcloud.com/v2/saas/anti_fraud/query_video` | 中文视频文件 |

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

| **参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 用于权限认证，开通账号服务时由数美提供 | 必传参数 | |
| btId | string | 视频唯一标识，用于查询识别结果，最长64位 | 必传参数 | |

###  返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | 详见[接口响应码列表](#接口响应码列表) |
| message | string | 返回码详情描述 | 是 | |
| requestId | string | 请求唯一标识 | 是 | |
| btId | string | 视频唯一标识 | 是 | 最长64位 |
| riskLevel | string | 风险级别，code为1100时存在 | 否 | 返回值：<br/>`PASS`：正常内容，建议直接放行<br/>`REVIEW`：可疑内容，建议人工审核<br/>`REJECT`：违规内容，建议直接拦截 |
| labels | string | 风险标签（code为1100时存在）| 否|  |
| detail | json_array | 风险详情 | 否 | 详见[detail说明](#callbackV2.callbackParameters.query..frameDetail) |
| addition | json_array | 音频片段信息 | 否 | code为`1100`时存在，详见[addition说明](#callbackV2.callbackParameters.query.audioDetail)|
| auxInfo | json_object | 辅助信息 | 是 | code为`1100`时存在，扩展辅助信息，详见[auxInfo说明](#queryV4.responseParameters.auxInfo) |
| tokenProfileLabels | json_array | 账号属性标签 | 否 |仅在开启功能时返回，详见[tokenProfileLabels说明](#tokenProfileLabels2)|
| tokenRiskLabels | json_array | 账号风险标签 | 否 |仅在开启功能时返回，详见[tokenRiskLabels说明](#tokenRiskLabels2)|

<span id="queryV4.responseParameters.auxInfo">其中，auxInfo数组中每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范**           |
| ---------- | -------- | ------------ | ------------ | ------------------ |
| frameCount | int      | 辅助信息     | 是           | 视频文件的截帧总数 |
| billingAudioDuration | float | 审核的视频中音频的时长  | 是 |  |
| billingImgNum | int | 审核的视频截帧数量 | 是 |  |
| time       | int      | 辅助信息     | 是           | 视频时长           |

<span id="callbackV2.callbackParameters.query..frameDetail">其中，detail中每个成员的具体内容如下：</span>

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
| logos | json_array | 返回视频帧识别出来的logo结果<br/>如："logos": ["douyin"] | 否 |  |
| businessLabels | json_array | 识别出的业务标签 | 否 |传入imgBusinessType时返回，详见[businessLabels说明](#businessLabels2)   |

<span id="businessLabels2">detail中，businessLabels数组的每个成员的内容如下：</span>

| **参数名**          | **类型**    | **参数说明** | **是否必返** | **规范**                                                 |
| ------------------- | ----------- | ------------ | ------------ | -------------------------------------------------------- |
| businessLabel1      | string      | 一级标签     | 是           | 一级标签                                                 |
| businessLabel2      | string      | 二级标签     | 是           | 二级标签                                                 |
| businessLabel3      | string      | 三级标签     | 是           | 三级标签                                                 |
| businessDescription | string      | 标签描述     | 是           | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| confidenceLevel     | int         | 置信等级     | 是           | 可选值在0～2之间，值越大，可信度越高<br/>                |
| probability         | float       | 置信度       | 是           | 可选值为0~1，值越大，可信度越高                          |
| businessDetail      | Json_object | 详细信息     | 是           |                                                          |

businessLabels中，businessDetail的内容如下：

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

<span id="callbackV2.callbackParameters.query.audioDetail">其中，音频片段addition中每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| audio_evidence | json_array | 音频片段详情数组 | 否 | 当请求参数retallAudio传值为1时<br/>数组中将包含正常内容片段的相关详情，否则只返回非PASS的片段的相关详情，详见[audio_evidence说明](#callbackV2.callbackParameters.query.audioEvidence) |
| subtitleDetail | json_array | 视频字幕存在时返回 | 否 | 视频字幕，详见[subtitleDetail说明](#subtitleDetail2) |
| videoCoverDetail | json_array | 视频封面存在时返回 | 否 |视频封面，详见[videoCoverDetail说明](#videoCoverDetail2)  |

addition中，audio_evidence的每个元素详细内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| audioModel | string | 命中规则编号 | 是 |  |
| audioText | string | 返回音转文文字 | 否 | |
| audio_starttime | float | 音频片段发生时间 | 否 | |
| audio_endtime | float | 音频片段结束时间 | 否 | |
| audio_url | string | 音频片段地址 | 否 | |
| audio_matchedItem | string | 违规音频敏感词内容 | 否 | |
| description | string | 音频片段风险原因描述 | 是 | |
| requestId | string | 音频片段唯一标识 | 是 | 一般用户历史记录查询某一片段内容 |
| riskLevel | string | 处置建议 | 是 | 取值范围：<br/>PASS：正常 <br/>REVIEW：审核 <br/>REJECT：拒绝|
| riskType | int | 风险类型 | 是 | 标识风险类型，可能取值：<br/>0：正常<br/>100：涉政/国歌<br/>110: 暴恐<br/>200：色情<br/>210：辱骂<br/>250：娇喘<br/>260：一号领导人声纹<br/>300：广告<br/>400：灌水<br/>500：无意义<br/>520：未成年人<br/>600 : 违禁<br/>700：其他<br/>720：黑账号<br/>730：黑IP<br/>800：高危账号<br/>900：自定义 |
| riskSource | int  | 音频转译文本的结果 | 是 |风险来源，可能取值：<br/>1000：无风险 <br/>1001：文字 <br/>1002：视觉图片 <br/>1003：音频语音 |
| isSing | int  | 该条音频片段是否唱歌 | 否 | 取值范围：<br/>0:表示没有唱歌，<br/>1:表示唱歌。<br/>仅当type传入值包含SING时返回。 |
| language | json_array | 语种标签与概率值列表 | 否| 详见[language说明](#language2) |
| businessLabels | json_array | 识别出的业务标签 | 否 | 详见[businessLabels说明](#businessLabels3) |

<span id="language2">音频的audio_evidence中，language数组中每一项具体参数如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| label | int | 语种识别类别 | 是 |语种识别类别标识，可能取值：<br/>0:普通话<br/>1:英语<br/>2:粤语<br/>3:藏语<br/>4:蒙语<br/>5:维语<br/>6:朝鲜语<br/>7:其他<br/> |
| probability | int | 对应音色标签可能性大小，取值0-100，数值越高表示概率越大 | 是 | 取值范围[0,100] |

<span id="businessLabels3">audio_evidence中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| confidenceLevel | int | 置信等级 | 是 | 可选值在0～2之间，值越大，可信度越高<br/> |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |
| businessDetail | Json_object | 详细信息 | 是 |  |

<span id="subtitleDetail2">addition中，subtitleDetail的每个元素详细内容如下：</span>

| **参数名**  | **类型** | **参数说明**   | **是否必返** | **规范**                                                     |
| ----------- | -------- | -------------- | ------------ | ------------------------------------------------------------ |
| code        | int      | 返回码         | 是           | 详见[接口响应码列表](#接口响应码列表)                              |
| message     | string   | 返回码详情描述 | 是           |                                                              |
| description | string   | 风险原因       | 是           |                                                              |
| riskLevel   | string   | 处置建议       | 是           | 取值范围：<br/>PASS：正常 <br/>REVIEW：审核 <br/>REJECT：拒绝 |
| requestId   | string   | 字幕唯一标识   | 是           |                                                              |

<span id="videoCoverDetail2">addition中，videoCoverDetail的详细内容同subtitleDetail</span>

<span id="tokenProfileLabels2">其中，tokenProfileLabels数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| label1 | string | 一级标签 | 否 | |
| label2 | string | 二级标签 | 否 | |
| label3 | string | 三级标签 | 否 | |
| description | string | 标签描述 | 否 | |
| timestamp | int | 打标签时间戳 | 否 | 13位Unix时间戳，单位：毫秒 |

<span id="tokenRiskLabels">其中，tokenRiskLabels数组每个成员的具体字段同tokenProfileLabels</span>

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
| AUTOTRADEAPPSLOGO | LOGO - 汽车交易平台类 | 如识别懂车帝、易车、太平洋汽车、爱卡等LOGO |
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
| EXTREMEWEATHER | 极端天气识别 |如水灾、暴雨、沙尘暴、冰等 |

## 接口响应码列表

code请求返回码列表如下：

| **code** | **message**      |
| -------- | ---------------- |
| 1100     | 成功             |
| 1101     | 请求正在处理     |
| 1901     | QPS超限          |
| 1902     | 参数不合法       |
| 1903     | 服务失败         |
| 1907     | 获取视频长度超时 |
| 9100     | 余额不足         |
| 9101     | 无权限操作       |

## 示例

### 上传接口请求示例：

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
        "detectFrequency": 3, # 若要添加高级截帧设置，参见"advancedFrequency"参数说明
        "tokenId": "test",
        "ip":"123.171.34.3",
        "url": "http://oss.xxx.com/static/photo/117608703147396.mp4",
        "retallAudio": 1,
        "retallImg": 1,
    }
    "callbackParam": {
        "passThrough": {
            "passThrough1": "透传字段1",
            "passThrough2": "透传字段2",
            "passThrough3": "透传字段3"
        }
    }
}
```

### 上传接口返回示例：

```json
{
    "code": 1100,
    "message": "成功",
    "requestId": "1639824316368",
    "btId": "1639824316368"
}
```

### 异步回调结果示例：

```json
{"checksum":"fcb7908131854c7bd451194a7d87f0d832bbd724bd2affbb70d75e109ed4c243","result":"{\"code\":1100,\"message\":\"成功\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8\",\"btId\":\"515032468\",\"labels\":\"音频文字:色情：性骚扰：重度性骚扰-音频 正常-图像 \",\"detail\":[{\"description\":\"正常-图像\",\"imgUrl\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v0.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D2d0c13df3ea2235b4fc97dfdeb8ac2b6357f05a6\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_v0\",\"riskLevel\":\"PASS\",\"riskSource\":1000,\"riskType\":0,\"similarity\":0,\"time\":0},{\"description\":\"正常-图像\",\"imgText\":\"0D入\",\"imgUrl\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v5.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D1445219d6ffc3101c66defb8931c5d8e99be1e7c\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_v5\",\"riskLevel\":\"PASS\",\"riskSource\":1000,\"riskType\":0,\"similarity\":0.3828125,\"time\":5},{\"description\":\"正常-图像\",\"imgText\":\"LD\",\"imgUrl\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v10.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3Dd073b20f6472b10f324659c1d99f42a2b2b4ab8d\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_v10\",\"riskLevel\":\"PASS\",\"riskSource\":1000,\"riskType\":0,\"similarity\":0.6640625,\"time\":10},{\"description\":\"正常-图像\",\"imgText\":\"山\",\"imgUrl\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v15.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D02fefdc10611061b450c1ec64e1342442e8206fe\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_v15\",\"riskLevel\":\"PASS\",\"riskSource\":1000,\"riskType\":0,\"similarity\":0.77734375,\"time\":15},{\"description\":\"正常-图像\",\"imgUrl\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v20.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D40829c647570c532d8faab28c36f0898757eaed3\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_v20\",\"riskLevel\":\"PASS\",\"riskSource\":1000,\"riskType\":0,\"similarity\":0.7421875,\"time\":20},{\"description\":\"正常-图像\",\"imgText\":\"#######\",\"imgUrl\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v25.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D64701f589de799dfed252e6a7b86d2db1764b255\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_v25\",\"riskLevel\":\"PASS\",\"riskSource\":1000,\"riskType\":0,\"similarity\":0.796875,\"time\":25},{\"description\":\"正常-图像\",\"imgUrl\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v30.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D97c04340256ec3394baa010f9becdc30d971669d\",\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_v30\",\"riskLevel\":\"PASS\",\"riskSource\":1000,\"riskType\":0,\"similarity\":0.7265625,\"time\":30}],\"auxInfo\":{\"frameCount\":7,\"time\":31,\"billingAudioDuration\":31,\"billingImgNum\":7},\"addition\":{\"audio_evidence\":[{\"audioModel\":\"MA000003002001000\",\"audioText\":\"想你等五百头我还是一样喜欢你，自慰，的\",\"audio_endtime\":30,\"audio_starttime\":20,\"audio_url\":\"http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/audio%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_a0002.wav?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D95d7c542d6546f85d3801f06744991620806f982\",\"description\":\"音频文字:色情：性骚扰：重度性骚扰-音频\",\"isSing\":0,\"language\":[{\"confidence\":0.0314832,\"label\":2},{\"confidence\":99.9603,\"label\":0},{\"confidence\":0.0366747,\"label\":1}],\"requestId\":\"e7a59eebd431415e92684cb6151c4de8_a0002\",\"riskLevel\":\"REJECT\",\"riskSource\":1001,\"riskType\":200}],\"subtitleDetail\":{\"code\":1100,\"description\":\"辱骂:辱骂:辱骂\",\"message\":\"成功\",\"requestId\":\"dbdddcbd4684a10e71bce2f768b9ec9csubtitle\",\"riskLevel\":\"REJECT\"},\"videoCoverDetail\":{\"code\":1100,\"description\":\"色情:色情:色情\",\"message\":\"成功\",\"requestId\":\"dbdddcbd4684a10e71bce2f768b9ec9cvideoCover\",\"riskLevel\":\"REJECT\"}},\"riskLevel\":\"REJECT\"}"}
```

### 查询接口结果示例：

```json

{
    "code": 1100,
    "message": "成功",
    "requestId": "25321f666ac62c57e2d59ac47e815350",
    "btId": "515032468",
    "labels": "音频文字:色情：性骚扰：重度性骚扰-音频 正常-图像 ",
    "auxInfo":{
        "frameCount":7,
        "billingAudioDuration":31,
        "billingImgNum":7,
        "time":31
    },
    "detail": [
        {
            "description": "正常-图像",
            "imgUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v0.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D2d0c13df3ea2235b4fc97dfdeb8ac2b6357f05a6",
            "requestId": "e7a59eebd431415e92684cb6151c4de8_v0",
            "riskLevel": "PASS",
            "riskSource": 1000,
            "riskType": 0,
            "similarity": 0,
            "time": 0
        },
        {
            "description": "正常-图像",
            "imgText": "0D入",
            "imgUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v5.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D1445219d6ffc3101c66defb8931c5d8e99be1e7c",
            "requestId": "e7a59eebd431415e92684cb6151c4de8_v5",
            "riskLevel": "PASS",
            "riskSource": 1000,
            "riskType": 0,
            "similarity": 0.3828125,
            "time": 5
        },
        {
            "description": "正常-图像",
            "imgText": "LD",
            "imgUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v10.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3Dd073b20f6472b10f324659c1d99f42a2b2b4ab8d",
            "requestId": "e7a59eebd431415e92684cb6151c4de8_v10",
            "riskLevel": "PASS",
            "riskSource": 1000,
            "riskType": 0,
            "similarity": 0.6640625,
            "time": 10
        },
        {
            "description": "正常-图像",
            "imgText": "山",
            "imgUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v15.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D02fefdc10611061b450c1ec64e1342442e8206fe",
            "requestId": "e7a59eebd431415e92684cb6151c4de8_v15",
            "riskLevel": "PASS",
            "riskSource": 1000,
            "riskType": 0,
            "similarity": 0.77734375,
            "time": 15
        },
        {
            "description": "正常-图像",
            "imgUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v20.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D40829c647570c532d8faab28c36f0898757eaed3",
            "requestId": "e7a59eebd431415e92684cb6151c4de8_v20",
            "riskLevel": "PASS",
            "riskSource": 1000,
            "riskType": 0,
            "similarity": 0.7421875,
            "time": 20
        },
        {
            "description": "正常-图像",
            "imgText": "#######",
            "imgUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v25.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D64701f589de799dfed252e6a7b86d2db1764b255",
            "requestId": "e7a59eebd431415e92684cb6151c4de8_v25",
            "riskLevel": "PASS",
            "riskSource": 1000,
            "riskType": 0,
            "similarity": 0.796875,
            "time": 25
        },
        {
            "description": "正常-图像",
            "imgUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/image%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_v30.jpg?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452139%3B1644044139%26q-key-time%3D1641452139%3B1644044139%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D97c04340256ec3394baa010f9becdc30d971669d",
            "requestId": "e7a59eebd431415e92684cb6151c4de8_v30",
            "riskLevel": "PASS",
            "riskSource": 1000,
            "riskType": 0,
            "similarity": 0.7265625,
            "time": 30
        }
    ],
    "addition": {
        "audio_evidence": [
            {
                "audioModel": "MA000003002001000",
                "audioText": "想你等五百头我还是一样喜欢你，自慰，的",
                "audio_endtime": 30,
                "audio_starttime": 20,
                "audio_url": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/audio%2F20220106%2Fe7a59eebd431415e92684cb6151c4de8_a0002.wav?sign=q-sign-algorithm%3Dsha1%26q-ak%3DAKIDcCe4LVKKzUvBIEtb2NZbS8lGblkbmoFW%26q-sign-time%3D1641452138%3B1644044138%26q-key-time%3D1641452138%3B1644044138%26q-header-list%3D%26q-url-param-list%3D%26q-signature%3D95d7c542d6546f85d3801f06744991620806f982",
                "description": "音频文字:色情：性骚扰：重度性骚扰-音频",
                "isSing": 0,
                "language": [
                    {
                        "confidence": 0.0314832,
                        "label": 2
                    },
                    {
                        "confidence": 99.9603,
                        "label": 0
                    },
                    {
                        "confidence": 0.0366747,
                        "label": 1
                    }
                ],
                "requestId": "e7a59eebd431415e92684cb6151c4de8_a0002",
                "riskLevel": "REJECT",
                "riskSource": 1001,
                "riskType": 200
            }
        ],
        "subtitleDetail": {
            "code": 1100,
            "description": "辱骂:辱骂:辱骂",
            "message": "成功",
            "requestId": "dbdddcbd4684a10e71bce2f768b9ec9csubtitle",
            "riskLevel": "REJECT"
        },
        "videoCoverDetail": {
            "code": 1100,
            "description": "色情:色情:色情",
            "message": "成功",
            "requestId": "dbdddcbd4684a10e71bce2f768b9ec9cvideoCover",
            "riskLevel": "REJECT"
        }
    },
    "riskLevel": "REJECT"
}

```
