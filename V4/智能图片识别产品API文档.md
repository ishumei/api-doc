# 数美智能图片识别产品API接口文档

- - - - -

***版权所有 翻版必究***

- - - - -

目录

- [数美智能图片识别产品API接口文档](#数美智能图片识别产品api接口文档)
- [同步单张上传接口](#同步单张上传接口)
  - [同步单条请求](#同步单条请求)
    - [请求URL：](#请求url)
    - [请求方法：](#请求方法)
    - [字符编码：](#字符编码)
    - [建议超时时间：](#建议超时时间)
    - [请求参数：](#请求参数)
  - [同步返回结果](#同步返回结果)
  - [回调的同步返回参数](#回调的同步返回参数)
  - [同步示例：](#同步示例)
    - [同步请求示例：](#同步请求示例)
    - [同步返回示例：](#同步返回示例)
    - [回调的同步返回参数](#回调的同步返回参数-1)
- [异步单张上传接口](#异步单张上传接口)
  - [异步单条请求](#异步单条请求)
    - [请求URL：](#请求url-1)
    - [请求方法：](#请求方法-1)
    - [字符编码：](#字符编码-1)
    - [建议超时时间：](#建议超时时间-1)
    - [请求参数：](#请求参数-1)
  - [返回结果](#返回结果)
  - [异步单条示例](#异步单条示例)
    - [异步单条请求示例](#异步单条请求示例)
    - [异步单条返回示例](#异步单条返回示例)
- [同步批量接口](#同步批量接口)
  - [同步批量请求参数](#同步批量请求参数)
    - [请求URL：](#请求url-2)
    - [请求方法：](#请求方法-2)
    - [字符编码：](#字符编码-2)
    - [建议超时时间：](#建议超时时间-2)
    - [请求参数：](#请求参数-2)
  - [同步批量返回参数](#同步批量返回参数)
  - [同步批量回调返回参数](#同步批量回调返回参数)
- [异步批量接口](#异步批量接口)
  - [异步批量上传请求](#异步批量上传请求)
    - [请求URL：](#请求url-3)
    - [请求方法：](#请求方法-3)
    - [字符编码：](#字符编码-3)
    - [建议超时时间：](#建议超时时间-3)
    - [请求参数：](#请求参数-3)
  - [异步批量返回参数](#异步批量返回参数)
    - [异步批量返回示例](#异步批量返回示例)
- [主动查询接口](#主动查询接口)
  - [同步查询请求](#同步查询请求)
    - [请求URL：](#请求url-4)
    - [请求方法：](#请求方法-4)
    - [字符编码：](#字符编码-4)
    - [建议超时时间：](#建议超时时间-4)
    - [请求参数：](#请求参数-4)
    - [查询请求示例](#查询请求示例)
  - [查询返回参数](#查询返回参数)
- [Demo](#demo)

# 同步单张上传接口

## 同步单条请求

### 请求URL：
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-img-bj.fengkongcloud.com/image/v4` | 图片 |
| 上海 | `http://api-img-sh.fengkongcloud.com/image/v4` | 图片 |
| 新加坡 | `http://api-img-xjp.fengkongcloud.com/image/v4` | 图片 |
| 印度 | `http://api-img-yd.fengkongcloud.com/image/v4` | 图片 |
| 硅谷 | `http://api-img-gg.fengkongcloud.com/image/v4` | 图片 |

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
| accessKey | string | 接口认证密钥<br/>用于权限认证，开通账号服务时由数美提供或使用开通邮箱登录数美后台右上角相关文档处查看 | 必传参数 | accessKey |
| appId | string | 应用标识，用于区分相同公司的不同应用数据 | 必传参数 | 默认应用值：`default`<br/>传递其他值时需联系数美服务协助开通 |
| eventId | string | 事件标识 | 必传参数 | 需要联系数美服务开通，请使用数美单独提供的传值为准<br/>可选值：<br/>`headImage`：头像<br/>`album`：相册<br/>`dynamic`：动态<br/>`article`：帖子<br/>`comment`：评论<br/>`roomCover`：房间封面<br/>`groupMessage`：群聊图片<br/>`message`：私聊图片<br/>`product`：商品图片 |
| type | string | 检测的风险类型 | 必传参数 | 监管一级标签<br/>可选值：<br/>`POLITICS`：涉政识别<br/>`VIOLENCE`：暴恐识别<br/>`BAN`：违禁识别<br/>`PORN`：色情识别<br/>`AD`：广告识别<br/>`OCR`：识别图片中所有文字<br/>`FACECOMPARE`：人脸比对<br/>如果需要多个识别功能，通过下划线连接（该字段与businessType字段必须选择一个传入） |
| businessType | string | 业务标签类型 | 否 | 业务一级标签<br/>可选值：<br/>`LOGO`：商企LOGO识别<br/>`OCR`：识别图片中所有文字<br/>`MINOR`：未成年人识别<br/>`SCREEN`：特殊画面识别<br/>`SCENCE`：场景画面识别<br/>`QR`：二维码识别<br/>`QUALITY`: 图像质量识别<br/>`FACE`：人脸识别<br/>`STAR`：公众人物识<br/>`PORTRAIT`:人像识别<br/>`BEAUTY`: 颜值识别<br/>`ANIMAL`: 动物识别<br/>`OBJECT`：物品识别<br/>`IMAGECONTENT`: 画面属性识别<br/>`FACECOMPARE`: 人脸比对<br/>`PLANT`: 植物<br/>`BODY`: 人体<br/>如果需要多个识别功能，通过下划线连接，该字段和type必须选择一个传入 |
| data | json_object | 请求的数据内容 | 必传参数 | 请求的数据内容，最长10MB，[详见data参数](#data) |
| callback | string | 回调请求url，传callback表示走异步回调逻辑，否则走同步逻辑 | 非必传参数 | 异步回调逻辑支持30M图片<br/>同步支持10M图片 |

其中，data的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| tokenId | string | 用户账号标识，建议使用贵司用户UID（可加密）自行生成 , 标识用户唯一身份用作灌水和广告等行为维度风控。如无用户uid的场景建议使用唯一的数据标识传值 | 必传参数 | 由数字、字母、下划线、短杠组成的长度小于等于64位的字符串 |
| img | string | 要检测的图片，可使用base64编码的图片数据或者图片的url链接 **建议图片下载从CDN源站下载，并且源站不能为单点<br/>风险：如果不是从源站下载，可能存在图片下载失败，导致无法审核** | 必传参数 | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>建议图片像素不小于256\*256, 目前最低支持20\*20分辨率的图片，图片大小最大10MB |
| imgCompareBase | string | 要检测比对的基准图片，请求参数Type字段包含标签`FACECOMPARE`时存在<br/>可使用base64编码的图片数据或者图片的url链接 | 非必传参数 | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>建议图片像素不小于256\*256图片大小最大10MB<br/><br/>基准图暂时不支持长图和动图格式 |
| room | string | 直播房间号，高曝光群聊等业务场景建议传入房间号 | 非必传参数 | 仅当event取值为 videoClip 时，可传入该字段 |
| role | string | 用户角色 | 非必传参数 | 用户角色，必须在可选范围有效对不同角色可配置不同策略。(默认为`USER`)<br/>直播领域可取值：<br/>`ADMIN`：房管<br/>`HOST`：主播<br/>`SYSTEM`：系统角色<br/>游戏领域可取值：<br/>`ADMIN`：管理员<br/>`USER`：普通用户 |
| ip | string | ip地址 | 非必传参数 | 发送该图片的用户公网ipv4地址 |
| deviceId | string | 数美设备指纹标识 | 非必传参数 | 数美设备指纹生成的设备唯一标识 |
| maxFrame | int | gif图片的最大截帧数量 | 非必传参数 | 截取gif等动图帧数，最大为20帧，默认为3帧 |
| interval | int | gif图片的截帧间隔 | 非必传参数 | 默认值为`1`，代表每一帧都需要进行检测，服务会自动调整该值以保证完全覆盖全部帧 |
| extra | json_object | 辅助参数 | 非必传参数 | 用于辅助检测的相关信息，[详见extra参数](#extra) |
| streamInfo | json_object | 相似帧审核参数 | 非必传参数 | 用于检测相似帧的相关信息，[详见streamInfo参数](#streamInfo) |
| receiveTokenId | string | 接收者的tokenId | 非必传参数 | 接收者的tokenId，私聊场景必选 |

data中，streamInfo的内容如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| similarDedup | bool | 是否开启相似功能 | 否 |  |
| streamId | string | 透传参数 | 否 | 唯一标识id similarDedup为true时，必传 |
| timeWindow | int | 透传参数 | 否 |时间窗口，单位秒。similarDedup为true时，必传  |
| frameTime | int | 透传参数 | 否 | 截帧时间，单位ms， similarDedup为true时，必传 |
| riskNum | int | 透传参数 | 否 | 相似截帧图片不改变张数    数量范围(1-5)，默认为1 |

data中，extra的内容如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| isIgnoreTls | bool | 辅助参数 | 否 | 可选值（默认为`false`）：<br/>`true`：忽略证书信任<br/>`false`：校验证书 |
| passThrough | json_object | 透传参数 | 否 | 客户传入透传字段，数美内部不回对该字段进行识别处理，随结果返回给用户，必须为json_object类型 |
| isTokenSeparate | int | 是否区分不同应用下的账号 | 非必传参数 | 是否区分不同应用下的账号，可能取值：<br/>0:不区分<br/>1:区分<br/>默认值为0 |
## 同步返回结果

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 |  `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1911`：图片下载失败<br/>`9101`：无权限操作<br/>除message和requestId之外的字段，只有当code为1100时才会存在 |
| message | string | 返回码描述 | 是 | 和code对应：成功QPS超限参数不合法服务失败余额不足无权限操作 |
| requestId | string | 请求标识 | 是 | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存 |
| riskLevel | string | 处置建议 | 是 | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| riskLabel1 | string | 一级风险标签 | 是 | 当riskLevel为`PASS`时返回`normal` |
| riskLabel2 | string | 二级风险标签 | 是 | 当riskLevel为`PASS`时为空 |
| riskLabel3 | string | 三级风险标签 | 是 | 当riskLevel为`PASS`时为空 |
| riskDescription | string | 风险原因 | 是 | 当riskLevel为`PASS`时为`正常` |
| riskDetail | json_object | 风险详情 | 是 | [详见riskDetail参数](#riskDetail) |
| auxInfo | json_object | 其他辅助信息 | 是 | [详见auxInfo参数](#auxInfo) |
| allLabels | json_array | 风险标签详情 | 是 | 返回命中的所有风险标签以及详情信息 |
| businessLabels | json_array | 业务标签详情 | 否 | 请求参数Type字段包含`OCR`，`SCREEN`，`SCENCE`，`MINOR`，`QR`，`FACE`，`QUALITY`，`FACECOMPARE`以上标签时不为空，[详见businessLabels参数](#businessLabels) |
| tokenLabels    | json_object  | 账号标签信息 | 否 | 见下面详情内容，仅在tokenId传入且联系数美开通时返回 |

其中，riskDetail结构如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| faces | json_array | 返回图片中涉政人物的名称及位置信息 | 否 | |
| objects | json_array | 返回图片中标识或物品的名称及位置信息 | 否 | |
| ocrText | json_object | 返回图片中违规文字相关信息，当请求参数type字段包含`OCR`时存在 | 否 | |
| riskSource | int | 标识资源哪里违规 | 是 | 标识风险结果的来源：<br/>`1000`：无风险<br/>`1001`：文字风险<br/>`1002`：视觉图片风险 |

riskDetail中，faces数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 人物名称 | 否 | 风险人物名称 |
| location | int_array | 人物位置信息，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否 |  |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | 否 | 0～1之间的浮点数 |

riskDetail中，objects数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 标识名称 | 否 | |
| location | int_array | 标识位置信息，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标  | 否 | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | 否 | 0～1之间的浮点数 |

riskDetail中，ocrText的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| text | string | 识别出的文字 | 否 | |
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

其中，auxInfo的内容如下：

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
| POLITICS | string | 涉政版本号 | 否 | 组成形式为`X.Y`，`X`为主版本号，一般代表模型整体的效果迭代；`Y`为子版本号，一般代表日常的例行迭代<br/>例如`1001001.2`代表主版本号为`1001001`，子版本号为`2` |
| VIOLENCE | string | 暴恐版本号 | 否 | 组成形式同上 |
| BAN | string | 违禁版本号 | 否 | 组成形式同上 |
| PORN | string | 色情版本号 | 否 | 组成形式同上 |
| MINOR | string | 未成年人版本号 | 否 | 组成形式同上 |
| AD | string | 广告版本号 | 否 | 组成形式同上 |
| SPAM | string | 灌水版本号 | 否 | 组成形式同上 |
| LOGO | string | 商企LOGO版本号 | 否 | 组成形式同上 |
| STAR | string | 公众人物版本号 | 否 | 组成形式同上 |
| OCR | string | OCR版本号 | 否 | 组成形式同上 |
| IMGTEXT | string | 违规文字版本号 | 否 | 组成形式同上 |
| SCREEN | string | 特殊画面版本号 | 否 | 组成形式同上 |
| SCENCE | string | 场景画面版本号 | 否 | 组成形式同上 |
| QR | string | 二维码版本号 | 否 | 组成形式同上 |
| FACE | string | 人脸版本号 | 否 | 组成形式同上 |
| QUALITY | string | 图像质量版本号 | 否 | 组成形式同上 |
| PORTRAIT | string | 人像版本号 | 否 | 组成形式同上 |
| ANIMAL | string | 动物版本号 | 否 | 组成形式同上 |
| BEAUTY | string | 颜值版本号 | 否 | 组成形式同上 |

其中allLabels数组的每个成员的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险原因 | 是 | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | 否 |  |
| riskDetail | json_object | 风险详情 | 否 | |

allLabels每个成员的riskDetail结构如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| faces | json_array | 人物信息，返回图片中涉政人物的名称及位置信息，内容与外层riskDetail.faces格式一致 | 否 |  |
| objects | json_array | 标识信息，返回图片中标识或物品的名称及位置信息，内容与外层riskDetail.objects格式一致 | 否 |  |

其中businessLabels数组的每个成员的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string |  一级业务标签，当请求参数type字段包含业务标签传值时存在：`SCREEN`,`SCENCE`,`QR`,`FACE`,`QUALITY`,`MINOR`,`LOGO`,`OCR`,`BEAUTY`,`FACECOMPARE` | 否 | 一级业务标签 |
| businessLabel2 | string | 二级业务标签 | 否 | 二级业务标签 |
| businessLabel3 | string | 三级业务标签 | 否 | 三级业务标签 |
| businessDescription | string | 业务标签描述 | 否 | 中文标签描述 |
| businessDetail | json_object | 业务标签详情 | 否 | 格式详见下方businessDetail结构 |
| probability | float | 置信度<br/>可选值在0～1之间，值越大，可信度越高 | 否 | |
| confidenceLevel | int | 置信等级<br/>可选值在0～2之间，值越大，可信度越高<br/>注意：当检测模型是QR,OCR时不返回<br/>注意：当检测模型是FACE且riskLabe2不等于`gender`时不返回 | 否 | |

businessLabels数组中的businessDetail的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 明星人物名称<br/>图片中的明星人名type传值包含`FACE`时存在 | 否 |  |
| probability | float | 明星人物置信区间<br/>可选值在0～1之间，值越大，可信度越高，当且仅当name存在时出现 | 否 |  |
| face_ratio | float | 人脸占比<br/>在区间0-1，数值越大，人脸占比越高type传值包含`FACE`时存在 | 否 |  |
| face_num | int | 人脸数检测<br/>图片中检测到的人脸个数<br/>type传值包含`FACE`，`FACECOMPARE`时存在 | 否 | |
| face_compare_num | int | 人脸比对人脸数检测<br/>图片中检测到的人脸个数，type传值包含`FACECOMPARE`时存在 | 否 | |
| location | int_array | 标识位置信息<br/>type传值包含`OBJECT`且时存在，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否 |  |
| person_num | int | 人体数量检测<br/>图片中检测到的人体个数type传值包含`PORTRAIT`且时存在 | 否 | |
| person_ratio | float | 人像占比<br/>在区间0-1，数值越大，人脸占比越高type传值包含`PORTRAIT`时存在 | 否 | |

tokenLabels的详情内容如下：

| ***返回结果参数名*** | ***参数类型*** | ***参数说明***   | ***规范***                 |
| -------------------- | -------------- | ---------------- | -------------------------- |
| machine_account_risk | json_object    | 机器控制相关风险 |                            |
| UGC_account_risk     | json_object    | UGC内容相关风险  |                            |
| scene_account_risk   | json_object    | 场景账号风险     | 特殊场景才可取到，如航司等 |

machine_account_risk的详情内容如下：

| ***返回结果参数名***              | ***参数类型*** | ***参数说明*** | ***规范***                       |
| --------------------------------- | -------------- | -------------- | -------------------------------- |
| b_machine_control_tokenid         | int            | 机器账号       | 0：非机器控制账号1：机器控制账号 |
| b_machine_control_tokenid_last_ts | int            | 机器账号时间   |                                  |
| b_offer_wall_tokenid              | int            | 积分墙账号     | 0：非积分墙账号1：积分墙账号     |
| b_offer_wall_tokenid_last_ts      | int            | 积分墙账号时间 |                                  |

UGC_account_risk的详情内容如下：

| ***返回结果参数名***             | ***参数类型*** | ***参数说明*** | ***规范***                         |
| -------------------------------- | -------------- | -------------- | ---------------------------------- |
| b_politics_risk_tokenid          | int            | 涉政风险       | 0：暂未发现涉政风险1：存在涉政风险 |
| b_politics_risk_tokenid_last_ts  | int            | 涉政风险时间   |                                    |
| b_sexy_risk_tokenid              | int            | 色情风险       | 0：暂未发现色情风险1：存在色情风险 |
| b_sexy_risk_tokenid_last_ts      | int            | 色情风险时间   |                                    |
| b_advertise_risk_tokenid         | int            | 广告风险       | 0：暂未发现广告风险1：存在广告风险 |
| b_advertise_risk_tokenid_last_ts | int            | 广告风险时间   |                                    |

scene_account_risk的详情内容如下：

| ***返回结果参数名***    | ***参数类型*** | ***参数说明*** | ***规范***                   |
| --------------------------- | ------------------ | ------------------ | -------------------------------- |
| i_tout_risk_tokenid         | int                | 航司占座账号       | 0：非航司占座账号1：航司占座账号 |
| i_tout_risk_tokenid_last_ts | int                | 航司占座时间       |                                  |

## 回调的同步返回参数

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9101`：无权限操作<r=br>除message和requestId之外的字段，只有当code为`1100`时才会存在 |
| message | string | 返回码描述 | 是 | 和code对应：成功QPS超限参数不合法服务失败余额不足无权限操作 |
| requestId | string | 请求标识 | 是 | 图片的流水号 |

如果在请求参数中指定了回调协议接口URL callback，则需要支持POST方法，传输编码采用utf-8，审核结果放在HTTP Body中，采用Json格式，具体参数和V4单张同步请求结果相同。

## 同步单张示例：

### 同步请求示例：

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

### 同步返回示例：

```json
{
    "requestId":"9l25odfa5280c50f49f7c40988a1e400",
    "code":1100,
    "message":"成功",
    "riskLevel":"PASS",
    "riskLabel1":"normal",
    "riskLabel2":"",
    "riskLabel3":"",
    "riskDescription":"正常",
    "riskDetail":{
        "riskSource":1000
    },
    "auxInfo":{
        "segments":1,
        "typeVersion":{
            "LOGO":"1001049.11"
        }
    },
    "allLabels":[
        {
            "probability":0.53285801410675,
            "riskDescription":"涉政:国内领导人:国外领导",
            "riskDetail":{

            },
            "riskLabel1":"politics",
            "riskLabel2":"lingdaoren",
            "riskLabel3":"guowailingdao"
        },
        {
            "probability":0.537527859210968,
            "riskDescription":"涉政:涉政:涉政简图",
            "riskDetail":{

            },
            "riskLabel1":"politics",
            "riskLabel2":"shezheng",
            "riskLabel3":"shezhengjiantu"
        },
        {
            "probability":0.536710560321808,
            "riskDescription":"涉政:一号领导:一号领导",
            "riskDetail":{

            },
            "riskLabel1":"politics",
            "riskLabel2":"yihaolingdao",
            "riskLabel3":"yihaolingdao"
        },
        {
            "probability":0.983799457550049,
            "riskDescription":"::",
            "riskDetail":{
                "objects":[
                    {
                        "location":[
                            595,
                            459,
                            645,
                            671
                        ],
                        "name":"south_weekend",
                        "probability":0.983799457550049
                    }
                ]
            },
            "riskLabel1":"business",
            "riskLabel2":"logo",
            "riskLabel3":"south_weekend"
        },
        {
            "probability":0.53285801410675,
            "riskDescription":"涉政:反动分裂:港独人物",
            "riskDetail":{

            },
            "riskLabel1":"politics",
            "riskLabel2":"fandongfenlie",
            "riskLabel3":"gangdurenwu"
        },
        {
            "probability":0.529117465019226,
            "riskDescription":"涉政:反动分裂:异见人士",
            "riskDetail":{

            },
            "riskLabel1":"politics",
            "riskLabel2":"fandongfenlie",
            "riskLabel3":"yijianrenshi"
        },
        {
            "probability":0.529117465019226,
            "riskDescription":"涉政:国内领导人:常委领导",
            "riskDetail":{

            },
            "riskLabel1":"politics",
            "riskLabel2":"lingdaoren",
            "riskLabel3":"changweilingdao"
        }
    ],
    "businessLabels":[
        {
            "businessDescription":"商企LOGO:商企LOGO:商企LOGO",
            "businessDetail":{
                "location":[
                    595,
                    459,
                    645,
                    671
                ]
            },
            "businessLabel1":"logo",
            "businessLabel2":"south_weekend",
            "businessLabel3":"south_weekend",
            "confidenceLevel":2,
            "probability":0.983799457550049
        }
    ],
    "tokenLabels":{
        "UGC_account_risk":{
            "sexy_risk_tokenid":0
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
| appId | string | 应用标识 | 必传参数 | 应用标识：用于区分相同公司的不同应用数据默认应用值：<br/>`default`传递其他值时需联系数美服务协助开通 |
| eventId | string | 事件标识 | 必传参数 | 需要联系数美服务开通，请使用数美单独提供的传值为准，可选值：<br/>`headImage`：头像<br/>`album`：相册<br/>`dynamic`：动态<br/>`article`：帖子<br/>`comment`：评论<br/>`roomCover`：房间封面<br/>`groupMessage`：群聊图片<br/>`message`：私聊图片<br/>`product`：商品图片 |
| type | string | 检测的风险类型 | 必传参数 | 请使用数美单独提供的传值为准，可选值：<br/>`POLITICS`：涉政识别<br/>`VIOLENCE`：暴恐识别<br/>`BAN`：违禁识别<br/>`PORN`：色情识别<br/>`AD`：广告识别<br/>`OCR`：识别图片中所有文字<br/>`FACECOMPARE`：人脸比对<br/>如果需要多个识别功能，通过下划线连接（该字段与businessType字段必须选择一个传入） |
| businessType | string | 业务标签类型 | 否 | 业务标签识别类型，可选值：<br/>`LOGO`：商企LOGO识别<br/>`OCR`：识别图片中所有文字<br/>`MINOR`：未成年人识别<br/>`SCREEN`：特殊画面识别<br/>`SCENCE`：场景画面识别<br/>`QR`：二维码识别<br/>`QUALITY`: 图像质量识别<br/>`FACE`：人脸识别<br/>`STAR`：公众人物识<br/>`PORTRAIT`:人像识别<br/>`BEAUTY`: 颜值识别<br/>`ANIMAL`: 动物识别<br/>`OBJECT`：物品识别<br/>`IMAGECONTENT`: 画面属性识别<br/>`FACECOMPARE`: 人脸比对<br/>`PLANT`: 植物<br/>`BODY`: 人体<br/>如果需要多个识别功能，通过下划线连接 |
| data | json_object | 请求的数据内容 | 必传参数 | 请求的数据内容，最长10MB |

其中，data的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| tokenId | string | 用户账号标识 | 必传参数 | 建议使用贵司用户UID（可加密）自行生成 , 标识用户唯一身份用作灌水和广告等行为维度风控。如无用户uid的场景建议使用唯一的数据标识传值 |
| img            | string       | 要检测的图片，可使用base64编码的图片数据或者图片的url链接 **建议图片下载从CDN源站下载，并且源站不能为单点<br/>风险：如果不是从源站下载，可能存在图片下载失败，导致无法审核** | 必传参数     | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>建议图片像素不小于256\*256, 目前最低支持20\*20分辨率的图片，图片大小最大10MB |
| imgCompareBase | string | 要检测比对的基准图片，请求参数Type字段包含标签`FACECOMPARE`时存在<br/>可使用base64编码的图片数据或者图片的url链接 | 非必传参数 | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>建议图片像素不小于256\*256图片大小最大10MB<br/><br/>基准图暂时不支持长图和动图格式 |
| room | string | 直播房间号 | 非必传参数 | 仅当event取值为 videoClip 时，可传入该字段 |
| role | string | 用户角色 | 非必传参数 | 用户角色，必须在可选范围有效对不同角色可配置不同策略。(默认为`USER`)<br/>直播领域可取值：<br/>`ADMIN`：房管<br/>`HOST`：主播<br/>`SYSTEM`：系统角色<br/>游戏领域可取值：<br/>`ADMIN`：管理员<br/>`USER`：普通用户 |
| ip | string | ip地址 | 非必传参数 | 发送该图片的用户公网ipv4地址 |
| deviceId | string | 数美设备指纹标识 | 非必传参数 | 数美设备指纹生成的设备唯一标识 |
| maxFrame | int | gif图片的最大截帧数量 | 非必传参数 | 截取gif等动图帧数，最大为20帧，默认为3帧 |
| interval | int | gif图片的截帧间隔 | 非必传参数 | 默认值为1，代表每一帧都需要进行检测，服务会自动调整该值以保证完全覆盖全部帧 |
| extra | json\_object | 辅助参数 | 非必传参数 | 用于辅助检测的相关信息 |
| receiveTokenId | string | 接收者的tokenId | 非必传参数 | 接收者的tokenId，私聊场景必选 |

其中，extra内容如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| isIgnoreTls | bool | 辅助参数 | 否 | 可选值（默认为`false`）：<br/>`true`：忽略证书信任<br/>`false`：校验证书 |
| passThrough | json_object | 透传参数 | 否 | 客户传入透传字段，数美内部不回对该字段进行识别处理，随结果返回给用户，必须为json_object类型 |
## 返回结果

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1911`：图片下载失败<br/>`9101`：无权限操作<br/>除message和requestId之外的字段，只有当code为1100时才会存在 |
| message | string | 返回码描述 | 是 | 和code对应：成功QPS超限参数不合法服务失败余额不足无权限操作 |
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
| appId | string | 应用标识 | 必传参数 | 用于区分相同公司的不同应用数据<br/>默认应用值：`default`<br/>传递其他值时需联系数美服务协助开通 |
| eventId | string | 事件标识 | 必传参数 | 需要联系数美服务开通，请使用数美单独提供的传值为准，可选值：`default`：默认<br/>`headImage`：头像<br/>`album`：相册<br/>`dynamic`：动态<br/>`article`：帖子<br/>`comment`：评论<br/>`roomCover`：房间封面<br/>`groupMessage`：群聊图片<br/>`message`：私聊图片<br/>`product`：商品图片 |
| type | string | 检测的风险类型 | 必传参数 | 请使用数美单独提供的传值为准，可选值：<br/>`POLITICS`：涉政识别<br/>`VIOLENCE`：暴恐识别<br/>`BAN`：违禁识别<br/>`PORN`：色情识别<br/>`AD`：广告识别<br/>`OCR`：识别图片中所有文字<br/>`FACECOMPARE`：人脸比对<br/>如果需要多个识别功能，通过下划线连接（该字段与businessType字段必须选择一个传入） |
| businessType | string | 业务标签类型 | 否 | 业务标签识别类型，可选值：<br/>`LOGO`：商企LOGO识别<br/>`OCR`：识别图片中所有文字<br/>`MINOR`：未成年人识别<br/>`SCREEN`：特殊画面识别<br/>`SCENCE`：场景画面识别<br/>`QR`：二维码识别<br/>`QUALITY`: 图像质量识别<br/>`FACE`：人脸识别<br/>`STAR`：公众人物识<br/>`PORTRAIT`:人像识别<br/>`BEAUTY`: 颜值识别<br/>`ANIMAL`: 动物识别<br/>`OBJECT`：物品识别<br/>`IMAGECONTENT`: 画面属性识别<br/>`FACECOMPARE`: 人脸比对<br/>`PLANT`: 植物<br/>`BODY`: 人体<br/>如果需要多个识别功能，通过下划线连接，该字段和type必须选择一个传入 |
| data | json_object | 请求数据内容 | 必传参数 | 请求的数据内容，最长10MB |
| callback | string | 回调请求url | 非必传参数 | 传callback表示走异步回调逻辑，异步回调逻辑支持30M图片；否则走同步逻辑，同步支持10M图片。 |

其中，data的内容如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| imgs | json_array | 要检测的图片数组 | 必传参数 | |
| tokenId | string | 用户账号标识 | 必传参数 | 用于区分用户账号，建议传入用户ID |
| ip | string | ipv4地址 | 非必传参数 | 发送该图片的用户公网ipv4地址 |
| deviceId | string | 数美设备指纹标识 | 非必传参数 | 数美设备指纹生成的设备唯一标识 |
| maxFrame | int | gif图片的最大截帧数量 | 非必传参数 | 截取gif等动图帧数，最大为20帧，默认为3帧 |
| interval | int | gif图片的截帧间隔 | 非必传参数 | 默认值为1，代表每一帧都需要进行检测，服务会自动调整该值以保证完全覆盖全部帧 |
| room | string | 直播房间号 | 非必传参数 | 仅当event取值为 videoClip 时，可传入该字段 |
| extra | json_object | 辅助参数 | 非必传参数 | 用于辅助文本检测的相关信息 |
| role | string | 用户角色 | 非必传参数 | 用户角色，必须在可选范围有效对不同角色可配置不同策略。(默认为`USER`)<br/>直播领域可取值：<br/>`ADMIN`：房管<br/>`HOST`：主播<br/>`SYSTEM`：系统角色<br/>游戏领域可取值：<br/>`ADMIN`：管理员<br/>`USER`：普通用户 |
| receiveTokenId | string | 接收者的tokenId | 非必传参数 | 接收者的tokenId，私聊场景必选 |

其中，extra的内容如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| passThrough | json_object | 透传参数 | 否 | 客户传入透传字段，数美内部不回对该字段进行识别处理，随结果返回给用户，必须为json_object类型 |
| isTokenSeparate | int | 是否区分不同应用下的账号 | 非必传参数 | 是否区分不同应用下的账号，可能取值：<br/>0:不区分<br/>1:区分<br/>默认值为0 |

其中，imgs数组每个元素的具体内容如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| img            | string   | 要检测的图片，可使用base64编码的图片数据或者图片的url链接 **建议图片下载从CDN源站下载，并且源站不能为单点<br/>风险：如果不是从源站下载，可能存在图片下载失败，导致无法审核** | 必传参数     | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>建议图片像素不小于256\*256, 目前最低支持20\*20分辨率的图片，图片大小最大10MB |
| imgCompareBase | string   | 要检测比对的基准图片，请求参数Type字段包含标签`FACECOMPARE`时存在<br/>可使用base64编码的图片数据或者图片的url链接 | 非必传参数   | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>建议图片像素不小于256\*256图片大小最大10MB<br/><br/>基准图暂时不支持长图和动图格式 |
| btId | string | 图片唯一标识 | 必传参数 | 同一次请求中不可重复，btId长度在30以内 |

## 同步批量返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 |  `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1911`：图片下载失败<br/>`9101`：无权限操作<br/>除message和requestId之外的字段，只有当code为1100时才会存在  |
| message | string | 返回码描述 | 是 | 和code对应：成功QPS超限参数不合法服务失败余额不足无权限操作 |
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
| businessLabels | json_array | 业务标签详情，请求参数Type字段包含`OCR`，`SCREEN`，`SCENCE`，`MINOR`，`QR`，`FACE`，`QUALITY`，`FACECOMPARE`以上标签时不为空 | 否 | 业务标签详情 |

imgs中，riskDetail的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| faces | json_array | 返回图片中涉政人物的名称及位置信息 | 否 | |
| objects | json_array | 返回图片中标识或物品的名称及位置信息 | 否 | |
| ocrText | json_object | 返回图片中违规文字相关信息，当请求参数type字段包含`OCR`时存在 | 否 | |
| riskSource | int | 标识资源哪里违规 | 是 | 标识风险结果的来源：<br/>`1000`：无风险<br/>`1001`：文字风险<br/>`1002`：视觉图片风险 |

riskDetail中，faces数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 人物名称 | 否 | 风险人物名称 |
| location | int_array | 人物位置信息，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否 | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | 否 | 0～1之间的浮点数 |

riskDetail中，objects数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 标识名称 | 否 | |
| location | int_array | 标识位置信息，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否 | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | 否 | 0～1之间的浮点数 |

riskDetail中，ocrText的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| text | string | 识别出的文字 | 否 | |
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
| POLITICS | string | 涉政版本号 | 否 | 组成形式为`X.Y`，`X`为主版本号，一般代表模型整体的效果迭代；`Y`为子版本号，一般代表日常的例行迭代<br/>例如`1001001.2`代表主版本号为`1001001`，子版本号为`2` |
| VIOLENCE | string | 暴恐版本号 | 否 | 组成形式同上 |
| BAN | string | 违禁版本号 | 否 | 组成形式同上 |
| PORN | string | 色情版本号 | 否 | 组成形式同上 |
| MINOR | string | 未成年人版本号 | 否 | 组成形式同上 |
| AD | string | 广告版本号 | 否 | 组成形式同上 |
| SPAM | string | 灌水版本号 | 否 | 组成形式同上 |
| LOGO | string | 商企LOGO版本号 | 否 | 组成形式同上 |
| STAR | string | 公众人物版本号 | 否 | 组成形式同上 |
| OCR | string | OCR版本号 | 否 | 组成形式同上 |
| IMGTEXT | string | 违规文字版本号 | 否 | 组成形式同上 |
| SCREEN | string | 特殊画面版本号 | 否 | 组成形式同上 |
| SCENCE | string | 场景画面版本号 | 否 | 组成形式同上 |
| QR | string | 二维码版本号 | 否 | 组成形式同上 |
| FACE | string | 人脸版本号 | 否 | 组成形式同上 |
| QUALITY | string | 图像质量版本号 | 否 | 组成形式同上 |
| PORTRAIT | string | 人像版本号 | 否 | 组成形式同上 |
| ANIMAL | string | 动物版本号 | 否 | 组成形式同上 |
| BEAUTY | string | 颜值版本号 | 否 | 组成形式同上 |

imgs中，allLabels数组的每个成员的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险原因 | 是 | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | 否 | |
| riskDetail | json_object | 风险详情 | 否 | |

allLabels每个成员的riskDetail结构如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| faces | json_array | 人物信息 | 否 | 返回图片中涉政人物的名称及位置信息，内容与外层riskDetail.faces格式一致 |
| objects | json_array | 标识信息 | 否 | 返回图片中标识或物品的名称及位置信息，内容与外层riskDetail.objects格式一致 |

其中imgs中businessLabels数组的每个成员的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string |  一级业务标签，当请求参数type字段包含业务标签传值时存在：`SCREEN`,`SCENCE`,`QR`,`FACE`,`QUALITY`,`MINOR`,`LOGO`,`OCR`,`BEAUTY`,`FACECOMPARE` | 否 | 一级业务标签 |
| businessLabel2 | string | 二级业务标签 | 否 | 二级业务标签 |
| businessLabel3 | string | 三级业务标签 | 否 | 三级业务标签 |
| businessDescription | string | 业务标签描述 | 否 | 中文标签描述 |
| businessDetail | json_object | 业务标签详情 | 否 | 格式详见下方businessDetail结构 |
| probability | float | 置信度<br/>可选值在0～1之间，值越大，可信度越高 | 否 | |
| confidenceLevel | int | 置信等级<br/>可选值在0～2之间，值越大，可信度越高<br/>注意：当检测模型是QR,OCR时不返回<br/>注意：当检测模型是FACE且riskLabe2不等于`gender`时不返回 | 否 | |

businessLabels数组中的businessDetail的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 明星人物名称<br/>图片中的明星人名type传值包含`FACE`时存在 | 否 | |
| probability | float | 明星人物置信区间<br/>可选值在0～1之间，值越大，可信度越高，当且仅当name存在时出现 | 否 | |
| face_ratio | float | 人脸占比<br/>在区间0-1，数值越大，人脸占比越高type传值包含`FACE`时存在 | 否 | |
| face_num | int | 人脸数检测<br/>图片中检测到的人脸个数 type传值包含`FACE`，`FACECOMPARE`时存在 | 否 | |
| face_compare_num | int | 人脸比对人脸数检测<br/>图片中检测到的人脸个数，type传值包含`FACECOMPARE`时存在 | 否 | |
| location | int_array | 标识位置信息<br/>type传值包含`OBJECT`且时存在，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否 | |
| person_num | int | 人体数量检测<br/>图片中检测到的人体个数type传值包含`PORTRAIT`且时存在 | 否 | |
| person_ratio | float | 人像占比<br/>在区间0-1，数值越大，人脸占比越高type传值包含`PORTRAIT`时存在 | 否 | |

##  同步批量回调返回参数

对于批量接口，和同步批量返回的结果相同。

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1911`：图片下载失败<br/>`9101`：无权限操作<br/>除message和requestId之外的字段，只有当code为1100时才会存在 |
| message | string | 返回码描述 | 是 | 和code对应：成功QPS超限参数不合法服务失败余额不足无权限操作 |
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
| appId | string | 应用标识 | 必传参数 | 用于区分应用，可选值如下：`default`：默认应用<br/>额外应用值需数美单独分配提供 |
| eventId | string | 事件标识 | 必传参数 |  需要联系数美服务开通，请使用数美单独提供的传值为准，可选值：<br/>`default`：默认<br/>`headImage`：头像<br/>`album`：相册<br/>`dynamic`：动态<br/>`article`：帖子<br/>`comment`：评论<br/>`roomCover`：房间封面<br/>`groupMessage`：群聊图片<br/>`message`：私聊图片<br/>`product`：商品图片 |
| type | string | 检测的风险类型 | 必传参数 | 请使用数美单独提供的传值为准，可选值：<br/>`POLITICS`：涉政识别<br/>`VIOLENCE`：暴恐识别<br/>`BAN`：违禁识别<br/>`PORN`：色情识别<br/>`AD`：广告识别<br/>`OCR`：识别图片中所有文字<br/>`FACECOMPARE`：人脸比对<br/>如果需要多个识别功能，通过下划线连接（该字段与businessType字段必须选择一个传入） |
| businessType | string | 业务标签类型 | 否 | 业务标签识别类型，可选值：<br/>`LOGO`：商企LOGO识别<br/>`OCR`：识别图片中所有文字<br/>`MINOR`：未成年人识别<br/>`SCREEN`：特殊画面识别<br/>`SCENCE`：场景画面识别<br/>`QR`：二维码识别<br/>`QUALITY`: 图像质量识别<br/>`FACE`：人脸识别<br/>`STAR`：公众人物识<br/>`PORTRAIT`:人像识别<br/>`BEAUTY`: 颜值识别<br/>`ANIMAL`: 动物识别<br/>`OBJECT`：物品识别<br/>`IMAGECONTENT`: 画面属性识别<br/>`FACECOMPARE`: 人脸比对<br/>`PLANT`: 植物<br/>`BODY`: 人体<br/>如果需要多个识别功能，通过下划线连接，该字段和type必须选择一个传入 |
| data | json_object | 请求数据内容 | 必传参数 | 请求的数据内容，最长10MB |

其中，data的内容如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| imgs | json_array | 要检测的图片数组 | 必传参数 | |
| tokenId | string | 用户账号标识 | 必传参数 | 用于区分用户账号，建议传入用户ID |
| ip | string | ipv4地址 | 非必传参数 | 发送该图片的用户公网ipv4地址 |
| deviceId | string | 数美设备指纹标识 | 非必传参数 | 数美设备指纹生成的设备唯一标识 |
| maxFrame | int | gif图片的最大截帧数量 | 非必传参数 | 截取gif等动图帧数，最大为20帧，默认为3帧 |
| interval | int | gif图片的截帧间隔 | 非必传参数 | 默认值为1，代表每一帧都需要进行检测，服务会自动调整该值以保证完全覆盖全部帧 |
| room | string | 直播房间号 | 非必传参数 | 仅当event取值为 videoClip 时，可传入该字段 |
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
| img            | string   | 要检测的图片，可使用base64编码的图片数据或者图片的url链接 **建议图片下载从CDN源站下载，并且源站不能为单点<br/>风险：如果不是从源站下载，可能存在图片下载失败，导致无法审核** | 必传参数     | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>建议图片像素不小于256\*256, 目前最低支持20\*20分辨率的图片，图片大小最大10MB |
| imgCompareBase | string   | 要检测比对的基准图片，请求参数Type字段包含标签`FACECOMPARE`时存在<br/>可使用base64编码的图片数据或者图片的url链接 | 非必传参数   | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>建议图片像素不小于256\*256图片大小最大10MB<br/><br/>基准图暂时不支持长图和动图格式 |
| btId | string | 图片唯一标识 | 必传参数 | 同一次请求中不可重复，btId长度在30以内 |

## 异步批量返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9101`：无权限操作<br/>除message和requestId之外的字段，只有当code为1100时才会存在 |
| message | string | 返回码描述 | 是 | 和code对应：成功QPS超限参数不合法服务失败余额不足无权限操作 |
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
| code | int | 返回码 | 是 |  `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`9101`：无权限操作<br/>除message和requestId之外的字段，只有当code为1100时才会存在 |
| message | string | 返回码描述 | 是 | 和code对应：成功QPS超限参数不合法服务失败无权限操作 |
| contents | json_array | 查询结果 | 是 | |

contents组成如下：

| **参数名称** | **类型** | **是否必选** | **说明** |
| --- | --- | --- | --- |
| requestId | string | 是 | 请求唯一标识 |
| result | json object | 否 | 返回结果 |
| btId | string | 否 | 图片id |
| code（表示该requestId对应的请求的状态） | int | 是 | `1102`:正在处理<br/>`1100`:处理完成<br/>`1910`:失败<br/>`1912`:处理超时(默认24h) |
| message | string | 是 | 和code对应：正在处理处理完成失败(根据不同情况显示具体失败的原因)处理超时(默认24h) |

result如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 |  `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1911`：图片下载失败<br/>`9101`：无权限操作<br/>除message和requestId之外的字段，只有当code为1100时才会存在 |
| message | string | 返回码描述 | 是 | 和code对应：成功QPS超限参数不合法服务失败余额不足无权限操作 |
| requestId | string | 请求标识 | 是 | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存 |
| riskLevel | string | 处置建议 | 是 | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| riskLabel1 | string | 一级风险标签 | 是 | 当riskLevel为PASS时返回normal |
| riskLabel2 | string | 二级风险标签 | 是 | 当riskLevel为PASS时为空 |
| riskLabel3 | string | 三级风险标签 | 是 | 当riskLevel为PASS时为空 |
| riskDescription | string | 风险原因 | 是 | 当riskLevel为`PASS`时为`正常` |
| riskDetail | json_object | 风险详情 | 是 | |
| auxInfo | json_object | 其他辅助信息 | 是 | |
| allLabels | json_array | 风险标签详情 | 是 | 返回命中的所有风险标签以及详情信息 |
| businessLabels | json_array | 业务标签详情 | 否 | 请求参数Type字段包含`OCR`，`SCREEN`，`SCENCE`，`MINOR`，`QR`，`FACE`，`QUALITY`，`FACECOMPARE`以上标签时不为空 |

其中，riskDetail的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| faces | json_array | 返回图片中涉政人物的名称及位置信息 | 否 | |
| objects | json_array | 返回图片中标识或物品的名称及位置信息 | 否 | |
| ocrText | json_object | 返回图片中违规文字相关信息，当请求参数type字段包含`OCR`时存在 | 否 | |
| riskSource | int | 标识资源哪里违规 | 是 | 标识风险结果的来源：<br/>`1000`：无风险<br/>`1001`：文字风险<br/>`1002`：视觉图片风险 |

riskDetail中，faces数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 人物名称 | 否 | 风险人物名称 |
| location | int_array | 人物位置信息，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否 | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | 否 | 0～1之间的浮点数 |

riskDetail中，objects数组每个元素的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 标识名称 | 否 | |
| location | int_array | 标识位置信息，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否 | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | 否 | 0～1之间的浮点数 |

riskDetail中，ocrText的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| text | string | 识别出的文字 | 否 | |
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
| POLITICS | string | 涉政版本号 | 否 | 组成形式为`X.Y`，`X`为主版本号，一般代表模型整体的效果迭代；`Y`为子版本号，一般代表日常的例行迭代<br/>例如`1001001.2`代表主版本号为`1001001`，子版本号为`2` |
| VIOLENCE | string | 暴恐版本号 | 否 | 组成形式同上 |
| BAN | string | 违禁版本号 | 否 | 组成形式同上 |
| PORN | string | 色情版本号 | 否 | 组成形式同上 |
| MINOR | string | 未成年人版本号 | 否 | 组成形式同上 |
| AD | string | 广告版本号 | 否 | 组成形式同上 |
| SPAM | string | 灌水版本号 | 否 | 组成形式同上 |
| LOGO | string | 商企LOGO版本号 | 否 | 组成形式同上 |
| STAR | string | 公众人物版本号 | 否 | 组成形式同上 |
| OCR | string | OCR版本号 | 否 | 组成形式同上 |
| IMGTEXT | string | 违规文字版本号 | 否 | 组成形式同上 |
| SCREEN | string | 特殊画面版本号 | 否 | 组成形式同上 |
| SCENCE | string | 场景画面版本号 | 否 | 组成形式同上 |
| QR | string | 二维码版本号 | 否 | 组成形式同上 |
| FACE | string | 人脸版本号 | 否 | 组成形式同上 |
| QUALITY | string | 图像质量版本号 | 否 | 组成形式同上 |
| PORTRAIT | string | 人像版本号 | 否 | 组成形式同上 |
| ANIMAL | string | 动物版本号 | 否 | 组成形式同上 |
| BEAUTY | string | 颜值版本号 | 否 | 组成形式同上 |

其中allLabels数组的每个成员的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | 一级风险标签 | 是 | 一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 是 | 二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 是 | 三级风险标签 |
| riskDescription | string | 风险原因 | 是 | |
| probability | float | 置信度，可选值在0～1之间，值越大，可信度越高 | 否 | |
| riskDetail | json_object | 风险详情 | 否 | |

allLabels每个成员的riskDetail结构如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| faces | json_array | 人物信息 | 否 | 返回图片中涉政人物的名称及位置信息，内容与外层riskDetail.faces格式一致 |
| objects | json_array | 标识信息 | 否 | 返回图片中标识或物品的名称及位置信息，内容与外层riskDetail.objects格式一致 |

其中businessLabels数组的每个成员的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string |  一级业务标签 | 否 | 一级业务标签 |
| businessLabel2 | string | 二级业务标签 | 否 | 二级业务标签 |
| businessLabel3 | string | 三级业务标签 | 否 | 三级业务标签 |
| businessDescription | string | 业务标签描述 | 否 | 中文标签描述 |
| businessDetail | json_object | 业务标签详情 | 否 | 格式详见下方businessDetail结构 |
| probability | float | 置信度<br/>可选值在0～1之间，值越大，可信度越高 | 否 | |
| confidenceLevel | int | 置信等级<br/>可选值在0～2之间，值越大，可信度越高<br/>注意：当检测模型是QR,OCR时不返回<br/>注意：当检测模型是FACE且riskLabe2不等于`gender`时不返回 | 否 | |

businessLabels数组中的businessDetail的内容如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| name | string | 明星人物名称<br/>图片中的明星人名type传值包含`FACE`时存在 | 否 | |
| probability | float | 明星人物置信区间<br/>可选值在0～1之间，值越大，可信度越高，当且仅当name存在时出现 | 否 | |
| face_ratio | float | 人脸占比<br/>在区间0-1，数值越大，人脸占比越高type传值包含`FACE`时存在 | 否 | |
| face_num | int | 人脸数检测<br/>图片中检测到的人脸个数<br/>type传值包含`FACE`，`FACECOMPARE`时存在 | 否 | |
| face_compare_num | int | 人脸比对人脸数检测<br/>图片中检测到的人脸个数，type传值包含`FACECOMPARE`时存在 | 否 | |
| location | int_array | 标识位置信息<br/>type传值包含`OBJECT`且时存在，该数组有四个值，分别代表左上角的坐标和右下角的坐标。例如[207,522,340,567]<br/>207代表的是左上角的x坐标<br/>522代表左上角的y坐标<br/>340代表的是右下角的x坐标<br/>567代表的是右下角的y坐标 | 否 | |
| person_num | int | 人体数量检测<br/>图片中检测到的人体个数type传值包含`PORTRAIT`且时存在 | 否 | |
| person_ratio | float | 人像占比<br/>在区间0-1，数值越大，人脸占比越高type传值包含`PORTRAIT`时存在 | 否 |

# Demo

目前提供了 go、java、lua、nodes、php、python 的 demo，代码位置：
[https://github.com/ishumei/api-demo/tree/master/v4](https://github.com/ishumei/api-demo/tree/master/v4)
