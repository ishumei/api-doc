# 数美智能文本流式代答查询产品API接口文档


## 请求参数

### 请求URL：

| 集群 | URL |
| --- | --- |
| 北京 | `http://api-text-bj.fengkongcloud.com/answer/v1` |
| 上海 | `http://api-text-sh.fengkongcloud.com/answer/v1` |
### 字符编码格式：

`UTF-8`字符集编码

### 请求方法：

`POST`

### 建议超时时长：

1s

### 请求参数：

放在HTTP Body中，采用Json格式，具体参数如下：

| **请求参数名** | **类型** | **参数说明** | **是否必传** | **规范** |
| --- | --- | --- | --- | --- |
| accessKey | string | 接口认证密钥 | Y | 由数美提供 |
| requestId | string | 请求标识 | Y | 本次流式代答查询数据的流水号|

## 返回结果

### 返回结果

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称** | **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| code | int| 返回码 | Y | `1100`：成功<br/>`1901`：QPS超限<br/>`1902`：参数不合法<br/>`1903`：服务失败<br/>`1905`：字数超限<br/>`9101`：无权限操作 |
| message | string | 返回码描述 | Y | 和code对应：<br/>成功<br/>QPS超限<br/>参数不合法<br/>服务失败<br/>字数超限<br/>无权限操作 |
| requestId | string | 请求标识 | Y | 本次请求数据的唯一标识，用于问题排查和效果优化，强烈建议保存|
| kbDetail       | json_object  | 知识库详情 | N | 知识库详情，[详见kbDetail参数](#kbDetail) |

<span id="kbDetail">其中，kbDetail字段内容如下：</span>

| **参数名称**| **类型** | **参数说明** | **是否必返** | **规范** |
| --- | --- | --- | --- | --- |
| qlabel| string | 问题标签| Y| 可选值：<br/>`UNKNOWN`：没有匹配<br/>`CANNOT_ASK`：问题本身不可提问/不可输入<br/>`EXACTNESS`：问题答案必须正确。包括立场正确<br/>`POSITIVE`：问题答案需要包含正向引导<br/> |
| answer | string | 建议答案 | Y | 当qlabel为"EXACTNESS"或者"POSITIVE"时，会给出数美建议的符合要求的答案。|
| isEnd | bool | 答案是否完整返回 | Y |答案是否完整返回 |
| hasAnswer | int | 是否有答案 | Y |1 有代答；0 没有代答，默认为0 |


## 示例

### 请求示例

```json
{
  "accessKey": "*************",
  "requestId": "xxxxxxxxxxxxxxxx"
}
```

### 返回示例

```json
{
  "code": 1100,
  "requestId": "xxxxxxx",
  "message": "成功",
  "kbDetail": {
    "answer": "东风汽车集团有限公司（以下简称东风公司）",
    "isEnd": false,
    "qlabel": "EXACTNESS",
    "hasAnswer": 1
  }
}
```
```json
{
  "code": 1100,
  "requestId": "xxxxxxx",
  "message": "成功",
  "kbDetail": {
    "answer": "成立于1969年，是中央直管的企业，累计用户近6000万。",
    "isEnd": true,
    "qlabel": "EXACTNESS",
    "hasAnswer": 1
  }
}
```



