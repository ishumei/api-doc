# **数美图片人工审核API接口文档**

- - - - -

***版权所有 翻版必究***

- - - - -


## **1.API接口描述**

### 1.1请求参数

**请求URL：**

北京：<http://api-audit-bj.fengkongcloud.com/audit/image/v1>

**字符编码格式：** UTF-8字符集编码

**请求方法：** POST

**建议超时时长：** 5s

**请求参数：**

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **传入说明** | **备注** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥 | 必传参数 | 由数美提供 |
| appId | string | 应用标识 | 非必传参数 | 用于区分应用，需要联系数美开通，请使用数美单独提供的传值为准 |
| data | json_object | 请求的数据内容 | 必传参数 | 请求的数据内容，最长10MB |
| result | json_object | 机审结果信息	 | 非必传参数 |  |


其中，data的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **备注** |
| --- | --- | --- | --- | --- |
| img | string | 要审核的图片 | 必传参数|图片数据或者图片的url链接<br/>支持格式：jpg，jpeg，jp2，png，webp，gif，bmp，tiff，tif，dib，ppm，pgm，pbm，hdr，pic<br/>建议图片像素不小于256*256<br/>图片大小最大10MB |
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


### 1.2返回结果

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **参数类型** | **参数说明** | **是否必返** | **备注** |
| --- | --- | --- | --- | --- |
| code | int | 返回码 | 是   | 1100：成功<br/>1901：QPS超限<br/>1902：参数不合法<br/>1903：服务失败<br/>9101：无权限操作<br/> |
| message | string | 返回码描述 | 是   | 和code对应：<br/>成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>无权限操作 |
| requestId | string | 请求标识 | 是   | 请求唯一标识，用于排查问题和后续效果优化，强烈建议保存 |

### 1.3示例

**请求示例**
```json
{
    "accessKey": "xxxx",
    "appId": "default",
    "data": {
        "tokenId": "df6afc8badc44130a37677b700b67f30",
        "img": "https://1.jpg",
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
