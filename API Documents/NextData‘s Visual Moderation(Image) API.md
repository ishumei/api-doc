# NextData‘s Visual Moderation(Image) API


# Single task upload synchronization interface

## Submit a Task Synchronously

### Request URL:
| Colony | URL |
| --- | --- |
| Peking | `http://api-img-bj.fengkongcloud.com/image/v4` |
| Shanghai | `http://api-img-sh.fengkongcloud.com/image/v4` |
| Singapore | `http://api-img-xjp.fengkongcloud.com/image/v4` |
| India | `http://api-img-yd.fengkongcloud.com/image/v4` |
| Silicon valley | `http://api-img-gg.fengkongcloud.com/image/v4` |

### Request method:

`POST`

### Character encoding:

`UTF-8`

### Recommended timeout:

5s

### Request parameters:

The parameters are placed in the HTTP Body in Json format. The specific parameters are as follows:

| **Request parameter name** | **Data type** | **Parameter Description** | **Must be transferred in or not** | **Detailed description** |
| :-- | :-- | :-- | --- | --- |
| accessKey | string | Interface authentication key<br/>It is used for authority authentication. | Y | When opening account service, it is provided by NextData or checked at the relevant documents in the upper right corner of the background of NextData by using the opening mailbox |
| appId | string | Application ID, used to distinguish different application data of the same company | Y | If you need to contact us to open, please refer to the value provided by NextData separately |
| eventId | string | identifier for the event | Y | If you need to contact us to open, please refer to the value provided by NextData separately |
| type | string | Types of risks detected | N | **<u>Optional values of supervision level 1 label:</u>**<br/>***POLITY*** :Identify political figures, sensitive political events, etc<br/>***EROTIC*** :Identify NSFW, pornographic content, etc <br/>***VIOLENT*** :Identify violence, terrorists, drugs, guns, etc <br/>***QRCODE*** :Identify QR code<br/>***ADVERT*** :Identify spams, ads<br/>***IMGTEXTRISK*** :Identify OCR violations<br/>If more than one function needs to be identified, connect it by underline, such as **POLITY_QRCODE_ADVERT** which means to identify politics, QR code and spams at the same time<br/>*（Note: This field and the businessType field must select an incoming）*<br/>Political, pornographic and violent terrorism only include the detection of the violation of the picture itself. If it is necessary to identify the violation of the text in the picture, be sure to transfer in type IMGTEXTRISK |
| businessType | string | Types used for business and operations | N | Optional values: [Please see in appendix](#Appendix)<br/> If multiple recognition functions are required, the field and type must be selected to be passed in |
| data | json_object | Requested data content | Y | The length of the requested data field is up to 10MB,[See **data** parameter for details](#data) |
| callback | string | Callback request url, send callback to indicate asynchronous callback logic, otherwise follow synchronous logic, callback the http address field. When this field is not empty, the service will notify the user of the audit result based on the callback of this field. The address must be the url of http or https specification | N | Asynchronous callback logic supports 30M image size<br/>synchronous support 10M image size<br/>asynchronous single and asynchronous batch need to call the query interface to query the results; The synchronization interface cannot call the query. If the callback is sent, the result will be returned to the corresponding server. If the callback is not sent, the synchronization will be returned |
| acceptLang | string | Returns the language type of interface information | N | Optional language type of returned information:<br/>zh: Chinese<br/>en: English<br/>The default is zh. If you need to return results in English, this field is required |

<span id="data">The content of "data" is as follows:</span>

| **Request parameter name** | **Data type** | **Parameter Description** | **Must be transferred in or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| tokenId | string | It is recommended to use your company's UID (encryptable) to generate the user account identification. The identification of the user's unique identity is used for the risk control of behavior dimensions such as irrigation and advertising. If there is no uid, it is recommended to use a unique data identifier to transfer values | Y | A string composed of numbers, letters, underscores, and dashes with a length less than or equal to 64 bits |
| img | string | The image to be detected can use base64 encoded image data or image url link **It is recommended to download pictures from the CDN origin, and the origin cannot be a single point<br/>Risk: If it is not downloaded from the source site, there may be image download failure, resulting in failure to audit** | Y | Supported formats:<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>It is recommended that the pixel size of the picture should not be less than 256 * 256. Currently, it supports pictures with resolution between 20 * 20~6000 * 6000. The maximum size of the picture is 10MB, and the maximum asynchronous size is 30M <br/>By default, the long image is not split. If necessary, please contact Next Data to open it. The charge after splitting is subject to the actual number of frames intercepted. |
| imgCompareBase | string | To detect the benchmark image for comparison, it exists when the request parameter Type field contains the label`FACECOMPARE`<br/>You can use base64 encoded image data or image url links | N | Supported formats:<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>It is recommended that the pixel size of the picture is not less than 256 * 256, and the maximum size of the picture is 10MB<br/><br/>The benchmark map temporarily does not support the format of long map and dynamic map |
| role | string | User role | N | User roles must be valid in the optional range. Different policies can be configured for different roles.(Default is `USER`)<br/>The value of live broadcast field can be:<br/>`ADMIN`:administrator<br/>`HOST`:host<br/>`SYSTEM`:system role<br/>The value of game field can be:<br/>`ADMIN`:administrators<br/>`USER`:user |
| ip | string | ip address | N | The public network IPv4 address of the user who sent the image |
| lang | string | Language type | N | When IMGTEXTRISK is included in the request type, the corresponding detection language type can be specified. Optional values:<br/>zh:Chinese<br/>en:English<br/>ar:Arabic<br/>The default detection is Chinese. Please note that the identification other than the optional value is not valid |
| deviceId | string | Digital device fingerprint identification | N | Unique identification of the device generated by the digital device fingerprint |
| maxFrame | int | Maximum number of frames of GIF image | N | The maximum number of frames intercepted is 20 frames, and the default is 3 frames. The charge is based on the actual number of frames intercepted. If the default is to intercept 3 frames, the charge is based on 3 frames. |
| interval | int | Detection interval of GIF frame | N                                 | The default value is`1`, which means that every frame needs to be detected. The service will automatically adjust this value to ensure full coverage of all frames |
| extra | json_object | Auxiliary parameter | N | Relevant information for auxiliary detection,[See the **extra** parameter for details](#extra) |
| streamInfo | json_object | Similar frame audit parameter | N | Relevant information used to detect similar frames,[See **streamInfo** parameter for details](#streamInfo)，If you need to understand or use the similar frame function, please contact customer service for consultation. |
| receiveTokenId | string | TokenId of receiver | N | TokenId of the receiver, mandatory for private chat scenario |

<span id="streamInfo">In data, the content of streamInfo is as follows:</span>

| ****Request parameter name**** | **Parameter type** | **Parameter Description** | **Must be transferred in or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| similarDedup | bool | Whether to enable similar functions | N |  |
| streamId | string | Transparent transmission parameter | N | When the similarDedup is true, it must be transfered in |
| timeWindow | int | Transparent transmission parameter | N |Time window, in seconds. When the similarDedup is true, it must be transfered in  |
| frameTime | int | Transparent transmission parameter | N | Frame truncation time, unit: ms, When the similarDedup is true, it must be transfered in |
| riskNum | int | Transparent transmission parameter | N | Similar frames do not change the number range (1-5), the default is 1 |

<span id="extra">In data, the content of extra is as follows:</span>

| **Request parameter name** | **Parameter type** | **Parameter Description** | Must be transferred in or not | **Detailed description** |
| --- | --- | --- | --- | --- |
| isIgnoreTls | bool | Auxiliary parameter to control whether to ignore the validation of ca certificate | N | Optional value (default`false`)：<br/>`true`：Ignore certificate trust<br/>`false`：Verification certificate |
| passThrough | json_object | Transparent transmission parameter | N | The pass-through field is passed in by the customer, and the field is not recognized and processed by us. It is returned to the user with the result, which must be json_ Object type |
| isTokenSeparate | int | Whether to distinguish accounts under different applications | N | Whether to distinguish accounts under different applications, possible values:<br/>0:Indistinguished<br/>1:distinguished<br/>Default is 0 |
| room | string | The room number is recommended for live broadcast, high exposure group chat and other business scenarios | N |  |
## Synchronize return results

It is placed in the HTTP Body in Json format. The specific parameters are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| code | int | Return code | Y | `1100`：Success<br/>`1901`：Request Limit Exceeded<br/>`1902`：Invalid Parameters<br/>`1903`：Internal Server Error<br/>`1911`：Picture Download Failed<br/>`9101`：Operation Denied<br/> |
| message | string | Return code description | Y | Corresponding to code, include: <br/>Success<br/>Request Limit Exceeded<br/>Invalid Parameters<br/>Internal Server Error<br/>Picture Download Failed<br/>Operation Denied |
| requestId | string | Request ID | Y | Request a unique identifier for troubleshooting and subsequent effect optimization. It is strongly recommended to save |
| riskLevel | string | Disposal suggestions | Y | Possible return values:<br/>`PASS`: Normal, direct release recommended<br/>`REVIEW`: Suspicious, manual review recommended<br/>`REJECT`: Violation, direct interception is recommended |
| riskLabel1 | string | Level 1 risk label | Y | Returns `normal`when riskLevel is`PASS` |
| riskLabel2 | string | Level 2 risk label | Y | Null when riskLevel is`PASS` |
| riskLabel3 | string | Level 3 risk label | Y | Null when riskLevel is`PASS` |
| riskDescription | string | Risk description | Y | Normal when riskLevel is`PASS` |
| riskDetail | json_object | Risk details | Y | [See riskDetail parameter for details](#riskDetail) |
| auxInfo | json_object | Other auxiliary information | Y | [See auxinfo parameter for details](#auxInfo) |
| allLabels | json_array | All label details | Y                           | Return all tags and details of hits |
| businessLabels | json_array | Business tag details | N | When only identification is done, the result of not configuring the reject and review policies is returned here,[See businessLabels parameter for details](#businessLabels) |
| tokenProfileLabels | json_array | Auxiliary information | N | Attribute account class label.[See account tag parameters for details](#tokenProfileLabels) |
| tokenRiskLabels | json_array | Auxiliary information | N | Risk account type label.[See account tag parameters for details](#tokenProfileLabels) |
| tokenLabels    | json_object  | Account tag information | N | See the details below. It is only returned when **tokenId** is transfered in and connected to the service of Next Data |

<span id="riskDetail">The riskDetail structure is as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| faces | json_array | Return the name and location information of the political figures in the picture | N |  |
| face_num | int | Number of faces | N |  |
| persons | json_array | Only when the portrait - multiple people are hit, there will be multiple array elements, up to 10 | N | |
| person_num | int | Number of portraits | N | There are only portraits - multiple people return |
| objects | json_array | Return the location information of the item or logo QR code in the picture | N | Array can only have one element |
| ocrText | json_object | Returns information about illegal text in the picture. It exists when the request parameter type field contains`IMGTEXTRISK`and ADVERT | N | |
| riskSource | int | Identify where the resource violates | Y | Identify the source of risk results<br/>`1000`：No risk<br/>`1001`：Text risk<br/>`1002`：Visual picture risk |

In riskDetail, the contents of each element of the faces array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| id | string | Person number | N | People in the same position in the picture have the same number under different labels.<br/>If the same person appears n times in the picture, assign n IDs |
| name | string | Character name | N | Identifiable public figure name |
| location | int_array | The array has four values, representing the coordinates of the upper left corner and the lower right corner. for example<br/>[207,522,340,567]<br/>207 Represents the x coordinate of the upper left corner<br/>522 Represents the y coordinate of the upper left corner<br/>340 Represents the x coordinate of the lower right corner<br/>567 Represents the y coordinate of the lower right corner | N |  |
| face_ratio | float | Proportion of faces | N | |
| probability | float | Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | N | Floating point number between 0 and 1 |

In riskDetail, the contents of each element of the objects array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| id | string | Number to ensure that the number of items in the same location is the same under different labels | N |  |
| name | string | Identification name N | N                           | |
| location | int_array | Identifies the location information. The array has four values, representing the coordinates of the upper left corner and the lower right corner. [207,522,340,567]<br/>207 Represents the x coordinate of the upper left corner<br/>522 Represents the y coordinate of the upper left corner<br/>340 Represents the x coordinate of the lower right corner<br/>567 Represents the y coordinate of the lower right corner | N | |
| probability | float | Degree of Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | N | Floating point number between 0 and 1 |
| qrContent | string | URL information of QR code | N | Only returned when the QR code related tag is hit |

In riskDetail, the contents of each element of the persons array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| id | string | Number, ensure that the same person has the same number under different labels. If the same person appears n times in the picture, assign n IDs | N |  |
| person_ratio | string | Proportion of portrait in the picture | N | |
| location | int_array | Position coordinates of portrait | N | |
| probability | float | Degree of Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | N | Floating point number between 0 and 1 |

In riskDetail, the content of ocrText is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| text | string | Recognized text | Y | |
| matchedLists | json\_array | Hit customer custom list list | N | |
| riskSegments | json\_array | High-risk segment content exists when the detection picture contains risk content such as politics, terrorism, prohibition, advertising law, etc | N |  |

In riskDetail, the content of ocrText is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| name | string | Hit list name | N | |
| words | json\_array | Hit sensitive word information | N | |

In matchedLists, the contents of each element of the words array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| word | string | Hit sensitive words | N | |
| position | int_array | Location of sensitive words | N | |

In ocrText, the details of each element of riskSegments are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| segment | string | High-risk content fragments | N | |
| position | int_array | Location of high-risk content fragments | N | |

<span id="auxInfo">The content of auxInfo is as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| segments | int | Number of pictures actually processed | Y | |
| typeVersion | json_object | The effect version number for each incoming type | Y | |
| errorCode | int | | N | `2001`：The input data format is incorrect, and it is not legal json data<br/>`2002`：The entered parameter field is illegal (required fields are missing, wrong type, illegal value, etc.)<br/>`2003`：Picture download failed<br/>`2004`：The picture is too large, more than 10M<br/>`2005`：Illegal picture format<br/>`2006`：Invalid risk monitoring type |
| passThrough | json_object | The pass-through field passed in by the customer | N |  |
| streamId | string | When the [streamInfo](#streamInfo) function is passed in the request parameter, similar functions will be returned | N |  |
| frameTime | int | When the [streamInfo](#streamInfo) function is passed in the request parameter, similar functions will be returned | N |  |
| qrContent | string | Return the identified QR code address | N | It is required to contact Next Data Configuration to return |

In auxInfo, the content of typeVersion is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| POLITY | string | Political version number | N | The composition form is`X.Y`, and`X`is the main version number, which generally represents the overall effect iteration of the model` Y ` is the sub-version number, which generally represents the routine iteration<br/>For example, `1001001.2` means the major version number is`1001001`and the sub-version number is`2` |
| VIOLENT | string | Terrorist version number | N | The composition is the same as above |
| EROTIC | string | Pornographic version number | N | The composition is the same as above |
| ADVERT | string | Advertising version number | N | The composition is the same as above |
| IMGTEXTRISK | string | Illegal text version number | N | The composition is the same as above |
| QRCODE | string | QR code version number | N | The composition is the same as above |

The contents of each member of the allLabels array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| riskLabel1 | string | Level 1 risk label | Y |  |
| riskLabel2 | string | Level 2 risk label | Y |  |
| riskLabel3 | string | Level 3 risk label | Y |  |
| riskDescription | string | Risk causes | Y | |
| riskLevel | string | Disposal suggestions | Y | |
| probability | float | Degree of Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | Y |  |
| riskDetail | json_object | See the riskDetail under result for risk details and field contents | Y |  |

<span id="businessLabels">The contents of each member of the businessLabels array are as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| businessLabel1 | string | Level 1 business label | Y |  |
| businessLabel2 | string | Level 2 business label | Y |  |
| businessLabel3 | string | Level 3 business label | Y |  |
| businessDescription | string | Business tag description | Y |  |
| businessDetail | json_object | Business tag details | N | See the following businessDetail structure for the format |
| probability | float | Degree of Confidence<br/>The optional value is between 0 and 1. The higher the value, the higher the confidence | Y | |
| confidenceLevel | int | Confidence level<br/>The optional value is between 0 and 2. The higher the value, the higher the credibility | N | |

The contents of businessDetail in the businessLabels array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| name | string | Star character name<br/>The star name type in the picture exists when the passed value contains`FACE` | N |  |
| probability | float | Star character confidence interval<br/>The optional value is between 0 and 1. The higher the value, the higher the credibility. It appears only when name exists | N |  |
| face_ratio | float | Proportion of faces<br/>In the range 0-1, the larger the value, the higher the proportion of faces. The type exists when the value contains`FACE` | N |  |
| faces | json_array | The content is consistent with the format of the outer **riskDetail.faces**, and the internal fields refer to the faces field under the outer **riskDetail** | N | |
| objects | json_array | In other cases, there is only one array element identification information, which returns the name and location information of the identification or item in the picture. The content is consistent with the format of the outer **riskDetail.objects** | N | Array can only have one element |
| persons | json_array | Only when the portrait - multiple people are hit, there will be multiple array elements, up to 10 (if more than 10, select the 10 with the highest **probability**). In other cases, only has one element, and the internal field refers to the **persons** field under the outer **riskDetail** | N |  |
| face_num | int | In other cases, there is only one array element to detect the number of faces detected in the image, Only when hitting face-face type-multi-face, there will be multiple array elements, up to 10 (if more than 10, select the 10 with the highest **probability**) | N | |
| face_compare_num | int | Face comparison face number detection, The number of faces detected in the image exists when the **businessType** value contains`FACECOMPARE` | N | |
| location | int_array | The identification location information<br/>type value contains `OBJECT` and exists from time to time. This array has four values, representing the coordinates of the upper left corner and the lower right corner. For example [207,522,340,567]<br/>207 represents the x coordinate of the upper left corner<br/>522 represents the y coordinate of the upper left corner<br/>340 represents the x coordinate of the lower right corner<br/>567 represents the y coordinate of the lower right corner | N |  |
| person_num | int | Human body number detection | N | Only return when hitting multiple tags |
| person_ratio | float | Portrait ratio<br/>In the range of 0-1, the larger the value, the higher the face ratio | N | |

The details of tokenLabels are as follows:

| **Parameter name**   | **Parameter type** | **Parameter Description**        | **Must be returned or not** | **Detailed description**                     |
| -------------------- | ------------------ | -------------------------------- | --------------------------- | -------------------------------------------- |
| machine_account_risk | json_object        | Risks related to machine control | N                           |                                              |
| UGC_account_risk     | json_object        | Risks related to UGC content     | N                           |                                              |
| scene_account_risk   | json_object        | Scenario account risk            | N                           | Only in special situations, such as airlines |

<span id="tokenProfileLabels">The contents of tokenProfileLabels and tokenRiskLabels are as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description**              |
| ------------------ | ------------------ | ------------------------- | --------------------------- | ------------------------------------- |
| label1             | string             | Level 1 label             | N                           |                                       |
| label2             | string             | Level 2 label             | N                           |                                       |
| label3             | string             | Level 3 label             | N                           |                                       |
| description        | string             | Label description         | N                           |                                       |
| timestamp          | Int                | Label time stamp          | N                           | 13-bit Unix timestamp in milliseconds |

 The details of machine_ account_risk are as follows:

| **Parameter name**                | **Parameter type** | **Parameter Description**   | **Must be returned or not** | **Detailed description**                                     |
| --------------------------------- | ------------------ | --------------------------- | --------------------------- | ------------------------------------------------------------ |
| b_machine_control_tokenid         | int                | Machine account             | N                           | 0: non-machine control account<br/> 1: machine control account |
| b_machine_control_tokenid_last_ts | int                | Machine account time        | N                           |                                                              |
| b_offer_wall_tokenid              | int                | Point wall account          | N                           | 0: non-integral wall account <br/>1: integral wall account   |
| b_offer_wall_tokenid_last_ts      | int                | Time of credit wall account | N                           |                                                              |

The details of UGC_account_risk are as follows:

| **Parameter name**               | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Must be returned or not**                                  |
| -------------------------------- | ------------------ | ------------------------- | --------------------------- | ------------------------------------------------------------ |
| b_politics_risk_tokenid          | int                | Political risks           | N                           | 0: No political-related risks found temporarily<br/>1: There are political-related risks |
| b_politics_risk_tokenid_last_ts  | int                | Political risk time       | N                           |                                                              |
| b_sexy_risk_tokenid              | int                | Pornographic risk         | N                           | 0: No pornographic risk found temporarily <br/>1: There is pornographic risk |
| b_sexy_risk_tokenid_last_ts      | int                | Pornographic risk time    | N                           |                                                              |
| b_advertise_risk_tokenid         | int                | Advertising risk          | N                           | 0: No advertising risk found<br/> 1: Advertising risk exists |
| b_advertise_risk_tokenid_last_ts | int                | Advertising risk time     | N                           |                                                              |

The details of scene_ account_ risk are as follows:

| **Parameter name** | **Must be returned or not** | **Must be returned or not** | **Must be returned or not** | **Must be returned or not** |
| --------------------------- | ------------------ | ------------------ | -------------------------------- | -------------------------------- |
| i_tout_risk_tokenid         | int                | Airline account | N      | 0: non-airline seat account <br/>1: airline seat account |
| i_tout_risk_tokenid_last_ts | int                | The airline company's seat occupancy time | N      |                                  |

## Synchronization return parameter of callback

| **Parameter name** | **Must be returned or not** | **Must be returned or not** | **Must be returned or not** | **Must be returned or not** |
| --- | --- | --- | --- | --- |
| code | int | Return code | Y | `1100`：Success<br/>`1901`：Request Limit Exceeded<br/>`1902`：Invalid Parameters<br/>`1903`：Internal Server Error<br/>`9101`：Operation Denied |
| message | string | Return code description | Y | Corresponding to code, include: Success<br/>Request Limit Exceeded<br/>Invalid Parameters<br/>Internal Server Error<br/>Operation Denied |
| requestId | string | Request ID | Y | Serial number of the picture |

If the callback protocol interface URL callback is specified in the request parameters, the POST method needs to be supported. The transmission code is utf-8. The audit results are placed in the HTTP Body in Json format. The specific parameters are the same as the Single synchronization request results.

## Demo

### Request:

```json
{
    "accessKey":"",
    "appId":"",
    "eventId":"",
    "type":"",
    "data":{
        "tokenId":"username123",
        "img":""
    },
    "acceptLang":"en"
}
```

### Return:

```json
{
    "requestId":"55cf0374642c5f2336ccb107aa9005e5",
    "code":1100,
    "message":"Success",
    "riskLevel":"REJECT",
    "riskLabel1":"violence",
    "riskLabel2":"SceneOfViolenceAndTerror",
    "riskLabel3":"SceneOfViolenceAndTerror",
    "riskDescription":"violence:SceneOfViolenceAndTerror:SceneOfViolenceAndTerror",
    "riskDetail":{
        "faces":[
            {
                "face_ratio":0.00357499998062849,
                "id":"be82442eaf2fe5fcaba84e7f2b3b1dbc",
                "location":[
                    403,
                    171,
                    436,
                    210
                ],
                "name":"习近平",
                "probability":0.803125739097595
            }
        ],
        "riskSource":1002
    },
    "auxInfo":{
        "segments":1,
        "typeVersion":{
            "POLITICS":"2014014.1",
            "VIOLENCE":"2012008.1",
            "BAN":"1002102.1",
            "PORN":"3048002.1"
        }
    },
    "allLabels":[
       {
            "riskLabel1":"politics",
            "riskLabel2":"PresidentOfChina",
            "riskLabel3":"PresidentOfChina Cartoon",
            "riskLevel":"REVIEW"
        },
        {
            "probability":0.867621600627899,
            "riskDescription":"politics：politics：politics",
            "riskDetail":{
                "riskSocrce":1002
            },
            "riskLabel1":"politics",
            "riskLabel2":"politics",
            "riskLabel3":"politics",
            "riskLevel":"REJECT"
        },
        {
            "probability":0.855094926869849,
            "riskDescription":"politics:PresidentOfChina:PresidentOfChina",
            "riskDetail":{
                "faces":[
                    {
                        "face_ratio":0.00357499998062849,
                        "id":"be82442eaf2fe5fcaba84e7f2b3b1dbc",
                        "location":[
                            403,
                            171,
                            436,
                            210
                        ],
                        "name":"习近平",
                        "probability":0.803125739097595
                    }
                ],
                "riskSocrce":1002
            },
            "riskLabel1":"politics",
            "riskLabel2":"PresidentOfChina",
            "riskLabel3":"PresidentOfChina",
            "riskLevel":"REJECT"
        }
    ],
    "businessLabels":[
        {
            "businessDescription":"Face:Face Posture:Front Face",
            "businessDetail":{

            },
            "businessLabel1":"Face",
            "businessLabel2":"FacePosture",
            "businessLabel3":"FrontFace",
            "confidenceLevel":1,
            "probability":0.450656906102068
        },
        {
            "businessDescription":"Face:Face Type:Multiple Faces",
            "businessDetail":{

            },
            "businessLabel1":"Face",
            "businessLabel2":"FaceType",
            "businessLabel3":"MultipleFaces",
            "confidenceLevel":1,
            "probability":0.458568899401581
        },
        {
            "businessDescription":"Face:FaceType:RealPeople",
            "businessDetail":{

            },
            "businessLabel1":"Face",
            "businessLabel2":"FaceType",
            "businessLabel3":"RealPeople",
            "confidenceLevel":2,
            "probability":0.867621600627899
        }
    ],
    "tokenLabels":{
        "UGC_account_risk":{

        }
    }
}
```

### Synchronization return parameter of callback
```json
{
    "code":1100,
    "message":"Success",
    "requestId":"69dbc1f81dc5c914b1f1b8a267fb9ec1"
}
```
When the user's server receives the push result and returns an HTTP status code of 200, it means that the push is successful. Otherwise, the system will retry the push (until the maximum number of retries is reached). The retry logic is to retry after [1, 2, 3, 4, 5, 6, 7, 8] seconds. If it fails after 8 times, it will not be retried.

### Callback request demo

```json
{
    "requestId":"testcallback002",
    "code":1100,
    "message":"Success",
    "riskLevel":"REJECT",
    "riskLabel1":"violence",
    "riskLabel2":"SceneOfViolenceAndTerror",
    "riskLabel3":"Bloody",
    "riskDescription":"violence:SceneOfViolenceAndTerror:Bloody",
    "riskDetail":{
        "riskSource":1002
    },
    "auxInfo":{
        "segments":1,
        "typeVersion":{

        }
    },
    "allLabels":[
        {
            "probability":0.989746093754127,
            "riskDescription":"violence:SceneOfViolenceAndTerror:Bloody",
            "riskDetail":{
                "riskSource":1002
            },
            "riskLabel1":"violence",
            "riskLabel2":"SceneOfViolenceAndTerror",
            "riskLabel3":"Bloody",
            "riskLevel":"REJECT"
        }
    ],
    "tokenLabels":{
        "UGC_account_risk":{

        }
    }
}
```


# Asynchronous single upload interface

## Asynchronous single request

### Request URL:
| Colony | URL | Product |
| --- | --- | --- |
| Peking | `http://api-img-bj.fengkongcloud.com/v4/saas/async/img` | Image |
| Shanghai | `http://api-img-sh.fengkongcloud.com/v4/saas/async/img` | Image |

### Request method:

`POST`

### Character encoding:

`UTF-8`

### Recommended timeout:

5s

### Request parameters:

It is placed in the HTTP Body in Json format. The specific parameters are as follows:

| **Request parameter name** | **Data type** | **Parameter Description**                                    | **Must be transferred in or not** | **Detailed description**                                     |
| :------------------------- | :------------ | :----------------------------------------------------------- | --------------------------------- | ------------------------------------------------------------ |
| accessKey                  | string        | Interface authentication key<br/>It is used for authority authentication. | Y                                 | When opening account service, it is provided by NextData or checked at the relevant documents in the upper right corner of the background of NextData by using the opening mailbox |
| appId                      | string        | Application ID, used to distinguish different application data of the same company | Y                                 | If you need to contact us to open, please refer to the value provided by NextData separately |
| eventId                    | string        | identifier for the event                                     | Y                                 | If you need to contact us to open, please refer to the value provided by NextData separately |
| type                       | string        | Types of risks detected                                      | N                                 | **<u>Optional values of supervision level 1 label:</u>**<br/>***POLITY*** :Identify political figures, sensitive political events, etc<br/>***EROTIC*** :Identify NSFW, pornographic content, etc <br/>***VIOLENT*** :Identify violence, terrorists, drugs, guns, etc <br/>***QRCODE*** :Identify QR code<br/>***ADVERT*** :Identify spams, ads<br/>***IMGTEXTRISK*** :Identify OCR violations<br/>If more than one function needs to be identified, connect it by underline, such as **POLITY_QRCODE_ADVERT** which means to identify politics, QR code and spams at the same time<br/>*（Note: This field and the businessType field must select an incoming）*<br/>Political, pornographic and violent terrorism only include the detection of the violation of the picture itself. If it is necessary to identify the violation of the text in the picture, be sure to transfer in type IMGTEXTRISK |
| businessType               | string        | Types used for business and operations                       | N                                 | Optional values: [Please see in appendix](#Appendix)<br/> If multiple recognition functions are required, the field and type must be selected to be passed in |
| data                       | json_object   | Requested data content                                       | Y                                 | The length of the requested data field is up to 10MB,[See **data** parameter for details](#data) |
| callback                   | string        | Callback request url, send callback to indicate asynchronous callback logic, otherwise follow synchronous logic, callback the http address field. When this field is not empty, the service will notify the user of the audit result based on the callback of this field. The address must be the url of http or https specification | N                                 | Asynchronous callback logic supports 30M image size<br/>synchronous support 10M image size<br/>asynchronous single and asynchronous batch need to call the query interface to query the results; The synchronization interface cannot call the query. If the callback is sent, the result will be returned to the corresponding server. If the callback is not sent, the synchronization will be returned |
| acceptLang                 | string        | Returns the language type of interface information           | N                                 | Optional language type of returned information:<br/>zh: Chinese<br/>en: English<br/>The default is zh. If you need to return results in English, this field is required |

The content of data is as follows:

| **Request parameter name** | **Data type** | **Parameter Description**                                    | **Must be transferred in or not** | **Detailed description**                                     |
| -------------------------- | ------------- | ------------------------------------------------------------ | --------------------------------- | ------------------------------------------------------------ |
| tokenId                    | string        | It is recommended to use your company's UID (encryptable) to generate the user account identification. The identification of the user's unique identity is used for the risk control of behavior dimensions such as irrigation and advertising. If there is no uid, it is recommended to use a unique data identifier to transfer values | Y                                 | A string composed of numbers, letters, underscores, and dashes with a length less than or equal to 64 bits |
| img                        | string        | The image to be detected can use base64 encoded image data or image url link **It is recommended to download pictures from the CDN origin, and the origin cannot be a single point<br/>Risk: If it is not downloaded from the source site, there may be image download failure, resulting in failure to audit** | Y                                 | Supported formats:<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>It is recommended that the pixel size of the picture should not be less than 256 * 256. Currently, it supports pictures with resolution between 20 * 20~6000 * 6000. The maximum size of the picture is 10MB, and the maximum asynchronous size is 30M <br/>By default, the long image is not split. If necessary, please contact Next Data to open it. The charge after splitting is subject to the actual number of frames intercepted. |
| imgCompareBase             | string        | To detect the benchmark image for comparison, it exists when the request parameter Type field contains the label`FACECOMPARE`<br/>You can use base64 encoded image data or image url links | N                                 | Supported formats:<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>It is recommended that the pixel size of the picture is not less than 256 * 256, and the maximum size of the picture is 10MB<br/><br/>The benchmark map temporarily does not support the format of long map and dynamic map |
| role                       | string        | User role                                                    | N                                 | User roles must be valid in the optional range. Different policies can be configured for different roles.(Default is `USER`)<br/>The value of live broadcast field can be:<br/>`ADMIN`:administrator<br/>`HOST`:host<br/>`SYSTEM`:system role<br/>The value of game field can be:<br/>`ADMIN`:administrators<br/>`USER`:user |
| ip                         | string        | ip address                                                   | N                                 | The public network IPv4 address of the user who sent the image |
| deviceId                   | string        | Digital device fingerprint identification                    | N                                 | Unique identification of the device generated by the digital device fingerprint |
| maxFrame                   | int           | Maximum number of frames of GIF image                        | N                                 | The maximum number of frames intercepted is 20 frames, and the default is 3 frames. The charge is based on the actual number of frames intercepted. If the default is to intercept 3 frames, the charge is based on 3 frames. |
| interval                   | int           | Detection interval of GIF frame                              | N                                 | The default value is`1`, which means that every frame needs to be detected. The service will automatically adjust this value to ensure full coverage of all frames |
| extra                      | json_object   | Auxiliary parameter                                          | N                                 | Relevant information for auxiliary detection,[See the **extra** parameter for details](#extra) |
| receiveTokenId             | string        | TokenId of receiver                                          | N                                 | TokenId of the receiver, mandatory for private chat scenario |

<span id="extra">In data, the content of extra is as follows:</span>

| **Request parameter name** | **Parameter type** | **Parameter Description**                                    | Must be transferred in or not | **Detailed description**                                     |
| -------------------------- | ------------------ | ------------------------------------------------------------ | ----------------------------- | ------------------------------------------------------------ |
| isIgnoreTls                | bool               | Auxiliary parameter to control whether to ignore the validation of ca certificate | N                             | Optional value (default`false`)：<br/>`true`：Ignore certificate trust<br/>`false`：Verification certificate |
| passThrough                | json_object        | Transparent transmission parameter                           | N                             | The pass-through field is passed in by the customer, and the field is not recognized and processed by us. It is returned to the user with the result, which must be json_ Object type |
| room                       | string             | The room number is recommended for live broadcast, high exposure group chat and other business scenarios | N                             |                                                              |

## Return results

It is placed in the HTTP Body in Json format. The specific parameters are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ------------------------- | --------------------------- | ------------------------------------------------------------ |
| code               | int                | Return code               | Y                           | `1100`：Success<br/>`1901`：Request Limit Exceeded<br/>`1902`：Invalid Parameters<br/>`1903`：Internal Server Error<br/>`1911`：Picture Download Failed<br/>`9101`：Operation Denied |
| message            | string             | Return code description   | Y                           | Corresponding to code, include: <br/>Success<br/>Request Limit Exceeded<br/>Invalid Parameters<br/>Internal Server Error<br/>Picture Download Failed<br/>Operation Denied |
| requestId          | string             | Request ID                | Y                           | Request a unique identifier for troubleshooting and subsequent effect optimization. It is strongly recommended to save |

## Demo

### Request:
```json
{
    "accessKey":"",
    "eventId":"",
    "type":"",
    "data":{
        "tokenId":"username123",
        "img":""
    },
    "acceptLang":"en"
}
```

### Return:
```json
{
    "requestId":"9l25odfa5280c50f49f7c40988a1e400",
    "code":1100,
    "message":"Success"
}
```

# Synchronous batch interface

## Synchronize batch request parameters

### Request URL：

| Colony | URL | Product |
| --- | --- | --- |
| Peking | `http://api-img-bj.fengkongcloud.com/images/v4` | Image |
| Shanghai | `http://api-img-sh.fengkongcloud.com/images/v4` | Image |
| Singapore | `http://api-img-xjp.fengkongcloud.com/images/v4` | Image |
| India | `http://api-img-yd.fengkongcloud.com/images/v4` | Image |
| Silicon valley | `http://api-img-gg.fengkongcloud.com/images/v4` | Image |
### Request method:

`POST`

### Character encoding:

`UTF-8`

### Recommended timeout:

60s

### Request parameters:

It is placed in the HTTP Body in Json format. The specific parameters are as follows:

| **Request parameter name** | **parameter type** | **Parameter Description**                                    | **Must be transferred in or not** | **Detailed description**                                     |
| :------------------------- | :----------------- | :----------------------------------------------------------- | --------------------------------- | ------------------------------------------------------------ |
| accessKey                  | string             | Interface authentication key<br/>It is used for authority authentication. | Y                                 | When opening account service, it is provided by NextData or checked at the relevant documents in the upper right corner of the background of NextData by using the opening mailbox |
| appId                      | string             | Application ID, used to distinguish different application data of the same company | Y                                 | If you need to contact us to open, please refer to the value provided by NextData separately |
| eventId                    | string             | identifier for the event                                     | Y                                 | If you need to contact us to open, please refer to the value provided by NextData separately |
| type                       | string             | Types of risks detected                                      | N                                 | **<u>Optional values of supervision level 1 label:</u>**<br/>***POLITY*** :Identify political figures, sensitive political events, etc<br/>***EROTIC*** :Identify NSFW, pornographic content, etc <br/>***VIOLENT*** :Identify violence, terrorists, drugs, guns, etc <br/>***QRCODE*** :Identify QR code<br/>***ADVERT*** :Identify spams, ads<br/>***IMGTEXTRISK*** :Identify OCR violations<br/>If more than one function needs to be identified, connect it by underline, such as **POLITY_QRCODE_ADVERT** which means to identify politics, QR code and spams at the same time<br/>*（Note: This field and the businessType field must select an incoming）*<br/>Political, pornographic and violent terrorism only include the detection of the violation of the picture itself. If it is necessary to identify the violation of the text in the picture, be sure to transfer in type IMGTEXTRISK |
| businessType               | string             | Types used for business and operations                       | N                                 | Optional values: [Please see in appendix](#Appendix)<br/> If multiple recognition functions are required, the field and type must be selected to be passed in |
| data                       | json_object        | Requested data content                                       | Y                                 | The length of the requested data field is up to 10MB,[See **data** parameter for details](#data) |
| callback                   | string             | Callback request url, send callback to indicate asynchronous callback logic, otherwise follow synchronous logic, callback the http address field. When this field is not empty, the service will notify the user of the audit result based on the callback of this field. The address must be the url of http or https specification | N                                 | Asynchronous callback logic supports 30M image size<br/>synchronous support 10M image size<br/>asynchronous single and asynchronous batch need to call the query interface to query the results; The synchronization interface cannot call the query. If the callback is sent, the result will be returned to the corresponding server. If the callback is not sent, the synchronization will be returned |
| acceptLang                 | string             | Returns the language type of interface information           | N                                 | Optional language type of returned information:<br/>zh: Chinese<br/>en: English<br/>The default is zh. If you need to return results in English, this field is required |

<span id="data">The content of "data" is as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be transferred in or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| imgs | json_array | Array of pictures to detect | Y | |
| tokenId | string | User account identification | Y | Used to distinguish user accounts. It is recommended to pass in user IDs |
| ip | string | ipv4 address | N | The public network IPv4 address of the user who sent the picture |
| deviceId | string | Digital device fingerprint identification | N | The public network IPv4 address of the user who sent the picture |
| maxFrame | int | Maximum number of frames of gif image | N | The maximum number of frames of the captured git isokinetic image is 20, and the default is 3 frames. The charge is based on the actual number of frames. If the default is to capture 3 frames, the charge is based on 3 frames |
| interval | int | Frame truncation interval of gif picture | N | The default value is 1, which means that every frame needs to be detected. The service will automatically adjust this value to ensure full coverage of all frames |
| room | string | Live room number | N |  |
| extra | json_object | Auxiliary parameters | N | Relevant information for auxiliary text detection |
| role | string | User role | N | User roles must be valid in the optional range. Different policies can be configured for different roles.(Default is `USER`)<br/>The value of live broadcast field can be:<br/>`ADMIN`:administrator<br/>`HOST`:host<br/>`SYSTEM`:system role<br/>The value of game field can be:<br/>`ADMIN`:administrators<br/>`USER`:user |
| receiveTokenId | string | TokenId of receiver | N | TokenId of the receiver, mandatory for private chat scenario |

The content of extra is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be transferred in or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| passThrough | json_object | Transparent transmission parameter | N | The pass-through field is passed in by the customer, and the field is not recognized and processed by us. It is returned to the user with the result, which must be json_ Object type |
| isTokenSeparate | int | Whether to distinguish accounts under different applications | N | Whether to distinguish accounts under different applications, possible values:<br/>0:Indistinguished<br/>1:distinguished<br/>Default is 0 |

The specific contents of each element of the imgs array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be transferred in or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| img            | string   | The image to be detected can be downloaded from the CDN source site by using base64 encoded image data or the URL link of the image. The source site cannot be a single point<br/>Risk: If the image is not downloaded from the source site, there may be image download failure, resulting in failure to audit | Y     | Supported formats:<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>It is recommended that the pixel size of the picture be no less than 256 * 256. At present, the minimum resolution of the picture is 20 * 20, and the maximum size of the picture is 10MB<br/>By default, the long image is not split. If necessary, please contact Digital America to open it. The charge after splitting is subject to the actual number of frames intercepted. |
| imgCompareBase | string   | To detect the benchmark image for comparison, if the request parameter businessType field contains the label `FACECOMPARE`, there is<br/>image data or image url link that can be encoded using base64 | N   | Supported formats:<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>The number of batch requests should not exceed 12 at a time. It is recommended that the pixel size of the picture should not be less than 256 * 256, and the maximum size of the picture should be 10MB<br/><br/>The benchmark map temporarily does not support the format of long map and dynamic map |
| btId | string | Picture unique identification | Y | The same request cannot be repeated, and the length of btId is within 30 |

## Synchronous batch return parameters

It is placed in the HTTP Body in Json format. The specific parameters are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| code | int | Return code | Y |  `1100`：Success<br/>`1901`：Request Limit Exceeded<br/>`1902`：Invalid Parameters<br/>`1903`：Internal Server Error<br/>`1911`：Picture Download Failed<br/>`9101`：Operation Denied  |
| message | string | Return code description | Y | Corresponding to code, include: <br/>Success<br/>Request Limit Exceeded<br/>Invalid Parameters<br/>Internal Server Error<br/>Picture Download Failed<br/>Operation Denied |
| requestId | string | Request ID | Y | |
| imgs | json_array | Picture recognition results | Y | |
| auxInfo | json_object | Other auxiliary information | Y | |

The contents of the outer auxInfo are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| passThrough | json_object | Transparent parameter | Y | The pass-through field is passed in by the customer, and the field is not recognized and processed by the company. It is returned to the user with the result, which must be json_ Object type |

The specific contents of each element of the imgs array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**   | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | --------------------------- | --------------------------- | ------------------------------------------------------------ |
| code               | int                | Return code                 | Y                           | `1100`：Success<br/>`1901`：Request Limit Exceeded<br/>`1902`：Invalid Parameters<br/>`1903`：Internal Server Error<br/>`1911`：Picture Download Failed<br/>`9101`：Operation Denied |
| message            | string             | Return code description     | Y                           | Corresponding to code, include: <br/>Success<br/>Request Limit Exceeded<br/>Invalid Parameters<br/>Internal Server Error<br/>Picture Download Failed<br/>Operation Denied |
| requestId          | string             | Request ID                  | Y                           | Request a unique identifier for troubleshooting and subsequent effect optimization. It is strongly recommended to save |
| riskLevel          | string             | Disposal suggestions        | Y                           | Possible return values:<br/>`PASS`: Normal, direct release recommended<br/>`REVIEW`: Suspicious, manual review recommended<br/>`REJECT`: Violation, direct interception is recommended |
| riskLabel1         | string             | Level 1 risk label          | Y                           | Returns `normal`when riskLevel is`PASS`                      |
| riskLabel2         | string             | Level 2 risk label          | Y                           | Null when riskLevel is`PASS`                                 |
| riskLabel3         | string             | Level 3 risk label          | Y                           | Null when riskLevel is`PASS`                                 |
| riskDescription    | string             | Risk description            | Y                           | Normal when riskLevel is`PASS`                               |
| riskDetail         | json_object        | Risk details                | Y                           | [See riskDetail parameter for details](#riskDetail)          |
| auxInfo            | json_object        | Other auxiliary information | Y                           | [See auxinfo parameter for details](#auxInfo)                |
| allLabels          | json_array         | All label details           | Y                           | Return all tags and details of hits                          |
| businessLabels     | json_array         | Business tag details        | N                           | When only identification is done, the result of not configuring the reject and review policies is returned here,[See businessLabels parameter for details](#businessLabels) |

<span id="riskDetail">The riskDetail structure is as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| faces              | json_array         | Return the name and location information of the political figures in the picture | N                           |                                                              |
| face_num           | int                | Number of faces                                              | N                           |                                                              |
| persons            | json_array         | Only when the portrait - multiple people are hit, there will be multiple array elements, up to 10 | N                           |                                                              |
| person_num         | int                | Number of portraits                                          | N                           | There are only portraits - multiple people return            |
| objects            | json_array         | Return the location information of the item or logo QR code in the picture | N                           | Array can only have one element                              |
| ocrText            | json_object        | Returns information about illegal text in the picture. It exists when the request parameter type field contains`IMGTEXTRISK`and ADVERT | N                           |                                                              |
| riskSource         | int                | Identify where the resource violates                         | Y                           | Identify the source of risk results<br/>`1000`：No risk<br/>`1001`：Text risk<br/>`1002`：Visual picture risk |

In riskDetail, the contents of each element of the faces array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| id                 | string             | Person number                                                | N                           | People in the same position in the picture have the same number under different labels.<br/>If the same person appears n times in the picture, assign n IDs |
| name               | string             | Character name                                               | N                           | Identifiable public figure name                              |
| location           | int_array          | The array has four values, representing the coordinates of the upper left corner and the lower right corner. for example<br/>[207,522,340,567]<br/>207 Represents the x coordinate of the upper left corner<br/>522 Represents the y coordinate of the upper left corner<br/>340 Represents the x coordinate of the lower right corner<br/>567 Represents the y coordinate of the lower right corner | N                           |                                                              |
| face_ratio         | float              | Proportion of faces                                          | N                           |                                                              |
| probability        | float              | Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | N                           | Floating point number between 0 and 1                        |

In riskDetail, the contents of each element of the objects array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                          |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------- |
| id                 | string             | Number to ensure that the number of items in the same location is the same under different labels | N                           |                                                   |
| name               | string             | Identification name N                                        | N                           |                                                   |
| location           | int_array          | Identifies the location information. The array has four values, representing the coordinates of the upper left corner and the lower right corner. [207,522,340,567]<br/>207 Represents the x coordinate of the upper left corner<br/>522 Represents the y coordinate of the upper left corner<br/>340 Represents the x coordinate of the lower right corner<br/>567 Represents the y coordinate of the lower right corner | N                           |                                                   |
| probability        | float              | Degree of Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | N                           | Floating point number between 0 and 1             |
| qrContent          | string             | URL information of QR code                                   | N                           | Only returned when the QR code related tag is hit |

In riskDetail, the contents of each element of the persons array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**              |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------- |
| id                 | string             | Number, ensure that the same person has the same number under different labels. If the same person appears n times in the picture, assign n IDs | N                           |                                       |
| person_ratio       | string             | Proportion of portrait in the picture                        | N                           |                                       |
| location           | int_array          | Position coordinates of portrait                             | N                           |                                       |
| probability        | float              | Degree of Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | N                           | Floating point number between 0 and 1 |

In riskDetail, the content of ocrText is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------ |
| text               | string             | Recognized text                                              | Y                           |                          |
| matchedLists       | json\_array        | Hit customer custom list list                                | N                           |                          |
| riskSegments       | json\_array        | High-risk segment content exists when the detection picture contains risk content such as politics, terrorism, prohibition, advertising law, etc | N                           |                          |

In riskDetail, the content of ocrText is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**      | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | ------------------------------ | --------------------------- | ------------------------ |
| name               | string             | Hit list name                  | N                           |                          |
| words              | json\_array        | Hit sensitive word information | N                           |                          |

In matchedLists, the contents of each element of the words array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**   | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | --------------------------- | --------------------------- | ------------------------ |
| word               | string             | Hit sensitive words         | N                           |                          |
| position           | int_array          | Location of sensitive words | N                           |                          |

In ocrText, the details of each element of riskSegments are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**               | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | --------------------------------------- | --------------------------- | ------------------------ |
| segment            | string             | High-risk content fragments             | N                           |                          |
| position           | int_array          | Location of high-risk content fragments | N                           |                          |

<span id="auxInfo">The content of auxInfo is as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| segments           | int                | Number of pictures actually processed                        | Y                           |                                                              |
| typeVersion        | json_object        | The effect version number for each incoming type             | Y                           |                                                              |
| errorCode          | int                |                                                              | N                           | `2001`：The input data format is incorrect, and it is not legal json data<br/>`2002`：The entered parameter field is illegal (required fields are missing, wrong type, illegal value, etc.)<br/>`2003`：Picture download failed<br/>`2004`：The picture is too large, more than 10M<br/>`2005`：Illegal picture format<br/>`2006`：Invalid risk monitoring type |
| passThrough        | json_object        | The pass-through field passed in by the customer             | N                           |                                                              |
| streamId           | string             | When the [streamInfo](#streamInfo) function is passed in the request parameter, similar functions will be returned | N                           |                                                              |
| frameTime          | int                | When the [streamInfo](#streamInfo) function is passed in the request parameter, similar functions will be returned | N                           |                                                              |
| qrContent          | string             | Return the identified QR code address                        | N                           | It is required to contact Next Data Configuration to return  |

In auxInfo, the content of typeVersion is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**   | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | --------------------------- | --------------------------- | ------------------------------------------------------------ |
| POLITY             | string             | Political version number    | N                           | The composition form is`X.Y`, and`X`is the main version number, which generally represents the overall effect iteration of the model` Y ` is the sub-version number, which generally represents the routine iteration<br/>For example, `1001001.2` means the major version number is`1001001`and the sub-version number is`2` |
| VIOLENT            | string             | Terrorist version number    | N                           | The composition is the same as above                         |
| EROTIC             | string             | Pornographic version number | N                           | The composition is the same as above                         |
| ADVERT             | string             | Advertising version number  | N                           | The composition is the same as above                         |
| IMGTEXTRISK        | string             | Illegal text version number | N                           | The composition is the same as above                         |
| QRCODE             | string             | QR code version number      | N                           | The composition is the same as above                         |

In imgs, the contents of each member of the allLabels array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------ |
| riskLabel1         | string             | Level 1 risk label                                           | Y                           |                          |
| riskLabel2         | string             | Level 2 risk label                                           | Y                           |                          |
| riskLabel3         | string             | Level 3 risk label                                           | Y                           |                          |
| riskDescription    | string             | Risk causes                                                  | Y                           |                          |
| riskLevel          | string             | Disposal suggestions                                         | Y                           |                          |
| probability        | float              | Degree of Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | Y                           |                          |
| riskDetail         | json_object        | See the riskDetail under result for risk details and field contents | Y                           |                          |

The riskDetail structure of each member of allLabels is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| faces              | json_array         | Person information                                           | N                           | Return the name and location information of the political figures in the picture. The content is consistent with the format of the outer riskDetail.faces |
| face_num           | int                | Only when hitting face - face type - multiple faces, there will be multiple array elements,<br/>10 at most (if more than 10, select the 10 with the highest probability) | N                           |                                                              |
| objects            | json_array         | Identification information                                   | N                           | Returns the name and location information of the logo or item in the picture. The content is consistent with the format of the outer riskDetail.objects |
| persons            | json_array         | Only when the portrait - multiple people are hit, there will be multiple array elements, up to 10 (if<br/>more than 10, select the 10 with the highest probability). In other cases,<br/>only has one element, and the internal field refers to the persons field under the outer riskDetail | N                           |                                                              |
| person_num         | int                | There are only portraits - multiple people return            | N                           |                                                              |
| ocrText            | json_array         | Return the information about the illegal text in the picture. It exists when the request parameter type field contains' OCR ', and the internal field refers to the ocrText field under the outer riskDetail | N                           |                                                              |
| YriskSource        | int                | Identify where the resource violates                         | Y                           | Identify the source of risk results:<br/>`1000`：No risk<br/>1001 ：Picture and text risk<br/>1002 ：Visual picture risk |

The contents of each member of the businessLabels array in imgs are as follows:

| **Parameter name**  | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                  |
| ------------------- | ------------------ | ------------------------------------------------------------ | --------------------------- | --------------------------------------------------------- |
| businessLabel1      | string             | Level 1 business label                                       | Y                           |                                                           |
| businessLabel2      | string             | Level 2 business label                                       | Y                           |                                                           |
| businessLabel3      | string             | Level 3 business label                                       | Y                           |                                                           |
| businessDescription | string             | Business tag description                                     | Y                           |                                                           |
| businessDetail      | json_object        | Business tag details                                         | N                           | See the following businessDetail structure for the format |
| probability         | float              | Degree of Confidence<br/>The optional value is between 0 and 1. The higher the value, the higher the confidence | Y                           |                                                           |
| confidenceLevel     | int                | Confidence level<br/>The optional value is between 0 and 2. The higher the value, the higher the credibility | N                           |                                                           |

The contents of businessDetail in the businessLabels array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**               |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | -------------------------------------- |
| name               | string             | Star character name<br/>The star name type in the picture exists when the passed value contains`FACE` | N                           |                                        |
| probability        | float              | Star character confidence interval<br/>The optional value is between 0 and 1. The higher the value, the higher the credibility. It appears only when name exists | N                           |                                        |
| face_ratio         | float              | Proportion of faces<br/>In the range 0-1, the larger the value, the higher the proportion of faces. The type exists when the value contains`FACE` | N                           |                                        |
| faces              | json_array         | The content is consistent with the format of the outer **riskDetail.faces**, and the internal fields refer to the faces field under the outer **riskDetail** | N                           |                                        |
| objects            | json_array         | In other cases, there is only one array element identification information, which returns the name and location information of the identification or item in the picture. The content is consistent with the format of the outer **riskDetail.objects** | N                           | Array can only have one element        |
| persons            | json_array         | Only when the portrait - multiple people are hit, there will be multiple array elements, up to 10 (if more than 10, select the 10 with the highest **probability**). In other cases, only has one element, and the internal field refers to the **persons** field under the outer **riskDetail** | N                           |                                        |
| face_num           | int                | In other cases, there is only one array element to detect the number of faces detected in the image, Only when hitting face-face type-multi-face, there will be multiple array elements, up to 10 (if more than 10, select the 10 with the highest **probability**) | N                           |                                        |
| face_compare_num   | int                | Face comparison face number detection, The number of faces detected in the image exists when the **businessType** value contains`FACECOMPARE` | N                           |                                        |
| location           | int_array          | The identification location information<br/>type value contains `OBJECT` and exists from time to time. This array has four values, representing the coordinates of the upper left corner and the lower right corner. For example [207,522,340,567]<br/>207 represents the x coordinate of the upper left corner<br/>522 represents the y coordinate of the upper left corner<br/>340 represents the x coordinate of the lower right corner<br/>567 represents the y coordinate of the lower right corner | N                           |                                        |
| person_num         | int                | Human body number detection                                  | N                           | Only return when hitting multiple tags |
| person_ratio       | float              | Portrait ratio<br/>In the range of 0-1, the larger the value, the higher the face ratio | N                           |                                        |

##  Synchronous batch callback return parameters

For batch interfaces, the results are the same as those returned by synchronous batch.

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| code | int | Return code | Y | `1100`：Success<br/>`1901`：Request Limit Exceeded<br/>`1902`：Invalid Parameters<br/>`1903`：Internal Server Error<br/>`1911`：Picture Download Failed<br/>`9101`：Operation Denied |
| message | string | Return code description | Y | Corresponding to code, include: <br/>Success<br/>Request Limit Exceeded<br/>Invalid Parameters<br/>Internal Server Error<br/>Picture Download Failed<br/>Operation Denied |
| requestIds | json\_array | Request ID | Y | Multiple picture serial numbers |

The details of requestIds are as follows:

| **Parameter name** |**Parameter type**|**Parameter Description**|**Must be returned or not**|**Detailed description** |
| --- | --- | --- | --- | --- |
| requestId | string | Serial number | Y | Returned requestId |
| btId | string | Picture number | Y                           | BtId of image |


## Demo of synchronization batch:

### Demo of synchronous batch request:

```json
{
    "accessKey":"",
    "eventId":"",
    "type":"",
    "data":{
        "imgs":[
            {
               "btId":"123",
               "img":""
            },
            {
               "btId":"456",
               "img":""
            },
        ]
    },
    "acceptLang":"en"
}
```

### Demo of synchronization return:

```json
{
    "message":"Success",
    "requestId":"faf10b672ddae5e5e51ea719c44ca94b",
    "auxInfo":{

    },
    "code" :1100,
    "imgs":[
        {
            "allLabels":[

            ],
            "auxInfo":{
                "segments":1,
                "typeVersion":{
                    "OCR":"2001003.1",
                    "PORN":"3043001.1"
                }
            },
            "btId":"123",
            "code":1100,
            "message":"Success",
            "requestId":"faf10b672ddae5e5e51ea719c44ca94b_123",
            "riskDescription":"normal",
            "riskDetail":{
                "riskSource":1000
            },
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskLevel":"PASS",
            "tokenLabels":{
                "UGC_account_risk":{

                }
            }
        },
        {
            "allLabels":[

            ],
            "auxInfo":{
                "segments":1,
                "typeVersion":{
                    "OCR":"2001003.1",
                    "PORN":"3043001.1"
                }
            },
            "btId":"456",
            "code":1100,
            "message":"Success",
            "requestId":"faf10b672ddae5e5e51ea719c44ca94b_456",
            "riskDescription":"normal",
            "riskDetail":{
                "riskSource":1000
            },
            "riskLabel1":"normal",
            "riskLabel2":"",
            "riskLabel3":"",
            "riskLevel":"PASS",
            "tokenLabels":{
                "UGC_account_risk":{
                }
            }
        }
    ]
}
```


# Asynchronous batch interface

## Asynchronous batch upload request

### Request URL:

| Colony | URL | Product |
| --- | --- | --- |
| Peking | `http://api-img-bj.fengkongcloud.com/v4/saas/async/imgs` | Image |
| Shanghai | `http://api-img-sh.fengkongcloud.com/v4/saas/async/imgs` | Image |

### Request method:
```
POST
```

### Character ecoding:
```
UTF-8
```

### Recommended timeout:
5s

### Request parameters:

It is placed in the HTTP Body in Json format. The specific parameters are as follows:

| **Request parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be transferred in or not** | **Detailed description**                                     |
| :------------------------- | :----------------- | :----------------------------------------------------------- | --------------------------------- | ------------------------------------------------------------ |
| accessKey                  | string             | Interface authentication key<br/>It is used for authority authentication. | Y                                 | When opening account service, it is provided by NextData or checked at the relevant documents in the upper right corner of the background of NextData by using the opening mailbox |
| appId                      | string             | Application ID, used to distinguish different application data of the same company | Y                                 | If you need to contact us to open, please refer to the value provided by NextData separately |
| eventId                    | string             | identifier for the event                                     | Y                                 | If you need to contact us to open, please refer to the value provided by NextData separately |
| type                       | string             | Types of risks detected                                      | N                                 | **<u>Optional values of supervision level 1 label:</u>**<br/>***POLITY*** :Identify political figures, sensitive political events, etc<br/>***EROTIC*** :Identify NSFW, pornographic content, etc <br/>***VIOLENT*** :Identify violence, terrorists, drugs, guns, etc <br/>***QRCODE*** :Identify QR code<br/>***ADVERT*** :Identify spams, ads<br/>***IMGTEXTRISK*** :Identify OCR violations<br/>If more than one function needs to be identified, connect it by underline, such as **POLITY_QRCODE_ADVERT** which means to identify politics, QR code and spams at the same time<br/>*（Note: This field and the businessType field must select an incoming）*<br/>Political, pornographic and violent terrorism only include the detection of the violation of the picture itself. If it is necessary to identify the violation of the text in the picture, be sure to transfer in type IMGTEXTRISK |
| businessType               | string             | Types used for business and operations                       | N                                 | Optional values: [Please see in appendix](#Appendix)<br/> If multiple recognition functions are required, the field and type must be selected to be passed in |
| data                       | json_object        | Requested data content                                       | Y                                 | The length of the requested data field is up to 10MB,[See **data** parameter for details](#data) |
| acceptLang                 | string             | Returns the language type of interface information           | N                                 | Optional language type of returned information:<br/>zh: Chinese<br/>en: English<br/>The default is zh. If you need to return results in English, this field is required |

<span id="data">The content of "data" is as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description**                 | **Must be transferred in or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ----------------------------------------- | --------------------------------- | ------------------------------------------------------------ |
| imgs               | json_array         | Array of pictures to detect               | Y                                 |                                                              |
| tokenId            | string             | User account identification               | Y                                 | Used to distinguish user accounts. It is recommended to pass in user IDs |
| ip                 | string             | ipv4 address                              | N                                 | The public network IPv4 address of the user who sent the picture |
| deviceId           | string             | Digital device fingerprint identification | N                                 | The public network IPv4 address of the user who sent the picture |
| maxFrame           | int                | Maximum number of frames of gif image     | N                                 | The maximum number of frames of the captured git isokinetic image is 20, and the default is 3 frames. The charge is based on the actual number of frames. If the default is to capture 3 frames, the charge is based on 3 frames |
| interval           | int                | Frame truncation interval of gif picture  | N                                 | The default value is 1, which means that every frame needs to be detected. The service will automatically adjust this value to ensure full coverage of all frames |
| room               | string             | Live room number                          | N                                 |                                                              |
| extra              | json_object        | Auxiliary parameters                      | N                                 | Relevant information for auxiliary text detection            |
| role               | string             | User role                                 | N                                 | User roles must be valid in the optional range. Different policies can be configured for different roles.(Default is `USER`)<br/>The value of live broadcast field can be:<br/>`ADMIN`:administrator<br/>`HOST`:host<br/>`SYSTEM`:system role<br/>The value of game field can be:<br/>`ADMIN`:administrators<br/>`USER`:user |
| receiveTokenId     | string             | TokenId of receiver                       | N                                 | TokenId of the receiver, mandatory for private chat scenario |

The content of extra is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be transferred in or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| passThrough | json_object | Transparent parameter | N | The pass-through field is passed in by the customer, and the field is not recognized and processed by us. It is returned to the user with the result, which must be json_ Object type |

The specific contents of each element of the imgs array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be transferred in or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------------- | ------------------------------------------------------------ |
| img                | string             | The image to be detected can be downloaded from the CDN source site by using base64 encoded image data or the URL link of the image. The source site cannot be a single point<br/>Risk: If the image is not downloaded from the source site, there may be image download failure, resulting in failure to audit | Y                                 | Supported formats:<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>It is recommended that the pixel size of the picture be no less than 256 * 256. At present, the minimum resolution of the picture is 20 * 20, and the maximum size of the picture is 10MB<br/>By default, the long image is not split. If necessary, please contact Digital America to open it. The charge after splitting is subject to the actual number of frames intercepted. |
| imgCompareBase     | string             | To detect the benchmark image for comparison, if the request parameter businessType field contains the label `FACECOMPARE`, there is<br/>image data or image url link that can be encoded using base64 | N                                 | Supported formats:<br/>`jpg`，`jpeg`，`png`，`webp`，`gif`，`tiff`，`tif`，`heif`<br/>The number of batch requests should not exceed 12 at a time. It is recommended that the pixel size of the picture should not be less than 256 * 256, and the maximum size of the picture should be 10MB<br/><br/>The benchmark map temporarily does not support the format of long map and dynamic map |
| btId               | string             | Picture unique identification                                | Y                                 | The same request cannot be repeated, and the length of btId is within 30 |

## Asynchronous batch return parameters

It is placed in the HTTP Body in Json format. The specific parameters are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ------------------------- | --------------------------- | ------------------------------------------------------------ |
| code               | int                | Return code               | Y                           | `1100`：Success<br/>`1901`：Request Limit Exceeded<br/>`1902`：Invalid Parameters<br/>`1903`：Internal Server Error<br/>`1911`：Picture Download Failed<br/>`9101`：Operation Denied |
| message            | string             | Return code description   | Y                           | Corresponding to code, include: <br/>Success<br/>Request Limit Exceeded<br/>Invalid Parameters<br/>Internal Server Error<br/>Picture Download Failed<br/>Operation Denied |
| requestIds         | json\_array        | Request ID                | Y                           | Multiple picture serial numbers                              |

Each item in requestIds is:

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- | --- |
| requestId | string | Serial number | Y |Returned requestId|
| btId | string | Picture number | N                              |User btId|

### Asynchronous batch return example:
```json
{
    "code":1100,
    "message":"Success",
    "requestIds":[
        {
            "reques-tId":"d123456322adasfajfafjaskfjaf",
            "btId":"aaeevugq"
        },
        {
            "reques-tId":"d123456322adasfajfafjaskfjaf",
            "btId":"vccsewqy"
        }
    ]
}
```

#  Active query interface

## Synchronous query request

### Request URL:

| Colony | URL | Product |
| --- | --- | --- |
| Peking | `http://api-img-active-query.fengkongcloud.com/v4/image/query` | Image |

### Request method:
```
POST
```

### Character encoding:
```
UTF-8
```

### Recommended timeout:
5s

### Request Parameter

It is placed in the HTTP Body in json format. The specific parameters are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**    | **Must be transferred in or not** | **Detailed description**                                     |
| --- | --- | --- | --- | --- |
| accessKey | string | Interface authentication key | Y | Company key: used for authority authentication. When opening the account service, it is provided by Sumi or checked at the relevant documents in the upper right corner of the background of Sumi by using the opening mailbox |
| requestIds | json_array | Serial number array | Y | Query list, supporting up to 10 serial numbers |

requestIds每一项是：

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be transferred in or not** | **Detailed description**                                     |
| --- | --- | --- | --- | --- |
| requestId | string | Serial number | Y | The requestId of the query |
| btId | string | Picture number | N | If there is a btId, the single result in the batch query of requestId+btId is returned; If there is no btId, it can be divided into two situations: the first is to return the result of a single request; In the second case, fuzzy matching returns the results of batch requests |

### Query Request Example

```json
{
    "accessKey":"***",
    "requestIds":[
        {
            "requestId":"***",
            "btId":""
        }
    ]
}
```

## Query return parameters

| **Parameter name** | **Parameter type** | **Parameter Description** | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ------------------------- | --------------------------- | ------------------------------------------------------------ |
| code               | int                | Return code               | Y                           | `1100`：Success<br/>`1901`：Request Limit Exceeded<br/>`1902`：Invalid Parameters<br/>`1903`：Internal Server Error<br/>`1911`：Picture Download Failed<br/>`9101`：Operation Denied |
| message            | string             | Return code description   | Y                           | Corresponding to code, include: <br/>Success<br/>Request Limit Exceeded<br/>Invalid Parameters<br/>Internal Server Error<br/>Picture Download Failed<br/>Operation Denied |
| contents           | json\_array        | Query results             | Y                           |                                                              |

The contents are composed as follows:

| **Parameter name** | **Parameter type** | **Must be returned or not** | **Detailed description** |
| --- | --- | --- | --- |
| requestId | string | Y                           | Request unique identification |
| result | json object | N | Return results |
| btId | string | N | Image id |
| code(Indicates the status of the request corresponding to the requestId) | int | Y | `1102`:Processing<br/>`1100`:Processing completed<br/>`1910`:failed<br/>`1912`:Processing timeout (24h by default) |
| message | string | Y | Corresponds to code: <br/>Processing<br/>Processing completed<br/>failed(Display specific failure reasons according to different situations)<br/>Processing timeout (24h by default) |

result are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**   | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | --------------------------- | --------------------------- | ------------------------------------------------------------ |
| code               | int                | Return code                 | Y                           | `1100`：Success<br/>`1901`：Request Limit Exceeded<br/>`1902`：Invalid Parameters<br/>`1903`：Internal Server Error<br/>`1911`：Picture Download Failed<br/>`9101`：Operation Denied |
| message            | string             | Return code description     | Y                           | Corresponding to code, include: <br/>Success<br/>Request Limit Exceeded<br/>Invalid Parameters<br/>Internal Server Error<br/>Picture Download Failed<br/>Operation Denied |
| requestId          | string             | Request ID                  | Y                           | Request a unique identifier for troubleshooting and subsequent effect optimization. It is strongly recommended to save |
| riskLevel          | string             | Disposal suggestions        | Y                           | Possible return values:<br/>`PASS`: Normal, direct release recommended<br/>`REVIEW`: Suspicious, manual review recommended<br/>`REJECT`: Violation, direct interception is recommended |
| riskLabel1         | string             | Level 1 risk label          | Y                           | Returns `normal`when riskLevel is`PASS`                      |
| riskLabel2         | string             | Level 2 risk label          | Y                           | Null when riskLevel is`PASS`                                 |
| riskLabel3         | string             | Level 3 risk label          | Y                           | Null when riskLevel is`PASS`                                 |
| riskDescription    | string             | Risk description            | Y                           | Normal when riskLevel is`PASS`                               |
| riskDetail         | json_object        | Risk details                | Y                           | [See riskDetail parameter for details](#riskDetail)          |
| auxInfo            | json_object        | Other auxiliary information | Y                           | [See auxinfo parameter for details](#auxInfo)                |
| allLabels          | json_array         | All label details           | Y                           | Return all tags and details of hits                          |
| businessLabels     | json_array         | Business tag details        | N                           | When only identification is done, the result of not configuring the reject and review policies is returned here,[See businessLabels parameter for details](#businessLabels) |

<span id="riskDetail">The riskDetail structure is as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| faces              | json_array         | Return the name and location information of the political figures in the picture | N                           |                                                              |
| face_num           | int                | Number of faces                                              | N                           |                                                              |
| persons            | json_array         | Only when the portrait - multiple people are hit, there will be multiple array elements, up to 10 | N                           |                                                              |
| person_num         | int                | Number of portraits                                          | N                           | There are only portraits - multiple people return            |
| objects            | json_array         | Return the location information of the item or logo QR code in the picture | N                           | Array can only have one element                              |
| ocrText            | json_object        | Returns information about illegal text in the picture. It exists when the request parameter type field contains`IMGTEXTRISK`and ADVERT | N                           |                                                              |
| riskSource         | int                | Identify where the resource violates                         | Y                           | Identify the source of risk results<br/>`1000`：No risk<br/>`1001`：Text risk<br/>`1002`：Visual picture risk |

In riskDetail, the contents of each element of the faces array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| id                 | string             | Person number                                                | N                           | People in the same position in the picture have the same number under different labels.<br/>If the same person appears n times in the picture, assign n IDs |
| name               | string             | Character name                                               | N                           | Identifiable public figure name                              |
| location           | int_array          | The array has four values, representing the coordinates of the upper left corner and the lower right corner. for example<br/>[207,522,340,567]<br/>207 Represents the x coordinate of the upper left corner<br/>522 Represents the y coordinate of the upper left corner<br/>340 Represents the x coordinate of the lower right corner<br/>567 Represents the y coordinate of the lower right corner | N                           |                                                              |
| face_ratio         | float              | Proportion of faces                                          | N                           |                                                              |
| probability        | float              | Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | N                           | Floating point number between 0 and 1                        |

In riskDetail, the contents of each element of the objects array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                          |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------- |
| id                 | string             | Number to ensure that the number of items in the same location is the same under different labels | N                           |                                                   |
| name               | string             | Identification name N                                        | N                           |                                                   |
| location           | int_array          | Identifies the location information. The array has four values, representing the coordinates of the upper left corner and the lower right corner. [207,522,340,567]<br/>207 Represents the x coordinate of the upper left corner<br/>522 Represents the y coordinate of the upper left corner<br/>340 Represents the x coordinate of the lower right corner<br/>567 Represents the y coordinate of the lower right corner | N                           |                                                   |
| probability        | float              | Degree of Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | N                           | Floating point number between 0 and 1             |
| qrContent          | string             | URL information of QR code                                   | N                           | Only returned when the QR code related tag is hit |

In riskDetail, the contents of each element of the persons array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**              |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------- |
| id                 | string             | Number, ensure that the same person has the same number under different labels. If the same person appears n times in the picture, assign n IDs | N                           |                                       |
| person_ratio       | string             | Proportion of portrait in the picture                        | N                           |                                       |
| location           | int_array          | Position coordinates of portrait                             | N                           |                                       |
| probability        | float              | Degree of Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | N                           | Floating point number between 0 and 1 |

In riskDetail, the content of ocrText is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------ |
| text               | string             | Recognized text                                              | Y                           |                          |
| matchedLists       | json\_array        | Hit customer custom list list                                | N                           |                          |
| riskSegments       | json\_array        | High-risk segment content exists when the detection picture contains risk content such as politics, terrorism, prohibition, advertising law, etc | N                           |                          |

In riskDetail, the content of ocrText is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**      | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | ------------------------------ | --------------------------- | ------------------------ |
| name               | string             | Hit list name                  | N                           |                          |
| words              | json\_array        | Hit sensitive word information | N                           |                          |

In matchedLists, the contents of each element of the words array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**   | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | --------------------------- | --------------------------- | ------------------------ |
| word               | string             | Hit sensitive words         | N                           |                          |
| position           | int_array          | Location of sensitive words | N                           |                          |

In ocrText, the details of each element of riskSegments are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**               | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | --------------------------------------- | --------------------------- | ------------------------ |
| segment            | string             | High-risk content fragments             | N                           |                          |
| position           | int_array          | Location of high-risk content fragments | N                           |                          |

<span id="auxInfo">The content of auxInfo is as follows:</span>

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| segments           | int                | Number of pictures actually processed                        | Y                           |                                                              |
| typeVersion        | json_object        | The effect version number for each incoming type             | Y                           |                                                              |
| errorCode          | int                |                                                              | N                           | `2001`：The input data format is incorrect, and it is not legal json data<br/>`2002`：The entered parameter field is illegal (required fields are missing, wrong type, illegal value, etc.)<br/>`2003`：Picture download failed<br/>`2004`：The picture is too large, more than 10M<br/>`2005`：Illegal picture format<br/>`2006`：Invalid risk monitoring type |
| passThrough        | json_object        | The pass-through field passed in by the customer             | N                           |                                                              |
| streamId           | string             | When the [streamInfo](#streamInfo) function is passed in the request parameter, similar functions will be returned | N                           |                                                              |
| frameTime          | int                | When the [streamInfo](#streamInfo) function is passed in the request parameter, similar functions will be returned | N                           |                                                              |
| qrContent          | string             | Return the identified QR code address                        | N                           | It is required to contact Next Data Configuration to return  |

In auxInfo, the content of typeVersion is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**   | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | --------------------------- | --------------------------- | ------------------------------------------------------------ |
| POLITY             | string             | Political version number    | N                           | The composition form is`X.Y`, and`X`is the main version number, which generally represents the overall effect iteration of the model` Y ` is the sub-version number, which generally represents the routine iteration<br/>For example, `1001001.2` means the major version number is`1001001`and the sub-version number is`2` |
| VIOLENT            | string             | Terrorist version number    | N                           | The composition is the same as above                         |
| EROTIC             | string             | Pornographic version number | N                           | The composition is the same as above                         |
| ADVERT             | string             | Advertising version number  | N                           | The composition is the same as above                         |
| IMGTEXTRISK        | string             | Illegal text version number | N                           | The composition is the same as above                         |
| QRCODE             | string             | QR code version number      | N                           | The composition is the same as above                         |

In imgs, the contents of each member of the allLabels array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description** |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------ |
| riskLabel1         | string             | Level 1 risk label                                           | Y                           |                          |
| riskLabel2         | string             | Level 2 risk label                                           | Y                           |                          |
| riskLabel3         | string             | Level 3 risk label                                           | Y                           |                          |
| riskDescription    | string             | Risk causes                                                  | Y                           |                          |
| riskLevel          | string             | Disposal suggestions                                         | Y                           |                          |
| probability        | float              | Degree of Confidence. The optional value is between 0 and 1. The higher the value, the higher the confidence | Y                           |                          |
| riskDetail         | json_object        | See the riskDetail under result for risk details and field contents | Y                           |                          |

The riskDetail structure of each member of allLabels is as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**                                     |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | ------------------------------------------------------------ |
| faces              | json_array         | Person information                                           | N                           | Return the name and location information of the political figures in the picture. The content is consistent with the format of the outer riskDetail.faces |
| face_num           | int                | Only when hitting face - face type - multiple faces, there will be multiple array elements,<br/>10 at most (if more than 10, select the 10 with the highest probability) | N                           |                                                              |
| objects            | json_array         | Identification information                                   | N                           | Returns the name and location information of the logo or item in the picture. The content is consistent with the format of the outer riskDetail.objects |
| persons            | json_array         | Only when the portrait - multiple people are hit, there will be multiple array elements, up to 10 (if<br/>more than 10, select the 10 with the highest probability). In other cases,<br/>only has one element, and the internal field refers to the persons field under the outer riskDetail | N                           |                                                              |
| person_num         | int                | There are only portraits - multiple people return            | N                           |                                                              |
| ocrText            | json_array         | Return the information about the illegal text in the picture. It exists when the request parameter type field contains' OCR ', and the internal field refers to the ocrText field under the outer riskDetail | N                           |                                                              |
| YriskSource        | int                | Identify where the resource violates                         | Y                           | Identify the source of risk results:<br/>`1000`：No risk<br/>1001 ：Picture and text risk<br/>1002 ：Visual picture risk |

The contents of businessDetail in the businessLabels array are as follows:

| **Parameter name** | **Parameter type** | **Parameter Description**                                    | **Must be returned or not** | **Detailed description**               |
| ------------------ | ------------------ | ------------------------------------------------------------ | --------------------------- | -------------------------------------- |
| name               | string             | Star character name<br/>The star name type in the picture exists when the passed value contains`FACE` | N                           |                                        |
| probability        | float              | Star character confidence interval<br/>The optional value is between 0 and 1. The higher the value, the higher the credibility. It appears only when name exists | N                           |                                        |
| face_ratio         | float              | Proportion of faces<br/>In the range 0-1, the larger the value, the higher the proportion of faces. The type exists when the value contains`FACE` | N                           |                                        |
| faces              | json_array         | The content is consistent with the format of the outer **riskDetail.faces**, and the internal fields refer to the faces field under the outer **riskDetail** | N                           |                                        |
| objects            | json_array         | In other cases, there is only one array element identification information, which returns the name and location information of the identification or item in the picture. The content is consistent with the format of the outer **riskDetail.objects** | N                           | Array can only have one element        |
| persons            | json_array         | Only when the portrait - multiple people are hit, there will be multiple array elements, up to 10 (if more than 10, select the 10 with the highest **probability**). In other cases, only has one element, and the internal field refers to the **persons** field under the outer **riskDetail** | N                           |                                        |
| face_num           | int                | In other cases, there is only one array element to detect the number of faces detected in the image, Only when hitting face-face type-multi-face, there will be multiple array elements, up to 10 (if more than 10, select the 10 with the highest **probability**) | N                           |                                        |
| face_compare_num   | int                | Face comparison face number detection, The number of faces detected in the image exists when the **businessType** value contains`FACECOMPARE` | N                           |                                        |
| location           | int_array          | The identification location information<br/>type value contains `OBJECT` and exists from time to time. This array has four values, representing the coordinates of the upper left corner and the lower right corner. For example [207,522,340,567]<br/>207 represents the x coordinate of the upper left corner<br/>522 represents the y coordinate of the upper left corner<br/>340 represents the x coordinate of the lower right corner<br/>567 represents the y coordinate of the lower right corner | N                           |                                        |
| person_num         | int                | Human body number detection                                  | N                           | Only return when hitting multiple tags |
| person_ratio       | float              | Portrait ratio<br/>In the range of 0-1, the larger the value, the higher the face ratio | N                           |                                        |


# Demo

Currently, demos of go, java, lua, nodes, php, and python are provided. Code location:
[https://github.com/ishumei/api-demo/tree/master/v4](https://github.com/ishumei/api-demo/tree/master/v4)



# Appendix


| Business label identification type | Description |
| ------------- | --------------- |
| AGE           | Identify the age of the person |
| GENDER        | Identify the gender of the person |
| BEAUTY        | Judge whether a person is good-looking or not |
| RACE          | Identify race |
| FACEDETECTION | Such as recognition of real person, face mask, front face, side face, etc |
| FAKEFACE | Judge whether there is a disguised face in the target image |
| FACECOMPARE | Compare the face similarity between two pictures |
| PUBLICFIGURE | Identify famous Chinese stars, online celebrities, etc |
| TAINTEDSTAR | Identify China's bad artists |
| POSTURE | Such as recognizing sitting position, kneeling position, etc |
| DRESS | Such as identifying jk and hanfu |
| TEMPERAMENT | Identify a person's physical temperament |
| BODY | Such as recognizing hair, eyes, nose, etc |
| PICTUREFORM | Such as recognition of animation, expression package, etc |
| PICTURESTRUCT | Such as identify the grid diagram, bridge section diagram, etc |
| LOWVISION | Such as recognition blur, smear, mosaic, etc |
| LOWCONTNET | For example, identification points and lines are dense, insects are dense, etc |
| LIVEPICTURE | Such as identification of live broadcast in bed and driving |
| SCREENSHOT | Such as screenshots of identifying circle of friends, chat, etc |
| FITNESS |  |
| CATE |  |
| MUSIC |  |
| SPORTS |  |
| SCENERY |  |
| CITYVIEW |  |
| 3CPRODUCTSLOGO |  |
| SHOPPINGAPPSLOGO |  |
| RETOUCHAPPSLOGO |  |
| SOCIALAPPSLOGO |  |
| PHOTOMATERIALLOGO |  |
| NEWSAPPSLOGO |  |
| ENTERTAINMENTAPPSLOGO |  |
| SPORTSLOGO |  |
| APPARELLOGO |  |
| ACCESSORIESLOGO |  |
| COSMETICSLOGO |  |
| FOODLOGO |  |
| VEHICLE |  |
| BUILDING |  |
| TABLEWARE |  |
| FOOD |  |
| HOMEAPPLICATION |  |
| OFFICESUPPLIES |  |
| FASHION |  |
| SPORTEQUIPMENT |  |
| TOY |  |
| MAKEUP |  |
| DRUGS |  |
| PAINTING |  |
| ELECTRONIC |  |
| MEDICALIMAGE |  |
| FURNITURE |  |
| DAILYSUPPLIES |  |
| CONSTELLATION |  |
| KITCHENWARE |  |
| KEEPSAKE |  |
| MAMMAL |  |
| BIRDS |  |
| REPTILE |  |
| FISH |  |
| ARTHROPOD |  |
| COELENTERATE |  |
| MOLLUSKS |  |
| CRUSTACEAN |  |
| PLANT |  |
| SETTING |  |

