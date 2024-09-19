# Shumei Manual Review Service API Documentation

- - - - -

***All rights reserved. Unauthorized reproduction is prohibited***

- - - - -

Table of Contents

[1. Preparation Before Integration](#_Toc21201 )

[1.1 Shumei Service Account Application](#_Toc17791 )

[1.2 Receiving Shumei Service Account Information](#_Toc19648 )

[2. Shumei Manual Review Service Description](#_Toc25359 )

[2.1 Regular Manual Review Result Callback Interface](#_Toc32283 )

[2.2 Human-Machine Integrated Manual Review Result Callback Interface](#_Toc11392 )





 

## 1. Preparation Before Integration

### 1.1 Shumei Service Account Application

The account manager has already established contact with your company or visited in person. You can directly provide the account opening and service-related information to the account manager.

The information required to open an account includes:

Company Full Name: xxxxxx

Company Abbreviation: xxxxxx

Contact Person Email: xxx@xxx.xxx

Contact Person Phone: 1xxxxxxxxxx

### 1.2 Receiving Shumei Service Account Information

The Shumei account manager will open the corresponding Shumei account and service for you within 1 working day. Subsequently, the contact person’s email will receive the following information:

| **Name**             | **Value** | **Description**                                   |
| -------------------- | ---------- | ------------------------------------------ |
| accessKey            | xxxxxx     | Authentication code for Shumei API service, required when calling Shumei API |
| Shumei Manual Review Management Backend Account | xxxxxx     | Used to log in to the Shumei Manual Review Management Backend |
| Shumei Manual Review Management Backend Password | xxxxxx     | Used to log in to the Shumei Manual Review Management Backend |
| Shumei Management Backend Address     |            | Used to log in to the Shumei Management Backend                       |

## 2. Shumei Manual Review Service Description

Shumei provides two main types of interfaces: regular manual review result callback and human-machine integrated manual review result callback. The regular manual review result callback interface is divided into: static single callback and live single callback.

### 2.1 Regular Manual Review Result Callback Interface

#### 2.1.1 Static Single Callback Result

(Static content includes text, images, audio files, video files, multimedia)

##### Supported Protocols:

HTTP or HTTPS

Customer-defined URL, please contact Shumei customer service to set the specific callback address.

##### Character Encoding Format:

Requests use UTF-8 character set for encoding

##### Request Method:

POST

| Parameter Name  | Type        | Required | Description                                                         |
| --------- | ----------- | -------- | ------------------------------------------------------------ |
| requestId | string      | Yes        | Request serial number                                                   |
| serviceId | string      | Yes        | POST_TEXT: text, POST_IMG: image, POST_AUDIO: audio, POST_VIDEO: video |
| result    | json_object | Yes        | Manual review result                                                     |
| data      | json_object | Yes        | Request data content                                                 |

Among them, the content of result is as follows:

| Parameter Name    | Type        | Required | Description                                                         |
| ----------- | ----------- | -------- | ------------------------------------------------------------ |
| operation   | int         | Yes        | Manual review result 1: pass, 2: fail                                     |
| description | string      | No        | Risk reason, supports customer-defined secondary reason configuration; if there is a secondary reason, it is concatenated with “/”, for example: pornography/nudity |
| tips        | string      | No        | User-side display text; used for user feedback                             |
| evidences   | json_object | No        | Evidence information (audio/video return)                                    |

 

Among them, the content of evidences is as follows:

| Parameter Name    | Type       | Required | Description                  |
| ----------- | ---------- | -------- | --------------------- |
| frameDetail | json_array |   No      | Risk frame              |
| audioDetail | json_array |   No      | Risk audio segment          |
| title       | string     |  No     | Audio file/video file name |
| cover       | string     |   No     | Video cover              |

Among them, the content of frameDetail is as follows:

| Parameter Name  | Type   | Required | Description                                       |
| --------- | ------ | -------- | ------------------------------------------ |
| requestId | string | No   | Frame serial number                                 |
| imgUrl    | string | No   | Frame image address                               |
| time      | float  |   No     | Time of the frame in the video file, in seconds           |
| imgText   | string | No   | Frame image OCR text recognition, will be present if OCR type is included |

Among them, the content of audioDetail is as follows:

| Parameter Name       | Type   | Required | Description                                               |
| -------------- | ------ | -------- | -------------------------------------------------- |
| requestId      | string | No        | Audio segment serial number                                     |
| audioUrl       | string |  No        | Audio segment address                                       |
| audioText      | string |  No        | Result of audio transcription                                 |
| audioStarttime | float  | No       | Start time of the audio segment, relative to the start time of the audio, in seconds |
| audioEndtime   | float  | No       | End time of the audio segment, relative to the start time of the audio, in seconds |

 

Among them, the content of data is as follows:

| Parameter Name    | Type       | Required | Description                                                         |
| ----------- | ---------- | -------- | ------------------------------------------------------------ |
| content     | string     | Yes       | Review content: text - review text content, image - image link address, audio - audio link address, video - video link address |
| tokenId     | string     | Yes       | User account                                                     |
| btId        | string     | No       | Unique identifier of image/audio file/video file                               |
| room        | string     | No       | Room number                                                       |
| appId       | string     | No      | Application                                                         |
| channel     | string     |  No     | Channel                                                         |
| passThrough | json_object | No    | Pass-through field                                                     |

##### Return Parameters

Placed in the HTTP Body, in Json format, the specific parameters are as follows:

| Parameter Name | Type   | Required | Description                                                     |
| -------- | ------ | -------- | -------------------------------------------------------- |
| code     | int    | Yes       | Return code, success returns 1100, will determine whether the request is successful; non-1100 is considered a failure |
| message  | string | Yes      | Description of the return code                                           |

 

#### 2.1.2 Live Single Callback Result

(Live content includes video live, audio live)

##### Supported Protocols:

HTTP or HTTPS

Customer-defined URL, please contact Shumei customer service to set the specific callback address.

##### Character Encoding Format:

Requests use UTF-8 character set for encoding

##### Request Method:

POST

| Parameter Name  | Type        | Required | Description                                             |
| --------- | ----------- | -------- | ------------------------------------------------ |
| requestId | string      | Yes        | Request serial number                                       |
| serviceId | string      | Yes      | POST_AUDIOSTREAM: audio stream, POST_VIDEOSTREAM: video stream |
| result    | json_object |   Yes      | Manual review result                                         |
| data      | json_object |  Yes     | Request data content                                     |

Among them, the content of result is as follows:

| Parameter Name    | Type        | Required | Description                                                         |
| ----------- | ----------- | -------- | ------------------------------------------------------------ |
| target      | int         | Yes     | Disposal object, optional values are as follows 1: user, 2: room                           |
| operation   | int         | Yes     | Manual review result 1: warning, 2: mute, 3: disconnect, 4: ban                         |
| description | String      |  No    | Risk reason, supports customer-defined secondary reason configuration; if there is a secondary reason, it is concatenated with “/”, for example: pornography/nudity |
| tips        | string      | No     | User-side display text; used for user feedback                             |
| duration    | int         | No    | Disposal duration, in seconds, exists when the disposal result is “mute” or “ban”             |
| evidences   | json_object | No     | Evidence information                                                     |

 

Among them, the content of evidences is as follows:

| Parameter Name    | Type       | Required | Description         |
| ----------- | ---------- | -------- | ------------ |
| frameDetail | json_array | No      | Risk frame     |
| audioDetail | json_array | No    | Risk audio segment |

 

Among them, the content of frameDetail is as follows:

| Parameter Name         | Type   | Required | Description                         |
| ---------------- | ------ | -------- | ---------------------------- |
| requestId        | string | No      | Frame serial number                   |
| imgUrl           | string | No       | Frame image address                 |
| beginProcessTime | string | No       | Frame start processing time (absolute time) |

Among them, the content of audioDetail is as follows:

| Parameter Name         | Type   | Required | Description                             |
| ---------------- | ------ | -------- | -------------------------------- |
| requestId        | string | No      | Audio segment serial number                   |
| audioUrl         | string | No     | Audio segment address                     |
| beginProcessTime | string | No       | Audio segment start processing time (absolute time) |

 

Among them, the content of data is as follows:

| Parameter Name    | Type       | Required | Description            |
| ----------- | ---------- | -------- | --------------- |
| tokenId     | string     | No        | User account        |
| room        | string     | No       | Room number          |
| streamId    | string     | No       | Audio stream/video stream ID |
| channel     | string     | No      | Channel            |
| passThrough | json_object | No        | Pass-through field        |

##### Return Parameters

Placed in the HTTP Body, in Json format, the specific parameters are as follows:

| Parameter Name | Type   | Required | Description                                                     |
| -------- | ------ | -------- | -------------------------------------------------------- |
| code     | int    | Yes       | Return code, success returns 1100, will determine whether the request is successful; non-1100 is considered a failure |
| message  | string | Yes       | Description of the return code                                           |

 

### 2.2 Human-Machine Integrated Manual Review Result Callback Interface

(Currently only supports text, images)

Shumei can support result callback according to your custom label system and disposal. For details, please consult Shumei business manager about the "Human-Machine Integration" access plan.

##### Supported Protocols:

HTTP or HTTPS

Customer-defined URL, please contact Shumei business manager to set the specific callback address.

##### Character Encoding Format:

Requests use UTF-8 character set for encoding

##### Request Method:

POST

| Request Parameter Name               | Field Type               | Required | Parameter Description                                                     | Specification                                                         |
| ------------------------ | ---------------------- | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| requestId                | string                 | Yes       |    Request identifier                                                       |                                                              |
| code                     | int                 | Yes       |  Return code                                                            |  1100: success|
| message                  | string                 | Yes       |    Description of the return code                                                          |                                                              |
| riskLevel                | string                 | No       | Shumei manual review disposal suggestion                                             |                                                              |
| riskLabel1               | string                 | No       | Shumei primary label confirmed by manual review                                       |                                                              |
| riskLabel2               | string                    | No       | Shumei secondary label confirmed by manual review                                                             |                                                              |
| riskLabel3               | string                   | No       |Shumei tertiary label confirmed by manual review                                                              |                                                              |
| riskDescription          | string                 | No       | Risk description                                                     | When riskLevel is PASS, it is "normal" |
| finalResult              | int                    | No       | Value is 1, your company can directly use the returned result for disposal, distribution, and other downstream scenarios; value is 0, indicating that the result is a process result of Shumei risk control, and it needs to be checked again by Shumei manual review before being returned to your company | 0: non-final result, 1: final result                                       |
| resultType               | int                    | No       | Whether the current result is given by machine review or manual review                             | 0: machine review, 1: manual review                                               |
| disposal                 | json_object            | No       | Shumei can return according to your custom label system and identifier; if the custom label system is not configured, this field will not be returned |                                       |

Among them, the content of disposal is as follows:

| Request Parameter Name               | Field Type               | Required | Parameter Description                                                     | Specification                                                         |
| ------------------------ | ---------------------- | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| riskLevel       | string                 | No       | If your company has its own disposal rules and means, Shumei can return corresponding disposal suggestions according to your company's disposal logic; if the custom disposal is not configured, the default Shumei disposal suggestion is returned | When Shumei label is not mapped to the custom label, the value is PASS                     |
| riskLabel1      | string                 | No       | Custom primary label identifier                                           |  When Shumei label is not mapped to the custom label, and riskLevel is PASS, it returns normal                             |
| riskLabel2      | string                 | No       | Custom secondary label identifier                                           | When riskLevel is PASS, it is an empty string                                |
| riskLabel3      | string                 | No       | Custom tertiary label identifier                                           | When riskLevel is PASS, it is an empty string                                |
| riskDescription | string                 | No       | Custom label name                                               | Format: primary label name: secondary label name: tertiary label name, when riskLevel is PASS, it is an empty string |