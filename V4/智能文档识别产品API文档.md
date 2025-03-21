



# 数美智能文档识别产品API接口文档

- - - - -

***版权所有 翻版必究***

- - - - -

目录

- [智能文档过滤服务](#A)
  
  - [请求参数](#Aa)
    - [请求URL](#Aa1)
    - [字符编码格式](#Aa2)
    - [请求方法](#Aa3)
    - [建议超时时长](#Aa4)
    - [请求参数](#Aa5)
  - [返回结果](#Ab)
    - [回调模式](#Ab2)
  
  - [示例](#Ac)
    - [回调模式](#Ac2)
      - [请求示例](#Ac21)
      - [响应示例](#Ac22)
  
- [智能文档上传服务](#B)
  
  - [请求参数](#Ba)
    - [请求URL](#Ba1)
    - [字符编码格式](#Ba2)
    - [请求方法](#Ba3)
    - [建议超时时长](#Ba4)
    - [请求参数](#Ba5)
  - [返回结果](#Bb)
    - [请求返回参数](#Bb1)
  - [示例](#Bc)
    - [请求示例](#Bc1)
    - [响应示例](#Bc2)
  
- [结果查询接口](#C)
  
  - [请求参数](#Ca)
    
    - [请求URL](#Ca1)
    - [字符编码格式](#Ca2)
    - [请求方法](#Ca3)
    - [建议超时时长](#Ca4)
    - [请求参数](#Ca5)
    
  - [返回结果](#Cb)
    
    - [请求返回参数](#Cb1)
    
  - [示例](#Cc)
    
    - [请求示例](#Cc1)
    
    - [返回示例](#Cc2)
    
      

# <span id="A">智能文档过滤服务接入说明</span>

## <span id="Aa">请求参数</span>

### <span id="Aa1">请求URL：</span>

| 集群 | URL | 支持产品列表 |
| --- | --- |--------|
| 北京 | `http://api-article-bj.fengkongcloud.com/v1/saas/anti_fraud/article` | 文档产品   |

### <span id="Aa2">字符编码格式：</span>

`UTF-8`字符集编码

### <span id="Aa3">请求方法：</span>

`POST`

### <span id="Aa4">建议超时时长：</span>

15s

### <span id="Aa5">请求参数：</span>

放在HTTP Body中，采用Json格式，Body大小不可超过3.5M，具体参数如下：

| **请求参数名** | **类型** | **参数说明**   | **是否必传** | **规范** |
| --- | --- |------------| --- | --- |
| accessKey | string | 接口认证密钥     | Y | 由数美提供 |
| type | string | 平台业务类型     | N | 可选值：<br/>`ZHIBO`:直播<br/>`ECOM`:电商<br/>`GAME`:游戏<br/>`NEWS`:新闻资讯<br/>`FORUM`:论坛<br/>`SOCIAL`:社交<br/>`NOVEL`:小说<br/> |
| imgType | string | 文档中的图片识别类型 | N | 可选值：<br/>`POLITICS`:涉政识别<br/>`PORN`:色情识别<br/>`AD`:广告识别<br/>`LOGO`:水印logo识别<br/>`BEHAVIOR`:不良场景识别，支持吸烟、喝酒、赌博、吸毒、避孕套和无意义画面<br/>`OCR`:图片中的OCR文字识别<br/>`VIOLENCE`:暴恐识别<br/>`NONE`:不需要识别图片<br/>如需做组合识别，通过下划线连接即可，例 如 POLITICS_PORN_AD 用于广告、色情和涉政识别<br/>不传时按涉政、色情、广告进行识别。 |
| txtType | string | 文档中的文字识别类型 | N | 可选值：<br/>POLITY：涉政检测<br/>VIOLENT：暴恐检测<br/>BAN：违禁检测<br/>EROTIC：色情检测<br/>DIRTY：辱>骂检测<br/>ADVERT：广告检测<br/>PRIVACY：隐私检测<br/>ADLAW：广告法检测<br/>MEANINGLESS：无意义检测<br/>FRUAD：网络诈骗检测<br/>UNPOACH：高价值用户防挖检测<br/>TEXTMINOR: 未成年人内容检测<br/>TEXTRISK：常规风险检测（包含：涉政、暴恐、违禁、色情、辱骂、广告、隐私、广告法、无意义）<br/>以上type可以下划线组>合，如：TEXTRISK_FRUAD；type间组合取并集，如：TEXTRISK_POLITY按照常规风险检测处理；不传时按传入常规风险处理。 |
| appId | string | 应用标识       | N | 用于区分相同公司的不同应用，该参数传递值可与数美服务协商 |
| callback | string | 回调http接口   | N | 当该字段非空时，服务将根据该字段回调通知用户审核结果；当传入fileFormat时必传 |
| callbackParam | json_object | 透传字段       | N | 当 callback 存在时可选，发送回调请求时服务将该字段内容同审核结果一起返回 |
| data | json\_object | 请求的数据内容    | Y | 最长1MB, [详见data参数](#data) |

 其中，<span id="data">data</span>的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- |----------| --- |
| contents | string | 要检测的内容 | Y        | 可填入url链接<br/>其中url支持网址链接或文档下载链接<br/>文件大小500m以内，文本长度限制50w字，图片张数限制500张，视频文件数限制50段。 |
| fileFormat | string | 要检测的文档格式 | Y        | 可选值：<br/>`PDF`<br/>`EPUB`<br/>`DOCX`<br/>`DOC`<br/>`XLSX`<br/>`XLS`<br/>`PPTX`<br/>`PPT`<br/>`PPS`<br/>`PPSX`<br/>`XLTX`<br/>`XLTM`<br/>`XLSB`<br/>`XLSM`<br/>`TXT`<br/>`CSV`<br/>`SRT`<br/>`VTT`  |
| tokenId | string | 客户端用户账号唯一标识，用于用户行为分析，建议传入用户UID | Y        |  |
| channel | string | 业务场景 | N        | 渠道表配置 |
| returnHtml | bool | 是否需要返回数美审核后高亮框处风险内容的html，用与展示给审核人员看 | N        | 可选值:<br/>`true`<br/>`false`<br/>默认为false |
| nickname | string | 用户昵称，强烈建议传递此参数，几乎所有平台的恶意用户都会通过昵称散播垃圾信息，存在涉政违禁和导流信息等风险 | N        |  |
| ip | string | 客户端ip地址，该参数用于IP维度的用户行为分析，同时可用于比对数美IP黑库 | N        |  |
| passThrough | json_object | 透传参数，原样返回 | N        |  |

## <span id="Ab">返回结果</span>

### <span id="Ab2">回调模式</span>

系统会自动推送机审结果至callback字段指定的URL上

#### 请求返回参数：

| **参数名称** | **类型**    | **参数说明** | **是否必返** | **规范**                                                     |
| ------------ | ----------- | ------------ | ------------ | ------------------------------------------------------------ |
| code         | int         | 返回码       | Y            | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9101`：无权限操作 |
| message      | string      | 返回码描述   | Y            | 和code对应：<br/>成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>无权限操作 |
| requestId    | string      | 请求标识     | Y            | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存  |
| score        | int         | 风险分数     | N            | code为1100时存在，取值范围[0,1000]，分数越高风险越大         |
| riskLevel    | string      | 处置建议     | N            | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| detail       | json_object | 风险详情     | N            | [详见detail参数](#Adetail)                                   |

#### 回调返回参数：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int| 返回码 | Y | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9100`：余额不足<br/>`9101`：无权限操作 |
| message | string | 返回码描述 | Y | 和code对应：<br/>成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>余额不足<br/>无权限操作 |
| requestId | string | 请求标识 | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存|
| score | int | 风险分数 | N | code为1100时存在，取值范围[0,1000]，分数越高风险越大 |
| riskLevel | string | 处置建议 | N | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| detail | json_object | 风险详情 | N | [详见detail参数](#Adetail) |
| status | int | 提示服务是否超时 | Y | 可能返回值：<br/>`0`：正常<br/>`501`：超时 |
| auxInfo | json_object | 辅助信息 | Y | [详见auxInfo参数](#AauxInfo)  |
| callbackParam | json_object | 透传字段	 | N	 |透传参数，原样返回  |

<span id="Adetail">其中detail字段如下：</span>

| 参数名称    | **类型**    | **是否必选** | **说明**                                                   |
| ----------- | ----------- | ------------ | ---------------------------------------------------------- |
| model       | string      | Y            | 规则标识                                                   |
| description | string      | Y            | 策略规则风险原因描述                                       |
| descriptionV2     | string      | N            | 新版策略规则风险原因描述<br/>注：该参数为新版API返回参数，过渡阶段只有新策略才会返回 |
| riskSummary | json object | N            | 风险摘要，目前包括各种风险类型的次数，如果type为NOVEL才返回<br/>[格式请见riskSummary结果详情](#AriskSummary) |
| riskDetail  | json array  | N            | 每一段内容的风险详情，如果type为NOVEL才返回。如果returnHtml参数为true只返回REJECT和REVIEW的风险内容片段，如果returnHtml参数为false会返回全部内容片段（包括REJECT和REVIEW和PASS）。<br/>[格式请见riskDetail结果详情](#AriskDetail) |
| riskHtml    | string      | N            | 风险内容标记的html,可嵌入需要展示的html页面，如果type为NOVEL且returnHtml参数为true才返回。<br/> |
| hits        | json_array  | N            | 命中信息，一般为空。命中详情在riskDetail中。             |
| passThrough | json_object | N            | 透传参数，原样返回             |

其中，<span id="AriskSummary">riskSummary</span>内容是风险类型，具体如下：

| **参数名称**| **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskType | int| 对应riskType风险出现的次数 | N| 风险类型：<br/>`0`：正常<br/>`100`：涉政<br/>`200`：色情<br/>`210`：辱骂<br/>`300`：广告<br/>`400`：灌水<br/>`500`：无意义<br/>`600`：违禁<br/>`700`：黑名单<br/>`710`：白名单<br/>`800`：高危账号<br/>`900`：自定义 |

其中，<span id="AriskDetail">riskDetail</span>是json array，其中每一项是一个内容片段的风险详情，具体如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| type     | string       | 当前内容片段的类型 | Y            | 可选值：<br/>`text`：文本<br/>`image`：图片<br/>          |
| content | string | 当前内容片段的内容 | Y           | text是文本内容，image是图片url |
| beginPosition | int       | 当前内容片段在输入中的起始位置，当type为`image`时该字段不返回 | N            | 检测出的文本内容，从0开始计算位置；文本切分后，每个片段的文本内容的首字在全局检测出文本中的位置 |
| endPosition | int     | 当前内容片段在输入中的结束位置，当type为`image`时该字段不返回 | N           | 检测出的文本内容，从0开始计算位置；文本切分后，每个片段的文本内容的末尾字在全局检测出文本中的位置 |
| description | string | 当前内容片段的风险描述 | Y           | 命中的对应名单中的所有敏感词                                 |
| riskLevel | string | 当前内容片段的处置建议 | Y           | 可选值：<br/>`PASS`：通过<br/>`REVIEW`：审核<br/>`REJECT`: 拒绝 |
| riskType | int | 当前内容片段的标识风险类型 | Y | 当type为文本时：<br/>`0`：正常<br/>`100`：涉政<br/>`200`：色情<br/>`210`：辱骂<br/>`300`：广告<br/>`400`：灌水<br/>`500`：无意义<br/>`600`：违禁<br/>`700`：黑名单<br/>`710`：白名单<br/>`800`：高危账号<br/>`900`：自定义<br/><br/>当type为图片时：<br/>`0`：正常<br/>`100`：涉政<br/>`200`：色情<br/>`210`：性感<br/>`300`：广告<br/>`310`：二维码<br/>`320`：水印<br/>`400`：暴恐<br/>`500`：违规<br/>`510`：不良场景<br/>`520`：未成年人<br/>`700`：黑名单<br/>`710`：白名单<br/>`800`：高危账号<br/>`900`：自定义 |
| riskTypeDec | string | riskType对应的描述 | N |  |
| model | string | 规则标识，用来标识文本命中的策略规则 | N |  |
| matchedList | string | 命中敏感词所在的名单名称（该参数仅在命中敏感词时存在） | N |  |
| matchedItem | string | 命中的具体敏感词（该参数仅在命中敏感词时存在） | N |  |
| matchedField | string | 标识昵称或文本内容命中了敏感词（该参数仅在命中敏感词时存在） | N | 可选值：<br/>`text`：文本命中敏感词<br/>`nickname`：昵称命中敏感词 |
| matchedDetail | json_array | 命中的名单详情 | N | [详见详细结构](#matcheddetail) |
| index | int | 当前处理的片段索引 | N | 索引不区分文本和图片 |
| keywordsPosition | string | 命中的敏感词位置 | N | 在该段中的位置 |
| text | string | 图片中的ocr内容 | N | 图片片段识别出ocr内容时会返回该字段 |

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

## <span id="Ac">示例</span>

### <span id="Ac2">回调模式</span>

#### <span id="Ac21">请求示例</span>

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

#### <span id="Ac22">响应示例</span>

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

回调结果:

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


# <span id="B">智能文档过滤上传接口</span>

## <span id="Ba">请求参数</span>

### <span id="Ba1">请求URL：</span>

| 集群 | URL                                                          | 支持产品列表 |
| ---- | ------------------------------------------------------------ |--------|
| 北京 | `http://api-article-bj.fengkongcloud.com/v1/saas/anti_fraud/article_async` | 文档产品   |

### <span id="Ba2">字符编码格式：</span>

`UTF-8`字符集编码

### <span id="Ba3">请求方法：</span>

`POST`

### <span id="Ba4">建议超时时长：</span>

5s

### <span id="Ba5">请求参数：</span>

放在HTTP Body中，采用Json格式，Body大小不可超过3.5M，具体参数如下：

| **请求参数名** | **类型**     | **参数说明**   | **是否必传** | **规范**                                                     |
| -------------- | ------------ |------------| ------------ | ------------------------------------------------------------ |
| accessKey      | string       | 接口认证密钥     | Y            | 由数美提供                                                   |
| type           | string       | 平台业务类型     | N            | 可选值：<br/>`ZHIBO`:直播<br/>`ECOM`:电商<br/>`GAME`:游戏<br/>`NEWS`:新闻资讯<br/>`FORUM`:论坛<br/>`SOCIAL`:社交<br/>`NOVEL`:小说<br/> |
| imgType        | string       | 文档中的图片识别类型 | N            | 可选值：<br/>`POLITICS`:涉政识别<br/>`PORN`:色情识别<br/>`AD`:广告识别<br/>`LOGO`:水印logo识别<br/>`BEHAVIOR`:不良场景识别，支持吸烟、喝酒、赌博、吸毒、避孕套和无意义画面<br/>`OCR`:图片中的OCR文字识别<br/>`VIOLENCE`:暴恐识别<br/>`NONE`:不需要识别图片<br/>如需做组合识别，通过下划线连接即可，例 如 POLITICS_PORN_AD 用于广告、色情和涉政识别 |
| txtType | string | 文档中的文字识别类型 | N | 可选值：<br/>POLITY：涉政检测<br/>VIOLENT：暴恐检测<br/>BAN：违禁检测<br/>EROTIC：色情检测<br/>DIRTY：辱>骂检测<br/>ADVERT：广告检测<br/>PRIVACY：隐私检测<br/>ADLAW：广告法检测<br/>MEANINGLESS：无意义检测<br/>FRUAD：网络诈骗检测<br/>UNPOACH：高价值用户防挖检测<br/>TEXTMINOR: 未成年人内容检测<br/>TEXTRISK：常规风险检测（包含：涉政、暴恐、违禁、色情、辱骂、广告、隐私、广告法、无意义）<br/>以上type可以下划线组>合，如：TEXTRISK_FRUAD；type间组合取并集，如：TEXTRISK_POLITY按照常规风险检测处理；不传时按传入常规风险处理。 |
| appId          | string       | 应用标识       | N            | 用于区分相同公司的不同应用，该参数传递值可与数美服务协商     |
| data           | json\_object | 请求的数据内容    | Y            | 最长1MB, [详见data参数](#data)                               |

 其中，data的内容同同步接口：

## <span id="Bb">返回结果</span>

### <span id="Bb1">请求返回参数</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范**                                                     |
| ------------ | -------- | ------------ | ------------ | ------------------------------------------------------------ |
| code         | int      | 返回码       | Y            | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9100`：余额不足<br/>`9101`：无权限操作 |
| message      | string   | 返回码描述   | Y            | 和code对应：<br/>成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>余额不足<br/>无权限操作 |
| requestId    | string   | 请求标识     | Y            | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存  |



### <span id="Bc">示例</span>

#### <span id="Bc1">请求示例</span>

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

#### <span id="Bc2">响应示例</span>

```json
{
    "code":1100,
    "message":"\u6210\u529f",
    "requestId":"tye7ert12asdfasdf31236444442333312"
}
```



# <span id="C">结果查询接口</span>

该接口用于查询机审和人审识别结果

## <span id="Ca">请求参数</span>

### <span id="Ca1">请求URL：</span>

| 集群 | URL                                                          | 支持产品列表 |
| ---- | ------------------------------------------------------------ |--------|
| 北京 | `http://api-article-bj.fengkongcloud.com/v1/saas/anti_fraud/article/query` | 文档产品   |

### <span id="Ca2">字符编码格式：</span>

`UTF-8`字符集编码

### <span id="Ca3">请求方法：</span>

`POST`

### <span id="Ca4">建议超时时长：</span>

1s

### <span id="Ca5">请求参数：</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明**   | **是否必传** | **规范**                                        |
| -------------- | -------- | -------------- | ------------ | ----------------------------------------------- |
| accessKey      | string   | 接口认证密钥   | Y            | 由数美提供                                      |
| requestIds     | array    | 机器审核流水号 | Y            | 最多支持10条 字符串数组 item 为数美返回的流水号 |

## <span id="Cb">返回结果</span>

### <span id="Cb1">请求返回参数</span>

| **请求参数名** | **类型**   | **参数说明** | **是否必传** | **规范**                      |
| -------------- | ---------- | ------------ | ------------ | ----------------------------- |
| code           | int        | 返回码       | Y            |                               |
| message        | string     | 返回码描述   | Y            |                               |
| contents       | json array | 内容         | Y            | [详见contents内容](#contents) |

<span id="contents">其中contents组成如下</span>

| **请求参数名** | **类型**    | **参数说明**                 | **是否必传** | **规范**                                                     |
| -------------- | ----------- | ---------------------------- | ------------ | ------------------------------------------------------------ |
| requestId      | string      | 请求唯一标识                 | Y            |                                                              |
| humanResult    | json object | 人审结果，人审完成后才会存在 | N            |                                                              |
| machineResult  | json object | 机审结果，机审完成后才会存在 | N            | [参考回调接口返回字段](#Ab2)                                 |
| mergeResult    | json_object | 统一人审和机审结果           | N            | 优先返回人审结果，如果人审结果没有，返回机审结果，如果都没有不存在 |

其中，humanResult/mergeResult的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范**                                     |
| -------------- | -------- | ------------ | ------------ | -------------------------------------------- |
| riskLevel      | string   | 处置指令     | Y            | 建议取值：<br/>`REJECT`:删除<br/>`PASS`:发布 |

## <span id="Cc">示例</span>

### <span id="Cc1">请求示例</span>

```json
{
    "accessKey":"xxxxxxxx",
    "requestIds":[
        "tye7ert12asdfasdf31236633346662333312"
    ]
}
```

### <span id="Cc2">响应示例</span>

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
            },
            "mergeResult":{
                "riskLevel":"REJECT"
            }
        }
    ]
}
```

