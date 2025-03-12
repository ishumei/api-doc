# Shumei Intelligent Webpage Recognition Product API Documentation

Table of Contents

- [Shumei Intelligent Webpage Recognition Product API Documentation](#shumei-intelligent-webpage-recognition-product-api-documentation)
  - [Synchronous Interface](#synchronous-interface)
    - [Request Parameters](#request-parameters)
      - [Request URL:](#request-url)
      - [Character Encoding Format:](#character-encoding-format)
      - [Request Method:](#request-method)
      - [Suggested Timeout Duration:](#suggested-timeout-duration)
      - [Request Parameters:](#request-parameters-1)
    - [Return Results](#return-results)
      - [Synchronous Mode](#synchronous-mode)
      - [Callback Mode](#callback-mode)
      - [Request Return Parameters:](#request-return-parameters)
      - [Callback Return Parameters:](#callback-return-parameters)
    - [Examples](#examples)
      - [Synchronous Mode](#synchronous-mode-1)
        - [Request Example](#request-example)
        - [Response Example](#response-example)
      - [Callback Mode](#callback-mode-1)
        - [Request Example](#request-example-1)
        - [Response Example](#response-example-1)
  - [Asynchronous Interface](#asynchronous-interface)
    - [Request Parameters](#request-parameters-2)
      - [Request URL:](#request-url-1)
      - [Character Encoding Format:](#character-encoding-format-1)
      - [Request Method:](#request-method-1)
      - [Suggested Timeout Duration:](#suggested-timeout-duration-1)
      - [Request Parameters:](#request-parameters-3)
    - [Return Results](#return-results-1)
      - [Request Return Parameters](#request-return-parameters-1)
    - [Examples](#examples-1)
      - [Request Example](#request-example-2)
      - [Response Example](#response-example-2)
  - [Result Query Interface](#result-query-interface)
    - [Request Parameters](#request-parameters-4)
      - [Request URL:](#request-url-2)
      - [Character Encoding Format:](#character-encoding-format-2)
      - [Request Method:](#request-method-2)
      - [Suggested Timeout Duration:](#suggested-timeout-duration-2)
      - [Request Parameters:](#request-parameters-5)
    - [Return Results](#return-results-2)
      - [Request Return Parameters](#request-return-parameters-2)
    - [Examples](#examples-2)
      - [Request Example](#request-example-3)
      - [Response Example](#response-example-3)

## Synchronous Interface

### Request Parameters

#### Request URL:

| Cluster | URL | 
| --- | --- | 
| Beijing | `http://api-article-bj.fengkongcloud.com/v1/saas/anti_fraud/article` | 

#### Character Encoding Format:

`UTF-8` character set encoding

#### Request Method:

`POST`

#### Suggested Timeout Duration:

15s

#### Request Parameters:

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name** | **Type** | **Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| accessKey | string | API authentication key | Y | View details in the attachment of the account activation email. |
| type | string | Platform business type | N | Optional values:<br/>`ZHIBO`: Live broadcast<br/>`ECOM`: E-commerce<br/>`GAME`: Game<br/>`NEWS`: News<br/>`FORUM`: Forum<br/>`SOCIAL`: Social<br/>`NOVEL`: Novel<br/> |
| imgType | string | Image recognition type in the webpage | N | Optional values:<br/>`POLITICS`: Political recognition<br/>`PORN`: Pornographic recognition<br/>`AD`: Advertisement recognition<br/>`LOGO`: Watermark logo recognition<br/>`BEHAVIOR`: Bad scene recognition, supports smoking, drinking, gambling, drug use, condoms, and meaningless images<br/>`OCR`: OCR text recognition in images<br/>`VIOLENCE`: Violent and terrorist recognition<br/>`NONE`: No image recognition needed<br/>For combined recognition, connect with an underscore, e.g., `POLITICS_PORN_AD` for advertisement, pornographic, and political recognition<br/>If not provided, it defaults to political, pornographic, and advertisement recognition.<br/>Note: Here `POLITICS` is equivalent to the following two types:<br/>`PERSON`: Political face recognition<br/>`VIOLENCE`: Violent and terrorist recognition |
| txtType | string | Text recognition type in the webpage | N | Optional values:<br/>`DEFAULT`: Recognize political, violent, prohibited, pornographic, abusive, and advertisement content<br/>`NONE`: No text recognition needed<br/>If not provided, it defaults to `DEFAULT`. |
| videoImgType | string | Recognition type for video frames in the webpage | N | Optional values:<br/>`POLITICS`: Political recognition, here POLITICS actually recognizes political figures and violent content<br/>`PERSON`: Political figure recognition<br/>`VIOLENCE`: Violent recognition<br/>`PORN`: Pornographic & sexy violation recognition<br/>`AD`: Advertisement recognition<br/>`QR`: QR code recognition<br/>`OCR`: OCR text recognition in images<br/>`BEHAVIOR`: Bad scene recognition, supports smoking, drinking, gambling, drug use, condoms, and meaningless images<br/>For multiple functions, connect with an underscore, e.g., POLITY_QRCODE_ADVERT for political, QR code, and advertisement combined recognition<br/>If reviewing videos, this field is required |
| videoAudioType | string | Recognition type for audio in videos in the webpage | N | Optional values:<br/>`POLITICS`: Political recognition<br/>`PORN`: Pornographic recognition<br/>`AD`: Advertisement recognition<br/>`MOAN`: Moaning recognition<br/>`ABUSE`: Abusive recognition<br/>`ANTHEN`: National anthem recognition<br/>`AUDIOPOLITICAL`: Political audio recognition<br/>`NONE`: No audio detection<br/>For combined recognition, connect with an underscore, e.g., POLITICAL_PORN_MOAN for advertisement, pornographic, and political recognition<br/>Audio-only review in videos is not supported |
| appId | string | Application identifier | N | Used to distinguish different applications of the same company, this parameter value can be negotiated with Shumei service |
| callback | string | Callback HTTP interface | N | When this field is not empty, the service will notify the user of the review results based on this field; when `fileFormat` is provided, this field is required |
| callbackParam | json_object | Pass-through field | N | When `callback` exists, this field is optional, and the service will return this field content along with the review results in the callback request |
| data | json\_object | Request data content | Y | Maximum 1MB, [see data parameters](#data) |

Among them, the content of <span id="data">data</span> is as follows:

| **Parameter Name** | **Type** | **Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| contents | string | Webpage content to be detected | Y | Can be a URL link or text content<br/>The URL supports website links or document download links<br/>File size within 500MB, text length limit 500,000 characters. Image limit 500 images. |
| fileFormat | string | Document format to be detected | N | Optional values:<br/>`DOCX`<br/>`PDF`<br/>`DOC`<br/>`XLS`<br/>`XLSX`<br/>`PPT`<br/>`PPTX`<br/>`PPS`<br/>`PPSX`<br/>`XLTX`<br/>`XLTM`<br/>`XLSB`<br/>`TXT`<br/>If not provided or empty, it defaults to webpage link or text content detection<br/>If `fileFormat` does not match the actual document format, an error parameter will be returned |
| tokenId | string | Unique identifier for the client user account, used for user behavior analysis, it is recommended to provide the user UID | Y | If it is a webpage recognition scenario, provide the webpage URL |
| channel | string | Business scenario | N | Channel table configuration |
| returnHtml | bool | Whether to return the HTML with highlighted risk content after Shumei review, for display to reviewers | N | Optional values:<br/>`true`<br/>`false`<br/>Defaults to false |
| nickname | string | User nickname, it is strongly recommended to provide this parameter, as almost all platforms' malicious users spread spam through nicknames, posing risks of political, prohibited, and diversion information | N |  |
| ip | string | Client IP address, this parameter is used for user behavior analysis at the IP dimension and can also be used to compare with Shumei's IP blacklist | N |  |
| detectFrequency | float | Frame interval for video screenshots, range 0.5~60s; defaults to 5s if not provided | N | Unit: seconds |
| passThrough | json_object | Pass-through parameters, returned as is | N |  |

### Return Results

#### <span id="Ab1">Synchronous Mode</span>

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name** | **Type** | **Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| code | int| Return code | Y | `1100`: Success<br/>`1901`: QPS limit exceeded<br/>`1902`: Invalid parameters<br/>`1903`: Service failure<br/>`9100`: Insufficient balance<br/>`9101`: No permission to operate |
| message | string | Return code description | Y | Corresponds to code:<br/>Success<br/>QPS limit exceeded<br/>Invalid parameters<br/>Service failure<br/>Insufficient balance<br/>No permission to operate |
| requestId | string | Request identifier | Y | Unique identifier for this request data, used for troubleshooting and performance optimization, it is strongly recommended to save |
| score | int | Risk score | N | Exists when code is 1100, range [0,1000], the higher the score, the greater the risk |
| riskLevel | string | Handling suggestion | N | Possible values:<br/>`PASS`: Normal, recommended to pass directly<br/>`REVIEW`: Suspicious, recommended for manual review<br/>`REJECT`: Violation, recommended to block directly |
| detail | json_object | Risk details | N | [See detail parameters](#Adetail) |
| status | int | Indicates whether the service timed out | Y | Possible values:<br/>`0`: Normal<br/>`501`: Timeout |
| auxInfo | json_object | Auxiliary information | Y | [See auxInfo parameters](#AauxInfo)  |
| callbackParam | json_object | Pass-through field | N | Pass-through parameters, returned as is  |

<span id="Adetail">The detail field is as follows:</span>

| Parameter Name    | **Type**    | **Required** | **Description**                                                     |
| ----------- | ----------- | ------------ | ------------------------------------------------------------ |
| model       | string      | Y            | Rule identifier                                                     |
| description | string      | Y            | Description of the risk reason for the strategy rule                                         |
| riskSummary | json object | N            | Risk summary, currently includes the number of occurrences of various risk types, returned only if type is NOVEL<br/>[See riskSummary result details](#AriskSummary) |
| riskDetail  | json array  | N            | Risk details of each content segment, returned only if type is NOVEL. If returnHtml parameter is true, only the risk content segments of REJECT and REVIEW are returned; if returnHtml parameter is false, all content segments (including REJECT, REVIEW, and PASS) are returned.<br/>[See riskDetail result details](#AriskDetail) |
| riskHtml    | string      | N            | HTML with marked risk content, can be embedded in the HTML page to be displayed, returned only if type is NOVEL and returnHtml parameter is true.<br/> |
| hits        | json_array  | N            | Webpage hit information, generally empty. Hit details are in riskDetail.             |
| passThrough | json_object | N            | Pass-through parameters, returned as is             |

Among them, the content of <span id="AriskSummary">riskSummary</span> is the risk type, as follows:

| **Parameter Name**| **Type** | **Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| riskType | int| Number of occurrences of the corresponding riskType | N| Risk types:<br/>`0`: Normal<br/>`100`: Political<br/>`200`: Pornographic<br/>`210`: Abusive<br/>`250`: Moaning<br/>`300`: Advertisement<br/>`400`: Flooding<br/>`500`: Meaningless<br/>`600`: Prohibited<br/>`700`: Blacklist<br/>`710`: Whitelist<br/>`800`: High-risk account<br/>`900`: Custom |

Among them, <span id="AriskDetail">riskDetail</span> is a JSON array, each item is the risk details of a content segment, as follows:

| **Parameter Name** | **Type** | **Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| type     | string       | Type of the current content segment | Y            | Optional values:<br/>`text`: Text<br/>`img`: Image<br/>`video`: Video      |
| videoImgDetail     | json_array       | Details of the screenshot images in the current video segment, returned when type is video and video frames are reviewed | N       | [See videoImgDetail parameters](#AvideoImgDetail)   |
| videoAudioDetail     | json_array       | Details of the audio in the current video segment, returned when type is video and audio is reviewed | N       | [See videoAudioDetail parameters](#AvideoAudioDetail)   |
| content | string | Content of the current content segment | Y           | text is text content, img is image URL |
| beginPosition | int       | Start position of the current content segment in the input, not returned when type is `img` | N            | Detected text content, position starts from 0; after text segmentation, the position of the first character of the text content in each segment in the globally detected text |
| endPosition | int     | End position of the current content segment in the input, not returned when type is `img` | N           | Detected text content, position starts from 0; after text segmentation, the position of the last character of the text content in each segment in the globally detected text |
| description | string | Risk description of the current content segment | Y           | All sensitive words hit in the corresponding list                                 |
| riskLevel | string | Handling suggestion for the current content segment | Y           | Optional values:<br/>`PASS`: Pass<br/>`REVIEW`: Review<br/>`REJECT`: Reject |
| riskType | int | Identified risk type of the current content segment | Y<br/>Note: Required when type is text and image, optional when type is video | When type is text:<br/>`0`: Normal<br/>`100`: Political<br/>`200`: Pornographic<br/>`210`: Abusive<br/>`300`: Advertisement<br/>`400`: Flooding<br/>`500`: Meaningless<br/>`600`: Prohibited<br/>`700`: Blacklist<br/>`710`: Whitelist<br/>`800`: High-risk account<br/>`900`: Custom<br/><br/>When type is image:<br/>`0`: Normal<br/>`100`: Political<br/>`200`: Pornographic<br/>`210`: Sexy<br/>`300`: Advertisement<br/>`310`: QR code<br/>`320`: Watermark<br/>`400`: Violent<br/>`500`: Violation<br/>`510`: Bad scene<br/>`520`: Minor<br/>`700`: Blacklist<br/>`710`: Whitelist<br/>`800`: High-risk account<br/>`900`: Custom |
| riskTypeDec | string | Description corresponding to riskType | N |  |
| model | string | Rule identifier, used to identify the strategy rule hit by the text | N |  |
| matchedList | string | Name of the list where the sensitive word is hit (this parameter exists only when a sensitive word is hit) | N |  |
| matchedItem | string | Specific sensitive word hit (this parameter exists only when a sensitive word is hit) | N |  |
| matchedField | string | Indicates whether the nickname or text content hit a sensitive word (this parameter exists only when a sensitive word is hit) | N | Optional values:<br/>`text`: Text hit a sensitive word<br/>`nickname`: Nickname hit a sensitive word |
| matchedDetail | json_array | Details of the list hit | N | [See detailed structure](#matcheddetail) |
| index | int | Index of the current segment | Y | Index does not distinguish between text and image |
| keywordsPosition | string | Position of the sensitive word hit | N | Position in the segment |
| text | string | OCR content in the image | N | Returned when OCR content is recognized in the image segment |

Among them, the structure of <span id="AvideoImgDetail">videoImgDetail</span> is as follows:

| **Parameter Name**   | **Type**     | **Description** | **Required** | **Specification**                                                     |
| -------------- | ------------ | ------------ | ------------ | ------------------------------------------------------------ |
| time         | float       | Position of the image in the video     | Y            | Screenshot image relative to the video file time                                  |
| riskLevel   | string | Handling suggestion for the current screenshot             | Y            | Optional values:<br/>`PASS`: Pass<br/>`REVIEW`: Review<br/>`REJECT`: Reject |
| imgText   | string       | OCR text content in the screenshot        | N            | OCR text recognition in the screenshot, returned when the recognition type includes OCR             |
| riskType   | int       | Risk type of the screenshot        | Y         | Return values:<br/>`0`: Normal<br/>`100`: Political<br/>`200`: Pornographic<br/>`210`: Sexy<br/>`300`: Advertisement<br/>`310`: QR code<br/>`320`: Watermark<br/>`400`: Violent<br/>`500`: Violation<br/>`510`: Bad scene<br/>`700`: Blacklist<br/>`710`: Whitelist<br/>`800`: High-risk account<br/>`900`: Custom        |
| matchedList   | string       | Name of the list where the sensitive word is hit