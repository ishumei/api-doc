# 数美智能图片识别产品API接口文档


# 同步单张上传接口

## 同步单条请求

### 请求URL：
| 集群 | URL |
| --- | --- | 
| 北京 | `http://api-img-bj.fengkongcloud.com/image/v4` | 
| 上海 | `http://api-img-sh.fengkongcloud.com/image/v4` | 
| 新加坡 | `http://api-img-xjp.fengkongcloud.com/image/v4` | 
| 印度 | `http://api-img-yd.fengkongcloud.com/image/v4` | 
| 硅谷 | `http://api-img-gg.fengkongcloud.com/image/v4` | 

### 请求方法：

`POST`

### 字符编码：

`UTF-8`

### 建议超时时间：

5s

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥<br/>用于权限认证，开通账号服务时由数美提供或使用开通邮箱登录数美后台右上角相关文档处查看 | 必传参数 |  |
| appId | string | 应用标识，用于区分相同公司的不同应用数据 | 必传参数 | 需要联系数美开通，请以数美单独提供的传值为准 |
| eventId | string | 事件标识 | 必传参数 | 需要联系数美服务开通，请使用数美单独提供的传值为准 |
| type | string | 检测的风险类型 | 非必传参数 | 监管一级标签 可选值:<br/>POLITY :涉政识别<br/>EROTIC :色情&性感违规识别 <br/>VIOLENT :暴恐&违禁识别 <br/>QRCODE :二维码识别<br/>ADVERT :广告识别<br/>IMGTEXTRISK :图片文字违规识别<br/>如果需要识别多个功能，通过下划线连接，如 POLITY_QRCODE_ADVERT 用于涉政、二维码和广告组合识别<br/>（该字段与businessType字段必须选择一个传入）<br/>涉政、色情、暴恐只包含了图片本身的违规检测，如需要识别图片里文字的违规内容，务必传入图片文字违规识别功能 |
| businessType | string | 业务标签类型 | 非必传参数 | 业务标签<br/>可选值：[见附录](#附录)如果需要多个识别功能，通过下划线连接，该字段和type必须选择一个传入 |
| data | json_object | 请求的数据内容 | 必传参数 | 请求的数据内容，data字段长度最长10MB，[详见data参数](#data) |
| callback | string | 回调请求url，传callback表示走异步回调逻辑，否则走同步逻辑，回调http地址字段，当该字段非空时，服务将根据该字段回调通知用户审核结果，地址必须为http或https的规范的url | 非必传参数 | 异步回调逻辑支持30M图片<br/>同步支持10M图片<br/>异步单张和异步批量都是需要调用查询接口来查结果的； 同步的接口不能调用查询，如果传callback是将结果回调给对应的服务器，如果没有传callback就是走同步返回 |

<span id="data">其中，data的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| tokenId | string | 用户账号标识，建议使用贵司用户UID（可加密）自行生成 , 标识用户唯一身份用作灌水和广告等行为维度风控。如无用户uid的场景建议使用唯一的数据标识传值 | 必传参数 | 由数字、字母、下划线、短杠组成的长度小于等于64位的字符串 |
| img | string | 要检测的图片，可使用base64编码的图片数据或者图片的url链接 **建议图片下载从CDN源站下载，并且源站不能为单点<br/>风险：如果不是从源站下载，可能存在图片下载失败，导致无法审核** | 必传参数 | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>建议图片像素不小于256\*256, 目前最低支持20\*20分辨率的图片，图片大小最大10MB，异步最大30M |
| imgCompareBase | string | 要检测比对的基准图片，请求参数Type字段包含标签`FACECOMPARE`时存在<br/>可使用base64编码的图片数据或者图片的url链接 | 非必传参数 | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>建议图片像素不小于256\*256，图片分辨率限制在6000\*6000以内，图片大小最大10MB<br/><br/>基准图暂时不支持长图和动图格式 |
| role | string | 用户角色 | 非必传参数 | 用户角色，必须在可选范围有效对不同角色可配置不同策略。(默认为`USER`)<br/>直播领域可取值：<br/>`ADMIN`：房管<br/>`HOST`：主播<br/>`SYSTEM`：系统角色<br/>游戏领域可取值：<br/>`ADMIN`：管理员<br/>`USER`：普通用户 |
| ip | string | ip地址 | 非必传参数 | 发送该图片的用户公网ipv4地址 |
| lang | string | 语言类型 | 非必传参数 | 请求type中包含 IMGTEXTRISK 时，可指定对应检测语种类型，可选值：<br/>zh：中⽂<br/>en：英语<br/>ar：阿拉伯语<br/>默认使⽤中⽂检测，请注意传⼊可选值之外的标识时⽆效 |
| deviceId | string | 数美设备指纹标识 | 非必传参数 | 数美设备指纹生成的设备唯一标识 |
| maxFrame | int | gif图片的最大截帧数量 | 非必传参数 | 截取git等动图帧数，最大为20帧，默认为3帧，计费按照实际截帧数量计费，如默认为截取3帧时按照3帧进行计费 |
| interval | int | gif截帧图片的检测间隔 | 非必传参数 | 默认值为`1`，代表每一帧都需要进行检测，服务会自动调整该值以保证完全覆盖全部帧 |
| extra | json_object | 辅助参数 | 非必传参数 | 用于辅助检测的相关信息，[详见extra参数](#extra) |
| streamInfo | json_object | 相似帧审核参数 | 非必传参数 | 用于检测相似帧的相关信息，[详见streamInfo参数](#streamInfo)，如需要了解或使用相似帧功能，请联系客服咨询 |
| receiveTokenId | string | 接收者的tokenId | 非必传参数 | 接收者的tokenId，私聊场景必选 |

<span id="streamInfo">data中，streamInfo的内容如下：</span>

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| similarDedup | bool | 是否开启相似功能 | 否 |  |
| streamId | string | 透传参数 | 否 | 唯一标识id similarDedup为true时，必传 |
| timeWindow | int | 透传参数 | 否 |时间窗口，单位秒。similarDedup为true时，必传  |
| frameTime | int | 透传参数 | 否 | 截帧时间，单位ms， similarDedup为true时，必传 |
| riskNum | int | 透传参数 | 否 | 相似截帧图片不改变张数    数量范围(1-5)，默认为1 |

<span id="extra">data中，extra的内容如下：</span>

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| isIgnoreTls | bool | 辅助参数，来控制是否要忽略ca证书的验证 | 否 | 可选值（默认为`false`）：<br/>`true`：忽略证书信任<br/>`false`：校验证书 |
| passThrough | json_object | 透传参数 | 否 | 客户传入透传字段，数美内部不回对该字段进行识别处理，随结果返回给用户，必须为json_object类型 |
| isTokenSeparate | int | 是否区分不同应用下的账号 | 否 | 是否区分不同应用下的账号，可能取值：<br/>0:不区分<br/>1:区分<br/>默认值为0 |
| room | string | 直播房间号，高曝光群聊等业务场景建议传入房间号 | 否 |  |
## 同步返回结果

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 |  `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1911`：图片下载失败<br/>`9101`：无权限操作<br/> |
| message | string | 返回码描述 | 是 | 和code对应：成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>图片下载失败<br/>无权限操作 |
| requestId | string | 请求标识 | 是 | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存 |
| riskLevel | string | 处置建议 | 是 | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| riskLabel1 | string | 一级风险标签 | 是 | 当riskLevel为`PASS`时返回`normal` |
| riskLabel2 | string | 二级风险标签 | 是 | 当riskLevel为`PASS`时为空 |
| riskLabel3 | string | 三级风险标签 | 是 | 当riskLevel为`PASS`时为空 |
| riskDescription | string | 风险原因 | 是 | 当riskLevel为`PASS`时为`正常` |
| riskDetail | json_object | 风险详情 | 是 | [详见riskDetail参数](#riskDetail) |
| auxInfo | json_object | 其他辅助信息 | 是 | [详见auxInfo参数](#auxInfo) |
| allLabels | json_array | 风险标签详情 | 是 | 返回命中的所有风险标签以及详情信息 |
| businessLabels | json_array | 业务标签详情 | 否 | 当仅做识别，不需要配置reject、review策略的结果在此返回，[详见businessLabels参数](#businessLabels) |
| tokenProfileLabels | json_array | 辅助信息 | 否 | 属性账号类标签。[详见账号标签参数](#tokenProfileLabels) |
| tokenRiskLabels | json_array | 辅助信息 | 否 | 风险账号类标签。[详见账号标签参数](#tokenProfileLabels) |
| tokenLabels    | json_object  | 账号标签信息 | 否 | 见下面详情内容，仅在tokenId传入且联系数美开通时返回 |

<span id="riskDetail">其中，riskDetail结构如下：</span>

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| faces | json_array | 返回图片中涉政人物的名称及位置信息 | 否 |  |
| face_num | int | 人脸数量 | 否 |  |
| persons | json_array | 仅当命中人像-多人时，数组元素会有多个，最多10个 |  | |
| person_num | int | 人像数量 | 否 | 有且仅有人像-多人下返回 |
| objects | json_array | 返回图片中物品或标志二维码的位置信息 | 否 | 数组仅会有一个元素 |
| ocrText | json_object | 返回图片中违规文字相关信息，当请求参数type字段包含`IMGTEXTRISK`和ADVERT时存在 | 否 | |
| riskSource | int | 标识资源哪里违规 | 是 | 标识风险结果的来源<br/>`1000`：无风险<br/>`1001`：文字风险<br/>`1002`：视觉图片风险 |

riskDetail中，faces数组每个元素的内容如下：

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

<span id="auxInfo">其中，auxInfo的内容如下：</span>

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segments | int | 实际处理的图片数量 | 是 | |
| typeVersion | json_object | 针对各个传入type的效果版本号 | 是 | |
| errorCode | int | | 否 | `2001`：输入数据格式不对，不是合法的json数据<br/>`2002`：输入的参数字段不合法（必填字段缺失、类型不对、值不合法等）<br/>`2003`：图片下载失败<br/>`2004`：图片过大，超过了10M<br/>`2005`：非法图片格式<br/>`2006`：无效风险监测类型 |
| passThrough | json_object | 客户传入的透传字段 | 否 |  |
| streamId | string | 请求参数中传入streamInfo功能时满足相似功能会返回 | 否 |  |
| frameTime | int | 请求参数中传入streamInfo功能时满足相似功能会返回 | 否 |  |
| qrContent | string | 返回识别的二维码地址| 否 | 需要联系数美配置返回 |

auxInfo中，typeVersion的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| POLITY | string | 涉政版本号 | 否 | 组成形式为`X.Y`，`X`为主版本号，一般代表模型整体的效果迭代；`Y`为子版本号，一般代表日常的例行迭代<br/>例如`1001001.2`代表主版本号为`1001001`，子版本号为`2` |
| VIOLENT | string | 暴恐版本号 | 否 | 组成形式同上 |
| EROTIC | string | 色情版本号 | 否 | 组成形式同上 |
| ADVERT | string | 广告版本号 | 否 | 组成形式同上 |
| IMGTEXTRISK | string | 违规文字版本号 | 否 | 组成形式同上 |
| QRCODE | string | 二维码版本号 | 否 | 组成形式同上 |

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

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级业务标签| 是 | 一级业务标签 |
| businessLabel2 | string | 二级业务标签 | 是 | 二级业务标签 |
| businessLabel3 | string | 三级业务标签 | 是 | 三级业务标签 |
| businessDescription | string | 业务标签描述 | 是 | 中文标签描述 |
| businessDetail | json_object | 业务标签详情 | 否 | 格式详见下方businessDetail结构 |
| probability | float | 置信度<br/>可选值在0～1之间，值越大，可信度越高 | 是 | |
| confidenceLevel | int | 置信等级<br/>可选值在0～2之间，值越大，可信度越高 | 否 | |

businessLabels数组中的businessDetail的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 明星人物名称<br/>图片中的明星人名type传值包含`FACE`时存在 | 否 |  |
| probability | float | 明星人物置信区间<br/>可选值在0～1之间，值越大，可信度越高，当且仅当name存在时出现 | 否 |  |
| face_ratio | float | 人脸占比<br/>在区间0-1，数值越大，人脸占比越高type传值包含`FACE`时存在 | 否 |  |
| faces | json_array | 内容与外层riskDetail.faces格式一致，内部字段参考外层riskDetail下的faces字段 | 否 | |
| objects | json_array | 其他情况下，仅有一个数组元素标识信息，返回图片中标识或物品的名称及位置信息，内容与外层riskDetail.objects格式一致 |  | 数组仅会有一个元素 |
| persons | json_array | 仅当命中人像-多人时，数组元素会有多个，最多10（如果<br/>超过10个，选择probability最高的10个），其他情况下，<br/>仅有一个元素，内部字段参考外层riskDetail下的persons字段 |  |  |
| face_num | int | 其他情况下，仅有一个数组元素人脸数检测<br/>图片中检测到的人脸个数<br/>仅当命中人脸-人脸类型-多人脸时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个） | 否 | |
| face_compare_num | int | 人脸比对人脸数检测<br/>图片中检测到的人脸个数，businessType传值包含`FACECOMPARE`时存在 | 否 | |
| location | int_array | 标识位置信息<br/>type传值包含`OBJECT`且时存在，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否 |  |
| person_num | int | 人体数量检测 | 否 | 仅当命中多人标签时返回 |
| person_ratio | float | 人像占比<br/>在区间0-1，数值越大，人脸占比越高 | 否 | |

tokenLabels的详情内容如下：

| ***返回结果参数名*** | ***参数类型*** | ***参数说明***   | 是否必返 | ***规范***                 |
| -------------------- | -------------- | ---------------- | -------- | -------------------------- |
| machine_account_risk | json_object    | 机器控制相关风险 | 否       |                            |
| UGC_account_risk     | json_object    | UGC内容相关风险  | 否       |                            |
| scene_account_risk   | json_object    | 场景账号风险     | 否       | 特殊场景才可取到，如航司等 |

<span id="tokenProfileLabels">其中，tokenProfileLabels、tokenRiskLabels的内容如下：</span>

| 参数名称    | 类型   | 参数说明     | 是否必返 | 规范                       |
| ----------- | ------ | ------------ | -------- | -------------------------- |
| label1      | string | 一级标签     | 否       |                            |
| label2      | string | 二级标签     | 否       |                            |
| label3      | string | 三级标签     | 否       |                            |
| description | string | 标签描述     | 否       |                            |
| timestamp   | Int    | 打标签时间戳 | 否       | 13位Unix时间戳，单位：毫秒 |

machine_account_risk的详情内容如下：

| ***返回结果参数名***              | ***参数类型*** | ***参数说明*** | 是否必返 | ***规范***                       |
| --------------------------------- | -------------- | -------------- | -------- | -------------------------------- |
| b_machine_control_tokenid         | int            | 机器账号       | 否       | 0：非机器控制账号1：机器控制账号 |
| b_machine_control_tokenid_last_ts | int            | 机器账号时间   | 否       |                                  |
| b_offer_wall_tokenid              | int            | 积分墙账号     | 否       | 0：非积分墙账号1：积分墙账号     |
| b_offer_wall_tokenid_last_ts      | int            | 积分墙账号时间 | 否       |                                  |

UGC_account_risk的详情内容如下：

| ***返回结果参数名***             | ***参数类型*** | ***参数说明*** | 是否必返 | ***规范***                         |
| -------------------------------- | -------------- | -------------- | -------- | ---------------------------------- |
| b_politics_risk_tokenid          | int            | 涉政风险       | 否       | 0：暂未发现涉政风险1：存在涉政风险 |
| b_politics_risk_tokenid_last_ts  | int            | 涉政风险时间   | 否       |                                    |
| b_sexy_risk_tokenid              | int            | 色情风险       | 否       | 0：暂未发现色情风险1：存在色情风险 |
| b_sexy_risk_tokenid_last_ts      | int            | 色情风险时间   | 否       |                                    |
| b_advertise_risk_tokenid         | int            | 广告风险       | 否       | 0：暂未发现广告风险1：存在广告风险 |
| b_advertise_risk_tokenid_last_ts | int            | 广告风险时间   | 否       |                                    |

scene_account_risk的详情内容如下：

| ***返回结果参数名***    | ***参数类型*** | ***参数说明*** | 是否必返 | ***规范***                   |
| --------------------------- | ------------------ | ------------------ | -------------------------------- | -------------------------------- |
| i_tout_risk_tokenid         | int                | 航司占座账号       | 否      | 0：非航司占座账号1：航司占座账号 |
| i_tout_risk_tokenid_last_ts | int                | 航司占座时间       | 否      |                                  |

## 回调的同步返回参数

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9101`：无权限操作 |
| message | string | 返回码描述 | 是 | 和code对应：成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>无权限操作 |
| requestId | string | 请求标识 | 是 | 图片的流水号 |

如果在请求参数中指定了回调协议接口URL callback，则需要支持POST方法，传输编码采用utf-8，审核结果放在HTTP Body中，采用Json格式，具体参数和V4单张同步请求结果相同。

## 同步单张示例：

### 同步请求示例：

```json
{
    "accessKey":"",
    "appId":"",
    "eventId":"",
    "type":"",
    "data":{
        "tokenId":"username123",
        "img":""
    }
}
```

### 同步返回示例：

```json
{
    "requestId":"55cf0374642c5f2336ccb107aa9005e5",
    "code":1100,
    "message":"成功",
    "riskLevel":"REJECT",
    "riskLabel1":"politics",
    "riskLabel2":"yihaolingdao",
    "riskLabel3":"yihaolingdao",
    "riskDescription":"涉政:一号领导:一号领导",
    "riskDetail":{
        "faces":[
            {
                "face_ratio":0.00357499998062849,
                "id":"be82442eaf2fe5fcaba84e7f2b3b1dbc",
                "location":[
                    403,
                    171,
                    436,
                    210
                ],
                "name":"习近平",
                "probability":0.803125739097595
            }
        ],
        "riskSource":1002
    },
    "auxInfo":{
        "segments":1,
        "typeVersion":{
            "POLITICS":"2014014.1",
            "VIOLENCE":"2012008.1",
            "BAN":"1002102.1",
            "PORN":"3048002.1"
        }
    },
    "allLabels":[
       {
            "riskLabel1":"politics",
            "riskLabel2":"lingdaoren",
            "riskLabel3":"lingdaorenguaxiang",
            "riskLevel":"REVIEW"
        },
        {
            "probability":0.867621600627899,
            "riskDescription":"涉政:涉政:涉政",
            "riskDetail":{
                "riskSocrce":1002
            },
            "riskLabel1":"politics",
            "riskLabel2":"shezheng",
            "riskLabel3":"shezheng",
            "riskLevel":"REJECT"
        },
        {
            "probability":0.855094926869849,
            "riskDescription":"涉政:一号领导:一号领导",
            "riskDetail":{
                "faces":[
                    {
                        "face_ratio":0.00357499998062849,
                        "id":"be82442eaf2fe5fcaba84e7f2b3b1dbc",
                        "location":[
                            403,
                            171,
                            436,
                            210
                        ],
                        "name":"习近平",
                        "probability":0.803125739097595
                    }
                ],
                "riskSocrce":1002
            },
            "riskLabel1":"politics",
            "riskLabel2":"yihaolingdao",
            "riskLabel3":"yihaolingdao",
            "riskLevel":"REJECT"
        }
    ],
    "businessLabels":[
        {
            "businessDescription":"人脸:人脸姿态:正脸",
            "businessDetail":{

            },
            "businessLabel1":"face",
            "businessLabel2":"renlianzitai",
            "businessLabel3":"zhenglian",
            "confidenceLevel":1,
            "probability":0.450656906102068
        },
        {
            "businessDescription":"人脸:人脸类型:多人脸",
            "businessDetail":{

            },
            "businessLabel1":"face",
            "businessLabel2":"renlianleixing",
            "businessLabel3":"duorenlian",
            "confidenceLevel":1,
            "probability":0.458568899401581
        },
        {
            "businessDescription":"人脸:人脸类型:真人",
            "businessDetail":{

            },
            "businessLabel1":"face",
            "businessLabel2":"renlianleixing",
            "businessLabel3":"zhenren",
            "confidenceLevel":2,
            "probability":0.867621600627899
        }
    ],
    "tokenLabels":{
        "UGC_account_risk":{

        }
    }
}
```

### 回调的同步返回参数
```json
{
    "code":1100,
    "message":"成功",
    "requestId":"69dbc1f81dc5c914b1f1b8a267fb9ec1"
}
```
当用户的服务端收到推送结果，并返回HTTP状态码为200时，表示推送成功，否则系统将进行重试推送（直至达到重试次数上限）重试逻辑为间隔[1, 2, 3, 4, 5, 6, 7, 8]秒后重试，8次之后依然失败则不在重试。

### 回调请求示例

```json
{
    "checksum":"236f8eea85c3c4407d96ff05d6108389b3b0cea8aa80bdf6642c1cecc77b2bde",
    "result":{
    "code":1100,
    "message":"成功",
    "requestId":"1e6e4e43cd35b545418fcef7d0f77ef4",
    "score":999,
    "riskLevel":"REJECT",
    "detail":{
        "description":"涉政文字",
        "matchedItem":"xxx",
        "matchedList":"test",
        "model":"M02601",
        "polityName":"xxx",
        "riskType":100,
        "riskSource":1002
    },
    "status":0
    }
}
```


# 异步单张上传接口

## 异步单条请求

### 请求URL：
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-img-bj.fengkongcloud.com/v4/saas/async/img` | 图片 |
| 上海 | `http://api-img-sh.fengkongcloud.com/v4/saas/async/img` | 图片 |

### 请求方法：

`POST`

### 字符编码：

`UTF-8`

### 建议超时时间：

5s

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥 | 必传参数 | 公司密钥：用于权限认证，开通账号服务时由数美提供或使用开通邮箱登录数美后台右上角相关文档处查看 |
| appId | string | 应用标识 | 必传参数 | 应用标识：用于区分相同公司的不同应用数据，需要联系数美开通，请以数美单独提供的传值为准 |
| eventId | string | 事件标识 | 必传参数 | 需要联系数美服务开通，请使用数美单独提供的传值为准 |
| type | string | 检测的风险类型 | 必传参数 | 监管一级标签 可选值:<br/>POLITY :涉政识别<br/>EROTIC :色情&性感违规识别 <br/>VIOLENT :暴恐&违禁识别 <br/>QRCODE :二维码识别<br/>ADVERT :广告识别<br/>IMGTEXTRISK :图片文字违规识别<br/>如果需要识别多个功能，通过下划线连接，如 POLITY_QRCODE_ADVERT 用于涉政、二维码和广告组合识别（该字段与businessType字段必须选择一个传入） |
| businessType | string | 业务标签类型 | 非必传参数 | 业务标签识别类型，可选值：[见附录](#附录) 如果需要多个识别功能，通过下划线连接 |
| data | json_object | 请求的数据内容 | 必传参数 | 请求的数据内容，最长10MB |

其中，data的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| tokenId | string | 用户账号标识 | 必传参数 | 建议使用贵司用户UID（可加密）自行生成 , 标识用户唯一身份用作灌水和广告等行为维度风控。如无用户uid的场景建议使用唯一的数据标识传值 |
| img            | string       | 要检测的图片，可使用base64编码的图片数据或者图片的url链接 建议图片下载从CDN源站下载，并且源站不能为单点<br/>风险：如果不是从源站下载，可能存在图片下载失败，导致无法审核 | 必传参数     | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>建议图片像素不小于256\*256, 目前最低支持20\*20分辨率的图片，图片大小最大10MB |
| imgCompareBase | string | 要检测比对的基准图片，请求参数Type字段包含标签`FACECOMPARE`时存在<br/>可使用base64编码的图片数据或者图片的url链接 | 非必传参数 | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>建议图片像素不小于256\*256，图片分辨率限制在6000\*6000以内，图片大小最大10MB<br/><br/>基准图暂时不支持长图和动图格式 |
| role | string | 用户角色 | 非必传参数 | 用户角色，必须在可选范围有效对不同角色可配置不同策略。(默认为`USER`)<br/>直播领域可取值：<br/>`ADMIN`：房管<br/>`HOST`：主播<br/>`SYSTEM`：系统角色<br/>游戏领域可取值：<br/>`ADMIN`：管理员<br/>`USER`：普通用户 |
| ip | string | ip地址 | 非必传参数 | 发送该图片的用户公网ipv4地址 |
| deviceId | string | 数美设备指纹标识 | 非必传参数 | 数美设备指纹生成的设备唯一标识 |
| maxFrame | int | gif图片的最大截帧数量 | 非必传参数 | 截取git等动图帧数，最大为20帧，默认为3帧，计费按照实际截帧数量计费，如默认为截取3帧时按照3帧进行计费 |
| interval | int | gif图片的截帧间隔 | 非必传参数 | 默认值为1，代表每一帧都需要进行检测，服务会自动调整该值以保证完全覆盖全部帧 |
| extra | json\_object | 辅助参数 | 非必传参数 | 用于辅助检测的相关信息 |
| receiveTokenId | string | 接收者的tokenId | 非必传参数 | 接收者的tokenId，私聊场景必选 |

其中，extra内容如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| isIgnoreTls | bool | 辅助参数 | 否 | 可选值（默认为`false`）：<br/>`true`：忽略证书信任<br/>`false`：校验证书 |
| passThrough | json_object | 透传参数 | 否 | 客户传入透传字段，数美内部不回对该字段进行识别处理，随结果返回给用户，必须为json_object类型 |
| room | string | 直播房间号 | 否 | |
## 返回结果

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1911`：图片下载失败<br/>`9101`：无权限操作<br/> |
| message | string | 返回码描述 | 是 | 和code对应：成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>无权限操作 |
| requestId | string | 请求标识 | 是 | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存 |

## 异步单条示例

### 异步单条请求示例
```json
{
    "accessKey":"",
    "eventId":"",
    "type":"",
    "data":{
        "tokenId":"username123",
        "img":""
    }
}
```

### 异步单条返回示例
```json
{
    "requestId":"9l25odfa5280c50f49f7c40988a1e400",
    "code":1100,
    "message":"成功"
}
```

# 同步批量接口

## 同步批量请求参数

### 请求URL：

| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-img-bj.fengkongcloud.com/images/v4` | 图片 |
| 上海 | `http://api-img-sh.fengkongcloud.com/images/v4` | 图片 |
| 新加坡 | `http://api-img-xjp.fengkongcloud.com/images/v4` | 图片 |
| 印度 | `http://api-img-yd.fengkongcloud.com/images/v4` | 图片 |
| 硅谷 | `http://api-img-gg.fengkongcloud.com/images/v4` | 图片 |
### 请求方法：

`POST`

### 字符编码：

`UTF-8`

### 建议超时时间：

60s

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥 | 必传参数 | 由数美提供 |
| appId | string | 应用标识 | 必传参数 | 用于区分相同公司的不同应用数据，需要联系数美开通，请以数美单独提供的传值为准 |
| eventId | string | 事件标识 | 必传参数 | 需要联系数美服务开通，请使用数美单独提供的传值为准 |
| type | string | 检测的风险类型 | 必传参数 | 监管一级标签 可选值:<br/>POLITY :涉政识别<br/>EROTIC :色情&性感违规识别 <br/>VIOLENT :暴恐&违禁识别 <br/>QRCODE :二维码识别<br/>ADVERT :广告识别<br/>IMGTEXTRISK :图片文字违规识别<br/>如果需要识别多个功能，通过下划线连接，如 POLITY_QRCODE_ADVERT 用于涉政、二维码和广告组合识别（该字段与businessType字段必须选择一个传入） |
| businessType | string | 业务标签类型 | 非必传参数 | 业务标签识别类型，可选值：[见附录](#附录) 如果需要多个识别功能，通过下划线连接，该字段和type必须选择一个传入 |
| data | json_object | 请求数据内容 | 必传参数 | 请求的数据内容，最长10MB |
| callback | string | 回调请求url | 非必传参数 | 传callback表示走异步回调逻辑，异步回调逻辑支持30M图片；否则走同步逻辑，同步支持10M图片。<br/>异步单张和异步批量都是需要调用查询接口来查结果的； 同步的接口不能调用查询，如果传callback是将结果回调给对应的服务器，如果没有传callback就是走同步返回 |

其中，data的内容如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| imgs | json_array | 要检测的图片数组 | 必传参数 | |
| tokenId | string | 用户账号标识 | 必传参数 | 用于区分用户账号，建议传入用户ID |
| ip | string | ipv4地址 | 非必传参数 | 发送该图片的用户公网ipv4地址 |
| deviceId | string | 数美设备指纹标识 | 非必传参数 | 数美设备指纹生成的设备唯一标识 |
| maxFrame | int | gif图片的最大截帧数量 | 非必传参数 | 截取git等动图帧数，最大为20帧，默认为3帧，计费按照实际截帧数量计费，如默认为截取3帧时按照3帧进行计费 |
| interval | int | gif图片的截帧间隔 | 非必传参数 | 默认值为1，代表每一帧都需要进行检测，服务会自动调整该值以保证完全覆盖全部帧 |
| room | string | 直播房间号 | 非必传参数 |  |
| extra | json_object | 辅助参数 | 非必传参数 | 用于辅助文本检测的相关信息 |
| role | string | 用户角色 | 非必传参数 | 用户角色，必须在可选范围有效对不同角色可配置不同策略。(默认为`USER`)<br/>直播领域可取值：<br/>`ADMIN`：房管<br/>`HOST`：主播<br/>`SYSTEM`：系统角色<br/>游戏领域可取值：<br/>`ADMIN`：管理员<br/>`USER`：普通用户 |
| receiveTokenId | string | 接收者的tokenId | 非必传参数 | 接收者的tokenId，私聊场景必选 |

其中，extra的内容如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| passThrough | json_object | 透传参数 | 否 | 客户传入透传字段，数美内部不回对该字段进行识别处理，随结果返回给用户，必须为json_object类型 |
| isTokenSeparate | int | 是否区分不同应用下的账号 | 否 | 是否区分不同应用下的账号，可能取值：<br/>0:不区分<br/>1:区分<br/>默认值为0 |

其中，imgs数组每个元素的具体内容如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| img            | string   | 要检测的图片，可使用base64编码的图片数据或者图片的url链接 建议图片下载从CDN源站下载，并且源站不能为单点<br/>风险：如果不是从源站下载，可能存在图片下载失败，导致无法审核 | 必传参数     | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>建议图片像素不小于256\*256, 目前最低支持20\*20分辨率的图片，图片大小最大10MB |
| imgCompareBase | string   | 要检测比对的基准图片，请求参数businessType字段包含标签`FACECOMPARE`时存在<br/>可使用base64编码的图片数据或者图片的url链接 | 非必传参数   | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>批量请求一次性不超过12张，建议图片像素不小于256\*256，图片大小最大10MB<br/><br/>基准图暂时不支持长图和动图格式 |
| btId | string | 图片唯一标识 | 必传参数 | 同一次请求中不可重复，btId长度在30以内 |

## 同步批量返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 |  `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1911`：图片下载失败<br/>`9101`：无权限操作<br/>  |
| message | string | 返回码描述 | 是 | 和code对应：成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>无权限操作 |
| requestId | string | 请求标识 | 是 | |
| imgs | json_array | 图片识别结果 | 是 | |
| auxInfo | json_object | 其他辅助信息 | 是 | |

其中，外层auxInfo内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| passThrough | json_object | 透传参数 | 否 | 客户传入透传字段，数美内部不回对该字段进行识别处理，随结果返回给用户，必须为json_object类型 |

其中，imgs数组每个元素的具体内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 该图片对应的返回码 | 是 | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1911`：图片下载失败<br/>`9101`：无权限操作<br/>除message和requestId之外的字段，只有当code为1100时才会存在 |
| message | string | 该图片对应的返回码描述 | 是 | 和code对应：成功QPS超限参数不合法服务失败余额不足无权限操作 |
| requestId | string | 该图片对应的请求标识 | 是 | |
| btId | string | 图片唯一标识 | 是 | 用于区分图片，和传入参数中的btId对应 |
| riskLevel | string | 处置建议 | 是 | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| riskLabel1 | string | 一级风险标签，当riskLevel为PASS时返回normal | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签，当riskLevel为PASS时为空 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签，当riskLevel为PASS时为空 | 是 | 三级风险标签 |
| riskDescription | string | 当riskLevel为`PASS`时为`正常` | 是 | 风险原因 |
| riskDetail | json_object | 风险详情 | 是 | 风险详情 |
| auxInfo | json_object | 其他辅助信息 | 是 | 其他辅助信息 |
| allLabels | json_array | 当riskLevel为PASS时为空 | 是 | 命中的所有风险标签以及详情信息 |
| businessLabels | json_array | 业务标签详情 | 否 | 当仅做识别，不需要配置reject、review策略的结果在此返回 |

imgs中，riskDetail的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| faces | json_array | 返回图片中涉政人物的名称及位置信息 | 否 | |
| face_num | int | 人脸数量 | 否 | |
| persons | json_array | 仅当命中人像-多人时，数组元素会有多个 |  | |
| person_num | int | 人像数量 |  | 有且仅有人像-多人下返回 |
| objects | json_array | 返回图片中标识或物品的名称及位置信息 | 否 | |
| ocrText | json_object | 返回图片中违规文字相关信息，当请求参数type字段包含`OCR`时存在 | 否 | |
| riskSource | int | 标识资源哪里违规 | 是 | 标识风险结果的来源：<br/>`1000`：无风险<br/>`1001`：文字风险<br/>`1002`：视觉图片风险 |

riskDetail中，faces数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| id | string | 编号，图片同一个位置下的人在不同标签下的编号相同。<br/>如果同一个人在图片中出现n次，分配n个ID |  |  |
| name | string | 人物名称 | 否 | 风险人物名称 |
| location | int_array | 人物位置信息，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否 | |
| face_ratio | float | 人脸占比 | 否 | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | 否 | 0～1之间的浮点数 |

riskDetail中，objects数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| id | string | 编号，保证同一个位置下的物品在不同标签下的编号相同 | 否 |  |
| name | string | 标识名称 | 否 | |
| location | int_array | 标识位置信息，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否 | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | 否 | 0～1之间的浮点数 |
| qrcontent | string | 二维码的url信息 | 否 |  |

riskDetail中，ocrText的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| text | string | 识别出的文字 | 是 | |
| matchedLists | json_array | 命中的客户自定义名单列表 | 否 | |
| riskSegments | json_array | 高风险片段内容 | 否 | 检测图片包含涉政、暴恐、违禁、竞品、广告法等风险内容的时候存在 |

ocrText中，matchedLists数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 命中的名单名称 | 否 | |
| words | json_array | 命中的敏感词信息 | 否 | |

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

imgs中，auxInfo的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segments | int | 实际处理的图片数量 | 是 | |
| typeVersion | json_object | 针对各个传入type的效果版本号 | 是 | |
| errorCode | int | | 否 | `2001`：输入数据格式不对，不是合法的json数据<br/>`2002`：输入的参数字段不合法（必填字段缺失、类型不对、值不合法等）<br/>`2003`：图片下载失败<br/>`2004`：图片过大，超过了10M<br/>`2005`：非法图片格式 |
| qrContent | string | 返回识别的二维码地址| 否 | 需要联系数美配置返回 |

auxInfo中，typeVersion的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| POLITY | string | 涉政版本号 | 否 | 组成形式为`X.Y`，`X`为主版本号，一般代表模型整体的效果迭代；`Y`为子版本号，一般代表日常的例行迭代<br/>例如`1001001.2`代表主版本号为`1001001`，子版本号为`2` |
| VIOLENT | string | 暴恐版本号 | 否 | 组成形式同上 |
| EROTIC | string | 色情版本号 | 否 | 组成形式同上 |
| ADVERT | string | 广告版本号 | 否 | 组成形式同上 |
| IMGTEXTRISK | string | 违规文字版本号 | 否 | 组成形式同上 |
| QRCODE | string | 二维码版本号 | 否 | 组成形式同上 |

imgs中，allLabels数组的每个成员的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险原因 | 是 | |
| riskLevel | string | 处置建议 | **是** | |
| probability | float | 置信度，可选值在0～1之间，值越大，风险可能性越高，值越小，无风险可能性越高 | 否 | |
| riskDetail | json_object | 风险详情 | 否 | |

riskLabel返回一级标签内容为：
| **一级标签** | **一级标识** | **类型** | **type类型** |
| --- | --- | --- | --- |
| 涉政 | politics | 监管标签 | POLITY |
| 色情 | porn | 监管标签 | EROTIC |
| 性感 | sexy | 监管标签 | EROTIC |
| 暴恐 | violence | 监管标签 | VIOLENT |
| 违禁 | ban | 监管标签 | VIOLENT |
| 广告 | ad | 监管标签 | ADVERT |
| 二维码 | qr | 监管标签 | QRCODE |

allLabels每个成员的riskDetail结构如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| faces | json_array | 人物信息，返回图片中涉政人物的名称及位置信息，内容与外层riskDetail.faces格式一致，内部字段参考外层riskDetail下的faces字段 | 否 |  |
| face_num | int | 仅当命中人脸-人脸类型-多人脸时，数组元素会有多个，<br/>最多10（如果超过10个，选择probability最高的10个） | 否 | |
| objects | json_array | 其他情况下，仅有一个数组元素标识信息，返回图片中标识或物品的名称及位置信息，内容与外层riskDetail.objects格式一致 | 否 |  |
| persons | json_array | 仅当命中人像-多人时，数组元素会有多个，最多10（如果<br/>超过10个，选择probability最高的10个），其他情况下，<br/>仅有一个元素，内部字段参考外层riskDetail下的persons字段 | 否 | |
| person_num | int | 有且仅有人像-多人下返回 | 否 | |
| ocrText | json_array | 返回图片中违规文字相关信息 | 否 | |
| riskSource | string | 标识资源哪里违规 | 是 | 标识风险结果的来源：`1000`：无风险<br/>`1001`：文字风险<br/>`1002`：视觉图片风险 |


其中imgs中businessLabels数组的每个成员的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string |  一级业务标签 | 是 | 一级业务标签 |
| businessLabel2 | string | 二级业务标签 | 是 | 二级业务标签 |
| businessLabel3 | string | 三级业务标签 | 是 | 三级业务标签 |
| businessDescription | string | 业务标签描述 | 是 | 中文标签描述 |
| businessDetail | json_object | 业务标签详情 | 否 | 格式详见下方businessDetail结构 |
| probability | float | 置信度<br/>可选值在0～1之间，值越大，可信度越高 | 是 | |
| confidenceLevel | int | 置信等级<br/>可选值在0～2之间，值越大，可信度越高<br/>注意：当检测模型是QR,OCR时不返回<br/>注意：当检测模型是FACE且riskLabe2不等于`gender`时不返回 | 否 | |

businessLabels数组中的businessDetail的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 明星人物名称<br/>图片中的明星人名type传值包含`FACE`时存在 | 否 |  |
| probability | float | 明星人物置信区间<br/>可选值在0～1之间，值越大，可信度越高，当且仅当name存在时出现 | 否 |  |
| face_ratio | float | 人脸占比<br/>在区间0-1，数值越大，人脸占比越高type传值包含`FACE`时存在 | 否 |  |
| faces | json_array | 内容与外层riskDetail.faces格式一致，内部字段参考外层riskDetail下的faces字段 | 否 | |
| objects | json_array | 其他情况下，仅有一个数组元素标识信息，返回图片中标识或物品的名称及位置信息，内容与外层riskDetail.objects格式一致 | 否 | 数组仅会有一个元素 |
| persons | json_array | 仅当命中人像-多人时，数组元素会有多个，最多10（如果<br/>超过10个，选择probability最高的10个），其他情况下，<br/>仅有一个元素，内部字段参考外层riskDetail下的persons字段 | 否 |  |
| face_num | int | 其他情况下，仅有一个数组元素人脸数检测<br/>图片中检测到的人脸个数<br/>仅当命中人脸-人脸类型-多人脸时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个） | 否 | |
| face_compare_num | int | 人脸比对人脸数检测<br/>图片中检测到的人脸个数，businessType传值包含`FACECOMPARE`时存在 | 否 | |
| location | int_array | 标识位置信息<br/>type传值包含`OBJECT`且时存在，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否 |  |
| person_num | int | 人体数量检测<br/>图片中检测到的人体个数type传值包含`PORTRAIT`且时存在 | 否 | |
| person_ratio | float | 人像占比<br/>在区间0-1，数值越大，人脸占比越高type传值包含`PORTRAIT`时存在 | 否 | |


##  同步批量回调返回参数

对于批量接口，和同步批量返回的结果相同。

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1911`：图片下载失败<br/>`9101`：无权限操作<br/> |
| message | string | 返回码描述 | 是 | 和code对应：成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>图片下载失败<br/>无权限操作 |
| requestIds | json\_array | 请求标识 | 是 | 多个图片流水号 |

requestIds详细如下：

| **返回结果**** 参数名 **|** 类型 **|** 参数说明 **|** 是否必返 **|** 规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 流水号 | 必返参数 | 返回的requestId |
| btId | string | 图片编号 | 必返参数 | 图片的btId |


## 同步批量示例：

### 同步批量请求示例：

```json
{
    "accessKey":"",
    "eventId":"",
    "type":"",
    "data":{
        "imgs":[
            {
               "btId":"123",
               "img":""
            },
            {
               "btId":"456",
               "img":""
            },
        ]
    }
}
```

### 同步返回示例：

```json
{
    "message":"成功",
    "requestId":"faf10b672ddae5e5e51ea719c44ca94b",
    "auxInfo":{

    },
    "code" :1100,
    "imgs":[
        {
            "allLabels":[

            ],
            "auxInfo":{
                "segments":1,
                "typeVersion":{
                    "OCR":"2001003.1",
                    "PORN":"3043001.1"
                }
            },
            "btId":"123",
            "code":1100,
            "message":"成功",
            "requestId":"faf10b672ddae5e5e51ea719c44ca94b_123",
            "riskDescription":"正常",
            "riskDetail":{
                "riskSource":1000
            },
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskLevel":"PASS",
            "tokenLabels":{
                "UGC_account_risk":{

                }
            }
        },
        {
            "allLabels":[

            ],
            "auxInfo":{
                "segments":1,
                "typeVersion":{
                    "OCR":"2001003.1",
                    "PORN":"3043001.1"
                }
            },
            "btId":"456",
            "code":1100,
            "message":"成功",
            "requestId":"faf10b672ddae5e5e51ea719c44ca94b_456",
            "riskDescription":"正常",
            "riskDetail":{
                "riskSource":1000
            },
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskLevel":"PASS",
            "tokenLabels":{
                "UGC_account_risk":{
                }
            }
        }
    ]
}
```


# 异步批量接口

## 异步批量上传请求

### 请求URL：

| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-img-bj.fengkongcloud.com/v4/saas/async/imgs` | 图片 |
| 上海 | `http://api-img-sh.fengkongcloud.com/v4/saas/async/imgs` | 图片 |

### 请求方法：
```
POST
```

### 字符编码：
```
UTF-8
```

### 建议超时时间：
5s

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥 | 必传参数 | 由数美提供 |
| appId | string | 应用标识 | 必传参数 | 用于区分应用，需要联系数美开通，请以数美单独提供的传值为准 |
| eventId | string | 事件标识 | 必传参数 |  需要联系数美服务开通，请使用数美单独提供的传值为准 |
| type | string | 检测的风险类型 | 必传参数 | **监管一级标签 可选值:<br/>POLITY :涉政识别<br/>EROTIC :色情&性感违规识别 <br/>VIOLENT :暴恐&违禁识别 <br/>QRCODE :二维码识别<br/>ADVERT :广告识别<br/>IMGTEXTRISK :图片文字违规识别<br/>如果需要识别多个功能，通过下划线连接，如 POLITY_QRCODE_ADVERT 用于涉政、二维码和广告组合识别（该字段与businessType字段必须选择一个传入）** |
| businessType | string | 业务标签类型 | 否 | 业务标签识别类型，可选值：[见附录](#附录) 如果需要多个识别功能，通过下划线连接，该字段和type必须选择一个传入 |
| data | json_object | 请求数据内容 | 必传参数 | 请求的数据内容，最长10MB |

其中，data的内容如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| imgs | json_array | 要检测的图片数组 | 必传参数 | |
| tokenId | string | 用户账号标识 | 必传参数 | 用于区分用户账号，建议传入用户ID |
| ip | string | ipv4地址 | 非必传参数 | 发送该图片的用户公网ipv4地址 |
| deviceId | string | 数美设备指纹标识 | 非必传参数 | 数美设备指纹生成的设备唯一标识 |
| maxFrame | int | gif图片的最大截帧数量 | 非必传参数 | 截取git等动图帧数，最大为20帧，默认为3帧，计费按照实际截帧数量计费，如默认为截取3帧时按照3帧进行计费 |
| interval | int | gif图片的截帧间隔 | 非必传参数 | 默认值为1，代表每一帧都需要进行检测，服务会自动调整该值以保证完全覆盖全部帧 |
| room | string | 直播房间号 | 非必传参数 |  |
| extra | json_object | 辅助参数 | 非必传参数 | 用于辅助文本检测的相关信息 |
| role | string | 用户角色 | 非必传参数 | 用户角色，默认USER，必须在可选范围有效对不同角色可配置不同策略。直播领域可取值：房管：ADMIN主播：HOST系统角色：SYSTEM游戏领域可取值：管理员：ADMIN普通用户：USER |
| receiveTokenId | string | 接收者的tokenId | 非必传参数 | 接收者的tokenId，私聊场景必选 |

其中，extra的内容如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| passThrough | json_object | 透传参数 | 否 | 客户传入透传字段，数美内部不回对该字段进行识别处理，随结果返回给用户，必须为json_object类型 |

其中，imgs数组每个元素的具体内容如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| img            | string   | 要检测的图片，可使用base64编码的图片数据或者图片的url链接 建议图片下载从CDN源站下载，并且源站不能为单点<br/>风险：如果不是从源站下载，可能存在图片下载失败，导致无法审核 | 必传参数     | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>建议图片像素不小于256\*256, 目前最低支持20\*20分辨率的图片，图片大小最大10MB |
| imgCompareBase | string   | 要检测比对的基准图片，请求参数Type字段包含标签`FACECOMPARE`时存在<br/>可使用base64编码的图片数据或者图片的url链接 | 非必传参数   | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>建议图片像素不小于256\*256图片大小最大10MB<br/><br/>基准图暂时不支持长图和动图格式 |
| btId | string | 图片唯一标识 | 必传参数 | 同一次请求中不可重复，btId长度在30以内 |

## 异步批量返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9101`：无权限操作<br/> |
| message | string | 返回码描述 | 是 | 和code对应：成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>无权限操作 |
| requestIds | json_array | 请求标识 | 是 | 多个图片流水号 |

requestIds中每一项是：

| **返回结果** | **参数名** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- | --- |
| requestId | string | 流水号 | 必返参数 | 返回的requestId |
| btId | string | 图片编号 | 必返参数 | 图片的btId |

### 异步批量返回示例
```json
{
    "code":1100,
    "message":"成功",
    "requestIds":[
        {
            "reques-tId":"d123456322adasfajfafjaskfjaf",
            "btId":"aaeevugq"
        },
        {
            "reques-tId":"d123456322adasfajfafjaskfjaf",
            "btId":"vccsewqy"
        }
    ]
}
```

#  主动查询接口

## 同步查询请求

### 请求URL：

| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-img-active-query.fengkongcloud.com/v4/image/query` | 图片 |

### 请求方法：
```
POST
```

### 字符编码：
```
UTF-8
```

### 建议超时时间：
5s

### 请求参数：

放在HTTP Body中，采用json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥 | 必传参数 | 公司密钥：用于权限认证，开通账号服务时由数美提供或使用开通邮箱登录数美后台右上角相关文档处查看 |
| requestIds | json_array | 流水号数组 | 必传参数 | 查询列表，最多支持10个流水号 |

requestIds每一项是：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| requestId | string | 流水号 | 必传参数 | 查询的requestId |
| btId | string | 图片编号 | 非必传参数 | 如果存在btId，则返回requestId+btId的批量查询中的单张结果；如果不存在btId，则分为两种情况：第一种情况，返回单张请求的结果；第二种情况，模糊匹配返回批量请求的结果 |

### 查询请求示例

```json
{
    "accessKey":"***",
    "requestIds":[
        {
            "requestId":"***",
            "btId":""
        }
    ]
}
```

## 查询返回参数

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 |  `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9101`：无权限操作<br/> |
| message | string | 返回码描述 | 是 | 和code对应：成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>无权限操作 |
| contents | json_array | 查询结果 | 是 | |

contents组成如下：

| **参数名称** | **类型** | **是否必选** | **说明** |
| --- | --- | --- | --- |
| requestId | string | 是 | 请求唯一标识 |
| result | json object | 否 | 返回结果 |
| btId | string | 否 | 图片id |
| code（表示该requestId对应的请求的状态） | int | 是 | `1102`:正在处理<br/>`1100`:处理完成<br/>`1910`:失败<br/>`1912`:处理超时(默认24h) |
| message | string | 是 | 和code对应：正在处理<br/>处理完成<br/>失败(根据不同情况显示具体失败的原因)<br/>处理超时(默认24h) |

result如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 |  `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1911`：图片下载失败<br/>`9101`：无权限操作<br/> |
| message | string | 返回码描述 | 是 | 和code对应：成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>图片下载失败<br/>无权限操作 |
| requestId | string | 请求标识 | 是 | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存 |
| riskLevel | string | 处置建议 | 是 | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| riskLabel1 | string | 一级风险标签 | 是 | 当riskLevel为PASS时返回normal |
| riskLabel2 | string | 二级风险标签 | 是 | 当riskLevel为PASS时为空 |
| riskLabel3 | string | 三级风险标签 | 是 | 当riskLevel为PASS时为空 |
| riskDescription | string | 风险原因 | 是 | 当riskLevel为`PASS`时为`正常` |
| riskDetail | json_object | 风险详情 | 是 | |
| auxInfo | json_object | 其他辅助信息 | 是 | |
| allLabels | json_array | 风险标签详情 | 是 | 返回命中的所有风险标签以及详情信息 |
| businessLabels | json_array | 业务标签详情 | 否 | 当仅做识别，不需要配置reject、review策略的结果在此返回 |

其中，riskDetail的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| faces | json_array | 返回图片中涉政人物的名称及位置信息 | 否 | |
| face_num | int | 人脸数量 | 否 | |
| persons | json_array | 仅当命中人像-多人时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个） |  | |
| person_num | int | 人像数量 |  | 有且仅有人像-多人下返回 |
| objects | json_array | 返回图片中标识或物品的名称及位置信息 | 否 | |
| ocrText | json_object | 返回图片中违规文字相关信息 | 否 | |
| riskSource | int | 标识资源哪里违规 | 是 | 标识风险结果的来源：<br/>`1000`：无风险<br/>`1001`：文字风险<br/>`1002`：视觉图片风险 |

riskDetail中，faces数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| id | string | 编号，图片同一个位置下的人在不同标签下的编号相同。<br/>如果同一个人在图片中出现n次，分配n个ID |  |  |
| name | string | 人物名称 | 否 | 风险人物名称 |
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
| qrcontent | string | 二维码的url信息 | 否 |  |

riskDetail中，ocrText的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| text | string | 识别出的文字 | 是 | |
| matchedLists | json_array | 命中的客户自定义名单列表 | 否 | |
| riskSegments | json_array | 高风险片段内容，检测图片包含涉政、暴恐、违禁、广告法等风险内容的时候存在 | 否 | |

ocrText中，matchedLists数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 命中的名单名称 | 否 | |
| words | json_array | 命中的敏感词信息 | 否 | |

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

其中，auxInfo的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| segments | int | 实际处理的图片数量 | 是 | |
| typeVersion | json_object | 针对各个传入type的效果版本号 | 是 | |
| errorCode | int | | 否 | `2001`：输入数据格式不对，不是合法的json数据<br/>`2002`：输入的参数字段不合法（必填字段缺失、类型不对、值不合法等）<br/>`2003`：图片下载失败<br/>`2004`：图片过大，超过了10M<br/>`2005`：非法图片格式<br/>`2006`：无效风险监测类型 |
| passThrough | json_object | 客户传入的透传字段 | 否 | |
| qrContent | string | 返回识别的二维码地址| 否 | 需要联系数美配置返回 |

auxInfo中，typeVersion的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| POLITY | string | 涉政版本号 | 否 | 组成形式为`X.Y`，`X`为主版本号，一般代表模型整体的效果迭代；`Y`为子版本号，一般代表日常的例行迭代<br/>例如`1001001.2`代表主版本号为`1001001`，子版本号为`2` |
| VIOLENT | string | 暴恐版本号 | 否 | 组成形式同上 |
| EROTIC | string | 色情版本号 | 否 | 组成形式同上 |
| ADVERT | string | 广告版本号 | 否 | 组成形式同上 |
| IMGTEXTRISK | string | 违规文字版本号 | 否 | 组成形式同上 |
| QRCODE | string | 二维码版本号 | 否 | 组成形式同上 |

其中allLabels数组的每个成员的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险原因 | 是 | |
| riskLevel | string | 处置建议 | **是** | |
| probability | float | 置信度，可选值在0～1之间，值越大，风险可能性越高，值越小，无风险可能性越高 | 否 | |
| riskDetail | json_object | 风险详情 | 否 | |

allLabels每个成员的riskDetail结构如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| faces | json_array | 人物信息 | 否 | 返回图片中涉政人物的名称及位置信息，内容与外层riskDetail.faces格式一致 |
| face_num | int | 仅当命中人脸-人脸类型-多人脸时，数组元素会有多个，<br/>最多10（如果超过10个，选择probability最高的10个） |||
| objects | json_array | 标识信息 | 否 | 返回图片中标识或物品的名称及位置信息，内容与外层riskDetail.objects格式一致 |
| persons | json_array | 仅当命中人像-多人时，数组元素会有多个，最多10（如果<br/>超过10个，选择probability最高的10个），其他情况下，<br/>仅有一个元素，内部字段参考外层riskDetail下的persons字段 |||
| person_num | int | 有且仅有人像-多人下返回 | 否 | |
| ocrText | json_array | 返回图片中违规文字相关信息，当请求参数type字段包含`OCR`时存在，内部字段参考外层riskDetail下的ocrText字段 |||
| riskSource | int | 标识资源哪里违规 |是|标识风险结果的来 源：<br/>`1000`：无风险<br/>1001 ：图片文字风险<br/>1002 ：视觉图片风险|

其中businessLabels数组的每个成员的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string |  一级业务标签 | 是 | 一级业务标签 |
| businessLabel2 | string | 二级业务标签 | 是 | 二级业务标签 |
| businessLabel3 | string | 三级业务标签 | 是 | 三级业务标签 |
| businessDescription | string | 业务标签描述 | 是 | 中文标签描述 |
| businessDetail | json_object | 业务标签详情 | 否 | 格式详见下方businessDetail结构 |
| probability | float | 置信度<br/>可选值在0～1之间，值越大，可信度越高 | 是 | |
| confidenceLevel | int | 置信等级<br/>可选值在0～2之间，值越大，可信度越高<br/>注意：当检测模型是QR,OCR时不返回<br/>注意：当检测模型是FACE且riskLabe2不等于`gender`时不返回 | 否 | |

businessLabels数组中的businessDetail的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 人物名称<br/> | 否 |  |
| probability | float | 明星人物置信区间<br/>可选值在0～1之间，值越大，可信度越高，当且仅当name存在时出现 | 否 |  |
| face_ratio | float | 人脸占比<br/>在区间0-1，数值越大，人脸占比越高 | 否 |  |
| faces | json_array | 内容与外层riskDetail.faces格式一致，内部字段参考外层riskDetail下的faces字段 | 否 | |
| objects | json_array | 其他情况下，仅有一个数组元素标识信息，返回图片中标识或物品的名称及位置信息，内容与外层riskDetail.objects格式一致 |  | 数组仅会有一个元素 |
| persons | json_array | 仅当命中人像-多人时，数组元素会有多个，最多10（如果<br/>超过10个，选择probability最高的10个），其他情况下，<br/>仅有一个元素，内部字段参考外层riskDetail下的persons字段 |  |  |
| face_num | int | 其他情况下，仅有一个数组元素人脸数检测<br/>图片中检测到的人脸个数<br/>仅当命中人脸-人脸类型-多人脸时，数组元素会有多个，最多10（如果超过10个，选择probability最高的10个） | 否 | |
| face_compare_num | int | 人脸比对人脸数检测<br/>图片中检测到的人脸个数，businessType传值包含`FACECOMPARE`时存在 | 否 | |
| location | int_array | 标识位置信息<br/>type传值包含`OBJECT`且时存在，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否 |  |
| person_num | int | 人体数量检测<br/>图片中检测到的人体个数type传值包含`PORTRAIT`且时存在 | 否 | |
| person_ratio | float | 人像占比<br/>在区间0-1，数值越大，人脸占比越高type传值包含`PORTRAIT`时存在 | 否 | |


# Demo

目前提供了 go、java、lua、nodes、php、python 的 demo，代码位置：
[https://github.com/ishumei/api-demo/tree/master/v4](https://github.com/ishumei/api-demo/tree/master/v4)



# 附录

| 一级标签 | 一级标识 | 类型 |
| ------------- | ------------- | --------------- |
| 色情| 	porn| 监管标签|
| 性感| 	sexy| 监管标签|
| 涉政| 	politics| 监管标签| 
| 违禁| 	ban| 监管标签| 
| 暴恐| 	violence| 监管标签| 
| 广告| 	ad| 监管标签| 
| 二维码| 	qr| 监管标签| 


| 业务标签识别类型 | 类型说明 | 备注 |
| ------------- | ------------- | --------------- |
| AGE           | 人脸 - 年龄   | 可识别未成年人 |
| GENDER        | 人脸 -性别   |  |
| BEAUTY        | 人脸 - 颜值   |  |
| RACE          | 人脸 - 人种   | 如黑种人、白种人、黄种人 |
| FACEDETECTION | 人脸-人脸检测 | 如识别真人、口罩人脸、正脸、侧脸等 |
| FAKEFACE | 人脸 - 伪造人脸 |  |
| FACECOMPARE | 人脸-人脸对比 |  |
| PUBLICFIGURE | 人物  - 公众人物 | 如识别知名明星、网红等 |
| TAINTEDSTAR | 人物 - 劣迹人物 |  |
| POSTURE | 人像-人像姿态 | 如识别坐姿、跪姿等 |
| DRESS | 人像 - 人像穿着 | 如识别jk、汉服等 |
| TEMPERAMENT | 人像 - 人像气质 | 如成熟大叔、靓丽女神等 |
| BODY | 人体 | 如识别头发、眼睛、鼻子等 |
| PICTUREFORM | 画面属性 - 画面类型 | 如识别动漫、表情包等 |
| PICTURESTRUCT | 画面属性-画面结构 | 如识别宫格图、桥段图等 |
| LOWVISION | 画面属性  - 画面低质 | 如识别模糊、涂抹、马赛克等 |
| LOWCONTNET | 画面属性 - 内容低质 | 如识别点线密集、虫类密集等 |
| LIVEPICTURE | 画面属性-直播画面 | 如识别床上直播、开车直播等 |
| SCREENSHOT | 画面属性 -  APP截图（内容搬运） | 如识别朋友圈截图、聊天截图等 |
| FITNESS | 场景主题-健身 |  |
| CATE | 场景主题-美食 |  |
| MUSIC | 场景主题-音乐 |  |
| SPORTS | 场景主题-体育 |  |
| SCENERY | 场景主题-自然风光 |  |
| CITYVIEW | 场景主题-城市风光 |  |
| 3CPRODUCTSLOGO | LOGO - 3C电子类品牌 |  |
| SHOPPINGAPPSLOGO | LOGO - 购物比价类应用 |  |
| RETOUCHAPPSLOGO | LOGO - 拍摄美化类应用 | 如识别快剪辑、秒拍等LOGO |
| SOCIALAPPSLOGO | LOGO - 社交通讯类应用 | 如识别微博、小红书等LOGO |
| PHOTOMATERIALLOGO | LOGO - 素材版权类应用 |  |
| NEWSAPPSLOGO | LOGO - 新闻阅读类应用 | 如识别新浪、视觉中国等LOGO |
| ENTERTAINMENTAPPSLOGO | LOGO - 影音娱乐类应用 | 如识别抖音、快手等LOGO |
| SPORTSLOGO | LOGO  - 体育赛事 |  |
| APPARELLOGO | LOGO - 鞋帽服饰类品牌 |  |
| ACCESSORIESLOGO | LOGO - 饰品首饰类品牌 |  |
| COSMETICSLOGO | LOGO - 化妆品类品牌 |  |
| FOODLOGO | LOGO - 食品类品牌 |  |
| VEHICLE | 物品-交通工具 |  |
| BUILDING | 物品-建筑 |  |
| TABLEWARE | 物品-餐具 |  |
| FOOD | 物品-食物 |  |
| HOMEAPPLICATION | 物品-家用电器 |  |
| OFFICESUPPLIES | 物品-办公用品 |  |
| FASHION | 物品-穿着用品 |  |
| SPORTEQUIPMENT | 物品-运动器材 |  |
| TOY | 物品-玩具 |  |
| MAKEUP | 物品-化妆品 |  |
| DRUGS | 物品-药品 |  |
| PAINTING | 物品-绘画作品 |  |
| ELECTRONIC | 物品-电子产品 |  |
| MEDICALIMAGE | 物品-医疗影像 |  |
| FURNITURE | 物品-家居用品 |  |
| DAILYSUPPLIES | 物品-生活用品 |  |
| CONSTELLATION | 物品-星座占卜 |  |
| KITCHENWARE | 物品-厨房用品 |  |
| KEEPSAKE | 物品 - 纪念品 |  |
| MAMMAL | 动物-哺乳动物 |  |
| BIRDS | 动物 - 鸟类 |  |
| REPTILE | 动物-爬行动物 |  |
| FISH | 动物-鱼 |  |
| ARTHROPOD | 动物  - 节肢动物 |  |
| COELENTERATE | 动物  - 腔肠动物 |  |
| MOLLUSKS | 动物  - 软体动物 |  |
| CRUSTACEAN | 动物  - 甲壳动物 |  |
| PLANT | 植物 |  |
| SETTING | 场所 |  |

