# Introduction

The API uses HTTPS.

API domain for end-users:

- test: https://testnet.app.zkex.com
- prd: https://app.zkex.com

API Invocation Method:

- Request Method: Always GET or POST.
- Request Path: Always starts with`/v1/`, for example `/v1/market/trades`.

## Request Parameters

- GET requests pass parameters in the URL, for example: [https://testnet.app.zkex.com/v1/market/bars/WBTC_USDC/SEC?start=0&end=1](https://testnet.app.zkex.com/v1/market/bars/WBTC_USDC/SEC?start=0&end=1).
- POST requests pass JSON as the HTTP BODY
- Content-Type must be set: application/json

## Response Parameters

If the API call is successful, it returns 200, the content is always JSON, and has`Content-Type: application/json`.

If the API call fails, it returns a non-200 status code, the content is always JSON, and has`Content-Type: application/json`.

The format is a fixed Error, please refer to [https://testnet.app.zkex.com/v1/market/error](https://testnet.app.zkex.com/v1/market/error).

If the API exceeds the business frequency limit, it returns 429 with no content. The frequency limit can be obtained based on the returned headers:

- `X-RateLimit-Biz-Limit`
- `X-RateLimit-Biz-Burst`
- `X-RateLimit-Biz-Remaining`


## Character Encoding

All requests and responses use UTF-8 exclusively.

Example: A complete GET request:
[https://testnet.app.zkex.com/v1/market/trades](https://testnet.app.zkex.com/v1/market/trades).

