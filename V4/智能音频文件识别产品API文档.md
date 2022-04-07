# 智能音频文件识别产品API文档

- - - - -

***版权所有 翻版必究***

- - - - -

目录

- [智能音频文件识别产品API文档](#智能音频文件识别产品api文档)
  - [**音频文件上传请求**](#音频文件上传请求)
    - [**请求URL：**](#请求url)
    - [**字符编码：**](#字符编码)
    - [**请求方法：**](#请求方法)
    - [**建议超时时长：**](#建议超时时长)
    - [**请求参数**](#请求参数)
      - [data](#data)
    - [**同步返回参数**](#同步返回参数)
    - [**回调结果**](#回调结果)
      - [audioDetail](#audiodetail)
      - [riskDetail](#riskdetail)
      - [matchedLists](#matchedlists)
      - [riskSegments](#risksegments)
      - [audioTags](#audiotags)
  - [**主动查询结果**](#主动查询结果)
    - [**请求URL：**](#请求url-1)
    - [**字符编码：**](#字符编码-1)
    - [**请求方法：**](#请求方法-1)
    - [**建议超时时长：**](#建议超时时长-1)
    - [**请求参数**](#请求参数-1)
    - [**返回参数**](#返回参数)
      - [audioDetail](#audiodetail-1)
      - [riskDetail](#riskdetail-1)
      - [matchedLists](#matchedlists-1)
      - [words](#words)
      - [riskSegments](#risksegments-1)
      - [audioTags](#audiotags-1)
  - [**示例**](#示例)
    - [**上传请求示例**](#上传请求示例)
    - [**同步返回示例**](#同步返回示例)
    - [**回调返回示例**](#回调返回示例)
    - [**主动查询结果请求示例**](#主动查询结果请求示例)
    - [**主动查询结果返回示例**](#主动查询结果返回示例)
- [**Demo**](#demo)

## **音频文件上传请求**

### **请求URL：**

| 集群 | URL                                              | 支持产品列表 |
| ---- | ------------------------------------------------ | ------------ |
| 北京 | `http://api-audio-bj.fengkongcloud.com/audio/v4` | 中文音频文件 |
| 上海 | `http://api-audio-sh.fengkongcloud.com/audio/v4` | 中文音频文件 |
| 硅谷 | `http://api-audio-gg.fengkongcloud.com/audio/v4` |              |

### **字符编码：**

`UTF-8`

### **请求方法：**

`POST`

### **建议超时时长：**

1s

### **请求参数**

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型**    | **参数说明**         | **传入说明** | **规范**                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| :------------- | :---------- | :------------------- | :----------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| accessKey      | string      | 公司密钥             | 必传参数     | 由数美提供                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| appId          | string      | 应用标识             | 必传参数     | <p>用于区分应用，可选值如下：</p><p>default：默认应用</p><p></p><p>额外应用值需数美单独分配提供</p>                                                                                                                                                                                                                                                                                                                                                 |
| eventId        | string      | 事件标识             | 必传参数     | <p>用于区分场景数据<br/>可选值：</p><p>default：默认事件</p><p>audiobook：有声书</p><p>education：教育音频</p><p>game：游戏语音房</p><p>live：秀场直播</p><p>ecommerce：电商直播</p><p>voiceroom：交友语音房</p><p>private：私密语音聊天</p><p>other：其他</p>                                                                                                                                                                                      |
| type           | string      | 检测的风险类型       | 必传参数     | <p>AUDIOPOLITICAL：一号领导人声纹识别</p><p>POLITICS：涉政识别</p><p>PORN：色情识别</p><p>AD：广告识别<br/>ANTHEN：国歌识别</p><p>MOAN：娇喘识别</p><p>ABUSE：辱骂识别<br/>GENDER：性别识别</p><p>TIMBRE：音色识别</p><p>SING：唱歌识别</p><p>LANGUAGE：语种识别</p><p></p><p>如需识别音色，唱歌,语种GENDER必传</p><p>如需做组合识别，通过下划线连接即可，例如POLITICS_PORN_MOAN涉政、色情和娇喘识别</p><p>建议传入：<br/>POLITICS_PORN_AD_MOAN</p> |
| contentType    | string      | 待识别音频内容的格式 | 必传参数     | <p>可选值：</p><p>URL：识别内容为音频url地址；</p><p>RAW：识别内容为音频的base64编码数据</p>                                                                                                                                                                                                                                                                                                                                                        |
| content        | string      | 待识别的音频内容     | 必传参数     | <p>可以为url地址或者base64编码数据。</p><p>其中，base64编码数据上限6M，仅支持pcm、wav、mp3格式, 并且pcm格式数据必须采用16-bit小端序编码。推荐使用pcm、wav格式传输</p>                                                                                                                                                                                                                                                                               |
| data           | json object | 本次请求相关信息     | 必传参数     | 最长1MB，[详见data参数](#data)                                                                                                                                                                                                                                                                                                                                                                                                                      |
| btId           | string      | 音频文件唯一标识     | 必传参数     | 唯一标识这条音频文件，方便将回调结果对应上，最高128位，不能重复                                                                                                                                                                                                                                                                                                                                                                                     |
| callback       | string      | 回调http接口         | 非必传参数   | 当该字段非空时，服务将根据该字段回调通知用户审核结果                                                                                                                                                                                                                                                                                                                                                                                                |

#### data

其中，data的内容如下：

| **请求参数名** | **类型** | **参数说明**       | **传入说明** | **规范**                                                                                   |
| :------------- | :------- | :----------------- | :----------- | :----------------------------------------------------------------------------------------- |
| tokenId        | string   | 用户账号           | 非必传参数   | 用于用户行为分析，建议传入用户UID                                                          |
| formatInfo     | string   | 音频数据格式       | 非必传参数   | 当音频内容格式为RAW时必须存在，可选值：pcm、wav、mp3                                       |
| rate           | int      | 音频数据采样率     | 非必传参数   | 当音频数据格式为pcm时必须存在，范围限制8000-32000。                                        |
| track          | int      | 音频数据声道数     | 非必传参数   | <p>当音频数据格式为pcm时必须存在，可选值：</p><p>1: 单声道</p><p>2: 双声道</p>             |
| returnAllText  | int      | 返回音频片段的等级 | 非必传参数   | <p>0：返回风险等级为非pass的音频片段</p><p>1：返回所有风险等级的音频片段</p><p>默认为0</p> |
| lang           | string   | 音频流语言类型     | 非必传参数   | 可选值如下，（默认值为zh）：<br/>zh：中文<br/>en：英文<br/>ar：阿拉伯语                    |

### **同步返回参数**

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明**                   | **是否必返** | **规范**                                                                                                 |
| :----------------- | :----------- | :----------------------------- | :----------- | :------------------------------------------------------------------------------------------------------- |
| requestId          | string       | 本次请求的唯一标识             | 是           |                                                                                                          |
| code               | int          | 请求返回码                     | 是           | <p>1100：成功</p><p>1901：QPS超限</p><p>1902：参数不合法</p><p>1903：服务失败</p><p>9101：无权限操作</p> |
| message            | string       | 请求返回描述，和请求返回码对应 | 是           |                                                                                                          |

### **回调结果**

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名**    | **类型**    | **参数说明**                   | **是否必返** | **规范**                                                                                                                                                                  |
| :------------ | :---------- | :----------------------------- | :----------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| requestId     | string      | 本次请求的唯一标识             | 是           |                                                                                                                                                                           |
| btId          | string      | 音频唯一标识                   | 是           |                                                                                                                                                                           |
| code          | int         | 请求返回码                     | 是           | <p>1100：成功</p><p>1901：QPS超限</p><p>1902：参数不合法</p><p>1903：服务失败</p><p>9101：无权限操作</p><p>除message和requestId之外的字段，只有当code为1100时才会存在</p> |
| message       | string      | 请求返回描述，和请求返回码对应 | 是           |                                                                                                                                                                           |
| riskLevel     | string      | 当前事件的处置建议             | 是           | <p>可能返回值：<br/>PASS：通过</p><p>REVIEW：审核</p><p>REJECT：拒绝</p><p>建议：对接初期不直接使用结果，进行拦截尺度调优，符合预期后在进行使用</p>                       |
| audioText     | string      | 整段音频转译文本结果           | 是           |                                                                                                                                                                           |
| audioTime     | int         | 整段音频的音频时长             | 是           | 单位秒                                                                                                                                                                    |
| audioDetail   | json_array  | 音频片段信息                   | 是           | 回调的音频片段信息，[详见audioDetail参数](#audioDetail)                                                                                                                   |
| audioTags     | json_object | 音频标签                       | 否           | 返回性别、音色、是否唱歌等标签                                                                                                                                            |
| requestParams | json_object | 透传字段                       | 是           | 返回data下所有字段                                                                                                                                                        |

#### audioDetail

其中，audioDetail详细内容如下：

| **参数名**      | **类型**    | **参数说明**     | **是否必返** | **规范**                                                                 |
| :-------------- | :---------- | :--------------- | :----------- | :----------------------------------------------------------------------- |
| audioStarttime  | float       | 音频片段起始时间 | 是           | 相对音频开始的时间距离，单位是秒                                         |
| audioEndtime    | float       | 音频片段结束时间 | 是           | 相对音频开始的时间距离，单位是秒                                         |
| audioUrl        | string      | 音频片段链接     | 是           | mp3格式                                                                  |
| riskLevel       | string      | 音频片段识别结果 | 是           | <p>可能返回值：<br/>PASS：通过</p><p>REVIEW：审核</p><p>REJECT：拒绝</p> |
| riskLabel1      | string      | 一级风险标签     | 是           |                                                                          |
| riskLabel2      | string      | 二级风险标签     | 是           |                                                                          |
| riskLabel3      | string      | 三级风险标签     | 是           |                                                                          |
| riskDescription | string      | 风险原因         | 是           |                                                                          |
| riskDetail      | json_object | 风险详情         | 否           | [详见riskDetail参数](#riskDetail)                                        |

#### riskDetail

其中，riskDetail详细内容如下：

| **参数名**   | **类型**   | **参数说明**             | **是否必返** | **规范**                                                                                |
| :----------- | :--------- | :----------------------- | :----------- | :-------------------------------------------------------------------------------------- |
| audioText    | string     | 音频转译文本的结果       | 否           |                                                                                         |
| matchedLists | json_array | 命中的客户自定义名单信息 | 否           | 命中客户自定义名单时返回，[详见matchedLists参数](#matchedLists)                         |
| riskSegments | json_array | 高风险内容片段           | 否           | 在涉政、暴恐、违禁、竞品、广告法等功能的时候存在，[详见riskSegments参数](#riskSegments) |

#### matchedLists

riskDetail中，matchedLists详细内容如下：

| **参数名** | **类型**   | **参数说明**                 | **是否必返** | **规范** |
| :--------- | :--------- | :--------------------------- | :----------- | :------- |
| name       | string     | 客户自定义名单名称           | 是           |          |
| words      | json_array | 命中的这个名单中的敏感词信息 | 是           |          |

matchedLists中，words详细内容如下：

| **参数名** | **类型**  | **参数说明**   | **是否必返** | **规范** |
| :--------- | :-------- | :------------- | :----------- | :------- |
| word       | string    | 敏感词         | 是           |          |
| position   | int_array | 敏感词所在位置 | 是           |          |

#### riskSegments

riskDetail中，riskSegments详细内容如下：

| **参数名** | **类型**  | **参数说明**           | **是否必返** | **规范** |
| :--------- | :-------- | :--------------------- | :----------- | :------- |
| segment    | string    | 高风险内容片段         | 否           |          |
| position   | int_array | 高风险内容片段所在位置 | 否           |          |

#### audioTags

其中，audioTags详细内容如下：

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

| **参数名**  | **类型** | **参数说明**           | **是否必返** | **规范**                                                                                                                               |
| :---------- | :------- | :--------------------- | :----------- | :------------------------------------------------------------------------------------------------------------------------------------- |
| label       | string   | 音色标签类别           | 是           | <p>可能取值：</p><p>大叔音</p><p>青年音</p><p>正太音</p><p>老年音</p><p>女王音</p><p>御姐音</p><p>少女音</p><p>萝莉音</p><p>大妈音</p> |
| probability | int      | 对应音色标签可能性大小 | 是           | 取值0-100，数值越高表示概率越大                                                                                                        |

audioTags中，language详细内容如下：

| **参数名**  | **类型** | **参数说明**           | **是否必返** | **规范**                                                   |
| :---------- | :------- | :--------------------- | :----------- | :--------------------------------------------------------- |
| label       | int      | 语种识别类别标识       | 是           | <p>可能取值：</p><p>0:普通话</p><p>1:英语</p><p>2:粤语</p> |
| probability | int      | 对应音色标签可能性大小 | 是           | 取值0-100，数值越高表示概率越大                            |

## **主动查询结果**

### **请求URL：**

| 集群 | URL                                                    | 支持产品列表 |
| ---- | ------------------------------------------------------ | ------------ |
| 北京 | `http://api-audio-bj.fengkongcloud.com/query_audio/v4` | 中文音频文件 |
| 上海 | `http://api-audio-sh.fengkongcloud.com/query_audio/v4` | 中文音频文件 |

### **字符编码：**

`UTF-8`

### **请求方法：**

`POST`

### **建议超时时长：**

1s

### **请求参数**

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明**     | **传入说明** | **规范**                               |
| :------------- | :------- | :--------------- | :----------- | :------------------------------------- |
| accessKey      | string   | 公司密钥         | 必传参数     | 由数美提供                             |
| btId           | string   | 音频文件唯一标识 | 必传参数     | 唯一标识这条音频文件，用于查询识别结果 |

### **返回参数**

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名**  | **类型**    | **参数说明**                   | **是否必返** | **规范**                                                                                                                                                                                                              |
| :---------- | :---------- | :----------------------------- | :----------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| requestId   | string      | 本次请求的唯一标识             | 是           |                                                                                                                                                                                                                       |
| btId        | string      | 音频唯一标识                   | 是           |                                                                                                                                                                                                                       |
| code        | int         | 请求返回码                     | 是           | <p>1100：成功</p><p>1101：正在处理中</p><p>1901：QPS超限</p><p>1902：参数不合法</p><p>1903：解码失败</p><p>9100：余额不足</p><p>9101：无权限操作</p><p>除message和requestId之外的字段，只有当code为1100时才会存在</p> |
| message     | string      | 请求返回描述，和请求返回码对应 | 是           |                                                                                                                                                                                                                       |
| riskLevel   | string      | 当前事件的处置建议             | 是           | <p>可能返回值：<br/>PASS：通过</p><p>REVIEW：审核</p><p>REJECT：拒绝</p><p>建议：对接初期不直接使用结果，进行拦截尺度调优，符合预期后在进行使用</p>                                                                   |
| audioText   | string      | 整段音频转译文本结果           | 是           |                                                                                                                                                                                                                       |
| audioTime   | int         | 整段音频的音频时长             | 是           | 单位秒                                                                                                                                                                                                                |
| audioDetail | json_array  | 音频片段信息                   | 是           | 回调的音频片段信息，[详见audioDetail参数](#audioDetail-1)                                                                                                                                                             |
| audioTags   | json_object | 音频标签                       | 否           | 返回性别、音色、是否唱歌等标签                                                                                                                                                                                        |

#### audioDetail

audioDetail内每个元素的内容如下：

| **参数名**      | **类型**    | **参数说明**     | **是否必返** | **规范**                                                                 |
| :-------------- | :---------- | :--------------- | :----------- | :----------------------------------------------------------------------- |
| audioStarttime  | float       | 音频片段起始时间 | 是           | 相对音频开始的时间距离，单位是秒                                         |
| audioEndtime    | float       | 音频片段结束时间 | 是           | 相对音频开始的时间距离，单位是秒                                         |
| audioUrl        | string      | 音频片段链接     | 是           | mp3格式                                                                  |
| riskLevel       | string      | 音频片段识别结果 | 是           | <p>可能返回值：<br/>PASS：通过</p><p>REVIEW：审核</p><p>REJECT：拒绝</p> |
| riskLabel1      | string      | 一级风险标签     | 是           |                                                                          |
| riskLabel2      | string      | 二级风险标签     | 是           |                                                                          |
| riskLabel3      | string      | 三级风险标签     | 是           |                                                                          |
| riskDescription | string      | 风险原因         | 是           |                                                                          |
| riskDetail      | json_object | 风险详情         | 否           | [详见riskDetail参数](#riskDetail-1)                                      |


#### riskDetail

其中，riskDetail详细内容如下：

| **参数名**   | **类型**   | **参数说明**             | **是否必返** | **规范**                                                                                  |
| :----------- | :--------- | :----------------------- | :----------- | :---------------------------------------------------------------------------------------- |
| audioText    | string     | 音频转译文本的结果       | 否           |                                                                                           |
| matchedLists | json_array | 命中的客户自定义名单信息 | 否           | 命中客户自定义名单时返回，[详见matchedLists参数](#matchedLists-1)                         |
| riskSegments | json_array | 高风险内容片段           | 否           | 在涉政、暴恐、违禁、竞品、广告法等功能的时候存在，[详见riskSegments参数](#riskSegments-1) |

#### matchedLists

riskDetail中，matchedLists详细内容如下：

| **参数名** | **类型**   | **参数说明**                 | **是否必返** | **规范**                  |
| :--------- | :--------- | :--------------------------- | :----------- | :------------------------ |
| name       | string     | 客户自定义名单名称           | 是           |                           |
| words      | json_array | 命中的这个名单中的敏感词信息 | 是           | [详见words参数](#words-1) |

#### words

matchedLists中，words详细内容如下：

| **参数名** | **类型**  | **参数说明**   | **是否必返** | **规范** |
| :--------- | :-------- | :------------- | :----------- | :------- |
| word       | string    | 敏感词         | 是           |          |
| position   | int_array | 敏感词所在位置 | 是           |          |

#### riskSegments

riskDetail中，riskSegments详细内容如下：

| **参数名** | **类型**  | **参数说明**           | **是否必返** | **规范** |
| :--------- | :-------- | :--------------------- | :----------- | :------- |
| segment    | string    | 高风险内容片段         | 否           |          |
| position   | int_array | 高风险内容片段所在位置 | 否           |          |

#### audioTags

其中，audioTags详细内容如下：

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

| **参数名**  | **类型** | **参数说明**           | **是否必返** | **规范**                                                                                                                               |
| :---------- | :------- | :--------------------- | :----------- | :------------------------------------------------------------------------------------------------------------------------------------- |
| label       | string   | 音色标签类别           | 是           | <p>可能取值：</p><p>大叔音</p><p>青年音</p><p>正太音</p><p>老年音</p><p>女王音</p><p>御姐音</p><p>少女音</p><p>萝莉音</p><p>大妈音</p> |
| probability | int      | 对应音色标签可能性大小 | 是           | 取值0-100，数值越高表示概率越大                                                                                                        |

audioTags中，language详细内容如下：

| **参数名**  | **类型** | **参数说明**           | **是否必返** | **规范**                                                   |
| :---------- | :------- | :--------------------- | :----------- | :--------------------------------------------------------- |
| label       | int      | 语种识别类别标识       | 是           | <p>可能取值：</p><p>0:普通话</p><p>1:英语</p><p>2:粤语</p> |
| probability | int      | 对应音色标签可能性大小 | 是           | 取值0-100，数值越高表示概率越大                            |

## **示例**

### **上传请求示例**

```bash
curl -v 'http://api-audio-bj.fengkongcloud.com/audio/v4' -d '{"accessKey":"*************","appId":"default","eventId":"default","type":"PORN_AD_POLITICS_MOAN_ABUSE_GENDER_TIMBRE_SING_LANGUAGE","btId":"test1","contentType":"URL","content":"*************","callback":"*************","data":{"returnAllText":1,"room":"general","tokenId":"token-short"}}'
```

### **同步返回示例**

```json
{
    "code": 1100,
    "message": "成功",
    "requestId":" *************",
    "btId":"*************"
}
```

### **回调返回示例**

```json
{
    "requestId":"6a9cb980346dfea41111656a514e9109",
    "btId":"1604311839040",
    "code":1100,
    "message":"正常",
    "riskLevel":"PASS",
    "audioDetail":[
        {
            "audioStarttime":0,
            "audioEndtime":10,
            "audioUrl":"https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0000.mp3",
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

### **主动查询结果请求示例**

```bash
curl -v 'http://api-audio-bj.fengkongcloud.com/query_audio/v4' -d '{"accessKey":"*************","btId":"*************"}'
```

### **主动查询结果返回示例**

```json
{
    "requestId": "6a9cb980346dfea41111656a514e9109",
    "btId": "1604311839040",
    "code": 1100,
    "message": "正常",
    "riskLevel": "PASS",
    "audioDetail": [
        {
            "audioStarttime": 0,
            "audioEndtime": 10,
            "audioUrl": "https://voice-bj-1251671073.cos.ap-beijing.myqcloud.com/20201102/6a9cb980346dfea41111656a514e9109_a0000.mp3",
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

# **Demo**

目前提供了 go、java、lua、nodes、php、python 的 demo，代码位置：
[https://github.com/ishumei/api-demo/tree/master/v4](https://github.com/ishumei/api-demo/tree/master/v4)
