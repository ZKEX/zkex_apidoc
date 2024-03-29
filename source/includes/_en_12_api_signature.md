# Signature

## API-Signature

API requests require a signature to confirm the user's identity and prevent replay attacks.

Signature Algorithm：

For an HTTP API request, the following information is needed:

* Method:  `GET`或`POST`
* Domain: For example, `uniapi.876ex.com`
* Path: URI starting with ‘/’, for example `/v1/trade/orders`
* Parameters: Parameters in the form of key1=value2&key2=value2, for example: `id=123456&sort=DESC&from=2017-09-10`
* Headers: For example, `Accept: */*`
* Body: Binary representation of a JSON string, applicable only for POST requests

Construct a string according to the following format:

```http
GET\n
uniapi.876ex.com\n
/v1/trade/orders\n
from=2017-09-10&id=123456&sort=DESC\n
API-KEY: xyz123456\n
API-SIGNATURE-METHOD: HmacSHA256\n
API-SIGNATURE-VERSION: 1\n
API-TIMESTAMP: 12300000000\n
API-UNIQUE-ID: uni-123-abc-xyz\n
<json body data>
```

Each line ends with a newline character `\n`. If the request body exists, do not add a newline character at the end.

The first line is the method name, in all caps, either `GET` or `POST`.

The second line is the domain, in all lowercase, for example `uniapi.876ex.com`.

The third line is the path, starting with `/`, strictly case-sensitive, and without a trailing `?`.

The fourth line is the parameters, sorted alphabetically, joined by `&`: `from=2017-09-10`，`id=123456`，`sort=DESC`. If there are no request parameters, the fourth line should be blank, and note that the blank line should also end with `\n`.

Next, sort the headers starting with `API-` in alphabetical order, format each header in the format `HEADER: Value`, where the header name is in uppercase, followed by a colon and a space, and the value is case-sensitive. Each header should be on its own line, ending with `\n`.

Finally, if it is a `POST` request and contains a body, add the body in JSON string format to the end, without `\n`. If it is a `GET` request or a `POST` request without a body, add an empty string `''`.

The above string is encoded in UTF-8 to obtain a binary byte array, then use the API Secret to calculate the Signature: ：</p>

```python
signature = HmacSHA256(payload.encode("UTF-8"), "my-api-secret")
```

Add the obtained signature to the header in lowercase hexadecimal string format:

```http
API-Signature: a1b2c3ff001234500900dd01ff
```

### Notes

Parameters should be used in their original string format, e.g., `a=1/5`, `a=1%2F5`.

Headers are uppercase when calculating the signature, but case-insensitive when sending.

Only headers starting with API- are included in the signature calculation (except for API-Signature, Because the API-Signature can only be computed at the end and then appended to the request).

API-Signature-Method must be `HmacSHA256`.

API-Signature-Version must be `1`.

API-Timestamp is the current timestamp in milliseconds, without decimals, with an error margin of not more than 1 minute.

API-Unique-ID is optional. If provided, the client needs to provide a unique string identifier, refer to [API Unique Id](unique-id).

For requests with a body, it's necessary to first serialize it into a JSON string and then include the body in the signature calculation. Avoid serializing an object twice because some language implementations may result in different JSON strings for two serializations of the same object. For example, `{"a":true, "b":1}` and `{"b":1, "a":true}` have identical JSON content but different strings, which would cause signature validation to fail.

### SDK

We provide the following SDKs encapsulating sample signature algorithms to assist developers in quickly calling the API:

* Java SDK [github](https://github.com/876ex-pub/signature-demo/blob/master/ApiClient.java)
* Python SDK [github](https://github.com/876ex-pub/signature-demo/blob/master/876ex.py)
* JavaScript/Node SDK [github](https://github.com/876ex-pub/signature-demo/blob/master/876ex.js)


### ApiKey & ApiSecret

API Key & API Secret retrieval interface is temporarily unavailable. Please contact us if needed.


## Zk-Signature

During transactions, users need to sign transaction data using ZK-Transaction.

* ou can refer to the official documentation of zkLink for more information: [zkLink X ](https://docs.zk.link/developer/sdk/changelog/signer)
* If needed, please feel free to contact us.
