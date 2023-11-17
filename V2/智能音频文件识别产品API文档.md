# 智能音频文件识别产品API文档

- - - - -

***版权所有 翻版必究***

- - - - -
## 上传音频识别请求

### 接口描述

该接口用于提交音频识别请求。

### 请求URL

上海集群：

http://api-audio-sh.fengkongcloud.com/v2/saas/anti_fraud/audio

硅谷集群：

http://api-audio-gg.fengkongcloud.com/v2/saas/anti_fraud/audio

新加坡集群：

http://api-audio-xjp.fengkongcloud.com/v2/saas/anti_fraud/audio

### 字符编码格式

使用UTF-8字符集进行编码

### 请求方法

POST

### 建议超时时长

3s

### 音频格式限制

`WAV`、`MP3`、`AAC`、`AMR`、`3GP`、`M4A`、`WMA`、`OGG`、`APE`、`FLAC`、`ALAC`、`WAVPACK`、`SILK_V3`等

### 请求体限制

所有请求参数大小总和不能超过18M

### 请求参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称**  | **类型**    | **是否必选** | **说明**                                                     |
| :------------ | :---------- | :----------- | :----------------------------------------------------------- |
| accessKey     | string      | Y            | 服务密匙，开通账号服务时由数美提供                           |
| type          | string      | Y            | <p>需要识别的违规类型，可选值：</p><p>AUDIOPOLITICAL：一号领导人声纹识别</p><p>POLITY：涉政识别</p><p>ANTHEN：国歌识别</p><p>EROTIC：色情</p><p>DIRTY: 辱骂识别</p><p>ADVERT：广告识别</p><p>MOAN：娇喘识别</p><p>BANEDAUDIO：违禁歌曲</p><p>如需做组合识别，通过下划线连接即可，例</p><p>如 POLITY_EROTIC_MOAN用于涉政、色情和娇喘识别。<br/>建议传入：<br/>POLITY_EROTIC_MOAN_ADVERT</p> |
| businessType  | string      | N            | 识别类型，可选值：<br/>SING：唱歌识别<br/>LANGUAGE：语种识别<br/>GENDER：性别识别<br/>TIMBRE：音色标签 （需要同时传入GENDER才能生效）<br/>VOICE：人声属性<br/>MINOR：未成年识别<br/>AUDIOSCENE：声音场景<br/>AGE：年龄识别<br/>type和 businessType 必须填其一 |
| appId         | string      | N            | 需要联系数美开通，请以数美单独提供的传值为准                 |
| btId          | string      | Y            | 音频唯一标识，超过128位将被截断, 不能重复，否则提示参数错误。 |
| data          | json_object | Y            | 请求数据，最长1MB，详细内容参见下表                          |
| callback      | string      | N            | 异步检测结果回调通知您的URL，支持HTTP和HTTPS。字段为空时，您必须通过查询接口主动查询结果。 |
| callbackParam | json_object | N            | 透传字段，当 callback 存在时可选，发送回调请求时服务将该字段内容同音频结果一起返回 |

*data的内容如下：*

| **参数名称**  | **类型**    | **是否必选** | **说明**                                                                                                                                                                    |
| :------------ | :---------- | :----------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| url           | string      | N            | 待识别音频url地址，url和content至少提供一个                                                                                                                                 |
| lang          | string      | N            | 可选值如下，（默认值为zh）：<br/>zh：中文<br/>en：英文<br/>ar：阿拉伯语<br/>hi：印地语<br/>es：西班牙语<br/>fr：法语<br/>ru：俄语<br/>pt：葡萄牙语<br/>id：印尼语<br/>de：德语<br/>ja：日语<br/>tr：土耳其语<br/>vi：越南语<br/>it：意大利语<br/>th：泰语<br/>tl：菲律宾语<br/>ko：韩语<br/>ms：马来语                                                                                                     |
| content       | string      | N            | 待识别音频的base64编码数据（上限15M，仅支持pcm、wav、mp3）, pcm格式数据必须采用16-bit小端序编码。推荐使用pcm、wav格式传输。url和content至少提供一个，同时存在时仅支持content |
| formatInfo    | json_object | N            | 当content存在时必须存在，本地语音文件格式信息。json内具体格式见下方说明<br/>（另外，如有其他特定音频格式的需求，在与数美沟通后，可传入该字段表示特殊解码逻辑）                               |
| audioName     | string      | N            | 音频文件名称                                                                                                                                                                |
| tokenId       | string      | N            | 用户账号标识                                                                                                                                                                |
| channel       | string      | N            | 业务场景名称，见渠道配置表                                                                                                                                                  |
| returnAllText | bool        | N            | 取值为true时，返回所有音频片段识别结果（每10秒一个音频片段）；取值为false时，返回风险片段（riskLevel为REJECT或REVIEW）识别结果。默认为false。                               |
| audioDetectStep | int | N | 间隔审核步长，取值范围为1-36整数，取1表示跳过一个10S的音频片段审核，取2表示跳过两个，以此类推，不使用该功能时音频内容全部过审。启用该功能时，建议开启returnAllText，采用每个片段的ASR识别结果。 |
| receiveTokenId | string | N | 私聊场景下消息接收者的tokenId,由数字、字母、下划线、短杠组成的长度小于等于64位的字符串 |
| nickname      | string      | N            | 用户昵称  |
| timestamp     | int         | N            | 时间戳（毫秒级）  |
| room          | string      | N            | 房间号     |
| deviceId        | string | N  |  数美设备指纹生成的设备唯一标识                                                                                              |
| dataId | string | N | 数据标识 |
| ip              | string | N    |  发送该音频的用户公网ipv4地址                                                                                             |

*formatInfo内容如下：*

| **参数名称** | **类型** | **是否必选** | **说明**                                                                          |
| :----------- | :------- | :----------- | :-------------------------------------------------------------------------------- |
| format       | string   | Y            | 语音数据格式，仅支持（取值）pcm、wav、mp3、opus-gin，严格小写。                             |
| rate         | int      | N            | 语音数据采样率，当语音数据格式为pcm时必须存在，范围限制8000-32000。               |
| track        | int      | N            | 语音数据声道数，当语音数据格式为pcm时必须存在，支持单声道(取值1)或双声道(取值2)。 |


### 返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型** | **是否必选** | **说明**                                         |
| :----------- | :------- | :----------- | :----------------------------------------------- |
| code         | int      | Y            | 详见接口响应码列表                                           |
| message      | string   | Y            | 返回码详情描述                                   |
| requestId    | string   | Y            | 请求唯一标识                                     |
| btId         | string   | Y            | 客户上传音频唯一标识，与请求参数中的btId字段对应 |


## 主动查询识别结果

### 接口描述

该接口用于查询音频识别结果，最大支持10qps。

### 请求URL

上海集群：

http://api-audio-sh.fengkongcloud.com/v2/saas/anti_fraud/query_audio

硅谷集群：

http://api-audio-gg.fengkongcloud.com/v2/saas/anti_fraud/query_audio

新加坡集群：

http://api-audio-xjp.fengkongcloud.com/v2/saas/anti_fraud/query_audio

### 字符编码格式

请求及返回结果都使用UTF-8字符集进行编码

### 请求方法

POST

### 建议超时时长

1s

### 请求参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型** | **是否必选** | **说明**                           |
| :----------- | :------- | :----------- | :--------------------------------- |
| accessKey    | string   | Y            | 服务密匙，开通账号服务时由数美提供 |
| btId         | string   | Y            | 音频唯一标识，用于查询识别结果     |

### 返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称**   | **类型**    | **是否必选** | **说明**                                                     |
| :------------- | :---------- | :----------- | :----------------------------------------------------------- |
| code           | int         | Y            | 详见接口响应码列表                                                       |
| message        | string      | Y            | 返回码详情描述                                               |
| requestId      | string      | Y            | 请求唯一标识                                                 |
| btId           | string      | Y            | 音频唯一标识                                                 |
| audioText      | string      | Y            | 整段音频转译文本结果                                         |
| audioTime      | int         | N            | 整段音频的音频时长，单位秒，code为1100时存在                 |
| labels         | string      | N            | 音频识别结果标签                                             |
| riskLevel      | string      | N            | <p>识别结果，可能返回值：</p><p>PASS：正常内容<br/>REVIEW：疑似违规内容<br/>REJECT：违规内容</p> |
| detail         | json_array  | N            | 风险详情                                                     |
| gender         | json_object | N            | 性别标签与概率值                                             |
| isSing         | int         | N            | <p>表示该条音频文件是否唱歌，0表示没有唱歌，1表示唱歌。</p><p>仅当type传入值包含SING时返回。</p> |
| language       | json_array  | N            | 语种标签与概率值列表                                         |
| tags           | json_array  | N            | 音色标签与概率值列表                                         |
| businessLabels | json_array  | N            | 业务标签返回（目前只支持MINOR,策略命中返回标签内容,否则为空,历史遗留字段,不建议使用） |
| auxInfo        | json_object | N            | 辅助信息|
| tokenProfileLabels | json_array  | N            | 账号属性标签，仅在开启功能时返回 |
| tokenRiskLabels | json_array  | N           | 账号风险标签，仅在开启功能时返回 |

*detail数组中每一项的具体参数如下：*

| **参数名称**     | **类型**   | **是否必选** | **说明**                                                     |
| :--------------- | :--------- | :----------- | :----------------------------------------------------------- |
| requestId        | string     | Y            | 风险音频片段请求唯一标识                                     |
| audioStarttime   | string     | Y            | 风险音频片段在音频中的起始时间，单位秒                       |
| audioEndtime     | string     | Y            | 风险音频片段在音频中的结束时间，单位秒                       |
| audioModel       | string     | Y            | 规则标识，命中的最高优先级规则标识（废弃字段，不建议使用 ）  |
| audioUrl         | string     | Y            | 风险音频片段地址，MP3格式                                    |
| audioText        | string     | Y            | 音频片段转译的文本内容                                       |
| riskLevel        | string     | Y            | <p>识别结果，可能返回值： <br/>REJECT：违规内容</p><p>REVIEW：疑似违规内容</p><p>PASS：正常内容</p> |
| businessLabels   | json_array | N            | 业务标签返回 [详见businessLabels参数](#businessLabels)       |
| riskType         | int        | N            | <p>标识风险类型，可能取值:<br/>风险类型，静音时不返回，可能取值:<br/>0:正常</p><p>100:涉政/国歌</p><p>110:暴恐</p><p>200:色情</p><p>210:辱骂</p><p>250:娇喘</p><p>260:一号领导声纹</p><p>270:人声属性</p><p>280:违禁歌曲</p><p>300:广告</p><p>400:灌水</p><p>500:无意义</p><p>520:未成年人</p><p>600:违禁</p><p>700:其他</p><p>720:黑账号</p><p>730:黑IP</p><p>800:高危账号</p><p>900:自定义</p> |
| audioMatchedItem | string     | N            | 音频中可能出现的敏感词                                       |
| description      | string     | Y            | 音频片段风险原因描述，仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |

*gender参数结构如下：*

| **参数名称** | **类型** | **是否必选** | **说明**                                              |
| :----------- | :------- | :----------- | :---------------------------------------------------- |
| label        | string   | Y            | <p>性别标签名称，可能取值：</p><p>男性</p><p>女性</p> |
| confidence   | int      | Y            | 对应性别可能性大小，取值0-100，数值越高表示概率越大。 |

*language数组中每一项具体参数如下：*

| **参数名称** | **类型** | **是否必选** | **说明**                                                                     |
| :----------- | :------- | :----------- | :--------------------------------------------------------------------------- |
| label        | int      | Y            | <p>语种识别类别标识，可能取值：</p><p>0:普通话</p><p>1:英语</p><p>2:粤语</p><p>3:藏语</p><p>4:维语</p><p>5:蒙语</p><p>6:朝鲜语</p><p>-1:其他语种</p> |
| confidence   | int      | Y            | 对应语种标签可能性大小，取值0-100，数值越高表示概率越大。                    |

*tags数组中每一项具体参数如下：*

| **参数名称** | **类型** | **是否必选** | **说明**                                                                                                                                                              |
| :----------- | :------- | :----------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| label        | string   | Y            | <p>音色标签类别，可能取值：</p><p>大叔</p><p>青年</p><p>正太</p><p>老年</p><p>女王</p><p>御姐</p><p>少女</p><p>萝莉</p><p>大妈</p><p>男性</p><p>女性</p><p>无人声</p> |
| confidence   | int      | Y            | 对应音色标签可能性大小，取值0-100，数值越高表示概率越大。                                                                                                             |

*auxInfo数组中每一项具体参数如下：*

| **参数名称**  | **类型** | **是否必返** | **说明**                                 |
| :------------| :-----  | :----------| :--------------------------------------- |
| errorCode    | int     | Y          |<p>状态码</p><p>2003：音频下载失败</p><p>2007：无有效数据</p>|

*tokenProfileLabels，tokenRiskLabels 数组中每一项具体参数如下:*

| **参数名称**    | **类型** | **是否必返** | **说明**                                 |
|:------------|:-------|:---------|:---------------------------------------|
| label1      | string | N        | 一级标签                                   |
| label2      | string | N        | 二级标签                                   |
| label3      | string | N        | 三级标签                                   |
| description | string | N        | 账号标签描述，仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |
| timestamp   | int    | N        | 打标签时间戳 13位Unix时间戳，单位：毫秒                |

## 异步回调识别结果

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

当用户收到推送结果，并返回 HTTP 状态码为200时，表示推送成功；否则系统将进行最多20次推送。

### 返回字段

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称**  | **类型**    | **是否必选** | **说明**                                                                                         |
| :------------ | :---------- | :----------- | :----------------------------------------------------------------------------------------------- |
| code          | int         | Y            | 详见接口响应码列表                                                                                     |
| message       | string      | Y            | 返回码详情描述                                                                                   |
| requestId     | string      | Y            | 请求唯一标识                                                                                     |
| btId          | string      | Y            | 音频唯一标识                                                                                     |
| audioText     | string      | Y            | 音频转译文本结果                                                                                 |
| audioTime     | int         | N            | 整段音频的音频时长，单位秒，code为1100时存在                                                     |
| labels        | string      | N            | 音频片段风险原因汇总                                                                             |
| riskLevel     | string      | N            | <p>识别结果，可能返回值：</p><p>PASS：正常内容<br/>REVIEW：疑似违规内容<br/>REJECT：违规内容</p> |
| detail        | json_array  | N            | 风险详情                                                                                         |
| gender        | json_object | N            | 性别标签与概率值                                                                                 |
| language       | json_array  | N            | 语种标签与概率值列表                                         |
| tags          | json_array  | N            | 音色标签与概率值列表                                                                             |
| callbackParam | json_object | Y            | 客户传入的透传字段                                                                               |
| auxInfo       | json_object | N            | 辅助信息                                                                                       |

*detail数组中每一项的具体参数如下：*

| **参数名称**     | **类型**   | **是否必选** | **说明**                                                     |
| :--------------- | :--------- | :----------- | :----------------------------------------------------------- |
| audioStarttime   | int        | Y            | 风险音频片段在音频中的起始时间，单位秒                       |
| audioEndtime     | int        | Y            | 风险音频片段在音频中的结束时间，单位秒                       |
| audioUrl         | string     | Y            | 风险音频片段地址                                             |
| audioText        | string     | Y            | 音频片段对应的文本内容                                       |
| riskLevel        | string     | Y            | <p>识别结果，可能返回值： <br/>REJECT：违规内容</p><p>REVIEW：疑似违规内容</p> |
| businessLabels   | json_array | N            | 业务标签返回 [详见businessLabels参数](#businessLabels)       |
| riskType         | int        | N            | <p>标识风险类型，可能取值:<br/>风险类型，静音时不返回，可能取值:<br/>0:正常</p><p>100:涉政/国歌</p><p>110:暴恐</p><p>200:色情</p><p>210:辱骂</p><p>250:娇喘</p><p>260:一号领导声纹</p><p>270:人声属性</p><p>280:违禁歌曲</p><p>300:广告</p><p>400:灌水</p><p>500:无意义</p><p>520:未成年人</p><p>600:违禁</p><p>700:其他</p><p>720:黑账号</p><p>730:黑IP</p><p>800:高危账号</p><p>900:自定义</p> |
| audioMatchedItem | string     | N            | 音频中可能出现的敏感词                                       |
| description      | string     | Y            | 风险原因描述，仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |

*gender参数结构如下：*

| **参数名称** | **类型** | **是否必选** | **说明**                                              |
| :----------- | :------- | :----------- | :---------------------------------------------------- |
| label        | string   | Y            | <p>性别标签名称，可能取值：</p><p>男性</p><p>女性</p> |
| confidence   | int      | Y            | 对应性别可能性大小，取值0-100，数值越高表示概率越大。 |

*<span id="businessLabels">businessLabels</span>数组中每一项具体参数如下：*

| **参数名**          | **类型**    | **是否必返** | **说明**                                     |
| ------------------- | ----------- | ------------ | -------------------------------------------- |
| businessLabel1      | string      | Y            | 一级标签                                     |
| businessLabel2      | string      | Y            | 二级标签                                     |
| businessLabel3      | string      | Y            | 三级标签                                     |
| businessDescription | string      | Y            | 格式为"一级标签:二级标签:三级标签"的中文名称 |
| confidenceLevel     | int         | N            | 可选值在0～2之间，值越大，可信度越高         |
| probability         | float       | N            | 可选值为0~1，值越大，可信度越高              |
| businessDetail      | json_object | N            | 详细信息，保留字段                           |

*language数组中每一项具体参数如下：*

| **参数名称** | **类型** | **是否必选** | **说明**                                                                                                                                             |
| :----------- | :------- | :----------- | :--------------------------------------------------------------------------------------------------------------------------------------------------- |
| label        | int      | Y            | <p>语种识别类别标识，可能取值：</p><p>-1:其他语种</p><p>0:普通话</p><p>1:英语</p><p>2:粤语</p><p>3:藏语</p><p>4:维语</p><p>5:蒙语</p><p>6:朝鲜语</p> |
| confidence   | int      | Y            | 对应语种标签可能性大小，取值0-100，数值越高表示概率越大。                                                                                            |

*tags数组中每一项具体参数如下：*

| **参数名称** | **类型** | **是否必选** | **说明**                                                                                                                                             |
| :----------- | :------- | :----------- | :--------------------------------------------------------------------------------------------------------------------------------------------------- |
| label        | string   | Y            | <p>音色标签类别，可能取值：</p><p>大叔音</p><p>青年音</p><p>正太音</p><p>老年音</p><p>女王音</p><p>御姐音</p><p>少女音</p><p>萝莉音</p><p>大妈音</p> |
| confidence   | int      | Y            | 对应音色标签可能性大小，取值0-100，数值越高表示概率越大。                                                                                            

*auxInfo数组中每一项具体参数如下：*

| **参数名称**  | **类型** | **是否必返** | **说明**                                 |
| :------------| :-----  | :----------| :--------------------------------------- |
| errorCode    | int     | Y          |<p>状态码</p><p>2003：音频下载失败</p><p>2007：无有效数据</p>|

## **接口响应码列表**
code和message的列表如下：

| **code** | **message** |
| :------- | :---------- |
| 1100     | 成功        |
| 1101     | 正在处理中  |
| 1901     | QPS超限     |
| 1902     | 参数不合法  |
| 1903     | 服务失败    |
| 1904     | 下载失败    |
| 1905     | 处理失败    |
| 9100     | 余额不足    |
| 9101     | 无权限操作  |

## 示例

### 上传接口请求示例
#### 上传url音频数据请求示例

```json
{
    "accessKey":"****************",
    "type": "DEFAULT",
    "appId": "default",
    "btId": "test01",
    "data": {
        "tokenId": "test_01",
        "url": "http://xxxxxxxx.mp3",
        "channel": "IM_MESSAGE",
        "returnAllText": true
    },
    "callback": "http://xxxxxxxxx",
    "callbackParam": {
        " param1": 1,
        " param2": "qew",
        " param3": true
    }
}
```
#### 上传base64音频数据请求示例

```json
{
    "accessKey": "****************",
    "type": "DEFAULT",
    "appId": "default",
    "btId": "test01",
    "data": {
        "content": "音频base64编码数据",
        "formatInfo": {
            "format": "pcm",
            "rate": 16000,
            "track": 1
        },
        "channel": "IM_MESSAGE",
        "tokenId": "asdwef",
        "returnAllText": true,
    },
    "callback": "http://xxxxxx",
    "callbackParam": {
        "param1": 1,
        "param2": "qew",
        "param3": true
    }
}
```

### 上传接口返回示例

```json
{
    "code":1100,
    "message":"成功",
    "requestId":" XXXXXXXXXXXX",
    "btId":"XXXXXXX"
}
```

### 主动查询请求示例

```json
{
    "accessKey":"*************",
    "btId":"xxxx"
}
```

### 主动查询返回示例

```json
{
    "code": 1100,
    "message": "成功",
    "requestId": "b891cf2d82e214de45df33fc2bea4875",
    "btId": "xxxx",
    "riskLevel": "REJECT",
    "audioText": "法轮大法好",
    "labels": "涉政-音频",
    "detail": [
        {
            "audioStarttime": 10,
            "audioEndtime": 20,
            "audioUrl": "<http://xxxxxxxx.>wav ",
            "audioText": "法轮大法好",
            "riskLevel": "REJECT",
            "businessLabels":[
                {
                    "businessDescription":"语种:其他语种:其他语种",
                    "businessDetail":{

                    },                  
                    "businessLabel1":"language",
                    "businessLabel2":"Languageother",
                    "businessLabel3":"Languageother",
                    "confidenceLevel":2,
                    "probability":0.854025190610373
                }
            ],          
            "audioMatchedItem": "法轮",
            "riskType": 100,
            "description": "涉政-音频"
        }
    ],
    "tags": [
        {
            "label": "男性",
            "confidence": 90
        },
        {
            "label": "大叔音",
            "confidence": 91
        },
        {
            "label": "正太音",
            "confidence": 21
        },
        {
            "label": "老年音",
            "confidence": 11
        },
        {
            "label": "青年音",
            "confidence": 31
        }
      ]
}
```
