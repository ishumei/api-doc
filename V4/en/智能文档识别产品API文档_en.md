# Shumei Intelligent Document Recognition Product API Documentation

- - - - -

***All rights reserved. Reproduction prohibited***

- - - - -

Table of Contents

- [Intelligent Document Filtering Service](#A)
  
  - [Request Parameters](#Aa)
    - [Request URL](#Aa1)
    - [Character Encoding Format](#Aa2)
    - [Request Method](#Aa3)
    - [Recommended Timeout Duration](#Aa4)
    - [Request Parameters](#Aa5)
  - [Return Results](#Ab)
    - [Callback Mode](#Ab2)
  
  - [Examples](#Ac)
    - [Callback Mode](#Ac2)
      - [Request Example](#Ac21)
      - [Response Example](#Ac22)
  
- [Intelligent Document Upload Service](#B)
  
  - [Request Parameters](#Ba)
    - [Request URL](#Ba1)
    - [Character Encoding Format](#Ba2)
    - [Request Method](#Ba3)
    - [Recommended Timeout Duration](#Ba4)
    - [Request Parameters](#Ba5)
  - [Return Results](#Bb)
    - [Request Return Parameters](#Bb1)
  - [Examples](#Bc)
    - [Request Example](#Bc1)
    - [Response Example](#Bc2)
  
- [Result Query Interface](#C)
  
  - [Request Parameters](#Ca)
    
    - [Request URL](#Ca1)
    - [Character Encoding Format](#Ca2)
    - [Request Method](#Ca3)
    - [Recommended Timeout Duration](#Ca4)
    - [Request Parameters](#Ca5)
    
  - [Return Results](#Cb)
    
    - [Request Return Parameters](#Cb1)
    
  - [Examples](#Cc)
    
    - [Request Example](#Cc1)
    
    - [Return Example](#Cc2)
    
      

# <span id="A">Intelligent Document Filtering Service Access Instructions</span>

## <span id="Aa">Request Parameters</span>

### <span id="Aa1">Request URL:</span>

| Cluster | URL | Supported Product List |
| --- | --- |--------|
| Beijing | `http://api-article-bj.fengkongcloud.com/v1/saas/anti_fraud/article` | Document Products   |

### <span id="Aa2">Character Encoding Format:</span>

`UTF-8` character set encoding

### <span id="Aa3">Request Method:</span>

`POST`

### <span id="Aa4">Recommended Timeout Duration:</span>

15s

### <span id="Aa5">Request Parameters:</span>

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Request Parameter Name** | **Type** | **Parameter Description**   | **Required** | **Specification** |
| --- | --- |------------| --- | --- |
| accessKey | string | API authentication key     | Y | View details in the attachment of the account activation email. |
| type | string | Platform business type     | N | Optional values:<br/>`ZHIBO`: Live broadcast<br/>`ECOM`: E-commerce<br/>`GAME`: Game<br/>`NEWS`: News<br/>`FORUM`: Forum<br/>`SOCIAL`: Social<br/>`NOVEL`: Novel<br/> |
| imgType | string | Image recognition type in the document | N | Optional values:<br/>`POLITICS`: Political recognition<br/>`PORN`: Pornographic recognition<br/>`AD`: Advertisement recognition<br/>`LOGO`: Watermark logo recognition<br/>`BEHAVIOR`: Bad scene recognition, supports smoking, drinking, gambling, drug use, condoms, and meaningless images<br/>`OCR`: OCR text recognition in images<br/>`VIOLENCE`: Violent and terrorist recognition<br/>`NONE`: No image recognition needed<br/>For combined recognition, connect with an underscore, e.g., POLITICS_PORN_AD for advertisement, pornographic, and political recognition<br/>If not provided, it defaults to political, pornographic, and advertisement recognition. |
| txtType | string | Text recognition type in the document | N | Optional values:<br/>`DEFAULT`: Recognizes political, violent, prohibited, pornographic, abusive, and advertisement content<br/>`NONE`: No text recognition needed<br/>If not provided, it defaults to `default`. |
| appId | string | Application identifier       | N | Used to distinguish different applications of the same company, this parameter value can be negotiated with Shumei service |
| callback | string | Callback HTTP interface   | N | When this field is not empty, the service will notify the user of the review result based on this field; when `fileFormat` is provided, this field is required |
| callbackParam | json_object | Pass-through field       | N | Optional when `callback` exists, the service will return this field content along with the review result when sending the callback request |
| data | json\_object | Request data content    | Y | Maximum 1MB, [see data parameters](#data) |

 Among them, the content of <span id="data">data</span> is as follows:

| **Request Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- |----------| --- |
| contents | string | Content to be checked | Y        | Can fill in URL links<br/>The URL supports website links or document download links<br/>File size within 500MB, text length limit 500,000 characters. Image limit 500 images. |
| fileFormat | string | Document format to be checked | Y        | Optional values:<br/>`DOCX`<br/>`PDF`<br/>`DOC`<br/>`XLS`<br/>`XLSX`<br/>`PPT`<br/>`PPTX`<br/>`PPS`<br/>`PPSX`<br/>`XLTX`<br/>`XLTM`<br/>`XLSB`<br/>`XLSM`<br/>`TXT`<br/>`CSV`<br/>`EPUB`<br/>`SRT`<br/>`VTT`<br/>If `fileFormat` does not match the actual format of the document, an error parameter will be returned<br/> |
| tokenId | string | Unique identifier of the client user account, used for user behavior analysis, it is recommended to pass in the user UID | Y        |  |
| channel | string | Business scenario | N        | Channel table configuration |
| returnHtml | bool | Whether to return the HTML with highlighted risk content after Shumei review, for display to reviewers | N        | Optional values:<br/>`true`<br/>`false`<br/>Default is false |
| nickname | string | User nickname, it is strongly recommended to pass this parameter, as almost all platforms' malicious users will spread spam through nicknames, containing political, prohibited, and diversion information risks | N        |  |
| ip | string | Client IP address, this parameter is used for IP dimension user behavior analysis, and can also be used to compare with Shumei IP blacklist | N        |  |
| passThrough | json_object | Pass-through parameters, returned as is | N        |  |

## <span id="Ab">Return Results</span>

### <span id="Ab2">Callback Mode</span>

The system will automatically push the machine review results to the URL specified in the `callback` field

#### Request Return Parameters:

| **Parameter Name** | **Type**    | **Parameter Description** | **Required** | **Specification**                                                     |
| ------------ | ----------- | ------------ | ------------ | ------------------------------------------------------------ |
| code         | int         | Return code       | Y            | `1100`: Success<br/>`1901`: QPS limit exceeded<br/>`1902`: Invalid parameters<br/>`1903`: Service failure<br/>`9100`: Insufficient balance<br/>`9101`: No permission to operate |
| message      | string      | Return code description   | Y            | Corresponds to code:<br/>Success<br/>QPS limit exceeded<br/>Invalid parameters<br/>Service failure<br/>Insufficient balance<br/>No permission to operate |
| requestId    | string      | Request identifier     | Y            | Unique identifier for this request data, used for troubleshooting and performance optimization, it is strongly recommended to save  |
| score        | int         | Risk score     | N            | Exists when code is 1100, range [0,1000], the higher the score, the greater the risk         |
| riskLevel    | string      | Disposal suggestion     | N            | Possible return values:<br/>`PASS`: Normal, recommended to pass directly<br/>`REVIEW`: Suspicious, recommended for manual review<br/>`REJECT`: Violation, recommended to block directly |
| detail       | json_object | Risk details     | N            | [See detail parameters](#Adetail)                                   |

#### Callback Return Parameters:

| **Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| code | int| Return code | Y | `1100`: Success<br/>`1901`: QPS limit exceeded<br/>`1902`: Invalid parameters<br/>`1903`: Service failure<br/>`9100`: Insufficient balance<br/>`9101`: No permission to operate |
| message | string | Return code description | Y | Corresponds to code:<br/>Success<br/>QPS limit exceeded<br/>Invalid parameters<br/>Service failure<br/>Insufficient balance<br/>No permission to operate |
| requestId | string | Request identifier | Y | Unique identifier for this request data, used for troubleshooting and performance optimization, it is strongly recommended to save|
| score | int | Risk score | N | Exists when code is 1100, range [0,1000], the higher the score, the greater the risk |
| riskLevel | string | Disposal suggestion | N | Possible return values:<br/>`PASS`: Normal, recommended to pass directly<br/>`REVIEW`: Suspicious, recommended for manual review<br/>`REJECT`: Violation, recommended to block directly |
| detail | json_object | Risk details | N | [See detail parameters](#Adetail) |
| status | int | Indicates whether the service timed out | Y | Possible return values:<br/>`0`: Normal<br/>`501`: Timeout |
| auxInfo | json_object | Auxiliary information | Y | [See auxInfo parameters](#AauxInfo)  |
| callbackParam | json_object | Pass-through field	 | N	 |Pass-through parameters, returned as is  |

<span id="Adetail">Among them, the detail field is as follows:</span>

| Parameter Name    | **Type**    | **Required** | **Description**                                                   |
| ----------- | ----------- | ------------ | ---------------------------------------------------------- |
| model       | string      | Y            | Rule identifier                                                   |
| description | string      | Y            | Description of the strategy rule risk reason                                       |
| riskSummary | json object | N            | Risk summary, currently includes the number of occurrences of various risk types, returned only if type is NOVEL<br/>[See riskSummary result details](#AriskSummary) |
| riskDetail  | json array  | N            | Risk details of each content segment, returned only if type is NOVEL. If the returnHtml parameter is true, only the risk content segments of REJECT and REVIEW are returned; if the returnHtml parameter is false, all content segments (including REJECT, REVIEW, and PASS) are returned.<br/>[See riskDetail result details](#AriskDetail) |
| riskHtml    | string      | N            | HTML with marked risk content, can be embedded in the HTML page to be displayed, returned only if type is NOVEL and returnHtml parameter is true.<br/> |
| hits        | json_array  | N            | Hit information, generally empty. Hit details are in riskDetail.             |
| passThrough | json_object | N            | Pass-through parameters, returned as is             |

Among them, the content of <span id="AriskSummary">riskSummary</span> is the risk type, specifically as follows:

| **Parameter Name**| **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| riskType | int| Number of occurrences of the corresponding riskType | N| Risk types:<br/>`0`: Normal<br/>`100`: Political<br/>`200`: Pornographic<br/>`210`: Abusive<br/>`300`: Advertisement<br/>`400`: Flooding<br/>`500`: Meaningless<br/>`600`: Prohibited<br/>`700`: Blacklist<br/>`710`: Whitelist<br/>`800`: High-risk account<br/>`900`: Custom |

Among them, <span id="AriskDetail">riskDetail</span> is a JSON array, each item is the risk details of a content segment, specifically as follows:

| **Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification** |
| --- | --- | --- | --- | --- |
| type     | string       | Type of the current content segment | Y            | Optional values:<br/>`text`: Text<br/>`image`: Image<br/>          |
| content | string | Content of the current content segment | Y           | text is the text content, image is the image URL |
| beginPosition | int       | Start position of the current content segment in the input, this field is not returned when type is `image` | N            | Detected text content, position starts from 0; after text segmentation, the position of the first character of the text content of each segment in the globally detected text |
| endPosition | int     | End position of the current content segment in the input, this field is not returned when type is `image` | N           | Detected text content, position starts from 0; after text segmentation, the position of the last character of the text content of each segment in the globally detected text |
| description | string | Risk description of the current content segment | Y           | All sensitive words hit in the corresponding list                                 |
| riskLevel | string | Disposal suggestion for the current content segment | Y           | Optional values:<br/>`PASS`: Pass<br/>`REVIEW`: Review<br/>`REJECT`: Reject |
| riskType | int | Identified risk type of the current content segment | Y | When type is text:<br/>`0`: Normal<br/>`100`: Political<br/>`200`: Pornographic<br/>`210`: Abusive<br/>`300`: Advertisement<br/>`400`: Flooding<br/>`500`: Meaningless<br/>`600`: Prohibited<br/>`700`: Blacklist<br/>`710`: Whitelist<br/>`800`: High-risk account<br/>`900`: Custom<br/><br/>When type is image:<br/>`0`: Normal<br/>`100`: Political<br/>`200`: Pornographic<br/>`210`: Sexy<br/>`300`: Advertisement<br/>`310`: QR code<br/>`320`: Watermark<br/>`400`: Violent and terrorist<br/>`500`: Violation<br/>`510`: Bad scene<br/>`520`: Minor<br/>`700`: Blacklist<br/>`710`: Whitelist<br/>`800`: High-risk account<br/>`900`: Custom |
| riskTypeDec | string | Description corresponding to riskType | N |  |
| model | string | Rule identifier, used to identify the strategy rule hit by the text | N |  |
| matchedList | string | Name of the list where the sensitive word hit (this parameter exists only when a sensitive word is hit) | N |  |
| matchedItem | string | Specific sensitive word hit (this parameter exists only when a sensitive word is hit) | N |  |
| matchedField | string | Identifies whether the nickname or text content hit a sensitive word (this parameter exists only when a sensitive word is hit) | N | Optional values:<br/>`text`: Text hit a sensitive word<br/>`nickname`: Nickname hit a sensitive word |
| matchedDetail | json_array | Details of the list hit | N | [See detailed structure](#matcheddetail) |
| index | int | Index of the current segment | N | Index does not distinguish between text and image |
| keywordsPosition | string | Position of the sensitive word hit | N | Position in the segment |
| text | string | OCR content in the image | N | This field is returned when OCR content is recognized in the image segment |

Among them, the structure of <span id="matcheddetail">matchedDetail</span> is as follows:

| **Parameter Name**   | **Type**     | **Parameter Description** | **Required** | **Specification**                                                     |
| -------------- | ------------ | ------------ | ------------ | ------------------------------------------------------------ |
| listId         | string       |              | Y            | Return code                                                       |
| matchedFiled   | string_array |              | N            | Identifies whether the nickname or text content hit a sensitive word (this parameter exists only when a sensitive word is hit), optional values:<br/>text: Text hit a sensitive word<br/>nickname: Nickname hit a sensitive word |
| name           | string       |              | Y            | Name of the list where the sensitive word hit                                     |
| organization   | string       |              | N            | Company identifier of the list hit, where "GLOBAL" is the global list               |
| words          | string_array |              | N            | All sensitive words hit in the corresponding list                                 |
| wordPositions | json_array   |              | N            | All sensitive words and positions hit in the corresponding list. [See wordPositions](#words) |

The content of each item in <span id="words">wordPositions</span>:

| **Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification**       |
| ------------ | -------- | ------------ | ------------ | -------------- |
| word         | string   | Auxiliary information     | N            | Sensitive word hit   |
| position     | string   | Auxiliary information     | N            | Position of the sensitive word |

<span id="AauxInfo">Among them, the auxInfo field is as follows:</span>

| **Parameter Name** | **Type** | **Parameter Description** | **Required** | **Specification**       |
| ------------ | -------- | ------------ | ------------ | -------------- |
| textNum      | int   | Number of characters in the current request, consistent with the billing number     | Y            | Number of characters in the current request, including Chinese characters, English, punctuation marks, spaces, etc.   |
| imgNum       | int   | Number of images in the current request, consistent with the billing number     | Y            | Number of images in the current request, if encountering GIFs, 3 frames will be captured; if encountering long images, they will be split |

## <span id="Ac">Examples</span>

### <span id="Ac2">Callback Mode</span>

#### <span id="Ac21