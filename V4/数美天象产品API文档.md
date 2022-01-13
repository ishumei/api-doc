# 数美天象产品API接口说明文档

- - - - -

***版权所有 翻版必究***

- - - - -



* [天象风险识别](#riskDiscernInterface)
    + [调用时机](#requestParameter)
    + [具体接口](#requestInterface)
        - [请求URL](#requestUrl)
        - [字符编码格式](#requestEncode)
        - [请求方法](#requestMethod)
        - [建议超时时长](#requestTimeout)
        - [请求参数](#requestParameters)
    + [返回结果](#response)
    + [示例](#example)
        - [请求示例](#requestExample)
        - [返回示例](#responseExample)


# <span id = "riskDiscernInterface">天象风险识别</span>



## <span id = "requestParameter">调用时机</span>

在需要进行风险识别时调用本接口，例如：

1. 新用户注册后，进行风险画像查询，用于新人红包、注册礼物等的差异化投放；
2. 存量用户登录后，进行风险画像查询，用于日常活动运营的差异化投放；
3. 活动奖励下发前，进行风险画像查询，例如随机红包、抽奖等概率性活动；
4. 每日的例行查询，进行画像库或风控引擎的数据更新。



## <span id = "requestInterface">具体接口</span>

### <span id = "requestUrl">请求URL：</span>
| 集群 | URL | 支持产品列表 |
| --- | --- | --- |
| 北京 | `http://api-tianxiang-bj.fengkongcloud.com/tianxiang/v4` | 天象风险识别 |
| 新加坡 | `http://api-tianxiang-xjp.fengkongcloud.com/tianxiang/v4` | 天象风险识别 |

### <span id = "requestMethod">请求方法：</span>

`POST` 

### <span id = "requestEncode">字符编码：</span>

`UTF-8`

### <span id = "requestTimeout">建议超时时间：</span>

1s

### <span id = "requestParameters">请求参数：</span>

放在HTTP Body中，采用Json格式，具体参数如下：
| **请求参数名** | **类型** | **参数说明** | **传入说明** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥<br>用于权限认证，开通账号服务时由数美提供或使用开通邮箱登录数美后台右上角相关文档处查看 | 必传参数 | 数美分配 |
| data | json_object | 请求的数据内容 | 必传参数 | 请求的数据内容，最长10MB，[详见data参数](#data) |

<span id = "data">其中，data的内容如下：</span>

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| tokenId | string | 用户账号标识，建议使用贵司用户UID（可加密）自行生成 , 标识用户唯一身份用作灌水和广告等行为维度风控。如无用户uid的场景建议使用唯一的数据标识传值 | 不能同时为空 | 该ID为用户的唯一标识，且与其它数美接口的tokenId保持一致 |
| deviceId | string | 待查询的设备指纹ID，由数美SDK生成 | 不能同时为空 | 该ID为用户设备的唯一标识，且与其他数美接口的smid保持一致 |

## <span id = "response">返回结果</span>

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是 |  `1100`：成功<br>`1901`：QPS超限<br>`1902`：参数不合法<br>`1903`：服务失败<br>`1911`：图片下载失败<br>`9101`：无权限操作<br>`3000`：运营商错误<br>除message和requestId之外的字段，只有当code为1100时才会存在 |
| message | string | 返回码描述 | 是 | 和code对应：`成功 QPS超限 参数不合法 服务失败 余额不足 无权限操作` |
| requestId | string | 请求标识 | 是 | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存 |
| profileExist       | int          | 设备画像存在 | 是 | `1`：画像库中存在该设备信息`0`：画像库中不存在该设备信息    |
| tokenLabels        | json_object  | 账号标签信息 | 否 | 见下面详情内容，仅在tokenId传入且服务开通时返回 |
| deviceLabels       | json_object  | 设备标签信息 | 否 | 见下面详情内容，仅在deviceId传入且服务开通时返回 |
| tokenProfileLabels | json_array   | 账号属性标签 | 否 | 见下面详情内容，仅在tokenId传入且服务开通时返回 |
| tokenRiskLabels    | json_array   | 账号风险标签 | 否 | 见下面详情内容，仅在tokenId传入且服务开通时返回 |
| deviceRiskLabels   | json_array   | 设备风险标签 | 否 | 见下面详情内容，仅在deviceId传入且服务开通时返回 |

其中

**1）tokenLabels的详情内容**

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

 **2）deviceLabels结果的详情内容**

| ***返回结果参数名*** | ***参数类型*** | ***参数说明***         | ***规范***         |
| ------------------------ | ------------------ | -------------------------- | ---------------------- |
| id                       | string             | 设备标识                   |                        |
| last_active_ts           | int                | 设备数据最后一次上传时间   | 可作为标签最后更新时间 |
| fake_device              | json_object        | 虚假信息相关的标签信息     |                        |
| device_suspicious_labels | json_object        | 设备可疑相关的标签信息     |                        |
| device_active_info       | json_object        | 设备活跃相关的标签信息     |                        |
| monkey_device            | Json_object        | 机器操控设备相关的标签信息 |                        |

 

fake_device的详情内容如下：

| ***返回结果参数名***      | ***参数类型*** | ***参数说明***       | ***规范***                                               |
| ----------------------------- | ------------------ | ------------------------ | ------------------------------------------------------------ |
| b_pc_emulator                 | int                | PC模拟器                 | PC上运行的安卓模拟器，如雷电，取值：0：非PC模拟器1：是PC模拟器 |
| b_pc_emulator_last_ts         | int                | PC模拟器时间             |                                                              |
| b_cloud_device                | int                | 云手机设备               | 云端手机设备，如红手指，取值：0：非云手机设备1：是云手机设备 |
| b_cloud_device_last_ts        | int                | 云手机设备时间           |                                                              |
| b_altered                     | int                | 篡改设备                 | 篡改设备信息，使设备ID被篡改，取值：0：非篡改设备1：是篡改设备 |
| b_altered_last_ts             | int                | 篡改设备时间             |                                                              |
| b_multi_boxing                | int                | 多开设备                 | 多开工具进行的多开，如分身大师，且当前APP处于多开环境中，取值：0：当前未处在多开环境中1：当前处在多开环境中 |
| b_multi_boxing_last_ts        | int                | 多开设备时间             |                                                              |
| b_faker                       | int                | 伪造设备                 | 设备数据上报时全部或部分关键数据缺失或被伪造，进而伪造设备ID打业务接口的恶意行为，取值：0：非伪造设备1：是伪造设备 |
| b_faker_last_ts               | int                | 伪造设备时间             |                                                              |
| b_farmer                      | int                | 农场设备                 | 自动化操作的多台设备，组成设备农场，批量作恶，取值：0：非农场设备1：是农场设备 |
| b_farmer_last_ts              | int                | 农场设备时间             |                                                              |
| b_offerwall                   | int                | 积分墙设备               | 安装积分墙等网赚类工具的设备，取值：0：非积分墙设备1：是积分墙设备 |
| b_offerwall_last_ts           | int                | 积分墙设备时间           |                                                              |
| other                         | json_object        | 其他虚假设备             |                                                              |
| b_phone_emulator              | int                | 手机模拟器               | 手机上运行的安卓模拟器，如vmos，取值：0：非手机模拟器1：是手机模拟器 |
| b_phone_emulator_last_ts      | int                | 手机模拟器时间           |                                                              |
| b_alter_apps                  | int                | 安装篡改工具             | 设备安装篡改工具，如：应用变量，取值：0：设备没有安装篡改工具1：设备安装篡改工具 |
| b_alter_apps_last_ts          | int                | 安装篡改工具时间         |                                                              |
| b_alter_route                 | int                | 篡改地理位置             | 设备篡改gps位置，取值：0：设备没有篡改gps信息1：设备篡改gps信息 |
| b_alter_route_last_ts         | int                | 篡改地理位置标签最后时间 | 13位时间戳                                                   |
| b_alter_route_periods         | array              | 篡改地理位置行程时间段   | 取值为每次行程中检测到第一次到最后一次篡改 地理位置的时间段。示例：b_alter_route_periods:[1616688431748- 1616688431749, 1616688431766- 1616688431799] |
| b_multi_boxing_by_os          |                    | 系统多开                 | 系统自带的多开，且当前APP处于多开环境中，取值：0：当前未处在多开环境中1：当前处在多开环境中 |
| b_multi_boxing_by_os_last_ts  | int                | 最后一次系统多开时间     | 13位时间戳                                                   |
| b_multi_boxing_by_app         |                    | 工具多开                 | 多开工具进行的多开，如分身大师，且当前APP处于多开环境中，取值：0：当前未处在多开环境中1：当前处在多开环境中 |
| b_multi_boxing_by_app_last_ts | int                | 最后一次工具多开时间     | 13位时间戳                                                   |

 

other的详情内容如下：

| ***返回结果参数名*** | ***参数类型*** | ***参数说明***     | ***规范***                                                   |
| -------------------- | -------------- | ------------------ | ------------------------------------------------------------ |
| b_mismatch           | int            | 设备参数不匹配     | 设备参数与正常参数不匹配，例如8核手机检测为3核等，取值：0：参数正常1：参数不正常 |
| b_mismatch_last_ts   | int            | 设备参数不匹配时间 |                                                              |



device_suspicious_labels的详情内容如下：

| ***返回结果参数名***      | ***参数类型*** | ***参数说明***   | ***规范***                                               |
| ----------------------------- | ------------------ | -------------------- | ------------------------------------------------------------ |
| b_root                        | int                | root设备             | 设备被root，取值：0：设备未被root1：设备被root               |
| b_root_last_ts                | int                | root时间             |                                                              |
| b_sim                         | int                | sim卡状态            | 设备的sim卡状态异常，如无sim卡或sim卡不能正常工作，取值：0：sim卡状态正常1：sim卡状态异常 |
| b_sim_last_ts                 | int                | sim卡时间            |                                                              |
| b_debuggable                  | int                | 调试模式             | 开启调试模式，开发者或黑产都有可能，取值：0：未处于调试模式1：处于调试模式 |
| b_debuggable_last_ts          | int                | 调试模式时间         |                                                              |
| b_vpn                         | int                | VPN                  | 使用VPN代理IP，可能用于翻墙或黑产作恶，取值：0：未使用VPN1：使用VPN |
| b_vpn_last_ts                 | int                | VPN时间              |                                                              |
| b_monkey_apps                 | int                | 自动化工具           | 安装如按键精灵、触动精灵等自动化框架的设备，取值：0：暂未发现安装自动化工具1：安装自动化工具 |
| b_monkey_apps_last_ts         | int                | 自动化工具时间       |                                                              |
| b_acc                         | int                | 辅助服务             | 设备已开启辅助服务，具备自动化操作能力，也可能被某些软件用于自动化安装，取值：0：未开启辅助服务1：开启辅助服务 |
| b_acc_last_ts                 | int                | 辅助服务时间         |                                                              |
| b_multi_boxing_apps           | int                | 多开工具             | 设备安装了多开工具，但不一定是针对当前APP进行多开，取值：0：暂未发现安装多开工具1：安装多开工具 |
| b_multi_boxing_apps_last_ts   | int                | 多开时间             |                                                              |
| b_hook                        | int                | 是否hook设备         | 进程是否被注入其他代码或库。取值：0：进程未被注入其他代码或库1：进程被注入其他代码或库 |
| b_hook_last_ts                | int                | hook时间             |                                                              |
| b_vpn_apps                    | int                | VPN工具              | 安装如芝麻代理等VPN工具的设备，取值：0：暂未发现1：安装VPN工具 |
| b_vpn_apps_last_ts            | int                | VPN工具时间          |                                                              |
| b_manufacture                 | int                | 工程模式             | 设备进入工程模式：0：未处于工程模式1：处于工程模式           |
| b_manufacture_last_ts         | int                | 工程模式时间         |                                                              |
| b_icloud                      | int                | 未登录iCloud账号     | 设备未登录iCloud账号，取值：0：设备已登录iCloud账号1：设备未登录iCloud账号 |
| b_icloud_last_ts              | int                | 未登录iCloud账号时间 |                                                              |
| b_wx_code                     | int                | 微信接码平台         | 安装微信接码平台的设备，取值：0：暂未发现安装微信接码平台1：安装微信接码平台 |
| b_wx_code_last_ts             | int                | 微信接码平台时间     |                                                              |
| b_sms_code                    | int                | 短信接码平台         | 安装短信接码平台的设备，取值：0：暂未发现安装短信接码平台1：安装短信接码平台 |
| b_sms_code_last_ts            | int                | 短信接码平台时间     |                                                              |
| b_low_osver                   | int                | 低操作系统版本       | ios设备操作系统版本小于9，取值：0：ios设备操作系统版本不小于91：ios设备操作系统版本小于9 |
| b_low_osver_last_ts           | int                | 低操作系统版本时间   |                                                              |
| b_remote_control_apps         | int                | 远程操控工具         | 设备使用远程操控工具，取值：0：设备没有使用远程操控工具1：设备使用远程操控工具 |
| b_remote_control_apps_last_ts | int                | 远程操控工具时间     |                                                              |
| b_repackage                   | int                | 重打包               | app名与签名不匹配，取值：0: app名与签名匹配1: app名与签名不匹配 |
| b_repackage_last_ts           | int                | 重打包时间           |                                                              |
| b_alter_loc                   | int                | 篡改地理位置         | 设备篡改地理位置，取值：0：设备没有篡改地理位置1：设备篡改地理位置 |
| b_alter_loc_last_ts           | int                | 篡改地理位置时间     |                                                              |
| b_reset                       | int                | 疑似重置             | 设备疑似重置，取值：0: 设备没有疑似重置1: 设备疑似重置       |
| b_reset_last_ts               | int                | 疑似重置时间         |                                                              |
| b_console | int | 开启调试模式 | 设备开启调试模式，取值：0:设备没有开启调试模式1:设备开启调试 |
| b_console_last_ts | int | 开启调试模式时间 | |
| b_webdriver | int | 浏览器自动操作插件 | 浏览器自动操作插件，取值：0:浏览器没有自动操作查件1:浏览器自动操作插件 |
| b_webdriver_last_ts | int | 浏览器自动操作插件时间 | |

 

device_active_info的详情内容如下：

| ***返回结果参数名*** | ***参数类型*** | ***参数说明*** | ***规范*** |
| ------------------------ | ------------------ | ------------------ | -------------- |
| i_smid_boot_timestamp    | int                | 系统启动时间       |                |

 

monkey_device的详情内容如下：

| ***返回结果参数名*** | ***参数类型*** | ***参数说明*** | ***规范*** |
| ------------------------ | ------------------ | ------------------ | -------------- |
| common                   | int                | 通用               |                |
| monkey_game              | int                | 游戏操控           |                |
| monkey_read              | int                | 咨询阅读           |                |

 

common的详情内容如下：

| ***返回结果参数名*** | ***参数类型*** | ***参数说明*** | ***规范***                                               |
| ------------------------ | ------------------ | ------------------ | ------------------------------------------------------------ |
| b_monkey_apps            | int                | 机器操控工具       | 安装如按键精灵、触动精灵等自动化框架的设备，取值：0：暂未发现安装机器操控工具1：安装机器操控工具 |
| b_monkey_apps_last_ts    | int                | 机器操控工具时间   |                                                              |

 

monkey_game的详情内容如下：

| ***返回结果参数名***   | ***参数类型*** | ***参数说明*** | ***规范***                                               |
| -------------------------- | ------------------ | ------------------ | ------------------------------------------------------------ |
| b_monkey_game_apps         | int                | 游戏操控工具       | 安装如游戏蜂窝等游戏操控工具的设备，取值：0：暂未发现1：安装游戏操控工具 |
| b_monkey_game_apps_last_ts | int                | 游戏操控工具时间   |                                                              |

 

monkey_read的详情内容如下：

| ***返回结果参数名***   | ***参数类型*** | ***参数说明*** | ***规范***                                               |
| -------------------------- | ------------------ | ------------------ | ------------------------------------------------------------ |
| b_monkey_read_apps         | int                | 资讯阅读工具       | 安装如聚合阅读等资讯阅读工具的设备，取值：0：暂未发现1：安装资讯阅读工具 |
| b_monkey_read_apps_last_ts | int                | 资讯阅读工具时间   |                                                              |

 

其中标签类返回字段

***1***）tokenProfileLabels的详情内容

| ***返回结果参数名*** | ***参数类型*** | ***参数说明***     | ***规范***               |
| ------------------------ | ------------------ | ---------------------- | ---------------------------- |
| label1                   | string             | 一级标签               | 展示账号属性标签的一级标签。 |
| label2                   | string             | 二级标签               | 展示账号属性标签的二级标签。 |
| label3                   | string             | 三级标签               | 展示账号属性标签的三级标签。 |
| description              | string             | 风险描述               | 展示账号属性标签的中文描述。 |
| timestamp                | int64              | 最近一次命中策略的时间 | 最近一次命中策略的时间       |
| detail                   | json_object        | 证据描述               | 证据细节                     |

 

***2***）tokenRiskLabels的详情内容

| ***返回结果参数名*** | ***参数类型*** | ***参数说明***     | ***规范***               |
| ------------------------ | ------------------ | ---------------------- | ---------------------------- |
| label1                   | string             | 一级标签               | 展示账号风险标签的一级标签。 |
| label2                   | string             | 二级标签               | 展示账号风险标签的二级标签。 |
| label3                   | string             | 三级标签               | 展示账号风险标签的三级标签。 |
| description              | string             | 风险描述               | 展示账号风险标签的中文描述。 |
| timestamp                | int64              | 最近一次命中策略的时间 | 最近一次命中策略的时间       |
| detail                   | json_object        | 证据描述               | 证据细节                     |

 

***3***）deviceRiskLabels的详情内容

| ***返回结果参数名*** | ***参数类型*** | ***参数说明***     | ***规范***               |
| ------------------------ | ------------------ | ---------------------- | ---------------------------- |
| label1                   | string             | 一级标签               | 展示设备风险标签的一级标签。 |
| label2                   | string             | 二级标签               | 展示设备风险标签的二级标签。 |
| label3                   | string             | 三级标签               | 展示设备风险标签的三级标签。 |
| description              | string             | 风险描述               | 展示设备风险标签的中文描述。 |
| timestamp                | int64              | 最近一次命中策略的时间 | 最近一次命中策略的时间       |
| detail                   | json_object        | 证据描述               | 证据细节                     |

 

 

 

## <span id = "example">示例：</span>

### <span id = "requestExample">请求示例：</span>

```json
{
  "accessKey": "xxxxxxxxxxxxxxxxxxxxxxxxx",
  "data": {
    "tokenId": "ceshizhanghao",
    "deviceId": "ceshideviceId"
  }
}
```

### <span id = "syncResponseExample">同步返回示例：</span>

```json
{
  "code": 1100,
  "message": "成功",
  "profileExist": 1,
  "requestId": "7a5445716f0581c2ab1d381a6af4d1b8",
  "tokenLabels": {
    "machine_account_risk": {
      "b_machine_control_tokenid": 1,
      "b_machine_control_tokenid_last_ts": 1587711321232,
      "b_offer_wall_tokenid": 1,
      "b_offer_wall_tokenid_last_ts": 1587711321232
    },
    "UGC_account_risk": {
      "b_politics_risk_tokenid": 1,
      "b_politics_risk_tokenid_last_ts": 1587711321232,
      "b_sexy_risk_tokenid": 1,
      "b_sexy_risk_tokenid_last_ts": 1587711321232,
      "b_advertise_risk_tokenid": 1,
      "b_advertise_risk_tokenid_last_ts": 1587711321232
    },
    "scene_account_risk": {
      "i_tout_risk_tokenid": 1,
      "i_tout_risk_tokenid_last_ts": 1587711321232
    },
    "account_active_info": {
      "i_tokenid_first_active_timestamp": 1587711321232,
      "i_tokenid_active_days_7d": 5,
      "i_tokenid_active_days_4w": 5
    },
    "account_freq_info": {
      "i_tokenid_login_cnt_1d": 5,
      "i_tokenid_login_cnt_7d": 5
    },
    "account_relate_info": {
      "i_tokenid_relate_smid_cnt_1d": 5,
      "i_tokenid_relate_smid_cnt_7d": 5,
      "i_tokenid_relate_ip_city_cnt_1d": 5,
      "i_tokenid_relate_ip_city_cnt_7d": 5
    },
    "account_common_info": {
      "s_tokenid_relate_smid_info_map_4w": [
        {
          "smid": "xxxx1",
          "days": "3"
        },
        {
          "smid": "xxxx2",
          "days": "5"
        },
        {
          "smid": "xxxx3",
          "days": "10"
        }
      ],
      "s_tokenid_relate_ip_city_info_map_4w": [
        {
          "city": "北京",
          "days": "3"
        },
        {
          "city": "长沙",
          "days": "5"
        },
        {
          "city": "武汉",
          "days": "10"
        }
      ]
    }
  },
  "deviceLabels": {
    "id": 123,
    "fake_device": {
      "b_pc_emulator": 1,
      "b_pc_emulator_last_ts": 1587711321232,
      "b_cloud_device": 1,
      "b_cloud_device_last_ts": 1587711321232,
      "b_altered": 1,
      "b_altered_last_ts": 1587711321232,
      "b_multi_boxing": 1,
      "b_multi_boxing_last_ts": 1587711321232,
      "b_multi_boxing_by_os": 1,
      "b_multi_boxing_by_os_last_ts": 1587711321232,
      "b_multi_boxing_by_app": 1,
      "b_multi_boxing_by_app_last_ts": 1587711321232,
      "b_faker": 1,
      "b_faker_last_ts": 1587711321232,
      "b_farmer": 1,
      "b_farmer_last_ts": 1587711321232,
      "b_offerwall": 1,
      "b_offerwall_last_ts": 1587711321232,
      "other": {
        "b_mismatch": 1,
        "b_mismatch_last_ts": 1587711321232
      }
    },
    "device_suspicious_labels": {
      "b_root": 1,
      "b_root_last_ts": 1587711321232,
      "b_sim": 1,
      "b_sim_last_ts": 1587711321232,
      "b_debuggable": 1,
      "b_debuggable_last_ts": 1587711321232,
      "b_vpn": 1,
      "b_vpn_last_ts": 1587711321232,
      "b_monkey_apps": 1,
      "b_monkey_apps_last_ts": 1587711321232,
      "b_acc": 1,
      "b_acc_last_ts": 1587711321232,
      "b_multi_boxing_apps": 1,
      "b_multi_boxing_apps_last_ts": 1587711321232,
      "b_hook": 1,
      "b_hook_last_ts": 1587711321232,
      "b_vpn_apps": 1,
      "b_vpn_apps_last_ts": 1587711321232,
      "b_manufacture": 1,
      "b_manufacture_last_ts": 1587711321232,
      "b_icloud": 1,
      "b_icloud_last_ts": 1587711321232,
      "b_wx_code": 1,
      "b_wx_code_last_ts": 1587711321232,
      "b_sms_code": 1,
      "b_ sms_code _last_ts": 1587711321232,
      "b_low_osver": 1,
      "b_ low_osver_last_ts": 1587711321232,
      "b_remote_control_apps": 1,
      "b_remote_control_apps_last_ts": 1587711321232,
      "b_repackage": 1,
      "b_repackage_last_ts": 1587711321232,
      "b_reset": 1,
      "b_repackage_last_ts": 1587711321232
    },
    "device_active_info": {
      "i_smid_boot_timestamp": 1587711321232
    },
    "monkey_device": {
      "common": {
        "b_monkey_apps": 0,
        "b_monkey_apps_last_ts": 1587711321232
      },
      "monkey_game": {
        "b_monkey_game_apps": 0,
        "b_monkey_game_apps_last_ts": 1587711321232
      },
      "monkey_read": {
        "b_monkey_read_apps": 0,
        "b_monkey_read_apps_last_ts": 1587711321232
      }
    }
  },
  "tokenProfileLabels": [
    {
      "label1": "age_gender",
      "lable2": "token_age",
      "label3": "minor_token",
      "description": "年龄性别:年龄:未成年人",
      "timestamp": 1634732525000,
      "detail": {
        
      }
    }
  ],
  "tokenRiskLabels": [
    {
      "label1": "risk_device_token",
      "label2": "b_cloud_token_device",
      "label3": "b_cloud_token_device",
      "description": "风险设备账号:云手机账号:云手机账号",
      "timestamp": 1634732525000,
      "detail": {
        
      }
    },
    {
      "label1": "risk_device_token",
      "label2": "b_hook_token",
      "label3": "b_hook_token",
      "description": "风险设备账号:hook设备:hook设备",
      "timestamp": 1634732525000,
      "detail": {
        
      }
    }
  ],
  "deviceRiskLabels": [
    {
      "label1": "monkey_device",
      "label2": "monkey_game",
      "label3": "b_monkey_game_apps",
      "description": "机器操控设备:游戏操控:安装",
      "timestamp": 1634732525000,
      "detail": {
        
      }
    }
  ]
}
```

