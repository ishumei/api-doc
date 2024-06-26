# **数美融媒体人工审核API接口文档**

- - - - -

***版权所有 翻版必究***

- - - - -



## **1.API接口描述**

### 1.1请求参数

**请求URL：**

北京：<http://api-audit-bj.fengkongcloud.com/audit/media/v1>

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

其中，data的内容如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **备注** |
| --- | --- | --- | --- | --- |
| tokenId | string | 用户账号标识 | 必传参数 |     |
| contents | json_object | 要检测的文本texts，图片images，音频audios，视频videos信息，各自均为数组类型，具体详见示例 | 必传参数 | 检测数据，类型为texts时最多传入40条文本内容;  <br/>类型为images时最多传入100张图片url;  <br/>类型为audios时最多传入10条语音url;  <br/>类型为videos时最多传入10条视频url; |
| channel | string | 渠道，用于区分不通数据审核要求不一致问题 | 非必传参数 |     |
| passThrough | Json_object | 透传字段，这里面的内容都会透传给回调接口，业务用于使用 | 非必传参数 |     |



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
    "accessKey": "",
    "appId": "default",
    "data": {
        "channel": "DEFAULT",
        "tokenId": "username123",
        "contents": {
            "texts": [
                "加个好友吧 qq12345",
                "手机号12345"
            ],
            "images": [
                "http://xxx.com/1.jpg",
                "http://xxx.com2.jpg"
            ],
            "audios": [
                "http://1.wav",
                "http://2.wav"
            ],
            "videos": [
                "http://1.mp4"
            ]
        },
        "passThrough": {

        }
    }
}
```  
