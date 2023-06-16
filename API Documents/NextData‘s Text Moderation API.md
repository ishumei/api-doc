# NextData‘s Text Moderation API


## Request parameters

### Request URL:

| Colony                   | URL                                              |
| ------------------------ | ------------------------------------------------ |
| Beijing                  | `http://api-text-bj.fengkongcloud.com/text/v4`   |
| Shanghai                 | `http://api-text-sh.fengkongcloud.com/text/v4`   |
| Guangzhou                | `http://api-text-gz.fengkongcloud.com/text/v4`   |
| United States (Virginia) | `http://api-text-fjny.fengkongcloud.com/text/v4` |
| Singapore                | `http://api-text-xjp.fengkongcloud.com/text/v4`  |

### Character encoding format:

`UTF-8`Character set encoding

### Request method:

`POST`

### Recommended timeout length:

1s

### Request parameters:

It is placed in the HTTP Body in Json format. The specific parameters are as follows:

| **Request parameter name** | **Type**     | **Parameter Description**              | **Must be transferred in or not** | **Detailed description**                                     |
| -------------------------- | ------------ | -------------------------------------- | --------------------------------- | ------------------------------------------------------------ |
| accessKey                  | string       | Interface authentication key           | Y                                 | Provided by NextData                                         |
| appId                      | string       | Application ID                         | Y                                 | It is used to distinguish between applications. You need to contact the service of NextData to open. Please use the value transfer provided by NextData separately |
| eventId                    | string       | Event ID                               | Y                                 | You need to contact the service of NextData to open. Please use the value transfer provided by NextData separately |
| type                       | string       | Types of risks to be detected          | Y                                 | Optional values:<br/>`TEXTRISK`：Routine risk detection (including:<br/>politics, violence, terrorism, prohibition, pornography, abuse, advertising, privacy, advertising law)<br/>`FRUAD`：Network fraud detection<br/>`UNPOACH`：Anti-digging detection for high-value users<br/>`TEXTMINOR`: Content detection of minors<br/>The above types can be combined with underscores, such as:`TEXTRISK_FRUAD` |
| data                       | json\_object | Requested data content                 | Y                                 | Up to 1MB, [see data parameter for details](#data)           |
| acceptLang                 | string       | Returns the language type of interface | N                                 | Optional language type of returned information:<br/>zh: Chinese<br/>en: English<br/>The default is zh. If you need to return results in English, this field is required |

<span id="data"> The content of data is as follows:</span>

| **Request parameter name** | **Type**     | **Parameter Description**                                    | **Must be transferred in or not** | **Detailed description**                                     |
| -------------------------- | ------------ | ------------------------------------------------------------ | --------------------------------- | ------------------------------------------------------------ |
| text                       | string       | Text to be detected                                          | Y                                 | When the value of lang field is `zh`, or the value of  `auto` is recognized as Chinese, the maximum number of characters in a single request is 10000 characters, and an error will be reported when the number exceeds 10000 characters<br/>If the nickname field is passed, the text+nickname content will be verified at the same time. |
| tokenId                    | string       | It is recommended to use your company's user UID (encryptable) to generate the user account identification. The identification of the user's unique identity is used for the risk control of behavior dimensions such as irrigation and advertising<br/>If there is no user uid, it is recommended to use a unique data identifier to transfer values | Y                                 | A string composed of numbers, letters, underscores, and dashes with a length less than or equal to 64 bits |
| lang                       | string       | Language of text content to be tested                        | N                                 | The optional values and corresponding languages are as follows:<br/>`zh`：Chinese<br/>`en`：English<br/>`ar `: Arabic<br/>` hi `: Hindi<br/>` es `: Spanish<br/>` fr `: French<br/>` ru `: Russian<br/>` pt `: Portuguese<br/>` id `: Indonesian<br/>` de `: German<br/>` ja `: Japanese<br/>` tr `: Turkish<br/>` vi `: Vietnamese<br/>` it `: Italian<br/>` th `: Thai<br/>` tl `: Filipino<br/>` ko `: Korean<br/>` ms `: Malay<br/>`auto`: Automatic recognition of language type<br/>The default value is zh. If you transfer data to  domestic colonies, you can choose not to transfer it or transfer it to zh; If the content you want to detect cannot distinguish between languages, it is recommended to select auto, and the system will automatically detect the language type. |
| nickname                   | string       | User nickname                                                | N                                 | Risk of verifying nickname content                           |
| ip                         | string       | IP address                                                   | N                                 | The public network IPv4 address of the user who sent the text |
| deviceId                   | string       | NextData's equipment identification                          | N                                 | Unique device identification generated by NextData's device fingerprint |
| extra                      | json\_object | Auxiliary parameters                                         | N                                 | Relevant information for auxiliary text detection, [see extra parameters](#extra) |

<span id="extra">The contents of each element of the extra array in data are as follows:</span>

| **Request parameter name** | **Type** | **Parameter Description**                                    | **Must be transferred in or not** | **Detailed description**                                     |
| -------------------------- | -------- | ------------------------------------------------------------ | --------------------------------- | ------------------------------------------------------------ |
| receiveTokenId             | string   | TokenId of the message receiver in the private chat scenario | N                                 | A string consisting of numbers, letters, underscores, and dashes with a length less than or equal to 64 bits. It must be passed when the eventId value is` message ` |
| topic                      | string   | It can be subject number, book review area number and forum post number | N                                 | Must be passed when the eventId value is`article`            |
| atId                       | string   | TokenId of the user mentioned in the group chat scenario     | N                                 | A string consisting of numbers, letters, underscores and dashes with a length of less than or equal to 64 bits. It must be passed when the eventId value is`groupChat` |
| room                       | string   | Live room/game room number                                   | N                                 | It must be passed when the eventId value is`groupChat`       |
| role                       | string   | User role                                                    | N                                 | It is used to distinguish users with different roles in the live broadcast/game industry. The optional value (default is normal user):<br/>`SYSTEM`: system<br/>`USER`: normal user<br/>When the incoming value is`SYSTEM`, the current request is risk-free by default |
| sex                        | int      | Gender                                                       | N                                 | Used for user gender, optional values:<br/>`0`: male<br/>`1`: female<br/>`2`: gender unknown |
| passThrough                | Json     | The pass-through field passed in by the customer             | N                                 | The content of this field will be returned along with the return value |

## Return results

### Return results

It is placed in the HTTP Body in Json format. The specific parameters are as follows:

| **Return parameter name** | **Type**    | **Parameter Description** | **Must be returned or not** | **Detailed description**                                     |
| ------------------------- | ----------- | ------------------------- | --------------------------- | ------------------------------------------------------------ |
| code                      | int         | Return code               | Y                           | `1100`：Success<br/>`1901`：Request Limit Exceeded<br/>`1902`：Invalid Parameters<br/>`1903`：Internal Server Error<br/>`9100`：Balance Not Enough<br/>`9101`：Operation Denied |
| message                   | string      | Return code description   | Y                           | Corresponding to code,<br/>include: <br/>Success<br/>Request Limit Exceeded<br/>Invalid Parameters<br/>Internal Server Error<br/>Balance Not Enough<br/>Operation Denied |
| requestId                 | string      | Request ID                | Y                           | The unique identifier of this request data is used for troubleshooting and effect optimization. It is strongly recommended to save it |
| riskLevel                 | string      | Disposal suggestions      | Y                           | Possible return values:<br/>`PASS `: Normal, direct release recommended<br/>`REVIEW` : Suspicious, manual review<br/>recommended<br/>`REJECT` : Violation, direct interception is<br/>recommended |
| riskLabel1                | string      | First risk label          | Y                           | First risk label,return`normal`when riskLevel is `PASS`      |
| riskLabel2                | string      | Second risk label         | Y                           | Second risk label,When riskLevel is`PASS`, the return value is Null |
| riskLabel3                | string      | Third risk label          | Y                           | Third risk label,When riskLevel is`PASS`, the return value is Null |
| riskDescription           | string      | Risk description          | Y                           | When riskLevel is`PASS`, the return value is `normal`        |
| riskDetail                | json_object | Risk details              | Y                           | Risk details, [see riskDetail parameter](#riskDetail)        |
| tokenLabels               | json object | Auxiliary information     | Y                           | See the following details for account risk portrait label information.[See tokenLabels parameter for details](#tokenLabels) |
| auxInfo                   | json_object | Auxiliary information     | Y                           | [See auxInfo parameter for details](#auxlnfo)                |
| allLabels                 | json_array  | Auxiliary information     | Y                           | All risk labels and details matched by the model.[See allLabels parameter for details](#allLabels) |
| businessLabels            | json_array  | Auxiliary information     | Y                           | All business labels and details matched by the model.[See allLabels parameter for details](#businessLabels) |
| tokenProfileLabels        | json_array  | Auxiliary information     | N                           | Attribute account class label.[See account class label parameters for details](#tokenProfileLabels) |
| tokenRiskLabels           | json_array  | Auxiliary information     | N                           | Risk account type label.[See account type label parameters for details](#tokenProfileLabels) |
| langResult                | json_object | Language result           | N                           | Language information.[See language information parameters for details](#langResult) |

<span id="langResult">The structure of language information langResult is as follows:</span>

| **Return parameter name** | **Type** | **Parameter Description**    | **Must be returned or not** | **Detailed description**                                     |
| ------------------------- | -------- | ---------------------------- | --------------------------- | ------------------------------------------------------------ |
| detectedLang              | string   | Language recognition results | N                           | This field is returned when the lang parameter is transferred into auto. It is the language recognition result, for example:`zh`、`en`、`ar`.etc |

<span id="auxlnfo">The auxInfo field is as follows:</span>

| **Return parameter name** | **Type**    | **Parameter Description** | **Must be returned or not** | **Detailed description**                                     |
| ------------------------- | ----------- | ------------------------- | --------------------------- | ------------------------------------------------------------ |
| filteredText              | string      | Auxiliary information     | N                           | The sensitive word is replaced with the text after * (this parameter only exists when the sensitive word is hit)<br/>Context model, and the risk related to contact information is not returned to this field |
| passThrough               | json_object | Auxiliary information     | N                           | The content of this field is the same as the passThrough value of extra in the request parameter data. |
| contactResult             | json_array  | Auxiliary information     | N                           | The identification result of contact information includes the string type and content of WeChat, Weibo, QQ and mobile phone number identified. [See contactResult parameter for details](#contactResult) |
| unauthorizedType          | string      | Auxiliary information     | N                           | Unauthorized type.                                           |

<span id="contactResult">In auxInfo, the contents of each element of the contactResult array are as follows:</span>

| **Return parameter name** | **Type** | **Parameter Description** | **Must be returned or not** | **Detailed description**                                     |
| ------------------------- | -------- | ------------------------- | --------------------------- | ------------------------------------------------------------ |
| contactType               | int      | Auxiliary information     | N                           | Contact type, optional value range [0-3], details are as follows:<br/>`0`: mobile number <br/>`1`: QQ <br/>`2`: WeChat <br/>`3`: Weibo |
| contactString             | string   | Auxiliary information     | N                           | Contact string                                               |

<span id="riskDetail">The contents of riskDetail are as follows:</span>

| **Return parameter name** | **Type**   | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| ------------------------- | ---------- | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| matchedLists              | json_array | Auxiliary information                                        | N                           | List of hit customer custom lists.[See the matchedLists parameter for details](#matchedLists) |
| riskSegments              | json_array | Auxiliary information, Exist when high-risk content segment detection text contains risk content such as politics, terrorism, prohibition, advertising law, etc | N                           | [See riskSegments parameter for details](#riskSegments)      |

<span id="matchedLists">In riskDetail, the contents of each element of the matchedLists array are as follows:</span>

| **Return parameter name** | **Type**   | **Parameter Description** | **Must be returned or not** | **Detailed description**                                     |
| ------------------------- | ---------- | ------------------------- | --------------------------- | ------------------------------------------------------------ |
| name                      | string     | Auxiliary information     | N                           | Name of matching list                                        |
| words                     | json_array | Auxiliary information     | N                           | Array of matching sensitive words.[See words parameter for details](#words) |

<span id="words">In matchedLists, the contents of each element of the words array are as follows:</span>

| **Return parameter name** | **Type**  | **Parameter Description** | **Must be returned or not** | **Detailed description**    |
| ------------------------- | --------- | ------------------------- | --------------------------- | --------------------------- |
| word                      | string    | Auxiliary information     | N                           | Sensitive words             |
| position                  | int_array | Auxiliary information     | N                           | Location of sensitive words |

<span id="riskSegments">In riskDetail, the contents of riskSegments are as follows:</span>

| **Return parameter name** | **Type**  | **Parameter Description** | **Must be returned or not** | **Detailed description**                |
| ------------------------- | --------- | ------------------------- | --------------------------- | --------------------------------------- |
| segment                   | string    | Auxiliary information     | N                           | High-risk content fragments             |
| position                  | int_array | Auxiliary information     | N                           | Location of high-risk content fragments |

<span id="tokenLabels">The details of tokenLabels are as follows:</span>

| **Return parameter name** | **Type**    | **Parameter Description** | **Must be returned or not** | **Detailed description**                                     |
| ------------------------- | ----------- | ------------------------- | --------------------------- | ------------------------------------------------------------ |
| UGC_account_risk          | json_object | Auxiliary information     | N                           | Risks related to UGC content.[See UGC for details_ account_ Risk parameter](#UGC_account_risk) |

<span id="UGC_account_risk">In tokenLabels, UGC_ account_ The details of risk are as follows:</span>

| **Return parameter name** | **Type** | **Parameter Description** | **Must be returned or not** | **Detailed description**                    |
| ------------------------- | -------- | ------------------------- | --------------------------- | ------------------------------------------- |
| sexy_risk_tokenid         | float    | Auxiliary information     | N                           | Pornographic account risk score range [0-1] |

<span id="allLabels">The contents of allLabels are as follows:</span>

| **Return parameter name** | **Type**    | **Parameter Description**               | **Must be returned or not** | **Detailed description**                                     |
| ------------------------- | ----------- | --------------------------------------- | --------------------------- | ------------------------------------------------------------ |
| riskLabel1                | string      | Must return when allLabels is not empty | Y                           | First risk label                                             |
| riskLabel2                | string      | Must return when allLabels is not empty | Y                           | Second risk label                                            |
| riskLabel3                | string      | Must return when allLabels is not empty | Y                           | Third risk label                                             |
| riskDescription           | string      | Must return when allLabels is not empty | Y                           | Risk description                                             |
| probability               | float       | Confidence                              | Y                           | The optional value is between 0 and 1. The higher the value, the higher the credibility. Note: return when allLabels is not empty |
| riskDetail                | json_object | Risk details                            | Y                           | The format is the same as that of the upper riskDetail structure <br/>Note: All Labels must be returned when they are not empty |
| riskLevel                 | string      | Risk level                              | Y                           | Possible return values:<br/>`REVIEW`：Suspicious<br/>`REJECT`：Violation |

<span id="businessLabels">The contents of businessLabels are as follows:</span>

| **Return parameter name** | **Type**    | **Parameter Description**                                    | **Must be returned or not** | **Detailed description** |
| ------------------------- | ----------- | ------------------------------------------------------------ | --------------------------- | ------------------------ |
| businessLabel1            | string      | Must return when businessLabels is not empty                 | Y                           | First Business label     |
| businessLabel2            | string      | Must return when businessLabels is not empty                 | Y                           | Second Business label    |
| businessLabel3            | string      | Must return when businessLabels is not empty                 | Y                           | Third Business label     |
| businessDescription       | string      | Must return when businessLabels is not empty                 | Y                           | Label description        |
| probability               | float       | Must return when businessLabels is not empty<br/>The optional value is between 0 and 1. The higher the value, the higher the credibility | Y                           | Confidence               |
| businessDetail            | Json_object | Must return when businessLabels is not empty                 | Y                           | Business details         |

<span id="tokenProfileLabels">The contents of tokenProfileLabels and tokenRiskLabels are as follows:</span>

| **Return parameter name** | **Type** | **Parameter Description** | **Must be returned in or not** | **Detailed description**              |
| ------------------------- | -------- | ------------------------- | ------------------------------ | ------------------------------------- |
| label1                    | string   | First Label               | N                              |                                       |
| label2                    | string   | Second Label              | N                              |                                       |
| label3                    | string   | Third Label               | N                              |                                       |
| description               | string   | Label description         | N                              |                                       |
| timestamp                 | Int      | Timestamp marked as label | N                              | 13-bit Unix timestamp in milliseconds |

When the value of lang field is zh, or the value of auto is recognized as Chinese, the contents of the primary label are as follows:

| FirstLabelId | Type | Remark            |
| ------------ | ---- | ----------------- |
| politics     | Risk | type is TEXTRISK  |
| violence     | Risk | type is TEXTRISK  |
| porn         | Risk | type is TEXTRISK  |
| ban          | Risk | type is TEXTRISK  |
| abuse        | Risk | type is TEXTRISK  |
| ad_law       | Risk | type is TEXTRISK  |
| ad           | Risk | type is TEXTRISK  |
| blacklist    | Risk | type is TEXTRISK  |
| meaningless  | Risk | type is TEXTRISK  |
| privacy      | Risk | type is TEXTRISK  |
| fraud        | Risk | type is FRUAD     |
| minor        | Risk | type is TEXTMINOR |

When it is not in Chinese, the contents of the primary label are as follows:

| FirstLabel | FirstLabelId | Type | Remark           |
| ---------- | ------------ | ---- | ---------------- |
| Politics   | Politics     | Risk | type is TEXTRISK |
| Violence   | Violence     | Risk | type is TEXTRISK |
| Erotic     | Erotic       | Risk | type is TEXTRISK |
| Prohibit   | Prohibit     | Risk | type is TEXTRISK |
| Abuse      | Abuse        | Risk | type is TEXTRISK |
| Ads        | Ads          | Risk | type is TEXTRISK |
| Blacklist  | Blacklist    | Risk | type is TEXTRISK |

## Demo

### Sample request code

```json
{
    "accessKey":"*************",
    "appId":"default",
    "eventId":"text",
	"type":"TEXTRISK",
    "data":
    {
        "text":"Contect me My whatsapp12345",
        "tokenId":"4567898765jhgfdsa",
        "ip":"118.89.214.89",
        "deviceId":"*************",
        "nickname":"***********",
        "extra":
        {
            "topic":"12345",
            "atId":"username1",
            "room":"test123",
            "receiveTokenId":"username2",
            "role":"USER"
    	}
    },
    "acceptLang":"en"
}
```

### Sample return code

```json
{
    "allLabels":[
        {
            "probability":1,
            "riskDescription":"Abuse:Abuse:Abuse",
            "riskDetail":{

            },
            "riskLabel1":"Abuse",
            "riskLabel2":"Abuse",
            "riskLabel3":"Abuse",
            "riskLevel":"REVIEW"
        },
        {
            "probability":0.95559550232975,
            "riskDescription":"Politics:HateSpeech:BlackRace",
            "riskDetail":{

            },
            "riskLabel1":"Politics",
            "riskLabel2":"HateSpeech",
            "riskLabel3":"BlackRace",
            "riskLevel":"REJECT"
        },
        {
            "probability":1,
            "riskDescription":"Ads:ContactInformation:WhatsApp",
            "riskDetail":{

            },
            "riskLabel1":"Ads",
            "riskLabel2":"ContactInformation",
            "riskLabel3":"WhatsApp",
            "riskLevel":"REJECT"
        }
    ],
    "auxInfo":{
        "contactResult":[
            {
                "contactString":"whatsapp12345",
                "contactType":2
            }
        ],
        "filteredText":"Contect me My whatsapp12345"
    },
    "businessLabels":[

    ],
    "code":1100,
    "message":"Success",
    "requestId":"bb917ec5fa11fd02d226fb384968feb1",
    "riskDescription":"Ads:ContactInformation:WhatsApp",
    "riskDetail":{

    },
    "riskLabel1":"ad",
    "riskLabel2":"ContactInformation",
    "riskLabel3":"WhatsApp",
    "riskLevel":"REJECT"
}
```
