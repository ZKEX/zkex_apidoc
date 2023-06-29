# 基础介绍

API使用HTTPS。

针对终端用户的API域名是`defiapi.876ex.com`。

API调用方式：

- 请求方式：总是GET或POST；
- 请求路径：总是以`/v2/`开头，例如`/v2/market/trades`；

## 请求参数

- GET请求以URL参数传入，例如：[https://defiapi.876ex.com/v2/market/bars/BTC_USDT/MIN?start=1594697861590&end=1594797861590](https://defiapi.876ex.com/v2/market/bars/BTC_USDT/MIN?start=1594697861590&end=1594797861590)
- POST请求以JSON作为HTTP BODY传入，且必须设置Content-Type: application/json

## 返回参数

如果API调用成功，返回200，内容总是JSON，且具有`Content-Type: application/json`。

如果API调用失败，返回400，内容总是JSON，且具有`Content-Type: application/json`，格式为固定的Error，请参考[https://defiapi.876ex.com/v2/market/error](https://defiapi.876ex.com/v2/market/error)。

如果API超过了业务频率限制，返回429，无内容，可根据返回头`X-RateLimit-Biz-Limit`、"`X-RateLimit-Biz-Burst`和`X-RateLimit-Biz-Remaining`获取限频次数。

## 字符编码

全部使用UTF-8且只能使用UTF-8。

示例：一个完整的GET请求：

```
https://defiapi.876ex.com/v2/market/trades
```
