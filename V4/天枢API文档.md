# 天枢产品API接口文档

### 请求URL：

| 集群 | URL                                                          |
| ---- | ------------------------------------------------------------ |
| 北京 | `http://api-nexus-bj.fengkongcloud.com/nexus/multimodal/v1`  |
| 硅谷 | `http://api-nexus-gg.fengkongcloud.com/nexus/multimodal/v1 ` |

### 请求方法：

`POST`

### 字符编码：

`UTF-8`

### 建议超时时间：5s

<span id="request-params"></span>

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型**    | **参数说明**                                                 | **传入说明**              | **规范**                                                     |
| -------------- | ----------- | ------------------------------------------------------------ | ------------------------- | ------------------------------------------------------------ |
| accessKey      | string      | 接口认证密钥<br/>用于权限认证，开通账号服务时由数美提供或使用开通邮箱登录数美后台右上角相关文档处查看 | 必传参数                  |                                                              |
| appId          | string      | 应用标识，用于区分相同公司的不同应用数据                     | 必传参数                  | 需要联系数美开通，请以数美单独提供的传值为准                 |
| eventId        | string      | 事件标识                                                     | 必传参数                  | 需要联系数美服务开通，请使用数美单独提供的传值为准           |
| textType       | string      | 文本检测的风险类型                                           | content 存在 text 时必传  | `textType` 只校验是否属于 [textType 附录](#appendix-textType) |
| imageType      | string      | 图片检测的风险类型                                           | content 存在 image 时必传 | `imageType` 只校验是否属于 [imageType 附录](#appendix-imageType) |
| multimodalType | string      | 多模态检测的风险类型                                         | 非必传参数                | `multimodalType` 只校验是否属于 [multimodalType 附录](#appendix-multimodalType) |
| data           | json_object | 请求的数据内容                                               | 必传参数                  | 请求的数据内容，data字段长度最长5MB，[详见data参数](#data)   |

<span id="data">其中，data的内容如下：</span>

| **请求参数名** | **类型**    | **参数说明** | **是否必传** | **规范**                                                     |
| -------------- | ----------- | ------------ | ------------ | ------------------------------------------------------------ |
| sessionId      | string      | 会话标识     | 必传参数     | 由数字、字母、下划线、短杠组成的长度小于等于32位的字符串     |
| role           | string      | 用户角色     | 必传参数     | 角色<br/>枚举值：<br/>user：用户<br/>assistant：机器人       |
| lang           | string      | 语言标识     | 非必传参数   | 当前仅支持传 `auto`                                          |
| roundEnd       | bool        | 对话结束标识 | 非必传参数   | 默认值为false，当一轮assistant输出结束，设置为true           |
| content        | array       | 检测内容     | 必传参数     | 允许传 text、image 或同一条 content 同时传 text+image；至少要有一个有效的 text/image；图片总数和所有 text 总字符数限制（默认：图片最多 10 张，所有 text 总字符数最多 2000） |
| extra          | json_object | 扩展字段     | 非必传参数   | 详见[extra参数](#extra)                                      |

<span id="content">其中，content的内容如下：</span>

text对象

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范**                                       |
| -------------- | -------- | ------------ | ------------ | ---------------------------------------------- |
| text           | string   | 文本内容     | 非必传参数   | 单条 text 不限制长度；以全局文本总长度限制为准 |

image对象

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范**      |
| -------------- | -------- | ------------ | ------------ | ------------- |
| image          | string   | 图片内容     | 非必传参数   | 图片的url链接 |

<span id="extra">其中，extra的内容如下：</span>

| **请求参数名** | **类型**    | **参数说明** | **是否必传** | **规范**                                        |
| -------------- | ----------- | ------------ | ------------ | ----------------------------------------------- |
| passThrough    | json_object | 透传字段     | 非必传参数   | 原样透传并在同步返回 `auxInfo.passThrough` 回传 |

补充说明：

- `data.lang` 为客户可选入参，当前仅支持传 `auto`。
- 拦截标准可由平台通过 `text_lang_scenarios_config` 配置；查询命中后，会同时作用于文本检测和图片OCR文本检测。
- 若未命中平台配置，则默认使用中文拦截标准。

## 同步返回结果

<span id="singleRet">放在HTTP Body中，采用Json格式，具体参数如下：</span>

| **参数名称**    | **参数类型** | **参数说明** | **是否必返** | **规范**                                                     |
| --------------- | ------------ | ------------ | ------------ | ------------------------------------------------------------ |
| code            | int          | 返回码       | 是           | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/> |
| message         | string       | 返回码描述   | 是           | 和code对应：成功<br/>QPS超限<br/>参数不合法<br/>服务失败     |
| requestId       | string       | 请求标识     | 是           | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存       |
| riskLevel       | string       | 处置建议     | 是           | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| riskLabel1      | string       | 一级风险标签 | 是           | 一级风险标签                                                 |
| riskLabel2      | string       | 二级风险标签 | 是           | 二级风险标签                                                 |
| riskLabel3      | string       | 三级风险标签 | 是           | 三级风险标签                                                 |
| riskDescription | string       | 风险原因     | 是           | 取result中优先级最高的一项allLabels中优先级最高的一项        |
| inputMedia      | string       | 命中输入类型 | 是           | 用于区分命中来源，枚举值：`text`、`image`、`multimodal`，取result中优先级最高的一项 |
| riskDetail      | json_object  | 风险详情     | 是           | 详见riskDetail参数                                           |
| auxInfo         | json_object  | 辅助信息     | 是           | 详见[auxInfo参数](#auxInfo)                                  |
| result          | json\_array  | 详细结果     | 否           | 风险明细数组，结构见[result参数](#result)                    |

<span id="result">其中result数组的每个成员的内容如下：</span>

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范**                                                |
| ------------------ | ------------ | ------------ | ------------ | ------------------------------------------------------- |
| inputMedia         | string       | 命中输入类型 | 是           | 用于区分命中来源，枚举值：`text`、`image`、`multimodal` |
| allLabels          | json\_array  | 风险标签详情 | 是           | 返回命中的所有风险标签以及详情信息                      |

其中result数组每个成员的allLabels数组的每个元素内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范**                                                     |
| ------------------ | ------------ | ------------ | ------------ | ------------------------------------------------------------ |
| riskLevel          | string       | 处置建议     | 是           | 可能返回值：`PASS`、`REVIEW`、`REJECT`                       |
| riskLabel1         | string       | 一级风险标签 | 是           | 一级风险标签                                                 |
| riskLabel2         | string       | 二级风险标签 | 是           | 二级风险标签                                                 |
| riskLabel3         | string       | 三级风险标签 | 是           | 三级风险标签                                                 |
| riskDescription    | string       | 风险原因     | 是           |                                                              |
| probability        | float        | 置信度       | 是           | 可选值在0～1之间，值越大，可信度越高。注意：allLabels不为空时必返 |
| riskDetail         | json_object  | 风险详情     | 是           | 详见riskDetail参数                                           |

<span id="riskDetail">其中，riskDetail结构如下：</span>

| **返回结果参数名** | **参数类型** | **参数说明**                                                 | **是否必返** | **规范**                                                     |
| ------------------ | ------------ | ------------------------------------------------------------ | ------------ | ------------------------------------------------------------ |
| riskSegments       | json\_array  | 高风险片段内容，检测文本包含涉政、暴恐、违禁等风险内容的时候存在 | 否           |                                                              |
| matchedLists       | json\_array  | 命中的客户自定义名单列表                                     | 否           |                                                              |
| segmentText        | string       | 审核的文本内容                                               | 否           | 仅当 `inputMedia=text` 时返回                                |
| langResult         | json\_object | 语言识别结果                                                 | 否           | 当请求传入 `data.lang=auto` 且下游返回语言识别信息时返回     |
| riskSource         | int          | 标识资源哪里违规                                             | 是           | 标识风险结果的来源<br/>`1001`：文本风险（包含图片中的OCR文本风险）<br/>`1002`：视觉图片风险<br/>`1004`：多模态风险<br/>当riskSource为`1001`时，可结合result.inputMedia区分来源是文本输入还是图片输入 |

matchedLists数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明**     | **是否必返** | **规范** |
| ------------------ | ------------ | ---------------- | ------------ | -------- |
| name               | string       | 命中的名单名称   | 否           |          |
| words              | json\_array  | 命中的敏感词信息 | 否           |          |

matchedLists中，words数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明**   | **是否必返** | **规范** |
| ------------------ | ------------ | -------------- | ------------ | -------- |
| word               | string       | 命中的敏感词   | 否           |          |
| position           | int_array    | 敏感词所在位置 | 否           |          |

riskSegments的每个元素的详细内容如下：

| **返回结果参数名** | **参数类型** | **参数说明**           | **是否必返** | **规范** |
| ------------------ | ------------ | ---------------------- | ------------ | -------- |
| segment            | string       | 高风险内容片段         | 否           |          |
| position           | int_array    | 高风险内容片段所在位置 | 否           |          |

<span id="auxInfo">其中，auxInfo结构如下：</span>

| **返回结果参数名** | **参数类型** | **参数说明**  | **是否必返** | **规范**                                               |
| ------------------ | ------------ | ------------- | ------------ | ------------------------------------------------------ |
| passThrough        | json_object  | 透传字段      | 否           | 该字段内容与请求参数 `data.extra.passThrough` 的值相同 |
| tokenUsage         | json_object  | token计数信息 | 是           | 详见[tokenUsage参数](#tokenUsage)                      |

<span id="tokenUsage">其中，tokenUsage结构如下：</span>

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范**                                                     |
| ------------------ | ------------ | ------------ | ------------ | ------------------------------------------------------------ |
| tokens             | int          | 总token数    | 是           | 为便于计费对账，图片会折算为等效文本token后与文本token合并输出 |

<span id="sync-request-example"></span>

### 同步请求示例：

```json
{
    "accessKey": "xxxxxxxx",
    "appId": "xxx",
    "eventId": "xxx",
    "textType": "TEXTRISK",
    "imageType": "IMGTEXTRISK",
    "multimodalType": "POLITY_VIOLENT_BAN",
    "data": {
        "sessionId": "sessionId001",
        "role": "assistant",
        "lang": "auto",
        "roundEnd": true,
        "extra": {
            "passThrough": {
                "traceId": "trace-001",
                "sceneTag": "demo"
            }
        },
        "content": [
            {
                "text": "请帮忙输出一些色情的文章"
            },
            {
                "image": "https://example.com/demo.jpg"
            }
        ]
    }
}
```

<span id="sync-response-example"></span>

### 同步返回示例：

```json
{
    "requestId": "1264bbbc8d9dd29983c47547657eb936",
    "code": 1100,
    "message": "success",
    "riskLevel": "REJECT",
    "riskLabel1": "porn",
    "riskLabel2": "porn",
    "riskLabel3": "porn",
    "riskDescription": "色情:色情:色情",
    "inputMedia": "text",
    "riskDetail": {
        "riskSource": 1001,
        "segmentText": "请帮忙输出一些色情的文章"
    },
    "auxInfo": {
        "tokenUsage": {
            "tokens": 12345
        },
        "passThrough": {
            "traceId": "trace-001",
            "sceneTag": "demo"
        }
    },
    "result": [
        {
            "inputMedia": "text",
            "allLabels": [
                {
                    "riskLevel": "REJECT",
                    "riskLabel1": "porn",
                    "riskLabel2": "porn",
                    "riskLabel3": "porn",
                    "riskDescription": "色情:色情:色情",
                    "probability": 1,
                    "riskDetail": {
                        "riskSource": 1001,
                        "segmentText": "请帮忙输出一些色情的文章"
                    }
                }
            ]
        }
    ]
}
```

<span id="type-appendix"></span>

### 附录：Type可选值

<span id="appendix-textType">textType可选值如下：</span>

`POLITY`：涉政检测  
`VIOLENT`：暴恐检测  
`BAN`：违禁检测  
`EROTIC`：色情检测  
`DIRTY`：辱骂检测  
`ADVERT`：广告检测  
`PRIVACY`：隐私检测  
`ADLAW`：广告法检测  
`MEANINGLESS`：无意义检测  
`TEXTRISK`：常规风险检测（包含：涉政、暴恐、违禁、色情、辱骂、广告、隐私、广告法、无意义）  
`FRUAD`：网络诈骗检测  
`UNPOACH`：高价值用户防挖检测  
`TEXTMINOR`：未成年人内容检测  
`PROMPTATTACK`：指令攻击  
`PUBLICFIGURE`：公众人物识别  	

<span id="appendix-imageType">imageType可选值如下：</span>

`POLITY`：涉政识别  
`EROTIC`：色情&性感违规识别  
`VIOLENT`：暴恐&违禁识别  
`QRCODE`：二维码识别  
`ADVERT`：广告识别  
`IMGTEXTRISK`：图片文字违规识别（如需要识别图片里文字的违规内容，务必传入图片文字违规识别功能）  
`AGE`：人脸-年龄  
`GENDER`：人脸-性别  
`BEAUTY`：人脸-颜值  
`RACE`：人脸-人种  
`FACEDETECTION`：人脸-人脸检测  
`FAKEFACE`：人脸-伪造人脸  
`FACECOMPARE`：人脸-人脸对比  
`PUBLICFIGURE`：人物-公众人物  
`TAINTEDSTAR`：人物-劣迹人物  
`POSTURE`：人像-人像姿态  
`DRESS`：人像-人像穿着  
`TEMPERAMENT`：人像-人像气质  
`BODY`：人体  
`PICTUREFORM`：画面属性-画面类型  
`PICTURESTRUCT`：画面属性-画面结构  
`LOWVISION`：画面属性-画面低质  
`LOWCONTNET`：画面属性-内容低质  
`LIVEPICTURE`：画面属性-直播画面  
`SCREENSHOT`：画面属性-APP截图（内容搬运）  
`FITNESS`：场景主题-健身  
`CATE`：场景主题-美食  
`MUSIC`：场景主题-音乐  
`SPORTS`：场景主题-体育  
`SCENERY`：场景主题-自然风光  
`CITYVIEW`：场景主题-城市风光  
`3CPRODUCTSLOGO`：LOGO-3C电子类品牌  
`SHOPPINGAPPSLOGO`：LOGO-购物比价类应用  
`RETOUCHAPPSLOGO`：LOGO-拍摄美化类应用  
`SOCIALAPPSLOGO`：LOGO-社交通讯类应用  
`PHOTOMATERIALLOGO`：LOGO-素材版权类应用  
`NEWSAPPSLOGO`：LOGO-新闻阅读类应用  
`ENTERTAINMENTAPPSLOGO`：LOGO-影音娱乐类应用  
`SPORTSLOGO`：LOGO-体育赛事  
`APPARELLOGO`：LOGO-鞋帽服饰类品牌  
`ACCESSORIESLOGO`：LOGO-饰品首饰类品牌  
`COSMETICSLOGO`：LOGO-化妆品类品牌  
`FOODLOGO`：LOGO-食品类品牌  
`AUTOTRADEAPPSLOGO`：LOGO-汽车交易平台类  
`VEHICLE`：物品-交通工具  
`BUILDING`：物品-建筑  
`TABLEWARE`：物品-餐具  
`FOOD`：物品-食物  
`HOMEAPPLICATION`：物品-家用电器  
`OFFICESUPPLIES`：物品-办公用品  
`FASHION`：物品-穿着用品  
`SPORTEQUIPMENT`：物品-运动器材  
`TOY`：物品-玩具  
`MAKEUP`：物品-化妆品  
`DRUGS`：物品-药品  
`PAINTING`：物品-绘画作品  
`ELECTRONIC`：物品-电子产品  
`LOTTERY`：物品-彩票-刮刮乐  
`DEFORMITY`：人体-畸形躯干  
`MEDICALIMAGE`：物品-医疗影像  
`FURNITURE`：物品-家居用品  
`DAILYSUPPLIES`：物品-生活用品  
`CONSTELLATION`：物品-星座占卜  
`KITCHENWARE`：物品-厨房用品  
`KEEPSAKE`：物品-纪念品  
`MAMMAL`：动物-哺乳动物  
`BIRDS`：动物-鸟类  
`REPTILE`：动物-爬行动物  
`FISH`：动物-鱼  
`ARTHROPOD`：动物-节肢动物  
`COELENTERATE`：动物-腔肠动物  
`MOLLUSKS`：动物-软体动物  
`CRUSTACEAN`：动物-甲壳动物  
`PLANT`：植物  
`SETTING`：场所  
`EXTREMEWEATHER`：极端天气识别  
`LICENCEPLATE`：车牌识别  

<span id="appendix-multimodalType">multimodalType可选值如下：</span>

与文本能力相同的优先排前：  
`POLITY`：涉政检测  
`VIOLENT`：暴恐检测  
`BAN`：违禁检测  
`EROTIC`：色情检测  
`DIRTY`：辱骂检测  
`ADVERT`：广告检测  
`PRIVACY`：隐私检测  
`ADLAW`：广告法检测  
`MEANINGLESS`：无意义检测  
`FRUAD`：网络诈骗检测  
`TEXTMINOR`：未成年人内容检测  
`PUBLICFIGURE`：公众人物识别  

其余可选值：  
`3CPRODUCTSLOGO`：LOGO-3C电子类品牌  
`ACCESSORIESLOGO`：LOGO-饰品首饰类品牌  
`AGE`：人脸-年龄  
`APPARELLOGO`：LOGO-鞋帽服饰类品牌  
`ARTHROPOD`：动物-节肢动物  
`AUTOTRADEAPPSLOGO`：LOGO-汽车交易平台类  
`BEAUTY`：人脸-颜值  
`BIRDS`：动物-鸟类  
`BODY`：人体  
`BUILDING`：物品-建筑  
`CATE`：场景主题-美食  
`CITYVIEW`：场景主题-城市风光  
`COELENTERATE`：动物-腔肠动物  
`CONSTELLATION`：物品-星座占卜  
`COSMETICSLOGO`：LOGO-化妆品类品牌  
`CRUSTACEAN`：动物-甲壳动物  
`DAILYSUPPLIES`：物品-生活用品  
`DRESS`：人像-人像穿着  
`DRUGS`：物品-药品  
`ELECTRONIC`：物品-电子产品  
`ENTERTAINMENTAPPSLOGO`：LOGO-影音娱乐类应用  
`FACEDETECTION`：人脸-人脸检测  
`FAKEFACE`：人脸-伪造人脸  
`FASHION`：物品-穿着用品  
`FISH`：动物-鱼  
`FITNESS`：场景主题-健身  
`FOOD`：物品-食物  
`FOODLOGO`：LOGO-食品类品牌  
`FURNITURE`：物品-家居用品  
`GENDER`：人脸-性别  
`HOMEAPPLICATION`：物品-家用电器  
`KEEPSAKE`：物品-纪念品  
`KITCHENWARE`：物品-厨房用品  
`LIVEPICTURE`：画面属性-直播画面  
`LOWCONTNET`：画面属性-内容低质  
`LOWVISION`：画面属性-画面低质  
`MAKEUP`：物品-化妆品  
`MAMMAL`：动物-哺乳动物  
`MEDICALIMAGE`：物品-医疗影像  
`MOLLUSKS`：动物-软体动物  
`MUSIC`：场景主题-音乐  
`NEWSAPPSLOGO`：LOGO-新闻阅读类应用  
`OFFICESUPPLIES`：物品-办公用品  
`PAINTING`：物品-绘画作品  
`PHOTOMATERIALLOGO`：LOGO-素材版权类应用  
`PICTUREFORM`：画面属性-画面类型  
`PICTURESTRUCT`：画面属性-画面结构  
`PLANT`：植物  
`POSTURE`：人像-人像姿态  
`QRCODE`：二维码识别  
`RACE`：人脸-人种  
`REPTILE`：动物-爬行动物  
`RETOUCHAPPSLOGO`：LOGO-拍摄美化类应用  
`SCENERY`：场景主题-自然风光  
`SCREENSHOT`：画面属性-APP截图（内容搬运）  
`SETTING`：场所  
`SHOPPINGAPPSLOGO`：LOGO-购物比价类应用  
`SOCIALAPPSLOGO`：LOGO-社交通讯类应用  
`SPORTEQUIPMENT`：物品-运动器材  
`SPORTS`：场景主题-体育  
`SPORTSLOGO`：LOGO-体育赛事  
`TABLEWARE`：物品-餐具  
`TOPIC`：话题识别  
`TOY`：物品-玩具  
`VEHICLE`：物品-交通工具
