# 数美智能视频文件识别产品API文档

## 视频文件上传请求

### 接口描述

该接口用于提交视频相关信息，自定义截帧频率等参数。识别结果需客户自行定期调用查询接口获取。

### 请求URL：
| 集群 | URL |
| --- | --- |
| 北京 | `http://api-video-bj.fengkongcloud.com/video/v4` |
| 上海 | `http://api-video-sh.fengkongcloud.com/video/v4` |
| 新加坡 | `http://api-video-xjp.fengkongcloud.com/video/v4` |
| 硅谷 | `http://api-video-gg.fengkongcloud.com/video/v4` |

### 请求方法：

`POST`

### 字符编码：

`UTF-8`

### 建议超时时间：

7s

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
| appId | string | 应用标识 | 必传参数 | 用于区分应用，需要联系数美服务开通，请使用数美单独提供的传值为准 |
| eventId | string | 事件标识 | 必传参数 | 用于区分场景数据，需要联系数美服务开通，请使用数美单独提供的传值为准 |
| imgType | string | 视频中的画面需要识别的监管类型，**和imgBusinessType至少传一个** | 非必传参数 | 监管一级标签<br/>可选值：<br/>`POLITY`：涉政识别<br/>`EROTIC`：色情&性感违规识别<br/>`VIOLENT`：暴恐&违禁识别<br/>`QRCODE`：二维码识别<br/>`ADVERT`：广告识别<br/>`IMGTEXTRISK`：图片文字违规识别<br/>如果需要识别多个功能，通过下划线连接，如`POLITY_QRCODE_ADVERT`用于涉政、二维码和广告组合识别 |
| audioType | string | 视频中的音频需要识别的监管类型,**和audioBusinessType至少传一个** | 非必传参数 | 监管一级标签<br/>可选值：<br/>`POLITY`：涉政识别<br/>`EROTIC`：色情识别<br/>`ADVERT`：广告识别<br/>`DIRTY`: 辱骂识别<br/>`ADLAW`:广告法<br/>`MOAN`：娇喘识别<br/>`AUDIOPOLITICAL`：声音涉政<br/>`ANTHEN`：国歌识别<br/>`NONE`:不检测音频<br/>如需做组合识别，通过下划线连接即可，例如`POLITY_EROTIC`用于涉政和色情识别 |
| imgBusinessType | string | 视频中的画面需要识别的业务类型， **和imgType至少传一个** | 非必传参数 | 可选值参考[imgBusinessType可选值列表](#imgbusinesstype可选值列表)<br/>如果需要识别多个功能，通过下划线连接 |
| audioBusinessType | String | 视频中的音频业务识别类型， **和audioType至少传一个** | 非必传参数 | 业务一级标签<br/>可选值：<br/>`SING`：唱歌识别<br/>`LANGUAGE`：语种识别（中文、英文、粤语、藏语、维吾尔语、朝鲜语、蒙语、其他）<br/>`MINOR`：未成年人识别<br/>`GENDER`：性别识别<br/>`TIMBRE`：音色识别，需要同时传入`GENDER`才能生效<br/>如果需要识别多个功能，通过下划线连接 |
| callback | string | 指定回调url地址 | 非必传参数 | 当该字段非空时，服务将根据该字段回调通知用户审核结果（支持`http`/`https`） |
| data | json\_object | 本次请求相关信息，最长1MB | 必传参数 | 最长1MB，其中[data内容如下](#data) |

<span id="data">其中，data的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| btId | string | 视频唯一标识 | 必传参数 | 视频唯一标识，用于查询识别结果，最长64位 |
| url | string | 要检测的视频url地址 | 必传参数 | |
| tokenId | string | | 必传参数 | 客户端用户账号唯一标识，用于用户行为分析，建议传入用户UID； 最长40位 |
| lang | string | 语言类型 | 非必传参数 | 可选值：<br/>zh ：中文<br/>en ：英文<br/>ar ：阿拉伯语<br/>不传默认进行中文检测 |
| detectFrequency | int | 截帧频率间隔，单位为秒 | 非必传参数 | 取值范围为1~60s；如不传递默认5s截帧一次 |
| advancedFrequency | json_object | 高级截帧间隔，单位为秒 | 非必传参数 | 高级截帧设置，此项填写，默认截帧策略失效<br/>参数配置如下<br/>{"durationPoints":[300,600],"frequencies":[1,5,10]}<br/>含义为：<br/>视频文件时长≤300s ——选用1s一截帧<br/>300s<视频文件时长≤600s ——选用5s一截帧<br/>视频文件时长>600s ——选用10s一截帧 |
| ip | string | 客户端IP | 非必传参数 | 用于IP维度的用户行为分析，同时可用于比对数美IP黑库,支持ipv4和ipv6的传入 |
| audioDetectStep | int | 视频文件中的音频审核步长 | 非必传参数 | 单位为个，取值范围为1-36整数，取1表示跳过一个10S的音频片段审核，取2表示跳过二个，以此类推。不使用该功能时音频内容全部过审 |
| returnAllImg | int | | 非必传参数 | 选择返回视频截帧图片的等级：0：返回风险等级为非pass的图片；1：返回所有风险等级的图片。默认为0 |
| returnAllAudio | int | | 非必传参数 | 选择返回视频音频片段的等级：0：返回风险等级为非pass的音频片段1：返回所有风险等级的音频片段默认为0 |
| videoTitle | string | 视频名称 | 非必传参数 | 视频名称，用于后台界面展示 |
| extra | json_object | 扩展信息 | 非必传参数 | 详见[extra说明](#extra) |
| dataId | string | 数据标识 | 非必传参数 |  |

<span id="advancedFrequency">data 中，advancedFrequency的内容如下</span>

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| durationPoints | int_array | 视频时长区间分割 | 非必传参数 | 用于规定视频文件支持动态截帧频率的时长区间，数组最多为5个 |
| frequencies | int_array | 视频时长区间对应的截帧频率 | 非必传参数 | 可设置范围为1~60秒，数组最多6个<br/>说明：frequencies数组设置的个数需要比durationPoints数组个数多1个，传错或传空报错返回1902 |

<span id="extra">data 中，extra的内容如下</span>

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| passThrough | json\_object | 透传字段 | 非必传参数 | 该字段内容会随着回调结果一起原样返回 |

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
| code | int | 返回码 | 是 | 详见[接口响应码列表](#接口响应码列表) |
| message | string | 返回码详情描述 | 是 | |
| requestId | string | 请求唯一标识 | 是 | |
| btId | string | 视频唯一标识 | 是 | 最长64位 |
| riskLevel | string | 风险级别，code为1100时存在 | 否 | 返回值：<br/>`PASS`：正常内容，建议直接放行<br/>`REVIEW`：可疑内容，建议人工审核<br/>`REJECT`：违规内容，建议直接拦截 |
| frameDetail | json_array | 风险详情 | 否 | 有风险片段或returnAllImg=1时返回，详见[frameDetail说明](#frameDetail) |
| audioDetail | json_array | 音频片段信息 | 否 |有风险片段或returnAllAudio=1时返回，详见[audioDetail说明](#audioDetail)|
| auxInfo | json_object | 辅助信息 | 否 |code为`1100`时存在，详见[auxInfo说明](#auxInfo)|
| tokenProfileLabels | json_array | 账号属性标签 | 否 |仅在开启功能时返回，详见[tokenProfileLabels说明](#tokenProfileLabels)|
| tokenRiskLabels | json_array | 账号风险标签 | 否 |仅在开启功能时返回，详见[tokenRiskLabels说明](#tokenRiskLabels)|
<span id="auxInfo">其中，auxInfo中的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| frameCount | int | 返回的视频截帧数量。returnAllImg=0时为风险数量，returnAllImg=1时为全部数量 | 是 |  |
| billingAudioDuration | float | 审核的视频中音频的时长 | 是 |  |
| billingImgNum | int | 审核的视频截帧数量 | 是 |  |
| time | int | 视频时长 | 是 |  |
| passThrough | json_object | 透传字段，该字段内容与请求参数data中extra的passThrough的值相同 | 否 |  |

<span id="frameDetail">其中，frameDetail数组中每个成员的具体内容如下：</span>

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
| riskDetail | json_object | 风险详情信息 | 是 | 详见[riskDetail说明](#riskDetail) |
| allLabels | json_array | 全部的风险标签列表 | 是 | 全部的风险标签列表，详见[allLabels说明](#allLabels) |
| businessLabels | json_array | 业务标签列表 | 否 | 传入imgBusinessType时返回，详见[businessLabels说明](#businessLabels) |
| auxInfo | json_object | 辅助信息 | 是 | 一些辅助信息放在这里，详见[auxInfo说明](#auxInfo3) |

<span id="auxInfo3">frameDetail中，auxInfo的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| qrContent | string | 截帧图片二维码链接识别 | 否 | 截帧图片二维码链接识别，如有需要可联系数美开启<br/>注意：开启该功能后，只有完整，可以正常识别到的二维码才会返回且imgType传值需要包含`AD` |
| similarity | float | 当前截帧图片和上一帧截帧图片的相似度 | 是 | 有图片则该字段就会返回，视频文件初始第一帧将比对纯黑背景图片 |

<span id="riskDetail">frameDetail中，riskDetail的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskSource | int | 风险来源 | 是 | 可选值：<br/>`1000`：无风险<br/>`1001`：文本风险<br/>`1002`：视觉风险<br/>`1003`：音频风险 |
| faces | json_array | 人脸信息 | 否 | 返回图片中涉政人物的名称及位置信息，详见[faces说明](#faces) |
| face_num | int | 人脸数量 | 否 |  |
| objects | json_array | 物品信息 | 否 | 返回图片中标识或物品的名称及位置信息，详见[objects说明](#objects) |
| persons | json_array | 人像信息 | 否 |  |
| person_num | int | 人像数量 | 否 |  |
| ocrText | json_object | 文字信息 | 否 | 返回图片中文字相关信息，详见[ocrText说明](#ocrText) |

<span id="faces">riskDetail中，faces数组的每个元素的内容如下：</span>

| **参数名**  | **类型**  | **参数说明** | **是否必返** | **规范**                                                     |
| ----------- | --------- | ------------ | ------------ | ------------------------------------------------------------ |
| id          | string    | 编号         | 是           |                                                              |
| name        | string    | 人名         | 否           |                                                              |
| location    | int_array | 位置坐标     | 否           | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 |
| probability | float     | 置信度       | 否           | 可选值在0～1之间，值越大，可信度越高                         |
| face_ratio  | float     | 人脸占比     | 否           |                                                              |

<span id="objects">riskDetail中，objects数组的每个元素的内容如下：</span>

| **参数名**  | **类型**  | **参数说明** | **是否必返** | **规范**                                                     |
| ----------- | --------- | ------------ | ------------ | ------------------------------------------------------------ |
| id          | string    | 编号         | 是           |                                                              |
| name        | string    | 名称         | 否           |                                                              |
| location    | int_array | 位置坐标     | 否           | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 |
| probability | float     | 置信度       | 否           | 可选值在0～1之间，值越大，可信度越高                         |
| qrContent   | string    | 二维码信息   | 否           |                                                              |

riskDetail中，persons数组的每个元素的内容如下：

| **参数名**   | **类型**  | **参数说明** | **是否必返** | **规范**                                                     |
| ------------ | --------- | ------------ | ------------ | ------------------------------------------------------------ |
| id           | string    | 编号         | 是           |                                                              |
| location     | int_array | 位置坐标     | 否           | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 |
| probability  | float     | 置信度       | 否           | 可选值在0～1之间，值越大，可信度越高                         |
| person_ratio | float     | 人像占比     | 否           |                                                              |

<span id="ocrText">riskDetail中，ocrText数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| text | string | 图片中识别出的文字 | 否 | 识别出来所有文字内容 |
| matchedLists | json_array | 命中的客户自定义名单信息 | 否 | 仅在命中客户自定义名单时返回，详见[matchedLists说明](#matchedLists) |
| riskSegments | json_array | 高风险内容片段 | 否 | 在启用涉政、暴恐、违禁、广告等功能时存在，详见[riskSegments说明](#riskSegments) |

<span id="matchedLists">ocrText中，matchedLists数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 客户自定义名单名称 | 否 | |
| words | json_array | 命中的这个名单中的敏感词信息 | 否 | 下标从0开始计数，详见[words说明](#words) |

<span id="words">matchedLists中，words数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string | 敏感词 | 否 | |
| position | int_array | 敏感词所在位置 | 否 | 下标从0开始计数 |

ocrText中，riskSegments每个成员的具体内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segment | string | 高风险内容片段 | 否 | |
| position | int_array | 高风险内容片段所在位置 | 否 | 下标从0开始计数 |

<span id="allLabels">frameDetail中，allLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险描述 | 是 | 格式为&quot;一级风险标签：二级风险标签：三级风险标签&quot;的中文名称<br/>对于命中用户自定义名单时返回：`命中自定义名单` |
| riskLevel | string | 处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |
| riskDetail | json_object | 风险详情 | 是 | 同frameDetail中的riskDetail结构一致 |

<span id="businessLabels">frameDetail中，businessLabels数组的每个成员的内容如下：</span>

| **参数名**          | **类型**    | **参数说明** | **是否必返** | **规范**                                                 |
| ------------------- | ----------- | ------------ | ------------ | -------------------------------------------------------- |
| businessLabel1      | string      | 一级标签     | 是           | 一级标签                                                 |
| businessLabel2      | string      | 二级标签     | 是           | 二级标签                                                 |
| businessLabel3      | string      | 三级标签     | 是           | 三级标签                                                 |
| businessDescription | string      | 标签描述     | 是           | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| confidenceLevel     | int         | 置信等级     | 是           | 可选值在0～2之间，值越大，可信度越高                     |
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

<span id="audioDetail">其中，audioDetail数组中每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 请求唯一标识 | 是 | |
| audioStarttime | float | 音频片段发生时间 | 是 | |
| audioEndtime | float | 音频片段结束时间 | 是 | |
| audioUrl | string | 音频片段地址 | 是 | |
| audioText | string | 音转文文字 | 否 | 识别出文本会返回 |
| riskLevel | string | 当前事件的处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| riskLabel1 | string | 各个一级标签之间是并列的关系，riskLevel为`PASS`时返回`normal` | 是 | 一级标签 |
| riskLabel2 | string | 二级标签归属于一级标签，当riskLevel为`PASS`时为空 | 是 | 二级标签 |
| riskLabel3 | string | 三级标签归属于二级标签，当riskLevel为`PASS`时为空 | 是 | 三级标签 |
| riskDescription | string | 标签解释 | 是 | 格式为&quot;一级风险标签：二级风险标签：三级风险标签&quot;的中文名称<br/>对于命中用户自定义名单时返回：`命中自定义名单` |
| riskDetail | json_object | 风险详情信息 | 是 | 详见[riskDetail说明](#riskDetail2) |
| allLabels | json_array | 全部的风险标签列表 | 是 | 全部的风险标签列表，详见[allLabels说明](#allLabels2) |
| businessLabels | json_array | 业务标签列表 | 否 | 传入audioBusinessType时会返回，详见[businessLabels说明](#businessLabels2) |

<span id="riskDetail2">audioDetail中，riskDetail的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskSource | int | 风险来源 | 是 | 风险来源，可选值：<br/>`1000`：无风险<br/>`1001`：文本风险<br/>`1002`：视觉风险<br/>`1003`：音频风险 |
| audioText | string | 音频转译文本的结果 | 否 | |
| matchedLists | json_array | 命中的客户自定义名单信息 | 否 | 命中客户自定义名单时返回，其他时不存在，详见[matchedLists说明](#matchedLists2) |
| riskSegments | json_array | 高风险内容片段 | 否 | 在涉政、暴恐、违禁、竞品、广告法等功能的时候存在，详见[riskSegments说明](#riskSegments2) |

<span id="matchedLists2">riskDetail中，matchedLists的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 客户自定义名单名 | 否 | |
| words | json_array | 命中的这个名单中的敏感词信息 | 否 | 下标从0开始计数，详见[words说明](#words2) |

<span id="words2">matchedLists中，words的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string | 敏感词 | 否 | |
| position | int_array | 敏感词所在位置 | 否 | 下标从0开始计数 |

<span id="riskSegments2">riskDetail中，riskSegments的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segment | string | 高风险内容片段 | 否 | |
| position | int_array | 高风险内容片段所在位置 | 否 | 下标从0开始计数 |

<span id="allLabels2">audioDetail中，allLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险描述 | 是 | 格式为&quot;一级风险标签：二级风险标签：三级风险标签&quot;的中文名称<br/>对于命中用户自定义名单时返回：`命中自定义名单` |
| riskLevel | string | 处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |
| riskDetail | json_object | 风险详情 | 是 | 同audioDetail中的riskDetail结构一致 |

<span id="businessLabels2">audioDetail中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| confidenceLevel | int | 置信等级 | 是 | 可选值在0～2之间，值越大，可信度越高<br/> |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |
| businessDetail | Json_object | 详细信息 | 是 |  |

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

该接口用于客户主动查询视频文件识别结果，建议每30s进行一次查询, 支持近三天结果的查询。

### 请求URL：
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-video-bj.fengkongcloud.com/video/query/v4` | 中文视频文件 |
| 上海 | `http://api-video-sh.fengkongcloud.com/video/query/v4` | 中文视频文件 |
| 新加坡 | `http://api-video-xjp.fengkongcloud.com/video/query/v4` | 中文视频文件 |
| 硅谷 | `http://api-video-gg.fengkongcloud.com/video/query/v4` | 中文视频文件 |
| 印度 | `http://api-video-yd.fengkongcloud.com/video/query/v4` | 中文视频文件 |

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
| riskLevel | string | 风险级别，code为1100时存在 | 否 | 可能返回值：<br/>`PASS`：正常内容，建议直接放行<br/>`REVIEW`：可疑内容，建议人工审核<br/>`REJECT`：违规内容，建议直接拦截 |
| frameDetail | json_array | 风险详情 | 否 | code为`1100`时存在，详见[frameDetail](#frameDetail2) |
| audioDetail | json_array | 音频片段信息 | 否 | code为`1100`时存在，详见[audioDetail](#audioDetail2) |
| auxInfo | json_object | 辅助信息 | 是 | code为`1100`时存在，扩展辅助信息，详见[auxInfo说明](#auxInfo2) |
| tokenProfileLabels | json_array | 账号属性标签 | 否 | 仅在开启功能时返回，详见[tokenProfileLabels说明](#tokenProfileLabels2) |
| tokenRiskLabels | json_array | 账号风险标签 | 否 | 仅在开启功能时返回，详见[tokenRiskLabels说明](#tokenRiskLabels2) |

<span id="auxInfo2">其中，auxInfo数组中每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| frameCount | int | 辅助信息 | 是 | 视频文件的截帧总数 |
| billingAudioDuration | float | 审核的视频中音频的时长 | 是 |  |
| billingImgNum | int | 审核的视频截帧数量 | 是 |  |
| time | int | 辅助信息 | 是 | 视频时长 |

<span id="frameDetail2">其中，frameDetail数组中每个成员的具体内容如下：</span>

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
| riskDetail | json_object | 风险详情信息 | 是 | 风险详情，详见[riskDetail说明](#riskDetail3) |
| allLabels | json_array | 全部的风险标签列表 | 是 | 全部的风险标签列表，详见[allLabels说明](#allLabels3) |
| businessLabels | json_array | 业务标签列表 | 否 | 传入imgBusinessType时返回，详见[businessLabels说明](#businessLabels3) |
| auxInfo | json_object | 辅助信息 | 是 | 一些辅助信息放在这里，详见[auxInfo说明](#auxInfo4) |

<span id="auxInfo4">frameDetail中，auxInfo的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| qrContent | string | 截帧图片二维码链接识别 | 否 | 截帧图片二维码链接识别，如有需要可联系数美开启<br/>注意：开启该功能后，只有完整，可以正常识别到的二维码才会返回且imgType传值需要包含`AD` |
| similarity | float | 当前截帧图片和上一帧截帧图片的相似度 | 是 | 有图片则该字段就会返回，视频文件初始第一帧将比对纯黑背景图片 |

<span id="riskDetail3">frameDetail中，riskDetail的内容如下：</span>

| **参数名** | **类型**    | **参数说明** | **是否必返** | **规范**                                                     |
| ---------- | ----------- | ------------ | ------------ | ------------------------------------------------------------ |
| riskSource | int         | 风险来源     | 是           | 可选值：<br/>`1000`：无风险<br/>`1001`：文本风险<br/>`1002`：视觉风险<br/>`1003`：音频风险 |
| faces      | json_array  | 人脸信息     | 否           | 返回图片中涉政人物的名称及位置信息，详见[faces说明](#faces2) |
| face_num   | int         | 人脸数量     | 否           |                                                              |
| objects    | json_array  | 物品信息     | 否           | 返回图片中标识或物品的名称及位置信息，详见[objects说明](#objects2) |
| persons    | json_array  | 人像信息     | 否           |                                                              |
| person_num | int         | 人像数量     | 否           |                                                              |
| ocrText    | json_object | 文字信息     | 否           | 返回图片中文字相关信息，详见[ocrText说明](#ocrText2) |

<span id="faces2">riskDetail中，faces数组的每个元素的内容如下：</span>

| **参数名**  | **类型**  | **参数说明** | **是否必返** | **规范**                                                     |
| ----------- | --------- | ------------ | ------------ | ------------------------------------------------------------ |
| id          | string    | 编号         | 是           |                                                              |
| name        | string    | 人名         | 否           |                                                              |
| location    | int_array | 位置坐标     | 否           | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 |
| probability | float     | 置信度       | 否           | 可选值在0～1之间，值越大，可信度越高<br/>                    |
| face_ratio  | float     | 人脸占比     | 否           |                                                              |

<span id="objects2">riskDetail中，objects数组的每个元素的内容如下：</span>

| **参数名**  | **类型**  | **参数说明** | **是否必返** | **规范**                                                     |
| ----------- | --------- | ------------ | ------------ | ------------------------------------------------------------ |
| id          | string    | 编号         | 是           |                                                              |
| name        | string    | 名称         | 否           |                                                              |
| location    | int_array | 位置坐标     | 否           | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 |
| probability | float     | 置信度       | 否           | 可选值在0～1之间，值越大，可信度越高<br/>                    |
| qrContent   | string    | 二维码信息   | 否           |                                                              |

riskDetail中，persons数组的每个元素的内容如下：

| **参数名**   | **类型**  | **参数说明** | **是否必返** | **规范**                                                     |
| ------------ | --------- | ------------ | ------------ | ------------------------------------------------------------ |
| id           | string    | 编号         | 是           |                                                              |
| location     | int_array | 位置坐标     | 否           | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 |
| probability  | float     | 置信度       | 否           | 可选值在0～1之间，值越大，可信度越高<br/>                    |
| person_ratio | float     | 人像占比     | 否           |                                                              |

<span id="ocrText2">riskDetail中，ocrText数组每个成员的具体内容如下：</span>

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

<span id="allLabels3">frameDetail，allLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险描述 | 是 | 格式为&quot;一级风险标签：二级风险标签：三级风险标签&quot;的中文名称<br/>对于命中用户自定义名单时返回：`命中自定义名单` |
| riskLevel | string | 处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |
| riskDetail | json_object | 风险详情 | 是 | 同frameDetail中的riskDetail结构一致 |

<span id="businessLabels3">frameDetail中，businessLabels数组的每个成员的内容如下：</span>

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
| person_ratio | float     | 人脸占比     | 否           |                                                              |

<span id="audioDetail2">其中，audioDetail数组中每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 请求唯一标识 | 是 | |
| audioStarttime | float | 音频片段发生时间 | 是 | |
| audioEndtime | float | 音频片段结束时间 | 是 | |
| audioUrl | string | 音频片段地址 | 是 | |
| audioText | string | 音转文文字 | 否 | 识别出文本会返回 |
| riskLevel | string | 当前事件的处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| riskLabel1 | string | 各个一级标签之间是并列的关系，riskLevel为`PASS`时返回`normal` | 是 | 一级标签 |
| riskLabel2 | string | 二级标签归属于一级标签，当riskLevel为`PASS`时为空 | 是 | 二级标签 |
| riskLabel3 | string | 三级标签归属于二级标签，当riskLevel为`PASS`时为空 | 是 | 三级标签 |
| riskDescription | string | 标签解释 | 是 | 格式为&quot;一级风险标签：二级风险标签：三级风险标签&quot;的中文名称<br/>对于命中用户自定义名单时返回：`命中自定义名单` |
| riskDetail | json_object | 风险详情信息 | 是 | 风险详情，详见[riskDetail说明](#riskDetail4) |
| allLabels | json_array | 全部的风险标签列表 | 是 | 全部的风险标签列表，详见[allLabels说明](#allLabels4) |
| businessLabels | json_array | 业务标签列表 | 否 | 传入audioBusinessType时会有，详见[businessLabels说明](#businessLabels4) |

<span id="riskDetail4">audioDetail中，riskDetail的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskSource | int | 风险来源 | 是 | 风险来源，可选值：<br/>`1000`：无风险<br/>`1001`：文本风险<br/>`1002`：视觉风险<br/>`1003`：音频风险 |
| audioText | string | 音频转译文本的结果 | 否 | |
| matchedLists | json_array | 命中的客户自定义名单信息 | 否 | 命中客户自定义名单时返回，其他时不存在，详见[matchedLists说明](#matchedLists3) |
| riskSegments | json_array | 高风险内容片段 | 否 | 在涉政、暴恐、违禁、竞品、广告法等功能的时候存在，详见[riskSegments说明](#riskSegments3) |

<span id="matchedLists3">riskDetail中，matchedLists数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 客户自定义名单名 | 否 | |
| words | json_array | 命中的这个名单中的敏感词信息 | 否 | 下标从0开始计数，详见[words说明](#words3) |

<span id="words3">matchedLists中，words的每个元素详细内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string | 敏感词 | 否 | |
| position | int_array | 敏感词所在位置 | 否 | 下标从0开始计数 |

<span id="riskSegments3">riskDetail中，riskSegments数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segment | string | 高风险内容片段 | 否 | |
| position | int_array | 高风险内容片段所在位置 | 否 | 下标从0开始计数 |

<span id="allLabels4">audioDetail中，allLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险描述 | 是 | 格式为&quot;一级风险标签：二级风险标签：三级风险标签&quot;的中文名称<br/>对于命中用户自定义名单时返回：`命中自定义名单` |
| riskLevel | string | 处置建议 | 是 | `PASS`：正常内容<br/>`REVIEW`：可疑内容<br/>`REJECT`：违规内容 |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |
| riskDetail | json_object | 风险详情 | 是 | 同audioDetail中的riskDetail结构一致 |

<span id="businessLabels4">audioDetail中，businessLabels数组的每个成员的内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级标签 | 是 | 一级标签 |
| businessLabel2 | string | 二级标签 | 是 | 二级标签 |
| businessLabel3 | string | 三级标签 | 是 | 三级标签 |
| businessDescription | string | 标签描述 | 是 | 格式为&quot;一级标签：二级标签：三级标签&quot;的中文名称 |
| confidenceLevel | int | 置信等级 | 是 | 可选值在0～2之间，值越大，可信度越高<br/> |
| probability | float | 置信度 | 是 | 可选值为0~1，值越大，可信度越高 |
| businessDetail | Json_object | 详细信息 | 是 |  |

<span id="tokenProfileLabels2">其中，tokenProfileLabels数组每个成员的具体内容如下：</span>

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| label1 | string | 一级标签 | 否 | |
| label2 | string | 二级标签 | 否 | |
| label3 | string | 三级标签 | 否 | |
| description | string | 标签描述 | 否 | |
| timestamp | int | 打标签时间戳 | 否 | 13位Unix时间戳，单位：毫秒 |

<span id="tokenRiskLabels2">其中，tokenRiskLabels数组每个成员的具体字段同tokenProfileLabels</span>

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
| 1905     | 识别内容不规范   |
| 9100     | 余额不足         |
| 9101     | 无权限操作       |

## 示例

### 上传接口请求示例：

```json
{
    "accessKey": "**********",
    "appId": "default",
    "eventId": "video",
    "imgType": "POLITY_EROTIC_ADVERT",
    "imgBusinessType": "BODY_FOOD_3CPRODUCTSLOGO",
    "audioType": "POLITY_EROTIC_ADVERT_MOAN",
    "audioBusinessType": "SING_LANGUAGE",
    "callback": "http://www.xxx.top/xxx",
    "data": {
        "btId": "1639824316368",
        "channel": "video",
        "detectFrequency": 3,
	"advancedFrequency": {"durationPoints":[300,600],"frequencies":[1,5,10]},
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
{
	"code": 1100,
	"message": "成功",
	"requestId": "66fb85e3149bb9e13d6c72161cc6c6cf",
	"btId": "1666684506188",
	"frameDetail": [
		{
			"allLabels": [
				{
					"probability": 0.665125370025635,
					"riskDescription": "涉政:政治象征:党徽",
					"riskDetail": {
						"ocrText": {
							"text": "2022/101/25 09:05"
						},
						"riskSource": 1002
					},
					"riskLabel1": "politics",
					"riskLabel2": "zhengzhixiangzheng",
					"riskLabel3": "danghui",
					"riskLevel": "REJECT"
				}
			],
			"auxInfo": {
				"similarity": 0.4765625
			},
			"businessLabels": [
				{
					"businessDescription": "人脸:人脸姿态:正脸",
					"businessDetail": {},
					"businessLabel1": "face",
					"businessLabel2": "renlianzitai",
					"businessLabel3": "zhenglian",
					"confidenceLevel": 1,
					"probability": 0.450656906102068
				},
				{
					"businessDescription": "人脸:人脸类型:真人",
					"businessDetail": {
						"face_num": 1,
						"faces": [
							{
								"face_ratio": 0.00227673095650971,
								"id": "f7bf8842f80a5a2192781064bd69e776",
								"location": [
									352,
									237,
									381,
									278
								],
								"name": "郭荣铿",
								"probability": 0.499512671029603
							}
						]
					},
					"businessLabel1": "face",
					"businessLabel2": "renlianleixing",
					"businessLabel3": "zhenren",
					"confidenceLevel": 2,
					"probability": 0.979977369308472
				}
			],
			"imgText": "2022/101/25 09:05",
			"imgUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/POST_VIDEO%2FPOST_VIDEO_IMG%2F20221025%2Fedaa113581ec1c18df7b44c86d36ae3b_v81.jpg?q-sign-algorithm=sha1&q-ak=AKIDg9LHyOYSAcmfHekZ6NN6XidHflbASUHn&q-sign-time=1666684548%3B1669276548&q-key-time=1666684548%3B1669276548&q-header-list=host&q-url-param-list=&q-signature=d7692c37694f1219092cbd3d7364481ab690d62e",
			"requestId": "edaa113581ec1c18df7b44c86d36ae3b_v81",
			"riskDescription": "涉政:政治象征:党徽",
			"riskDetail": {
				"ocrText": {
					"text": "2022/101/25 09:05"
				},
				"riskSource": 1002
			},
			"riskLabel1": "politics",
			"riskLabel2": "zhengzhixiangzheng",
			"riskLabel3": "danghui",
			"riskLevel": "REJECT",
			"time": 81
		},
		{
			"allLabels": [
				{
					"probability": 0.553634166717529,
					"riskDescription": "涉政:政治象征:党徽",
					"riskDetail": {
						"ocrText": {
							"text": "新器 20210/2509:05"
						},
						"riskSource": 1002
					},
					"riskLabel1": "politics",
					"riskLabel2": "zhengzhixiangzheng",
					"riskLabel3": "danghui",
					"riskLevel": "REJECT"
				}
			],
			"auxInfo": {
				"similarity": 0.95703125
			},
			"businessLabels": [
				{
					"businessDescription": "人脸:人脸类型:无人脸",
					"businessDetail": {},
					"businessLabel1": "face",
					"businessLabel2": "renlianleixing",
					"businessLabel3": "wurenlian",
					"confidenceLevel": 1,
					"probability": 0.457959338261496
				}
			],
			"imgText": "新器 20210/2509:05",
			"imgUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/POST_VIDEO%2FPOST_VIDEO_IMG%2F20221025%2Fedaa113581ec1c18df7b44c86d36ae3b_v82.jpg?q-sign-algorithm=sha1&q-ak=AKIDg9LHyOYSAcmfHekZ6NN6XidHflbASUHn&q-sign-time=1666684549%3B1669276549&q-key-time=1666684549%3B1669276549&q-header-list=host&q-url-param-list=&q-signature=2606d67861e62622926d9d7f10037d70f068ceb5",
			"requestId": "edaa113581ec1c18df7b44c86d36ae3b_v82",
			"riskDescription": "涉政:政治象征:党徽",
			"riskDetail": {
				"ocrText": {
					"text": "新器 20210/2509:05"
				},
				"riskSource": 1002
			},
			"riskLabel1": "politics",
			"riskLabel2": "zhengzhixiangzheng",
			"riskLabel3": "danghui",
			"riskLevel": "REJECT",
			"time": 82
		}
	],
	"audioDetail": [
		{
			"allLabels": [
				{
					"probability": 0.998463273048401,
					"riskDescription": "辱骂:人身攻击:重度人身攻击",
					"riskDetail": {
						"audioText": "操你妈，几个，那你几个一起干，操你妈，你和奶奶的都还在不捣蛋也不",
						"riskSource": 1001
					},
					"riskLabel1": "abuse",
					"riskLabel2": "renshengongji",
					"riskLabel3": "zhongdurenshengongji",
					"riskLevel": "REJECT"
				}
			],
			"audioEndtime": 20,
			"audioStarttime": 10,
			"audioText": "操你妈，几个，那你几个一起干，操你妈，你和奶奶的都还在不捣蛋也不",
			"audioUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/POST_VIDEO%2FPOST_VIDEO_AUDIO%2F20221025%2Fedaa113581ec1c18df7b44c86d36ae3b_a0001.wav?q-sign-algorithm=sha1&q-ak=AKIDg9LHyOYSAcmfHekZ6NN6XidHflbASUHn&q-sign-time=1666684511%3B1669276511&q-key-time=1666684511%3B1669276511&q-header-list=host&q-url-param-list=&q-signature=e87204b53077ddc763ddd2b7b5bd5e1382d4cc63",
			"businessLabels": [],
			"requestId": "edaa113581ec1c18df7b44c86d36ae3b_a0001",
			"riskDescription": "辱骂:人身攻击:重度人身攻击",
			"riskDetail": {
				"audioText": "操你妈，几个，那你几个一起干，操你妈，你和奶奶的都还在不捣蛋也不",
				"riskSource": 1001
			},
			"riskLabel1": "abuse",
			"riskLabel2": "renshengongji",
			"riskLabel3": "zhongdurenshengongji",
			"riskLevel": "REJECT"
		},
		{
			"allLabels": [
				{
					"probability": 0.857458027460472,
					"riskDescription": "涉政:国家机构:国家机构",
					"riskDetail": {
						"audioText": "妈一起干，没事，让他报警，让他报警，找警察来，去干，备",
						"riskSource": 1001
					},
					"riskLabel1": "politics",
					"riskLabel2": "guojiajigou",
					"riskLabel3": "guojiajigou",
					"riskLevel": "REJECT"
				}
			],
			"audioEndtime": 40,
			"audioStarttime": 30,
			"audioText": "妈一起干，没事，让他报警，让他报警，找警察来，去干，备",
			"audioUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/POST_VIDEO%2FPOST_VIDEO_AUDIO%2F20221025%2Fedaa113581ec1c18df7b44c86d36ae3b_a0003.wav?q-sign-algorithm=sha1&q-ak=AKIDg9LHyOYSAcmfHekZ6NN6XidHflbASUHn&q-sign-time=1666684511%3B1669276511&q-key-time=1666684511%3B1669276511&q-header-list=host&q-url-param-list=&q-signature=fcf1b1275ca7dbafacaf06bd61cd05f5d612e9dc",
			"businessLabels": [],
			"requestId": "edaa113581ec1c18df7b44c86d36ae3b_a0003",
			"riskDescription": "涉政:国家机构:国家机构",
			"riskDetail": {
				"audioText": "妈一起干，没事，让他报警，让他报警，找警察来，去干，备",
				"riskSource": 1001
			},
			"riskLabel1": "politics",
			"riskLabel2": "guojiajigou",
			"riskLabel3": "guojiajigou",
			"riskLevel": "REJECT"
		},
		{
			"allLabels": [
				{
					"probability": 0.998539209365845,
					"riskDescription": "辱骂:人身攻击:重度人身攻击",
					"riskDetail": {
						"audioText": "你别动他，别动他，让他报警，哎呀，日你妈了个逼，继干，你继续干，让他们",
						"riskSource": 1001
					},
					"riskLabel1": "abuse",
					"riskLabel2": "renshengongji",
					"riskLabel3": "zhongdurenshengongji",
					"riskLevel": "REJECT"
				}
			],
			"audioEndtime": 70,
			"audioStarttime": 60,
			"audioText": "你别动他，别动他，让他报警，哎呀，日你妈了个逼，继干，你继续干，让他们",
			"audioUrl": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/POST_VIDEO%2FPOST_VIDEO_AUDIO%2F20221025%2Fedaa113581ec1c18df7b44c86d36ae3b_a0006.wav?q-sign-algorithm=sha1&q-ak=AKIDg9LHyOYSAcmfHekZ6NN6XidHflbASUHn&q-sign-time=1666684511%3B1669276511&q-key-time=1666684511%3B1669276511&q-header-list=host&q-url-param-list=&q-signature=55b6544e7408f29d4b7286690eb7494113ad7b31",
			"businessLabels": [],
			"requestId": "edaa113581ec1c18df7b44c86d36ae3b_a0006",
			"riskDescription": "辱骂:人身攻击:重度人身攻击",
			"riskDetail": {
				"audioText": "你别动他，别动他，让他报警，哎呀，日你妈了个逼，继干，你继续干，让他们",
				"riskSource": 1001
			},
			"riskLabel1": "abuse",
			"riskLabel2": "renshengongji",
			"riskLabel3": "zhongdurenshengongji",
			"riskLevel": "REJECT"
		}
	],
	"riskLevel": "REJECT",
	"auxInfo": {
		"frameCount": 2,
		"time": 85,
		"billingAudioDuration":85,
		"billingImgNum":2,
		"passThrough": {
                "passThrough1": "透传字段1",
                "passThrough2": "透传字段2",
                "passThrough3": "透传字段3"
		}
	}
}
```
