# Main Interface Encryption - Guidance Plan

This plan is intended to guide customers on how to encrypt data.



## I. Document Description

Provide an encryption demo in Go language and offer explanations.



### Background Information

Encryption scheme background: The main interface supports the transmission of encrypted data to ensure the security and reliability of data during transmission. To ensure that the data can be decrypted normally, customers need to ensure that their encryption method is consistent with the encryption method provided by the Shumei side.



## II. Guidance Plan

### Main Interface Encryption Interface Description

- Currently, most interfaces of Tianjing and Tianwang already support data decryption. (Refer to the official website for specific products)

- The currently supported encryption/decryption methods include the following:
  - `SM4` encryption method



### Encryption Interface Usage Instructions

1. Add the **`encryptType`** field to the outermost layer of the original request parameters. The type is a string, and the optional value is `sm4`.
2. Serialize the data field (originally of type json_object) into a JSON string and encrypt it.
   - The encryption method is: **SM4/CBC/PKCS7Padding**
   - By default, **IV** is 16 bytes, all zeros.
3. Base64 encode the encrypted bytes above and use it as the data parameter. At this point, data is of string type.

### Example of Request Before Encryption

```json
{
    "accessKey":"",
    "appId":"default",
    "eventId":"login",
    "type":"POLITY",
    "contentType":"RAW",
    "data":{
        "tokenId":"",
        "formatInfo":"",
        "rate":"",
        "track":"",
        "returnAllText":"",
        "audioDetectStep":"",
        "lang":""
    },
    "btId":"",
    "callback":""
}
```



### Example of Request After Encryption

```json
{
    "accessKey":"",
    "appId":"default",
    "eventId":"login",
    "type":"POLITY",
    "contentType":"RAW",
    "encryptType":"sm4",  // This parameter must be passed when using the encryption scheme
    "data":"xxxxx",      // Encrypted data
    "btId":"",
    "callback":""
}
```



## III. Demo

Required information:

- Plaintext: JSON string of the data field
- Key: secretKey (the **first 16 bytes** of the key provided by Shumei, please contact Shumei colleagues to obtain it)

Key encryption process:

`base64`( `sm4_cbc_encrypt`( `secretKey[:16]`, `data_string`) )

First perform SM4 encryption, then base64 encode the byte array, and use the resulting string as the data parameter for the request.

```go
package main

import (
	"encoding/base64"
	"fmt"
	"github.com/tjfoc/gmsm/sm4"
)

func main() {
	plainData := `{"text":"待检测的文本", "tokenId": "userId-1"}`
	secretKey := `secretKey1234567890secretKey`

	encrypted := encrypt(secretKey[:16], plainData)
	fmt.Printf("encrypted: %v\n", encrypted)
	// MrxSrGA2dEo3MDOwYEXQjChHZu5WwVEm7sf9mDBOlU3SLFEQxqk/oVAC0TBu6/BnATtbhMj4UiamfDL1+0zjdg==

	decrypted := decrypt(secretKey[:16], encrypted)
	fmt.Printf("decrypted: %v\n", decrypted)
	if plainData == decrypted {
		fmt.Println("decrypt success")
	} else {
		fmt.Println("decrypt failed!!!")
	}
}

func encrypt(secretKey, data string) string {
	out, err := sm4.Sm4Cbc([]byte(secretKey), []byte(data), true)
	if err != nil {
		panic(err)
	}
	return base64.StdEncoding.EncodeToString(out)
}

func decrypt(secretKey, encryptedData string) string {
	b, err := base64.StdEncoding.DecodeString(encryptedData)
	if err != nil {
		panic(err)
	}
	encryptedData = string(b)
	out, err := sm4.Sm4Cbc([]byte(secretKey), []byte(encryptedData), false)
	if err != nil {
		panic(err)
	}
	return string(out)
}
```