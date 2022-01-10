

# eHSM REST API Reference

Currently, the eHSM-KMS-Service now provides the following restful APIs to the customers :


- **Cryptographic Functionalities APIs**<br>
  Notes: These below Rest APIs are used to provide crypto functionalities for users.
  - [CreateKey](#CreateKey)
  - [Encrypt](#Encrypt)
  - [Decrypt](#Decrypt)
  - [AsymmetricEncrypt](#AsymmetricEncrypt)
  - [AsymmetricDecrypt](#AsymmetricDecrypt)
  - [Sign](#Sign)
  - [Verify](#Verify)
  - [GenerateDataKey](#GenerateDataKey)
  - [GenerateDataKeyWithoutPlaintext](#GenerateDataKeyWithoutPlaintext)
  - [ExportDataKey](#ExportDataKey)

## Common Prameters
This section describes the parameters that are common to all API requests and responses.
 | Name | Type | Reference Value | Description |
 |:-----------|:-----------|:-----------|:-----------|
 | appid | string | 12345678-0123-4567-*** | An unique id to request ehsm in a domain, which is requested<br> from ehsm service maintainer |
 | nonce | string  | 2866307081280200*** | 16 bytes random number, 15 minute validity; <br>Taking the current time as the benchmark, detect nonce within 15 minutes, which cannot be repeated  |
 | timestamp | string  | 20211215*** | The timestamp of sending request; format: yyyymmddhhmmssmmm |
 |sign|string   |iw6mkXDqNipxweCH****| The signature string of the current request.<br><br>**Notes:** Before to request the ehsm-kms cryptographic APIs, the cutomer should to request the unique appid and APIKey from the ehsm kms service maintainer, and make sure they are securely stored.<br>The API key will participate in the signature, but does not participate in the parameter transfer.<br><br>*Signature= base64(HMAC-SHA256(APIKey, RequestData))*, <br>where, *RequestData=[appid=\<appid\>&nonce=\<nonce\>&timestamp=\<timestamp\>&payload=\<payload\>]* |
 |payload | Object | payload ={<br>"keyspec":"EH_RSA_3072",<br> "origin": "EH_INTERNAL_KEY"<br>} | The specific parameters of each method call|
   

## Createkey
Create a customer master key(CMK) for the user, which can be a symmetric or an asymmetric key, for the symmetric cmk mainly used to wrap the [datakey](#Generatedatakey), also can be used to encrypted an arbitrary set of bytes data(<6KB). And for the asymmetric cmk mainly used to sign/verify or asymmetric encrypt/decrypt datas(not for the datakey.)

- **Rest API format:**

   POST <ehsm_srv_address>/ehsm?Action=CreateKey


- **Request Payload:**

   | Name | Type | Reference Value | Description |
   |:-----------|:-----------|:-----------|:-----------|
   | KeySpec | String   |  EH_AES_GCM_128 |The keyspec the user want to create, it can be the following one: <br>EH_AES_GCM_128 <br>EH_AES_GCM_256<br>EH_RSA_3072<br>EH_RSA_4096<br>EH_EC_P256<br>EH_EC_P512<br>EH_EC_SM2<br>EH_SM4<br><br>**Notes:**  currently on support the keyspec(EH_AES_GCM_128 and EH_RSA_3072), for others will support later.|
   | KeyOrigin| String | EH_INTERNAL_KEY | The source about the cmk comes from, it can be:<br> EH_INTERNAL_KEY (generated from the eHSM inside)<br>EXTERNAL_KEY (generated by the customer and want to import into the eHSM)<br><br>**Notes:** currently it only support the type of EH_INTERNAL_KEY. |

  Notes: for the common request parameters, please refer to the [common params](#Common-Prameters)
   
- **Response Data:**

  | Name | Type | Reference Value | Description |
  |:-----------|:-----------|:-----------|:-----------|
  | code | int | 200 | The result of the method call, 200 is success, others are fail|
  | message | String | "success" | The description of result |
  | cmk| String |cmk_base64":<br> ""AAAAAAAAAAAAAAAAAAAAAGCcKdP/fwAA***"}<br>  }  |The result in json object for the cmk which in based64 encoding |

- **Example**
	- Request sample in python
   ```python
    params = OrderedDict()
    params["appid"] = appid
    params["nonce"] = str(int.from_bytes(os.urandom(16), "big"))
    params["timestamp"] = int(time.time())
    params["sign"] = sign
    params["payload"] = {
      "keyspec":"EH_RSA_3072",
      "origin":"EH_INTERNAL_KEY"
    }
    requests.post(url="<ehsm_srv_address>/ehsm?Action=CreateKey", data=json.dumps(params), headers=headers)
   ```

	- Response data
	 ```python
	Response= {
    "code":200,
    "message":"success!",
    "result": {
        "cmk_base64":"AAAAAAAAAAAAAAAAAAAAAGCcKdP/fwA***"
     }
     }
    ```
   *(return to the [Cryptographic Functionalities APIs](#eHSM-REST-API-Reference).)*
---
## Encrypt

## Decrypt

## AsymmetricEncrypt

## AsymmetricDecrypt

## Sign

## Verify

## Generatedatakey

## Generatedatakeywithoutplaintext

## ExportDataKey