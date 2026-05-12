## 目录

- [同步接口](#同步接口)
    - [请求参数](#请求参数)
        - [请求URL](#请求url)
        - [字符编码格式](#字符编码格式)
        - [请求方法](#请求方法)
        - [建议超时时长](#建议超时时长)
        - [请求参数](#请求参数-1)
    - [同步返回结果](#同步返回结果)
    - [回调返回结果](#回调返回结果)
    - [示例](#示例)
- [结果查询接口](#结果查询接口)
    - [请求参数](#请求参数-2)
        - [请求URL](#请求url-1)
        - [字符编码格式](#字符编码格式-1)
        - [请求方法](#请求方法-1)
        - [建议超时时长](#建议超时时长-1)
        - [请求参数](#请求参数-3)
    - [同步返回结果](#同步返回结果-1)
    - [示例](#示例-1)

---

## 同步接口

### 请求参数

#### 请求URL：


| 集群   | URL                                                    |
| ---- | ------------------------------------------------------ |
| 北京   | `http://api-article-bj.fengkongcloud.com/webpage/v4`   |
| 弗吉尼亚 | `http://api-article-fjny.fengkongcloud.com/webpage/v4` |
| 新加坡  | `http://api-article-xjp.fengkongcloud.com/webpage/v4`  |


#### 字符编码格式：

`UTF-8`字符集编码

#### 请求方法：

`POST`

#### 建议超时时长：

15s

#### 请求参数：

放在HTTP Body中，采用Json格式，Body大小不可超过3.5M，具体参数如下：


| **请求参数名**               | **类型**      | **参数说明**     | **是否必传** | **规范**                                                                                                                                                                                                                                                                                                                                                               |
| ----------------------- | ----------- | ------------ | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| accessKey               | string      | 接口认证密钥       | Y        | 由数美提供，用于权限认证，开通服务时由数美提供                                                                                                                                                                                                                                                                                                                                              |
| appId                   | string      | 应用标识         | Y        | 用于区分应用，需要联系数美服务开通，请使用数美单独提供的传值为准                                                                                                                                                                                                                                                                                                                                     |
| eventId                 | string      | 事件标识         | Y        | 用于区分场景数据，需要联系数美服务开通，请使用数美单独提供的传值为准                                                                                                                                                                                                                                                                                                                                   |
| imgType                 | string      | 图片识别类型       | Y        | 可选值： `NONE`：不审核图片 `POLITY`：涉政识别 `EROTIC`：色情&性感违规识别 `VIOLENT`：暴恐&违禁识别 `QRCODE`：二维码识别 `ADVERT`：广告识别 `IMGTEXTRISK`：图片文字违规识别（如需要识别图片里文字的违规内容，务必传入图片文字违规识别功能） `BOCR`：OCR小语种识别支持和语种自动检测（仅限新加坡集群） 组合说明：除`NONE`以外都可以通过下划线组合，如`POLITY_QRCODE_ADVERT`用于涉政、二维码和广告组合识别。`NONE`不可以和其他type拼接                                                                                        |
| txtType                 | string      | 文本识别类型       | Y        | 可选值： `NONE`：不审核文本 `POLITY`：涉政检测 `VIOLENT`：暴恐检测 `BAN`：违禁检测 `EROTIC`：色情检测 `DIRTY`：辱骂检测 `ADVERT`：广告检测 `PRIVACY`：隐私检测 `ADLAW`：广告法检测 `MEANINGLESS`：无意义检测 `FRUAD`：网络诈骗检测 `UNPOACH`：高价值用户防挖检测 `TEXTMINOR`：未成年人内容检测 `TEXTRISK`：常规风险检测（包含：涉政、暴恐、违禁、色情、辱骂、广告、隐私、广告法、无意义） 组合说明：除`NONE`以外都可以通过下划线组合，如`TEXTRISK_FRUAD`。type间组合取并集，如`TEXTRISK_POLITY`按照常规风险检测处理。`NONE`不可以和其他type拼接 |
| audioType               | string      | 音频识别类型       | N        | 可选值： `NONE`：不审核音频 `AUDIOPOLITICAL`：一号领导人声纹识别 `POLITY`：涉政识别 `EROTIC`：色情识别 `ADVERT`：广告识别 `ADLAW`：广告法识别 `BAN`：违禁识别 `VIOLENT`：暴恐识别 `ANTHEN`：国歌识别 `MOAN`：娇喘识别 `DIRTY`：辱骂识别 `BANEDAUDIO`：违禁歌曲 `COPYRIGHTSONGS`：版权歌曲 组合说明：如需做组合识别，通过下划线连接即可，例如`POLITY_EROTIC_MOAN`用于涉政、色情和娇喘识别。建议传入：`POLITY_EROTIC_MOAN_ADVERT`。`NONE`不可以和其他type拼接                                          |
| videoImgType            | string      | 视频图片识别类型     | N        | 可选值： `NONE`：不审核视频图片 `POLITY`：涉政识别 `EROTIC`：色情&性感违规识别 `VIOLENT`：暴恐&违禁识别 `QRCODE`：二维码识别 `ADVERT`：广告识别 `IMGTEXTRISK`：图片文字违规识别 组合说明：如需识别多个功能，通过下划线连接，如`POLITY_QRCODE_ADVERT`用于涉政、二维码和广告组合识别。`NONE`不可以和其他type拼接                                                                                                                                                           |
| videoAudioType          | string      | 视频音频识别类型     | N        | 可选值： `NONE`：不检测视频中的音频 `POLITY`：涉政识别 `EROTIC`：色情识别 `ADVERT`：广告识别 `BAN`：违禁识别 `VIOLENT`：暴恐识别 `DIRTY`：辱骂识别 `ADLAW`：广告法识别 `MOAN`：娇喘识别 `AUDIOPOLITICAL`：一号领导人声纹识别 `ANTHEN`：国歌识别 `BANEDAUDIO`：违禁歌曲 组合说明：如需做组合识别，通过下划线连接即可，例如`POLITY_EROTIC`用于涉政和色情识别。`NONE`不可以和其他type拼接                                                                                                     |
| callback                | string      | 回调http接口     | N        | 指定回调url地址。当该字段非空时，服务将根据该字段回调通知用户审核结果（支持`http`/`https`）                                                                                                                                                                                                                                                                                                               |
| articleDoubleJumpConfig | json_object | 是否开启网页二跳审核方式 | N        | [详见articleDoubleJumpConfig参数](#articleDoubleJumpConfig)                                                                                                                                                                                                                                                                                                              |
| articleScreenShotConfig | json_object | 是否开启网页截屏审核方式 | N        | [详见articleScreenShotConfig参数](#articleScreenShotConfig)                                                                                                                                                                                                                                                                                                              |
| articleDynamicConfig    | json_object | 是否开启网页动态审核方式 | N        | [详见articleDynamicConfig参数](#articleDynamicConfig)                                                                                                                                                                                                                                                                                                                    |
| data                    | json_object | 请求的数据内容      | Y        | 最长1MB, [详见data参数](#data)                                                                                                                                                                                                                                                                                                                                             |


其中，data的内容如下：


| **请求参数名**           | **类型**      | **参数说明**          | **是否必传** | **规范**                                                                                                                                                                                                                                                        |
| ------------------- | ----------- | ----------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| url                 | string      | 要检测的网页链接          | N        | 网址链接可下载，其中网址头部的content-type需为text/html。 内容大小`500M`以内，文本长度默认限制`50万`字，图片张数默认限制`500张`。（url、text、contents传且只能传其中一个）                                                                                                                                               |
| text                | string      | 要检测的网页文本          | N        | 纯文本内容审核，文本长度限制50w字。（url、text、contents传且只能传其中一个）                                                                                                                                                                                                               |
| contents            | string      | 要检测的网页源码          | N        | 网址源码审核，文本长度默认限制`50万`字，图片张数默认限制`500张`。（url、text、contents传且只能传其中一个）                                                                                                                                                                                             |
| lang                | string      | 待检测的文本内容语种        | N        | 可选值和对应语种如下： `zh`：中文 `en`：英文 `ar`：阿拉伯语 `hi`：印地语 `es`：西班牙语 `fr`：法语 `ru`：俄语 `pt`：葡萄牙语 `id`：印尼语 `de`：德语 `ja`：日语 `tr`：土耳其语 `vi`：越南语 `it`：意大利语 `th`：泰语 `tl`：菲律宾语 `ko`：韩语 `ms`：马来语 `auto`：自动识别语种类型 注意：默认值为`zh`。国内集群客户可不传或传入`zh`；海外文本内容如无法确定语种，建议传入`auto`，系统将自动检测语种类型 |
| acceptLang          | string      | 返回标签的语种类型         | N        | 选择返回标签的语种类型 可选值： `zh`：中文 `en`：英文 不传入默认为返回中文标签                                                                                                                                                                                                                 |
| returnAllImg        | int         | 返回图片的等级           | N        | 可选值： `0`：返回风险等级为非pass的图片 `1`：返回所有风险等级的图片 默认值为`0`                                                                                                                                                                                                              |
| returnAllText       | int         | 返回文本的等级           | N        | 可选值： `0`：返回风险等级为非pass的文本 `1`：返回所有风险等级的文本 默认值为`0`                                                                                                                                                                                                              |
| returnAllVideoImg   | int         | 返回视频里的图片的等级       | N        | 可选值： `0`：返回风险等级为非pass的图片 `1`：返回所有风险等级的图片 默认值为`0`                                                                                                                                                                                                              |
| returnAllVideoAudio | int         | 返回视频里的音频的等级       | N        | 可选值： `0`：返回风险等级为非pass的音频 `1`：返回所有风险等级的音频 默认值为`0`                                                                                                                                                                                                              |
| returnAllVideo      | int         | 返回视频的等级           | N        | 可选值： `0`：返回风险等级为非pass的视频 `1`：返回所有风险等级的视频 默认值为`0`                                                                                                                                                                                                              |
| returnAllAudio      | int         | 返回音频片段的等级         | N        | 可选值： `0`：返回风险等级为非pass的音频 `1`：返回所有风险等级的音频 默认值为`0`                                                                                                                                                                                                              |
| tokenId             | string      | 用户账号标识            | Y        | 由数字、字母、下划线、短杠组成的长度小于等于64位的字符串。建议使用贵司用户UID（可加密）自行生成，标识用户唯一身份用作灌水和广告等行为维度风控。如无用户uid的场景建议使用唯一的数据标识传值                                                                                                                                                             |
| receiveTokenId      | string      | 私聊场景下消息接收者的用户唯一标识 | N        | 由数字、字母、下划线、短杠组成的字符串，长度小于等于64位                                                                                                                                                                                                                                 |
| level               | int         | 用户等级              | N        | 针对不同等级的用户可配置不同拦截策略。可选值： `0`：最低级用户，典型如新注册、完全不活跃或等级为0的用户等 `1`：较低级用户，典型如低活跃或低等级用户等 `2`：中等级用户，典型如具备一定活跃或等级中等的用户等 `3`：较高级用户，典型如高活跃或高等级用户等 `4`：最高级用户，典型如付费用户、VIP用户等                                                                                                 |
| gender              | string      | 用户性别              | N        | 可选值： `male`：男性 `female`：女性                                                                                                                                                                                                                                    |
| nickname            | string      | 用户昵称              | N        | 校验昵称内容风险                                                                                                                                                                                                                                                      |
| ip                  | string      | IP地址              | N        | 发送该文本的用户公网IPv4或IPv6地址                                                                                                                                                                                                                                         |
| deviceId            | string      | 数美设备标识            | N        | 数美设备指纹生成的设备唯一标识                                                                                                                                                                                                                                               |
| dataId              | string      | 数据标识              | N        | 数据唯一标识，用于关联和追踪数据                                                                                                                                                                                                                                              |
| extra               | json_object | 辅助参数              | N        | 用于辅助文本检测的相关信息，[详见extra参数](#extra)                                                                                                                                                                                                                             |


其中，data 下 extra数组每个元素的内容如下：


| **请求参数名**   | **类型** | **参数说明**   | **是否必传** | **规范**                                                       |
| ----------- | ------ | ---------- | -------- | ------------------------------------------------------------ |
| room        | string | 直播间/游戏房间编号 | N        | 传入的是直播间、聊天室等数据（eventId值为groupChat）时，开启上下文识别功能，建议传入，否则不能关联上下文 |
| passThrough | Json   | 透传字段       | N        | 该字段内容会随着返回值一起返回                                              |


其中，articleDoubleJumpConfig的内容如下：


| **请求参数名** | **类型** | **参数说明**   | **是否必传** | **规范** |
| --------- | ------ | ---------- | -------- | ------ |
| isOpen    | bool   | 是否开启网页二跳审核 | N        | 默认不开启  |


其中，articleScreenShotConfig的内容如下：


| **请求参数名**     | **类型** | **参数说明**   | **是否必传** | **规范**                                                                                                                  |
| ------------- | ------ | ---------- | -------- | ----------------------------------------------------------------------------------------------------------------------- |
| isOpen        | bool   | 是否开启网页截图审核 | N        | 默认不开启                                                                                                                   |
| width         | int    | 截图的宽度      | N        | 默认截图宽度1080                                                                                                              |
| height        | int    | 截图的高度      | N        | 默认截图高度6480                                                                                                              |
| timeoutSecond | int    | 截图的超时时间    | N        | 默认超时时间30s                                                                                                               |
| userAgent     | string | 浏览器类型      | N        | 默认"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36" |


其中，articleDynamicConfig的内容如下：


| **请求参数名** | **类型** | **参数说明**      | **是否必传** | **规范**                                                                                                                  |
| --------- | ------ | ------------- | -------- | ----------------------------------------------------------------------------------------------------------------------- |
| isOpen    | bool   | 是否开启动态网页审核    | N        | 默认不开启                                                                                                                   |
| userAgent | string | 浏览器类型         | N        | 默认"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36" |
| cookie    | string | 动态网页审核的cookie | N        | 默认""                                                                                                                    |


#### 同步返回结果

**说明**：调用接口时会立即返回以下基础响应参数。若请求参数中传入了`callback`回调地址，完整的结果数据将通过回调接口异步返回。

放在HTTP Body中，采用Json格式，具体参数如下：


| **参数名称**  | **类型** | **参数说明** | **是否必返** | **规范**                                                                   |
| --------- | ------ | -------- | -------- | ------------------------------------------------------------------------ |
| code      | int    | 返回码      | Y        | `1100`：成功 `1901`：QPS超限 `1902`：参数不合法 `1903`：服务失败 `9100`：余额不足 `9101`：无权限操作 |
| message   | string | 返回码描述    | Y        | 和code对应： 成功 QPS超限 参数不合法 服务失败 余额不足 无权限操作                                  |
| requestId | string | 请求标识     | Y        | 本次请求数据的唯一标识，用于问题排查和效果优化，强烈建议保存                                           |


#### 回调返回结果

**说明**：当请求参数中传入了`callback`回调地址时，完整的结果数据将通过回调接口异步返回。以下参数中，除code、message、requestId以外，其他必返参数仅在code返回1100时返回。

放在HTTP Body中，采用Json格式，具体参数如下：


| **参数名称**     | **类型**      | **参数说明**  | **是否必返** | **规范**                                                                                                      |
| ------------ | ----------- | --------- | -------- | ----------------------------------------------------------------------------------------------------------- |
| code         | int         | 返回码       | Y        | `1100`：成功 `1901`：QPS超限 `1902`：参数不合法 `1903`：服务失败 `1905`：字数超限 `9100`：余额不足 `9101`：无权限操作                        |
| message      | string      | 返回码描述     | Y        | 和code对应： 成功 QPS超限 参数不合法 服务失败 字数超限 余额不足 无权限操作                                                                |
| requestId    | string      | 请求标识      | Y        | 本次请求数据的唯一标识，用于问题排查和效果优化，强烈建议保存                                                                              |
| riskLevel    | string      | 处置建议      | Y        | 可能返回值： `PASS`：正常，建议直接放行 `REVIEW`：可疑，建议人工审核 `REJECT`：违规，建议直接拦截                                               |
| textDetails  | json_array  | 文本风险详情    | Y        | 网页中文本的风险详情，[详见textDetails参数](#textDetails)                                                                  |
| imgDetails   | json_array  | 图片风险详情    | Y        | 网页中图片的风险详情，[详见imgDetails参数](#imgDetails)                                                                    |
| audioDetails | json_array  | 音频风险详情    | Y        | 网页中音频的风险详情，[详见audioDetails参数](#audioDetails)                                                                |
| videoDetails | json_array  | 视频风险详情    | Y        | 网页中视频的风险详情，[详见videoDetails参数](#videoDetails)                                                                |
| auxInfo      | json_object | 辅助信息      | Y        | [详见auxInfo参数](#auxInfo)                                                                                     |
| resultType   | int         | 结果类型      | Y        | 当前结果类型 `0`：机审 `1`：人审                                                                                        |
| finalResult  | int         | 是否为最终审核结果 | Y        | 是否为最终审核结果（如仅接入机审，则默认返回1）。 `0`：非最终结果。说明该结果为数美风控的机审结果，还需要经过数美人审再次审核后回传贵司。 `1`：最终结果。贵司可直接拿返回结果进行处置、分发等下游场景的使用。 |


其中，textDetails的内容如下：


| **参数名称**           | **类型**      | **参数说明**   | **是否必返** | **规范**                                                        |
| ------------------ | ----------- | ---------- | -------- | ------------------------------------------------------------- |
| code               | int         | 返回码        | Y        | `1100`：成功 `1901`：QPS超限 `1902`：参数不合法 `1903`：服务失败               |
| message            | string      | 返回码描述      | Y        | 和code对应：成功、QPS超限、参数不合法、服务失败、字数超限                              |
| requestId          | string      | 请求标识       | Y        | 本次请求数据的唯一标识，用于问题排查和效果优化，强烈建议保存                                |
| riskLevel          | string      | 处置建议       | Y        | 可能返回值： `PASS`：正常，建议直接放行 `REVIEW`：可疑，建议人工审核 `REJECT`：违规，建议直接拦截 |
| riskLabel1         | string      | 一级风险标签     | Y        | 一级风险标签，当riskLevel为`PASS`时返回`normal`                           |
| riskLabel2         | string      | 二级风险标签     | Y        | 二级风险标签，当riskLevel为`PASS`时为空                                   |
| riskLabel3         | string      | 三级风险标签     | Y        | 三级风险标签，当riskLevel为`PASS`时为空                                   |
| riskDescription    | string      | 风险原因       | Y        | 当riskLevel为`PASS`时为"正常"                                       |
| riskDetail         | json_object | 风险详情       | Y        | 风险详情，[详见riskDetail参数](#riskDetail)                            |
| tokenLabels        | json_object | 账号风险画像标签信息 | Y        | 账号风险画像标签信息，[详见tokenLabels参数](#tokenLabels)                    |
| auxInfo            | json_object | 辅助信息       | Y        | 辅助信息，[详见auxInfo参数](#textDetailsAuxInfo)                       |
| allLabels          | json_array  | 风险标签列表     | Y        | 命中的所有风险标签以及详情信息，[详见allLabels参数](#allLabels)                   |
| businessLabels     | json_array  | 业务标签       | Y        | 命中的所有业务标签以及详细信息，[详见businessLabels参数](#businessLabels)         |
| tokenProfileLabels | json_array  | 账号属性标签     | N        | 属性账号类标签，仅在提供tokenId并开通标签服务时返回，[详见账号标签参数](#tokenProfileLabels) |
| tokenRiskLabels    | json_array  | 账号风险标签     | N        | 风险账号类标签，仅在提供tokenId并开通标签服务时返回，[详见账号标签参数](#tokenProfileLabels) |
| langResult         | json_object | 语种信息       | N        | 语种识别和翻译结果，[详见langResult参数](#langResult)                       |


其中，textDetails的riskDetail的内容如下：


| **参数名称**     | **类型**     | **参数说明**     | **是否必返** | **规范**                                                         |
| ------------ | ---------- | ------------ | -------- | -------------------------------------------------------------- |
| matchedLists | json_array | 命中的客户自定义名单列表 | N        | 仅在命中客户自定义名单时返回，[详见matchedLists参数](#matchedLists)               |
| riskSegments | json_array | 高风险内容片段      | N        | 检测文本包含涉政、暴恐、违禁、广告法等风险内容的时候存在，[详见riskSegments参数](#riskSegments) |


其中，textDetails的riskDetail下matchedLists数组每个元素的内容如下：


| **参数名称** | **类型**     | **参数说明** | **是否必返** | **规范**                       |
| -------- | ---------- | -------- | -------- | ---------------------------- |
| name     | string     | 命中的名单名称  | N        | 命中的名单名称                      |
| words    | json_array | 命中的敏感词数组 | N        | 命中的敏感词数组，[详见words参数](#words) |


其中，matchedLists中，words数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明** | **是否必返** | **规范**  |
| -------- | --------- | -------- | -------- | ------- |
| word     | string    | 命中的敏感词   | N        | 命中的敏感词  |
| position | int_array | 敏感词所在位置  | N        | 敏感词所在位置 |


其中，textDetails的riskDetail下riskSegments数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明**    | **是否必返** | **规范**      |
| -------- | --------- | ----------- | -------- | ----------- |
| segment  | string    | 高风险内容片段     | N        | 高风险内容片段     |
| position | int_array | 高风险内容片段所在位置 | N        | 高风险内容片段所在位置 |


其中，allLabels的内容如下：


| **参数名称**        | **类型**      | **参数说明** | **是否必返** | **规范**                                                                        |
| --------------- | ----------- | -------- | -------- | ----------------------------------------------------------------------------- |
| riskLevel       | string      | 风险等级     | Y        | 可能返回值： `REVIEW`：可疑 `REJECT`：违规                                                |
| riskLabel1      | string      | 一级风险标签   | Y        | allLabels不为空时必返                                                               |
| riskLabel2      | string      | 二级风险标签   | Y        | allLabels不为空时必返                                                               |
| riskLabel3      | string      | 三级风险标签   | Y        | allLabels不为空时必返                                                               |
| riskDescription | string      | 风险原因     | Y        | allLabels不为空时必返                                                               |
| probability     | float       | 置信度      | Y        | 可选值在0～1之间，值越大，可信度越高。注意：allLabels不为空时必返                                        |
| riskDetail      | json_object | 映射后风险详情  | Y        | 格式与上层riskDetail结构相同。注意：allLabels不为空时必返，[详见riskDetail参数](#allLabelsRiskDetail) |


其中，allLabels的riskDetail的内容如下：


| **参数名称**     | **类型**     | **参数说明**     | **是否必返** | **规范**                                                                          |
| ------------ | ---------- | ------------ | -------- | ------------------------------------------------------------------------------- |
| matchedLists | json_array | 命中的客户自定义名单列表 | N        | 命中的客户自定义名单列表，[详见matchedLists参数](#allLabelsMatchedLists)                         |
| riskSegments | json_array | 高风险内容片段      | N        | 高风险内容片段，检测文本包含涉政、暴恐、违禁、广告法等风险内容的时候存在，[详见riskSegments参数](#allLabelsRiskSegments) |


其中，allLabels的riskDetail下matchedLists数组每个元素的内容如下：


| **参数名称** | **类型**     | **参数说明** | **是否必返** | **规范**                                |
| -------- | ---------- | -------- | -------- | ------------------------------------- |
| name     | string     | 命中的名单名称  | N        | 命中的名单名称                               |
| words    | json_array | 命中的敏感词数组 | N        | 命中的敏感词数组，[详见words参数](#allLabelsWords) |


其中，allLabels的riskDetail下matchedLists的words数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明** | **是否必返** | **规范**  |
| -------- | --------- | -------- | -------- | ------- |
| word     | string    | 命中的敏感词   | N        | 命中的敏感词  |
| position | int_array | 敏感词所在位置  | N        | 敏感词所在位置 |


其中，allLabels的riskDetail下riskSegments数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明**    | **是否必返** | **规范**      |
| -------- | --------- | ----------- | -------- | ----------- |
| segment  | string    | 高风险内容片段     | N        | 高风险内容片段     |
| position | int_array | 高风险内容片段所在位置 | N        | 高风险内容片段所在位置 |


其中，tokenLabels的内容如下：


| **参数名称**         | **类型**      | **参数说明**  | **是否必返** | **规范**                                                       |
| ---------------- | ----------- | --------- | -------- | ------------------------------------------------------------ |
| UGC_account_risk | json_object | UGC内容相关风险 | N        | UGC内容相关风险，[详见UGC_account_risk参数](#tokenLabelsUGCAccountRisk) |


其中，tokenLabels的UGC_account_risk的内容如下：


| **参数名称**          | **类型** | **参数说明** | **是否必返** | **规范**            |
| ----------------- | ------ | -------- | -------- | ----------------- |
| sexy_risk_tokenid | float  | 色情账号风险分  | N        | 色情账号风险分，取值区间[0-1] |


其中，textDetails的auxInfo的内容如下：


| **参数名称**         | **类型**      | **参数说明**      | **是否必返** | **规范**                                                                           |
| ---------------- | ----------- | ------------- | -------- | -------------------------------------------------------------------------------- |
| filteredText     | string      | 风险片段被替换为*后的文本 | N        | 风险片段被替换为*后的文本                                                                    |
| passThrough      | json_object | 透传字段          | N        | 该字段内容与请求参数data中extra的passThrough的值相同                                             |
| contactResult    | json_array  | 联系方式识别结果      | N        | 联系方式识别结果，包含识别出的微信、QQ、手机号的字符串类型和内容，[详见contactResult参数](#textDetailsContactResult) |
| contextText      | string      | 上下文文本         | N        | 上下文生效时返回                                                                         |
| unauthorizedType | string      | 未授权的type      | N        | 未授权的type                                                                         |


其中，textDetails的auxInfo的contactResult数组每个元素的内容如下：


| **参数名称**      | **类型** | **参数说明** | **是否必返** | **规范**                              |
| ------------- | ------ | -------- | -------- | ----------------------------------- |
| contactType   | int    | 联系方式类型   | N        | 联系方式类型，可选值： `0`：手机号 `1`：QQ号 `2`：微信号 |
| contactString | string | 联系方式串    | N        | 联系方式串                               |


其中，businessLabels的内容如下：


| **参数名称**            | **类型**      | **参数说明** | **是否必返** | **规范**                                  |
| ------------------- | ----------- | -------- | -------- | --------------------------------------- |
| businessLabel1      | string      | 一级业务标签   | Y        | businessLabels不为空必返                     |
| businessLabel2      | string      | 二级业务标签   | Y        | businessLabels不为空必返                     |
| businessLabel3      | string      | 三级业务标签   | Y        | businessLabels不为空必返                     |
| businessDescription | string      | 标签描述     | Y        | businessLabels不为空必返                     |
| probability         | float       | 置信度      | Y        | 可选值在0～1之间，值越大，可信度越高。businessLabels不为空必返 |
| businessDetail      | json_object | 业务详情     | Y        | businessLabels不为空必返                     |


其中，langResult的内容如下：


| **参数名称**       | **类型** | **参数说明** | **是否必返** | **规范**                                                       |
| -------------- | ------ | -------- | -------- | ------------------------------------------------------------ |
| detectedLang   | string | 语种识别结果   | N        | 当在国际化文本产品下传入lang的值为`auto`时返回该字段。值为标准语言代码表，例如：`zh`、`en`、`ar`等 |
| translatedText | string | 文本翻译结果   | N        | 当传入`translationTargetLang`时返回该字段。值为翻译后的文本                    |


其中，tokenProfileLabels、tokenRiskLabels的内容如下：


| 参数名称        | 类型     | 参数说明   | 是否必返 | 规范               |
| ----------- | ------ | ------ | ---- | ---------------- |
| label1      | string | 一级标签   | N    |                  |
| label2      | string | 二级标签   | N    |                  |
| label3      | string | 三级标签   | N    |                  |
| description | string | 标签描述   | N    |                  |
| timestamp   | int    | 打标签时间戳 | N    | 13位Unix时间戳，单位：毫秒 |


其中，imgDetails的内容如下：


| **参数名称**           | **类型**      | **参数说明**        | **是否必返** | **规范**                                                                             |
| ------------------ | ----------- | --------------- | -------- | ---------------------------------------------------------------------------------- |
| requestId          | string      | 请求标识            | Y        | 本次请求数据的唯一标识，用于问题排查和效果优化，强烈建议保存                                                     |
| message            | string      | 返回码描述           | Y        | 和code对应：成功、QPS超限、参数不合法、服务失败、字数超限                                                   |
| code               | int         | 返回码             | Y        | `1100`：成功 `1901`：QPS超限 `1902`：参数不合法 `1903`：服务失败 `1911`：图片下载失败                      |
| riskLevel          | string      | 处置建议            | Y        | 可能返回值： `PASS`：正常，建议直接放行 `REVIEW`：可疑，建议人工审核 `REJECT`：违规，建议直接拦截                      |
| riskLabel1         | string      | 一级风险标签          | Y        | 一级风险标签，当riskLevel为`PASS`时返回`normal`                                                |
| riskLabel2         | string      | 二级风险标签          | Y        | 二级风险标签，当riskLevel为`PASS`时为空                                                        |
| riskLabel3         | string      | 三级风险标签          | Y        | 三级风险标签，当riskLevel为`PASS`时为空                                                        |
| riskDescription    | string      | 风险原因            | Y        | 当riskLevel为`PASS`时为"正常"                                                            |
| resultType         | int         | 当前结果是机审还是人审环节结果 | Y        | 可选值： `0`：机审 `1`：人审                                                                 |
| finalResult        | int         | 是否最终结果          | Y        | 可选值： `0`：不是最终结果，该结果为数美风控的过程结果，还需要经过数美人审再次check后回传 `1`：最终结果，可直接拿返回结果进行处置、分发等下游场景的使用 |
| auxInfo            | json_object | 辅助信息            | Y        | 辅助信息，[详见auxInfo参数](#imgDetailsAuxInfo)                                             |
| allLabels          | json_array  | 风险标签列表          | Y        | 命中的所有风险标签以及详情信息，[详见allLabels参数](#imgDetailsAllLabels)                              |
| businessLabels     | json_array  | 业务标签            | N        | 命中的所有业务标签以及详细信息，[详见businessLabels参数](#imgDetailsBusinessLabels)                    |
| riskDetail         | json_object | 风险详情            | Y        | 风险详情，[详见riskDetail参数](#imgDetailsRiskDetail)                                       |
| disposal           | json_object | 处置和映射结果         | N        | 数美可按照贵司的标签体系和标识进行返回；未配置自定义标签体系则不返回该字段，[详见disposal参数](#imgDetailsDisposal)          |
| tokenLabels        | json_object | 账号风险画像标签信息      | Y        | 账号风险画像标签信息，仅在提供tokenId并联系数美开通时返回，[详见tokenLabels参数](#imgDetailsTokenLabels)         |
| tokenProfileLabels | json_array  | 辅助信息            | N        | 属性账号类标签，仅在提供tokenId并开通标签服务时返回。[详见账号标签参数](#tokenProfileLabels)                      |
| tokenRiskLabels    | json_array  | 辅助信息            | N        | 风险账号类标签，仅在提供tokenId并开通标签服务时返回。[详见账号标签参数](#tokenProfileLabels)                      |


其中，imgDetails的riskDetail的内容如下：


| **参数名称**   | **类型**      | **参数说明**        | **是否必返** | **规范**                                                                                             |
| ---------- | ----------- | --------------- | -------- | -------------------------------------------------------------------------------------------------- |
| riskSource | int         | 风险来源            | Y        | 可能取值： `1000`：无风险 `1001`：文字风险 `1002`：视觉图片风险                                                         |
| face_num   | int         | 人脸数量            | N        | 人脸数量                                                                                               |
| person_num | int         | 人像数量            | N        | 人像数量                                                                                               |
| faces      | json_array  | 图片中涉政人物的名称及位置信息 | N        | 当命中人脸-人脸类型-多人脸时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个），[详见faces参数](#imgDetailsRiskDetailFaces) |
| objects    | json_array  | 物品信息            | N        | 返回图片中标识或物品的名称及位置信息，[详见objects参数](#imgDetailsRiskDetailObjects)                                     |
| ocrText    | json_object | 返回图片中文字识别内容     | N        | 当请求参数imgType字段包含`IMGTEXTRISK`或`ADVERT`时存在，[详见ocrText参数](#imgDetailsRiskDetailOcrText)              |
| persons    | json_array  | 图片中人物的名称及位置信息   | N        | 仅当命中人像-多人时，数组元素会有多个，最多10个，[详见persons参数](#imgDetailsRiskDetailPersons)                              |


其中，imgDetails的riskDetail下faces数组每个元素的内容如下：


| **参数名称**    | **类型**    | **参数说明** | **是否必返** | **规范**                                                                                                  |
| ----------- | --------- | -------- | -------- | ------------------------------------------------------------------------------------------------------- |
| face_ratio  | float     | 人脸占比     | N        | 在区间0-1，数值越大，人脸占比越高                                                                                      |
| id          | string    | 人物编号     | N        | 图片同一个位置下的人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID                                                             |
| name        | string    | 人物名称     | N        | 能识别的公众人物名称                                                                                              |
| probability | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                                                                                     |
| location    | int_array | 人物位置信息   | N        | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567] 207代表的是左上角的x坐标 522代表左上角的y坐标 340代表的是右下角的x坐标 567代表的是右下角的y坐标 |


其中，imgDetails的riskDetail下objects数组每个元素的内容如下：


| **参数名称**    | **类型**    | **参数说明** | **是否必返** | **规范**                                                                                                  |
| ----------- | --------- | -------- | -------- | ------------------------------------------------------------------------------------------------------- |
| id          | string    | 物品或标识编号  | N        | 保证同一个位置下的物品在不同标签下的编号相同                                                                                  |
| name        | string    | 物品名称     | N        | 物品名称                                                                                                    |
| probability | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                                                                                     |
| qrContent   | string    | 二维码地址    | N        | 仅当命中二维码相关标签时返回                                                                                          |
| location    | int_array | 物品位置信息   | N        | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567] 207代表的是左上角的x坐标 522代表左上角的y坐标 340代表的是右下角的x坐标 567代表的是右下角的y坐标 |


其中，imgDetails的riskDetail下persons数组每个元素的内容如下：


| **参数名称**     | **类型**    | **参数说明** | **是否必返** | **规范**                                  |
| ------------ | --------- | -------- | -------- | --------------------------------------- |
| id           | string    | 人物编号     | N        | 保证同一个人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID |
| person_ratio | float     | 人像占比     | N        | 在区间0-1，数值越大，人脸占比越高                      |
| probability  | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                     |
| location     | int_array | 人像位置坐标   | N        | 人像位置坐标                                  |


其中，imgDetails的riskDetail下ocrText的内容如下：


| **参数名称**     | **类型**     | **参数说明**     | **是否必返** | **规范**                                                                             |
| ------------ | ---------- | ------------ | -------- | ---------------------------------------------------------------------------------- |
| text         | string     | 图片中识别出的文字    | Y        | 图片中识别出的文字                                                                          |
| matchedLists | json_array | 命中的客户自定义名单列表 | N        | 仅在命中客户自定义名单时返回，[详见matchedLists参数](#imgDetailsRiskDetailMatchedLists)               |
| riskSegments | json_array | 高风险片段内容      | N        | 检测图片包含涉政、暴恐、违禁、广告法等风险内容的时候存在，[详见riskSegments参数](#imgDetailsRiskDetailRiskSegments) |


其中，imgDetails的riskDetail下ocrText的matchedLists数组每个元素的内容如下：


| **参数名称** | **类型**     | **参数说明** | **是否必返** | **规范**                                                 |
| -------- | ---------- | -------- | -------- | ------------------------------------------------------ |
| name     | string     | 命中的名单名称  | N        | 命中的名单名称                                                |
| words    | json_array | 命中的敏感词信息 | N        | 命中的这个名单中的敏感词信息，[详见words参数](#imgDetailsRiskDetailWords) |


其中，imgDetails的riskDetail下ocrText的matchedLists的words数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明** | **是否必返** | **规范**  |
| -------- | --------- | -------- | -------- | ------- |
| word     | string    | 命中的敏感词   | N        | 命中的敏感词  |
| position | int_array | 敏感词所在位置  | N        | 敏感词所在位置 |


其中，imgDetails的riskDetail下ocrText的riskSegments数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明**    | **是否必返** | **规范**               |
| -------- | --------- | ----------- | -------- | -------------------- |
| segment  | string    | 高风险内容片段     | N        | 高风险内容片段              |
| position | int_array | 高风险内容片段所在位置 | N        | 高风险内容片段所在位置，下标从0开始计数 |


其中，imgDetails的auxInfo的内容如下：


| **参数名称**    | **类型**      | **参数说明** | **是否必返** | **规范**                                                                                                                             |
| ----------- | ----------- | -------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| segments    | int         | 实际处理的片段数 | Y        | 实际处理的片段数                                                                                                                           |
| typeVersion | json_object | 模型版本号    | Y        | 对应传入type值的模型版本号，[详见typeVersion参数](#imgDetailsTypeVersion)                                                                          |
| errorCode   | int         | 错误码      | N        | 可选值： `2001`：输入数据格式不正确，不是有效的JSON数据 `2002`：输入参数字段非法（缺少必填字段、类型错误、取值非法等） `2003`：图片下载失败 `2004`：图片过大，超过10M `2005`：非法图片格式 `2006`：非法风险监控类型 |
| frameTime   | int         | 截帧时间戳    | N        | 当请求参数传入`streamInfo`功能时返回相似功能。13位毫秒级时间戳                                                                                             |
| qrContent   | string      | 二维码地址    | N        | 返回图片中识别的二维码地址                                                                                                                      |
| streamId    | string      | 流标识      | N        | 请求参数中包含streamInfo等参数，传入后会返回相似功能                                                                                                    |
| passThrough | json_object | 透传字段     | N        | 客户传入的透传字段。数美内部不会识别处理该字段，随结果返回给用户                                                                                                   |


其中，imgDetails的auxInfo的typeVersion的内容如下：


| **参数名称**    | **类型** | **参数说明**  | **是否必返** | **规范**                                                                                        |
| ----------- | ------ | --------- | -------- | --------------------------------------------------------------------------------------------- |
| ADVERT      | string | 广告版本号     | N        | 格式为`X.Y`，其中`X`表示主版本号，通常表示模型整体效果迭代；`Y`表示次版本号，通常表示日常例行迭代。例如`1001001.2`表示主版本号为`1001001`，次版本号为`2` |
| EROTIC      | string | 色情版本号     | N        | 格式为`X.Y`，其中`X`表示主版本号，通常表示模型整体效果迭代；`Y`表示次版本号，通常表示日常例行迭代。例如`1001001.2`表示主版本号为`1001001`，次版本号为`2` |
| IMGTEXTRISK | string | 图片文字违规版本号 | N        | 格式为`X.Y`，其中`X`表示主版本号，通常表示模型整体效果迭代；`Y`表示次版本号，通常表示日常例行迭代。例如`1001001.2`表示主版本号为`1001001`，次版本号为`2` |
| POLITY      | string | 涉政版本号     | N        | 格式为`X.Y`，其中`X`表示主版本号，通常表示模型整体效果迭代；`Y`表示次版本号，通常表示日常例行迭代。例如`1001001.2`表示主版本号为`1001001`，次版本号为`2` |
| QRCODE      | string | 二维码版本号    | N        | 格式为`X.Y`，其中`X`表示主版本号，通常表示模型整体效果迭代；`Y`表示次版本号，通常表示日常例行迭代。例如`1001001.2`表示主版本号为`1001001`，次版本号为`2` |
| VIOLENT     | string | 暴恐版本号     | N        | 格式为`X.Y`，其中`X`表示主版本号，通常表示模型整体效果迭代；`Y`表示次版本号，通常表示日常例行迭代。例如`1001001.2`表示主版本号为`1001001`，次版本号为`2` |


其中，imgDetails的allLabels的内容如下：


| **参数名称**        | **类型**      | **参数说明** | **是否必返** | **规范**                                                                                              |
| --------------- | ----------- | -------- | -------- | --------------------------------------------------------------------------------------------------- |
| riskLevel       | string      | 风险等级     | Y        | 可能返回值： `REVIEW`：可疑 `REJECT`：违规                                                                      |
| riskLabel1      | string      | 一级风险标签   | Y        | allLabels不为空时必返                                                                                     |
| riskLabel2      | string      | 二级风险标签   | Y        | allLabels不为空时必返                                                                                     |
| riskLabel3      | string      | 三级风险标签   | Y        | allLabels不为空时必返                                                                                     |
| riskDescription | string      | 风险原因     | Y        | allLabels不为空时必返。当riskLevel为PASS时返回正常；其他情况展现形式为："一级标签:二级标签:三级标签"的中文名。仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |
| probability     | float       | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高。注意：allLabels不为空时必返                                                              |
| riskDetail      | json_object | 映射后风险详情  | Y        | 格式与imgDetails的riskDetail结构相同。注意：allLabels不为空时必返，[详见riskDetail参数](#imgDetailsAllLabelsRiskDetail)    |


其中，imgDetails的allLabels的riskDetail的内容如下：


| **参数名称**   | **类型**      | **参数说明**        | **是否必返** | **规范**                                                                                              |
| ---------- | ----------- | --------------- | -------- | --------------------------------------------------------------------------------------------------- |
| riskSource | int         | 风险来源            | Y        | 可能取值： `1000`：无风险 `1001`：文字风险 `1002`：视觉图片风险                                                          |
| face_num   | int         | 人脸数量            | N        | 人脸数量                                                                                                |
| person_num | int         | 人像数量            | N        | 人像数量                                                                                                |
| faces      | json_array  | 图片中涉政人物的名称及位置信息 | N        | 当命中人脸-人脸类型-多人脸时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个），[详见faces参数](#imgDetailsAllLabelsFaces)   |
| objects    | json_array  | 物品信息            | N        | 返回图片中标识或物品的名称及位置信息，[详见objects参数](#imgDetailsAllLabelsObjects)                                       |
| ocrText    | json_object | 返回图片中文字识别内容     | N        | 当请求参数imgType字段包含IMGTEXTRISK或ADVERT时存在，[详见ocrText参数](#imgDetailsAllLabelsOcrText)                    |
| persons    | json_array  | 图片中人物的名称及位置信息   | N        | 当命中'人像-多人'标签时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个），[详见persons参数](#imgDetailsAllLabelsPersons) |


其中，imgDetails的allLabels的riskDetail的faces数组每个元素的内容如下：


| **参数名称**    | **类型**    | **参数说明** | **是否必返** | **规范**                                                                                                  |
| ----------- | --------- | -------- | -------- | ------------------------------------------------------------------------------------------------------- |
| face_ratio  | float     | 人脸占比     | N        | 在区间0-1，数值越大，人脸占比越高                                                                                      |
| id          | string    | 编号       | N        | 图片同一个位置下的人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID                                                             |
| name        | string    | 人物名称     | N        | 人物名称                                                                                                    |
| probability | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                                                                                     |
| location    | int_array | 人物位置信息   | N        | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567] 207代表的是左上角的x坐标 522代表左上角的y坐标 340代表的是右下角的x坐标 567代表的是右下角的y坐标 |


其中，imgDetails的allLabels的riskDetail的objects数组每个元素的内容如下：


| **参数名称**    | **类型**    | **参数说明**      | **是否必返** | **规范**                                                                                                  |
| ----------- | --------- | ------------- | -------- | ------------------------------------------------------------------------------------------------------- |
| id          | string    | 物品或标识编号       | N        | 保证同一个位置下的物品在不同标签下的编号相同                                                                                  |
| name        | string    | 物品名称          | N        | 物品名称                                                                                                    |
| probability | float     | 置信度           | N        | 可选值在0～1之间，值越大，可信度越高                                                                                     |
| qrContent   | string    | 返回图片中识别的二维码地址 | N        | 返回图片中识别的二维码地址                                                                                           |
| location    | int_array | 物品位置信息        | N        | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567] 207代表的是左上角的x坐标 522代表左上角的y坐标 340代表的是右下角的x坐标 567代表的是右下角的y坐标 |


其中，imgDetails的allLabels的riskDetail的ocrText的内容如下：


| **参数名称**     | **类型**     | **参数说明**     | **是否必返** | **规范**                                                                               |
| ------------ | ---------- | ------------ | -------- | ------------------------------------------------------------------------------------ |
| text         | string     | 图片中识别出的文字    | Y        | 图片中识别出的文字                                                                            |
| matchedLists | json_array | 命中的客户自定义名单信息 | N        | 仅在命中客户自定义名单时返回，[详见matchedLists参数](#imgDetailsAllLabelsMatchedLists)                  |
| riskSegments | json_array | 高风险片段内容      | N        | 检测图片包含涉政、暴恐、违禁、竞品、广告法等风险内容的时候存在，[详见riskSegments参数](#imgDetailsAllLabelsRiskSegments) |


其中，imgDetails的allLabels的riskDetail的ocrText的matchedLists数组每个元素的内容如下：


| **参数名称** | **类型**     | **参数说明**       | **是否必返** | **规范**                                 |
| -------- | ---------- | -------------- | -------- | -------------------------------------- |
| name     | string     | 命中的名单名称        | N        | 命中的名单名称                                |
| words    | json_array | 命中的这个名单中的敏感词信息 | N        | [详见words参数](#imgDetailsAllLabelsWords) |


其中，imgDetails的allLabels的riskDetail的ocrText的matchedLists的words数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明** | **是否必返** | **规范**  |
| -------- | --------- | -------- | -------- | ------- |
| word     | string    | 命中的敏感词   | N        | 命中的敏感词  |
| position | int_array | 敏感词所在位置  | N        | 敏感词所在位置 |


其中，imgDetails的allLabels的riskDetail的ocrText的riskSegments数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明**    | **是否必返** | **规范**               |
| -------- | --------- | ----------- | -------- | -------------------- |
| segment  | string    | 高风险内容片段     | N        | 高风险内容片段              |
| position | int_array | 高风险内容片段所在位置 | N        | 高风险内容片段所在位置，下标从0开始计数 |


其中，imgDetails的allLabels的riskDetail的persons数组每个元素的内容如下：


| **参数名称**     | **类型**    | **参数说明** | **是否必返** | **规范**                                  |
| ------------ | --------- | -------- | -------- | --------------------------------------- |
| id           | string    | 编号       | N        | 保证同一个人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID |
| person_ratio | float     | 人像占比     | N        | 在区间0-1，数值越大，人脸占比越高                      |
| probability  | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                     |
| location     | int_array | 人像位置坐标   | N        | 人像位置坐标                                  |


其中，imgDetails的businessLabels数组每个元素的内容如下：


| **参数名称**            | **类型**      | **参数说明** | **是否必返** | **规范**                                          |
| ------------------- | ----------- | -------- | -------- | ----------------------------------------------- |
| businessDescription | string      | 业务标签描述   | Y        | 格式为"一级标签:二级标签:三级标签"的中文名称                        |
| businessLabel1      | string      | 一级业务标签   | Y        | 一级业务标签                                          |
| businessLabel2      | string      | 二级业务标签   | Y        | 二级业务标签                                          |
| businessLabel3      | string      | 三级业务标签   | Y        | 三级业务标签                                          |
| probability         | float       | 置信度      | Y        | 可选值在0～1之间，值越大，可信度越高                             |
| confidenceLevel     | int         | 置信等级     | N        | 可选值在0～2之间，值越大，可信度越高                             |
| businessDetail      | json_object | 业务标签详情   | N        | [详见businessDetail参数](#imgDetailsBusinessDetail) |


其中，imgDetails的businessLabels的businessDetail的内容如下：


| **参数名称**         | **类型**     | **参数说明**        | **是否必返** | **规范**                                                                                                   |
| ---------------- | ---------- | --------------- | -------- | -------------------------------------------------------------------------------------------------------- |
| face_compare_num | int        | 人脸对比数量          | N        | 当传入FACECOMPARE业务类型和imgCompareBase时存在。imgCompareBase检测传入图片中的人脸数量                                          |
| face_num         | int        | 人脸数量            | N        | 人脸数量                                                                                                     |
| face_ratio       | float      | 人脸占比            | N        | 在区间0-1，数值越大，人脸占比越高                                                                                       |
| name             | string     | 人物名称            | N        | 人物名称                                                                                                     |
| person_num       | int        | 人像数量            | N        | 人像数量                                                                                                     |
| person_ratio     | float      | 人像占比            | N        | 在区间0-1，数值越大，人脸占比越高                                                                                       |
| probability      | float      | 置信度             | N        | 可选值在0～1之间，值越大，可信度越高                                                                                      |
| faces            | json_array | 图片中涉政人物的名称及位置信息 | N        | 当命中人脸-人脸类型-多人脸时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个），[详见faces参数](#imgDetailsBusinessDetailFaces)   |
| location         | int_array  | 位置信息            | N        | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567] 207代表的是左上角的x坐标 522代表左上角的y坐标 340代表的是右下角的x坐标 567代表的是右下角的y坐标  |
| objects          | json_array | 物品信息            | N        | 返回图片中标识或物品的名称及位置信息，[详见objects参数](#imgDetailsBusinessDetailObjects)                                       |
| persons          | json_array | 图片中人物的名称及位置信息   | N        | 当命中'人像-多人'标签时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个），[详见persons参数](#imgDetailsBusinessDetailPersons) |


其中，imgDetails的businessLabels的businessDetail的faces数组每个元素的内容如下：


| **参数名称**    | **类型**    | **参数说明** | **是否必返** | **规范**                                                                                                  |
| ----------- | --------- | -------- | -------- | ------------------------------------------------------------------------------------------------------- |
| face_ratio  | float     | 人脸占比     | N        | 在区间0-1，数值越大，人脸占比越高                                                                                      |
| id          | string    | 编号       | N        | 图片同一个位置下的人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID                                                             |
| name        | string    | 人物名称     | N        | 人物名称                                                                                                    |
| probability | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                                                                                     |
| location    | int_array | 人物位置信息   | N        | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567] 207代表的是左上角的x坐标 522代表左上角的y坐标 340代表的是右下角的x坐标 567代表的是右下角的y坐标 |


其中，imgDetails的businessLabels的businessDetail的objects数组每个元素的内容如下：


| **参数名称**    | **类型**    | **参数说明**      | **是否必返** | **规范**                                                                                                  |
| ----------- | --------- | ------------- | -------- | ------------------------------------------------------------------------------------------------------- |
| id          | string    | 物品或标识编号       | N        | 保证同一个位置下的物品在不同标签下的编号相同                                                                                  |
| name        | string    | 物品名称          | N        | 物品名称                                                                                                    |
| probability | float     | 置信度           | N        | 可选值在0～1之间，值越大，可信度越高                                                                                     |
| qrContent   | string    | 返回图片中识别的二维码地址 | N        | 返回图片中识别的二维码地址                                                                                           |
| location    | int_array | 物品位置信息        | N        | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567] 207代表的是左上角的x坐标 522代表左上角的y坐标 340代表的是右下角的x坐标 567代表的是右下角的y坐标 |


其中，imgDetails的businessLabels的businessDetail的persons数组每个元素的内容如下：


| **参数名称**     | **类型**    | **参数说明** | **是否必返** | **规范**                                  |
| ------------ | --------- | -------- | -------- | --------------------------------------- |
| id           | string    | 编号       | N        | 保证同一个人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID |
| person_ratio | float     | 人像占比     | N        | 在区间0-1，数值越大，人脸占比越高                      |
| probability  | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                     |
| location     | int_array | 人像位置坐标   | N        | 人像位置坐标                                  |


其中，imgDetails的disposal的内容如下：


| **参数名称**        | **类型**      | **参数说明**  | **是否必返** | **规范**                                                                       |
| --------------- | ----------- | --------- | -------- | ---------------------------------------------------------------------------- |
| riskDescription | string      | 映射后风险原因   | Y        | 当riskLevel为PASS时为正常                                                          |
| riskLabel1      | string      | 映射后一级风险标签 | Y        | 一级风险标签，当数美标签未映射上自定义标签，且disposal下的riskLevel为PASS时，riskLabel1值为normal          |
| riskLabel2      | string      | 映射后二级风险标签 | Y        | 二级风险标签，当数美标签未映射上自定义标签，且disposal下的riskLevel为PASS时，riskLabel2值为空               |
| riskLabel3      | string      | 映射后三级风险标签 | Y        | 三级风险标签，当数美标签未映射上自定义标签，且disposal下的riskLevel为PASS时，riskLabel3值为空               |
| riskLevel       | string      | 处置建议      | Y        | 若贵司有自己的处置规则，数美可按照贵司的处置逻辑配置并返回对应的处置建议；若规则标签未映射上，则返回默认处置建议                     |
| riskDetail      | json_object | 映射后风险详情   | Y        | 格式与imgDetails的riskDetail结构相同，[详见riskDetail参数](#imgDetailsDisposalRiskDetail) |


其中，imgDetails的disposal的riskDetail的内容如下：


| **参数名称**   | **类型**      | **参数说明**        | **是否必返** | **规范**                                                                                             |
| ---------- | ----------- | --------------- | -------- | -------------------------------------------------------------------------------------------------- |
| riskSource | int         | 风险来源            | Y        | 可能取值： `1000`：无风险 `1001`：文字风险 `1002`：视觉图片风险                                                         |
| face_num   | int         | 人脸数量            | N        | 人脸数量                                                                                               |
| person_num | int         | 人像数量            | N        | 人像数量                                                                                               |
| faces      | json_array  | 图片中涉政人物的名称及位置信息 | N        | 当命中人脸-人脸类型-多人脸时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个），[详见faces参数](#imgDetailsDisposalFaces)   |
| objects    | json_array  | 物品信息            | N        | 返回图片中标识或物品的名称及位置信息，[详见objects参数](#imgDetailsDisposalObjects)                                       |
| ocrText    | json_object | 返回图片中文字识别内容     | N        | 当请求参数imgType字段包含IMGTEXTRISK或ADVERT时存在，[详见ocrText参数](#imgDetailsDisposalOcrText)                    |
| persons    | json_array  | 图片中人物的名称及位置信息   | N        | 当命中'人像-多人'标签时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个），[详见persons参数](#imgDetailsDisposalPersons) |


其中，imgDetails的disposal的riskDetail的faces数组每个元素的内容如下：


| **参数名称**    | **类型**    | **参数说明** | **是否必返** | **规范**                                                                                                  |
| ----------- | --------- | -------- | -------- | ------------------------------------------------------------------------------------------------------- |
| face_ratio  | float     | 人脸占比     | N        | 在区间0-1，数值越大，人脸占比越高                                                                                      |
| id          | string    | 编号       | N        | 图片同一个位置下的人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID                                                             |
| name        | string    | 人物名称     | N        | 人物名称                                                                                                    |
| probability | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                                                                                     |
| location    | int_array | 人物位置信息   | N        | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567] 207代表的是左上角的x坐标 522代表左上角的y坐标 340代表的是右下角的x坐标 567代表的是右下角的y坐标 |


其中，imgDetails的disposal的riskDetail的objects数组每个元素的内容如下：


| **参数名称**    | **类型**    | **参数说明**      | **是否必返** | **规范**                                                                                                  |
| ----------- | --------- | ------------- | -------- | ------------------------------------------------------------------------------------------------------- |
| id          | string    | 物品或标识编号       | N        | 保证同一个位置下的物品在不同标签下的编号相同                                                                                  |
| name        | string    | 物品名称          | N        | 物品名称                                                                                                    |
| probability | float     | 置信度           | N        | 可选值在0～1之间，值越大，可信度越高                                                                                     |
| qrContent   | string    | 返回图片中识别的二维码地址 | N        | 返回图片中识别的二维码地址                                                                                           |
| location    | int_array | 物品位置信息        | N        | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567] 207代表的是左上角的x坐标 522代表左上角的y坐标 340代表的是右下角的x坐标 567代表的是右下角的y坐标 |


其中，imgDetails的disposal的riskDetail的ocrText的内容如下：


| **参数名称**     | **类型**     | **参数说明**     | **是否必返** | **规范**                                                                              |
| ------------ | ---------- | ------------ | -------- | ----------------------------------------------------------------------------------- |
| text         | string     | 图片中识别出的文字    | Y        | 图片中识别出的文字                                                                           |
| matchedLists | json_array | 命中的客户自定义名单信息 | N        | 仅在命中客户自定义名单时返回，[详见matchedLists参数](#imgDetailsDisposalMatchedLists)                  |
| riskSegments | json_array | 高风险片段内容      | N        | 检测图片包含涉政、暴恐、违禁、竞品、广告法等风险内容的时候存在，[详见riskSegments参数](#imgDetailsDisposalRiskSegments) |


其中，imgDetails的disposal的riskDetail的ocrText的matchedLists数组每个元素的内容如下：


| **参数名称** | **类型**     | **参数说明**       | **是否必返** | **规范**                                |
| -------- | ---------- | -------------- | -------- | ------------------------------------- |
| name     | string     | 命中的名单名称        | N        | 命中的名单名称                               |
| words    | json_array | 命中的这个名单中的敏感词信息 | N        | [详见words参数](#imgDetailsDisposalWords) |


其中，imgDetails的disposal的riskDetail的ocrText的matchedLists的words数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明** | **是否必返** | **规范**  |
| -------- | --------- | -------- | -------- | ------- |
| word     | string    | 命中的敏感词   | N        | 命中的敏感词  |
| position | int_array | 敏感词所在位置  | N        | 敏感词所在位置 |


其中，imgDetails的disposal的riskDetail的ocrText的riskSegments数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明**    | **是否必返** | **规范**               |
| -------- | --------- | ----------- | -------- | -------------------- |
| segment  | string    | 高风险内容片段     | N        | 高风险内容片段              |
| position | int_array | 高风险内容片段所在位置 | N        | 高风险内容片段所在位置，下标从0开始计数 |


其中，imgDetails的disposal的riskDetail的persons数组每个元素的内容如下：


| **参数名称**     | **类型**    | **参数说明** | **是否必返** | **规范**                                  |
| ------------ | --------- | -------- | -------- | --------------------------------------- |
| id           | string    | 编号       | N        | 保证同一个人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID |
| person_ratio | float     | 人像占比     | N        | 在区间0-1，数值越大，人脸占比越高                      |
| probability  | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                     |
| location     | int_array | 人像位置坐标   | N        | 人像位置坐标                                  |


其中，imgDetails的tokenLabels的内容如下：


| **参数名称**             | **类型**      | **参数说明**  | **是否必返** | **规范**                                                                         |
| -------------------- | ----------- | --------- | -------- | ------------------------------------------------------------------------------ |
| UGC_account_risk     | json_object | UGC内容相关风险 | N        | UGC内容相关风险，[详见UGC_account_risk参数](#imgDetailsUGCAccountRisk)                    |
| machine_account_risk | json_object | 机器控制相关风险  | N        | 机器控制相关风险，[详见machine_account_risk参数](#imgDetailsMachineAccountRisk)             |
| scene_account_risk   | json_object | 场景账号风险    | N        | 场景账号风险，仅在特殊场景下可获得，如航空公司等，[详见scene_account_risk参数](#imgDetailsSceneAccountRisk) |


其中，imgDetails的tokenLabels的UGC_account_risk的内容如下：


| **参数名称**                         | **类型** | **参数说明** | **是否必返** | **规范**                           |
| -------------------------------- | ------ | -------- | -------- | -------------------------------- |
| b_advertise_risk_tokenid         | int    | 广告风险     | N        | 可选值： `0`：当前未检测到广告风险 `1`：存在广告风险   |
| b_advertise_risk_tokenid_last_ts | int    | 广告风险时间   | N        | 广告风险时间戳                          |
| b_politics_risk_tokenid          | int    | 涉政风险     | N        | 可选值： `0`：当前未识别到涉政风险 `1`：存在涉政风险   |
| b_politics_risk_tokenid_last_ts  | int    | 涉政风险时间   | N        | 涉政风险时间戳                          |
| b_sexy_risk_tokenid              | int    | 色情风险     | N        | 可选值： `0`：当前暂时未检测到色情风险 `1`：存在色情风险 |
| b_sexy_risk_tokenid_last_ts      | int    | 色情风险时间   | N        | 色情风险时间戳                          |


其中，imgDetails的tokenLabels的machine_account_risk的内容如下：


| **参数名称**                          | **类型** | **参数说明** | **是否必返** | **规范**                      |
| --------------------------------- | ------ | -------- | -------- | --------------------------- |
| b_machine_control_tokenid         | int    | 机器账号     | N        | 可选值： `0`：非机器控制账号 `1`：机器控制账号 |
| b_machine_control_tokenid_last_ts | int    | 机器账号时间   | N        | 机器账号时间戳                     |
| b_offer_wall_tokenid              | int    | 积分墙账号    | N        | 可选值： `0`：非积分墙账号 `1`：积分墙账号   |
| b_offer_wall_tokenid_last_ts      | int    | 积分墙账号时间  | N        | 积分墙账号时间戳                    |


其中，imgDetails的tokenLabels的scene_account_risk的内容如下：


| **参数名称**                    | **类型** | **参数说明** | **是否必返** | **规范**                          |
| --------------------------- | ------ | -------- | -------- | ------------------------------- |
| i_tout_risk_tokenid         | int    | 航空公司占座账号 | N        | 可选值： `0`：非航空公司占座账号 `1`：航空公司占座账号 |
| i_tout_risk_tokenid_last_ts | int    | 航空公司占座时间 | N        | 航空公司占座时间戳                       |


其中，audioDetails的内容如下：


| **参数名称**    | **类型**     | **参数说明**   | **是否必返** | **规范**                                                                                                                         |
| ----------- | ---------- | ---------- | -------- | ------------------------------------------------------------------------------------------------------------------------------ |
| requestId   | string     | 请求标识       | Y        | 本次请求的唯一标识，用于问题排查和效果优化，强烈建议保存                                                                                                   |
| code        | int        | 返回码        | Y        | `1100`：成功 `1101`：正在处理中 `1901`：QPS超限 `1902`：参数不合法 `1903`：服务失败 `1904`：下载失败 `1905`：解码失败 除message和requestId之外的字段，只有当code为1100时才会存在 |
| message     | string     | 返回码描述      | Y        | 和code对应：成功、正在处理中、QPS超限、参数不合法、服务失败、下载失败、解码失败、余额不足、无权限操作                                                                         |
| riskLevel   | string     | 处置建议       | Y        | 可能返回值： `PASS`：正常，建议直接放行 `REVIEW`：可疑，建议人工审核 `REJECT`：违规，建议直接拦截 建议：对接初期不直接使用结果，进行拦截尺度调优，符合预期后再进行使用                               |
| audioText   | string     | 整段音频转译文本结果 | Y        | 整段音频转译文本结果                                                                                                                     |
| audioTime   | int        | 整段音频的音频时长  | Y        | 单位秒                                                                                                                            |
| audioDetail | json_array | 音频片段信息     | Y        | 回调的音频片段信息，[详见audioDetail参数](#audioDetail)                                                                                      |


其中，audioDetail具体参数如下：


| **参数名称**        | **类型**      | **参数说明** | **是否必返** | **规范**                                                           |
| --------------- | ----------- | -------- | -------- | ---------------------------------------------------------------- |
| requestId       | string      | 请求标识     | Y        | 音频片段请求唯一标识                                                       |
| audioStarttime  | float       | 音频片段起始时间 | Y        | 相对音频开始的时间距离，单位是秒                                                 |
| audioEndtime    | float       | 音频片段结束时间 | Y        | 相对音频开始的时间距离，单位是秒                                                 |
| audioUrl        | string      | 音频片段链接   | Y        | mp3格式                                                            |
| riskLevel       | string      | 处置建议     | Y        | 可能返回值： `PASS`：正常，建议直接放行 `REVIEW`：可疑，建议人工审核 `REJECT`：违规，建议直接拦截    |
| businessLabels  | json_array  | 业务标签     | N        | 命中的所有业务标签以及详细信息，[详见businessLabels参数](#audioDetailBusinessLabels) |
| allLabels       | json_array  | 风险标签列表   | N        | 命中的所有风险标签以及详情信息，[详见allLabels参数](#audioDetailAllLabels)           |
| riskLabel1      | string      | 一级风险标签   | Y        | 一级风险标签，当riskLevel为`PASS`时返回`normal`                              |
| riskLabel2      | string      | 二级风险标签   | Y        | 二级风险标签，当riskLevel为`PASS`时为空                                      |
| riskLabel3      | string      | 三级风险标签   | Y        | 三级风险标签，当riskLevel为`PASS`时为空                                      |
| riskDescription | string      | 风险原因     | Y        | 当riskLevel为`PASS`时为"正常"                                          |
| riskDetail      | json_object | 风险详情     | N        | 风险详情，[详见riskDetail参数](#audioDetailRiskDetail)                    |


其中，audioDetail的businessLabels数组每个元素的内容如下：


| **参数名称**            | **类型**      | **参数说明** | **是否必返** | **规范**                   |
| ------------------- | ----------- | -------- | -------- | ------------------------ |
| businessLabel1      | string      | 一级业务标签   | Y        | 一级业务标签                   |
| businessLabel2      | string      | 二级业务标签   | Y        | 二级业务标签                   |
| businessLabel3      | string      | 三级业务标签   | Y        | 三级业务标签                   |
| businessDescription | string      | 业务标签描述   | Y        | 格式为"一级标签:二级标签:三级标签"的中文名称 |
| confidenceLevel     | int         | 置信等级     | N        | 可选值在0～2之间，值越大，可信度越高      |
| probability         | float       | 置信度      | Y        | 可选值为0~1，值越大，可信度越高        |
| businessDetail      | json_object | 业务标签详情   | N        | 保留字段                     |


其中，audioDetail的allLabels数组每个元素的内容如下：


| **参数名称**        | **类型**      | **参数说明** | **是否必返** | **规范**                                                                                              |
| --------------- | ----------- | -------- | -------- | --------------------------------------------------------------------------------------------------- |
| riskLevel       | string      | 风险等级     | Y        | 可能返回值： `REVIEW`：可疑 `REJECT`：违规                                                                      |
| riskLabel1      | string      | 一级风险标签   | Y        | allLabels不为空时必返                                                                                     |
| riskLabel2      | string      | 二级风险标签   | Y        | allLabels不为空时必返                                                                                     |
| riskLabel3      | string      | 三级风险标签   | Y        | allLabels不为空时必返                                                                                     |
| riskDescription | string      | 风险原因     | Y        | allLabels不为空时必返。当riskLevel为PASS时返回正常；其他情况展现形式为："一级标签:二级标签:三级标签"的中文名。仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |
| probability     | float       | 置信度      | N        | 可选值在0～1之间，值越大，风险可能性越高，值越小，无风险可能性越高。注意：allLabels不为空时必返                                               |
| riskDetail      | json_object | 映射后风险详情  | Y        | 格式与audioDetail的riskDetail结构相同。注意：allLabels不为空时必返，[详见riskDetail参数](#audioDetailAllLabelsRiskDetail)  |


其中，audioDetail的allLabels的riskDetail的内容如下：


| **参数名称**     | **类型**     | **参数说明**     | **是否必返** | **规范**                                                               |
| ------------ | ---------- | ------------ | -------- | -------------------------------------------------------------------- |
| riskSource   | int        | 风险来源         | Y        | 可能取值： `1000`：无风险 `1001`：文字风险 `1003`：音频语音风险                           |
| audioText    | string     | 音频转译文本的结果    | N        | 音频转译文本的结果                                                            |
| matchedLists | json_array | 命中的客户自定义名单信息 | N        | 仅在命中客户自定义名单时返回，[详见matchedLists参数](#audioDetailAllLabelsMatchedLists) |
| riskSegments | json_array | 高风险内容片段      | N        | [详见riskSegments参数](#audioDetailAllLabelsRiskSegments)                |


其中，audioDetail的allLabels的riskDetail的matchedLists数组每个元素的内容如下：


| **参数名称** | **类型**     | **参数说明**       | **是否必返** | **规范**                                  |
| -------- | ---------- | -------------- | -------- | --------------------------------------- |
| name     | string     | 客户自定义名单名称      | Y        | 客户自定义名单名称                               |
| words    | json_array | 命中的这个名单中的敏感词信息 | Y        | [详见words参数](#audioDetailAllLabelsWords) |


其中，audioDetail的allLabels的riskDetail的matchedLists的words数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明** | **是否必返** | **规范**  |
| -------- | --------- | -------- | -------- | ------- |
| word     | string    | 敏感词      | Y        | 敏感词     |
| position | int_array | 敏感词所在位置  | Y        | 敏感词所在位置 |


其中，audioDetail的allLabels的riskDetail的riskSegments数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明**    | **是否必返** | **规范**      |
| -------- | --------- | ----------- | -------- | ----------- |
| segment  | string    | 高风险内容片段     | N        | 高风险内容片段     |
| position | int_array | 高风险内容片段所在位置 | N        | 高风险内容片段所在位置 |


其中，audioDetail的riskDetail的内容如下：


| **参数名称**     | **类型**     | **参数说明**     | **是否必返** | **规范**                                                      |
| ------------ | ---------- | ------------ | -------- | ----------------------------------------------------------- |
| riskSource   | int        | 风险来源         | Y        | 可能取值： `1000`：无风险 `1001`：文字风险 `1003`：音频语音风险                  |
| audioText    | string     | 音频转译文本的结果    | N        | 音频转译文本的结果                                                   |
| matchedLists | json_array | 命中的客户自定义名单信息 | N        | 仅在命中客户自定义名单时返回，[详见matchedLists参数](#audioDetailMatchedLists) |
| riskSegments | json_array | 高风险内容片段      | N        | [详见riskSegments参数](#audioDetailRiskSegments)                |


其中，audioDetail的riskDetail的matchedLists数组每个元素的内容如下：


| **参数名称** | **类型**     | **参数说明**       | **是否必返** | **规范**                         |
| -------- | ---------- | -------------- | -------- | ------------------------------ |
| name     | string     | 客户自定义名单名称      | Y        | 客户自定义名单名称                      |
| words    | json_array | 命中的这个名单中的敏感词信息 | Y        | [详见words参数](#audioDetailWords) |


其中，audioDetail的riskDetail的matchedLists的words数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明** | **是否必返** | **规范**  |
| -------- | --------- | -------- | -------- | ------- |
| word     | string    | 敏感词      | Y        | 敏感词     |
| position | int_array | 敏感词所在位置  | Y        | 敏感词所在位置 |


其中，audioDetail的riskDetail的riskSegments数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明**    | **是否必返** | **规范**      |
| -------- | --------- | ----------- | -------- | ----------- |
| segment  | string    | 高风险内容片段     | N        | 高风险内容片段     |
| position | int_array | 高风险内容片段所在位置 | N        | 高风险内容片段所在位置 |


其中，videoDetails具体参数如下：


| **参数名称**           | **类型**      | **参数说明** | **是否必返** | **规范**                                                              |
| ------------------ | ----------- | -------- | -------- | ------------------------------------------------------------------- |
| code               | int         | 返回码      | Y        | `1100`：成功 `1901`：QPS超限 `1902`：参数不合法 `1903`：服务失败 `1905`：字数超限         |
| message            | string      | 返回码描述    | Y        | 和code对应：成功、QPS超限、参数不合法、服务失败、字数超限、无权限操作                              |
| requestId          | string      | 请求标识     | Y        | 本次请求数据的唯一标识，用于问题排查和效果优化，强烈建议保存                                      |
| riskLevel          | string      | 处置建议     | Y        | 可能返回值： `PASS`：正常，建议直接放行 `REVIEW`：可疑，建议人工审核 `REJECT`：违规，建议直接拦截       |
| frameDetail        | json_array  | 风险详情     | N        | 有风险片段或returnAllVideoImg=1时返回，[详见frameDetail说明](#frameDetail)        |
| audioDetail        | json_array  | 音频片段信息   | N        | 有风险片段或returnAllVideoAudio=1时返回，[详见audioDetail说明](#videoAudioDetail) |
| auxInfo            | json_object | 辅助信息     | N        | code为1100时存在，[详见auxInfo说明](#videoAuxInfo)                           |
| tokenProfileLabels | json_array  | 账号属性标签   | N        | 仅在开启功能时返回，[详见tokenProfileLabels说明](#tokenProfileLabels)             |
| tokenRiskLabels    | json_array  | 账号风险标签   | N        | 仅在开启功能时返回，[详见tokenProfileLabels说明](#tokenProfileLabels)             |


其中，frameDetail具体参数如下：


| **参数名称**        | **类型**      | **参数说明**    | **是否必返** | **规范**                                                                           |
| --------------- | ----------- | ----------- | -------- | -------------------------------------------------------------------------------- |
| requestId       | string      | 请求标识        | Y        | 本次请求数据的唯一标识，用于问题排查和效果优化，强烈建议保存                                                   |
| imgUrl          | string      | 截帧图片地址      | Y        | 截帧图片地址                                                                           |
| riskDescription | string      | 风险描述        | Y        | 当riskLevel为`PASS`时返回：正常。格式为："一级风险标签：二级风险标签：三级风险标签"，的中文名称。对于命中用户自定义名单时返回：命中自定义名单。 |
| riskLabel1      | string      | 一级风险标签      | Y        | 一级风险标签，当riskLevel为`PASS`时返回`normal`                                              |
| riskLabel2      | string      | 二级风险标签      | Y        | 二级风险标签，当riskLevel为`PASS`时为空                                                      |
| riskLabel3      | string      | 三级风险标签      | Y        | 三级风险标签，当riskLevel为`PASS`时为空                                                      |
| riskLevel       | string      | 处置建议        | Y        | 可能返回值： `PASS`：正常，建议直接放行 `REVIEW`：可疑，建议人工审核 `REJECT`：违规，建议直接拦截                    |
| allLabels       | json_array  | 风险标签列表      | Y        | 命中的所有风险标签以及详情信息，[详见allLabels参数](#frameAllLabels)                                 |
| auxInfo         | json_object | 辅助信息        | Y        | 辅助信息，[详见auxInfo参数](#frameAuxInfo)                                                |
| riskDetail      | json_object | 风险详情        | Y        | 风险详情，[详见riskDetail参数](#frameRiskDetail)                                          |
| imgText         | string      | 截帧图片OCR文本内容 | N        | 截帧图片OCR文字识别，仅传入ADVERT、IMGTEXTRISK时返回                                             |
| time            | float       | 截帧在视频文件中的时间 | N        | 截帧图片相对视频文件的时间，单位为秒                                                               |
| businessLabels  | json_array  | 业务标签        | N        | 命中的所有业务标签以及详细信息，[详见businessLabels参数](#frameBusinessLabels)                       |


其中，frameDetail的allLabels数组每个元素的内容如下：


| **参数名称**        | **类型**      | **参数说明** | **是否必返** | **规范**                                                                                              |
| --------------- | ----------- | -------- | -------- | --------------------------------------------------------------------------------------------------- |
| riskLevel       | string      | 风险等级     | Y        | 可能返回值： `REVIEW`：可疑 `REJECT`：违规                                                                      |
| riskLabel1      | string      | 一级风险标签   | Y        | allLabels不为空时必返                                                                                     |
| riskLabel2      | string      | 二级风险标签   | Y        | allLabels不为空时必返                                                                                     |
| riskLabel3      | string      | 三级风险标签   | Y        | allLabels不为空时必返                                                                                     |
| riskDescription | string      | 风险原因     | Y        | allLabels不为空时必返。当riskLevel为PASS时返回正常；其他情况展现形式为："一级标签:二级标签:三级标签"的中文名。仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |
| probability     | float       | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高。注意：allLabels不为空时必返                                                              |
| riskDetail      | json_object | 映射后风险详情  | Y        | 格式与frameDetail的riskDetail结构相同。注意：allLabels不为空时必返，[详见riskDetail参数](#frameAllLabelsRiskDetail)        |


其中，frameDetail的allLabels的riskDetail的内容如下：


| **参数名称**   | **类型**      | **参数说明**        | **是否必返** | **规范**                                                                                         |
| ---------- | ----------- | --------------- | -------- | ---------------------------------------------------------------------------------------------- |
| riskSource | int         | 风险来源            | Y        | 可能取值： `1000`：无风险 `1001`：文字风险 `1002`：视觉图片风险                                                     |
| face_num   | int         | 人脸数量            | N        | 人脸数量                                                                                           |
| person_num | int         | 人像数量            | N        | 人像数量                                                                                           |
| faces      | json_array  | 图片中涉政人物的名称及位置信息 | N        | 当命中人脸-人脸类型-多人脸时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个），[详见faces参数](#frameAllLabelsFaces)   |
| objects    | json_array  | 物品信息            | N        | 返回图片中标识或物品的名称及位置信息，[详见objects参数](#frameAllLabelsObjects)                                       |
| ocrText    | json_object | 返回图片中文字识别内容     | N        | 当请求参数imgType字段包含IMGTEXTRISK或ADVERT时存在，[详见ocrText参数](#frameAllLabelsOcrText)                    |
| persons    | json_array  | 图片中人物的名称及位置信息   | N        | 当命中'人像-多人'标签时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个），[详见persons参数](#frameAllLabelsPersons) |


其中，frameDetail的allLabels的riskDetail的faces数组每个元素的内容如下：


| **参数名称**    | **类型**    | **参数说明** | **是否必返** | **规范**                                                                                                  |
| ----------- | --------- | -------- | -------- | ------------------------------------------------------------------------------------------------------- |
| face_ratio  | float     | 人脸占比     | N        | 在区间0-1，数值越大，人脸占比越高                                                                                      |
| id          | string    | 人物编号     | N        | 图片同一个位置下的人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID                                                             |
| name        | string    | 人物名称     | N        | 能识别的公众人物名称                                                                                              |
| probability | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                                                                                     |
| location    | int_array | 人物位置信息   | N        | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567] 207代表的是左上角的x坐标 522代表左上角的y坐标 340代表的是右下角的x坐标 567代表的是右下角的y坐标 |


其中，frameDetail的allLabels的riskDetail的objects数组每个元素的内容如下：


| **参数名称**    | **类型**    | **参数说明** | **是否必返** | **规范**                                                                                                  |
| ----------- | --------- | -------- | -------- | ------------------------------------------------------------------------------------------------------- |
| id          | string    | 物品或标识编号  | N        | 保证同一个位置下的物品在不同标签下的编号相同                                                                                  |
| name        | string    | 物品名称     | N        | 物品名称                                                                                                    |
| probability | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                                                                                     |
| qrContent   | string    | 二维码地址    | N        | 仅当命中二维码相关标签时返回                                                                                          |
| location    | int_array | 物品位置信息   | N        | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567] 207代表的是左上角的x坐标 522代表左上角的y坐标 340代表的是右下角的x坐标 567代表的是右下角的y坐标 |


其中，frameDetail的allLabels的riskDetail的ocrText的内容如下：


| **参数名称**     | **类型**     | **参数说明**     | **是否必返** | **规范**                                                                          |
| ------------ | ---------- | ------------ | -------- | ------------------------------------------------------------------------------- |
| text         | string     | 图片中识别出的文字    | Y        | 图片中识别出的文字                                                                       |
| matchedLists | json_array | 命中的客户自定义名单信息 | N        | 仅在命中客户自定义名单时返回，[详见matchedLists参数](#frameAllLabelsMatchedLists)                  |
| riskSegments | json_array | 高风险片段内容      | N        | 检测图片包含涉政、暴恐、违禁、竞品、广告法等风险内容的时候存在，[详见riskSegments参数](#frameAllLabelsRiskSegments) |


其中，frameDetail的allLabels的riskDetail的ocrText的matchedLists数组每个元素的内容如下：


| **参数名称** | **类型**     | **参数说明**       | **是否必返** | **规范**                            |
| -------- | ---------- | -------------- | -------- | --------------------------------- |
| name     | string     | 命中的名单名称        | N        | 命中的名单名称                           |
| words    | json_array | 命中的这个名单中的敏感词信息 | N        | [详见words参数](#frameAllLabelsWords) |


其中，frameDetail的allLabels的riskDetail的ocrText的matchedLists的words数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明** | **是否必返** | **规范**  |
| -------- | --------- | -------- | -------- | ------- |
| word     | string    | 命中的敏感词   | N        | 命中的敏感词  |
| position | int_array | 敏感词所在位置  | N        | 敏感词所在位置 |


其中，frameDetail的allLabels的riskDetail的ocrText的riskSegments数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明**    | **是否必返** | **规范**               |
| -------- | --------- | ----------- | -------- | -------------------- |
| segment  | string    | 高风险内容片段     | N        | 高风险内容片段              |
| position | int_array | 高风险内容片段所在位置 | N        | 高风险内容片段所在位置，下标从0开始计数 |


其中，frameDetail的allLabels的riskDetail的persons数组每个元素的内容如下：


| **参数名称**     | **类型**    | **参数说明** | **是否必返** | **规范**                                  |
| ------------ | --------- | -------- | -------- | --------------------------------------- |
| id           | string    | 人物编号     | N        | 保证同一个人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID |
| person_ratio | float     | 人像占比     | N        | 在区间0-1，数值越大，人脸占比越高                      |
| probability  | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                     |
| location     | int_array | 人像位置坐标   | N        | 人像位置坐标                                  |


其中，frameDetail的auxInfo的内容如下：


| **参数名称**   | **类型** | **参数说明** | **是否必返** | **规范**                                                                     |
| ---------- | ------ | -------- | -------- | -------------------------------------------------------------------------- |
| similarity | float  | 相似度      | Y        | 返回当前截帧图片和上一帧截帧图片的相似度。视频文件初始第一帧将比对纯黑背景图片。与上一张截帧图片的相似概率值，取值范围[0-1]，数值越接近1越相似 |
| qrContent  | string | 二维码地址    | N        | 返回图片中识别的二维码地址                                                              |


其中，frameDetail的riskDetail的内容如下：


| **参数名称**   | **类型**      | **参数说明**        | **是否必返** | **规范**                                                                                          |
| ---------- | ----------- | --------------- | -------- | ----------------------------------------------------------------------------------------------- |
| riskSource | int         | 风险来源            | Y        | 可能取值： `1000`：无风险 `1001`：文字风险 `1002`：视觉图片风险                                                      |
| face_num   | int         | 人脸数量            | N        | 人脸数量                                                                                            |
| person_num | int         | 人像数量            | N        | 人像数量                                                                                            |
| faces      | json_array  | 图片中涉政人物的名称及位置信息 | N        | 当命中人脸-人脸类型-多人脸时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个），[详见faces参数](#frameRiskDetailFaces)   |
| objects    | json_array  | 物品信息            | N        | 返回图片中标识或物品的名称及位置信息，[详见objects参数](#frameRiskDetailObjects)                                       |
| ocrText    | json_object | 返回图片中文字识别内容     | N        | 当请求参数imgType字段包含IMGTEXTRISK或ADVERT时存在，[详见ocrText参数](#frameRiskDetailOcrText)                    |
| persons    | json_array  | 图片中人物的名称及位置信息   | N        | 当命中'人像-多人'标签时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个），[详见persons参数](#frameRiskDetailPersons) |


其中，frameDetail的riskDetail的faces数组每个元素的内容如下：


| **参数名称**    | **类型**    | **参数说明** | **是否必返** | **规范**                                                                                                  |
| ----------- | --------- | -------- | -------- | ------------------------------------------------------------------------------------------------------- |
| face_ratio  | float     | 人脸占比     | N        | 在区间0-1，数值越大，人脸占比越高                                                                                      |
| id          | string    | 人物编号     | N        | 图片同一个位置下的人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID                                                             |
| name        | string    | 人物名称     | N        | 能识别的公众人物名称                                                                                              |
| probability | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                                                                                     |
| location    | int_array | 人物位置信息   | N        | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567] 207代表的是左上角的x坐标 522代表左上角的y坐标 340代表的是右下角的x坐标 567代表的是右下角的y坐标 |


其中，frameDetail的riskDetail的objects数组每个元素的内容如下：


| **参数名称**    | **类型**    | **参数说明** | **是否必返** | **规范**                                                                                                  |
| ----------- | --------- | -------- | -------- | ------------------------------------------------------------------------------------------------------- |
| id          | string    | 物品或标识编号  | N        | 保证同一个位置下的物品在不同标签下的编号相同                                                                                  |
| name        | string    | 物品名称     | N        | 物品名称                                                                                                    |
| probability | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                                                                                     |
| qrContent   | string    | 二维码地址    | N        | 仅当命中二维码相关标签时返回                                                                                          |
| location    | int_array | 物品位置信息   | N        | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567] 207代表的是左上角的x坐标 522代表左上角的y坐标 340代表的是右下角的x坐标 567代表的是右下角的y坐标 |


其中，frameDetail的riskDetail的ocrText的内容如下：


| **参数名称**     | **类型**     | **参数说明**     | **是否必返** | **规范**                                                                           |
| ------------ | ---------- | ------------ | -------- | -------------------------------------------------------------------------------- |
| text         | string     | 图片中识别出的文字    | Y        | 图片中识别出的文字                                                                        |
| matchedLists | json_array | 命中的客户自定义名单信息 | N        | 仅在命中客户自定义名单时返回，[详见matchedLists参数](#frameRiskDetailMatchedLists)                  |
| riskSegments | json_array | 高风险片段内容      | N        | 检测图片包含涉政、暴恐、违禁、竞品、广告法等风险内容的时候存在，[详见riskSegments参数](#frameRiskDetailRiskSegments) |


其中，frameDetail的riskDetail的ocrText的matchedLists数组每个元素的内容如下：


| **参数名称** | **类型**     | **参数说明**       | **是否必返** | **规范**                             |
| -------- | ---------- | -------------- | -------- | ---------------------------------- |
| name     | string     | 命中的名单名称        | N        | 命中的名单名称                            |
| words    | json_array | 命中的这个名单中的敏感词信息 | N        | [详见words参数](#frameRiskDetailWords) |


其中，frameDetail的riskDetail的ocrText的matchedLists的words数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明** | **是否必返** | **规范**  |
| -------- | --------- | -------- | -------- | ------- |
| word     | string    | 命中的敏感词   | N        | 命中的敏感词  |
| position | int_array | 敏感词所在位置  | N        | 敏感词所在位置 |


其中，frameDetail的riskDetail的ocrText的riskSegments数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明**    | **是否必返** | **规范**               |
| -------- | --------- | ----------- | -------- | -------------------- |
| segment  | string    | 高风险内容片段     | N        | 高风险内容片段              |
| position | int_array | 高风险内容片段所在位置 | N        | 高风险内容片段所在位置，下标从0开始计数 |


其中，frameDetail的riskDetail的persons数组每个元素的内容如下：


| **参数名称**     | **类型**    | **参数说明** | **是否必返** | **规范**                                  |
| ------------ | --------- | -------- | -------- | --------------------------------------- |
| id           | string    | 人物编号     | N        | 保证同一个人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID |
| person_ratio | float     | 人像占比     | N        | 在区间0-1，数值越大，人脸占比越高                      |
| probability  | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                     |
| location     | int_array | 人像位置坐标   | N        | 人像位置坐标                                  |


其中，frameDetail的businessLabels数组每个元素的内容如下：


| **参数名称**            | **类型**      | **参数说明** | **是否必返** | **规范**                                     |
| ------------------- | ----------- | -------- | -------- | ------------------------------------------ |
| businessDescription | string      | 业务标签描述   | Y        | 格式为"一级标签:二级标签:三级标签"的中文名称                   |
| businessLabel1      | string      | 一级业务标签   | Y        | 一级业务标签                                     |
| businessLabel2      | string      | 二级业务标签   | Y        | 二级业务标签                                     |
| businessLabel3      | string      | 三级业务标签   | Y        | 三级业务标签                                     |
| probability         | float       | 置信度      | Y        | 可选值在0～1之间，值越大，可信度越高                        |
| confidenceLevel     | int         | 置信等级     | N        | 可选值在0～2之间，值越大，可信度越高                        |
| businessDetail      | json_object | 业务标签详情   | N        | [详见businessDetail参数](#frameBusinessDetail) |


其中，frameDetail的businessLabels的businessDetail的内容如下：


| **参数名称**   | **类型**     | **参数说明**        | **是否必返** | **规范**                                                                                              |
| ---------- | ---------- | --------------- | -------- | --------------------------------------------------------------------------------------------------- |
| face_num   | int        | 人脸数量            | N        | 人脸数量                                                                                                |
| person_num | int        | 人像数量            | N        | 人像数量                                                                                                |
| faces      | json_array | 图片中涉政人物的名称及位置信息 | N        | 当命中人脸-人脸类型-多人脸时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个），[详见faces参数](#frameBusinessDetailFaces)   |
| objects    | json_array | 物品信息            | N        | 返回图片中标识或物品的名称及位置信息，[详见objects参数](#frameBusinessDetailObjects)                                       |
| persons    | json_array | 图片中人物的名称及位置信息   | N        | 当命中'人像-多人'标签时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个），[详见persons参数](#frameBusinessDetailPersons) |


其中，frameDetail的businessLabels的businessDetail的faces数组每个元素的内容如下：


| **参数名称**    | **类型**    | **参数说明** | **是否必返** | **规范**                                                                                                  |
| ----------- | --------- | -------- | -------- | ------------------------------------------------------------------------------------------------------- |
| face_ratio  | float     | 人脸占比     | N        | 在区间0-1，数值越大，人脸占比越高                                                                                      |
| id          | string    | 人物编号     | N        | 图片同一个位置下的人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID                                                             |
| name        | string    | 人物名称     | N        | 能识别的公众人物名称                                                                                              |
| probability | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                                                                                     |
| location    | int_array | 人物位置信息   | N        | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567] 207代表的是左上角的x坐标 522代表左上角的y坐标 340代表的是右下角的x坐标 567代表的是右下角的y坐标 |


其中，frameDetail的businessLabels的businessDetail的objects数组每个元素的内容如下：


| **参数名称**    | **类型**    | **参数说明** | **是否必返** | **规范**                                                                                                  |
| ----------- | --------- | -------- | -------- | ------------------------------------------------------------------------------------------------------- |
| id          | string    | 物品或标识编号  | N        | 保证同一个位置下的物品在不同标签下的编号相同                                                                                  |
| name        | string    | 物品名称     | N        | 物品名称                                                                                                    |
| probability | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                                                                                     |
| qrContent   | string    | 二维码地址    | N        | 仅当命中二维码相关标签时返回                                                                                          |
| location    | int_array | 物品位置信息   | N        | 该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567] 207代表的是左上角的x坐标 522代表左上角的y坐标 340代表的是右下角的x坐标 567代表的是右下角的y坐标 |


其中，frameDetail的businessLabels的businessDetail的persons数组每个元素的内容如下：


| **参数名称**     | **类型**    | **参数说明** | **是否必返** | **规范**                                  |
| ------------ | --------- | -------- | -------- | --------------------------------------- |
| id           | string    | 人物编号     | N        | 保证同一个人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID |
| person_ratio | float     | 人像占比     | N        | 在区间0-1，数值越大，人脸占比越高                      |
| probability  | float     | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高                     |
| location     | int_array | 人像位置坐标   | N        | 人像位置坐标                                  |


其中，videoDetails的audioDetail具体参数如下：


| **参数名称**        | **类型**      | **参数说明**   | **是否必返** | **规范**                                                                           |
| --------------- | ----------- | ---------- | -------- | -------------------------------------------------------------------------------- |
| requestId       | string      | 请求标识       | Y        | 本次请求数据的唯一标识，用于问题排查和效果优化，强烈建议保存                                                   |
| audioUrl        | string      | 音频片段地址     | Y        | 音频片段地址                                                                           |
| riskDescription | string      | 风险描述       | Y        | 当riskLevel为`PASS`时返回：正常。格式为："一级风险标签：二级风险标签：三级风险标签"，的中文名称。对于命中用户自定义名单时返回：命中自定义名单。 |
| riskLabel1      | string      | 一级风险标签     | Y        | 一级风险标签，当riskLevel为`PASS`时返回`normal`                                              |
| riskLabel2      | string      | 二级风险标签     | Y        | 二级风险标签，当riskLevel为`PASS`时为空                                                      |
| riskLabel3      | string      | 三级风险标签     | Y        | 三级风险标签，当riskLevel为`PASS`时为空                                                      |
| riskLevel       | string      | 处置建议       | Y        | 可能返回值： `PASS`：正常，建议直接放行 `REVIEW`：可疑，建议人工审核 `REJECT`：违规，建议直接拦截                    |
| allLabels       | json_array  | 风险标签列表     | Y        | 命中的所有风险标签以及详情信息，[详见allLabels参数](#videoAudioAllLabels)                            |
| audioEndtime    | float       | 音频片段结束时间   | N        | 相对音频开始时间的距离，单位是秒                                                                 |
| audioStarttime  | float       | 音频片段开始时间   | N        | 相对音频开始时间的距离，单位是秒                                                                 |
| audioText       | string      | 该片段识别的文字内容 | N        | 该片段识别的文字内容                                                                       |
| businessLabels  | json_array  | 业务标签       | N        | 命中的所有业务标签以及详细信息，[详见businessLabels参数](#videoAudioBusinessLabels)                  |
| riskDetail      | json_object | 风险详情       | N        | 风险详情，[详见riskDetail参数](#videoAudioRiskDetail)                                     |


其中，videoDetails的audioDetail的allLabels数组每个元素的内容如下：


| **参数名称**        | **类型**      | **参数说明** | **是否必返** | **规范**                                                                                                         |
| --------------- | ----------- | -------- | -------- | -------------------------------------------------------------------------------------------------------------- |
| riskLevel       | string      | 风险等级     | Y        | 可能返回值： `REVIEW`：可疑 `REJECT`：违规                                                                                 |
| riskLabel1      | string      | 一级风险标签   | Y        | allLabels不为空时必返                                                                                                |
| riskLabel2      | string      | 二级风险标签   | Y        | allLabels不为空时必返                                                                                                |
| riskLabel3      | string      | 三级风险标签   | Y        | allLabels不为空时必返                                                                                                |
| riskDescription | string      | 风险原因     | Y        | allLabels不为空时必返。当riskLevel为PASS时返回正常；其他情况展现形式为："一级标签:二级标签:三级标签"的中文名。仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理            |
| probability     | float       | 置信度      | N        | 可选值在0～1之间，值越大，可信度越高。注意：allLabels不为空时必返                                                                         |
| riskDetail      | json_object | 映射后风险详情  | Y        | 格式与videoDetails的audioDetail的riskDetail结构相同。注意：allLabels不为空时必返，[详见riskDetail参数](#videoAudioAllLabelsRiskDetail) |


其中，videoDetails的audioDetail的allLabels的riskDetail的内容如下：


| **参数名称**     | **类型**     | **参数说明**     | **是否必返** | **规范**                                                                               |
| ------------ | ---------- | ------------ | -------- | ------------------------------------------------------------------------------------ |
| riskSource   | int        | 风险来源         | Y        | 可能取值： `1000`：无风险 `1001`：文字风险 `1003`：音频语音风险                                           |
| audioText    | string     | 该片段识别的文字内容   | N        | 该片段识别的文字内容                                                                           |
| matchedLists | json_array | 命中的客户自定义名单信息 | N        | 仅在命中客户自定义名单时返回，[详见matchedLists参数](#videoAudioAllLabelsMatchedLists)                  |
| riskSegments | json_array | 高风险片段内容      | N        | 检测图片包含涉政、暴恐、违禁、竞品、广告法等风险内容的时候存在，[详见riskSegments参数](#videoAudioAllLabelsRiskSegments) |


其中，videoDetails的audioDetail的allLabels的riskDetail的matchedLists数组每个元素的内容如下：


| **参数名称** | **类型**     | **参数说明**       | **是否必返** | **规范**                                 |
| -------- | ---------- | -------------- | -------- | -------------------------------------- |
| name     | string     | 名单名称           | N        | 名单名称                                   |
| words    | json_array | 命中的这个名单中的敏感词信息 | N        | [详见words参数](#videoAudioAllLabelsWords) |


其中，videoDetails的audioDetail的allLabels的riskDetail的matchedLists的words数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明** | **是否必返** | **规范**           |
| -------- | --------- | -------- | -------- | ---------------- |
| word     | string    | 敏感词      | N        | 命中的敏感词           |
| position | int_array | 敏感词所在位置  | N        | 敏感词所在位置，下标从0开始计数 |


其中，videoDetails的audioDetail的allLabels的riskDetail的riskSegments数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明**    | **是否必返** | **规范**               |
| -------- | --------- | ----------- | -------- | -------------------- |
| segment  | string    | 高风险内容片段     | N        | 高风险内容片段              |
| position | int_array | 高风险内容片段所在位置 | N        | 高风险内容片段所在位置，下标从0开始计数 |


其中，videoDetails的audioDetail的businessLabels数组每个元素的内容如下：


| **参数名称**            | **类型**      | **参数说明** | **是否必返** | **规范**                                          |
| ------------------- | ----------- | -------- | -------- | ----------------------------------------------- |
| businessDescription | string      | 业务标签描述   | Y        | 格式为"一级标签:二级标签:三级标签"的中文名称                        |
| businessLabel1      | string      | 一级业务标签   | Y        | 一级业务标签                                          |
| businessLabel2      | string      | 二级业务标签   | Y        | 二级业务标签                                          |
| businessLabel3      | string      | 三级业务标签   | Y        | 三级业务标签                                          |
| probability         | float       | 置信度      | Y        | 可选值在0～1之间，值越大，可信度越高                             |
| confidenceLevel     | int         | 置信等级     | N        | 可选值在0～2之间，值越大，可信度越高                             |
| businessDetail      | json_object | 业务标签详情   | N        | [详见businessDetail参数](#videoAudioBusinessDetail) |


其中，videoDetails的audioDetail的businessLabels的businessDetail的内容如下：


| **参数名称**     | **类型**     | **参数说明**     | **是否必返** | **规范**                                                                     |
| ------------ | ---------- | ------------ | -------- | -------------------------------------------------------------------------- |
| riskSource   | int        | 风险来源         | Y        | 可能取值： `1000`：无风险 `1001`：文字风险 `1003`：音频语音风险                                 |
| audioText    | string     | 该片段识别的文字内容   | N        | 该片段识别的文字内容                                                                 |
| matchedLists | json_array | 命中的客户自定义名单信息 | N        | 仅在命中客户自定义名单时返回，[详见matchedLists参数](#videoAudioBusinessMatchedLists)         |
| riskSegments | json_array | 高风险片段内容      | N        | 检测音频包含未成年，唱歌等风险内容的时候存在，[详见riskSegments参数](#videoAudioBusinessRiskSegments) |


其中，videoDetails的audioDetail的businessLabels的businessDetail的matchedLists数组每个元素的内容如下：


| **参数名称** | **类型**     | **参数说明**       | **是否必返** | **规范**                                |
| -------- | ---------- | -------------- | -------- | ------------------------------------- |
| name     | string     | 名单名称           | N        | 名单名称                                  |
| words    | json_array | 命中的这个名单中的敏感词信息 | N        | [详见words参数](#videoAudioBusinessWords) |


其中，videoDetails的audioDetail的businessLabels的businessDetail的matchedLists的words数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明** | **是否必返** | **规范**           |
| -------- | --------- | -------- | -------- | ---------------- |
| word     | string    | 敏感词      | N        | 命中的敏感词           |
| position | int_array | 敏感词所在位置  | N        | 敏感词所在位置，下标从0开始计数 |


其中，videoDetails的audioDetail的businessLabels的businessDetail的riskSegments数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明**    | **是否必返** | **规范**               |
| -------- | --------- | ----------- | -------- | -------------------- |
| segment  | string    | 高风险内容片段     | N        | 高风险内容片段              |
| position | int_array | 高风险内容片段所在位置 | N        | 高风险内容片段所在位置，下标从0开始计数 |


其中，videoDetails的audioDetail的riskDetail的内容如下：


| **参数名称**     | **类型**     | **参数说明**     | **是否必返** | **规范**                                                                                |
| ------------ | ---------- | ------------ | -------- | ------------------------------------------------------------------------------------- |
| riskSource   | int        | 风险来源         | Y        | 可能取值： `1000`：无风险 `1001`：文字风险 `1003`：音频语音风险                                            |
| audioText    | string     | 该片段识别的文字内容   | N        | 该片段识别的文字内容                                                                            |
| matchedLists | json_array | 命中的客户自定义名单信息 | N        | 仅在命中客户自定义名单时返回，[详见matchedLists参数](#videoAudioRiskDetailMatchedLists)                  |
| riskSegments | json_array | 高风险片段内容      | N        | 检测图片包含涉政、暴恐、违禁、竞品、广告法等风险内容的时候存在，[详见riskSegments参数](#videoAudioRiskDetailRiskSegments) |


其中，videoDetails的audioDetail的riskDetail的matchedLists数组每个元素的内容如下：


| **参数名称** | **类型**     | **参数说明**       | **是否必返** | **规范**                                  |
| -------- | ---------- | -------------- | -------- | --------------------------------------- |
| name     | string     | 名单名称           | N        | 名单名称                                    |
| words    | json_array | 命中的这个名单中的敏感词信息 | N        | [详见words参数](#videoAudioRiskDetailWords) |


其中，videoDetails的audioDetail的riskDetail的matchedLists的words数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明** | **是否必返** | **规范**           |
| -------- | --------- | -------- | -------- | ---------------- |
| word     | string    | 命中的敏感词   | N        | 命中的敏感词           |
| position | int_array | 敏感词所在位置  | N        | 敏感词所在位置，下标从0开始计数 |


其中，videoDetails的audioDetail的riskDetail的riskSegments数组每个元素的内容如下：


| **参数名称** | **类型**    | **参数说明**    | **是否必返** | **规范**               |
| -------- | --------- | ----------- | -------- | -------------------- |
| segment  | string    | 高风险内容片段     | N        | 高风险内容片段              |
| position | int_array | 高风险内容片段所在位置 | N        | 高风险内容片段所在位置，下标从0开始计数 |


其中，videoDetails的auxInfo具体参数如下：


| **参数名称**      | **类型** | **参数说明**       | **是否必返** | **规范**                                                                       |
| ------------- | ------ | -------------- | -------- | ---------------------------------------------------------------------------- |
| audioDuration | float  | 当前请求中的视频里的音频时长 | Y        | 单位是秒，与计费数目一致。审核视频时，如果视频文件中音轨数据和视频时长不一致，计费时长以实际的音轨时长为准；例如可能会存在没有音轨的情况，计费时长就为0 |
| frameCount    | int    | 返回的视频截帧数量      | Y        | returnAllImg=0时为风险数量，returnAllImg=1时为全部数量                                    |
| videoDuration | float  | 视频时长           | Y        | 单位是秒                                                                         |


其中，auxInfo的内容如下：


| **参数名称**    | **类型**      | **参数说明**  | **是否必返** | **规范**                                 |
| ----------- | ----------- | --------- | -------- | -------------------------------------- |
| textNum     | int         | 当前请求中的字符数 | Y        | 当前请求中的字符数，与计费数目一致。字符数包括汉字、英文、标点符号、空格等  |
| imgNum      | int         | 当前请求中的图片数 | Y        | 当前请求中的图片数，与计费数目一致。如遇动图会截取3帧；如遇长图会进行切分  |
| audioNum    | int         | 当前请求中的音频数 | Y        | 当前请求中的音频数，与计费数目一致                      |
| videoNum    | int         | 当前请求中的视频数 | Y        | 当前请求中的视频数，与计费数目一致                      |
| passThrough | json_object | 透传字段      | N        | 客户传入的透传字段，数美内部不会对该字段进行识别处理，会随结果原样返回给用户 |


### 示例

#### 回调模式示例

##### 请求示例

```json
{
    "accessKey": "your_access_key",
    "appId": "your_app_id",
    "eventId": "webpage",
    "imgType": "POLITY_EROTIC_ADVERT",
    "txtType": "TEXTRISK",
    "audioType": "POLITY_EROTIC_ADVERT",
    "videoImgType": "POLITY_EROTIC_ADVERT",
    "videoAudioType": "POLITY_EROTIC_ADVERT",
    "callback": "http://www.xxx.top/callbackaddr",
    "articleDoubleJumpConfig": {
        "isOpen": true
    },
    "articleScreenShotConfig": {
        "isOpen": true,
        "width": 1080,
        "height": 6480,
        "timeoutSecond": 30,
        "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36"
    },
    "articleDynamicConfig": {
        "isOpen": true,
        "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36",
        "cookie": ""
    },
    "data": {
        "url": "https://www.example.com/page",
        "tokenId": "user_123456",
        "lang": "zh",
        "acceptLang": "zh",
        "returnAllImg": 0,
        "returnAllText": 0,
        "returnAllVideoImg": 0,
        "returnAllVideoAudio": 0,
        "returnAllVideo": 0,
        "returnAllAudio": 0,
        "level": 2,
        "gender": "male",
        "nickname": "test_user",
        "ip": "123.171.34.3",
        "deviceId": "device_fingerprint_123",
        "dataId": "data_unique_id_001",
        "extra": {
            "room": "room_001",
            "passThrough": {
                "customField": "customValue"
            }
        }
    }
}
```

##### 同步返回示例

```json
{
    "code":1100,
    "message":"success",
    "requestId":"xxx"
}
```

##### 回调返回示例

```json
{
    "code": 1100,
    "message": "success",
    "requestId": "xxx",
    "riskLevel": "REVIEW",
    "textDetails": [
        {
            "allLabels": [
                {
                    "probability": 1,
                    "riskDescription": "涉政:涉政:涉政",
                    "riskDetail": {
                        "matchedLists": [
                            {
                                "name": "测试zyk",
                                "words": [
                                    {
                                        "position": [
                                            10,
                                            11
                                        ],
                                        "word": "58"
                                    }
                                ]
                            }
                        ]
                    },
                    "riskLabel1": "politics",
                    "riskLabel2": "politics",
                    "riskLabel3": "politics",
                    "riskLevel": "REVIEW"
                },
                {
                    "probability": 1,
                    "riskDescription": "涉政:涉政:涉政",
                    "riskDetail": {
                        "matchedLists": [
                            {
                                "name": "测试zyk",
                                "words": [
                                    {
                                        "position": [
                                            10,
                                            11,
                                            12
                                        ],
                                        "word": "585"
                                    }
                                ]
                            }
                        ]
                    },
                    "riskLabel1": "politics",
                    "riskLabel2": "shezheng",
                    "riskLabel3": "shezheng",
                    "riskLevel": "REVIEW"
                }
            ],
            "requestId": "rerlgiewregliweruogrerwd_1",
            "riskDescription": "涉政:涉政:涉政",
            "riskDetail": {
                "matchedLists": [
                    {
                        "name": "测试zyk",
                        "words": [
                            {
                                "position": [
                                    10,
                                    11,
                                    12
                                ],
                                "word": "585"
                            }
                        ]
                    }
                ]
            },
            "riskLabel1": "politics",
            "riskLabel2": "shezheng",
            "riskLabel3": "shezheng"
        }
    ],
    "imgDetails": [

    ],
    "auxInfo": {
        "imgCount": 4,
        "textCount": 317
    }
}
```

---

## 结果查询接口

用于异步队列、`callback` 回调或音视频分段回调等场景下，按检测接口返回的 `requestId` 主动查询终态审核结果（业务字段与上文[回调返回结果](#回调返回结果)一致）。

### 请求参数

#### 请求URL：


| 集群   | URL                                                          |
| ---- | ------------------------------------------------------------ |
| 北京   | `http://api-article-bj.fengkongcloud.com/query_webpage/v4`   |
| 弗吉尼亚 | `http://api-article-fjny.fengkongcloud.com/query_webpage/v4` |
| 新加坡  | `http://api-article-xjp.fengkongcloud.com/query_webpage/v4`  |


#### 字符编码格式：

`UTF-8`字符集编码

#### 请求方法：

`POST`

#### 建议超时时长：

15s

#### 请求参数：

放在HTTP Body中，采用Json格式，Body大小不可超过1M，具体参数如下：


| **请求参数名**  | **类型**       | **参数说明**     | **是否必传** | **规范**                                                  |
| ---------- | ------------ | ------------ | -------- | ------------------------------------------------------- |
| accessKey  | string       | 接口认证密钥       | Y        | 与检测接口一致，由数美提供                                           |
| requestIds | string_array | 待查询的检测请求标识列表 | Y        | 传入网页检测接口同步返回体中的 `requestId`；单次最多20条，单条长度不超过128字符；重复项会去重 |


#### 同步返回结果

放在HTTP Body中，采用Json格式，具体参数如下：


| **参数名称**  | **类型**     | **参数说明** | **是否必返** | **规范**                                                                    |
| --------- | ---------- | -------- | -------- | ------------------------------------------------------------------------- |
| code      | int        | 返回码      | Y        | `1100`：成功；`1902`：整单请求不合法。`1101`、`1903` 不出现在本字段，仅见于下文 `machineResult.code` |
| message   | string     | 返回码描述    | Y        | 与 code 对应                                                                 |
| requestId | string     | 请求标识     | Y        | 本次查询请求的标识                                                                 |
| contents  | json_array | 查询结果列表   | Y        | 与请求 `requestIds` 顺序一致，元素结构见下表                                             |


其中，**contents** 数组每个元素的内容如下：


| **参数名称**      | **类型**      | **参数说明** | **是否必返** | **规范**                                                                                                                                                                                  |
| ------------- | ----------- | -------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| requestId     | string      | 检测请求标识   | Y        | 对应某项待查 `requestId`                                                                                                                                                                      |
| machineResult | json_object | 机审结果     | Y        | 单项 `code`（与顶层 `code` 独立）：`1100` 终态成功，业务字段同上文[回调返回结果](#回调返回结果)；`1101` 无历史记录或仍在处理（message 为「请求正在处理」）；`1902` 历史表 `data` 无法解析为合法 JSON；`1903` 历史结果因数据过长写入降级记录（message 为「服务失败」，含 `detail` 等）。 |
| humanResult   | json_object | 人审结果     | N        | 有人审时返回                                                                                                                                                                                  |
| mergeResult   | json_object | 合并结果     | N        | 优先展示策略合并结果，例如 `riskLevel`                                                                                                                                                               |


### 示例

#### 请求示例

```json
{
    "accessKey": "your_access_key",
    "requestIds": ["xxx_detection_request_id"]
}
```

#### 返回示例

##### 处理中（machineResult.code 为 1101）

```json
{
    "code": 1100,
    "message": "正常",
    "requestId": "query_req_001",
    "contents": [
        {
            "requestId": "xxx_detection_request_id",
            "machineResult": {
                "code": 1101,
                "message": "请求正在处理",
                "requestId": "xxx_detection_request_id"
            },
            "mergeResult": {
                "riskLevel": "xxx"
            }
        }
    ]
}
```

##### 终态成功（machineResult.code 为 1100）

```json
{
    "code": 1100,
    "message": "正常",
    "requestId": "query_req_001",
    "contents": [
        {
            "requestId": "xxx_detection_request_id",
            "machineResult": {
                "code": 1100,
                "message": "成功",
                "requestId": "xxx_detection_request_id",
                "riskLevel": "xxx",
                "textDetails": [],
                "imgDetails": [],
                "audioDetails": [],
                "videoDetails": [],
                "auxInfo": {},
                "resultType": 0,
                "finalResult": 1
            },
            "mergeResult": {
                "riskLevel": "xxx"
            }
        }
    ]
}
```

