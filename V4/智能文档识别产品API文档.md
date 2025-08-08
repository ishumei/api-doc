## 同步接口

### 请求参数

#### 请求URL：

| 集群 | URL | 
| --- | --- | 
| 北京 | `http://api-article-bj.fengkongcloud.com/document/v4` | 
| 弗吉尼亚 | `http://api-article-fjny.fengkongcloud.com/document/v4` |
| 新加坡 | `http://api-article-xjp.fengkongcloud.com/document/v4` |

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
| imgType | string | 网页中的图片识别类型 | Y | 可选值：<br/>POLITY：涉政识别<br/>EROTIC：色情&性感违规识别<br/>VIOLENT：暴恐&违禁识别<br/>QRCODE：二维码识别<br/>ADVERT：广告识别<br/>IMGTEXTRISK：图片文字违规识别（如需要识别图片里文字的违规内容，务必传入图片文字违规识别功能）<br/>BOCR：OCR小语种识别支持和语种自动检测（仅限新加坡集群）<br/>NONE：不审核图片<br/>以上type除NONE以外都可以下划线组合，如POLITY_QRCODE_ADVERT用于涉政、二维码和广告组合识别 |
| txtType | string | 网页中的文字识别类型 | Y | 可选值：<br/>POLITY：涉政检测<br/>VIOLENT：暴恐检测<br/>BAN：违禁检测<br/>EROTIC：色情检测<br/>DIRTY：辱骂检测<br/>ADVERT：广告检测<br/>PRIVACY：隐私检测<br/>ADLAW：广告法检测<br/>MEANINGLESS：无意义检测<br/>FRUAD：网络诈骗检测<br/>UNPOACH：高价值用户防挖检测<br/>TEXTMINOR:未成年人内容检测<br/>TEXTRISK：常规风险检测（包含：涉政、暴恐、违禁、色情、辱骂、广告、隐私、广告法、无意义）<br/>NONE：不审核文本<br/> 以上type除NONE以外都可以下划线组合，如：TEXTRISK_FRUAD；type间组合取并集，如：TEXTRISK_POLITY按照常规风险检测处理 |
| appId | string | 应用标识 | Y | 用于区分应用，需要联系数美服务开通，请使用数美单独提供的传值为准 |
| eventId | string | 事件标识 | Y | 需要联系数美服务开通，请使用数美单独提供的传值为准 |
| callback | string | 回调http接口 | N | 当该字段非空时，服务将根据该字段回调通知用户审核结果|
| acceptLang | string  | 返回标签的语种类型   | N    | 选择返回标签的语种类型<br/>可选值：<br/>zh：中文<br/>en：英文<br/>不传入默认为返回中文标签 |
| data | json_object | 请求的数据内容 | Y | 最长1MB, [详见data参数](#data) |

 其中，<span id="data">data</span>的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| url | string | 要检测的文档链接 | Y | 文档链接可下载，其中网址头部的content-type需为text/html。<br/>网址内容大小500m以内，文本长度限制50w字，图片张数限制500张。（url、text、contents传且只能传其中一个）|
| fileFormat        | string  | 要检测的文档格式| Y   | 要检测的文档格式，传入数据为文档时必传，可选值：<br/>DOCX<br/>PDF<br/>DOC<br/>XLS<br/>XLSX<br/>PPT<br/>PPTX<br/>PPS<br/>PPSX<br/>XLTX<br/>XLTM<br/>XLSB<br/>XLSM<br/>TXT<br/>CSV<br/>EPUB<br/>若fileFormat与文档实际格式不一致，则返回报错参数错误 |
| nickname | string | 用户昵称 | N | 校验昵称内容风险|
| ip | string | ip地址 | N | 发送该文本的的用户公网ipv4或ipv6地址 |
| tokenId | string | 用户账号标识，建议使用贵司用户UID（可加密）自行生成，标识用户唯一身份用作灌水和广告等行为维度风控。<br/>如无用户uid的场景建议使用唯一的数据标识传值 | Y | 由数字、字母、下划线、短杠组成的长度小于等于64位的字符串 |
| lang | string | 待检测的文本内容语种 | N | 可选值和对应语种如下：<br/>`zh`：中文<br/>`en`：英文<br/>`ar`：阿拉伯语<br/>`hi`：印地语<br/>`es`：西班牙语<br/>`fr`：法语<br/>`ru`：俄语<br/>`pt`：葡萄牙语<br/>`id`：印尼语<br/>`de`：德语<br/>`ja`：日语<br/>`tr`：土耳其语<br/>`vi`：越南语<br/>`it`：意大利语<br/>`th`：泰语<br/>`tl`：菲律宾语<br/>`ko`：韩语<br/>`ms`：马来语<br/>`auto`：自动识别语种类型<br/>默认值zh，国内集群客户可不传或zh；海外文本内容如果不能区分语种建议取值auto，系统会自动检测语种类型 |
| receiveTokenId | string | 私聊场景下消息接收者的tokenId | N | 由数字、字母、下划线、短杠组成的字符串|
| returnAllImg | int | 返回图片的等级 | N | 选择返回图片的等级：0：返回风险等级为非pass的图片；1：返回所有风险等级的图片。默认为0 |
| returnAllText | int | 返回文本的等级| N  | 选择返回文本的等级：0：返回风险等级为非pass的文本；1：返回所有风险等级的文本。默认为0 |
| level | int | 用户等级，针对不同等级的用户可配置不同拦截策略 | N  | 可选值：<br/>`0`：最低级用户，典型如新注册、完全不活跃或等级为0的用户等;<br/>`1`：较低级用户，典型如低活跃或低等级用户等；<br/>`2`：中等级用户，典型如具备一定活跃或等级中等的用户等；<br/>`3`：较高级用户，典型如高活跃或高等级用户等；<br/>`4`：最高级用户，典型如付费用户、VIP用户等 |
| gender | string | 用户性别 | N  | 可选值：<br/>male男性 <br/>female女性 |
| deviceId | string | 数美设备标识 | N | 数美设备指纹生成的设备唯一标识 |
| dataId | string | 数据标识 | N | 数据标识 |
| extra | json_object | 辅助参数 | N | 用于辅助文本检测的相关信息，[详见extra参数](#extra) |

<span id="extra">data 中 extra数组每个元素的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| room | string | 直播间/游戏房间编号 | N | 传入的是直播间、聊天室等数据（eventId值为groupChat）时，开启上下文识别功能，建议传入，否则不能关联上下文 |
| passThrough | Json | 透传字段 | N | 该字段内容会随着返回值一起返回 |

#### 回调返回参数：

#### <span id="Ab1">回调返回</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int| 返回码 | Y | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9101`：无权限操作 |
| message | string | 返回码描述 | Y | 和code对应：<br/>成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>无权限操作 |
| requestId | string | 请求标识 | Y | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存|
| riskLevel | string | 处置建议 | N | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| textDetail | json_array | 风险详情 | N | [详见textDetail参数](#textDetail) |
| imgDetail | json_array | 风险详情 | N | [详见imgDetail参数](#imgDetail) |
| auxInfo | json_object | 辅助信息 | Y | [详见auxInfo参数](#auxInfo)  |

其中，<span id="textDetail">textDetail</span>的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1| string | 一级风险标签 | Y | 一级风险标签，当riskLevel为`PASS`时返回`normal` |
| riskLabel2| string | 二级风险标签 | Y | 二级风险标签，当riskLevel为`PASS`时为空|
| riskLabel3| string | 三级风险标签 | Y | 三级风险标签，当riskLevel为`PASS`时为空|
| riskDescription | string | 风险原因 | Y | 当riskLevel为`PASS`时为"正常"|
| riskDetail | json_object| 风险详情 | Y | 风险详情，[详见riskDetail参数](#riskDetail)|
| allLabels | json_array | 辅助信息 | Y | 命中的所有风险标签以及详情信息。[详见allLabels参数](#allLabels) |
| tokenProfileLabels | json_array | 辅助信息 | N | 属性账号类标签。[详见账号标签参数](#tokenProfileLabels) |
| tokenRiskLabels | json_array | 辅助信息 | N | 风险账号类标签。[详见账号标签参数](#tokenProfileLabels) |

其中，<span id="riskDetail">textDetail的riskDetail的内容如下：</span>

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| matchedLists | json_array | 辅助信息 | N| 命中的客户自定义名单列表。[详见matchedLists参数](#matchedLists) |
| riskSegments | json_array | 辅助信息，高风险内容片段检测文本包含涉政、暴恐、违禁、广告法等风险内容的时候存在 | N| [详见riskSegments参数](#riskSegments) |

其中，<span id="matchedLists">textDetail的riskDetail下matchedLists数组每个元素的内容如下：</span>

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范**|
| --- | --- | --- | --- | --- |
| name | string | 辅助信息 | N| 命中的名单名称|
| words| json_array | 辅助信息 | N| 命中的敏感词数组。[详见words参数](#words) |

其中，<span id="words">matchedLists中，words数组每个元素的内容如下：</span>

| **参数名称** | **类型**| **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string| 辅助信息 | N| 命中的敏感词 |
| position | int_array | 辅助信息 | N| 敏感词所在位置 |

其中，<span id="riskSegments">textDetail的riskDetail下riskSegments的内容如下：</span>

| **参数名称** | **类型**| **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segment| string| 辅助信息 | N| 高风险内容片段 |
| position | int_array | 辅助信息 | N| 高风险内容片段所在位置 |

其中，<span id="imgDetail">imgDetail</span>的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1| string | 一级风险标签 | Y | 一级风险标签，当riskLevel为`PASS`时返回`normal` |
| riskLabel2| string | 二级风险标签 | Y | 二级风险标签，当riskLevel为`PASS`时为空|
| riskLabel3| string | 三级风险标签 | Y | 三级风险标签，当riskLevel为`PASS`时为空|
| riskDescription | string | 风险原因 | Y | 当riskLevel为`PASS`时为"正常"|
| riskDetail | json_object| 风险详情 | Y | 风险详情，[详见riskDetail参数](#riskDetail)|
| allLabels | json_array | 辅助信息 | Y | 命中的所有风险标签以及详情信息。[详见allLabels参数](#allLabels) |
| tokenProfileLabels | json_array | 辅助信息 | N | 属性账号类标签。[详见账号标签参数](#tokenProfileLabels) |
| tokenRiskLabels | json_array | 辅助信息 | N | 风险账号类标签。[详见账号标签参数](#tokenProfileLabels) |

其中，<span id="riskDetail">imgDetail的riskDetail结构如下：</span>

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| faces | json_array | 返回图片中涉政人物的名称及位置信息 | N |  |
| face_num | int | 人脸数量 | N |  |
| persons | json_array | 仅当命中人像-多人时，数组元素会有多个，最多10个 | N | |
| person_num | int | 人像数量 | N | 有且仅有人像-多人下返回 |
| objects | json_array | 返回图片中物品或标志二维码的位置信息 | N | 数组仅会有一个元素 |
| ocrText | json_object | 返回图片中违规文字相关信息，当请求参数type字段包含`IMGTEXTRISK`和ADVERT时存在 | N | |
| riskSource | int | 标识资源哪里违规 | Y | 标识风险结果的来源<br/>`1000`：无风险<br/>`1001`：文字风险<br/>`1002`：视觉图片风险 |

其中,imgDetail的riskDetail下faces数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| id | string | 人物编号 | N | 图片同一个位置下的人在不同标签下的编号相同。<br/>如果同一个人在图片中出现n次，分配n个ID |
| name | string | 人物名称 | N | 能识别的公众人物名称 |
| location | int_array | 人物位置信息，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | N |  |
| face_ratio | float | 人脸占比 | N | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | N | 0～1之间的浮点数 |

其中，imgDetail的riskDetail下objects数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| id | string | 编号，保证同一个位置下的物品在不同标签下的编号相同 | N |  |
| name | string | 标识名称 | N | |
| location | int_array | 标识位置信息，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标  | N | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | N | 0～1之间的浮点数 |
| qrContent | string | 二维码的url信息 | N | 仅当命中二维码相关标签时返回 |

其中，imgDetail的riskDetail下persons数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| id | string | 编号，保证同一个人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID | N |  |
| person_ratio | string | 人像在图中的占比 | N | |
| location | int_array | 人像位置坐标 | N | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | N | 0～1之间的浮点数 |

其中，imgDetail的riskDetail下ocrText的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| text | string | 识别出的文字 | Y | |
| matchedLists | json\_array | 命中的客户自定义名单列表 | N | |
| riskSegments | json\_array | 高风险片段内容，检测图片包含涉政、暴恐、违禁、广告法等风险内容的时候存在 | N |  |

其中，ocrText的matchedLists数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 命中的名单名称 | N | |
| words | json\_array | 命中的敏感词信息 | N | |

其中，matchedLists的words数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string | 命中的敏感词 | N | |
| position | int_array | 敏感词所在位置 | N | |

其中，ocrText的riskSegments的每个元素的详细内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segment | string | 高风险内容片段 | N | |
| position | int_array | 高风险内容片段所在位置 | N | |

其中，<span id="allLabels">allLabels的内容如下：</span>

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | allLabels不为空时必返 | Y | 一级风险标签 |
| riskLabel2 | string | allLabels不为空时必返 | Y | 二级风险标签|
| riskLabel3 | string | allLabels不为空时必返 | Y | 三级风险标签|
| riskDescription| string | allLabels不为空时必返 | Y | 风险原因|
| probability| float| 置信度 | Y | 可选值在0～1之间，值越大，可信度越高 注意：allLabels不为空时必返 |
| riskDetail | json_object| 风险详情 | Y | 格式与上层riskDetail结构相同 注意：allLabels不为空时必返 |
| riskLevel | string | 风险等级 | Y | 可能返回值：<br/>`REVIEW`：可疑<br/>`REJECT`：违规 |

其中，<span id="tokenProfileLabels">tokenProfileLabels、tokenRiskLabels的内容如下：</span>

| 参数名称    | 类型   | 参数说明     | 是否必返 | 规范                       |
| ----------- | ------ | ------------ | -------- | -------------------------- |
| label1      | string | 一级标签     | N      |                            |
| label2      | string | 二级标签     | N       |                            |
| label3      | string | 三级标签     | N       |                            |
| description | string | 标签描述     | N       |                            |
| timestamp   | Int    | 打标签时间戳 | N       | 13位Unix时间戳，单位：毫秒 |

其中，<span id="auxInfo">auxInfo</span>的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| textNum      | int   | 当前请求中的字符数，与计费数目一致     | Y            | 当前请求中的字符数，其中字符数包括汉字，英文，标点符号，空格等   |
| imgNum       | int   | 当前请求中的图片数，与计费数目一致     | Y            | 当前请求中的图片数，如遇动图会截取3帧；如遇长图会进行切分 |

#### 回调模式的同步返回

#### 请求返回参数：

| **参数名称** | **类型**    | **参数说明** | **是否必返** | **规范**                                                     |
| ------------ | ----------- | ------------ | ------------ | ------------------------------------------------------------ |
| code         | int         | 返回码       | Y            | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9100`：余额不足<br/>`9101`：无权限操作 |
| message      | string      | 返回码描述   | Y            | 和code对应：<br/>成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>余额不足<br/>无权限操作 |
| requestId    | string      | 请求标识     | Y            | 本次请求数据的唯一标识,用于问题排查和效果优化，强烈建议保存  |


### 示例

#### 回调模式

##### 请求示例

```json
{
    "accessKey": "xxx",
    "appId": "xxx",
    "imgType": "xxx",
    "txtType": "xxx",
    "eventId": "xxx",
    "data": {
        "url": "https://test-10029305.cos.ap-shanghai.myqcloud.com/wg_txt/README.md",
        "fileFormat":"MD",
        "tokenId": "xxx"
    }
}
```

#### 同步返回示例
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
    "textDetail": [
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
    "imgDetail": [

    ],
    "auxInfo": {
        "imgCount": 4,
        "textCount": 317
    }
}
```

