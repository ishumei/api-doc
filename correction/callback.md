---
title: 纠错后回调接口 | 数美
sidebar_label: 纠错后回调接口

hide_title: true
description: 
keywords:
- 纠错文档
---

# **纠错后回调接口**

---

版权所有，翻版必究

---

## **文档相关名词说明**

必传参数：使用数美接口服务时，接口使用者必须传入的参数，否则将导致该接口服务无法正常使用；

强烈建议：使用数美接口服务时，强烈建议接口使用者传入的参数并可大幅度提高识别准确率；

可选：使用数美接口服务时，建议接口使用者传入的参数并可在特定的场景下提高识别准确率。

## **客户配置信息**

使用前将需要回调的 url 地址提供给客服进行平台配置

## **纠错回调接口定义**

接口描述

该接口用于提供纠错后回调功能，支持将纠错的相关主要信息提供给客户。

请求URL：客户提供，平台配置后可使用

字符编码：UTF-8

请求方法：POST

建议超时时长：1s

### 传入回调地址参数

放在HTTP Body中，采用Json格式，具体参数如下：

| **参数名称**     | **类型** | **是否必传** | **说明**            |
|--------------|--------|----------|-------------------|
| requestId    | string | 是        | 请求流水号             |
| organization | string | 是        | 公司标识              |
| serviceId    | string | 是        | 具体服务与对应信息见 附录 1-1 |
| appId        | string | 是        | 应用                |
| channel      | string | 是        | 渠道                |
| tokenId      | string | 是        | 账号                |
| timestamp     | string | 是        | 审核的时间             |
| dataId     | string | 否        | 数据标识              |
| text     | string | 是        | 审核的内容             |
| nickname     | string | 否        | 用户昵称              |
| feedbackReason     | string | 是        | 纠错选择的风险类型         |
| feedbackResult     | string | 是        | 枚举（漏杀或误杀）         |


## 附录

| **服务名称**               | **服务标识**             |
| ---------------------- | -------------------- |
| POST_TEXT              | 文本服务             |
| POST_IMG               | 图片服务             |
| POST_AUDIOCLIPS        | 音频文件（片段）     |
| POST_AUDIOSTREAMCLIPS  | 音频流（片段）       |
| POST_VIDEO_IMG         | 视频文件（截帧）     |
| POST_VIDEO_AUDIO       | 视频文件（音频片段） |
| POST_VIDEOSTREAM_IMG   | 视频流（截帧）       |
| POST_VIDEOSTREAM_AUDIO | 视频流（音频片段）   |