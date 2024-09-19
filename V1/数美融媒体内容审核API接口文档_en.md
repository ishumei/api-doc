# **Shumei Media Content Review API Documentation**

------

All rights reserved. Unauthorized reproduction is prohibited.

------

## Detection Submission Interface

### Interface Description

This interface is for media detection submission, supporting HTTP protocol interface calls, and currently only supports asynchronous returns.

### Request URL

| Cluster Name | URL                                            |
| ------------ | ---------------------------------------------- |
| Silicon Valley Cluster | http://api-media-gg.fengkongcloud.com/media/v1 |
| Shanghai Cluster | http://api-media-sh.fengkongcloud.com/media/v1 |

### Detection Data Requirements

Request body limit: The total size of all request parameters cannot exceed 10M.

#### Text Requirements

- Text limit: Single request < 10000 characters, if the field length exceeds 10000 characters, it will prompt a parameter error.

#### Image Requirements

- Supported image types: URL
- Supported image formats: jpg, jpeg, png, webp, gif, tiff, tif, heif
- Image size: Single image 30M, pixel not less than 20px\*20px, not more than 6000px\*6000px, it is recommended that the image pixel is not less than 256px\*256px, too low pixels will affect the recognition effect.
- Image download: Download time limit is within 5 seconds, if the download time exceeds 5 seconds, the interface detection fails.
- Frame capture description: Shumei automatically captures frames for GIF images and long images (images with an aspect ratio greater than 4), and charges according to the actual number of screenshots.

#### Audio File Requirements

- Supported audio types: URL
- Supported audio formats: wav, mp3, aac, amr, 3gp, m4a, wma, ogg, ape, flac, alac, wavpack, silk_v3, etc.
- Audio size: Audio file size does not exceed 550M.
- Audio duration: Duration less than 5 hours.

#### Video File Requirements

- Supported video types: URL
- Supported video formats: AVI, FLV, MP4, MPG, WMV, MOV, WMA, RMVB, m3u8, etc.
- Video size: Video file size does not exceed 300M.
- Video duration: Duration less than 2 hours.

#### Document Requirements

- Supported document types: URL
- Supported document formats: DOCX, PDF, DOC, XLS, XLSX, PPT, PPTX, PPS, PPSX, XLTX, XLTM, XLSB, TXT
- Document size: Single document < 500M, text length limit 500,000 words, image number limit 500 images.

### Request Parameters

| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| accessKey | string | Y | Company secret key, contact Shumei to provide |
| appId | string | Y | Used to distinguish applications, need to contact Shumei service to open, please use the value provided by Shumei separately |
| eventId | string | Y | Used to distinguish scene data, need to contact Shumei service to open, please use the value provided by Shumei separately |
| data | json_object | Y | Request data |
| callback | string | Y | Callback address, used to receive review results |
| passThrough | json_object | N | Pass-through field |

Contents of data

| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| btId | string | Y | Work Id |
| tokenId | string | N | User account |
| contents | json_array | Y | Detection data, up to 20 text contents can be passed in when the type is text, each with a maximum of 10000 characters; up to 50 image URLs can be passed in when the type is image, each with a maximum of 512 characters; up to 5 audio URLs can be passed in when the type is audio, each with a maximum of 512 characters; up to 5 video URLs can be passed in when the type is video, each with a maximum of 512 characters; up to 10 document URLs can be passed in when the type is file, each with a maximum of 512 characters, if one does not meet the trigger error |
| detectFrequency | float | N | Frame capture frequency interval, in seconds, the value range is 0.5~60s; if not passed, the default is 5s per frame |
| advancedFrequency | json_object | N | Advanced frame capture interval, in seconds, if this item is filled, the default frame capture strategy is invalid, the parameter configuration is as follows {"durationPoints":[300,600],"frequencies":[1,5,10]} meaning: video file duration ≤ 300s —— choose 1s per frame 300s < video file duration ≤ 600s —— choose 5s per frame video file duration > 600s —— choose 10s per frame |
| returnVideoAllImg | int | N | Choose the level of returning video frame capture images: 0: return images with risk level other than pass; 1: return images of all risk levels. The default is 1 |
| returnVideoAllAudio | int | N | Choose the level of returning video audio clips: 0: return audio clips with risk level other than pass; 1: return audio clips of all risk levels. The default is 1 |
| returnAudioAllText | int | N | Optional values are as follows (default is 1):<br/>0: return the recognition results of risk segments<br/>1: return the recognition results of all segments<br/>This parameter is only used to control the return of segment recognition results and does not affect the return of overall recognition results.<br/>When "return all segment recognition results" is selected, the segment recognition results include the segment recognition results with riskLevel of PASS, REVIEW, and REJECT; when "return risk segment recognition results" is selected, the segment recognition results only include the segment recognition results with riskLevel of REVIEW and REJECT; the segment recognition results correspond to the audioDetail field in the response. |

Contents of advancedFrequency in data

| **Request Parameter Name** | **Type** | **Parameter Description** | **Transmission Description** | **Specification** |
| --- | --- | --- | --- | --- |
| durationPoints | float64[] | Video duration interval segmentation | Non-required parameter | Used to specify the duration interval of the video file supporting dynamic frame capture frequency, the array can have up to 5 elements |
| frequencies | float64[] | Frame capture frequency corresponding to the video duration interval | Non-required parameter | The range can be set to 0.5~60 seconds, the array can have up to 6 elements. Note: The number of elements set in the frequencies array needs to be one more than the number of elements in the durationPoints array, if wrong or empty, an error 1902 will be returned |

Contents of contents

| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| dataType | string | Y | Data type, possible values: <br/> text: text<br/> image: image <br/>audio: audio file <br/>video: video file <br/>file: document |
| content | string | Y | When the detection data type is text, the original text data is passed in, and when the detection data type is other, the data URL is passed in |
| dataId | string | N | Work Id, used to query works |
| btId | string | Y | Unique data identifier, used to query historical records, note: duplicates will report an error |
| txtType | string | N | Text and document text detection risk type, optional value: TEXTRISK. Must be passed when dataType is text or file |
| audioType | string | N | Audio and video audio part detection risk type, optional value:<br/>PORN: Pornography recognition<br/>AD: Advertisement recognition<br/>POLITICAL: Political recognition<br/>ABUSE: Abuse recognition<br/>MOAN: Moan recognition<br/>ANTHEN: National anthem recognition<br/>AUDIOPOLITICAL: Voiceprint recognition of the number one leader<br/>NONE: Do not review the audio in the video, and do not support passing in audio files for review<br/>If combined recognition is needed, connect with an underscore, for example, POLITICAL_PORN_MOAN for political, pornography, and moan recognition. Must be passed when dataType is audio or video |
| imgType | string | N | Image, document image, and video file frame capture detection risk type, optional value: POLITICS: Political recognition<br/>PORN: Pornography recognition<br/>AD: Advertisement recognition<br/>LOGO: Watermark logo recognition<br/>BEHAVIOR: Bad scene recognition, supporting smoking, drinking, gambling, drug use, condoms, and meaningless pictures<br/>OCR: OCR text recognition in images<br/>VIOLENCE: Violence and terrorism recognition<br/>If multiple functions need to be recognized, connect with an underscore, such as POLITICS_AD for political and advertisement combined recognition. Must be passed when dataType is image, video, or file |
| imageBusinessType | string | N | Image business label<br/>Optional value: [see appendix](#Appendix) If multiple recognition functions are needed, connect with an underscore, one of this field and imgType must be passed |
| audioBusinessType | string | N | Optional value: First, second, and third-level labels of audio business tags<br/>GENDER: Gender recognition<br/>AGE: Age recognition<br/>TIMBRE: Timbre recognition<br/>SING: Singing recognition<br/>LANGUAGE: Language recognition<br/>VOICE: Voice attribute<br/>AUDIOSCENE: Sound scene, if multiple recognition functions are needed, connect with an underscore, one of this field and audioType must be passed |
| fileFormat | string | N | Document format to be detected, must be passed when the data is a document, optional values:<br/>DOCX<br/>PDF<br/>DOC<br/>XLS<br/>XLSX<br/>PPT<br/>PPTX<br/>PPS<br/>PPSX<br/>XLTX<br/>XLTM<br/>XLSB<br/>XLSM<br/>TXT<br/>CSV<br/>EPUB<br/>If fileFormat does not match the actual format of the document, an error will be returned |

### Response Parameters

| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| code | int | Y | Return code |
| message | string | Y | Return code description |
| requestId | string | Y | The unique identifier of this request data, used for problem troubleshooting and effect optimization, it is strongly recommended to save |

## Asynchronous Callback Results

### Interface Description

After all data reviews in the task review are completed, Shumei will push the manual review results or asynchronous machine detection results to the customer to ensure that the customer gets the results as quickly as possible. The customer needs to set the callback address callback parameter when calling the detection interface, and the customer needs to ensure the availability of the callback receiving interface to ensure that the result data pushed can be received normally. Push strategy: When the user receives the push result and returns the HTTP status code 200, it indicates a successful push; otherwise, the system will repeatedly push, pushing 5 times, with an interval of 20s each time.

### Machine Detection Results

| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| btId | string | Y | Task Id |
| requestId | string | Y | Serial number |
| riskLevel | string | Y | Possible return values:<br/>PASS: Normal, recommended to pass directly<br/>REVIEW: Suspicious, recommended for manual review<br/>REJECT: Violation, recommended to intercept directly |
| resultType | int | Y | 0: Machine review, 1: Manual review |
| details | json_object | Y | Risk details |
| passThrough | json_object | N | Pass-through field |

Contents of details:

| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| texts | json_array | Y | Text review results, if not passed in, it is empty |
| images | json_array | Y | Image review results, if not passed in, it is empty |
| audios | json_array | Y | Audio file review results, if not passed in, it is empty |
| videos | json_array | Y | Video file review results, if not passed in, it is empty |
| files | json_array | Y | Document review results, if not passed in, it is empty |

**Contents of texts** for each element:

| **Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| code | int | Return code | Y | See response code parameter description |
| message | string | Return code description | Y | See response code parameter description |
| requestId | string | The unique identifier of this data task | Y | The unique identifier of this request data, used for problem troubleshooting and effect optimization, it is strongly recommended to save |
| dataId | string | Work Id | N | Corresponds to the passed dataId |
| btId | string | Text unique identifier | Y | Corresponds to the passed btId |
| riskLevel | string | Disposal suggestion | Y | Possible return values:<br/>PASS: Normal, recommended to pass directly<br/>REVIEW: Suspicious, recommended for manual review<br/>REJECT: Violation, recommended to intercept directly |
| riskLabel1 | string | First-level risk label | Y | First-level risk label, when riskLevel is PASS, return normal |
| riskLabel2 | string | Second-level risk label | Y | Second-level risk label, when riskLevel is PASS, it is empty |
| riskLabel3 | string | Third-level risk label | Y | Third-level risk label, when riskLevel is PASS, it is empty |
| riskDescription | string | Risk reason | Y | When riskLevel is PASS, it is "normal" |
| disposal | json_object | Disposal result | N | The value after mapping the outermost third-level label is placed in disposal, when the company label mapping is not enabled or the label mapping is not configured, this field is not returned |
| riskDetail | json_object | Risk details | Y | Risk details, [see](#riskDetail) [riskDetail parameters](#riskDetail) |

Contents of disposal:

| **Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| riskLevel | string | Disposal suggestion | N | Possible return values:<br/>PASS: Normal, recommended to pass directly<br/>REVIEW: Suspicious, recommended for manual review<br/>REJECT: Violation, recommended to intercept directly |
| riskLabel1 | string | Mapped first-level risk label | N | First-level risk label, when riskLevel is PASS, return normal |
| riskLabel2 | string | Mapped second-level risk label | N | Second-level risk label, when riskLevel is PASS, it is empty |
| riskLabel3 | string | Mapped third-level risk label | N | Second-level risk label, when riskLevel is PASS, it is empty |
| riskDescription | string | Mapped risk reason | N | When riskLevel is PASS, it is "normal" |
| riskDetail | json_object | Mapped risk details | N | Risk details |

Contents of riskDetail:

| **Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| matchedLists | json_array | Auxiliary information | N | List of custom lists hit by the customer. [See](#matchedLists) [matchedLists parameters](#matchedLists) |
| riskSegments | json_array | Auxiliary information, high-risk content segments detected when the text contains political, violent, prohibited, advertising law, etc. | N | [See](#riskSegments) riskSegments parameters |

Contents of matchedLists array in riskDetail:

| **Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| name | string | Auxiliary information | N | Name of the list hit |
| words | json_array | Auxiliary information | N | Array of sensitive words hit. [See](#words) [words parameters](#words) |

Contents of words array in matchedLists:

| **Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| word | string | Auxiliary information | N | Sensitive word hit |
| position | int_array | Auxiliary information | N | Position of the sensitive word |

Contents of riskSegments in riskDetail:

| **Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| segment | string | Auxiliary information | N | High-risk content segment |
| position | int_array | Auxiliary information | N | Position of the high-risk content segment |

**Contents of images** for each element:

| **Parameter Name** | **Parameter Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| code | int | Return code | Y | See response code parameter description |
| message | string | Return code description | Y | See response code parameter description |
| requestId | string | The unique identifier of this data task | Y | The unique identifier of this request data, used for problem troubleshooting and effect optimization, it is strongly recommended to save |
| dataId | string | Work Id | N | Corresponds to the passed dataId |
| btId | string | Single data unique identifier | Y | Corresponds to the passed btId |
| riskLevel | string | Disposal suggestion | Y | Possible return values:<br/>PASS: Normal, recommended to pass directly<br/>REVIEW: Suspicious, recommended for manual review<br/>REJECT: Violation, recommended to intercept directly |
| riskLabel1 | string | First-level risk label | Y | First-level risk label, when riskLevel is PASS, return normal |
| riskLabel2 | string | Second-level risk label | Y | When riskLevel is PASS, it is empty |
| riskLabel3 | string | Third-level risk label | Y | When riskLevel is PASS, it is empty |
| riskDescription | string | Risk reason | Y | When riskLevel is PASS, it is normal |
| disposal | json_object | Disposal result | N | The value after mapping the outermost third-level label is placed in disposal, when the company label mapping is not enabled or the label mapping is not configured, this field is not returned |
| riskDetail | json_object | Risk details | Y | [See](#riskDetail) riskDetail parameters |

Contents of disposal:

| **Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- |