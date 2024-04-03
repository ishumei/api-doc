**数美文本人工审核API接口文档**

**北京数美时代科技有限公司提供**

**（版权所有，翻版必究）**

目录

[1.API接口描述 ](#_Toc85617988)

[1.1请求参数 ](#_Toc85617989)

[1.2返回结果 ](#_Toc85617990)

[1.3示例 ](#_Toc85617991)



# **1.API接口描述**

## 1.1请求参数

**请求URL：**

北京：<http://api-audit-bj.fengkongcloud.com/audit/text/v1>

**字符编码格式：** UTF-8字符集编码

**请求方法：** POST

**建议超时时长：** 5s

**请求参数：**

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **备注** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥 | 必传参数 | 公司密钥：<br><br>用于权限认证，开通账号服务时由数美提供或使用开通邮箱登录数美后台右上角相关文档处查看 |
| appId | string | 应用标识 | 非必传参数 | 应用标识：用于区分相同公司的不同应用数据默认应用值：\`default\` |
| data | json_object | 请求的数据内容 | 必传参数 | 请求的数据内容，最长10MB |
| result | json_object | 机审结果信息	 | 非必传参数 |  |


其中，data的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **备注** |
| --- | --- | --- | --- | --- |
| text | string | 要审核的文本内容 | 必传参数| |
| tokenId | string | 用户账号标识 | 必传参数 |     |
| channel | string | 渠道，用于区分不通数据审核要求不一致问题 | 非必传参数 |     |
| passThrough | Json_object | 透传字段，这里面的内容都会透传给回调接口，业务用于使用 | 非必传参数 |     |



其中，result的内容如下：


| **请求参数名** | **类型** | **参数说明** | **是否必传** | **备注** |
| --- | --- | --- | --- | --- |
| riskLevel | string | 处置建议	 | 必传参数 |取机审返回的处置建议   |
| riskLabel1 | string | 一级风险标签 | 必传参数 | 取机审返回的一级风险标签 |
| riskLabel2 | string | 二级风险标签 | 必传参数 | 取机审返回的二级风险标签 |
| riskLabel3 | string | 三级风险标签 | 必传参数 | 取机审返回的三级风险标签 |
| riskDescription | string | 风险原因 | 必传参数 | 取机审返回的风险原因 |


## 1.2返回结果

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **备注** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是   | 1100：成功<br><br>1901：QPS超限<br><br>1902：参数不合法<br><br>1903：服务失败<br><br>9101：无权限操作<br><br>除message和requestId之外的字段，只有当code为1100时才会存在 |
| message | string | 返回码描述 | 是   | 和code对应：<br><br>成功<br><br>QPS超限<br><br>参数不合法<br><br>服务失败<br><br>余额不足<br><br>无权限操作 |
| requestId | string | 请求标识 | 是   | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存 |

## 1.3示例

**请求示例**
```json
{
    "accessKey": "xxxx",
    "appId": "default",
    "data": {
        "tokenId": "df6afc8badc44130a37677b700b67f30",
        "text": "阿斗先生",
        "channel": "default",
        "passThrough": {

        }
    },
    "result": {
        "riskLevel": "REJECT",
        "riskLabel1": "ad",
        "riskLabel2": "ad",
        "riskLabel3": "ad",
        "riskDescription": "广告：广告：广告"
    }
}
```  
