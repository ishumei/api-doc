# 智能音频文件识别产品API文档

## 异步接口

### 请求URL

| 集群 | URL                                              | 支持语种                                 |
| ---- | ------------------------------------------------ | ---------------------------------------------- |
| 上海 | `http://api-audio-sh.fengkongcloud.com/audio/v4` | 中文、阿拉伯语 |
| 硅谷 | `http://api-audio-gg.fengkongcloud.com/audio/v4` | 中文、英文、阿拉伯语 |
| 新加坡 | `http://api-audio-xjp.fengkongcloud.com/audio/v4` | 中文、英文、阿拉伯语 |

### 字符编码

`UTF-8`

### 请求方法

`POST`

### 建议超时时长

1s

### 音频格式限制

`WAV`、`MP3`、`AAC`、`AMR`、`3GP`、`M4A`、`WMA`、`OGG`、`APE`、`FLAC`、`ALAC`、`WAVPACK`、`SILK_V3`等

### 请求体限制

所有请求参数大小总和不能超过18M

### 请求参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名**        | **类型**    | **参数说明**         | **传入说明**               | **规范**                                                     |
| :-------------------- | :---------- | :------------------- | :------------------------- | :----------------------------------------------------------- |
| accessKey             | string      | 公司密钥             | 必传参数                   | 由数美提供                                                   |
| appId                 | string      | 应用标识             | 必传参数                   | 用于区分应用，需要联系数美服务开通，请使用数美单独提供的传值为准 |
| eventId               | string      | 事件标识             | 必传参数                   | 用于区分场景数据，需要联系数美服务开通，请使用数美单独提供的传值为准 |
| type                  | string      | 检测的风险类型       | businesstype和type必传其一 | <p>AUDIOPOLITICAL：一号领导人声纹识别</p><p>POLITY：涉政识别</p><p>EROTIC：色情识别</p><p>ADVERT：广告识别</p><p>ADLAW：广告法识别</p><p>BAN：违禁识别</p><p>VIOLENT：暴恐识别</p><p>ANTHEN：国歌识别</p><p>MOAN：娇喘识别</p><p>DIRTY：辱骂识别</p><p>BANEDAUDIO：违禁歌曲</p><p>COPYRIGHTSONGS：版权歌曲</p><p>如需做组合识别，通过下划线连接即可，例如POLITY_EROTIC_MOAN涉政、色情和娇喘识别</p><p>建议传入：<br/>POLITY_EROTIC_MOAN_ADVERT</p> |
| businessType          | string      | 检测的业务标签类型   | businesstype和type必传其一 | 可选值：<br/>SING：唱歌识别<br/>LANGUAGE：语种识别<br/>GENDER：性别识别<br/>TIMBRE：音色识别 <br/>VOICE：人声属性<br/>MINOR：未成年识别<br/>AUDIOSCENE：声音场景<br/>AGE：年龄识别<br/>如需识别音色、唱歌、语种GENDER必传<br/>type和 businessType 必须填其一 |
| translationTargetLang | string      | 翻译目标语种         | 非必传参数                 | 将输入的文本翻译成目标语种。如需开通使用请联系数美商务 可选值： `zh`：中文 `en`：英文 |
| contentType           | string      | 待识别音频内容的格式 | 必传参数                   | <p>可选值：</p><p>URL：识别内容为音频url地址；</p><p>RAW：识别内容为音频的base64编码数据</p> |
| content               | string      | 待识别的音频内容     | 必传参数                   | <p>可以为url地址或者base64编码数据。</p><p>其中，base64编码数据上限15M，仅支持pcm、wav、mp3格式, 并且pcm格式数据必须采用16-bit小端序编码。推荐使用pcm、wav格式传输</p> |
| data                  | json object | 本次请求相关信息     | 必传参数                   | 最长1MB，[详见data参数](#data)                               |
| btId                  | string      | 音频文件唯一标识     | 必传参数                   | 唯一标识这条音频文件，方便将回调结果对应上，超过128位将被截断，不能重复 |
| callback              | string      | 回调http接口         | 非必传参数                 | 当该字段非空时，服务将根据该字段回调通知用户审核结果         |
| acceptLang            | string      | 返回标签的语种类型   | 非必传参数                 | 选择返回标签的语种类型<br/>可选值：<br/>zh：中文<br/>en：英文<br/>不传入默认为返回中文标签 |

其中，<span id="data">data</span>的内容如下：

| **请求参数名** | **类型** | **参数说明**       | **传入说明** | **规范**                                                                                   |
| :------------- | :------- | :----------------- | :----------- | :----------------------------------------------------------------------------------------- |
| tokenId        | string   | 用户账号           | 非必传参数   | 用于用户行为分析，建议传入用户UID                                                          |
| formatInfo     | string   | 音频数据格式       | 非必传参数   | 当音频内容格式为RAW时必须存在，可选值：pcm、wav、mp3                                       |
| rate           | int      | 音频数据采样率     | 非必传参数   | 当音频数据格式为pcm时必须存在，范围限制8000-32000。                                        |
| track          | int      | 音频数据声道数     | 非必传参数   | <p>当音频数据格式为pcm时必须存在，可选值：</p><p>1: 单声道</p><p>2: 双声道</p>             |
| returnAllText  | int      | 返回音频片段的等级 | 非必传参数   | 可选值如下（默认为`0`）：<br/> `0`：返回风险片段识别结果<br/> `1`：返回所有片段识别结果<br/>该参数仅用于控制片段识别结果的返回， 不影响整体识别结果的返回。<br/>当选择“返回所有片段识别结果”时，片段识别结果中包含riskLevel为PASS、REVIEW和REJECT的片段识别结果；<br/>当选择“返回风险片段识别结果”时，片段识别结果中仅包含riskLevel为REVIEW和REJECT的片段识别结果；<br/>片段识别结果对应回调或者查询响应中的audioDetail字段。 |
| audioDetectStep | int | 间隔审核步长 | 非必传参数 | 间隔审核步长，取值范围为1-36整数，取1表示跳过一个10S的音频片段审核，取2表示跳过两个，以此类推，不使用该功能时音频内容全部过审。启用该功能时，建议开启returnAllText，采用每个片段的ASR识别结果。 |
| receiveTokenId | string | 私聊场景下消息接收者的tokenId | N | 由数字、字母、下划线、短杠组成的长度小于等于64位的字符串 |
| lang           | string   | 音频语言类型     | 非必传参数   | 可选值如下，（默认值为zh）：<br/>`zh`：中文<br/>`en`：英文<br/>`ar`：阿拉伯语<br/>`hi`：印地语<br/>`es`：西班牙语<br/>`fr`：法语<br/>`ru`：俄语<br/>`pt`：葡萄牙语<br/>`id`：印尼语<br/>`de`：德语<br/>`ja`：日语<br/>`tr`：土耳其语<br/>`vi`：越南语<br/>`it`：意大利语<br/>`th`：泰语<br/>`tl`：菲律宾语<br/>`ko`：韩语<br/>`ms`：马来语                    |
| deviceId        | string | 数美设备指纹标识  | 非必传参数    | 数美设备指纹生成的设备唯一标识                                                               |
| room        | string | 房间号  | 非必传参数    | 房间号，建议传入                                                                |
| dataId | string | 数据标识 | 非必填参数 | 数据标识 |
| ip              | string | ipv4或ipv6地址 | 非必传参数    | 发送该音频的用户公网ip地址                                                      |
| level | int | 用户等级，针对不同等级的用户可配置不同拦截策略 | 非必传参数 | 可选值：<br/>`0`：最低级用户，典型如新注册、完全不活跃或等级为0的用户等;<br/>`1`：较低级用户，典型如低活跃或低等级用户等；<br/>`2`：中等级用户，典型如具备一定活跃或等级中等的用户等；<br/>`3`：较高级用户，典型如高活跃或高等级用户等；<br/>`4`：最高级用户，典型如付费用户、VIP用户等 |
| gender | string | 用户性别 | 非必传参数 | 可选值：<br/>male男性 <br/>female女性 |
| extra | json object | 辅助参数 | 非必传参数 | [详见extra参数](#extra) |

<span id="extra">data中，extra的内容如下：</span>

| **请求参数名** | **类型**    | **参数说明** | **是否必传** | **规范**                                   |
| -------------- | ----------- | ------------ | ------------ | ------------------------------------------ |
| passThrough    | json_object | 透传字段     | N            | 透传字段，该字段下所有内容会通过回调返回。 |

### 返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明**                   | **是否必返** | **规范**                                                     |
| :----------------- | :----------- | :----------------------------- | :----------- | :----------------------------------------------------------- |
| requestId          | string       | 请求的唯一标识                 | 是           |                                                              |
| code               | int          | 请求返回码                     | 是           | <p>1100：成功</p><p>1901：QPS超限</p><p>1902：参数不合法</p><p>1903：服务失败</p><p>1904：下载失败</p><p>1905：解码失败</p><p>9101：无权限操作</p> |
| message            | string       | 请求返回描述，和请求返回码对应 | 是           |                                                              |
| btId               | string       | 音频唯一标识                   | 否           | code为1100情况下返回                                         |

## 主动查询结果

### 请求URL

| 集群 | URL                                                    | 支持语种                                   |
| ---- | ------------------------------------------------------ | ---------------------------------------------- |
| 上海 | `http://api-audio-sh.fengkongcloud.com/query_audio/v4` | 中文               |
| 硅谷 | `http://api-audio-gg.fengkongcloud.com/query_audio/v4` | 中文、英文、阿拉伯语 |
| 新加坡 | `http://api-audio-xjp.fengkongcloud.com/query_audio/v4` | 中文、英文、阿拉伯语 |

### 字符编码

`UTF-8`

### 请求方法

`POST`

### 建议超时时长

1s

### 请求参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明**     | **传入说明** | **规范**                               |
| :------------- | :------- | :--------------- | :----------- | :------------------------------------- |
| accessKey      | string   | 公司密钥         | 必传参数     | 由数美提供                             |
| btId           | string   | 音频文件唯一标识 | 必传参数     | 唯一标识这条音频文件，用于查询识别结果 |

### 返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名**    | **类型**    | **参数说明**                   | **是否必返** | **规范**                                                     |
| :------------ | :---------- | :----------------------------- | :----------- | :----------------------------------------------------------- |
| requestId     | string      | 本次请求的唯一标识             | 是           |                                                              |
| btId          | string      | 音频唯一标识                   | 是           |                                                              |
| code          | int         | 请求返回码                     | 是           | <p>1100：成功</p><p>1101：正在处理中</p><p>1901：QPS超限</p><p>1902：参数不合法</p><p>1903：服务失败</p><p>1904：下载失败</p><p>1905：解码失败</p><p>9100：余额不足</p><p>9101：无权限操作</p><p>除message和requestId之外的字段，只有当code为1100时才会存在</p> |
| message       | string      | 请求返回描述，和请求返回码对应 | 是           |                                                              |
| riskLevel     | string      | 当前事件的处置建议             | 是           | <p>可能返回值：<br/>PASS：通过</p><p>REVIEW：审核</p><p>REJECT：拒绝</p><p>建议：对接初期不直接使用结果，进行拦截尺度调优，符合预期后在进行使用</p> |
| audioText     | string      | 整段音频转译文本结果           | 是           |                                                              |
| audioTime     | int         | 整段音频的音频时长             | 是           | 单位秒                                                       |
| audioDetail   | json_array  | 音频片段信息                   | 是           | 回调的音频片段信息，[详见audioDetail参数](#audioDetail)      |
| audioTags     | json_object | 音频标签                       | 否           | 返回性别、音色、是否唱歌等标签。历史兼容字段，建议直接使用businessLabels |
| requestParams | json_object | 透传字段                       | 是           | 返回data下所有字段                                           |
| auxInfo       | json_object | 辅助信息                       | 否           |                                                              |

<span id="audioDetail">audioDetail</span>内每个元素的内容如下：

| **参数名**      | **类型**    | **参数说明**     | **是否必返** | **规范**                                                     |
| :-------------- | :---------- | :--------------- | :----------- | :----------------------------------------------------------- |
| audioStarttime  | float       | 音频片段起始时间 | 是           | 相对音频开始的时间距离，单位是秒                             |
| audioEndtime    | float       | 音频片段结束时间 | 是           | 相对音频开始的时间距离，单位是秒                             |
| audioUrl        | string      | 音频片段链接     | 是           | mp3格式                                                      |
| riskLevel       | string      | 音频片段识别结果 | 是           | <p>可能返回值：<br/>PASS：通过</p><p>REVIEW：审核</p><p>REJECT：拒绝</p> |
| businessLabels  | json_array  | 业务标签返回     | 否           | 全部业务标签，[详见businessLabels参数](#businessLabels2)     |
| allLabels       | json_array  | 风险标签返回     | 否           | 全部风险标签，[详见allLabels参数](#allLabels2)               |
| riskLabel1      | string      | 一级风险标签     | 是           |                                                              |
| riskLabel2      | string      | 二级风险标签     | 是           |                                                              |
| riskLabel3      | string      | 三级风险标签     | 是           |                                                              |
| riskDescription | string      | 风险原因         | 是           | 仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |
| riskDetail      | json_object | 风险详情         | 否           | [详见riskDetail参数](#riskDetail)                            |


其中，<span id="riskDetail">riskDetail</span>详细内容如下：

| **参数名**   | **类型**   | **参数说明**             | **是否必返** | **规范**                                                                                  |
| :----------- | :--------- | :----------------------- | :----------- | :---------------------------------------------------------------------------------------- |
| audioText    | string     | 音频转译文本的结果       | 否           |                                                                                           |
| matchedLists | json_array | 命中的客户自定义名单信息 | 否           | 命中客户自定义名单时返回，[详见matchedLists参数](#matchedLists)                         |
| riskSegments | json_array | 高风险内容片段           | 否           | 在涉政、暴恐、违禁、竞品、广告法等功能的时候存在，[详见riskSegments参数](#riskSegments) |

riskDetail中，<span id="matchedLists">matchedLists</span>详细内容如下：

| **参数名** | **类型**   | **参数说明**                 | **是否必返** | **规范**                  |
| :--------- | :--------- | :--------------------------- | :----------- | :------------------------ |
| name       | string     | 客户自定义名单名称           | 是           |                           |
| words      | json_array | 命中的这个名单中的敏感词信息 | 是           | [详见words参数](#words) |

matchedLists中，<span id="words">words</span>详细内容如下：

| **参数名** | **类型**  | **参数说明**   | **是否必返** | **规范** |
| :--------- | :-------- | :------------- | :----------- | :------- |
| word       | string    | 敏感词         | 是           |          |
| position   | int_array | 敏感词所在位置 | 是           |          |

riskDetail中，<span id="riskSegments">riskSegments</span>详细内容如下：

| **参数名** | **类型**  | **参数说明**           | **是否必返** | **规范** |
| :--------- | :-------- | :--------------------- | :----------- | :------- |
| segment    | string    | 高风险内容片段         | 否           |          |
| position   | int_array | 高风险内容片段所在位置 | 否           |          |

其中，<span id="audioTags">audioTags</span>详细内容如下：

| **参数名** | **类型**    | **参数说明** | **是否必返** | **规范**                                                                           |
| :--------- | :---------- | :----------- | :----------- | :--------------------------------------------------------------------------------- |
| gender     | json_object | 性别标签     | 否           | 当type取值包含GENDER时返回                                                         |
| timbre     | json_array  | 音色标签     | 否           | 当type取值包含TIMBRE时返回                                                         |
| song       | int         | 唱歌标签     | 否           | <p>当type取值包含SING时返回</p><p>可能取值：</p><p>0：没有唱歌</p><p>1：有唱歌</p> |
| language   | json_object | 语种识别     | 否           | type取值包含LANGUAGE时返回                                                         |

audioTags中，gender详细内容如下：

| **参数名**  | **类型** | **参数说明**       | **是否必返** | **规范**                                |
| :---------- | :------- | :----------------- | :----------- | :-------------------------------------- |
| label       | string   | 性别标签名称       | 是           | <p>可能取值：</p><p>男性</p><p>女性</p> |
| probability | int      | 对应性别可能性大小 | 是           | 取值0-100，数值越高表示概率越大         |

audioTags中，timbre详细内容如下：

| **参数名**  | **类型** | **参数说明**           | **是否必返** | **规范**                                                     |
| :---------- | :------- | :--------------------- | :----------- | :----------------------------------------------------------- |
| label       | string   | 音色标签类别           | 是           | <p>可能取值：</p><p>大叔</p><p>青年</p><p>正太</p><p>老年</p><p>女王</p><p>御姐</p><p>少女</p><p>萝莉</p><p>大妈</p><p>男性</p><p>女性</p><p>无人声</p> |
| probability | int      | 对应音色标签可能性大小 | 是           | 取值0-100，数值越高表示概率越大                              |

audioTags中，language详细内容如下：

| **参数名**  | **类型** | **参数说明**           | **是否必返** | **规范**                                                   |
| :---------- | :------- | :--------------------- | :----------- | :--------------------------------------------------------- |
| label       | int      | 语种识别类别标识       | 是           | <p>可能取值：</p><p>0:普通话</p><p>1:英语</p><p>2:粤语</p><p>3:藏语</p><p>4:维语</p><p>5:蒙语</p><p>6:朝鲜语</p><p>-1:其他语种</p> |
| probability | int      | 对应音色标签可能性大小 | 是           | 取值0-100，数值越高表示概率越大                            |

其中，<span id="auxInfo">auxInfo</span>详细内容如下：

| **参数名**   | **类型**   | **参数说明**             | **是否必返** | **规范**                                                                                |
| :----------- | :--------- | :----------------------- | :----------- | :-------------------------------------------------------------------------------------- |
| errorCode    | int        | 状态码                 | 是           |<p>状态码</p><p>2003：音频下载失败</p><p>2007：无有效数据</p>  |

## 异步回调结果

用户如果需要服务端主动对音频检测结果进行回调，则需要在请求参数中传入callback参数，服务端审核完成后将识别结果主动回调到该地址。

### 支持协议

HTTP 或 HTTPS

### 字符编码格式

请求使用 UTF-8 字符集进行编码

### 请求方法

POST

### 建议超时时长

5s

### 推送策略

当用户收到推送结果，并返回 HTTP 状态码为200时，表示推送成功；否则系统将进行最多12次推送，第一次失败会等待5秒，第二次失败会等待10秒.....逐渐递增至60秒

### 回调参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名**     | **类型**    | **参数说明**                   | **是否必返** | **规范**                                                     |
| :------------- | :---------- | :----------------------------- | :----------- | :----------------------------------------------------------- |
| requestId      | string      | 本次请求的唯一标识             | 是           |                                                              |
| btId           | string      | 音频唯一标识                   | 是           |                                                              |
| code           | int         | 请求返回码                     | 是           | <p>1100：成功</p><p>1901：QPS超限</p><p>1902：参数不合法</p><p>1903：服务失败</p><p>1904：下载失败</p><p>1905：解码失败</p><p>9101：无权限操作</p><p>除message和requestId之外的字段，只有当code为1100时才会存在</p> |
| message        | string      | 请求返回描述，和请求返回码对应 | 是           |                                                              |
| riskLevel      | string      | 当前事件的处置建议             | 是           | <p>可能返回值：<br/>PASS：通过</p><p>REVIEW：审核</p><p>REJECT：拒绝</p><p>建议：对接初期不直接使用结果，进行拦截尺度调优，符合预期后在进行使用</p> |
| audioText      | string      | 整段音频转译文本结果           | 是           |                                                              |
| audioTime      | int         | 整段音频的音频时长             | 是           | 单位秒                                                       |
| audioDetail    | json_array  | 音频片段信息                   | 是           | 回调的音频片段信息，[详见audioDetail参数](#audioDetail2)      |
| audioTags      | json_object | 音频标签                       | 否           | 返回性别、音色、是否唱歌等标签                               |
| requestParams  | json_object | 透传字段                       | 是           | 返回data下所有字段 |
| businessLabels | json_array  | 业务标签返回                   | 否           | 返回业务标签内容（目前只支持MINOR,策略命中返回标签内容,否则为空,历史字段不建议使用） |
| auxInfo        | json_object | 辅助信息                      | 否           |                                                            |
| tokenProfileLabels | json_array  | 账号属性标签                   | 否           | 仅在开启功能时返回 |
| tokenRiskLabels | json_array  | 账号风险标签                   | 否           | 仅在开启功能时返回  |

其中，<span id="audioDetail2">audioDetail</span>详细内容如下：

| **参数名**      | **类型**    | **参数说明**     | **是否必返** | **规范**                                                                 |
| :-------------- | :---------- | :--------------- | :----------- | :----------------------------------------------------------------------- |
| requestId       | string      | 音频片段请求唯一标识 | 是           |                                                                |
| audioStarttime  | float       | 音频片段起始时间 | 是           | 相对音频开始的时间距离，单位是秒                                         |
| audioEndtime    | float       | 音频片段结束时间 | 是           | 相对音频开始的时间距离，单位是秒                                         |
| audioUrl        | string      | 音频片段链接     | 是           | mp3格式                                                                  |
| riskLevel       | string      | 音频片段识别结果 | 是           | <p>可能返回值：<br/>PASS：通过</p><p>REVIEW：审核</p><p>REJECT：拒绝</p> |
| businessLabels | json_array | 业务标签返回 | 否 | 全部业务标签，[详见businessLabels参数](#businessLabels2) |
| langResult | json_object | 语种信息 | 否 | 语种信息。[详见语种信息参数](#langResult) |
| allLabels | json_array | 风险标签返回 | 否 | 全部风险标签，[详见allLabels参数](#allLabels2) |
| riskLabel1      | string      | 一级风险标签     | 是           |                                                                          |
| riskLabel2      | string      | 二级风险标签     | 是           |                                                                          |
| riskLabel3      | string      | 三级风险标签     | 是           |                                                                          |
| riskDescription | string      | 风险原因         | 是           | 仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理                                                                         |
| riskDetail      | json_object | 风险详情         | 否           | [详见riskDetail参数](#riskDetail2)                                        |

其中，<span id="riskDetail2">riskDetail</span>详细内容如下：

| **参数名**   | **类型**   | **参数说明**             | **是否必返** | **规范**                                                                                |
| :----------- | :--------- | :----------------------- | :----------- | :-------------------------------------------------------------------------------------- |
| audioText    | string     | 音频转译文本的结果       | 否           |                                                                                         |
| matchedLists | json_array | 命中的客户自定义名单信息 | 否           | 命中客户自定义名单时返回，[详见matchedLists参数](#matchedLists2)                         |
| riskSegments | json_array | 高风险内容片段           | 否           | 在涉政、暴恐、违禁、竞品、广告法等功能的时候存在，[详见riskSegments参数](#riskSegments2) |

riskDetail中，<span id="matchedLists2">matchedLists</span>详细内容如下：

| **参数名** | **类型**   | **参数说明**                 | **是否必返** | **规范**                |
| :--------- | :--------- | :--------------------------- | :----------- | :---------------------- |
| name       | string     | 客户自定义名单名称           | 是           |                         |
| words      | json_array | 命中的这个名单中的敏感词信息 | 是           | [详见words参数](#words2) |

matchedLists中，<span id="words2">words</span>详细内容如下：

| **参数名** | **类型**  | **参数说明**   | **是否必返** | **规范** |
| :--------- | :-------- | :------------- | :----------- | :------- |
| word       | string    | 敏感词         | 是           |          |
| position   | int_array | 敏感词所在位置 | 是           |          |

riskDetail中，<span id="riskSegments2">riskSegments</span>详细内容如下：

| **参数名** | **类型**  | **参数说明**           | **是否必返** | **规范** |
| :--------- | :-------- | :--------------------- | :----------- | :------- |
| segment    | string    | 高风险内容片段         | 否           |          |
| position   | int_array | 高风险内容片段所在位置 | 否           |          |

其中，<span id="audioTags">audioTags</span>详细内容如下：

| **参数名** | **类型**    | **参数说明** | **是否必返** | **规范**                                                                           |
| :--------- | :---------- | :----------- | :----------- | :--------------------------------------------------------------------------------- |
| gender     | json_object | 性别标签     | 否           | 当type取值包含GENDER时返回                                                         |
| timbre     | json_array  | 音色标签     | 否           | 当type取值包含TIMBRE时返回                                                         |
| song       | int         | 唱歌标签     | 否           | <p>当type取值包含SING时返回</p><p>可能取值：</p><p>0：没有唱歌</p><p>1：有唱歌</p> |
| language   | json_object | 语种识别     | 否           | type取值包含LANGUAGE时返回                                                         |

audioTags中，gender详细内容如下：

| **参数名**  | **类型** | **参数说明**       | **是否必返** | **规范**                                |
| :---------- | :------- | :----------------- | :----------- | :-------------------------------------- |
| label       | string   | 性别标签名称       | 是           | <p>可能取值：</p><p>男性</p><p>女性</p> |
| probability | int      | 对应性别可能性大小 | 是           | 取值0-100，数值越高表示概率越大         |

audioTags中，timbre详细内容如下：

| **参数名**  | **类型** | **参数说明**           | **是否必返** | **规范**                                                     |
| :---------- | :------- | :--------------------- | :----------- | :----------------------------------------------------------- |
| label       | string   | 音色标签类别           | 是           | <p>可能取值：</p><p>大叔</p><p>青年</p><p>正太</p><p>老年</p><p>女王</p><p>御姐</p><p>少女</p><p>萝莉</p><p>大妈</p><p>男性</p><p>女性</p><p>无人声</p> |
| probability | int      | 对应音色标签可能性大小 | 是           | 取值0-100，数值越高表示概率越大                              |

audioTags中，language详细内容如下：

| **参数名**  | **类型** | **参数说明**           | **是否必返** | **规范**                                                     |
| :---------- | :------- | :--------------------- | :----------- | :----------------------------------------------------------- |
| label       | int      | 语种识别类别标识       | 是           | <p>可能取值：</p><p>0:普通话</p><p>1:英语</p><p>2:粤语</p><p>3:藏语</p><p>4:维语</p><p>5:蒙语</p><p>6:朝鲜语</p><p>-1:其他语种</p> |
| probability | int      | 对应音色标签可能性大小 | 是           | 取值0-100，数值越高表示概率越大                              |

*<span id="businessLabels2">businessLabels</span>*数组中每一项具体参数如下：

| **参数名**          | **类型**    | **参数说明** | **是否必返** | **规范**                                     |
| ------------------- | ----------- | ------------ | ------------ | -------------------------------------------- |
| businessLabel1      | string      | 一级标签     | 是           | 一级标签                                     |
| businessLabel2      | string      | 二级标签     | 是           | 二级标签                                     |
| businessLabel3      | string      | 三级标签     | 是           | 三级标签                                     |
| businessDescription | string      | 标签描述     | 是           | 格式为"一级标签:二级标签:三级标签"的中文名称 |
| confidenceLevel     | int         | 置信等级     | 否           | 可选值在0～2之间，值越大，可信度越高         |
| probability         | float       | 置信度       | 否           | 可选值为0~1，值越大，可信度越高              |
| businessDetail      | json_object | 详细信息     | 否           | 保留字段                                     |

 *<span id="allLabels2">allLabels</span>*数组中每一项具体参数如下：

| **参数名**      | **类型**    | **参数说明** | **是否必返** | **规范**                                                     |
| --------------- | ----------- | ------------ | ------------ | ------------------------------------------------------------ |
| riskLabel1      | string      | 一级风险标签 | 是           | 一级风险标签                                                 |
| riskLabel2      | string      | 二级风险标签 | 是           | 二级风险标签                                                 |
| riskLabel3      | string      | 三级风险标签 | 是           | 三级风险标签                                                 |
| riskDescription | string      | 风险原因     | 是           | 风险原因，仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |
| riskLevel       | string      | 处置建议     | 是           | 可能返回值： PASS：通过REVIEW：审核REJECT：拒绝              |
| probability     | float       | 置信度       | 否           | 可选值在0～1之间，值越大，风险可能性越高，值越小，无风险可能性越高 |
| riskDetail      | json_object | 风险详情     | 否           | [详见riskDetail参数](#riskDetail2)                           |

*tokenProfileLabels，tokenRiskLabels 数组中每一项具体参数如下:*

| **参数名称**        | **类型** | 参数说明 | **是否必返** | **说明**                                 |
| :------------------ | :------- | -------- | :----------- |:---------------------------------------|
| label1      | string   | 一级标签     | 否           |                                        |
| label2      | string   | 二级标签     | 否           |                                        |
| label3      | string   | 三级标签     | 否           |                                        |
| description | string   | 标签描述     | 否           | 账号标签描述，仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |
| timestamp   | int      | 打标签时间戳 | 否           | 13位Unix时间戳，单位：毫秒                       |

其中，<span id="auxInfo">auxInfo</span>详细内容如下：

| **参数名**   | **类型**   | **参数说明**             | **是否必返** | **规范**                                                                                |
| :----------- | :--------- | :----------------------- | :----------- | :-------------------------------------------------------------------------------------- |
| errorCode    | int        | 状态码                 | 是           |<p>状态码</p><p>2003：音频下载失败</p><p>2007：无有效数据</p>  |

<span id='langResult'>语种信息langResult结构如下：</span>

| **参数名称**   | **类型** | **参数说明** | **是否必返** | **规范**                                                    |
| :------------- | :------- | :----------- | :----------- | :---------------------------------------------------------- |
| translatedText | string   | 文本翻译结果 | N            | 当传入translationTargetLang时返回的字段。值为翻译后的文本。 |

## 示例

### 上传请求示例

```bash
curl -v 'http://api-audio-bj.fengkongcloud.com/audio/v4' -d '{
    "accessKey": "*************",
    "appId": "default",
    "eventId": "default",
    "type": "POLITY_EROTIC_ADVERT_MOAN",
    "businessType":"GENDER_TIMBRE_SING_LANGUAGE",
    "btId": "test1",
    "contentType": "URL",
    "content": "*************",
    "callback": "*************",
    "data": {
        "returnAllText": 1,
        "tokenId": "token-short"
        }
    }
}'

```

### 同步返回示例

```json
{
    "code": 1100,
    "message": "成功",
    "requestId":" *************",
    "btId":"*************"
}
```


### 主动查询结果请求示例

```bash
curl -v 'http://api-audio-bj.fengkongcloud.com/query_audio/v4' -d '{
    "accessKey": "*************",
    "btId": "*************"
}'
```

### 主动查询结果返回示例

```json
{
    "requestId": "6a9cb980346dfea41111656a514e9109",
    "btId": "1604311839040",
    "code": 1100,
    "message": "正常",
    "riskLevel": "PASS",
    "audioDetail": [
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0000",
            "audioStarttime": 0,
            "audioEndtime": 10,
            "audioUrl": "https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0000.mp3",
            "businessLabels":[
                {
                    "businessDescription":"唱歌:唱歌:唱歌",
                    "businessLabel1":"sing",
                    "businessLabel2":"changge",
                    "businessLabel3":"changge",
                    "confidenceLevel":2,
                    "probability":0.858334402569294
                }
            ],
            "allLabels":[],
            "riskLevel": "PASS",
            "riskLabel1": "normal",
            "riskLabel2": "",
            "riskLabel3": "",
            "riskDescription": "正常",
            "riskDetail": {
                "audioText": ""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0001",
            "audioStarttime": 10,
            "audioEndtime": 20,
            "audioUrl": "https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0001.mp3",
            "riskLevel": "PASS",
            "riskLabel1": "normal",
            "riskLabel2": "",
            "riskLabel3": "",
            "riskDescription": "正常",
            "riskDetail": {
                "audioText": ""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0002",
            "audioStarttime": 20,
            "audioEndtime": 30,
            "audioUrl": "https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0002.mp3",
            "riskLevel": "PASS",
            "riskLabel1": "normal",
            "riskLabel2": "",
            "riskLabel3": "",
            "riskDescription": "正常",
            "riskDetail": {
                "audioText": ""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0003",
            "audioStarttime": 30,
            "audioEndtime": 40,
            "audioUrl": "https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0003.mp3",
            "riskLevel": "PASS",
            "riskLabel1": "normal",
            "riskLabel2": "",
            "riskLabel3": "",
            "riskDescription": "正常",
            "riskDetail": {
                "audioText": ""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0004",
            "audioStarttime": 40,
            "audioEndtime": 50,
            "audioUrl": "https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0004.mp3",
            "riskLevel": "PASS",
            "riskLabel1": "normal",
            "riskLabel2": "",
            "riskLabel3": "",
            "riskDescription": "正常",
            "riskDetail": {
                "audioText": ""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0005",
            "audioStarttime": 50,
            "audioEndtime": 60,
            "audioUrl": "https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0005.mp3",
            "riskLevel": "PASS",
            "riskLabel1": "normal",
            "riskLabel2": "",
            "riskLabel3": "",
            "riskDescription": "正常",
            "riskDetail": {
                "audioText": ""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0006",
            "audioStarttime": 60,
            "audioEndtime": 60,
            "audioUrl": "https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0006.mp3",
            "riskLevel": "PASS",
            "riskLabel1": "normal",
            "riskLabel2": "",
            "riskLabel3": "",
            "riskDescription": "正常"
        }
    ],
    "audioTags": {
        "gender": {
            "label": "女性",
            "probability": 95
        },
        "language": [
            {
                "confidence": 0,
                "label": 2
            },
            {
                "confidence": 99,
                "label": 0
            },
            {
                "confidence": 0,
                "label": 1
            }
        ],
        "song": 0,
        "timbre": [
            {
                "label": "女性",
                "probability": 95
            },
            {
                "label": "女王",
                "probability": 12
            },
            {
                "label": "御姐",
                "probability": 37
            },
            {
                "label": "少女",
                "probability": 56
            },
            {
                "label": "大妈",
                "probability": 67
            },
            {
                "label": "萝莉",
                "probability": 24
            }
        ]
    }
}
```

### 回调返回示例

```json
{
    "requestId":"6a9cb980346dfea41111656a514e9109",
    "btId":"1604311839040",
    "code":1100,
    "message":"正常",
    "riskLevel":"PASS",
    "audioDetail":[
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0000",
            "audioStarttime":0,
            "audioEndtime":10,
            "audioUrl":"https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0000.mp3",
            "businessLabels":[
                {
                    "businessDescription":"唱歌:唱歌:唱歌",
                    "businessDetail":{

                    },
                    "businessLabel1":"sing",
                    "businessLabel2":"changge",
                    "businessLabel3":"changge",
                    "confidenceLevel":2,
                    "probability":0.858334402569294
                }
            ],
            "allLabels":[],
            "riskLevel":"PASS",
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskDescription":"正常",
            "riskDetail":{
                "audioText":""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0001",
            "audioStarttime":10,
            "audioEndtime":20,
            "audioUrl":"https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0001.mp3",
            "riskLevel":"PASS",
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskDescription":"正常",
            "riskDetail":{
                "audioText":""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0002",
            "audioStarttime":20,
            "audioEndtime":30,
            "audioUrl":"https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0002.mp3",
            "riskLevel":"PASS",
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskDescription":"正常",
            "riskDetail":{
                "audioText":""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0003",
            "audioStarttime":30,
            "audioEndtime":40,
            "audioUrl":"https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0003.mp3",
            "riskLevel":"PASS",
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskDescription":"正常",
            "riskDetail":{
                "audioText":""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0004",
            "audioStarttime":40,
            "audioEndtime":50,
            "audioUrl":"https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0004.mp3",
            "riskLevel":"PASS",
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskDescription":"正常",
            "riskDetail":{
                "audioText":""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0005",
            "audioStarttime":50,
            "audioEndtime":60,
            "audioUrl":"https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0005.mp3",
            "riskLevel":"PASS",
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskDescription":"正常",
            "riskDetail":{
                "audioText":""
            }
        },
        {
            "requestId": "6a9cb980346dfea41111656a514e9109_a0006",
            "audioStarttime":60,
            "audioEndtime":60,
            "audioUrl":"https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0006.mp3",
            "riskLevel":"PASS",
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskDescription":"正常"
        }
    ],
    "audioTags":{
        "gender":{
            "label":"女性",
            "probability":95
        },
        "language":[
            {
                "confidence":0,
                "label":2
            },
            {
                "confidence":99,
                "label":0
            },
            {
                "confidence":0,
                "label":1
            }
        ],
        "song":0,
        "timbre":[
            {
                "label":"女性",
                "probability":95
            },
            {
                "label":"女王",
                "probability":12
            },
            {
                "label":"御姐",
                "probability":37
            },
            {
                "label":"少女",
                "probability":56
            },
            {
                "label":"大妈",
                "probability":67
            },
            {
                "label":"萝莉",
                "probability":24
            }
        ]
    }
}
```



## 同步接口

### 请求URL

| 集群 | URL                                                     | 支持语种 |
| ---- | ------------------------------------------------------- | -------- |
| 上海 | `http://api-audio-sh.fengkongcloud.com/audiomessage/v4` | 中文     |

### 字符编码

`UTF-8`

### 请求方法

`POST`

### 建议超时时长

10s

### 音频格式限制

`WAV`、`MP3`、`AAC`、`AMR`、`3GP`、`M4A`、`WMA`、`OGG`、`APE`、`FLAC`、`ALAC`、`WAVPACK`、`SILK_V3`等

### 音频时长限制

同步请求单条语音时长不长于60秒，否则报错，如需要审核超过60秒的语音数据，请使用异步接口

### 请求体限制

所有请求参数大小总和不能超过18M

### 请求参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型**    | **参数说明**         | **传入说明** | **规范**                                                     |
| :------------- | :---------- | :------------------- | :----------- | :----------------------------------------------------------- |
| accessKey      | string      | 公司密钥             | 必传参数     | 由数美提供                                                   |
| appId          | string      | 应用标识             | 必传参数     | 用于区分应用，需要联系数美服务开通，请使用数美单独提供的传值为准 |
| eventId        | string      | 事件标识             | 必传参数     | 用于区分场景数据，需要联系数美服务开通，请使用数美单独提供的传值为准 |
| type           | string      | 检测的风险类型       | 必传参数     | <p>AUDIOPOLITICAL：一号领导人声纹识别</p><p>POLITY：涉政识别</p><p>EROTIC：色情识别</p><p>ADVERT：广告识别</p><p>BAN：违禁识别</p><p>VIOLENT：暴恐识别</p><p>ANTHEN：国歌识别</p><p>MOAN：娇喘识别</p><p>DIRTY：辱骂识别</p><p>BANEDAUDIO：违禁歌曲</p><p>COPYRIGHTSONGS：版权歌曲</p><p>如需做组合识别，通过下划线连接即可，例如POLITY_EROTIC_MOAN涉政、色情和娇喘识别</p><p>建议传入：<br/>POLITY_EROTIC_MOAN_ADVERT</p> |
| businessType   | string      | 检测的业务标签类型   | businesstype和type必传其一 | 可选值：<br/>SING：唱歌识别<br/>LANGUAGE：语种识别<br/>GENDER：性别识别<br/>TIMBRE：音色识别 <br/>VOICE：人声属性<br/>MINOR：未成年识别<br/>AUDIOSCENE：声音场景<br/>AGE：年龄识别<br/>如需识别音色、唱歌、语种GENDER必传<br/>type和 businessType 必须填其一 |
| contentType    | string      | 待识别音频内容的格式 | 必传参数     | <p>可选值：</p><p>URL：识别内容为音频url地址；</p><p>RAW：识别内容为音频的base64编码数据</p> |
| content        | string      | 待识别的音频内容     | 必传参数     | <p>可以为url地址或者base64编码数据。</p><p>其中，base64编码数据上限15M，仅支持pcm、wav、mp3格式, 并且pcm格式数据必须采用16-bit小端序编码。推荐使用pcm、wav格式传输</p> |
| data           | json object | 本次请求相关信息     | 必传参数     | 最长1MB，[详见data参数](#data)                               |
| btId           | string      | 音频文件唯一标识     | 必传参数     | 唯一标识这条音频文件，方便将回调结果对应上，最高128位，不能重复 |

其中，<span id="data">data</span>的内容如下：

| **请求参数名** | **类型** | **参数说明**                                   | **传入说明** | **规范**                                                     |
| :------------- | :------- | :--------------------------------------------- | :----------- | :----------------------------------------------------------- |
| tokenId        | string   | 用户账号                                       | 非必传参数   | 用于用户行为分析，建议传入用户UID                            |
| receiveTokenId | string   | 私聊场景下消息接收者的tokenId                  | 非必传参数   | 由数字、字母、下划线、短杠组成的长度小于等于64位的字符串     |
| deviceId       | string   | 数美设备标识                                   | 非必传参数   |                                                              |
| ip             | string   | 发送该音频的用户公网ip地址                     | 非必传参数   | 支持传入IPV4或IPV6                                           |
| dataId         | string   | 数据标识                                       | 非必传参数   |                                                              |
| level          | int      | 用户等级，针对不同等级的用户可配置不同拦截策略 | 非必传参数   | 可选值：<br/>`0`：最低级用户，典型如新注册、完全不活跃或等级为0的用户等;<br/>`1`：较低级用户，典型如低活跃或低等级用户等；<br/>`2`：中等级用户，典型如具备一定活跃或等级中等的用户等；<br/>`3`：较高级用户，典型如高活跃或高等级用户等；<br/>`4`：最高级用户，典型如付费用户、VIP用户等 |
| gender         | string   | 用户性别                                       | 非必传参数   | 可选值：<br/>male男性 <br/>female女性                        |
| formatInfo     | string   | 音频数据格式                                   | 非必传参数   | 当音频内容格式为RAW时必须存在，可选值：pcm、wav、mp3、opus-gin |
| rate           | int      | 音频数据采样率                                 | 非必传参数   | 当音频数据格式为pcm时必须存在，范围限制8000-32000。          |
| track          | int      | 音频数据声道数                                 | 非必传参数   | <p>当音频数据格式为pcm时必须存在，可选值：</p><p>1: 单声道</p><p>2: 双声道</p> |
| returnAllText  | int      | 返回音频片段的等级                             | 非必传参数   | 可选值如下（默认为`0`）：<br/> `0`：返回风险片段识别结果<br/> `1`：返回所有片段识别结果<br/>该参数仅用于控制片段识别结果的返回， 不影响整体识别结果的返回。<br/>当选择“返回所有片段识别结果”时，片段识别结果中包含riskLevel为PASS、REVIEW和REJECT的片段识别结果；<br/>当选择“返回风险片段识别结果”时，片段识别结果中仅包含riskLevel为REVIEW和REJECT的片段识别结果；<br/>片段识别结果对应响应中的audioDetail字段。 |

### 返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明**                   | **是否必返** | **规范**                                                     |
| :----------------- | :----------- | :----------------------------- | :----------- | :----------------------------------------------------------- |
| requestId          | string       | 本次请求的唯一标识             | 是           |                                                              |
| code               | int          | 请求返回码                     | 是           | <p>1100：成功</p><p>1901：QPS超限</p><p>1902：参数不合法</p><p>1903：服务失败</p><p>1904：下载失败</p><p>1905：解码失败</p><p>9101：无权限操作</p> |
| message            | string       | 请求返回描述，和请求返回码对应 | 是           |                                                              |
| detail             | json         | 请求返回明细数据               | 否           | 当code码为1100时此参数必返                                   |

detail内容：

| **参数名**  | **类型**    | **参数说明**         | **是否必返** | **规范**                                                     |
| :---------- | :---------- | :------------------- | :----------- | :----------------------------------------------------------- |
| riskLevel   | string      | 当前事件的处置建议   | 是           | <p>可能返回值：<br/>PASS：通过</p><p>REVIEW：审核</p><p>REJECT：拒绝</p><p>建议：对接初期不直接使用结果，进行拦截尺度调优，符合预期后在进行使用</p> |
| audioText   | string      | 整段音频转译文本结果 | 是           |                                                              |
| audioTime   | int         | 整段音频的音频时长   | 是           | 单位秒                                                       |
| audioDetail | json_array  | 音频片段信息         | 是           | 回调的音频片段信息，[详见audioDetail参数](#audioDetail3)      |
| audioTags   | json_object | 音频标签             | 否           | 返回性别、音色、是否唱歌、语种等标签                         |
| auxInfo     | json_object | 辅助信息             | 否           |                                                              |

其中，<span id="audioDetail3">audioDetail</span>详细内容如下：

| **参数名**      | **类型**    | **参数说明**         | **是否必返** | **规范**                                                     |
| :-------------- | :---------- | :------------------- | :----------- | :----------------------------------------------------------- |
| requestId       | string      | 音频片段请求唯一标识 | 是           |                                                              |
| audioStarttime  | float       | 音频片段起始时间     | 是           | 相对音频开始的时间距离，单位是秒                             |
| audioEndtime    | float       | 音频片段结束时间     | 是           | 相对音频开始的时间距离，单位是秒                             |
| audioUrl        | string      | 音频片段链接         | 是           | mp3格式                                                      |
| businessLabels  | json_array  | 业务标签返回     | 否           | 全部业务标签，[详见businessLabels参数](#businessLabels3)     |
| allLabels       | json_array  | 风险标签返回     | 否           | 全部风险标签，[详见allLabels参数](#allLabels3)  
| riskLevel       | string      | 音频片段识别结果     | 是           | <p>可能返回值：<br/>PASS：通过</p><p>REVIEW：审核</p><p>REJECT：拒绝</p> |
| riskLabel1      | string      | 一级风险标签         | 是           |                                                              |
| riskLabel2      | string      | 二级风险标签         | 是           |                                                              |
| riskLabel3      | string      | 三级风险标签         | 是           |                                                              |
| riskDescription | string      | 风险原因             | 是           | 仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |
| riskDetail      | json_object | 风险详情             | 否           | [详见riskDetail参数](#riskDetail3)                            |

*<span id="businessLabels3">businessLabels</span>*数组中每一项具体参数如下：

| **参数名**          | **类型**    | **参数说明** | **是否必返** | **规范**                                     |
| ------------------- | ----------- | ------------ | ------------ | -------------------------------------------- |
| businessLabel1      | string      | 一级标签     | 是           | 一级标签                                     |
| businessLabel2      | string      | 二级标签     | 是           | 二级标签                                     |
| businessLabel3      | string      | 三级标签     | 是           | 三级标签                                     |
| businessDescription | string      | 标签描述     | 是           | 格式为"一级标签:二级标签:三级标签"的中文名称 |
| confidenceLevel     | int         | 置信等级     | 否           | 可选值在0～2之间，值越大，可信度越高         |
| probability         | float       | 置信度       | 否           | 可选值为0~1，值越大，可信度越高              |
| businessDetail      | json_object | 详细信息     | 否           | 保留字段                                     |

 *<span id="allLabels3">allLabels</span>*数组中每一项具体参数如下：

| **参数名**      | **类型**    | **参数说明** | **是否必返** | **规范**                                                     |
| --------------- | ----------- | ------------ | ------------ | ------------------------------------------------------------ |
| riskLabel1      | string      | 一级风险标签 | 是           | 一级风险标签                                                 |
| riskLabel2      | string      | 二级风险标签 | 是           | 二级风险标签                                                 |
| riskLabel3      | string      | 三级风险标签 | 是           | 三级风险标签                                                 |
| riskDescription | string      | 风险原因     | 是           | 风险原因，仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |
| riskLevel       | string      | 处置建议     | 是           | 可能返回值： PASS：通过REVIEW：审核REJECT：拒绝              |
| probability     | float       | 置信度       | 否           | 可选值在0～1之间，值越大，风险可能性越高，值越小，无风险可能性越高 |
| riskDetail      | json_object | 风险详情     | 否           | [详见riskDetail参数](#riskDetail3)                           |

其中，<span id="riskDetail3">riskDetail</span>详细内容如下：

| **参数名**   | **类型**   | **参数说明**             | **是否必返** | **规范**                                                     |
| :----------- | :--------- | :----------------------- | :----------- | :----------------------------------------------------------- |
| audioText    | string     | 音频转译文本的结果       | 否           |                                                              |
| riskSource   | int        | 风险来源                 | 否           | 可选值：<br/>1000：无风险<br/>1001：文本风险<br/>1003：音频风险 |
| matchedLists | json_array | 命中的客户自定义名单信息 | 否           | 命中客户自定义名单时返回，[详见matchedLists参数](#matchedLists) |
| riskSegments | json_array | 高风险内容片段           | 否           | 在涉政、暴恐、违禁、竞品、广告法等功能的时候存在，[详见riskSegments参数](#riskSegments) |

riskDetail中，<span id="matchedLists">matchedLists</span>详细内容如下：

| **参数名** | **类型**   | **参数说明**                 | **是否必返** | **规范**                |
| :--------- | :--------- | :--------------------------- | :----------- | :---------------------- |
| name       | string     | 客户自定义名单名称           | 是           |                         |
| words      | json_array | 命中的这个名单中的敏感词信息 | 是           | [详见words参数](#words) |

matchedLists中，<span id="words">words</span>详细内容如下：

| **参数名** | **类型**  | **参数说明**   | **是否必返** | **规范** |
| :--------- | :-------- | :------------- | :----------- | :------- |
| word       | string    | 敏感词         | 是           |          |
| position   | int_array | 敏感词所在位置 | 是           |          |

riskDetail中，<span id="riskSegments">riskSegments</span>详细内容如下：

| **参数名** | **类型**  | **参数说明**           | **是否必返** | **规范** |
| :--------- | :-------- | :--------------------- | :----------- | :------- |
| segment    | string    | 高风险内容片段         | 否           |          |
| position   | int_array | 高风险内容片段所在位置 | 否           |          |

其中，<span id="audioTags">audioTags</span>详细内容如下：

| **参数名** | **类型**    | **参数说明** | **是否必返** | **规范**                                                     |
| :--------- | :---------- | :----------- | :----------- | :----------------------------------------------------------- |
| gender     | json_object | 性别标签     | 否           | 当type取值包含GENDER时返回                                   |
| timbre     | json_array  | 音色标签     | 否           | 当type取值包含TIMBRE时返回                                   |
| song       | int         | 唱歌标签     | 否           | <p>当type取值包含SING时返回</p><p>可能取值：</p><p>0：没有唱歌</p><p>1：有唱歌</p> |
| language   | json_object | 语种识别     | 否           | type取值包含LANGUAGE时返回                                   |

audioTags中，gender详细内容如下：

| **参数名**  | **类型** | **参数说明**       | **是否必返** | **规范**                                |
| :---------- | :------- | :----------------- | :----------- | :-------------------------------------- |
| label       | string   | 性别标签名称       | 是           | <p>可能取值：</p><p>男性</p><p>女性</p> |
| probability | int      | 对应性别可能性大小 | 是           | 取值0-100，数值越高表示概率越大         |

audioTags中，timbre详细内容如下：

| **参数名**  | **类型** | **参数说明**           | **是否必返** | **规范**                                                     |
| :---------- | :------- | :--------------------- | :----------- | :----------------------------------------------------------- |
| label       | string   | 音色标签类别           | 是           | <p>可能取值：</p><p>大叔</p><p>青年</p><p>正太</p><p>老年</p><p>女王</p><p>御姐</p><p>少女</p><p>萝莉</p><p>大妈</p><p>男性</p><p>女性</p><p>无人声</p> |
| probability | int      | 对应音色标签可能性大小 | 是           | 取值0-100，数值越高表示概率越大                              |

audioTags中，language详细内容如下：

| **参数名** | **类型** | **参数说明**           | **是否必返** | **规范**                                                     |
| :--------- | :------- | :--------------------- | :----------- | :----------------------------------------------------------- |
| label      | int      | 语种识别类别标识       | 是           | <p>可能取值：</p><p>0:普通话</p><p>1:英语</p><p>2:粤语</p><p>3:藏语</p><p>4:维语</p><p>5:蒙语</p><p>6:朝鲜语</p><p>-1:其他语种</p> |
| confidence | int      | 对应音色标签可能性大小 | 是           | 取值0-100，数值越高表示概率越大                              |



## 示例

### 上传请求示例

```bash
curl -v 'http://api-audio-bj.fengkongcloud.com/audiomessage/v4' -d '{
    "accessKey": "*************",
    "appId": "default",
    "eventId": "default",
    "type": "POLITY_EROTIC",
    "businessType":"TIMBRE",
    "btId": "test1",
    "contentType": "URL",
    "content": "*************",
    "data": {
    		"returnAllText":1,
        "tokenId": "token-short"
        }
    }'
```

### 同步返回示例

```json
{
    "code":1100,
    "message":"成功",
    "requestId":"817c8509359500c898a762ffe93a582b",
    "btId":"1667392054643",
    "detail":{
        "audioDetail":[
            {
                "requestId":"817c8509359500c898a762ffe93a582b_a0000",
                "audioStarttime":0,
                "audioEndtime":10,
                "audioUrl":"http://voice-stream.oss-cn-hangzhou.aliyuncs.com/POST_AUDIO%2F20221102%2F817c8509359500c898a762ffe93a582b_a0000.mp3",
                "businessLabels":[
                {
                    "businessDescription":"唱歌:唱歌:唱歌",
                    "businessDetail":{

                    },
                    "businessLabel1":"sing",
                    "businessLabel2":"changge",
                    "businessLabel3":"changge",
                    "confidenceLevel":2,
                    "probability":0.858334402569294
                }],
                "allLabels":[],
                "riskLevel":"REJECT",
                "riskLabel1":"abuse",
                "riskLabel2":"buwenmingyongyu",
                "riskLabel3":"qingdubuwenmingyongyu",
                "riskDescription":"辱骂:不文明用语:轻度不文明用语",
                "riskDetail":{
                    "audioText":"超你今年十一月份十二月份你找个毛东啊我了下乡那就装了的下个箱子装了不不挂现在查整好你过两年都能你"
                }
            }
        ],
        "audioTags":{
            "gender":{
                "label":"女性",
                "probability":95
            },
            "language":[
                {
                    "confidence":0,
                    "label":2
                },
                {
                    "confidence":99,
                    "label":0
                },
                {
                    "confidence":0,
                    "label":1
                }
            ],
            "song":0,
            "timbre":[
                {
                    "label":"女性",
                    "probability":95
                },
                {
                    "label":"女王",
                    "probability":12
                },
                {
                    "label":"御姐",
                    "probability":37
                },
                {
                    "label":"少女",
                    "probability":56
                },
                {
                    "label":"大妈",
                    "probability":67
                },
                {
                    "label":"萝莉",
                    "probability":24
                }
            ]
        },
        "audioText":"超你今年十一月份十二月份你找个毛东啊我了下乡那就装了的下个箱子装了不不挂现在查整好你过两年都能你",
        "audioTime":10,
        "code":1100,
        "requestParams":{
            "channel":"TEST",
            "lang":"zh",
            "returnAllText":1,
            "tokenId":"test01"
        },
        "riskLevel":"REJECT"
    }
}
```

