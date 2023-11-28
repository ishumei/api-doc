数美智能语音对话识别API接口文档 ![](RackMultipart20230807-1-hn1vxm_html_1aad4b0c45090f7d.png)

**# 数美智能语音对话识别API接口文档**

**北京数美时代科技有限公司提供**

**（版权所有，翻版必究）**

目录

[数美智能语音对话识别](#_Toc12274551)API接口文档 1

**[1.](#_Toc12274552)****接入前准备 **** 3**

[1.1](#_Toc12274553)数美服务账号申请 3

[1.2](#_Toc12274554)渠道配置表 3

[1.3](#_Toc12274555)数美服务账号信息接收 3

**[2.](#_Toc12274556)****智能音频过滤服务接口说明 **** 4**

[2.1](#_Toc12274557)上传语音消息文件 4

[2.2](#_Toc12274558)回调音频结果 7

**[3.](#_Toc12274559)****智能音频过滤纠错接口说明 **** 10**

[3.1](#_Toc12274560)请求参数 10

[3.2](#_Toc12274561)返回参数 11

[3.3](#_Toc12274562)示例 11

**[4. FAQ 12](#_Toc12274563)**

[4.1](#_Toc12274564)调用接口返回参数错误（1902） 12

[4.2](#_Toc12274565)调用接口返回无权限操作（9101） 12

[4.3](#_Toc12274566)调用接口超时问题 12

# 1. 接入前准备

## 1.1 数美服务账号申请

客户经理已提前与贵公司建立联系或当面拜访，可直接将开通账号及服务相关信息提供至客户经理。

开通账号所需信息包括：

公司全称：xxxxxx

公司简称：xxxxxx

接口人邮箱：xxx@xxx.xxx

接口人手机：1xxxxxxxxxx

## 1.2 渠道配置表

数美根据客户不同业务场景，配置不同的渠道（channel），制定针对性的拦截策略，同时也方便客户针对不同业务场景的数据进行筛选、分析。业务场景和渠道取值对应表如下（支持客户自定义）：

| **业务场景** | **channel**** 取值 **|** 备注** |
| --- | --- | --- |
| 群聊语音 | GROUP\_CHAT | 群聊中的语音消息 |
| 私聊语音 | MESSAGE | 私聊中的语音消息 |
| 录播语音 | AUDIO\_PROFILE | 语音录播文件 |

## 1.3数美服务账号信息接收

数美客户经理会在1个工作日内为您开通相应数美账号及服务，随后接口人邮箱会收到如下信息：

| **名称** | **具体值** | **说明** |
| --- | --- | --- |
| accessKey | xxxxxx | 数美API服务的认证码，调用数美API时需要传入 |
| organization | xxxxxx | 数美分配的企业唯一标识码，调用SDK时需要传入 |
| 数美管理后台账号 | xxxxxx | 用于登陆数美管理后台 |
| 数美管理后台密码 | xxxxxx | 用于登陆数美管理后台 |
| 数美管理后台地址 | https://www.fengkongcloud.com | 用于登陆数美管理后台 |

# 2. 智能语音对话识别接口说明

## 2.1 上传语音对话文件

**接口描述**

该接口用于提交语音对话文件相关信息，识别结果需客户通过回调接口获取。

**请求**** URL ****：**

http://audio-api.fengkongcloud.com/v2/saas/anti\_fraud/conversation

**字符编码格式：** 使用UTF-8字符集进行编码

**请求方法：** POST

**建议超时时长** ：3s

**请求参数：**

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型** | **是否必选** | **说明** |
| --- | --- | --- | --- |
| accessKey | string | Y | 用于权限认证，开通账号服务时由数美提供 |
| type | string | Y | 识别违规类型，可选值：DEFAULT：默认取值POLITICAL\_PORN\_MOANPORN：色情识别AD：广告识别POLITICAL：涉政识别MOAN：娇喘识别GENDER：性别识别如需做组合识别，通过下划线连接即可，例如 AD\_PORN\_POLITICAL 用于广告、色情和涉政识别。 |
| btId | string | Y | 音频唯一标识，用于查询识别结果，40个字符以内。 |
| data | json\_object | Y | 请求数据内容，最长1MB，详细内容参见下表 |
| appId | string | N | 应用标识，用于区分相同公司的不同应用，该参数传递值可与数美服务协商，不传递则为默认应用 |
| callback | string | Y | 回调 http 接口，服务将根据该字段回调通知用户审核结果 |
| callbackParam | json\_object | N | 回调透传字段，发送回调请求时服务将该字段内容同音频结果一起返回 |

其中，data的内容如下：

| **参数名称** | **类型** | **是否必选** | **说明** |
| --- | --- | --- | --- |
| url | string | Y | 要检测的音频url地址 |
| channel | string | N | 数据业务场景 |

**返回参数**

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型** | **是否必选** | **说明** |
| --- | --- | --- | --- |
| code | int | Y | 返回码 |
| message | string | Y | 返回码详情描述 |
| requestId | string | Y | 请求唯一标识 |
| btId | int | Y | 唯一标识客户上传的音频，与请求参数中的btId字段对应 |

code和message的列表如下：

| **code** | **message** |
| --- | --- |
| 1100 | 成功 |
| 1902 | 参数不合法 |
| 1903 | 服务失败 |
| 9100 | 余额不足 |
| 9101 | 无权限操作 |

**示例**

**输入音频**** url ****参数示例**

{

"accessKey":"sdsfds",

"type":"DEFAULT",

"data":{

"url":"http://image.qinglv666.com/2018/11/05/152004399\_44183002.mp3",

"tokenId":"asdwef",

"returnAllText":true

},

"callback":"http://localhost:19989",

"appId":"asde",

"btId":"shesdwdddfr43f",

"callbackParam":{

"test1":1,

"test2":"qew",

"test3":true

}

}

**返回示例**

{

"code":1100,

"message":"成功",

"requestId":" a78eef377079acc6cdec24967ecde722",

"btId":"xxxx"

}

## 2.2回调识别结果

用户如果需要服务端主动对音频检测结果进行回调，则需要在请求参数中指定回 调协议接口 URL callback 参数，服务端根据该参数在音频审核完成后，主动回调用户

支持协议：

HTTP

字符编码格式：

请求使用 UTF-8 字符集进行编码

请求方法：

POST

超时时长：

5s

推送策略：

当用户收到推送结果，并返回 HTTP 状态码为 200 时，表示推送成功；否则系 统将进行最多 10 次推送。

请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型** | **是否必选** | **说明** |
| --- | --- | --- | --- |
| code | int | Y | 返回码 |
| message | string | Y | 返回码详情描述 |
| requestId | string | Y | 请求唯一标识 |
| btId | int | Y | 音频唯一标识 |
| callbackParam | json\_object | N | 回调透传字段 |
| riskLevel | string | N | 风险级别（code为1100时存在）
可能返回值：PASS，REVIEW，REJECT
 PASS：正常内容，建议直接放行
 REVIEW：可疑内容，建议人工审核
 REJECT：违规内容，建议直接拦截 |
| detail | json\_array | N | 风险详情（code为1100时存在） |

其中，detail数组中每一项的具体参数如下：

| **参数名称** | **类型** | **是否必选** | **说明** |
| --- | --- | --- | --- |
| startTime | int | Y | 音频片段在音频中的起始时间，单位毫秒 |
| endTime | int | Y | 音频片段在音频中的结束时间，单位毫秒 |
| channelId | int | Y | 该片段说话人身份 |
| audioText | string | Y | 该片段识别的文字内容 |
| audioUrl | string | Y | 音频片段地址 |
| riskLevel | string | Y | 可能返回值： REJECT、REVIEW
 REJECT：违规内容，建议直接拦截REVIEW：可疑内容，建议人工审核 |
| riskType | int | N | 音频片段风险类型，可能取值：
 0：正常100：涉政200：色情210：辱骂250：娇喘300：广告700：黑名单900：自定义 |
| description | string | Y | 风险原因描述 |

code和message的列表如下：

| **code** | **message** |
| --- | --- |
| 1100 | 成功 |
| 1101 | 正在处理中 |
| 1902 | 参数不合法 |
| 1903 | 服务失败 |
| 9100 | 余额不足 |
| 9101 | 无权限操作 |

**返回示例**

{

     "code":1100,

     "message":"成功",

     "requestId":"b891cf2d82e214de45df33fc2bea4875",

"riskLevel":"REJECT",

"btId":"xxxx",

"detail":[{

"audioStarttime":0,

"audioEndtime":4,

"channelId":0,

"description":"涉政-音频",

"riskLevel":"REJECT",

"audioText":"法轮大法好",

"riskType":300

}]

}

# 3. 智能音频过滤纠错接口说明

客户使用数美服务时发现结果有误，可以通过此接口给数美反馈错误信息。

## 3.1请求参数

**请求**** URL**

[https://www.fengkongcloud.com/ApiV2/Feedback/batchError](https://www.fengkongcloud.com/ApiV2/Feedback/batchError)

**字符编码**

请求及返回结果都使用UTF-8字符集进行编码

**请求方法** ：

POST

放在HTTP URL中，具体参数如下：

| **字段** | **类型** | **说明** | **是否必须** |
| --- | --- | --- | --- |
| accessKey | string | 用于权限认证，由数美提供 | 是 |

放在HTTP Body中，采用Json格式，具体参数如下：

| **字段** | **类型** | **说明** | **是否必须** |
| --- | --- | --- | --- |
| feedbackData | json\_array | 反馈数据，为纠错数据所构成的数组 | 是 |
| serviceId | string | 服务类型，取值：POST\_AUDIO | 是 |

feedbackData数组的每个元素为纠错数据，内容如下：

| **字段** | **类型** | **说明** | **是否必须存在** |
| --- | --- | --- | --- |
| organization | string | 公司标识 | 是 |
| serviceId | string | 服务类型，取值POST\_AUDIO | 是 |
| requestId | string | 流水号 | 是 |
| btId | string | 业务唯一标识，如果业务没有的话，不需要传 | 否 |
| timestamp | int | 该流水号数据的请求时间，毫秒时间。 | 是 |
| riskLevel | string | 风险等级PASS：通过REJECT：拒绝 | 是 |
| description | string | 风险原因备注 | 是 |

## 3.2返回参数

请求成功，返回HTTP STATUS 200，HTTP Body为空字符串；

请求失败，返回结果放在HTTP Body中，采用Json格式，具体参数如下：

| **字段名** | **类型** | **说明** | **是否必须** |
| --- | --- | --- | --- |
| code | string | 返回码 | 是 |
| message | string | 详细描述 | 是 |

## 3.3示例

curl -X POST -H "Content-Type: application/json" -d

'{{"organization":"xxxx","serviceId":"POST\_AUDIO","feedbackData":[{"organization":"xxx","serviceId":"POST\_AUDIO","requestId":"a2b8b5af8654072b2a76a4fb32f3fece","timestamp":1531734482000 ,"riskLevel":"REJECT","description":"测试接口"}]}' https://www.fengkongcloud.com/ApiV2/Feedback/batchError?accessKey=xxxx

# 4. FAQ

## 4.1 调用接口返回参数错误（1902）

答：调用数美接口时，code返回1902参数不合法，一般为客户输入的参数格式存在问题，客户可自行分析一下请求格式是否按照接口文档输入，或将请求的数据及返回数据反馈给数美分析解决。

## 4.2 调用接口返回无权限操作（9101）

答：调用数美接口时，code返回9101无权限操作，一般为调用了未开通的服务，沟通确认客户调用的服务接口，开通相应的服务。

## 4.3 调用接口超时问题

答：有如下两个常见问题：

1）DNS问题:

客户通过公网调用数美接口进行测试，客户DNS解析域名较慢，导致第一次请求超时，建议客户更换DNS，不建议客户在host中将域名和ip做绑定，数美更换接口IP导致无法请求接口。

2）网络问题:

客户通过公网调用数美接口，公网网络延迟较长，导致少量请求存在超时。可以建议客户ping数美不同的集群网络，建议客户接入网络延迟较低的数美集群。

12

**CONFIDENTIAL**