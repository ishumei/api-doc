# **获取**DeepLink API接⼝⽂档




## 语音消息请求

### 请求URL

| 集群 | URL                                                        |
| ---- | ---------------------------------------------------------- |
| 测试 | http://[43.143.172.225](43.143.172.225)/deeplink/upload/v1 |
| 上海 | http://api-img-sh.fengkongcloud.com/deeplink/upload/v1     |

### 字符编码

`UTF-8`

### 请求方法

`POST`

### 建议超时时长

10s

### 请求参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型**    | **参数说明**     | **传入说明** | **规范**                               |
| :------------- | :---------- | :--------------- | :----------- | :------------------------------------- |
| accessKey      | string      | 公司密钥         | 必传参数     | 由数美提供                             |
| callback       | string      | 回调地址         | 必传参数     |                                        |
| data           | json object | 本次请求相关信息 | 必传参数     | 请求的数据内容， [详见data参数](#data) |

其中，<span id="data">data</span>的内容如下：

| **请求参数名**        | **类型** | **参数说明**         | **传入说明** | **规范**                                                     |
| :-------------------- | :------- | :------------------- | :----------- | :----------------------------------------------------------- |
| enableSwipeScreenshot | bool     | 是否开启滑动截图功能 | 非必传参数   |                                                              |
| deeplink              | string   | deeplink             | 必传参数     | Android deeplink 链接，目前已知支持如下 deeplink：  <br/>携程：ctrip://wireless/h5 美团： imeituan://[www.meituan.com/hotel/homepage](http://www.meituan.com/hotel/homepage)  <br/>小红书-微信小程序： xhsdiscover://wechat_miniprogram 其它类型待测试 |

### 同步返回参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **返回结果参数名** | **参数类型** | **参数说明**                   | **是否必返** | **规范**                                                     |
| :----------------- | :----------- | :----------------------------- | :----------- | :----------------------------------------------------------- |
| requestId          | string       | 本次请求的唯一标识             | 是           |                                                              |
| code               | int          | 请求返回码                     | 是           | <p>1100：成功</p><p>1901：QPS超限</p><p>1902：参数不合法</p><p>1903：服务失败</p><p>9101：无权限操作</p> |
| message            | string       | 请求返回描述，和请求返回码对应 | 是           | 和code对应                                                   |

### 异步回调

| **参数名** | **类型**    | **参数说明** | **是否必返** | **规范**                                                     |
| :--------- | :---------- | :----------- | :----------- | :----------------------------------------------------------- |
| code       | int         | 返回码       | 是           | <p>1100：成功</p><p>1901：QPS超限</p><p>1902：参数不合法</p><p>1903：服务失败</p> |
| message    | string      | 返回码描 述  | 是           | 和code对应                                                   |
| requestId  | string      | 请求标识     | 是           |                                                              |
| detail     | json_object | 返回数据     | 是           |                                                              |

detail内容

| **参数名**  | **类型**     | **参数说明**        | **是否必返** | **规范**                                        |
| :---------- | :----------- | :------------------ | :----------- | :---------------------------------------------- |
| screenshot  | string       | deeplink截图url     | 否           | 废弃字段，以后版本会删除                        |
| screenshots | string_array | 滑动截图的url       | 否           | 开启滑动截图功能后，会返回5张截图，否者返回一张 |
| qrcode      | string       | deeplink的二维码url | 否           |                                                 |
| deeplink    | string       | 原始deeplink        | 是           |                                                 |



## 示例

### 同步请求示例

```bash
{
    "accessKey": "mYVBvwVUvNqa4VHXXBO3_eR46sBuqF0fdw7KWFLYa",
    "callback":"http://127.0.0.1:9943",
    "data":
        {
            "deeplink": "xhsdiscover://wechat_miniprogram/gh_e7eeee3b9e52/?path=/pages/application/applyFirst/applyFirst?templateCode=160018&tfChannel=56001210-01&click_id=__CLICK_ID__",
            "enableSwipeScreenshot":true
        }
}
```

### 同步返回示例

```json
{"requestId":"02a171efcdf9a2789296cf021a556d1f","code":1100,"message":"成功"}
```

### 回调返回示例

```json
{
    "code": 1100,
    "message": "成功",
    "requestId": "02a171efcdf9a2789296cf021a556d1f",
    "detail": {
        "screenshot": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/POST_DEEPLINK/20250226/content/fca89dc387b5b48298e82258f284f810_0.jpg",
        "screenshots": [
            "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/POST_DEEPLINK/20250226/content/fca89dc387b5b48298e82258f284f810_0.jpg"
        ],
        "qrcode": "http://bj-video-1251671073.cos.ap-beijing.myqcloud.com/POST_DEEPLINK/20250226/content/fca89dc387b5b48298e82258f284f810_qrcode.jpg",
        "deeplink": "xhsdiscover://wechat_miniprogram/gh_e7eeee3b9e52/?path=/pages/application/applyFirst/applyFirst?templateCode=160018\u0026tfChannel=56001210-01\u0026click_id=__CLICK_ID__"
    }
}
```
