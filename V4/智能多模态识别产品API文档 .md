# 数美智能多模态识别产品API接口文档

### 请求URL：

| 集群 | URL |
| --- | --- |
| 北京 | `http://api-img-bj.fengkongcloud.com/multimodal/v1` |

### 请求方法：

`POST`

### 字符编码：

`UTF-8`

### 建议超时时间：10s

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥<br/>用于权限认证，开通账号服务时由数美提供或使用开通邮箱登录数美后台右上角相关文档处查看 | 必传参数 |  |
| appId | string | 应用标识，用于区分相同公司的不同应用数据 | 必传参数 | 需要联系数美开通，请以数美单独提供的传值为准 |
| eventId | string | 事件标识 | 必传参数 | 需要联系数美服务开通，请使用数美单独提供的传值为准 |
| type | string | 检测的风险类型 | type和businessType必传其一 | 监管一级标签 可选值:<br/>POLITY :涉政识别<br/>EROTIC :色情 <br/>VIOLENT :暴恐<br/>EROTIC :色情/性感 <br/>DIRTY :辱骂 <br/>ADVERT :广告<br/>ADLAW :广告法 <br/>MEANINGLESS :无意义 <br/>PRIVACY :隐私<br/>TEXTMINOR :未成年人 <br/>FRUAD :网络诈骗 <br/>QRCODE :二维码 <br/>IMGTEXTRISK  :图片文字风险 <br/>TEXTRISK :文本风险 <br/>如果需要识别多个功能，通过下划线连接，如 POLITY_BAN_EROTIC<br/> |
| businessType | string | 检查的业务标签 | type和businessType必传其一 | 业务标签<br/>可选值：[见附录](#附录)如果需要多个识别功能，通过下划线连接，该字段和type必须选择一个传入 |
| data | json_object | 请求的数据内容 | 必传参数 | 请求的数据内容，data字段长度最长5MB，[详见data参数](#data) |

<span id="data">其中，data的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| tokenId | string | 用户账号标识 | 非必传参数 | 由数字、字母、下划线、短杠组成的长度小于等于32位的字符串 |
| sessionId | string | 会话标识 | 必传参数 | 由数字、字母、下划线、短杠组成的长度小于等于32位的字符串 |
| role | string | 用户角色 | 必传参数 | 角色<br/>枚举值：<br/>user：用户<br/>assistant：机器人<br/> |
| content | array | 检测内容 | 必传参数 | 包含顺序关系的图文对象，最多传入图文各一个 |

<span id="content">其中，content的内容如下：</span>

image对象

| **请求参数名** | **类型** | **参数说明**           | **是否必传** | **规范**                                                   |
| -------------- | -------- | ---------------------- | ------------ | ---------------------------------------------------------- |
| image          | string   | 图片地址或者base64数据 | 非必传参数   | image请求格式支持：PNG、JPG、JPEG、BMP，图片大小限制小于5M |

text对象

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范**              |
| -------------- | -------- | ------------ | ------------ | --------------------- |
| text           | string   | 文本内容     | 非必传参数   | 文本内容小于512个字符 |

## 同步返回结果

<span id="singleRet">放在HTTP Body中，采用Json格式，具体参数如下：</span>

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 |  `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/> |
| message | string | 返回码描述 | 是 | 和code对应：成功<br/>QPS超限<br/>参数不合法<br/>服务失败 |
| requestId | string | 请求标识 | 是 | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存 |
| riskLevel | string | 处置建议 | 是 | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| riskLabel1 | string | 一级风险标签 | 是 | 当riskLevel为`PASS`时返回`normal` |
| riskLabel2 | string | 二级风险标签 | 是 | 当riskLevel为`PASS`时为空 |
| riskLabel3 | string | 三级风险标签 | 是 | 当riskLevel为`PASS`时为空 |
| riskDescription | string | 风险原因 | 是 | 当riskLevel为`PASS`时为`正常` |
| riskDetail | json_object | 风险详情 | 是 | [详见riskDetail参数](#riskDetail) |
| allLabels | json_array | 风险标签详情 | 否 | 返回命中的所有风险标签以及详情信息 |
| businessLabels | json_array | 业务标签详情 | 否 | 返回命中的所有业务标签以及详情信息 |



<span id="riskDetail">其中，riskDetail结构如下：</span>

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskSegments | json\_array | 高风险片段内容，检测文本包含涉政、暴恐、违禁等风险内容的时候存在 | 否 |  |
| matchedLists | json\_array | 命中的客户自定义名单列表 | 否 |  |
| faces | json_array | 返回图片中涉政人物的名称及位置信息 | 否 |  |
| face_num | int | 人脸数量 | 否 |  |
| persons | json_array | 仅当命中人像-多人时，数组元素会有多个，最多10个 |否  | |
| person_num | int | 人像数量 | 否 | 有且仅有人像-多人下返回 |
| ocrText | json_object | 返回图片中违规图片文字相关信息 | 否 | |
| riskSource | int | 标识资源哪里违规 | 是 | 标识风险结果的来源<br/>`1001`：文字风险<br/>`1002`：视觉图片风险<br/>`1004`：多模态风险<br/> |

riskDetail中，faces数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| id | string | 人物编号 | 否 | 图片同一个位置下的人在不同标签下的编号相同。<br/>如果同一个人在图片中出现n次，分配n个ID |
| name | string | 人物名称 | 否 | 能识别的公众人物名称 |
| location | int_array | 人物位置信息，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否 |  |
| face_ratio | float | 人脸占比 | 否 | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | 否 | 0～1之间的浮点数 |

riskDetail中，persons数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| id | string | 编号，保证同一个人在不同标签下的编号相同。如果同一个人在图片中出现n次，分配n个ID | 否 |  |
| person_ratio | string | 人像在图中的占比 | 否 | |
| location | int_array | 人像位置坐标 | 否 | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | 否 | 0～1之间的浮点数 |

riskDetail中，ocrText的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| text | string | 识别出的文字 | 是 | |
| matchedLists | json\_array | 命中的客户自定义名单列表 | 否 | |
| riskSegments | json\_array | 高风险片段内容，检测图片包含涉政、暴恐、违禁、广告法等风险内容的时候存在 | 否 |  |

matchedLists数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 命中的名单名称 | 否 | |
| words | json\_array | 命中的敏感词信息 | 否 | |

matchedLists中，words数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string | 命中的敏感词 | 否 | |
| position | int_array | 敏感词所在位置 | 否 | |

riskSegments的每个元素的详细内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segment | string | 高风险内容片段 | 否 | |
| position | int_array | 高风险内容片段所在位置 | 否 | |

其中allLabels数组的每个成员的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险原因 | 是 | |
| riskLevel | string | 处置建议 | **是** | |
| probability | float | 置信度，可选值在0～1之间，值越大，风险可能性越高，值越小，无风险可能性越高 | 否 |  |
| riskDetail | json_object | 风险详情，字段内容见result下的riskDetail | 否 |  |



<span id="businessLabels">其中businessLabels数组的每个成员的内容如下：</span>

| **返回结果参数名**  | **参数类型** | **参数说明**                                      | **是否必返** | **规范**                       |
| ------------------- | ------------ | ------------------------------------------------- | ------------ | ------------------------------ |
| businessLabel1      | string       | 一级业务标签                                      | 是           | 一级业务标签                   |
| businessLabel2      | string       | 二级业务标签                                      | 是           | 二级业务标签                   |
| businessLabel3      | string       | 三级业务标签                                      | 是           | 三级业务标签                   |
| businessDescription | string       | 业务标签描述                                      | 是           | 中文标签描述                   |
| businessDetail      | json_object  | 业务标签详情                                      | 否           | 格式详见下方businessDetail结构 |
| probability         | float        | 置信度<br/>可选值在0～1之间，值越大，可信度越高   | 是           |                                |
| confidenceLevel     | int          | 置信等级<br/>可选值在0～2之间，值越大，可信度越高 | 否           |                                |

businessLabels数组中的businessDetail的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明**                                                 | **是否必返** | **规范**               |
| ------------------ | ------------ | ------------------------------------------------------------ | ------------ | ---------------------- |
| name               | string       | 明星人物名称<br/>图片中的明星人名type传值包含`FACE`时存在    | 否           |                        |
| probability        | float        | 明星人物置信区间<br/>可选值在0～1之间，值越大，可信度越高，当且仅当name存在时出现 | 否           |                        |
| face_ratio         | float        | 人脸占比<br/>在区间0-1，数值越大，人脸占比越高type传值包含`FACE`时存在 | 否           |                        |
| faces              | json_array   | 内容与外层riskDetail.faces格式一致，内部字段参考外层riskDetail下的faces字段 | 否           |                        |
| objects            | json_array   | 其他情况下，仅有一个数组元素标识信息，返回图片中标识或物品的名称及位置信息，内容与外层riskDetail.objects格式一致 |              | 数组仅会有一个元素     |
| persons            | json_array   | 仅当命中人像-多人时，数组元素会有多个，最多10（如果<br/>超过10个，选择probability最高的10个），其他情况下，<br/>仅有一个元素，内部字段参考外层riskDetail下的persons字段 |              |                        |
| face_num           | int          | 其他情况下，仅有一个数组元素人脸数检测<br/>图片中检测到的人脸个数<br/>仅当命中人脸-人脸类型-多人脸时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个） | 否           |                        |
| face_compare_num   | int          | 人脸比对人脸数检测<br/>图片中检测到的人脸个数，businessType传值包含`FACECOMPARE`时存在 | 否           |                        |
| location           | int_array    | 标识位置信息<br/>type传值包含`OBJECT`且时存在，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否           |                        |
| person_num         | int          | 人体数量检测                                                 | 否           | 仅当命中多人标签时返回 |
| person_ratio       | float        | 人像占比<br/>在区间0-1，数值越大，人脸占比越高               |              |                        |



### 同步请求示例：

```json
{
  "accessKey": "xxxxxxxx",
  "appId": "xxx",
  "eventId": "xxx",
  "type": "EROTIC",
  "data":{
      "tokenId": "tokenId001",
      "sessionId": "sessionId001",
      "role": "user",
      "content": [
                {"image": "https://vi2.6rooms.com/live/2024/11/28/07/1004v1732750690826352234.jpg"},
       					{"text": "xxxxxxx"}
       ]
  }
}
```

### 同步返回示例：

```json
{
    "requestId": "1264bbbc8d9dd29983c47547657eb936",
    "code": 1100,
    "message": "成功",
    "riskLevel": "REJECT",
    "riskLabel1": "porn",
    "riskLabel2": "sm",
    "riskLabel3": "sm",
    "riskDescription": "色情:SM:SM",
    "riskDetail": {
        "riskSource": 1001
    },
    "allLabels": [
        {
            "riskLabel1": "porn",
            "riskLabel2": "sm",
            "riskLabel3": "sm",
            "riskLevel": "REJECT",
            "riskDescription": "色情:SM:SM",
            "probability": 1,
            "riskDetail": {
                "riskSource": 1001
            }
        }
    ]
}
```



# 附录


| 业务标签识别类型      | 类型说明                        | 备注                                       |
| --------------------- | ------------------------------- | ------------------------------------------ |
| TOPIC                 | 主题                            |                                            |
| AGE                   | 人脸 - 年龄                     | 可识别未成年人                             |
| GENDER                | 人脸 -性别                      |                                            |
| BEAUTY                | 人脸 - 颜值                     |                                            |
| RACE                  | 人脸 - 人种                     | 如黑种人、白种人、黄种人                   |
| FACEDETECTION         | 人脸-人脸检测                   | 如识别无人脸、真人、口罩人脸、正脸、侧脸等 |
| FAKEFACE              | 人脸 - 伪造人脸                 |                                            |
| FACECOMPARE           | 人脸-人脸对比                   |                                            |
| PUBLICFIGURE          | 人物  - 公众人物                | 如识别知名明星、网红等                     |
| TAINTEDSTAR           | 人物 - 劣迹人物                 |                                            |
| POSTURE               | 人像-人像姿态                   | 如识别坐姿、跪姿等                         |
| DRESS                 | 人像 - 人像穿着                 | 如识别jk、汉服等                           |
| TEMPERAMENT           | 人像 - 人像气质                 | 如成熟大叔、靓丽女神等                     |
| BODY                  | 人体                            | 如识别头发、眼睛、鼻子等                   |
| PICTUREFORM           | 画面属性 - 画面类型             | 如识别动漫、表情包等                       |
| PICTURESTRUCT         | 画面属性-画面结构               | 如识别宫格图、桥段图等                     |
| LOWVISION             | 画面属性  - 画面低质            | 如识别模糊、涂抹、马赛克等                 |
| LOWCONTNET            | 画面属性 - 内容低质             | 如识别点线密集、虫类密集等                 |
| LIVEPICTURE           | 画面属性-直播画面               | 如识别床上直播、开车直播等                 |
| SCREENSHOT            | 画面属性 -  APP截图（内容搬运） | 如识别朋友圈截图、聊天截图等               |
| FITNESS               | 场景主题-健身                   |                                            |
| CATE                  | 场景主题-美食                   |                                            |
| MUSIC                 | 场景主题-音乐                   |                                            |
| SPORTS                | 场景主题-体育                   |                                            |
| SCENERY               | 场景主题-自然风光               | 如识别天空、大海、草原等                   |
| CITYVIEW              | 场景主题-城市风光               | 如识别街景                                 |
| 3CPRODUCTSLOGO        | LOGO - 3C电子类品牌             | 如识别华为、小米、OPPO等LOGO               |
| SHOPPINGAPPSLOGO      | LOGO - 购物比价类应用           | 如识别拼多多等LOGO                         |
| RETOUCHAPPSLOGO       | LOGO - 拍摄美化类应用           | 如识别快剪辑、秒拍等LOGO                   |
| SOCIALAPPSLOGO        | LOGO - 社交通讯类应用           | 如识别微博、小红书等LOGO                   |
| PHOTOMATERIALLOGO     | LOGO - 素材版权类应用           | 如识别CFP等LOGO                            |
| NEWSAPPSLOGO          | LOGO - 新闻阅读类应用           | 如识别新浪、视觉中国等LOGO                 |
| ENTERTAINMENTAPPSLOGO | LOGO - 影音娱乐类应用           | 如识别抖音、快手等LOGO                     |
| SPORTSLOGO            | LOGO  - 体育赛事                | 如识别奥运会等LOGO                         |
| APPARELLOGO           | LOGO - 鞋帽服饰类品牌           | 如识别VANS、H&M等LOGO                      |
| ACCESSORIESLOGO       | LOGO - 饰品首饰类品牌           | 如识别AudemarsPiguet、Nomos等LOGO          |
| COSMETICSLOGO         | LOGO - 化妆品类品牌             | 如识别LOTTE、EyesLipsFace等LOGO            |
| FOODLOGO              | LOGO - 食品类品牌               | 如识别Starbucks、LOTTE等LOGO               |
| AUTOTRADEAPPSLOGO     | LOGO - 汽车交易平台类           | 如识别懂车帝、易车、太平洋汽车、爱卡等LOGO |
| VEHICLE               | 物品-交通工具                   |                                            |
| BUILDING              | 物品-建筑                       |                                            |
| TABLEWARE             | 物品-餐具                       |                                            |
| FOOD                  | 物品-食物                       |                                            |
| HOMEAPPLICATION       | 物品-家用电器                   |                                            |
| OFFICESUPPLIES        | 物品-办公用品                   |                                            |
| FASHION               | 物品-穿着用品                   |                                            |
| SPORTEQUIPMENT        | 物品-运动器材                   |                                            |
| TOY                   | 物品-玩具                       |                                            |
| MAKEUP                | 物品-化妆品                     |                                            |
| DRUGS                 | 物品-药品                       |                                            |
| PAINTING              | 物品-绘画作品                   |                                            |
| ELECTRONIC            | 物品-电子产品                   |                                            |
| MEDICALIMAGE          | 物品-医疗影像                   |                                            |
| FURNITURE             | 物品-家居用品                   |                                            |
| DAILYSUPPLIES         | 物品-生活用品                   |                                            |
| CONSTELLATION         | 物品-星座占卜                   |                                            |
| KITCHENWARE           | 物品-厨房用品                   |                                            |
| KEEPSAKE              | 物品 - 纪念品                   |                                            |
| MAMMAL                | 动物-哺乳动物                   |                                            |
| BIRDS                 | 动物 - 鸟类                     |                                            |
| REPTILE               | 动物-爬行动物                   |                                            |
| FISH                  | 动物-鱼                         |                                            |
| ARTHROPOD             | 动物  - 节肢动物                |                                            |
| COELENTERATE          | 动物  - 腔肠动物                |                                            |
| MOLLUSKS              | 动物  - 软体动物                |                                            |
| CRUSTACEAN            | 动物  - 甲壳动物                |                                            |
| PLANT                 | 植物                            |                                            |
| SETTING               | 场所                            | 如识别卫生间、酒店、厨房等                 |
| EXTREMEWEATHER        | 极端天气识别                    | 如水灾、暴雨、沙尘暴、冰等                 |
| LICENCEPLATE          | 车牌识别                        |                                            |
