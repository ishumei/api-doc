# 数美智能图片识别产品API接口文档

- - - - -

***版权所有 翻版必究***

- - - - -

目录

- [数美智能图片识别产品API接口文档](#数美智能图片识别产品api接口文档)
- [接入前准备](#接入前准备)
    - [数美服务账号申请](#数美服务账号申请)
    - [渠道配置表](#渠道配置表)
    - [数美服务账号信息接收](#数美服务账号信息接收)
- [智能图片过滤服务单张接口接入说明](#智能图片过滤服务单张接口接入说明)
  - [同步单条请求](#同步单条请求)
    - [请求URL：](#请求url)
    - [请求方法：](#请求方法)
    - [字符编码：](#字符编码)
    - [建议超时时间：](#建议超时时间)
    - [请求参数：](#请求参数)
  - [同步返回结果](#同步返回结果)
  - [回调的同步返回参数](#回调的同步返回参数)
  - [回调请求参数](#回调请求参数)
  - [同步示例：](#同步示例)
    - [同步请求示例：](#同步请求示例)
    - [同步返回示例：](#同步返回示例)
    - [回调请求参数](#回调请求参数-1)
    - [接口返回参数](#接口返回参数)
    - [回调请求示例](#回调请求示例)
- [智能图片过滤服务批量接口说明](#智能图片过滤服务批量接口说明)
  - [同步批量请求](#同步批量请求)
    - [请求URL：](#请求url-1)
    - [请求方法：](#请求方法-1)
    - [字符编码：](#字符编码-1)
    - [建议超时时间：](#建议超时时间-1)
    - [请求参数：](#请求参数-1)
  - [同步返回结果](#同步返回结果-1)
  - [批量接口回调请求参数](#批量接口回调请求参数)
  - [批量请求示例](#批量请求示例)
    - [同步批量接口请求示例](#同步批量接口请求示例)
    - [同步批量接口返回示例](#同步批量接口返回示例)
    - [异步回调批量请求示例](#异步回调批量请求示例)
    - [接口返回示例](#接口返回示例)
    - [回调数据示例](#回调数据示例)
- [智能图片过滤纠错接口](#智能图片过滤纠错接口)
    - [请求URL：](#请求url-2)
    - [请求方法：](#请求方法-2)
    - [字符编码：](#字符编码-2)
    - [建议超时时间：](#建议超时时间-2)
    - [请求参数：](#请求参数-2)
    - [响应参数：](#响应参数)
    - [接口请求示例](#接口请求示例)
    - [接口响应示例](#接口响应示例)
- [Demo](#demo)
- [FAQ](#faq)
  - [图片的大小有限制吗？](#图片的大小有限制吗)
  - [图片分辨率/像素是否有限制？](#图片分辨率像素是否有限制)
  - [批量图片处理最多几张图片？](#批量图片处理最多几张图片)
  - [批量图片的qps定义？](#批量图片的qps定义)
  - [是否可以自定义图片黑白名单，添加后多久可以生效？](#是否可以自定义图片黑白名单添加后多久可以生效)
  - [图片同时命中多种风险类型时，返回哪种结果？](#图片同时命中多种风险类型时返回哪种结果)
  - [测试服务效果欠佳](#测试服务效果欠佳)
  - [调用接口返回参数错误（1902）](#调用接口返回参数错误1902)
  - [调用接口返回无权限操作（9101）](#调用接口返回无权限操作9101)
  - [调用接口超时问题](#调用接口超时问题)

# 接入前准备

### 数美服务账号申请
客户经理已提前与贵公司建立联系或当面拜访，可直接将开通账号及服务相关信息提供至客户经理。
开通账号所需信息包括：

| 参数项 |  |
| --- | --- |
| 公司全称 | xxxx |
| 公司简称 | xxxx |
| 接口人邮箱 | xxxx |
| 接口人手机 | xxxx |

### 渠道配置表
数美根据客户不同业务场景，配置不同的渠道（channel），制定针对性的拦截策略，同时也方便客户针对不同业务场景的数据进行筛选、分析。业务场景和渠道取值对应表如下（支持客户自定义）：

| 业务场景 | channel取值 | 备注 |
| --- | --- | --- |
| 头像 | HEAD_IMG | 用户头像 |
| 相册 | IMGS | 用户相册 |
| 动态 | DYNAMIC | 社交平台的动态配图 |
| 文章配图 | ARTICLE | 博客、文章中的配图 |
| 评论插图 | COMMENT | 评论里面的配图 |
| 封面 | COVER | 相册中的封面或者背景图 |
| 商品图片 | PRODUCT | 电商平台的商品图片 |
| 群聊图片 | GROUP_CHAT | 群聊里面的图片消息 |
| 私聊图片 | MESSAGE | 私聊里面的图片消息 |
| 离线测试 | OFFLINE_TEST | 关闭了画像和行为相关策略，离线测试专用 |

### 数美服务账号信息接收
数美客户经理会在1个工作日内为您开通相应数美账号及服务，随后接口人邮箱会收到如下信息：

| 名称 | 具体值 | 说明 |
| --- | --- | --- |
| accessKey | xxxx | 数美API服务的认证码，调用数美API时需要传入 |
| organization | xxxx | 数美分配的企业唯一标识码，调用SDK时需要传入 |
| 数美管理后台账号 | xxxx | 用于登陆数美管理后台 |
| 数美管理后台密码 | xxxx | 用于登陆数美管理后台 |
| 数美管理后台地址 | https://www.fengkongcloud.com | 用于登陆数美管理后台 |

# 智能图片过滤服务单张接口接入说明

## 同步单条请求

### 请求URL：
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-img-bj.fengkongcloud.com/v2/saas/anti_fraud/img` | 图片 |
| 上海 | `http://api-img-sh.fengkongcloud.com/v2/saas/anti_fraud/img` | 图片 |
| 新加坡 | `http://api-img-xjp.fengkongcloud.com/v2/saas/anti_fraud/img` | 图片 |
| 印度 | `http://api-img-yd.fengkongcloud.com/v2/saas/anti_fraud/img` | 图片 |
| 硅谷 | `http://api-img-gg.fengkongcloud.com//v2/saas/anti_fraud/img` | 图片 |

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
| type | string | 检测的风险类型 | 必传参数 | 监管一级标签<br/>可选值：<br/>`POLITICS`：涉政识别<br/>`PORN`：色情识别<br/>`OCR`：图片中的OCR文字识别<br/>`AD`：广告识别<br/>`BEHAVIOR`：不良场景识别，支持吸烟、喝酒、赌博、吸毒、避孕套和无意义画面<br/>`PERSON`：涉政人脸识别<br/>`VIOLENCE`：暴恐识别<br/>`PORN`：色情识别<br/><br/>多个type通过下划线连接，例如`AD_PORN_POLITICS`用于广告、色情和涉政组合识别<br/>建议传入：`POLITICS_PORN_AD_BEHAVIOR`<br/><br/>注意：这里`POLITICS`实际上等价于以下两个类型：<br/>`PERSON`：涉政人脸识别 <br/>`VIOLENCE`：暴恐识别 <br/>（该字段与`businessType`字段必须选择一个传入）|
| businessType | string | 业务标签类型 | 非必传参数 | 业务标签<br/>可选值：[见附录](#附录)如果需要多个识别功能，通过下划线连接，该字段和type必须选择一个传入 |
| appId | string | 应用标识，用于区分相同公司的不同应用数据 | 必传参数 | 默认应用值：`default`<br/>传递其他值时需联系数美服务协助开通 |
| callback | string | 回调地址 | 非必传参数 | 回调http接口，当该字段非空时，服务将根据该字段回调通知用户审核结果<br/>地址必须为http或https的规范url |
| data | json_object | 请求的数据内容 | 必传参数 | 请求的数据内容，最长10MB，[详见data参数](#data) |
| callbackParam | json_object | 透传参数 | 非必传参数 |  |

其中，data的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| tokenId | string | 用户账号标识，建议使用贵司用户UID（可加密）自行生成 , 标识用户唯一身份用作灌水和广告等行为维度风控。如无用户uid的场景建议使用唯一的数据标识传值 | 必传参数 | 由数字、字母、短杠组成的长度小于等于64位的字符串 |
| img | string | 要检测的图片，可使用base64编码的图片数据或者图片的url链接 **建议图片下载从CDN源站下载，并且源站不能为单点<br/>风险：如果不是从源站下载，可能存在图片下载失败，导致无法审核** | 必传参数 | 支持格式：<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`, `hief`<br/>建议图片像素不小于256\*256, 目前最低支持20\*20分辨率的图片，图片大小最大10MB |
| btId | string | 用户指定的图片标识 | 非必传参数 | 当callback存在时，在回调请求中向用户返回，限制长度为30位 |
| channel | string | 渠道配置 | 非必传参数 | 见渠道配置表 |
| registerTime | int | 帐号注册时间 | 非必传参数 | 建议传递此参数，新注册帐号的异常操作风险较高，毫秒级时间戳 |
| friendNum | int | 帐号好友数 | 非必传参数 | 社交场景强烈推荐传此参数，标识用户质量 |
| fansNum | int | 帐号粉丝数 | 非必传参数 | 直播/社区场景强烈推荐传此参数，标识用户质量 |
| isPremiumUser | int | 是否优质（如付费）用户 | 非必传参数 | 配置不同等级，标识用户质量 1为优质用户，0为默认值 |
| ip | string | 客户端IP | 非必传参数 | 该参数用于IP维度的用户行为分析，同时可用于比对数美IP黑库 |
| receiveTokenId | string | 接收者的tokenId | 非必传参数 | 接收者的tokenId，私聊场景必选 |
| sex | int | 用户的性别 | 非必传参数 | 用户的性别，可选值：<br/>0：女性<br/> 1：男性 |
| age | int | 用户的年龄 | 非必传参数 | 用户的年龄，可选值：<br/>0：青年（大约18-45岁）<br/>1：中年（大约45-60岁）<br/>2：老年（大于60岁） |
| level | int | 用户等级 | 非必传参数 | 用户等级，针对不同等级的用户可配置不同拦截策略 |
| role | string | 用户角色 | 非必传参数 | 对不同角色可配置不同策略。<br/>直播领域"ADMIN"表示房管，"HOST"表示主播, "SYSTEM"系统角色, 游戏领域"ADMIN"表示管理员，"USER"表示普通用户，缺失或者"USER"默认普通用户 |
| topic | string | 讨论的话题编号 | 非必传参数 | 可为书评区编号、论坛帖子编号 |
| phone | string | 用户手机号 | 非必传参数 | 用户手机号，可用于比对数美手机号黑库 |
| deviceId | string | 数美设备指纹标识 | 非必传参数 | 强烈建议传入，数美设备指纹标识，用于用户行为分析。当恶意用户篡改mac、imei等设备信息时，使用deviceId能够发现和识别此类恶意行为，同时可用于比对数美设备指纹黑名单 |
| imei | string | 用户android设备唯一标识 | 非必传参数 | 相比tokenId和IP，imei和mac更难被更换，当恶意用户使用多个不同账户和IP进作恶时，通过imei和mac能够有效关联识别此类恶意行为，同时可用于比对数美设备黑名单。 |
| mac | string | 用户android设备唯一标识 | 非必传参数 | 同imei字段 |
| idfv | string | 用户iOS应用唯一标识 | 非必传参数 | 相比tokenId和IP，idfv不能被修改，当恶意用户使用多个不同账户和IP进行恶意行为时，使用idfv能够发现和识别此类恶意行为 |
| idfa | string | 用户iOS应用唯一标识 | 非必传参数 | 同idfv字段 |
| maxFrame | int | gif最大截帧数量 | 非必传参数 | 最大截帧数量，GIF图检测专用，默认值为3，最大值为20。 |
| interval | int | gif截帧频率 | 非必传参数 | GIF图检测专用，默认值为1。每interval张图片抽取一张进行检测。<br/>当interval*maxFrame小于该图片所包含的图片数量时，截帧间隔会自动修改为该图片所包含的图片数/maxFrame，以提高整体检测效果。 |
| isTokenSeparate | int | 是否区分不同应用下的账号 | 非必传参数 | 是否区分不同应用下的账号，可能取值：<br/>0:不区分<br/>1:区分<br/>默认值为0。<br/>取值为1时不同应用下的账号体系各自独立，账号相关的策略特征在不同应用下单独统计和生效。 |
| passThrough | json_object | 透传参数 | 非必传参数 | 客户传入透传字段，数美内部不回对该字段进行识别处理，随结果返回给用户，必须为json_object类型 |

## 同步返回结果

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | [详见code与message对应关系](#code-message) |
| message | string | 返回码描述 | 是 | 和code对应：成功QPS超限参数不合法服务失败余额不足无权限操作 |
| requestId | string | 请求标识 | 是 | 请求唯一标识，唯一标识该次图片审核任务 |
| taskId | string | 任务编号 | 是 | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| btId | string | 用户上传的图片标识 | 否 | 用户请求的图片标识（当请求中传入btId时存在）|
| score | int | 风险分数 | 否 | 风险分数（callback不存在或者为空并且code为1100时存在）取值范围[0,1000]，分数越高风险越大 |
| riskLevel | string | 风险级别 | 否 | 风险级别（callback不存在或者为空并且code为1100时存在）可能返回值：PASS，REVIEW，REJECT<br/>PASS：正常内容，建议直接放行<br/>REVIEW：可疑内容，建议人工审核<br/>REJECT：违规内容，建议直接拦截 |
| status | int | 提示服务是否超时 | 是 | 提示服务是否超时<br/>0：正常<br/>501：超时 |
| detail | json_object | 风险详情 | 否 | [详见detail参数](#detail) |
| businessLabels | json_object | 业务标签 | 否 | 业务标签结果，仅在传入参数businessType不为空时存在，[详见businessLabels结构](#businessLabels) |

其中，code和message的列表如下：

| **code** | **message** |
| --- | --- |
| 1100 | 成功 |
| 1901 | QPS超限 |
| 1902 | 参数不合法 |
| 1903 | 服务失败 |
| 1911 | 下载超时 |
| 9100 | 余额不足 |
| 9101 | 无权限操作 |

其中，detail结构如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| riskType | int | 风险类型 | 是 | 标识风险类型，[详见riskType取值](#vision-riskType) |
| riskSource | int | 风险结果来源 | 是 | 标识风险结果的来源：<br/>1000：无风险 <br/>1001：文字风险 <br/>1002：视觉图片风险 |
| model | string | 策略规则标识 | 是 | 用来标识命中的策略规则。<br/>注：该参数为旧版API返回参数，兼容保留，后续版本会取消，请勿依赖此参数，仅供参考 |
| description | string | 拦截的风险原因解释 | 是 | 仅供人了解风险原因时作为参考，程序请勿依赖该参数的值做逻辑处理 |
| descriptionV2 | string | 新版策略规则风险原因描述 | 否 | 该参数为新版API返回参数，过渡阶段只有新策略才会返回 |
| hits | json_array | 命中的策略集 | 是 | |
| text | string | OCR识别出的文字 | 否 | OCR识别出的文字，可根据需求返回该参数 |
| matchedItem | string | 命中的具体敏感词 | 否 | 命中的具体敏感词（该参数仅在命中敏感词时存在），可根据需求返回该参数 |
| matchedList | string | 命中敏感词所在的名单名称 | 否 | 命中敏感词所在的名单名称（该参数仅在命中敏感词时存在），可根据需求返回该参数 |
| matchedDetail | string | 命中的敏感词详细信息 | 否 | 命中的敏感词详细信息，可以反序列化为json_array，可根据需求返回该参数 |
| qrcontent | string | 二维码内容信息 | 否 | 二维码内容信息，可根据需求返回该参数，需要和数美协商开通 |
| pornLabel | string | 色情识别标签 | 否 | 色情识别标签，标识色情识别结果；<br/>开启色情识别后可根据需求选择是否返回该参数；<br/>可选值："色情"、"性感"、"正常" |
| pornRate | float | 色情图片概率 | 否 | 色情图片概率，可根据需求返回该参数 |
| sexyRate | float | 性感图片概率 | 否 | 性感图片概率，可根据需求返回该参数 |
| normalRate | float | 正常图片概率 | 否 | 正常图片概率，可根据需求返回该参数 |
| polityRate | float | 最相似的涉政人物概率 | 否 | 最相似的涉政人物概率，可根据需求返回该参数 |
| violenceLabel | string | 暴恐识别标签 | 否 | 暴恐识别标签，标识暴恐识别结果；开启暴恐识别后可根据需求选择是否返回该参数；可选值："暴乱场景"、"国旗国徽"、"军装"、"恐怖组织"、"枪支刀具"、"血腥场景"、”游戏枪支刀具”、"中国地图"、"坦克"、"蜡烛"、"制服"、"正常"  |
| rebelRate | float | 暴乱场景概率 | 否 | 暴乱场景概率，可根据需求返回该参数 |
| flagRate | float | 国旗国徽概率 | 否 | 国旗国徽概率，可根据需求返回该参数 |
| armyRate | float | 军装概率 | 否 | 军装概率，可根据需求返回该参数 |
| terrorismRate | float | 恐怖组织概率 | 否 | 恐怖组织概率，可根据需求返回该参数 |
| weaponRate | float | 枪支刀具概率 | 否 | 枪支刀具概率，可根据需求返回该参数 |
| bloodRate | float | 血腥场景概率 | 否 | 血腥场景概率，可根据需求返回该参数 |
| gameWeaponRate | float | 游戏枪支刀具概率 | 否 | 游戏枪支刀具概率，可根据需求返回该参数 |
| chinamapRate | float | 中国地图概率 | 否 | 中国地图概率，可根据需求返回该参数 |
| tankRate | float | 坦克概率 | 否 | 坦克概率，可根据需求返回该参数 |
| candleRate | float | 蜡烛概率 | 否 | 蜡烛概率，可根据需求返回该参数 |
| uniformRate | float | 制服概率 | 否 | 制服概率，可根据需求返回该参数 |
| nonViolenceRate | float | 非暴恐图片概率 | 否 | 非暴恐图片概率，可根据需求返回该参数 |
| segments | int | 实际处理的片段数量 | 否 | 实际处理的片段数量，当检测的图片为GIF图或长图时，会返回该参数 |
| logos | json_array | 图片logo结果 | 否 | 返回图片识别出来的logo结果 |
| passThrough | json_object | 用户透传字段 | 否 | 该字段是客户传入透传字段 |

其中，businessLabels结构如下：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | 一级业务标签 | 是 | |
| businessLabel2 | string | 二级业务标签 | 是 | |
| businessLabel3 | string | 三级业务标签 | 是 | |
| businessDescription | string | 业务标签中文描述 | 是 | |

视觉riskType取值如下：

| **风险类型** | **code码** |
| --- | --- |
| 正常 | 0 |
| 涉政 | 100 |
| 色情 | 200 |
| 性感 | 210 |
| 广告 | 300 |
| 二维码 | 310 |
| 水印 | 320 |
| 暴恐 | 400 |
| 违规 | 500 |
| 不良场景 | 510 |
| 未成年人 | 520 |
| 人脸 | 530 |
| 人像 | 531 |
| 颜值 | 533 |
| 人脸比对 | 534 |
| 公众人物 | 535 |
| 物品 | 540 |
| 动物 | 541 |
| 植物 | 542 |
| 场景 | 550 |
| 行业违规 | 560 |
| 画面属性 | 570 |
| 黑名单 | 700 |
| 白名单 | 710 |
| 高危账号 | 800 |
| 自定义 | 900 |

文本riskType取值如下：

| **风险类型** | **code码** |
| --- | --- |
| 正常 | 0 |
| 涉政 | 100 |
| 色情 | 200 |
| 辱骂 | 210 |
| 广告 | 300 |
| 灌水 | 400 |
| 无意义 | 500 |
| 违禁 | 600  |
| 其他 | 700 |
| 黑账号 | 720 |
| 黑IP | 730 |
| 高危账号 | 800 |
| 自定义 | 900 |

## 回调的同步返回参数

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | [详见code与message对应关系](#code-message) |
| message | string | 返回码描述 | 是 | 和code对应：成功/QPS超限/参数不合法/服务失败/余额不足/无权限操作 |
| requestId | string | 请求标识 | 是 | 请求唯一标识，唯一标识该次图片审核任务 |
| taskId | string | 任务编号 | 是 | 可能返回值：<br/>`PASS`：正常，建议直接放行<br/>`REVIEW`：可疑，建议人工审核<br/>`REJECT`：违规，建议直接拦截 |
| btId | string | 用户上传的图片标识 | 否 | 用户请求的图片标识（当请求中传入btId时存在）|

如果在请求参数中指定了回调协议接口URL callback，则需要支持POST方法，传输编码采用utf-8，审核结果放在HTTP Body中，采用Json格式，具体参数和V2单张同步请求结果相同。

## 回调请求参数

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| checksum | string | 校验和 | 是 | 由accessKey + btId + result拼成字符串，（当btId不存在时，由accessKey + result生成）通过SHA256算法生成。为防止篡改，可以按此算法生成字符串，与checksum做一次校验。 |
| result | string | 审核结果 | 是 | 返回结果与同步结构一致, [详见单张接口同步返回结果](#syncResponseParameters) |

## 同步示例：

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
  "code":1100,
  "message":"成功",
  "requestId":"test-asfasfasfsgsdfasfda",
  "taskId":"a82a4413-1a9be262-96b6d383-3c833309",
  "btId":"12",
  "score":850,
  "riskLevel":"REJECT",
  "detail":{
    "description":"二维码：微信二维码：微信二维码",
    "descriptionV2":"二维码：微信二维码：微信二维码",
    "hits":[
      {
        "description":"广告：联系方式：联系方式",
        "descriptionV2":"广告：联系方式：联系方式",
        "model":"MA001020002001004",
        "riskLevel":"REJECT",
        "riskType":300,
        "score":675
      },
      {
        "description":"二维码",
        "descriptionV2":"二维码：二维码：二维码",
        "model":"MA001018001001001",
        "riskLevel":"REJECT",
        "riskType":310,
        "score":700
      },
      {
        "description":"二维码：微信二维码：微信二维码",
        "descriptionV2":"二维码：微信二维码：微信二维码",
        "model":"MA001018002001001",
        "riskLevel":"REJECT",
        "riskType":310,
        "score":850
      }
    ],
    "model":"MA001018002001001",
    "original_text":"黑白印象0!度門，福建掃描上面的 QR Code 加我 Wechat",
    "original_text_context":"黑白印象0!度門，福建掃描上面的 QR Code 加我 Wechat",
    "qrcontent":"https://u.wechat.com/MFdBUI_k8OfRvWji45gggKO8",
    "riskSource":1002,
    "riskType":310,
    "sexy_risk_tokenid":0,
    "text":"黑白印象0!度門，福建掃描上面的 QR Code 加我 Wechat"
  },
  "status":0
}
```
### 回调请求参数
```json
{
  "accessKey":"yourkey",
  "type":"POLITICS_PORN_AD",
  "callback":"http://xxx",
  "data":{
    "img":"http://www.leilingfushi.com/UpFiles/Article/2017/5/11/2017051152012237.jpg",
    "tokenId":"test",
    "btId":"xxx"
  }
}
```

### 接口返回参数
```json
{
  "code":1100,
  "message":"成功",
  "requestId":"dgdfh7sd4dsd3s22223wexsdsd",
  "taskId":"fc250d49-fc9459b2-0e4c12c9-3618a292",
  "btId":"xxx"
}
```
当用户的服务端收到推送结果，并返回HTTP状态码为200时，表示推送成功，否则系统将进行重试推送（直至达到重试次数上限）
### 回调请求示例
```json
{
    "checksum":"236f8eea85c3c4407d96ff05d6108389b3b0cea8aa80bdf6642c1cecc77b2bde",
    "result":{
    "code":1100,
    "message":"成功",
    "requestId":"1e6e4e43cd35b545418fcef7d0f77ef4",
    "taskId":"5ba3efe0-949ccac9-4e9ba8b2-31f84bdd",
    "score":999,
    "riskLevel":"REJECT",
    "detail":{
        "description":"涉政文字",
        "hits":[

        ],
        "matchedItem":"xxx",
        "matchedList":"test",
        "model":"M02601",
        "polityName":"xxx",
        "riskType":100,
        "riskSource":1002
    },
    "status":0,
    "callbackParam":{
        " param1":1,
        " param2":"qew",
        " param3":true
       }
    }
}
```

# 智能图片过滤服务批量接口说明

## 同步批量请求

### 请求URL：
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-img-bj.fengkongcloud.com/v2/saas/anti_fraud/imgs` | 图片 |
| 上海 | `http://api-img-sh.fengkongcloud.com/v2/saas/anti_fraud/imgs` | 图片 |
| 新加坡 | `http://api-img-xjp.fengkongcloud.com/v2/saas/anti_fraud/imgs` | 图片 |
| 印度 | `http://api-img-yd.fengkongcloud.com/v2/saas/anti_fraud/imgs` | 图片 |
| 硅谷 | `http://api-img-gg.fengkongcloud.com//v2/saas/anti_fraud/imgs` | 图片 |

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
| accessKey | string | 接口认证密钥<br/>用于权限认证，开通账号服务时由数美提供或使用开通邮箱登录数美后台右上角相关文档处查看 | 必传参数 | accessKey |
| type | string | 检测的风险类型 | 必传参数 | 监管一级标签<br/>可选值：<br/>`POLITICS`：涉政识别<br/>`PORN`：色情识别<br/>`OCR`：图片中的OCR文字识别<br/>`AD`：广告识别<br/>`BEHAVIOR`：不良场景识别，支持吸烟、喝酒、赌博、吸毒、避孕套和无意义画面<br/>`PERSON`：涉政人脸识别<br/>`VIOLENCE`：暴恐识别<br/>`PORN`：色情识别<br/><br/>多个type通过下划线连接，例如`AD_PORN_POLITICS`用于广告、色情和涉政组合识别<br/>建议传入：`POLITICS_PORN_AD_BEHAVIOR`<br/><br/>注意：这里`POLITICS`实际上等价于以下两个类型：<br/>`PERSON`：涉政人脸识别 <br/>`VIOLENCE`：暴恐识别 <br/>（该字段与`businessType`字段必须选择一个传入）|
| businessType | string | 业务标签类型 | 非必传参数 | 业务标签<br/>可选值：[见附录](#附录)如果需要多个识别功能，通过下划线连接，该字段和type必须选择一个传入 |
| appId | string | 应用标识，用于区分相同公司的不同应用数据 | 必传参数 | 默认应用值：`default`<br/>传递其他值时需联系数美服务协助开通 |
| callback | string | 回调地址 | 非必传参数 | 回调http接口，当该字段非空时，服务将根据该字段回调通知用户审核结果<br/>地址必须为http或https的规范url |
| data | json_object | 请求的数据内容 | 必传参数 | 请求的数据内容，最长10MB，[详见data参数](#data) |
| callbackParam | json_object | 透传参数 | 非必传参数 | 透传字段，当 callback 存在时可选，发送回调请求时服务将该字段内容同识别结果一起返回 |

其中，批量接口data的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| tokenId | string | 用户账号标识 | 必传参数 | 建议使用贵司用户UID（可加密）自行生成 , 标识用户唯一身份用作灌水和广告等行为维度风控。如无用户uid的场景建议使用唯一的数据标识传值 |
| imgs | json_array | 要检测的图片数组 | 必传参数 | 要检测的图片数组，要求数组长度在12以内 |
| channel | string | 渠道标识 | 非必传参数 | 业务渠道标识，用于区分不同业务渠道的图片内容（如头像、发帖、私信等），同时对不同业务渠道可配置不同的策略规则，该参数传递值可与数美服务协商，在数美管理后台进行配置，不传递则默认值为IMAGE默认图片 |
| role | string | 用户角色 | 非必传参数 | 用户角色，必须在可选范围有效对不同角色可配置不同策略。(默认为`USER`)<br/>直播领域可取值：<br/>`ADMIN`：房管<br/>`HOST`：主播<br/>`SYSTEM`：系统角色<br/>游戏领域可取值：<br/>`ADMIN`：管理员<br/>`USER`：普通用户 |
| ip | string | ip地址 | 非必传参数 | 发送该图片的用户公网ipv4地址 |
| phone | string | 用户手机号 | 非必传参数 | 用户手机号，可用于比对数美手机号黑库 |
| deviceId | string | 数美设备指纹标识 | 非必传参数 | 强烈建议传入，数美设备指纹标识，用于用户行为分析。当恶意用户篡改mac、imei等设备信息时，使用deviceId能够发现和识别此类恶意行为，同时可用于比对数美设备指纹黑名单 |
| imei | string | 用户android设备唯一标识 | 非必传参数 | 相比tokenId和IP，imei和mac更难被更换，当恶意用户使用多个不同账户和IP进作恶时，通过imei和mac能够有效关联识别此类恶意行为，同时可用于比对数美设备黑名单。 |
| mac | string | 用户android设备唯一标识 | 非必传参数 | 同imei字段 |
| idfv | string | 用户iOS应用唯一标识 | 非必传参数 | 相比tokenId和IP，idfv不能被修改，当恶意用户使用多个不同账户和IP进行恶意行为时，使用idfv能够发现和识别此类恶意行为 |
| idfa | string | 用户iOS应用唯一标识 | 非必传参数 | 同idfv字段 |
| sex | int | 用户的性别 | 非必传参数 | 用户的性别，可选值：<br/>0：女性<br/> 1：男性 |
| age | int | 用户的年龄 | 非必传参数 | 用户的年龄，可选值：<br/>0：青年（大约18-45岁）<br/>1：中年（大约45-60岁）<br/>2：老年（大于60岁） |
| level | int | 用户等级 | 非必传参数 | 用户等级，针对不同等级的用户可配置不同拦截策略 |
| maxFrame | int | gif最大截帧数量 | 非必传参数 | 最大截帧数量，GIF图检测专用，默认值为3，最大值为20。 |
| interval | int | gif截帧频率 | 非必传参数 | GIF图检测专用，默认值为1。每interval张图片抽取一张进行检测。<br/>当interval*maxFrame小于该图片所包含的图片数量时，截帧间隔会自动修改为该图片所包含的图片数/maxFrame，以提高整体检测效果。 |
| isTokenSeparate | int | 是否区分不同应用下的账号 | 非必传参数 | 是否区分不同应用下的账号，可能取值：<br/>0:不区分<br/>1:区分<br/>默认值为0。<br/>取值为1时不同应用下的账号体系各自独立，账号相关的策略特征在不同应用下单独统计和生效。 |
| passThrough | json_object | 透传参数 | 非必传参数 | 客户传入透传字段，数美内部不回对该字段进行识别处理，随结果返回给用户，必须为json_object类型 |

其中，imgs数组每个成员的具体内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| img | string | 要检测的图片 | 必传参数 | 要检测的图片，可使用图片的base64编码或者图片的url链接 <br/>建议图片下载从CDN源站下载，并且源站不能为单点<br/>风险：如果不是从源站下载，可能存在图片下载失败，导致无法审核<br/>支持格式：`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`, `hief`<br/>建议图片像素不小于256*256，目前最低支持20*20分辨率的图片 |
| btId | string | 图片唯一标识 | 必传参数 | 图片唯一标识，同一次请求中，图片标识不可重复，不可传入特殊字符 |

## 同步返回结果

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1911`：图片下载失败<br/>`9101`：无权限操作<br/>除message和requestId之外的字段，只有当code为1100时才会存在 |
| message | string | 返回码描述 | 是 | 和code对应：成功QPS超限参数不合法服务失败余额不足无权限操作 |
| requestId | string | 请求标识 | 是 | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存 |
| imgs | json_array | 检测结果 | 否 | 多张图片的识别结果（code为1100时存在） |
| statistics | int_array | 整形数组 | 否 | 整形数组，长度为4，分别表示一次批量图片请求中拒绝数、审核数、通过数（code为1100时存在）和错误数 |

其中，imgs数组每个成员的具体内容如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| btId | string | 图片唯一标识 | 是 | 图片唯一标识，该参数在发起请求时传入，同一次请求中图片标识不可重复，不可传入特殊字符 |
| requestId | string | 请求唯一标识 | 是 | 请求唯一标识，后续可用于数据查询，调用纠错接口时使用此requestId |
| score | int | 风险分数 | 否 | 风险分数（callback不存在或者为空并且code为1100时存在）取值范围[0,1000]，分数越高风险越大 |
| riskLevel | string | 图片唯一标识 | 否 | 风险级别（callback不存在或者为空并且code为1100时存在）<br/>可能返回值：PASS，REVIEW，REJECT<br/>PASS：正常内容，建议直接放行<br/>REVIEW：可疑内容，建议人工审核<br/>REJECT：违规内容，建议直接拦截 |
| detail | json_object | 风险详情 | 否 | 风险详情（callback不存在或者为空并且code为1100时存在）[详见detail参数](#detail) |
| businessLabels | json_object | 业务标签结果 | 否 |  业务标签结果，仅在传入参数businessType不为空时存在，[详见businessLabels结构](#businessLabels) |
| code | int | 返回码 | 是 | [详见code与message对应关系](#code-message) |
| message | string | 返回码详情描述 | 是 | 返回码详情描述 |

## 批量接口回调请求参数
**对于批量接口，系统为每张图片执行一次推送任务，每次推送机制及参数要求与上述单个图片回调请求一致。**

## 批量请求示例
### 同步批量接口请求示例
```
curl –d '{"accessKey":"xxxxx","type":"XX","businessType":"XX","data":{"imgs":[{"img":"xxxx","btId":"xxxx"}],"tokenId":"username"}}' 'http://api-img-bj.fengkongcloud.com/v2/saas/anti_fraud/imgs'
```

### 同步批量接口返回示例
```json
{
  "code":1100,
  "message":"成功",
  "requestId":"test",
  "imgs":[
    {
      "btId":"test",
      "code":1100,
      "detail":{
        "description":"涉政：敏感政治事件：六四事件",
        "descriptionV2":"涉政：敏感政治事件：六四事件",
        "hits":[
          {
            "description":"涉政：敏感政治事件：六四事件",
            "descriptionV2":"涉政：敏感政治事件：六四事件",
            "model":"MA001001007006003",
            "riskLevel":"REJECT",
            "riskType":100,
            "score":850
          }
        ],
        "model":"MA001001007006003",
        "riskSource":1002,
        "riskType":100
      },
      "message":"成功",
      "requestId":"test_bdcc36bb9d6ee6a060b051176d53fa",
      "riskLevel":"REJECT",
      "score":850
    }
  ],
  "statistics":[
    1,
    0,
    0,
    0
  ],
  "passThrough":{
    "my_age":9
  }
}
```

### 异步回调批量请求示例
```
curl –d '{"accessKey":"xxxxx","type":"XX","callback":"http://xxx","data":{"imgs":[{"img":"xxxx","btId":"xxxx"}],"tokenId":"username"}}' 'http://api-img-bj.fengkongcloud.com/v2/saas/anti_fraud/imgs'
```

### 接口返回示例
```json
{
    "code":1100,
    "message":"成功",
    "requestId":"1859599420fd4c32e02841f1d8ae9429",
    "statistics":[
        0,
        0,
        2
    ]
}
```
### 回调数据示例
同单张接口的返回数据格式一致

# 智能图片过滤纠错接口
客户使用数美服务时发现结果有误，可以通过此接口给数美反馈错误信息

### 请求URL：
https://webapi.fengkongcloud.com/saas/feedback/add/v1

### 请求方法：

`POST`

### 字符编码：

`UTF-8`

### 建议超时时间：

1s

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥 | 必传参数 | 由数美提供 |
| serviceId | string | 服务标识 | 必传参数 | 服务标识，可选值：`POST_TEXT`,`POST_IMG`,`POST_AUDIO`,`POST_AUDIOSTREAM`,`POST_VIDEO_IMG`,`POST_VIDEO_AUDIO`,`POST_VIDEOSTREAM_IMG`,`POST_VIDEOSTREAM_AUDIO`,`POST_EVENT`,`ACCOUNT_LOGIN`,`ACCOUNT_REGISTER` |
| requestId | string | 流水号 | 必传参数 | |
| timestamp | int | 当条记录13位时间戳 | 必传参数 |  |
| riskLevel | string | 期望处置结果 | 必传参数 | 期望处置结果，可选值：`PASS`,`REJECT`|
| type | string | 错误类别 | 必传参数 | 误杀、漏杀类型，误杀传error,漏杀传miss |
| text | string | 文本内容 | 非必传参数 | 文本内容，会加入黑白名单 |
| imgUrl | string | 图片url | 非必传参数 | 图片url，会加入黑白名单 |
| tokenId | string | 账号标识 | 非必传参数 | 账号标识，文本、图片、天网服务会加黑白名单 |
| labelId | string | 一级标签英文标识 | 非必传参数 | 文本、图片服务各自对应的一级标签英文标识，参考下方表格 |

其中，labelId的列表如下：

| **风险类型** | **说明** |
| --- | --- |
| politics | 涉政 |
| porn | 色情 |
| sexy | 性感 |
| ad | 广告 |
| violence | 暴恐 |
| ban | 违禁 |
| logo | logo |
| qr | 二维码 |
| socialFace | 社交人脸 |
| minor | 未成年人 |
| star | 公众人物 |

### 响应参数：

| **返回结果参数名** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 状态码 | 是 | 状态码，1100成功，其他失败 |
| message | string | 提示语 | 是 | |
| feedbackRes | boolean | 反馈结果 | 是 | |
| listRes | boolean | 名单反馈结果 | 是 | |
| profileRes | boolean | 画像反馈结果 | 是 | |

### 接口请求示例
```json
{
  "accessKey":"xxxx",
  "serviceId":"POST_IMG",
  "requestId":"3fe2d8ba7d4b290ee8baaf336110fe8y",
  "timestamp":1554345195048,
  "riskLevel":"REJECT",
  "type":"miss",
  "imgUrl":"http://www.fengkongcloud.com/xxx.jpg",
  "labelId":"ad"
}

```
### 接口响应示例
```json
{
  "code": 1100,
  "message": "成功",
  "feedbackRes": true,
  "listRes": false,
  "profileRes": true
}
```

# Demo

目前提供了 go、java、lua、nodes、php、python 的 demo，代码位置：
[https://github.com/ishumei/api-demo/tree/master/v4](https://github.com/ishumei/api-demo/tree/master/v4)

# 附录


| 业务标签识别类型 | 类型说明 | 备注 |
| ------------- | ------------- | --------------- |
| AGE           | 人脸 - 年龄   |  |
| GENDER        | 人脸 -性别   |  |
| BEAUTY        | 人脸 - 颜值   |  |
| FACEDETECTION | 人脸-人脸检测 | 如识别真人、口罩人脸、正脸、侧脸等 |
| FAKEFACE | 人脸 - 伪造人脸 |  |
| FACECOMPARE | 人脸-人脸对比 |  |
| PUBLICFIGURE | 人物  - 公众人物 | 如识别知名明星、网红等 |
| TAINTEDSTAR | 人物 - 劣迹人物 |  |
| POSTURE | 人像-人像姿态 | 如识别坐姿、跪姿等 |
| DRESS | 人像 - 人像穿着 | 如识别jk、汉服等 |
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
| AUTOMOBILELOGO | LOGO - 汽车品牌 |  |
| 3CPRODUCTSLOGO | LOGO - 3C电子类品牌 |  |
| SHOPPINGAPPSLOGO | LOGO - 购物比价类应用 |  |
| RETOUCHAPPSLOGO | LOGO - 拍摄美化类应用 | 如识别快剪辑、秒拍等LOGO |
| SOCIALAPPSLOGO | LOGO - 社交通讯类应用 | 如识别微博、小红书等LOGO |
| PHOTOMATERIALLOGO | LOGO - 素材版权类应用 |  |
| NEWSAPPSLOGO | LOGO - 新闻阅读类应用 | 如识别新浪、视觉中国等LOGO |
| ENTERTAINMENTAPPSLOGO | LOGO - 影音娱乐类应用 | 如识别抖音、快手等LOGO |
| SPORTSLOGO | LOGO  - 体育赛事 |  |
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


# FAQ

## 图片的大小有限制吗？
答：图片大小要小于10MB。

## 图片分辨率/像素是否有限制？
答：答：强烈建议图片像素不小于256*256，最小不低于20*20像素，图片不清晰会影响识别准确率。

## 批量图片处理最多几张图片？
答：最大支持12张，总大小不超过10MB，这两个因素是同时限制的。

## 批量图片的qps定义？
答：批量图片的qps是根据图片张数计算的，比如：1s打3个请求，每个请求带20张图片，则qps为3*20=60。

## 是否可以自定义图片黑白名单，添加后多久可以生效？
答：数美支持自定义图片黑白名单功能，客户可通过登录数美后台管理界面进行相应操作，添加完成后1-3分钟即可生效，最多不会超过3分钟。

## 图片同时命中多种风险类型时，返回哪种结果？
答：会返回风险概率高的结果。例如一张图片即包含色情，也包含广告二维码，则会根据模型计算出的色情概率和二维码概率，选择较高的作为最终返回的结果。

## 测试服务效果欠佳
答：首先明确双方是否沟通过拦截标准，数美需根据不同平台客户的需求来针对性的制定策略。即使双方沟通过拦截标准，也可能存在理解上和细节上的差异，需对不明确的点进行确认。同时可提供badcase给数美进行分析、策略调优，数美也会主动提供评测报告，看拦截标准和效果是否与客户预期方向一致。最后，测试和效果调优是一个不断迭代的过程，通常需要1-2周的测试调优期，需看最终效果。

## 调用接口返回参数错误（1902）
答：调用数美接口时，code返回1902参数不合法，一般为客户输入的参数格式存在问题，客户可自行分析一下请求格式是否按照接口文档输入，或将请求的数据及返回数据反馈给数美分析解决。

## 调用接口返回无权限操作（9101）
答：调用数美接口时，code返回9101无权限操作，一般为调用了未开通的服务，沟通确认客户调用的服务接口，开通相应的服务。

## 调用接口超时问题
答：有如下两个常见问题：<br/>
1）DNS问题:<br/>
客户通过公网调用数美接口进行测试，客户DNS解析域名较慢，导致第一次请求超时，建议客户更换DNS，不建议客户在host中将域名和ip做绑定，数美更换接口IP导致无法请求接口。<br/>
2）网络问题:<br/>
客户通过公网调用数美接口，公网网络延迟较长，导致少量请求存在超时。可以建议客户ping数美不同的集群网络，建议客户接入网络延迟较低的数美集群。<br/>
