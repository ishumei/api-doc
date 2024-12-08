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
目前数美侧下载图片时的连接超时时间是2s，读取超时时间是3s，内部平均处理时间在500ms左右（具体时长和请求type，图片大小相关），下载失败会重试一次，建议客户设置超时时间为7-10s

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥<br/>用于权限认证，开通账号服务时由数美提供或使用开通邮箱登录数美后台右上角相关文档处查看 | 必传参数 |  |
| appId | string | 应用标识，用于区分相同公司的不同应用数据 | 必传参数 | 需要联系数美开通，请以数美单独提供的传值为准 |
| eventId | string | 事件标识 | 必传参数 | 需要联系数美服务开通，请使用数美单独提供的传值为准 |
| type | string | 检测的风险类型 | 非必传参数 | 监管一级标签 可选值:<br/>POLITY :涉政识别<br/>EROTIC :色情&性感违规识别 <br/>VIOLENT :暴恐&违禁识别 <br/>QRCODE :二维码识别<br/>ADVERT :广告识别<br/>IMGTEXTRISK :图片文字违规识别<br/>如果需要识别多个功能，通过下划线连接，如 POLITY_QRCODE_ADVERT 用于涉政、二维码和广告组合识别<br/>（该字段与businessType字段必须选择一个传入）<br/>涉政、色情、暴恐只包含了图片本身的违规检测，如需要识别图片里文字的违规内容，务必传入图片文字违规识别功能 |
| data | json_object | 请求的数据内容 | 必传参数 | 请求的数据内容，data字段长度最长10MB，[详见data参数](#data) |

<span id="data">其中，data的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| tokenId | string | 用户账号标识 | 非必传参数 | 由数字、字母、下划线、短杠组成的长度小于等于32位的字符串 |
| sessionId | string | 会话标识 | 必传参数 | 由数字、字母、下划线、短杠组成的长度小于等于32位的字符串 |
| role | string | 用户角色 | 必传参数 | 用户角色<br/>枚举值：<br/>user：房管<br/>assistant：机器人<br/> |
| content | array | 检测内容 | 必传参数 | 包含顺序关系的图文对象，最多传入图文各一个 |

<span id="content">其中，content的内容如下：</span>

image对象

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范**                               |
| -------------- | -------- | ------------ | ------------ | -------------------------------------- |
| image          | string   | 图片地址     | 非必传参数   | image请求格式支持：PNG、JPG、JPEG、BMP |

text对象

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范**              |
| -------------- | -------- | ------------ | ------------ | --------------------- |
| text           | string   | 文本内容     | 非必传参数   | 文本内容小于512个字符 |





## 同步返回结果

<span id="singleRet">放在HTTP Body中，采用Json格式，具体参数如下：</span>

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 |  `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/> |
| message | string | 返回码描述 | 是 | 和code对应：成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>图片下载失败<br/>无权限操作 |
| requestId | string | 请求标识 | 是 | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存 |
| riskLevel | string | 处置建议 | 是 | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| riskLabel1 | string | 一级风险标签 | 是 | 当riskLevel为`PASS`时返回`normal` |
| riskLabel2 | string | 二级风险标签 | 是 | 当riskLevel为`PASS`时为空 |
| riskLabel3 | string | 三级风险标签 | 是 | 当riskLevel为`PASS`时为空 |
| riskDescription | string | 风险原因 | 是 | 当riskLevel为`PASS`时为`正常` |
| riskDetail | json_object | 风险详情 | 是 | [详见riskDetail参数](#riskDetail) |
| allLabels | json_array | 风险标签详情 | 是 | 返回命中的所有风险标签以及详情信息 |



<span id="riskDetail">其中，riskDetail结构如下：</span>

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| faces | json_array | 返回图片中涉政人物的名称及位置信息 | 否 |  |
| face_num | int | 人脸数量 | 否 |  |
| persons | json_array | 仅当命中人像-多人时，数组元素会有多个，最多10个 |否  | |
| person_num | int | 人像数量 | 否 | 有且仅有人像-多人下返回 |
| objects | json_array | 返回图片中物品或标志二维码的位置信息 | 否 | 数组仅会有一个元素 |
| ocrText | json_object | 返回图片中违规文字相关信息，当请求参数type字段包含`IMGTEXTRISK`和ADVERT时存在 | 否 | |
| riskSource | int | 标识资源哪里违规 | 是 | 标识风险结果的来源<br/>`1000`：无风险<br/>`1001`：文字风险<br/>`1002`：视觉图片风险<br/>`1004`：多模态<br/> |

标识风险结果的来源<br/>`1000`：无风险<br/>`1001`：文字风险<br/>`1002`：视觉图片风险<br/>riskDetail中，faces数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| id | string | 人物编号 |  | 图片同一个位置下的人在不同标签下的编号相同。<br/>如果同一个人在图片中出现n次，分配n个ID |
| name | string | 人物名称 | 否 | 能识别的公众人物名称 |
| location | int_array | 人物位置信息，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否 |  |
| face_ratio | float | 人脸占比 | 否 | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | 否 | 0～1之间的浮点数 |

riskDetail中，objects数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| id | string | 编号，保证同一个位置下的物品在不同标签下的编号相同 | 否 |  |
| name | string | 标识名称 | 否 | |
| location | int_array | 标识位置信息，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标  | 否 | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | 否 | 0～1之间的浮点数 |
| qrContent | string | 二维码的url信息 | 否 | 仅当命中二维码相关标签时返回 |

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

ocrText中，matchedLists数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 命中的名单名称 | 否 | |
| words | json\_array | 命中的敏感词信息 | 否 | |

matchedLists中，words数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| word | string | 命中的敏感词 | 否 | |
| position | int_array | 敏感词所在位置 | 否 | |

ocrText中，riskSegments的每个元素的详细内容如下：

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





### 同步请求示例：

```json
{
  "accessKey": "xxxxxxxx",
  "appId": "xxx",
  "eventId": "xxx",
  "type": "EROTIC",
  "data":{
      "tokenId": "MLing344434",
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

