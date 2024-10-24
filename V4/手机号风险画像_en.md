# Shumei Tianxiang Product API Documentation - Risk Mobile Number Profiling

---

***All rights reserved. Reproduction prohibited***

---

* [Risk Mobile Number Profiling](#riskDiscernInterface)
  + [Invocation Timing](#requestParameter)
  + [Specific Interface](#requestInterface)
    - [Request URL](#requestUrl)
    - [Character Encoding Format](#requestEncode)
    - [Request Method](#requestMethod)
    - [Suggested Timeout Duration](#requestTimeout)
    - [Request Parameters](#requestParameters)
  + [Response](#response)
  + [Example](#example)
    - [Request Example](#requestExample)
    - [Response Example](#responseExample)

# <span id = "riskDiscernInterface">Risk Mobile Number Profiling</span>

## <span id = "requestParameter">Invocation Timing</span>

This interface is called when risk identification is needed, for example:

1. After a new user registers, perform a risk profile query for differentiated distribution of newcomer red packets, registration gifts, etc.;
2. After an existing user logs in, perform a risk profile query for differentiated distribution in daily activity operations;
3. Before issuing activity rewards, perform a risk profile query for probabilistic activities such as random red packets, lotteries, etc.;
4. Daily routine queries to update the profile database or risk control engine data.

## <span id = "requestInterface">Specific Interface</span>

### <span id = "requestUrl">Request URL:</span>

| Cluster         | URL                                                        | Supported Product List |
| -------------- | ------------------------------------------------------------ | -------------- |
| Beijing Cluster     | `http://api-tianxiang-bj.fengkongcloud.com/tianxiang/v4`   | Tianxiang Risk Identification |
| Singapore Cluster   | `http://api-tianxiang-xjp.fengkongcloud.com/tianxiang/v4`  | Tianxiang Risk Identification |
| Frankfurt Cluster | `http://api-tianxiang-eur.fengkongcloud.com/tianxiang/v4`  | Tianxiang Risk Identification |
| Virginia Cluster | `http://api-tianxiang-fjny.fengkongcloud.com/tianxiang/v4` | Tianxiang Risk Identification |

### <span id = "requestMethod">Request Method:</span>

`POST`

### <span id = "requestEncode">Character Encoding:</span>

`UTF-8`

### <span id = "requestTimeout">Suggested Timeout Duration:</span>

1s

### <span id = "requestParameters">Request Parameters:</span>

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Request Parameter Name** | **Type**    | **Parameter Description**                                                                                         | **Required** | **Specification**                                        |
| ---------------- | ------------- | ------------------------------------------------------------------------------------------------------ | -------------- | ------------------------------------------------- |
| accessKey      | string      | Interface authentication key<br/>Used for permission authentication, provided by Shumei when opening account services or viewed in the relevant documentation section in the upper right corner of the Shumei backend when logging in with the opening email | Required     | Assigned by Shumei                                        |
| data           | json_object | Request data content | Required     | Request data content, maximum 10MB, [see data parameters](#data) |

<span id = "data">Among them, the content of data is as follows:</span>

| **Request Parameter Name** | **Type** | **Parameter Description**   | **Required** | **Specification**                   |
| ---------------- | ---------- | ---------------- | -------------- | ---------------------------- |
| phoneMd5       | string   | MD5 encrypted mobile number to be queried | phoneMd5, phoneSm3, phoneSha256 cannot all be empty |  This information is the MD5 encrypted (32-bit) mobile number to be checked by the customer |
| phoneSm3  | string | SM3 encrypted mobile number to be queried | phoneMd5, phoneSm3, phoneSha256 cannot all be empty | This information is the SM3 encrypted mobile number to be checked by the customer |
| phoneSha256 | string | Sha256 encrypted mobile number to be queried | phoneMd5, phoneSm3, phoneSha256 cannot all be empty | This information is the Sha256 encrypted mobile number to be checked by the customer |
| newCountryCode | string      | Country Code                                                                       | Recommended   | A four digit string. If the parameter was not passed in, the default value is China country code 0086                                                                                                                                                                                                       |
| type | string | Label type | Optional | Mobile number label<br/>Optional values:<br/>BLACKRECORDPHONE: Black industry mobile number<br/>SMSPLATFORMPHONE: SMS platform mobile number<br/>IOTSIMCARDPHONE: IoT SIM card mobile number<br/>MVNOSIMCARDPHONE: MVNO mobile number<br/>RISKPHONE: Risk mobile number<br/>RELATERISKDEVICEPHONE: Mobile number associated with risk device<br/>RELATERISKTOKENPHONE: Mobile number associated with risk account<br/>TWOPHONE: Secondary mobile number<br/>DEFAULT: Default returns all labels except "TWOPHONE"<br/>Supports multiple primary risk labels separated by underscores_<br/>If the type parameter is not passed, all labels except "TWOPHONE" are returned by default |
| registerTimestamp | int64 | Mobile number check time | Optional, required when type includes "TWOPHONE" | Accurate to the hour (format YYYYMMDDHH24)<br/>For example: 2017121200 |

## <span id = "response">Response</span>

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name**     | **Parameter Type** | **Parameter Description**   | **Required** | **Specification**                                                                                                                                                                                                    |
| ------------------ | -------------- | ---------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| code             | int          | Return code         | Yes           | `1100`: Success<br/>`1901`: QPS limit exceeded<br/>`1902`: Invalid parameter<br/>`1903`: Service failure<br/>`1911`: Image download failure<br/>`9101`: No permission to operate<br/>`3000`: Operator error<br/>Except for message and requestId, fields only exist when code is 1100 |
| message          | string       | Return code description     | Yes           | Corresponds to code: `Success QPS limit exceeded Invalid parameter Service failure Insufficient balance No permission to operate`                                                                                                                                          |
| requestId        | string       | Request identifier       | Yes           | Unique request identifier, used for troubleshooting and subsequent effect optimization, strongly recommended to save                                                                                                                                                      |
| phonePrimaryInfo | Json_object  | Basic mobile number information | No           | See details below, returned only when phoneMd5 or phoneSm3 is passed (no need to open)                                                                                                                                                           |
| phoneRiskLabels  | json_array   | Mobile number risk labels | No           | See details below, returned only when phoneMd5 or phoneSm3 is passed (no need to open)                                                                                                                                                          |

Among them
**1**）Details of phonePrimaryInfo

| ***Response Parameter Name*** | ***Parameter Type*** | ***Required*** | ***Specification***                                                                          |
|-------------------------------|----------------------|----------------|----------------------------------------------------------------------------------------------|
| phone_province                | string               | No             | Province of attribution                                                                      |
| phone_city                    | string               | No             | City of attribution                                                                          |
| phone_operator                | string               | No             | Operator                                                                                     |
| intl_phone	                   | string	              | No	            | Overseas phone number plaintext, only returned when "newCountryCode" is not "0086" passed in |
| intl_phone_country            | 	list	               | No	            | Enter the country code corresponding to the list of all countries                            |

**2**）Details of phoneRiskLabels

| ***Response Parameter Name*** | ***Parameter Type*** | ***Parameter Description***         | ***Specification***                     |
| ------------------ | ---------------- | ------------------------ | -------------------------------- |
| label1               | string         | Primary label               | Displays the primary label of the mobile number risk label. |
| label2               | string         | Secondary label               | Displays the secondary label of the mobile number risk label. |
| label3               | string         | Tertiary label               | Displays the tertiary label of the mobile number risk label. |
| description          | string         | Risk description               | Displays the Chinese description of the mobile number risk label. |
| timestamp            | int64          | Time of the most recent hit strategy | Time of the most recent hit strategy         |
| statusCode | int64 | Query status returned by China Mobile | Returned only under the secondary mobile number label, [see statusCode parameter](#statusCode) |
| detail               | json_object    | Evidence description               | Evidence details                       |

**3**）Explanation of tertiary labels:

| English Name (Primary Label)      | Chinese Name (Primary Label)   | English Name (Secondary Label)            | Chinese Name (Secondary Label)   | English Name (Tertiary Label)               | Chinese Name (Tertiary Label)                   |
| ------------------------- | -------------------- | -------------------------------- | ------------------------ | ---------------------------------- | ------------------------------ |
| relate_riskdevice_phone | Mobile number associated with risk device | fake_device_phone              | Mobile number associated with fake device     | b_pc_emulator_phone              | PC emulator device mobile number           |
| relate_riskdevice_phone | Mobile number associated with risk device | fake_device_phone              | Mobile number associated with fake device     | b_cloud_device_phone             | Cloud phone device mobile number             |
| relate_riskdevice_phone | Mobile number associated with risk device | fake_device_phone              | Mobile number associated with fake device     | b_phone_emulator_phone           | Mobile emulator device mobile number         |
| relate_riskdevice_phone | Mobile number associated with risk device | fake_device_phone              | Mobile number associated with fake device     | b_altered_phone                  | Altered device mobile number               |
| relate_riskdevice_phone | Mobile number associated with risk device | fake_device_phone              | Mobile number associated with fake device     | b_multi_boxing_by_os_phone       | System multi-open device mobile number           |
| relate_riskdevice_phone | Mobile number associated with risk device | fake_device_phone              | Mobile number associated with fake device     | b_multi_boxing_by_app_phone      | Tool multi-open device mobile number           |
| relate_riskdevice_phone | Mobile number associated with risk device | fake_device_phone              | Mobile number associated with fake device     | b_faker_phone                    | Fake device mobile number               |
| relate_riskdevice_phone | Mobile number associated with risk device | fake_device_phone              | Mobile number associated with fake device     | b_farmer_phone                   | Farm device mobile number               |
| relate_riskdevice_phone | Mobile number associated with risk device | fake_device_phone              | Mobile number associated with fake device     | b_offerwall_phone                | Offerwall device mobile number             |
| relate_riskdevice_phone | Mobile number associated with risk device | fake_device_phone              | Mobile number associated with fake device     | b_mismatch_phone                 | Illegal parameter device mobile number         |
| relate_riskdevice_phone | Mobile number associated with risk device | monkey_device_phone            | Mobile number associated with machine-controlled device | b_monkey_apps_phone              | Device mobile number with machine-controlled apps    |
| relate_riskdevice_phone | Mobile number associated with risk device | monkey_device_phone            | Mobile number associated with machine-controlled device | b_webdriver_phone                | Device mobile number with machine-controlled browser plugins |
| relate_riskdevice_phone | Mobile number associated with risk device | monkey_device_phone            | Mobile number associated with machine-controlled device | b_monkey_game_apps_phone         | Device mobile number with game-controlled apps    |
| relate_riskdevice_phone | Mobile number associated with risk device | monkey_device_phone            | Mobile number associated with machine-controlled device | b_monkey_read_apps_phone         | Device mobile number with reading apps    |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_root_phone                     | ROOT device mobile number               |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_sim_phone                      | Device mobile number without SIM card          |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_alter_apps_phone               | Device mobile number with altered apps            |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_debuggable_phone               | Debug mode device mobile number           |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_vpn_phone                      | VPN device mobile number                |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_hook_phone                     | Device mobile number with HOOK installed           |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_manufacture_phone              | Engineering mode mobile number               |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_wx_code_phone                  | WeChat SMS platform device mobile number       |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_sms_code_phone                 | SMS platform device mobile number       |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_low_osver_phone                | Low OS version device mobile number     |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_remote_control_apps_phone      | Device mobile number with remote control tools       |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_repackage_phone                | Repackaged device mobile number             |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_console_phone                  | Device mobile number with console enabled         |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_reset_phone                    | Suspected reset device mobile number           |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_alter_loc_phone                | Device mobile number with altered location       |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_game_cheat_apps_phone          | Device mobile number with game cheat tools   |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_headless_phone                 | Headless browser device mobile number         |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_old_model_phone                 | Old device mobile number             |
| relate_riskdevice_phone | Mobile number associated with risk device | suspicious_device_phone        | Mobile number associated with suspicious device     | b_wangzhuan_active_phone          | 	Online earning device mobile number           |
| relate_risktoken_phone  | Mobile number associated with risk account | risk_register_token_phone      | Mobile number associated with risk registration account | monkey_register_token_phone      | Machine registration mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | risk_login_token_phone         | Mobile number associated with risk login account | unusual_device_login_token_phone | Abnormal device login mobile number           |
| relate_risktoken_phone  | Mobile number associated with risk account | risk_login_token_phone         | Mobile number associated with risk login account | unusual_region_login_token_phone | Abnormal region login mobile number           |
| relate_risktoken_phone  | Mobile number associated with risk account | risk_login_token_phone         | Mobile number associated with risk login account | monkey_login_token_phone         | Machine login mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | marketing_cheating_token_phone | Mobile number associated with marketing cheating account | fission_cheat_token_phone        | Fission cheating mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | marketing_cheating_token_phone | Mobile number associated with marketing cheating account | rank_cheat_token_phone           | Ranking cheating mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | marketing_cheating_token_phone | Mobile number associated with marketing cheating account | task_cheat_token_phone           | Task cheating mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | marketing_cheating_token_phone | Mobile number associated with marketing cheating account | seckill_cheat_token_phone        | Machine seckill cheating mobile number           |
| relate_risktoken_phone  | Mobile number associated with risk account | marketing_cheating_token_phone | Mobile number associated with marketing cheating account | risk_discount_token_phone        | Discount fraud mobile number                 |
| relate_risktoken_phone  | Mobile number associated with risk account | marketing_cheating_token_phone | Mobile number associated with marketing cheating account | consume_stock_token_phone        | Stock/seat occupancy mobile number            |
| relate_risktoken_phone  | Mobile number associated with risk account | data_stealing_token_phone      | Mobile number associated with data stealing account | data_steal_token_phone           | Data stealing mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | ad_scam_token_phone            | Mobile number associated with scam diversion account | net_scam_token_phone             | Internet scam mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | ad_scam_token_phone            | Mobile number associated with scam diversion account | user_digging_token_phone         | Competitor poaching mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | ad_scam_token_phone            | Mobile number associated with scam diversion account | ad_diversion_token_phone         | Ad diversion mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | risk_group_token_phone         | Mobile number associated with high-risk group account | risk_group_token_phone           | High-risk group mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | risk_content_token_phone       | Mobile number associated with risk content account | porn_token_phone                 | Pornographic content mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | risk_content_token_phone       | Mobile number associated with risk content account | politics_token_phone             | Political content mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | other_risk_token_phone         | Mobile number associated with other suspicious account | monkey_token_phone               | Machine-controlled mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | other_risk_token_phone         | Mobile number associated with other suspicious account | high_frequency_token_phone       | Abnormal high-frequency mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | other_risk_token_phone         | Mobile number associated with other suspicious account | abnormal_active_token_phone      | Abnormally active mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | other_risk_token_phone         | Mobile number associated with other suspicious account | discrete_region_token_phone      | Discrete region mobile number               |
| relate_risktoken_phone  | Mobile number associated with risk account | other_risk_token_phone         | Mobile number associated with other suspicious account