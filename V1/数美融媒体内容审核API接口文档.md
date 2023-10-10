# **数美融媒体内容审核API接口文档**

------

版权所有 翻版必究

------

## 检测提交接口

### 接口说明

该接口为融媒体检测提交接口，支持http协议接口调用，目前仅支持异步返回

### 请求URL

| 集群名称 | url                                            |
| -------- | ---------------------------------------------- |
| 硅谷集群 | http://api-media-gg.fengkongcloud.com/media/v1 |
| 上海集群 | http://api-media-sh.fengkongcloud.com/media/v1 |
### 检测数据要求

请求体限制：所有请求参数大小总和不能超过10M

#### 文本要求

- 文本限制：单次请求\<10000字符，字段长度超过10000字符，会提示参数错误

#### 图片要求

- 图片支持类型：URL
- 图片支持格式：jpg，jpeg，png，webp，gif，tiff，tif，heif
- 图片大小: 单张30M，像素不低于20px\*20px，不超过6000px\*6000px，建议图片像素不小于256px\*256px，像素过低会影响识别效果
- 图片下载：下载时间限制为5秒内，如果下载时间超过5秒，接口检测失败
- 截帧说明：数美自动将GIF图、长图（长宽比大于4的图片）截帧过检，GIF图，长图均按照实际截图张数进行计费

#### 音频文件要求

- 音频支持类型：URL
- 音频支持格式：wav、mp3、aac、amr、3gp、m4a、wma、ogg、ape、flac、alac、wavpack、silk\_v3等
- 音频大小: 音频文件大小不超过550M
- 音频时长: 时长小于5小时

#### 视频文件要求

- 视频支持类型：URL
- 视频支持格式：AVI、FLV、MP4、MPG、WMV、MOV、WMA、RMVB、m3u8等
- 视频大小：视频文件大小不超过300M
- 视频时长: 时长小于2小时

#### 文档要求

- 文档支持类型：URL
- 文档支持格式： DOCX、PDF、DOC、XLS、XLSX、PPT、PPTX、PPS、PPSX、XLTX、XLTM、XLSB、TXT
- 文档大小：单文档\<500M，文本长度限制50w字，图片张数限制500张

###


### 请求参数

| **名称** | **类型** | **是否必填** | **说明** |
| --- | --- | --- | --- |
| accessKey | string | Y | 公司秘钥，联系数美提供 |
| appId | string | Y | 用于区分应用，需要联系数美服务开通，请使用数美单独提供的传值为准 |
| eventId | string | Y | 用于区分场景数据，需要联系数美服务开通，请使用数美单独提供的传值为准 |
| data | json\_object | Y | 请求数据 |
| callback | string | Y | 回调地址，用于接受审核结果 |
| passThrough | json\_object | N | 透传字段 |

data里包含的内容

| **名称** | **类型** | **是否必填** | **说明** |
| --- | --- | --- | --- |
| btId | string | Y | 作品Id |
| tokenId | string | N | 用户账号 |
| contents | json\_array | Y | 检测数据，类型为text时最多传入20条文本内容，每条最长10000字符；类型为image时最多传入50张图片url，每张最长512字符；类型为audio时最多传入5条语音url，每条最长512字符；类型为video时最多传入5条音视频url，每条最长512字符；类型为file时最多传入10个文档url，每个最长512字符_，_如果有一条不满足触发报错 |
| detectFrequency | float | N | 截帧频率间隔，单位为秒，取值范围为0.5~60s；如不传递默认5s截帧一次 |
| advancedFrequency | json\_object | N | 高级截帧间隔，单位为秒，此项填写，默认截帧策略失效，参数配置如{"durationPoints":[300,600],"frequencies":[1,5,10]}含义为：视频文件时长≤300s ——选用1s一截帧300s\<视频文件时长≤600s ——选用5s一截帧视频文件时长\>600s ——选用10s一截帧 |
| returnVideoAllImg | int | N | 选择返回视频截帧图片的等级：0：返回风险等级为非pass的图片；1：返回所有风险等级的图片。默认为1 |
| returnVideoAllAudio | int | N | 选择返回视频音频片段的等级：0：返回风险等级为非pass的音频片段1：返回所有风险等级的音频片段默认为1 |
| returnAudioAllText | int | N | 可选值如下（默认为1）：<br/>0：返回风险片段识别结果<br/>1：返回所有片段识别结果<br/>该参数仅用于控制片段识别结果的返回， 不影响整体识别结果的返回。<br/>当选择"返回所有片段识别结果"时，片段识别结果中包含riskLevel为PASS、REVIEW和REJECT的片段识别结果；当选择"返回风险片段识别结果"时，片段识别结果中仅包含riskLevel为REVIEW和REJECT的片段识别结果；片段识别结果对应响应中的audioDetail字段。 |

data 中，advancedFrequency的内容如下

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| durationPoints | float64[] | 视频时长区间分割 | 非必传参数 | 用于规定视频文件支持动态截帧频率的时长区间，数组最多为5个 |
| frequencies | float64[] | 视频时长区间对应的截帧频率 | 非必传参数 | 可设置范围为0.5~60秒，数组最多6个说明：frequencies数组设置的个数需要比durationPoints数组个数多1个，传错或传空报错返回1902 |

contents里包含的内容

| **名称** | **类型** | **是否必填** | **说明** |
| --- | --- | --- | --- |
| dataType | string | Y | 数据类型，可能取值： <br/> text：文本<br/> image：图片 <br/>audio：音频文件 <br/>video：视频文件 <br/>file：文档 |
| content | string | Y | 检测数据类型是文本时传入原始文本数据，检测数据类型是其他时传入数据url |
| dataId | string | N | 作品Id，用于查询作品 |
| btId | string | Y | 数据唯一标识，用于查询历史记录，注意：重复会报错 |
| txtType | string | N | 文本和文档中文本检测风险类型，可选值：TEXTRISK。dataType为text或者file时必传 |
| audioType | string | N | 音频和视频中音频部分检测风险类型，可选值：<br/>PORN：色情识别<br/>AD：广告识别<br/>POLITICAL：涉政识别<br/>ABUSE：辱骂识别<br/>MOAN：娇喘识别<br/>ANTHEN：国歌识别<br/>AUDIOPOLITICAL：一号领导人声纹识别<br/>NONE不审核视频中的音频,并且不支持传入音频文件审核<br/>如需做组合识别，通过下划线连接即可，例如POLITICAL\_PORN\_MOAN涉政、色情和娇喘识别dataType为audio或video时必传 |
| imgType | string | N | 图片、文档中图片和视频文件中截帧检测风险类型，可选值：POLITICS:涉政识别<br/>PORN:色情识别<br/>AD:广告识别<br/>LOGO:水印logo识别<br/>BEHAVIOR:不良场景识别，支持吸烟、喝酒、赌博、吸毒、避孕套和无意义画面<br/>OCR:图片中的OCR文字识别<br/>VIOLENCE:暴恐识别<br/>如果需要识别多个功能，通过下划线连接，如 POLITICS\_AD 用于涉政和广告组合识别dataType为image、video或file时必传 |
| fileFormat | string | N | 要检测的文档格式，传入数据为文档时必传，可选值：<br/>DOCX<br/>PDF<br/>DOC<br/>XLS<br/>XLSX<br/>PPT<br/>PPTX<br/>PPS<br/>PPSX<br/>XLTX<br/>XLTM<br/>XLSB<br/>XLSM<br/>TXT<br/>CSV<br/>EPUB<br/>若fileFormat与文档实际格式不一致，则返回报错参数错误 |

### 响应参数

| **名称** | **类型** | **是否必填** | **说明** |
| --- | --- | --- | --- |
| code | int | Y | 返回码 |
| message | string | Y | 返回码描述 |
| requestId | string | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存 |

## 异步回调结果

### 接口说明

任务审核中所有数据审核完成后，数美会将人工审核结果或者异步机器检测结果推送给客户，保证客户最快的获取结果，需客户在调用检测接口时设置回调地址callback参数，客户方需保证回调接收接口的可用性和稳定性，确保能正常接收推送过来的结果数据。推送策略：当用户收到推送结果，并返回HTTP状态码为200时，表示推送成功；否则系统将重复推送，推送5次，每次间隔20s。

### 机器检测结果

| **名称** | **类型** | **是否必填** | **说明** |
| --- | --- | --- | --- |
| btId | string | Y | 任务Id |
| requestId | string | Y | 流水号 |
| riskLevel | string | Y | 可能返回值：<br/>PASS：正常，建议直接放行<br/>REVIEW：可疑，建议人工审核<br/>REJECT：违规，建议直接拦截 |
| resultType | int | Y | 0：机审，1：人审 |
| details | json\_object | Y | 风险详情 |
| passThrough | json\_object | N | 透传字段 |

details的内容是：

| **名称** | **类型** | **是否必填** | **说明** |
| --- | --- | --- | --- |
| texts | json\_array | Y | 文本审核结果，如果没有传入，则为空 |
| images | json\_array | Y | 图片审核结果，如果没有传入，则为空 |
| audios | json\_array | Y | 音频文件审核结果，如果没有传入，则为空 |
| videos | json\_array | Y | 视频文件审核结果，如果没有传入，则为空 |
| files | json\_array | Y | 文档审核结果，如果没有传入，则为空 |

**texts** 里每个元素的内容

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | Y | 见响应码参数说明 |
| message | string | 返回码描述 | Y | 见响应码参数说明 |
| requestId | string | 该条数据任务的唯一标识 | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存 |
| dataId | string | 作品Id | N | 和传入的dataId对应 |
| btId | string | 文本唯一标识 | Y | 和传入的btId对应 |
| riskLevel | string | 处置建议 | Y | 可能返回值：<br/>PASS：正常，建议直接放行<br/>REVIEW：可疑，建议人工审核<br/>REJECT：违规，建议直接拦截 |
| riskLabel1 | string | 一级风险标签 | Y | 一级风险标签，当riskLevel为PASS时返回normal |
| riskLabel2 | string | 二级风险标签 | Y | 二级风险标签，当riskLevel为PASS时为空 |
| riskLabel3 | string | 三级风险标签 | Y | 三级风险标签，当riskLevel为PASS时为空 |
| riskDescription | string | 风险原因 | Y | 当riskLevel为PASS时为"正常" |
| disposal | json\_object | 处置结果 | N | 最外层的三级标签做映射之后的值放入disposal中，当未开启公司标签映射 或 未配置标签映射时不返回该字段 |
| riskDetail | json\_object | 风险详情 | Y | 风险详情，[详见](#riskDetail)[riskDetail参数](#riskDetail) |

其中，disposal的内容如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLevel | string | 处置建议 | N | 可能返回值：<br/>PASS：正常，建议直接放行<br/>REVIEW：可疑，建议人工审核<br/>REJECT：违规，建议直接拦截 |
| riskLabel1 | string | 映射后一级风险标签 | N | 一级风险标签，当riskLevel为PASS时返回normal |
| riskLabel2 | string | 映射后二级风险标签 | N | 二级风险标签，当riskLevel为PASS时为空 |
| riskLabel3 | string | 映射后三级风险标签 | N | 二级风险标签，当riskLevel为PASS时为空 |
| riskDescription | string | 映射后风险原因 | N | 当riskLevel为PASS时为"正常" |
| riskDetail | json\_object | 映射后风险详情 | N | 风险详情 |

其中，riskDetail的内容如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| matchedLists | json\_array | 辅助信息 | N | 命中的客户自定义名单列表。[详见](#matchedLists)[matchedLists参数](#matchedLists) |
| riskSegments | json\_array | 辅助信息，高风险内容片段检测文本包含涉政、暴恐、违禁、广告法等风险内容的时候存在 | N | [详见](#riskSegments)riskSegments参数 |

riskDetail中，matchedLists数组每个元素的内容如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 辅助信息 | N | 命中的名单名称 |
| words | json\_array | 辅助信息 | N | 命中的敏感词数组。[详见](#words)[words参数](#words) |

matchedLists中，words数组每个元素的内容如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string | 辅助信息 | N | 命中的敏感词 |
| position | int\_array | 辅助信息 | N | 敏感词所在位置 |

riskDetail中，riskSegments的内容如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segment | string | 辅助信息 | N | 高风险内容片段 |
| position | int\_array | 辅助信息 | N | 高风险内容片段所在位置 |

**images** 里每个元素的内容

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | Y | 见响应码参数说明 |
| message | string | 返回码描述 | Y | 见响应码参数说明 |
| requestId | string | 该条数据任务的唯一标识 | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存 |
| dataId | string | 作品Id | N | 和传入的dataId对应 |
| btId | string | 单条数据唯一标识 | Y | 和传入的btId对应 |
| riskLevel | string | 处置建议 | Y | 可能返回值：<br/>PASS：正常，建议直接放行<br/>REVIEW：可疑，建议人工审核<br/>REJECT：违规，建议直接拦截 |
| riskLabel1 | string | 一级风险标签 | Y | 一级风险标签，当riskLevel为PASS时返回normal |
| riskLabel2 | string | 二级风险标签 | Y | 当riskLevel为PASS时为空 |
| riskLabel3 | string | 三级风险标签 | Y | 当riskLevel为PASS时为空 |
| riskDescription | string | 风险原因 | Y | 当riskLevel为PASS时为正常 |
| disposal | json\_object | 处置结果 | N | 最外层的三级标签做映射之后的值放入disposal中，当未开启公司标签映射 或 未配置标签映射时不返回该字段 |
| riskDetail | json\_object | 风险详情 | Y | [详见](#riskDetail)riskDetail参数 |

其中，disposal的内容如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLevel | string | 处置建议 | N | 可能返回值：<br/>PASS：正常，建议直接放行<br/>REVIEW：可疑，建议人工审核<br/>REJECT：违规，建议直接拦截 |
| riskLabel1 | string | 映射后一级风险标签 | N | 一级风险标签，当riskLevel为PASS时返回normal |
| riskLabel2 | string | 映射后二级风险标签 | N | 二级风险标签，当riskLevel为PASS时为空 |
| riskLabel3 | string | 映射后三级风险标签 | N | 二级风险标签，当riskLevel为PASS时为空 |
| riskDescription | string | 映射后风险原因 | N | 当riskLevel为PASS时为"正常" |
| riskDetail | json\_object | 映射后风险详情 | N | 风险详情 |

其中，riskDetail结构如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| faces | json\_array | 返回图片中涉政人物的名称及位置信息 | N ||
| face\_num | int | 人脸数量 | N ||
| persons | json\_array | 仅当命中人像-多人时，数组元素会有多个，最多10个 | N ||
| person\_num | int | 人像数量 | N | 有且仅有人像-多人下返回 |
| objects | json\_array | 返回图片中物品或标志二维码的位置信息 | N | 数组仅会有一个元素 |
| ocrText | json\_object | 返回图片中违规文字相关信息，当请求参数type字段包含IMGTEXTRISK和ADVERT时存在 | N ||
| riskSource | int | 标识资源哪里违规 | Y | 标识风险结果的来源：<br/>1000：无风险<br/>1001：文字风险<br/>1002：视觉图片风险 |

riskDetail中，faces数组每个元素的内容如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| id | string | 人物编号 | N | 图片同一个位置下的人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID |
| name | string | 人物名称 | N | 能识别的公众人物名称 |
| location | int\_array | 人物位置信息，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 |N | |
| face\_ratio | float | 人脸占比 | N | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | N | 0～1之间的浮点数 |

riskDetail中，objects数组每个元素的内容如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| id | string | 编号，保证同一个位置下的物品在不同标签下的编号相同 | N | |
| name | string | 标识名称 | N ||
| location | int\_array | 标识位置信息，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/> 522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | N ||
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | N | 0～1之间的浮点数 |
| qrContent | string | 二维码的url信息 | N | 仅当命中二维码相关标签时返回 |

riskDetail中，persons数组每个元素的内容如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| id | string | 编号，保证同一个人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID | N ||
| person\_ratio | string | 人像在图中的占比 | N ||
| location | int\_array | 人像位置坐标 | N ||
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | N | 0～1之间的浮点数 |

riskDetail中，ocrText的内容如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** |
| --- | --- | --- | --- |
| text | string | 识别出的文字 | Y |
| matchedLists | json\_array | 命中的客户自定义名单列表，_matchedLists_数组每个元素的内容见上文_matchedLists_说明 | N |
| riskSegments | json\_array | 高风险片段内容，检测图片包含涉政、暴恐、违禁、广告法等风险内容的时候存在，_riskSegments_的每个元素的详细内容见上文_riskSegments_说明 | N |

**audios** 里每个元素的内容

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 状态码 | Y | 见响应码参数说明 |
| message | string | 返回码描述 | Y | 见响应码参数说明 |
| requestId | string | 该条数据任务的唯一标识 | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存 |
| dataId | string | 作品Id | N | 和传入的dataId对应 |
| btId | string | 单条数据唯一标识 | Y | 和传入的btId对应 |
| riskLevel | string | 当前事件的处置建议 | Y | 返回值：<br/>PASS：正常内容，建议直接放行<br/>REVIEW：可疑内容，建议人工审核<br/>REJECT：违规内容，建议直接拦截 |
| audioText | string | 整段音频转译文本结果 | Y ||
| audioTime | int | 整段音频的音频时长 | Y | 单位秒 |
| audioDetail | json\_array | 音频片段信息 | Y | 回调的音频片段信息，[详见](#audioDetail)[audioDetail参数](#audioDetail) |

其中，audioDetail详细内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 音频片段请求唯一标识 | Y ||
| audioStarttime | float | 音频片段起始时间 | Y | 相对音频开始的时间距离，单位是秒 |
| audioEndtime | float | 音频片段结束时间 | Y | 相对音频开始的时间距离，单位是秒 |
| audioUrl | string | 音频片段链接 | Y | mp3格式 |
| riskLevel | string | 音频片段识别结果 | Y | 可能返回值：<br/>PASS：正常内容，建议直接放行<br/>REVIEW：可疑内容，建议人工审核<br/>REJECT：违规内容，建议直接拦截|
| riskLabel1 | string | 一级风险标签 | Y ||
| riskLabel2 | string | 二级风险标签 | Y ||
| riskLabel3 | string | 三级风险标签 | Y ||
| riskDescription | string | 风险原因 | Y | 仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |
| riskDetail | json\_object | 风险详情 | N | [详见](#riskDetail)riskDetail参数 |

其中，riskDetail详细内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| audioText | string | 音频转译文本的结果 | N ||
| matchedLists | json\_array | 命中的客户自定义名单信息 | N | 命中客户自定义名单时返回，_matchedLists_数组每个元素的内容见上文_matchedLists_说明 |
| riskSegments | json\_array | 高风险内容片段 | N | 在涉政、暴恐、违禁、竞品、广告法等功能的时候存在，_riskSegments_的每个元素的详细内容见上文_riskSegments_说明 |

**videos** 里的每个元素的内容

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 状态码 | Y | 见响应码参数说明 |
| message | string | 返回码描述 | Y | 见响应码参数说明 |
| requestId | string | 该条数据任务的唯一标识 | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存 |
| dataId | string | 作品Id | N | 和传入的dataId对应 |
| btId | string | 视频唯一标识 | Y | 和传入的btId对应 |
| riskLevel | string | 风险级别，code为1100时存在 | Y | 返回值：<br/>PASS：正常内容，建议直接放行<br/>REVIEW：可疑内容，建议人工审核<br/>REJECT：违规内容，建议直接拦截 |
| frameDetail | json\_array | 风险详情 | N | 有风险片段或returnAllImg=1时返回，详见[frameDetail说明](#frameDetail) |
| audioDetail | json\_array | 音频片段信息 | N | 有风险片段或returnAllAudio=1时返回，详见[audioDetail说明](#audioDetail) |

其中，frameDetail数组中每个成员的具体内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| time | float | 截帧在视频文件中的时间，单位为秒 | Y | 截帧图片相对视频文件的时间 |
| requestId | string | 当前截帧片段的唯一标识 | Y ||
| imgUrl | string | 当前截帧的URL | Y ||
| imgText | string | 截帧图片OCR文本内容 | N | 截帧图片OCR文字识别，识别类型包含OCR时会有 |
| riskLevel | string | 当前截帧的处置建议 | Y | PASS：正常内容<br/>REVIEW：可疑内容<br/>REJECT：违规内容 |
| riskLabel1 | string | 各个一级标签之间是并列的关系，当riskLevel为PASS时返回normal | Y | 一级标签 |
| riskLabel2 | string | 二级标签归属于一级标签，当riskLevel为PASS时为空 | Y | 二级标签 |
| riskLabel3 | string | 三级标签归属于二级标签，当riskLevel为PASS时为空 | Y | 三级标签 |
| riskDescription | string | 标签解释 | Y | 对于命中用户自定义名单时返回：命中自定义名单；当riskLevel为PASS时返回:正常；其他情况展现形式为一级标签：二级标签：三级标签的中文名 |
| disposal | json\_object | 处置结果 | N | 最外层的三级标签做映射之后的值放入disposal中，当未开启公司标签映射 或 未配置标签映射时不返回该字段 |
| riskDetail | json\_object | 风险详情信息 | Y | 详见images里的_riskDetail_内容 |
| auxInfo | json\_object | 辅助信息 | Y | 一些辅助信息放在这里，详见[auxInfo说明](#auxInfo3) |

其中，disposal的内容如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLevel | string | 处置建议 | N | 可能返回值：<br/>PASS：正常，建议直接放行<br/>REVIEW：可疑，建议人工审核<br/>REJECT：违规，建议直接拦截 |
| riskLabel1 | string | 映射后一级风险标签 | N | 一级风险标签，当riskLevel为PASS时返回normal |
| riskLabel2 | string | 映射后二级风险标签 | N | 二级风险标签，当riskLevel为PASS时为空 |
| riskLabel3 | string | 映射后三级风险标签 | N | 二级风险标签，当riskLevel为PASS时为空 |
| riskDescription | string | 映射后风险原因 | N | 当riskLevel为PASS时为"正常" |
| riskDetail | json\_object | 映射后风险详情 | N | 风险详情 |

frameDetail中，auxInfo的内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| qrContent | string | 截帧图片二维码链接识别 | N | 截帧图片二维码链接识别，如有需要可联系数美开启<br/>注意：开启该功能后，只有完整，可以正常识别到的二维码才会返回且imgType传值需要包含AD |
| similarity | float | 当前截帧图片和上一帧截帧图片的相似度 | Y | 有图片则该字段就会返回，视频文件初始第一帧将比对纯黑背景图片 |

其中，audioDetail数组中每个成员的具体内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 流水号 | Y ||
| audioStarttime | float | 音频片段发生时间 | Y ||
| audioEndtime | float | 音频片段结束时间 | Y ||
| audioUrl | string | 音频片段地址 | Y ||
| audioText | string | 音转文文字 | N | 识别出文本会返回 |
| riskLevel | string | 当前事件的处置建议 | Y | PASS：正常内容<br/>REVIEW：可疑内容<br/>REJECT：违规内容 |
| riskLabel1 | string | 各个一级标签之间是并列的关系，riskLevel为PASS时返回normal | Y | 一级标签 |
| riskLabel2 | string | 二级标签归属于一级标签，当riskLevel为PASS时为空 | Y | 二级标签 |
| riskLabel3 | string | 三级标签归属于二级标签，当riskLevel为PASS时为空 | Y | 三级标签 |
| riskDescription | string | 标签解释 | Y | 格式为"一级风险标签：二级风险标签：三级风险标签"的中文名称；对于命中用户自定义名单时返回：命中自定义名单 |
| riskDetail | json\_object | 风险详情信息 | Y | 详见[riskDetail说明](#riskDetail2) |

audioDetail中，riskDetail的每个元素详细内容如下：

| **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskSource | int | 风险来源 | Y | 风险来源，可选值：<br/>1000：无风险<br/>1001：文本风险<br/>1002：视觉风险<br/>1003：音频风险 |
| audioText | string | 音频转译文本的结果 | N ||
| matchedLists | json\_array | 命中的客户自定义名单信息 | N | 命中客户自定义名单时返回，其他时不存在，详见[matchedLists说明](#matchedLists2) |
| riskSegments | json\_array | 高风险内容片段 | N | 在涉政、暴恐、违禁、竞品、广告法等功能的时候存在，详见[riskSegments说明](#riskSegments2) |

**files** 里每个元素的内容

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 响应码 | Y | 见响应码参数说明 |
| message | string | 响应码说明 | Y | 见响应码参数说明 |
| requestId | string | 该条数据任务的唯一标识 | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存 |
| dataId | string | 作品Id | N | 和传入的dataId对应 |
| btId | string | 请求标识 | Y | 和传入的btId对应 |
| riskLevel | string | 处置建议 | Y | 可能返回值：<br/>PASS：正常，建议直接放行<br/>REVIEW：可疑，建议人工审核<br/>REJECT：违规，建议直接拦截 |
| detail | json\_array | 风险详情 | Y | [详见](#Adetail)detail参数 |

其中detail字段如下：

| **参数名称** | **类型** | **是否必选** | **说明** |
| --- | --- | --- | --- |
| description | string | Y | 策略规则风险原因描述 |

**响应码参数**

| **code** | **message** |
| --- | --- |
| 1100 | 成功 |
| 1901 | QPS超限 |
| 1902 | 参数不合法 |
| 1903 | 服务失败 |
| 1911 | 下载失败 |
| 9101 | 无权限操作 |

### 人工审核结果

| **名称** | **类型** | **是否必填** | **说明** |
| --- | --- | --- | --- |
| btId | string | Y | 任务唯一标识 |
| requestId | string | Y | 请求唯一流水号 |
| riskLevel | string | Y | 可能返回值：<br/>PASS：正常<br/>REJECT：违规 |
| resultType | int | Y | 0：机审，1：人审 |
| details | json\_object | Y | 审核结果详情（需要后台配置开关，默认是关闭状态，里层的字段为空） |
| passThrough | json\_object | N | 透传字段 |

details里面的内容（返回所有的子项审核结果）

| **名称** | **类型** | **是否必填** | **说明** |
| --- | --- | --- | --- |
| texts | json\_array | Y | 文本审核结果，如果没有传入或者没有开启单项审核功能，则为空 |
| images | json\_array | Y | 图片审核结果，如果没有传入或者没有开启单项审核功能，则为空 |
| audios | json\_array | Y | 音频文件审核结果，如果没有传入或者没有开启单项审核功能，则为空 |
| videos | json\_array | Y | 视频文件审核结果，如果没有传入或者没有开启单项审核功能，则为空 |
| files | json\_array | Y | 文档审核结果，如果没有传入或者没有开启单项审核功能，则为空 |

texts里每个元素的内容：

| **参数名称** | **类型** | **是否必填** | **参数说明** |
| --- | --- | --- | --- |
| btId | string | Y | 文本唯一标识，和传入值一致 |
| requestId | string | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存 |
| riskLevel | string | Y | 可能返回值：<br/>PASS：正常<br/>REJECT：违规 |
| description | string | N | 风险原因，支持客户自定义二级原因配置；若存在二级原因，则通过"/"进行拼接，例如：色情/露点 |

images里每个元素的内容：

| **参数名称** | **类型** | **是否必填** | **参数说明** |
| --- | --- | --- | --- |
| btId | string | Y | 数据唯一标识 |
| requestId | string | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存 |
| riskLevel | string | Y | 可能返回值：<br/>PASS：正常<br/>REJECT：违规|
| description | string | N | 风险原因，支持客户自定义二级原因配置；若存在二级原因，则通过"/"进行拼接，例如：色情/露点 |

audios里每个元素的内容：

| **参数名称** | **类型** | **是否必填** | **参数说明** |
| --- | --- | --- | --- |
| btId | string | Y | 数据唯一标识 |
| requestId | string | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存 |
| riskLevel | string | Y | 可能返回值：<br/>PASS：正常<br/>REJECT：违规|
| description | string | N | 风险原因，支持客户自定义二级原因配置；若存在二级原因，则通过"/"进行拼接，例如：色情/露点 |
| audioDetail | json\_array | N | 音频片段详情，见下表说明 |

videos里每个元素的内容：

| **参数名称** | **类型** | **是否必填** | **参数说明** |
| --- | --- | --- | --- |
| btId | string | Y | 数据唯一标识 |
| requestId | string | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存 |
| riskLevel | string | Y | 可能返回值：<br/>PASS：正常<br/>REJECT：违规 |
| description | string | N | 风险原因，支持客户自定义二级原因配置；若存在二级原因，则通过"/"进行拼接，例如：色情/露点 |
| frameDetail | json\_array | N | 截帧片段详情，见下表说明 |
| audioDetail | json\_array | N | 音频片段详情，见下表说明 |

frameDetail内容

| **参数名称** | **类型** | **是否必填** | **参数说明** |
| --- | --- | --- | --- |
| requestId | string | Y | 唯一标识 |
| imgUrl | string | Y | 截帧图片地址，需要人工手动勾选，否则为空 |
| time | float | N | 截帧在视频文件中的时间，单位为秒 |
| imgText | string | N | 截帧图片OCR文字识别，识别类型包含OCR时会有 |

audioDetail内容

| **参数名称** | **类型** | **是否必填** | **参数说明** |
| --- | --- | --- | --- |
| requestId | string | Y | 唯一标识 |
| audioUrl | string | Y | 音频片段地址，需要人工手动勾选，否则为空 |
| audioStarttime | float | N | 音频片段结束时间，相对音频开始时间的距离，单位是秒 |
| audioEndtime | float | N | 音频片段结束时间，相对音频开始时间的距离，单位是秒 |

files里每个元素的内容：

| **参数名称** | **类型** | **是否必填** | **参数说明** |
| --- | --- | --- | --- |
| btId | string | Y | 数据唯一标识 |
| requestId | string | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存 |
| riskLevel | string | Y | 可能返回值：<br/>PASS：正常<br/>REJECT：违规 |
| description | string | N | 风险原因，支持客户自定义二级原因配置；若存在二级原因，则通过"/"进行拼接，例如：色情/露点 |

**返回参数**

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型** | **是否必选** | **说明** |
| --- | --- | --- | --- |
| code | int | Y | 返回码，成功返回1100，会判断请求是否成功；非1100认为失败 |
| message | string | Y | 返回码详情描述 |

## 示例

### 上传接口请求示例：

```json
{
    "accessKey":"xxxxxxx",
    "appId":"1580843258245054465",
    "callback":"https://callback/mapping",
    "data":{
        "btId":"1054078867_61ed6f2e801a648ea71748d0067ecb10",
        "contents":[
            {
                "audioType":"POLITICAL_PORN_AD_MOAN_ABUSE",
                "btId":"f338c6a3ce094188c5ad7379837d396e",
                "content":"https://video/1054078867_5191515227_56.mp4",
                "dataType":"video",
                "imgType":"POLITICS_AD_PORN"
            },
            {
                "btId":"a7b58140f37a7d5c3293233fede4fc29",
                "content":"10月5日（发布）河北石家庄，来自遵纪守法的快乐！男子酒后叫,10月5日（发布）河北石家庄，来自遵纪守法的快乐！男子酒后叫",
                "dataType":"text",
                "txtType":"TEXTRISK"
            },
            {
                "btId":"96273f7420ba4cd4e79f58706d8a5a90",
                "content":"https://image/557155_H169_sc.jpg",
                "dataType":"image",
                "imgType":"POLITICS_AD_PORN"
            }
        ],
        "detectFrequency":1,
        "returnAudioAllText":0,
        "returnVideoAllAudio":0,
        "returnVideoAllImg":0,
        "tokenId":"1054078867"
    },
    "eventId":"201",
    "passThrough":{
        "ack":"T6bRheiofkGwku6gXQGi",
        "taskIdImageUrMap":{
            "96273f7420ba4cd4e79f58706d8a5a90":"xxxxxx"
        }
    }
}
```

### 上传接口返回示例：

```json
{
    "code": 1100,
    "message": "成功",
    "requestId": "871fe334c04301fbfc1d2dce919383a5"
}
```

### 异步回调结果示例：

```json
{
    "btId":"1054078867_61ed6f2e801a648ea71748d0067ecb10",
    "details":{
        "audios":[

        ],
        "files":[

        ],
        "images":[

        ],
        "texts":[

        ],
        "videos":[
            {
                "btId":"f338c6a3ce094188c5ad7379837d396e",
                "code":1100,
                "frameDetail":[
                    {
                        "auxInfo":{
                            "similarity":0.63671875
                        },
                        "imgText":"星沙时报掌上星图@@咕男子酒后叫代驾送自己团家途中偶遇交警查酒驾骂大方打招呼",
                        "imgUrl":"http://sh-oss-jan1.cn-shanghai.oss.aliyuncs.com/POST_VIDEO%2FPOST_VIDEO_IMG%2F20231010%2F5ccdcac4d3c017ba3a28e6cec6497037_v0.jpg?Expires=1699514866\u0026OSSAccessKeyId=LTAI5tLsVBxJ8nhyy5gQVW3K\u0026Signature=LLOAfFq0Ii9aBbzrojeLMxuvuKA%3D",
                        "requestId":"5ccdcac4d3c017ba3a28e6cec6497037_v0",
                        "riskDescription":"涉政:政治象征:公检法制服",
                        "riskDetail":{
                            "ocrText":{
                                "text":"星沙时报掌上星图@@咕男子酒后叫代驾送自己团家途中偶遇交警查酒驾骂大方打招呼"
                            },
                            "riskSource":1002
                        },
                        "riskLabel1":"politics",
                        "riskLabel2":"zhengzhixiangzheng",
                        "riskLabel3":"gongjianfazhifu",
                        "riskLevel":"REJECT",
                        "time":0
                    },
                    {
                        "auxInfo":{
                            "similarity":0.7421875
                        },
                        "imgText":"星沙时报掌上TV：0米咕男子酒后叫代驾送自己回家途中偶遇交警查酒驾大方打招呼底气满满星沙时报IN行专HA TIME510月5日（发布时间）河北石家庄如有侵权 联系删除",
                        "imgUrl":"http://sh-oss-jan1.cn-shanghai.oss.aliyuncs.com/POST_VIDEO%2FPOST_VIDEO_IMG%2F20231010%2F5ccdcac4d3c017ba3a28e6cec6497037_v1.jpg?Expires=1699514866\u0026OSSAccessKeyId=LTAI5tLsVBxJ8nhyy5gQVW3K\u0026Signature=8DMpchD7vwYuTc%2B4CBq1voAA0gM%3D",
                        "requestId":"5ccdcac4d3c017ba3a28e6cec6497037_v1",
                        "riskDescription":"涉政:政治象征:公检法制服",
                        "riskDetail":{
                            "ocrText":{
                                "text":"星沙时报掌上TV：0米咕男子酒后叫代驾送自己回家途中偶遇交警查酒驾大方打招呼底气满满星沙时报IN行专HA TIME510月5日（发布时间）河北石家庄如有侵权 联系删除"
                            },
                            "riskSource":1002
                        },
                        "riskLabel1":"politics",
                        "riskLabel2":"zhengzhixiangzheng",
                        "riskLabel3":"gongjianfazhifu",
                        "riskLevel":"REJECT",
                        "time":1
                    },
                    {
                        "auxInfo":{
                            "similarity":0.875
                        },
                        "imgText":"星沙时报掌上星 米咕男子酒后叫代驾送自己回家途中偶遇交警查酒驾大方打招呼底气满满星沙时报XINGSHA TIME510月5日（发布时间）河北石家庄如有侵权 联系删除",
                        "imgUrl":"http://sh-oss-jan1.cn-shanghai.oss.aliyuncs.com/POST_VIDEO%2FPOST_VIDEO_IMG%2F20231010%2F5ccdcac4d3c017ba3a28e6cec6497037_v3.jpg?Expires=1699514866\u0026OSSAccessKeyId=LTAI5tLsVBxJ8nhyy5gQVW3K\u0026Signature=H2ZtcBLWeOl88wXTXsng4lt%2Btm4%3D",
                        "requestId":"5ccdcac4d3c017ba3a28e6cec6497037_v3",
                        "riskDescription":"涉政:政治象征:公检法制服",
                        "riskDetail":{
                            "ocrText":{
                                "text":"星沙时报掌上星 米咕男子酒后叫代驾送自己回家途中偶遇交警查酒驾大方打招呼底气满满星沙时报XINGSHA TIME510月5日（发布时间）河北石家庄如有侵权 联系删除"
                            },
                            "riskSource":1002
                        },
                        "riskLabel1":"politics",
                        "riskLabel2":"zhengzhixiangzheng",
                        "riskLabel3":"gongjianfazhifu",
                        "riskLevel":"REJECT",
                        "time":3
                    },
                    {
                        "auxInfo":{
                            "similarity":0.734375
                        },
                        "imgText":"星沙时报掌上星0米咕男子酒后叫代驾送自己回家途中偶遇交警查酒驾大方打招呼底气满满星沙时报XING5HA TIME5如有侵权 联系删除",
                        "imgUrl":"http://sh-oss-jan1.cn-shanghai.oss.aliyuncs.com/POST_VIDEO%2FPOST_VIDEO_IMG%2F20231010%2F5ccdcac4d3c017ba3a28e6cec6497037_v6.jpg?Expires=1699514867\u0026OSSAccessKeyId=LTAI5tLsVBxJ8nhyy5gQVW3K\u0026Signature=nr%2B3BQGOj6aqIrDkLm776HiTvxs%3D",
                        "requestId":"5ccdcac4d3c017ba3a28e6cec6497037_v6",
                        "riskDescription":"涉政:政治象征:公检法制服",
                        "riskDetail":{
                            "ocrText":{
                                "text":"星沙时报掌上星0米咕男子酒后叫代驾送自己回家途中偶遇交警查酒驾大方打招呼底气满满星沙时报XING5HA TIME5如有侵权 联系删除"
                            },
                            "riskSource":1002
                        },
                        "riskLabel1":"politics",
                        "riskLabel2":"zhengzhixiangzheng",
                        "riskLabel3":"gongjianfazhifu",
                        "riskLevel":"REJECT",
                        "time":6
                    },
                    {
                        "auxInfo":{
                            "similarity":0.6953125
                        },
                        "imgText":"星沙时报掌上星y.o米咕男子酒后叫代驾送自己回家途中偶遇交警查酒驾大方打招呼底气满满星沙时报INGSHA TIMEs如有侵权 联系删除",
                        "imgUrl":"http://sh-oss-jan1.cn-shanghai.oss.aliyuncs.com/POST_VIDEO%2FPOST_VIDEO_IMG%2F20231010%2F5ccdcac4d3c017ba3a28e6cec6497037_v9.jpg?Expires=1699514867\u0026OSSAccessKeyId=LTAI5tLsVBxJ8nhyy5gQVW3K\u0026Signature=%2Bixrpu9pU6VX4Bw%2BZbC%2BEbPukWc%3D",
                        "requestId":"5ccdcac4d3c017ba3a28e6cec6497037_v9",
                        "riskDescription":"涉政:政治象征:公检法制服",
                        "riskDetail":{
                            "ocrText":{
                                "text":"星沙时报掌上星y.o米咕男子酒后叫代驾送自己回家途中偶遇交警查酒驾大方打招呼底气满满星沙时报INGSHA TIMEs如有侵权 联系删除"
                            },
                            "riskSource":1002
                        },
                        "riskLabel1":"politics",
                        "riskLabel2":"zhengzhixiangzheng",
                        "riskLabel3":"gongjianfazhifu",
                        "riskLevel":"REJECT",
                        "time":9
                    },
                    {
                        "auxInfo":{
                            "similarity":0.62890625
                        },
                        "imgText":"星沙时报掌上星y 0米咕男子酒后叫代驾送自己回家途中偶遇交警查酒驾大方打招呼底气满满如有侵权联系删除",
                        "imgUrl":"http://sh-oss-jan1.cn-shanghai.oss.aliyuncs.com/POST_VIDEO%2FPOST_VIDEO_IMG%2F20231010%2F5ccdcac4d3c017ba3a28e6cec6497037_v11.jpg?Expires=1699514868\u0026OSSAccessKeyId=LTAI5tLsVBxJ8nhyy5gQVW3K\u0026Signature=niYP2Ps5UEWrsVClMzcKQaWw3TQ%3D",
                        "requestId":"5ccdcac4d3c017ba3a28e6cec6497037_v11",
                        "riskDescription":"涉政:政治象征:公检法制服",
                        "riskDetail":{
                            "ocrText":{
                                "text":"星沙时报掌上星y 0米咕男子酒后叫代驾送自己回家途中偶遇交警查酒驾大方打招呼底气满满如有侵权联系删除"
                            },
                            "riskSource":1002
                        },
                        "riskLabel1":"politics",
                        "riskLabel2":"zhengzhixiangzheng",
                        "riskLabel3":"gongjianfazhifu",
                        "riskLevel":"REJECT",
                        "time":11
                    },
                    {
                        "auxInfo":{
                            "similarity":0.6796875
                        },
                        "imgText":"星沙时报掌上星.o米咕男子酒后叫代驾送自己回家途中偶遇交警查酒驾大方打招呼底气满满星沙时报当事人史先生：喝酒肯定是不能开车的嘛如有侵权联系删除",
                        "imgUrl":"http://sh-oss-jan1.cn-shanghai.oss.aliyuncs.com/POST_VIDEO%2FPOST_VIDEO_IMG%2F20231010%2F5ccdcac4d3c017ba3a28e6cec6497037_v12.jpg?Expires=1699514868\u0026OSSAccessKeyId=LTAI5tLsVBxJ8nhyy5gQVW3K\u0026Signature=oxS9mKa%2BZRND8OKjVEWXcHHsjrc%3D",
                        "requestId":"5ccdcac4d3c017ba3a28e6cec6497037_v12",
                        "riskDescription":"涉政:政治象征:公检法制服",
                        "riskDetail":{
                            "ocrText":{
                                "text":"星沙时报掌上星.o米咕男子酒后叫代驾送自己回家途中偶遇交警查酒驾大方打招呼底气满满星沙时报当事人史先生：喝酒肯定是不能开车的嘛如有侵权联系删除"
                            },
                            "riskSource":1002
                        },
                        "riskLabel1":"politics",
                        "riskLabel2":"zhengzhixiangzheng",
                        "riskLabel3":"gongjianfazhifu",
                        "riskLevel":"REJECT",
                        "time":12
                    },
                    {
                        "auxInfo":{
                            "similarity":0.8203125
                        },
                        "imgText":"星沙时报掌上星 o米咕男子酒后叫代驾送自己回家途中偶遇交警查酒驾大方打招呼底气满满星沙时报当事人史先生：拍这个视频如有侵权联系删除",
                        "imgUrl":"http://sh-oss-jan1.cn-shanghai.oss.aliyuncs.com/POST_VIDEO%2FPOST_VIDEO_IMG%2F20231010%2F5ccdcac4d3c017ba3a28e6cec6497037_v13.jpg?Expires=1699514868\u0026OSSAccessKeyId=LTAI5tLsVBxJ8nhyy5gQVW3K\u0026Signature=qAD%2BuEk8iNovLM7hl%2Fi%2BA2WMFYM%3D",
                        "requestId":"5ccdcac4d3c017ba3a28e6cec6497037_v13",
                        "riskDescription":"涉政:政治象征:公检法制服",
                        "riskDetail":{
                            "ocrText":{
                                "text":"星沙时报掌上星 o米咕男子酒后叫代驾送自己回家途中偶遇交警查酒驾大方打招呼底气满满星沙时报当事人史先生：拍这个视频如有侵权联系删除"
                            },
                            "riskSource":1002
                        },
                        "riskLabel1":"politics",
                        "riskLabel2":"zhengzhixiangzheng",
                        "riskLabel3":"gongjianfazhifu",
                        "riskLevel":"REJECT",
                        "time":13
                    }
                ],
                "message":"success",
                "requestId":"5ccdcac4d3c017ba3a28e6cec6497037",
                "riskLevel":"REJECT"
            }
        ]
    },
    "passThrough":{
        "ack":"T6bRheiofkGwku6gXQGi",
        "taskIdImageUrMap":{
            "96273f7420ba4cd4e79f58706d8a5a90":"xxxxxx"
        }
    },
    "requestId":"871fe334c04301fbfc1d2dce919383a5",
    "resultType":0,
    "riskLevel":"REJECT"
}
```
