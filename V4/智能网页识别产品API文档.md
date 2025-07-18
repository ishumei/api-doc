



# 数美智能网页识别产品API接口文档



目录

- [数美智能网页识别产品API接口文档](#数美智能网页识别产品api接口文档)
  - [同步接口](#同步接口)
    - [请求参数](#请求参数)
      - [请求URL：](#请求url)
      - [字符编码格式：](#字符编码格式)
      - [请求方法：](#请求方法)
      - [建议超时时长：](#建议超时时长)
      - [请求参数：](#请求参数-1)
    - [返回结果](#返回结果)
      - [同步模式](#同步模式)
      - [回调模式](#回调模式)
      - [请求返回参数：](#请求返回参数)
      - [回调返回参数：](#回调返回参数)
    - [示例](#示例)
      - [同步模式](#同步模式-1)
        - [请求示例](#请求示例)
        - [响应示例](#响应示例)
      - [回调模式](#回调模式-1)
        - [请求示例](#请求示例-1)
        - [响应示例](#响应示例-1)
  - [异步接口](#异步接口)
    - [请求参数](#请求参数-2)
      - [请求URL：](#请求url-1)
      - [字符编码格式：](#字符编码格式-1)
      - [请求方法：](#请求方法-1)
      - [建议超时时长：](#建议超时时长-1)
      - [请求参数：](#请求参数-3)
    - [返回结果](#返回结果-1)
      - [请求返回参数](#请求返回参数-1)
    - [示例](#示例-1)
      - [请求示例](#请求示例-2)
      - [响应示例](#响应示例-2)
  - [结果查询接口](#结果查询接口)
    - [请求参数](#请求参数-4)
      - [请求URL：](#请求url-2)
      - [字符编码格式：](#字符编码格式-2)
      - [请求方法：](#请求方法-2)
      - [建议超时时长：](#建议超时时长-2)
      - [请求参数：](#请求参数-5)
    - [返回结果](#返回结果-2)
      - [请求返回参数](#请求返回参数-2)
    - [示例](#示例-2)
      - [请求示例](#请求示例-3)
      - [响应示例](#响应示例-3)
    
      

## 同步接口

### 请求参数

#### 请求URL：

| 集群 | URL | 
| --- | --- | 
| 北京 | `http://api-article-bj.fengkongcloud.com/v1/saas/anti_fraud/article` | 

#### 字符编码格式：

`UTF-8`字符集编码

#### 请求方法：

`POST`

#### 建议超时时长：

15s

#### 请求参数：

放在HTTP Body中，采用Json格式，Body大小不可超过3.5M，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥 | Y | 由数美提供 |
| type | string | 平台业务类型 | N | 可选值：<br/>`ZHIBO`:直播<br/>`ECOM`:电商<br/>`GAME`:游戏<br/>`NEWS`:新闻资讯<br/>`FORUM`:论坛<br/>`SOCIAL`:社交<br/>`NOVEL`:小说<br/> |
| imgType | string | 网页中的图片识别类型 | N | 可选值：<br/>`POLITICS`:涉政识别<br/>`PORN`:色情识别<br/>`AD`:广告识别<br/>`LOGO`:水印logo识别<br/>`BEHAVIOR`:不良场景识别，支持吸烟、喝酒、赌博、吸毒、避孕套和无意义画面<br/>`OCR`:图片中的OCR文字识别<br/>`VIOLENCE`:暴恐识别<br/>`NONE`:不需要识别图片<br/>如需做组合识别，通过下划线连接即可，例 如 `POLITICS_PORN_AD` 用于广告、色情和涉政识别<br/>不传时按涉政、色情、广告进行识别。<br/>注意：这里`POLITICS`实际上等价于以下两个类型：<br/>`PERSON`：涉政人脸识别<br/>`VIOLENCE`：暴恐识别 |
| txtType | string | 网页中的文字识别类型 | N | 可选值：<br/>POLITY：涉政检测<br/>VIOLENT：暴恐检测<br/>BAN：违禁检测<br/>EROTIC：色情检测<br/>DIRTY：辱骂检测<br/>ADVERT：广告检测<br/>PRIVACY：隐私检测<br/>ADLAW：广告法检测<br/>MEANINGLESS：无意义检测<br/>FRUAD：网络诈骗检测<br/>UNPOACH：高价值用户防挖检测<br/>TEXTMINOR: 未成年人内容检测<br/>TEXTRISK：常规风险检测（包含：涉政、暴恐、违禁、色情、辱骂、广告、隐私、广告法、无意义）<br/>以上type可以下划线组合，如：TEXTRISK_FRUAD；type间组合取并集，如：TEXTRISK_POLITY按照常规风险检测处理；不传时按传入常规风险处理。 |
| audioType | string |网页中的音频识别类型   | N            | 需要识别的违规类型，可选值：<br/>AUDIOPOLITICAL：一号领导人声纹识别<br/>POLITY：涉政识别<br/>ANTHEN：国歌识别<br/>EROTIC：色情<br/>DIRTY: 辱骂识别<br/>ADVERT：广告识别<br/>BAN：违禁识别<br/>VIOLENT：暴恐识别<br/>ADLAW：广告法识别<br/>MOAN：娇喘识别<br/>BANEDAUDIO：违禁歌曲<br/>COPYRIGHTSONGS：版权歌曲<br/>如需做组合识别，通过下划线连接即可，例如 POLITY_EROTIC_MOAN用于涉政、色情和娇喘识别。<br/> |
| videoImgType | string | 网页中视频截帧图片的识别类型 | N | 可选值：<br/>`POLITICS`：涉政识别, 这里POLITICS实际识别内容为涉政人物和暴恐<br/>`PERSON`：涉政人物识别<br/>`VIOLENCE`：暴恐识别<br/>`PORN`：色情&性感违规识别<br/>`AD`：广告识别<br/>`QR`：二维码识别<br/>`OCR`：图片文字违规识别<br/>`BEHAVIOR`：不良场景识别,支持吸烟、喝酒、赌博、吸毒、避孕套和无意义画面<br/>如果需要识别多个功能，通过下划线连接，如POLITY_QRCODE_ADVERT用于涉政、二维码和广告组合识别<br/>如果审核视频，该字段必传|
| videoAudioType | string | 网页中视频内音频的识别类型 | N | 可选值：<br/>`POLITICS`：涉政识别<br/>`PORN`：色情识别<br/>`AD`：广告识别<br/>`MOAN`：娇喘识别<br/>`ABUSE`：辱骂识别<br/>`ANTHEN`：国歌识别<br/>`AUDIOPOLITICAL`：声音涉政<br/>`NONE`:不检测音频<br/>如需做组合识别，通过下划线连接即可，例如POLITICAL_PORN_MOAN用于广告、色情和涉政识别<br/>不支持只审核视频中音频的情况|
| appId | string | 应用标识 | N | 用于区分相同公司的不同应用，该参数传递值可与数美服务协商 |
| callback | string | 回调http接口 | N | 当该字段非空时，服务将根据该字段回调通知用户审核结果；当传入fileFormat时必传 |
| callbackParam | json_object | 透传字段 | N | 当 callback 存在时可选，发送回调请求时服务将该字段内容同审核结果一起返回 |
| articleDoubleJumpConfig | json_object | 是否开启网页二跳审核方式 | N |  [详见articleDoubleJumpConfig参数](#articleDoubleJumpConfig) |
| articleScreenShotConfig | json\_object | 是否开启网页截屏审核方式 | N | [详见articleScreenShotConfig参数](#articleScreenShotConfig) |
| data | json\_object | 请求的数据内容 | Y | 最长1MB, [详见data参数](#data) |

其中，<span id="articleDoubleJumpConfig">articleDoubleJumpConfig</span>的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| isOpen | bool | 是否开启网页二跳审核 | N | 默认不开启 |


 其中，<span id="articleScreenShotConfig">articleScreenShotConfig</span>的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| isOpen | bool | 是否开启网页截图审核 | N | 默认不开启 |
| width | int | 截图的宽度 | N | 默认截图宽度1080 |
| height | int | 截图的高度 | N | 默认截图高度6480 |

 其中，<span id="data">data</span>的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| url | string | 要检测的网页链接 | N | 网址链接可下载，其中网址头部的content-type需为text/html。<br/>网址内容大小500m以内，文本长度限制50w字，图片张数限制500张，视频文件数限制50段。（url和text必须传其中一个）|
| text | string | 要检测的网页文本 | N | 纯文本内容审核，文本长度限制50w字。（url和text必须传其中一个）|
| tokenId | string | 客户端用户账号唯一标识，用于用户行为分析，建议传入用户UID | Y | 如果是网页识别场景，传入网页url即可 |
| channel | string | 业务场景 | N | 渠道表配置 |
| returnHtml | bool | 是否需要返回数美审核后高亮框处风险内容的html，用与展示给审核人员看 | N | 可选值:<br/>`true`<br/>`false`<br/>默认为false |
| nickname | string | 用户昵称，强烈建议传递此参数，几乎所有平台的恶意用户都会通过昵称散播垃圾信息，存在涉政违禁和导流信息等风险 | N |  |
| ip | string | 客户端ip地址，该参数用于IP维度的用户行为分析，同时可用于比对数美IP黑库 | N |  |
| detectFrequency | float | 视频中的截帧频率间隔，取值范围为0.5~60s；如不传递默认5s截帧一次 | N | 单位为秒s |
| passThrough | json_object | 透传参数，原样返回 | N |  |

### 返回结果

#### <span id="Ab1">同步模式</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int| 返回码 | Y | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9101`：无权限操作 |
| message | string | 返回码描述 | Y | 和code对应：<br/>成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>无权限操作 |
| requestId | string | 请求标识 | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存|
| score | int | 风险分数 | N | code为1100时存在，取值范围[0,1000]，分数越高风险越大 |
| riskLevel | string | 处置建议 | N | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| detail | json_object | 风险详情 | N | [详见detail参数](#Adetail) |
| status | int | 提示服务是否超时 | Y | 可能返回值：<br/>`0`：正常<br/>`501`：超时 |
| auxInfo | json_object | 辅助信息 | Y | [详见auxInfo参数](#AauxInfo)  |
| callbackParam | json_object | 透传字段	 | N	 |透传参数，原样返回  |

<span id="Adetail">其中detail字段如下：</span>

| 参数名称    | **类型**    | **是否必选** | **说明**                                                     |
| ----------- | ----------- | ------------ | ------------------------------------------------------------ |
| model       | string      | Y            | 规则标识                                                     |
| description | string      | Y            | 策略规则风险原因描述                                         |
| descriptionV2     | string      | N            | 新版策略规则风险原因描述<br/>注：该参数为新版API返回参数，过渡阶段只有新策略才会返回 |
| riskSummary | json object | N            | 风险摘要，目前包括各种风险类型的次数，如果type为NOVEL才返回<br/>[格式请见riskSummary结果详情](#AriskSummary) |
| riskDetail  | json array  | N            | 每一段内容的风险详情，如果type为NOVEL才返回。如果returnHtml参数为true只返回REJECT和REVIEW的风险内容片段，如果returnHtml参数为false会返回全部内容片段（包括REJECT和REVIEW和PASS）。<br/>[格式请见riskDetail结果详情](#AriskDetail) |
| riskHtml    | string      | N            | 风险内容标记的html,可嵌入需要展示的html页面，如果type为NOVEL且returnHtml参数为true才返回。<br/> |
| hits        | json_array  | N            | 网页命中信息，一般为空。命中详情在riskDetail中。             |
| passThrough | json_object | N            | 透传参数，原样返回             |
| doubleJumpDetails | json_array | N            | 开启二跳网址审核功能后返回，[详见doubleJumpDetails参数](#AdoubleJumpDetails) |

<span id="AdoubleJumpDetails">其中doubleJumpDetails字段如下：</span>

| 参数名称    | **类型**    | **是否必选** | **说明**                                                     |
| ----------- | ----------- | ------------ | ------------------------------------------------------------ |
| url       | string      | Y            | 二跳网址链接                                                     |
| model       | string      | Y            | 规则标识                                                     |
| description | string      | Y            | 策略规则风险原因描述                                         |
| descriptionV2     | string      | N            | 新版策略规则风险原因描述<br/>注：该参数为新版API返回参数，过渡阶段只有新策略才会返回 |
| riskSummary | json object | N            | 风险摘要，目前包括各种风险类型的次数，如果type为NOVEL才返回<br/>[格式请见riskSummary结果详情](#AriskSummary) |
| riskDetail  | json array  | N            | 每一段内容的风险详情，如果type为NOVEL才返回。如果returnHtml参数为true只返回REJECT和REVIEW的风险内容片段，如果returnHtml参数为false会返回全部内容片段（包括REJECT和REVIEW和PASS）。<br/>[格式请见riskDetail结果详情](#AriskDetail) |
| riskHtml    | string      | N            | 风险内容标记的html,可嵌入需要展示的html页面，如果type为NOVEL且returnHtml参数为true才返回。<br/> |
| hits        | json_array  | N            | 网页命中信息，一般为空。命中详情在riskDetail中。             |
| passThrough | json_object | N            | 透传参数，原样返回             |


其中，<span id="AriskSummary">riskSummary</span>内容是风险类型，具体如下：

| **参数名称**| **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskType | int| 对应riskType风险出现的次数 | N| 风险类型：<br/>`0`：正常<br/>`100`：涉政<br/>`200`：色情<br/>`210`：辱骂<br/>`250`：娇喘<br/>`300`：广告<br/>`400`：灌水<br/>`500`：无意义<br/>`600`：违禁<br/>`700`：黑名单<br/>`710`：白名单<br/>`800`：高危账号<br/>`900`：自定义 |

其中，<span id="AriskDetail">riskDetail</span>是json array，其中每一项是一个内容片段的风险详情，具体如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| type     | string       | 当前内容片段的类型 | Y            | 可选值：<br/>`text`：文本<br/>`img`：图片<br/>`video`：视频      |
| audioDetail     | json_array       | 当前音频片段处理详情,当传了audioType且有音频时返回 | N       | [详见audioDetail参数](#AaudioDetail)   |
| videoImgDetail     | json_array       | 当前视频片段中截帧图片详情，当type为video且审核视频截帧时返回 | N       | [详见videoImgDetail参数](#AvideoImgDetail)   |
| videoAudioDetail     | json_array       | 当前视频片段中音频详情，当type为video且审核音频时返回 | N       | [详见videoAudioDetail参数](#AvideoAudioDetail)   |
| content | string | 当前内容片段的内容 | Y           | text是文本内容，img是图片url |
| beginPosition | int       | 当前内容片段在输入中的起始位置，当type为`img`时该字段不返回 | N            | 检测出的文本内容，从0开始计算位置；文本切分后，每个片段的文本内容的首字在全局检测出文本中的位置 |
| endPosition | int     | 当前内容片段在输入中的结束位置，当type为`img`时该字段不返回 | N           | 检测出的文本内容，从0开始计算位置；文本切分后，每个片段的文本内容的末尾字在全局检测出文本中的位置 |
| description | string | 当前内容片段的风险描述 | Y           | 命中的对应名单中的所有敏感词                                 |
| riskLevel | string | 当前内容片段的处置建议 | Y           | 可选值：<br/>`PASS`：通过<br/>`REVIEW`：审核<br/>`REJECT`: 拒绝 |
| riskType | int | 当前内容片段的标识风险类型 | Y<br/>说明：当type为文本和图片时必返，当type为视频时为非必返 | 当type为文本时：<br/>`0`：正常<br/>`100`：涉政<br/>`200`：色情<br/>`210`：辱骂<br/>`300`：广告<br/>`400`：灌水<br/>`500`：无意义<br/>`600`：违禁<br/>`700`：黑名单<br/>`710`：白名单<br/>`800`：高危账号<br/>`900`：自定义<br/><br/>当type为图片时：<br/>`0`：正常<br/>`100`：涉政<br/>`200`：色情<br/>`210`：性感<br/>`300`：广告<br/>`310`：二维码<br/>`320`：水印<br/>`400`：暴恐<br/>`500`：违规<br/>`510`：不良场景<br/>`520`：未成年人<br/>`700`：黑名单<br/>`710`：白名单<br/>`800`：高危账号<br/>`900`：自定义 |
| riskTypeDec | string | riskType对应的描述 | N |  |
| model | string | 规则标识，用来标识文本命中的策略规则 | N |  |
| matchedList | string | 命中敏感词所在的名单名称（该参数仅在命中敏感词时存在） | N |  |
| matchedItem | string | 命中的具体敏感词（该参数仅在命中敏感词时存在） | N |  |
| matchedField | string | 标识昵称或文本内容命中了敏感词（该参数仅在命中敏感词时存在） | N | 可选值：<br/>`text`：文本命中敏感词<br/>`nickname`：昵称命中敏感词 |
| matchedDetail | json_array | 命中的名单详情 | N | [详见详细结构](#matcheddetail) |
| index | int | 当前处理的片段索引 | Y | 索引不区分文本和图片 |
| keywordsPosition | string | 命中的敏感词位置 | N | 在该段中的位置 |
| text | string | 图片中的ocr内容 | N | 图片片段识别出ocr内容时会返回该字段 |

其中，<span id="AaudioDetail">audioDetail</span>结构如下:
| **参数名称**     | **类型**   | **是否必选** | **说明**                                                     |
| :--------------- | :--------- | :----------- | :----------------------------------------------------------- |
| requestId        | string     | Y            | 风险音频片段请求唯一标识                                     |
| audioStarttime   | string     | Y            | 风险音频片段在音频中的起始时间，单位秒                       |
| audioEndtime     | string     | Y            | 风险音频片段在音频中的结束时间，单位秒                       |
| audioModel       | string     | Y            | 规则标识，命中的最高优先级规则标识（废弃字段，不建议使用 ）  |
| audioUrl         | string     | Y            | 风险音频片段地址，MP3格式                                    |
| audioText        | string     | Y            | 音频片段转译的文本内容                                       |
| riskLevel        | string     | Y            | 识别结果，可能返回值： <br/>REJECT：违规内容<br/>REVIEW：疑似违规内容<br/>PASS：正常内容<br/> |
| businessLabels   | json_array | N            | 业务标签返回 [详见businessLabels参数](#businessLabels)       |
| riskType         | int        | N            | 标识风险类型，可能取值:<br/>风险类型，静音时不返回，可能取值:<br/>0:正常<br/>100:涉政/国歌<br/>110:暴恐<br/>200:色情<br/>210:辱骂<br/>250:娇喘<br/>260:一号领导声纹<br/>270:人声属性<br/>280:违禁歌曲<br/>300:广告<br/>340:网络诈骗<br/>400:灌水<br/>500:无意义<br/>520:未成年人<br/>600:违禁<br/>700:其他<br/>720:黑账号<br/>730:黑IP<br/>800:高危账号<br/>900:自定义<br/> |
| audioMatchedItem | string     | N            | 音频中可能出现的敏感词                                       |
| matchedList      | string     | N            | 命中敏感词所在的名单名称                                     |
| matchedDetail    | string     | N            | 命中所有名单详情 [详见matchedDetail](#matchedDetail1)        |
| description      | string     | Y            | 音频片段风险原因描述，仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |

其中，<span id="AvideoImgDetail">videoImgDetail</span>结构如下:

| **参数名称**   | **类型**     | **参数说明** | **是否必返** | **规范**                                                     |
| -------------- | ------------ | ------------ | ------------ | ------------------------------------------------------------ |
| time         | float       | 该图片在视频中的位置     | Y            | 截帧图片相对视频文件的时间                                  |
| riskLevel   | string | 当前截帧的处置建议             | Y            | 可选值：<br/>`PASS`：通过<br/>`REVIEW`：审核<br/>`REJECT`: 拒绝 |
| imgText   | string       | 截帧图片OCR文本内容        | N            | 截帧图片OCR文字识别，识别类型包含OCR时会返回             |
| riskType   | int       | 截帧图片风险类型        | Y         | 返回值：<br/>`0`： 正常<br/>`100`：涉政<br/>`200`：色情<br/>`210`：性感<br/>`300`：广告<br/>`310`：二维码<br/>`320`：水印<br/>`400`：暴恐<br/>`500`：违规<br/>`510`：不良场景<br/>`700`：黑名单<br/>`710`：白名单<br/>`800`：高危账号<br/>`900`：自定义        |
| matchedList   | string       | 命中敏感词所在的名单名称（该参数仅在命中敏感词时存在）  | N            |  |
| matchedltem   | string       | 命中的具体敏感词（该参数仅在命中敏感词时存在）        | N            |  |
| riskSource   | int       | 风险来源           | Y            | 可返值：<br/>`1000`：无风险 <br/>`1001`：文字风险 <br/>`1002`：视觉图片风险    |

其中，<span id="AvideoAudioDetail">videoAudioDetail</span>结构如下:

| **参数名称**   | **类型**     | **参数说明** | **是否必返** | **规范**                                                     |
| -------------- | ------------ | ------------ | ------------ | ------------------------------------------------------------ |
| audio_starttime         | float       | 音频片段发生时间     | N            |  |
| audio_endtime           | float       | 音频片段结束时间     | N            |  |
| riskLevel   | string | 当前截帧的处置建议             | Y            | 可选值：<br/>`PASS`：通过<br/>`REVIEW`：审核<br/>`REJECT`: 拒绝 |
| audioText   | string       | 返回音转文文字        | N            | 截帧图片OCR文字识别，识别类型包含OCR时会有             |
| riskType   | int       | 风险类型        | Y            | 返回值：<br/>`0`：正常<br/>`100`：涉政/国歌<br/>`110`: 暴恐<br/>`200`：色情<br/>`210`：辱骂<br/>`250`：娇喘<br/>`260`：一号领导人声纹<br/>`300`：广告<br/>`400`：灌水<br/>`500`：无意义<br/>`600`: 违禁<br/>`700`：其他<br/>`720`：黑账号<br/>`730`：黑IP<br/>`800`：高危账号<br/>`900`：自定义             |
| audio_matchedItem   | string       | 违规音频敏感词内容 （该参数仅在命中敏感词时存在）        | N            |  |
| riskSource   | int       | 风险来源           | Y            | 可返值：<br/>`1000`：无风险 <br/>`1001`：文字风险 <br/>`1003`：语音风险      |

其中，<span id="matcheddetail">matchedDetail</span>结构如下:

| **参数名称**   | **类型**     | **参数说明** | **是否必返** | **规范**                                                     |
| -------------- | ------------ | ------------ | ------------ | ------------------------------------------------------------ |
| listId         | string       |              | Y            | 返回码                                                       |
| matchedFiled   | string_array |              | N            | 标识昵称或文本内容命中了敏感词（该参数仅在命中敏感词时存在），可选值：<br/>text：文本命中敏感词<br/>nickname：昵称命中敏感词 |
| name           | string       |              | Y            | 命中敏感词所在的名单名称                                     |
| organization   | string       |              | N            | 命中名单所属的公司标识，其中“GLOBAL”为全局名单               |
| words          | string_array |              | N            | 命中的对应名单中的所有敏感词                                 |
| wordPositions | json_array   |              | N            | 命中的对应名单中的所有敏感词及位置。[详见wordPositions](#words) |

<span id="words">wordPositions</span>中的每一项内容：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范**       |
| ------------ | -------- | ------------ | ------------ | -------------- |
| word         | string   | 辅助信息     | N            | 命中的敏感词   |
| position     | string   | 辅助信息     | N            | 敏感词所在位置 |

<span id="AauxInfo">其中auxInfo字段如下：</span>

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范**       |
| ------------ | -------- | ------------ | ------------ | -------------- |
| textNum      | int   | 当前请求中的字符数，与计费数目一致     | Y            | 当前请求中的字符数，其中字符数包括汉字，英文，标点符号，空格等   |
| imgNum       | int   | 当前请求中的图片数，与计费数目一致     | Y            | 当前请求中的图片数，如遇动图会截取3帧；如遇长图会进行切分 |
| videoNum       | int   | 当前请求中的视频数     | Y            | 遗留历史兼容字段，不建议使用 |
| audioNum       | int   | 当前请求中的音频数     | Y            | 审核音频的数量 |
| audioDuration       | int   | 当前请求中的音频时长     | Y            | 审核音频的时长 |
| billingImgNum       | int   | 当前请求中的视频里的截帧图片数，与计费数目一致    | Y            | 审核视频时，视频文件中截帧图片数 |
| billingAudioDuration       | int   | 当前请求中的视频里的音频时长，单位是秒，与计费数目一致     | Y            | 审核视频时，如果视频文件中音轨数据和视频时长不一致，计费时长以实际的音轨时长为准；例如可能会存在没有音轨的情况，计费时长就为0 |

#### 回调模式

如果在请求参数中指定了 callback，系统会自动推送机审结果至指定URL

#### 请求返回参数：

| **参数名称** | **类型**    | **参数说明** | **是否必返** | **规范**                                                     |
| ------------ | ----------- | ------------ | ------------ | ------------------------------------------------------------ |
| code         | int         | 返回码       | Y            | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9100`：余额不足<br/>`9101`：无权限操作 |
| message      | string      | 返回码描述   | Y            | 和code对应：<br/>成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>余额不足<br/>无权限操作 |
| requestId    | string      | 请求标识     | Y            | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存  |
| score        | int         | 风险分数     | N            | code为1100时存在，取值范围[0,1000]，分数越高风险越大         |
| riskLevel    | string      | 处置建议     | N            | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| detail       | json_object | 风险详情     | N            | [详见detail参数](#Adetail)                                   |

#### 回调返回参数：

回调返回结构[同同步请求响应](#Ab1)；返回HTTP状态码为200时，表示推送成功；否则系统将进行最多8次推送。

### 示例

#### 同步模式

##### 请求示例

```json
{"accessKey":"xxxxxxxx",
 "type":"NOVEL", 
 "appId":"xxxx",
 "data":{
     "tokenId":"xxxx",
     "contents":"xxxx",
     "returnHtml":true
  }
 }
```

##### 响应示例

```json
{
    "code":1100,
    "message":"成功",
    "requestId":"918123911b23cf4077119dd58c8edf91",
    "score":700,
    "riskLevel":"REJECT",
    "detail":{
        "description":"图片违规",
        "hits":[

        ],
        "model":"M04301",
        "riskDetail":[
            {
                "beginPosition":1235,
                "content":"为了防范电信网络诈骗，如网民接到962110电话，请立即接听",
                "description":"包含联系方式",
                "endPosition":1264,
                "index":287,
                "model":"",
                "riskLevel":"REJECT",
                "riskType":300,
                "type":"text"
            },
            {
                "content":"http://icon.qiantucdn.com/img/searchnew/wechat-g.png",
                "description":"二维码",
                "index":281,
                "model":"",
                "riskLevel":"REJECT",
                "riskType":300,
                "type":"image"
            }
        ],
        "riskHtml":"xxxx",
        "riskSummary":{
            "300":5
        }
    },
    "status":0,
    "auxInfo":{
         "textNum":"100",
         "imgNum":"10"
    }
}
```



#### 回调模式

##### 请求示例

```json
{"accessKey":"xxxxxxxx",
 "type":"NOVEL",
 "appId":"xxxx",
 "callback":"",
 "callbackParam":{
     "callbackId":"Id123"
 },
 "data":{
     "tokenId":"xxxx",
     "contents":"xxxx",
     "returnHtml":true
 }
}
```

##### 响应示例

```json
{
  "code": 1100,
  "message": "成功",
  "requestId": "xxxxxxxxxxxxxxxxxx",
  "score": 0,
  "riskLevel": "PASS",
  "detail": {
    "description": "正常",
    "model": "M1000",
    "riskType": 0
  },
  "status": 0
}

回调结果：

{
    "code":1100,
    "message":"成功",
    "requestId":"xxxxxxxxxxxxxxxxxx",
    "score":700,
    "riskLevel":"REJECT",
    "callbackParam":{
        "callbackId":"Id123"
    }
    "detail":{
        "description":"图片违规",
        "hits":[

        ],
        "model":"M04301",
        "riskDetail":[
            {
                "beginPosition":1235,
                "content":"为了防范电信网络诈骗，如网民接到962110电话，请立即接听",
                "description":"包含联系方式",
                "endPosition":1264,
                "index":287,
                "model":"",
                "riskLevel":"REJECT",
                "riskType":300,
                "type":"text"
            },
            {
                "content":"http://icon.qiantucdn.com/img/searchnew/wechat-g.png",
                "description":"二维码",
                "index":281,
                "model":"",
                "riskLevel":"REJECT",
                "riskType":300,
                "type":"image"
            }
        ],
        "riskHtml":"xxxx",
        "riskSummary":{
            "300":5
        }
    },
    "status":0,
    "auxInfo":{
         "textNum":"100",
         "imgNum":"10"
    }
}
```





## 异步接口

### 请求参数

#### 请求URL：

| 集群 | URL                                                          | 支持产品列表 |
| ---- | ------------------------------------------------------------ | ------------ |
| 北京 | `http://api-article-bj.fengkongcloud.com/v1/saas/anti_fraud/article_async` | 网页产品     |

#### 字符编码格式：

`UTF-8`字符集编码

#### 请求方法：

`POST`

#### 建议超时时长：

5s

#### 请求参数：

放在HTTP Body中，采用Json格式，Body大小不可超过3.5M，具体参数如下：

| **请求参数名** | **类型**     | **参数说明**         | **是否必传** | **规范**                                                     |
| -------------- | ------------ | -------------------- | ------------ | ------------------------------------------------------------ |
| accessKey      | string       | 接口认证密钥         | Y            | 由数美提供                                                   |
| type           | string       | 平台业务类型         | N            | 可选值：<br/>`ZHIBO`:直播<br/>`ECOM`:电商<br/>`GAME`:游戏<br/>`NEWS`:新闻资讯<br/>`FORUM`:论坛<br/>`SOCIAL`:社交<br/>`NOVEL`:小说<br/> |
| imgType        | string       | 网页中的图片识别类型 | N            | 可选值：<br/>`POLITICS`:涉政识别<br/>`PORN`:色情识别<br/>`AD`:广告识别<br/>`LOGO`:水印logo识别<br/>`BEHAVIOR`:不良场景识别，支持吸烟、喝酒、赌博、吸毒、避孕套和无意义画面<br/>`OCR`:图片中的OCR文字识别<br/>`VIOLENCE`:暴恐识别<br/>`NONE`:不需要识别图片<br/>如需做组合识别，通过下划线连接即可，例 如 POLITICS_PORN_AD 用于广告、色情和涉政识别 |
| txtType | string | 网页中的文字识别类型 | N | 可选值：<br/>POLITY：涉政检测<br/>VIOLENT：暴恐检测<br/>BAN：违禁检测<br/>EROTIC：色情检测<br/>DIRTY：辱>骂检测<br/>ADVERT：广告检测<br/>PRIVACY：隐私检测<br/>ADLAW：广告法检测<br/>MEANINGLESS：无意义检测<br/>FRUAD：网络诈骗检测<br/>UNPOACH：高价值用户防挖检测<br/>TEXTMINOR: 未成年人内容检测<br/>TEXTRISK：常规风险检测（包含：涉政、暴恐、违禁、色情、辱骂、广告、隐私、广告法、无意义）<br/>以上type可以下划线组>合，如：TEXTRISK_FRUAD；type间组合取并集，如：TEXTRISK_POLITY按照常规风险检测处理；不传时按传入常规风险处理。 |
| appId          | string       | 应用标识             | N            | 用于区分相同公司的不同应用，该参数传递值可与数美服务协商     |
| articleScreenShotConfig | json\_object | 是否开启网页截屏审核方式 | N | [详见articleScreenShotConfig参数](#articleScreenShotConfig) |
| articleDoubleJumpConfig | json_object | 是否开启网页二跳审核方式 | N |  [详见articleDoubleJumpConfig参数](#articleDoubleJumpConfig) |
| data           | json\_object | 请求的数据内容       | Y            | 最长1MB, [详见data参数](#data)                               |

 其中，data的内容同同步接口：

### 返回结果

#### 请求返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范**                                                     |
| ------------ | -------- | ------------ | ------------ | ------------------------------------------------------------ |
| code         | int      | 返回码       | Y            | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9100`：余额不足<br/>`9101`：无权限操作 |
| message      | string   | 返回码描述   | Y            | 和code对应：<br/>成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>余额不足<br/>无权限操作 |
| requestId    | string   | 请求标识     | Y            | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存  |



### 示例

#### 请求示例

```json
{
    "accessKey":"xxxxxxxx",
    "type":"NOVEL",
    "callback":"",
    "txtType":"",
    "imgType":"",
    "requestId":"",
    "tokenId":"",
    "serviceId":"POST_ARTICLE",
    "appId":"",
    "data":{
        "channel":"",
        "contents":"<div>凡涉及到发进来客人爱斯达克解放军阿卡丽色绕口令加凉开水的解放路口而爱上对方<img src=\"http://www.chedan5.net/upload/article/202012/05/1854275fcb66e37e370KkBbBv_thumb.jpg\" alt=\"图片加载失败\"></div>",
        "returnHtml":true,
        "itemId":"CHAPTER_CONTENT_0",
        "tokenId":"49930319"
    }
}
```

#### 响应示例

```json
{
    "code":1100,
    "message":"\u6210\u529f",
    "requestId":"tye7ert12asdfasdf31236444442333312"
}
```



## 结果查询接口

该接口用于查询机审和人审识别结果

### 请求参数

#### 请求URL：

| 集群 | URL                                                          | 支持产品列表 |
| ---- | ------------------------------------------------------------ | ------------ |
| 北京 | `http://api-article-bj.fengkongcloud.com/v1/saas/anti_fraud/article/query` | 网页产品     |

#### 字符编码格式：

`UTF-8`字符集编码

#### 请求方法：

`POST`

#### 建议超时时长：

1s

#### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明**   | **是否必传** | **规范**                                        |
| -------------- | -------- | -------------- | ------------ | ----------------------------------------------- |
| accessKey      | string   | 接口认证密钥   | Y            | 由数美提供                                      |
| requestIds     | array    | 机器审核流水号 | Y            | 最多支持10条 字符串数组 item 为数美返回的流水号 |

### 返回结果

#### 请求返回参数

| **请求参数名** | **类型**   | **参数说明** | **是否必传** | **规范**                      |
| -------------- | ---------- | ------------ | ------------ | ----------------------------- |
| code           | int        | 返回码       | Y            |                               |
| message        | string     | 返回码描述   | Y            |                               |
| requestId      | string     | 请求唯一标识 | N            | code不等于1100时返回          |
| contents       | json array | 内容         | N            | code等于1100时返回[详见contents内容](#contents) |

<span id="contents">其中contents组成如下</span>

| **请求参数名** | **类型**    | **参数说明**                 | **是否必传** | **规范**                                                     |
| -------------- | ----------- | ---------------------------- | ------------ | ------------------------------------------------------------ |
| requestId      | string      | 机器审核流水号               | Y            |                                                              |
| humanResult    | json object | 人审结果，人审完成后才会存在 | N            |                                                              |
| machineResult  | json object | 机审结果，机审完成后才会存在 | N            | [参考同步接口返回字段](#Ab1)                                 |
| mergeResult    | json_object | 统一人审和机审结果           | N            | 优先返回人审结果，如果人审结果没有，返回机审结果，如果都没有不存在 |

其中，humanResult/mergeResult的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范**                                     |
| -------------- | -------- | ------------ | ------------ | -------------------------------------------- |
| riskLevel      | string   | 处置指令     | Y            | 建议取值：<br/>`REJECT`:删除<br/>`PASS`:发布 |

### 示例

#### 请求示例

```json
{
    "accessKey":"xxxxxxxx",
    "requestIds":[
        "tye7ert12asdfasdf31236633346662333312"
    ]
}
```

#### 响应示例

```json
{
    "code":1100,
    "message":"正常",
    "contents":[
        {
            "requestId":"tye7ert12asdfasdf31236633346662333312",
            "machineResult":{
                "code":1100,
                "detail":{
                    "description":"文本违规",
                    "hits":[
                        {
                            "description":"广告：广告：广告",
                            "descriptionV2":"广告：广告：广告",
                            "model":"MA000007020001001",
                            "riskLevel":"REJECT",
                            "riskType":300,
                            "score":650,
                            "type":"text"
                        }
                    ],
                    "model":"M03101",
                    "riskDetail":[
                        {
                            "beginPosition":0,
                            "content":"凡涉及到发进来客人爱斯达克解放军阿卡丽色绕口令加凉开水的解放路口而爱上对方",
                            "description":"广告：广告：广告",
                            "endPosition":36,
                            "index":0,
                            "keywordsPosition":"8",
                            "matchedDetail":[
                                {
                                    "listId":"9da189a5bf1919d242d745f19ea3e5d7",
                                    "matchedFiled":[
                                        "text"
                                    ],
                                    "name":"原文名单",
                                    "organization":"12312312",
                                    "wordPositions":[
                                        {
                                            "position":"8",
                                            "word":"人"
                                        }
                                    ],
                                    "words":[
                                        "人"
                                    ]
                                },
                                {
                                    "listId":"cf5c160194954812fc279d3045fe3237",
                                    "matchedFiled":[
                                        "text"
                                    ],
                                    "name":"同音",
                                    "organization":"RlokQwRlVjUrTUlkIqOg",
                                    "wordPositions":[
                                        {
                                            "position":"22",
                                            "word":"零"
                                        }
                                    ],
                                    "words":[
                                        "零"
                                    ]
                                },
                                {
                                    "listId":"d75d056d88702cbf6198e2aa82eb0fdc",
                                    "matchedFiled":[
                                        "text"
                                    ],
                                    "name":"涉政_国家机构_军队",
                                    "organization":"GLOBAL",
                                    "wordPositions":[
                                        {
                                            "position":"13,14,15",
                                            "word":"解放軍"
                                        }
                                    ],
                                    "words":[
                                        "解放軍"
                                    ]
                                },
                                {
                                    "listId":"70cecfffebf31c2ac2b612cc3b6af142",
                                    "matchedFiled":[
                                        "text"
                                    ],
                                    "name":"涉政词库3",
                                    "organization":"RlokQwRlVjUrTUlkIqOg",
                                    "wordPositions":[
                                        {
                                            "position":"0",
                                            "word":"解放軍"
                                        }
                                    ],
                                    "words":[
                                        "解放軍"
                                    ]
                                }
                            ],
                            "matchedItem":"人",
                            "matchedList":"原文名单",
                            "model":"MA000007020001001",
                            "riskLevel":"REJECT",
                            "riskType":300,
                            "type":"text"
                        }
                    ],
                    "riskHtml":"\u003cdiv class=\"list\" style=\"position: relative;\"\u003e\u003cdiv class=\"list\" style=\"position: relative;\"\u003e\u003cdiv style=\"width: 70%;text-align: left;padding: 0px 20px;min-height: 20px;margin-bottom: 5px;line-height: 1.5;word-break: break-all;display:inline-block;border: 1px solid red;padding:10px;\"\u003e\u003cspan\u003e凡涉及到发进来客\u003cspan style=\"color:red;font-weight:bold;display:inline-block;\"\u003e人\u003c/span\u003e爱斯达克解放军阿卡丽色绕口令加凉开水的解放路口而爱上对方\u003c/span\u003e\u003c/div\u003e\u003cdiv style=\"position: absolute;top: 50%;left: 75%;transform: translateY(-50%);width: 20px;height: 20px;background-color: rgb(255, 255, 255);color:red;\"\u003e\u0026gt;\u003c/div\u003e\u003cdiv style=\"width: 30%;position: absolute;right: 20px;top: 50%;transform: translateY(-50%);text-align: center;display:inline-block;\"\u003eMA000007020001001(广告：广告：广告-人)\u003c/div\u003e\u003c/div\u003e\u003cdiv class=\"list\" style=\"position: relative;\"\u003e\u003cdiv style=\"width: 70%;text-align: left;padding: 0px 20px;min-height: 20px;margin-bottom: 5px;line-height: 1.5;word-break: break-all;display:inline-block;border: 1px ;padding:10px;text-align:center;\"\u003e\u003cimg src=\"http://www.chedan5.net/upload/article/202012/05/1854275fcb66e37e370KkBbBv_thumb.jpg\" alt=\"图片加载失败\"/\u003e\u003c/div\u003e\u003c/div\u003e",
                    "riskSummary":{
                        "300":1
                    }
                },
                "message":"正常",
                "requestId":"tye7ert12asdfasdf31236633346662333312",
                "riskLevel":"REJECT",
                "score":700,
                "auxInfo":{
                    "textNum":"100",
                    "imgNum":"10"
                }
            },
            "mergeResult":{
                "riskLevel":"REJECT"
            }
        }
    ]
}
```



