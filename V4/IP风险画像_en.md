# ShuMei Tianxiang Product API Documentation - IP Risk Profile
---
***All rights reserved. Unauthorized reproduction is prohibited***
---

* [IP Risk Profile](#riskDiscernInterface)
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

# <span id = "riskDiscernInterface">Risk IP Profile</span>

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
| accessKey      | string      | Interface authentication key<br/>Used for permission authentication, provided by ShuMei when the account service is activated or can be viewed in the relevant document section in the upper right corner of the ShuMei backend when logged in with the activation email | Required     | Assigned by ShuMei                                        |
| data           | json_object | Request data content | Required     | Request data content, maximum 10MB, [see data parameters](#data) |
| passThrough    | json_object | Pass-through parameter, returned as-is | Optional     | Pass-through parameter, custom parameters passed in by the interface caller will be returned as-is, used to associate custom context information between requests and responses. This field can be omitted or return an empty JSON object when not passed in |

<span id = "data">Among them, the content of data is as follows:</span>


| **Request Parameter Name** | **Type** | **Parameter Description**   | **Required** | **Specification**                   |
| ---------------- | ---------- | ---------------- | -------------- | ---------------------------- |
| ip | string | Client's public IPv4 and IPv6 address when the current business event occurs | Required | Non-intranet IP |
| type | string | Label type | Optional | IP label<br/>Optional values:<br/>RISKIP: Risk IP<br/>BPROXY: Proxy IP<br/>DEFAULT: Default returns all labels<br/>Supports multiple primary risk labels separated by underscores_<br/>If the type parameter is not passed, all labels are returned by default |


## <span id = "response">Response</span>

Placed in the HTTP Body, in JSON format, with specific parameters as follows:

| **Parameter Name**     | **Parameter Type** | **Parameter Description**   | **Required** | **Specification**    |
| ------------------ | -------------- | ---------------- | -------------- | --------------
| code             | int          | Return code         | Yes           | `1100`: Success<br/>`1901`: QPS limit exceeded<br/>`1902`: Invalid parameter<br/>`1903`: Service failure<br/>`9101`: Unauthorized operation<br/>  |
| message          | string       | Return code description     | Yes           | Corresponds to code: `Success QPS limit exceeded Invalid parameter Service failure Insufficient balance Unauthorized operation`|
| requestId        | string       | Request identifier       | Yes           | Unique request identifier, used for troubleshooting and subsequent effect optimization, strongly recommended to save|
| ipLabels         | json_object  | IP labels | No           | See details below, returned only when the IP field is passed. |
| passThrough      | json_object  | Pass-through parameter, returned as-is | No           | Pass-through parameter, custom parameters passed in by the interface caller will be returned as-is, used to associate custom context information between requests and responses. This field can be omitted or return an empty JSON object when not passed in |

The details of ipLabels are as follows:

| **Response Parameter Name** | **Parameter Type** | **Required** | **Specification** |
| ---------------------- | ---------------- | ---------------- | ------------ |
| b_cgn        | json_object         | No          | Carrier reserved IP    |
| b_cmwap      | json_object         | No          | Mobile DreamNet IP     |
| risk_ip      | json_object            | No          | Whether it is a risk IP, a risk IP is an IP detected as risky through ShuMei's risk control strategy and model |
| b_secdial    | json_object            | No          | Whether it is a second dial IP, a second dial IP is an IP obtained by frequently dialing up the broadband |
| b_idc        | json_object            | No          | Whether it is a data center IP, a data center IP is an IP used by the data center server |
| b_proxy      | json_object            | No          | Whether it is a proxy IP, a proxy IP is an IP that acts as an intermediary to obtain network information for network users |
| ip_continent | json_object         | No          | IP continent       |
| ip_country   | json_object         | No          | IP country     |
| ip_province  | json_object         | No          | IP province     |
| ip_city      | json_object         | No          | IP city     |
| ip_latitude  | json_object        | No          | IP latitude        |
| ip_longitude | json_object        | No          | IP longitude        |
| ip_owner     | json_object         | No          | IP carrier   |

Among them, risk_ip


| ***Response Parameter Name*** | ***Parameter Type*** | ***Parameter Description*** | ***Specification***                |
| ---------------------- | ---------------- | ---------------- | --------------------------- |
| risk_ip              | int            | Business risk IP       | `0`: Non-business risk IP `1`: Business risk IP |
| risk_ip_last_ts      | int            | Business risk IP hit time |                           |

Among them, b_secdial


| ***Response Parameter Name*** | ***Parameter Type*** | ***Parameter Description*** | ***Specification***                |
| -------------------- | -------------- | -------------- | ------------------------- |
| b_secdial            | int            | Second dial IP         | `0`: Non-second dial IP `1`: Second dial IP |
| b_secdial_last_ts    | int            | Second dial IP verification time |                           |

Among them, b_idc


| ***Response Parameter Name*** | ***Parameter Type*** | ***Parameter Description*** | ***Specification***                |
| -------------------- | -------------- | -------------- | ------------------------- |
| b_idc                | int            | Data center IP         | `0`: Non-data center IP `1`: Data center IP |
| b_idc_last_ts        | int            | Data center IP verification time |                           |

Among them, b_proxy


| ***Response Parameter Name*** | ***Parameter Type*** | ***Parameter Description*** | ***Specification***                |
| -------------------- | -------------- | -------------- | ------------------------- |
| b_proxy              | int            | Proxy IP         | `0`: Non-proxy IP `1`: Proxy IP |
| b_proxy_last_ts      | int            | Proxy IP verification time |                           |



Among them, ip_continent


| ***Response Parameter Name*** | ***Parameter Type*** | ***Parameter Description*** | ***Specification*** |
| ---------------------- | ---------------- | ---------------- | ------------ |
| ip_continent         | string         | Continent         |            |

Among them, ip_country


| ***Response Parameter Name*** | ***Parameter Type*** | ***Parameter Description*** | ***Specification*** |
| ---------------------- | ---------------- | ---------------- | ------------ |
| ip_country           | string         | Country       |            |

Among them, ip_province


| ***Response Parameter Name*** | ***Parameter Type*** | ***Parameter Description*** | ***Specification*** |
| ---------------------- | ---------------- | ---------------- | ------------ |
| ip_province          | string         | Province       |            |

Among them, ip_city


| ***Response Parameter Name*** | ***Parameter Type*** | ***Parameter Description*** | ***Specification*** |
| ---------------------- | ---------------- | ---------------- | ------------ |
| ip_city              | string         | City       |            |

Among them, ip_owner


| ***Response Parameter Name*** | ***Parameter Type*** | ***Parameter Description*** | ***Specification*** |
| ---------------------- | ---------------- | ---------------- | ------------ |
| ip_owner             | string         | Carrier     |            |

Among them, ip_longitude


| ***Response Parameter Name*** | ***Parameter Type*** | ***Parameter Description*** | ***Specification*** |
| ---------------------- | ---------------- | ---------------- | ------------ |
| ip_longitude         | float          | Longitude   |            |

Among them, ip_latitude


| ***Response Parameter Name*** | ***Parameter Type*** | ***Parameter Description*** | ***Specification*** |
| ---------------------- | ---------------- | ---------------- | ------------ |
| ip_latitude          | float          | Latitude   |            |

Among them, b_cmwap


| ***Response Parameter Name*** | ***Parameter Type*** | ***Parameter Description*** | ***Specification***                      |
| ---------------------- | ---------------- | ---------------- | --------------------------------- |
| b_cmwap              | int            | Mobile DreamNet       | `0`: Non-Mobile DreamNet `1`: Mobile DreamNet |

Among them, b_cgn


| ***Response Parameter Name*** | ***Parameter Type*** | ***Parameter Description*** | ***Specification***                                 |
| ---------------------- | ---------------- | ---------------- | -------------------------------------------- |
| b_cgn                | int            | Carrier reserved NAT  | `0`: Non-carrier reserved NAT  `1`: Carrier reserved NAT |


## <span id = "example">Example:</span>

### <span id = "requestExample">Request Example:</span>

```json
{
  "accessKey": "xxxxxxxxxxxxxxxxxxxxxxxxx",
  "data": {
    "ip": "116.237.65.34"
  },
  "passThrough": {}
}
```

### <span id = "syncResponseExample">Synchronous Response Example:</span>

```json
{
    "code": 1100,
    "detail": null,
    "devicePrimaryInfo": {},
    "deviceRiskLabels": [],
    "ipLabels": {
        "b_cgn": {
            "b_cgn": 0
        },
        "ip_city": {
            "ip_city": "Shenzhen"
        },
        "ip_country": {
            "ip_country": "China"
        },
        "ip_latitude": {
            "ip_latitude": 22.5427
        },
        "ip_longitude": {
            "ip_longitude": 114.01449
        },
        "ip_owner": {
            "ip_owner": "China Telecom"
        },
        "ip_province": {
            "ip_province": "Guangdong"
        },
        "risk_ip": {
            "risk_ip": 0
        }
    },
    "message": "Success",
    "profileExist": 1,
    "requestId": "23e753672a69050d3dfb6db5d3259413",
    "tokenProfileLabels": [],
    "tokenRiskLabels": [],
    "passThrough": {}
}
```