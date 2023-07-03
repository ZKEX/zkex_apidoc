# 现货交易

现货交易API可用于现货交易，包括下单、撤单、查看当前活动订单以及查询历史订单。

## 查询账户余额

API描述：查询用户现货账户余额。

API路径：GET /v1/trading/:accountId/balances

API请求参数(PATH)：

| 参数        | 类型       | 说明               |
| :---------- |----------| :----------------- |
| *accountId* | **path** | **必填**<br>账户id |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "BTC": [
      "0", // 可用 
      "0" // 现货冻结
    ],
    "USDT": [
      "0",
      "0"
    ]
  }
}
```

## 查询用户费率

API描述：查询用户现货交易费率。

API路径：GET /v1/trading/spots/fee/rate

API请求参数(Query Param)：

| 参数       | 类型     | 说明                           |
| :--------- | -------- |:-----------------------------|
| **symbol** | **string** | **必填**<br>交易对名称,例如`BTC_USDT` |




```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "taker": 0.002,
    "maker": 0.001,
    "timestamp": 1688386621807
  }
}
```

## 获取下单nonce值信息
API描述：创建订单时入参需要携带用户订单nonce,该接口是实时获取订单nonce信息接口

API路径：GET /v1/orderNonce/gen/:slotType

API请求参数(PATH):

| 参数        | 类型    | 说明               |
| :---------- | ------- | :----------------- |
| *slotType* | **int** | **必填**<br> 现货 0 合约 1 |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "userId": 10030,
    "nonce": 0,    // 下单时需要
    "slotId": 28773,  // 下单时需要
    "available": true // 是否可以使用
  }
}
```


## 下单

API描述：创建一个新订单。

API路径：POST /v1/trading/:accountId/spots/orders

API请求参数(PATH)：

| 参数        | 类型    | 说明               |
| :---------- | ------- | :----------------- |
| *accountId* | **int** | **必填**<br>账户id |

API请求参数(Request Json Body)：

| 参数          | 类型          | 说明                                                                  |
| :------------ |-------------|---------------------------------------------------------------------|
| **symbol**    | **string**  | **必填**<br>交易对名称,例如`BTC_USDT`                                        |
| **type**      | **enum**    | **必填**<br/>订单类型：限价单="LIMIT"，市价单="MARKET"                            |
| **direction** | **enum**    | **必填**<br/>订单方向：买入="LONG"，卖出="SHORT"                                |
| **price**     | **decimal** | **仅限限价单**<br/>订单价格，例如`7123.5`                                       |
| **quantity**  | **decimal** | **必填**<br/>订单数量，例如`1.02`                                            |
| **fillOrKill**  | **boolean** | **非必填**<br/>是否FOK订单，例如`true`                                        |
| **immediateOrCancel**  | **boolean** | **非必填**<br/>是否IOC订单，例如`true`                                        |
| **postOnly**  | **boolean** | **必填**<br/>是否被动委托订单，例如`true`                                        |
| **hidden**  | **boolean** | **必填**<br/>是否冰山委托订单，例如`true`，冰山委托订单手续费率使用taker费率                    |
| **clientOrderId** | **string**  | **选填**<br/>用户自定义OrderId，可用于订单查询、撤单。自定义OrderId activity Order 可以查询使用。 |
| **slotId**| **long**    | **必填**<br/>订单slotId，通过接口/v1/orderNonce/gen获取 例如`1`                  |
| **nonce**| **long**    | **必填**<br/>订单NonceId，通过接口/v1/orderNonce/gen获取 例如`0`                 |
| **signature**| **String**  | **必填**<br/>请求参数签名串，通过2层私钥签名          |



```
API响应样例：订单信息
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "orderId": 133001824436288
  }
}
```
异步返回订单Id，订单交易结果回通过ws通知，也可以主动去查询交易结果

## 撤单

API描述：撤销一个订单。

API路径：POST /v1/trading/:accountId/spots/orders/:orderId/cancel

API请求参数(Path Param)：

| 参数        | 类型     | 说明               |
| :---------- | -------- | :----------------- |
| **order_id** | **path** | **必填**<br>订单ID |
| **accountId** | **path** | **必填**<br/>账户ID |

```
API响应样例：
{
    "code": 200,
    "msg": "success",
    "data": {
        "orderId": 133016311562368
    }
}
```
```
API错误响应：
{
"code": 1003,
"msg": "Active spots order not found.",
"data": null
}
```

## 撤销所有订单

API描述：撤销所有订单。

API路径：POST /v1/trading/:accountId/spots/orders/cancel

API请求参数(PATH)：

| 参数        | 类型    | 说明               |
| :---------- | ------- | :----------------- |
| *accountId* | **int** | **必填**<br>账户id |

API请求参数(Request Json Body)：

```json
{
    "symbol":"BTC_USDT"
}
```

| 参数       | 类型       | 说明                                                         |
| :--------- | ---------- | :----------------------------------------------------------- |
| **symbol** | **string** | **选填**<br>交易对名称,例如`BTC_USDT`，为空则撤销所有交易对活动订单 |

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "result": "ok"
  }
}
```
说明：
该接口是服务端代为执行的批处理操作，并非所有订单同时尝试取消。所以不能保证所有订单取消的时间有效性。

## 查询订单详情

API描述：查询某个指定订单的详情。

API路径：GET /v1/trading/:accountId/spots/orders/:orderId

API请求参数(Path Param)：

| 参数        | 类型     | 说明               |
| :---------- | -------- | :----------------- |
| **accountId** | **path** | **必填**<br/>账户ID |
| **order_id** | **path** | **必填**<br>订单ID |



```
API响应样例：订单详细信息
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": 128598358884418,
    "userId": 10030,
    "accountId": 0,
    "clientOrderId": null,
    "symbolId": 101103,
    "sequenceId": 539693,
    "type": "LIMIT",
    "status": "FULLY_FILLED",
    "direction": "LONG",
    "features": 0,
    "price": 1148.000000000000000000,
    "fee": 0.000175000000000000,
    "fillPrice": 1148.0,
    "makerFeeRate": 0.001000000000000000,
    "takerFeeRate": 0.002000000000000000,
    "createdAt": 1687861319658,
    "updatedAt": 1687861319658,
    "chargeQuote": false,
    "quantity": 0.010000000000000000,
    "unfilledQuantity": 0E-18,
    "symbol": "ETH_USDT"
  }
}
```

OrderStatus：订单状态说明

- PENDING：正在等待成交的活动单；
- FAILED：订单执行失败（无足够保证金等原因），最终状态；
- FULLY_FILLED：全部成交，最终状态；
- PARTIAL_FILLED：已部分成交；
- PARTIAL_CANCELLED：部分成交后被用户取消，最终状态；
- FULLY_CANCELLED：订单尚未成交就被用户取消，最终状态；



## 查询订单详情（通过自定义订单ID，仅支持活跃订单）

API描述：通过自定义订单ID查询订单的详情。

API路径：GET /v1/trading/:accountId/spots/clientOrders/open/:client_order_id

API请求参数(Path Param)：

| 参数                | 类型     | 说明                         |
| :------------------ | -------- | :--------------------------- |
| **accountId**       | **path** | **必填**<br/>账户ID          |
| **client_order_id** | **path** | **必填**<br>用户自定义订单ID |



```
API响应样例：订单详细信息
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": 128598358884418,
    "userId": 10030,
    "accountId": 0,
    "clientOrderId": null,
    "symbolId": 101103,
    "sequenceId": 539693,
    "type": "LIMIT",
    "status": "PENDING",
    "direction": "LONG",
    "features": 0,
    "price": 1148.000000000000000000,
    "fee": 0.000175000000000000,
    "fillPrice": 1148.0,
    "makerFeeRate": 0.001000000000000000,
    "takerFeeRate": 0.002000000000000000,
    "createdAt": 1687861319658,
    "updatedAt": 1687861319658,
    "chargeQuote": false,
    "quantity": 0.010000000000000000,
    "unfilledQuantity": 0E-18,
    "symbol": "ETH_USDT"
  }
}
```



## 查询订单成交详情

API描述：查询当前用户的某个订单成交详情。

API路径：GET /v1/trading/:accountId/spots/orders/:order_id/matches

API请求参数(Path Param)：


| 参数   | 类型     | 说明                            |
| :----- | -------- | :------------------------------ |
| **accountId** | **path** | **必填**<br/>账户ID |
| **order_id** | **path** | **必填**<br>URL地址参数，订单ID |



```
API响应样例：订单成交详细信息，按时间排序
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      {
        "taker": true,
        "price": 40000.000000000000000000,
        "quantity": 0.500000000000000000,
        "fee": 40.000000000000000000,
        "createdAt": 1688386253562
      }
    ]
  }
}
```

如果一个订单的成交记录超过1000条，则最多返回前1000条记录。

## 查询清算结果

API描述：查询当前用户的某个订单成交详情。

API路径：GET /v1/trading/:accountId/spots/match/clearings

API请求参数(Path Param)：


| 参数          | 类型     | 说明                |
| :------------ | -------- | :------------------ |
| **accountId** | **path** | **必填**<br/>账户ID |

API请求参数(Request Param)：

| 参数         | 类型       | 说明                                                         |
| :----------- | ---------- | :----------------------------------------------------------- |
| **symbol**   | **string** | **选填**<br>交易对名称，例如`BTC_USDT`，默认空               |
| **limit**    | **long**   | **选填**<br/>返回结果集的最大记录数量，范围1～100，默认为100 |
| **range**    | **string** | **选填**<br/>查询月份，格式为YYYYMM，例如"201907"，默认为""，表示当前月份 |
| **offsetId** | **long**   | **选填**<br/>传入当前页的起始id，默认为0，表示第一页         |



```
API响应样例：订单成交详细信息，按时间排序
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "range": "202307",
    "hasMore": false,
    "nextOffsetId": 0,
    "results": [
      {
        "id": 10012,
        "orderId": 133001413394496,
        "userId": 10030,
        "accountId": 0,
        "sequenceId": 597555,
        "direction": "LONG",
        "type": "MAKER",
        "matchPrice": 40000.000000000000000000,
        "matchQuantity": 0.500000000000000000,
        "orderStatusAfterClearing": "FULLY_FILLED",
        "orderUnfilledQuantityAfterClearing": 0E-18,
        "feeCurrency": "BTC",
        "fee": 0.000500000000000000,
        "rate": 0.001000000000000000,
        "createdAt": 1688386253562,
        "txHash": "0x42027101d6739a9b95d94701f6886620500fa853a27cd4d7d147a7bdfba46722",
        "symbol": "BTC_USDT"
      }
    ]
  }
}
```

## 查询所有活跃订单（todo）

API描述：查询当前用户活跃订单。

API路径：GET /v1/trading/:accountId/spots/orders/open

API请求参数(Path Param)：


| 参数          | 类型     | 说明                |
| :------------ | -------- | :------------------ |
| **accountId** | **path** | **必填**<br/>账户ID |

```
API响应样例：订单详细信息，按时间排序
```

```json
{
    "results": [
        {
            "id": 3400192007,
            "features": 0,
            "price": 9133.2,
            "fee": 0.0000196200000000000000,
            "fillPrice": 9133.199999999999,
            "marginTrade": false,
            "chargeQuote": false,
            "quantity": 0.7944,
            "unfilledQuantity": 0.5982,
            "makerFeeRate": 0.000100000000000000,
            "takerFeeRate": 0.000200000000000000,
            "type": "LIMIT",
            "status": "PARTIAL_FILLED",
            "direction": "LONG",
            "triggerDirection": "LONG",
            "triggerOn": 0,
            "trailingBasePrice": 0,
            "trailingDistance": 0,
            "createdAt": 1595242057585,
            "updatedAt": 1595299047936,
            "symbol": "BTC_USDT",
            "trailing": false
        },
        {
            "id": 3527072007,
            "features": 0,
            "price": 9132.5,
            "fee": 0,
            "fillPrice": 0.0,
            "marginTrade": false,
            "chargeQuote": false,
            "quantity": 0.1803,
            "unfilledQuantity": 0.1803,
            "makerFeeRate": 0.000100000000000000,
            "takerFeeRate": 0.000200000000000000,
            "type": "LIMIT",
            "status": "PENDING",
            "direction": "LONG",
            "triggerDirection": "LONG",
            "triggerOn": 0,
            "trailingBasePrice": 0,
            "trailingDistance": 0,
            "createdAt": 1595253140322,
            "updatedAt": 1595253140322,
            "symbol": "BTC_USDT",
            "trailing": false
        }
    ]
}
```

## 查询交易对活跃订单

API描述：查询当前用户活跃订单。

API路径：GET /v1/trading/:accountId/spots/orders/open

API请求参数(Path Param)：


| 参数          | 类型     | 说明                |
| :------------ | -------- | :------------------ |
| **accountId** | **path** | **必填**<br/>账户ID |


```
API响应样例：订单详细信息，按时间排序
```

```json
{
    "results": [
        {
            "id": 3400192007,
            "features": 0,
            "price": 9133.2,
            "fee": 0.0000196200000000000000,
            "fillPrice": 9133.199999999999,
            "marginTrade": false,
            "chargeQuote": false,
            "quantity": 0.7944,
            "unfilledQuantity": 0.5982,
            "makerFeeRate": 0.000100000000000000,
            "takerFeeRate": 0.000200000000000000,
            "type": "LIMIT",
            "status": "PARTIAL_FILLED",
            "direction": "LONG",
            "triggerDirection": "LONG",
            "triggerOn": 0,
            "trailingBasePrice": 0,
            "trailingDistance": 0,
            "createdAt": 1595242057585,
            "updatedAt": 1595299047936,
            "symbol": "BTC_USDT",
            "trailing": false
        },
        {
            "id": 3527072007,
            "features": 0,
            "price": 9132.5,
            "fee": 0,
            "fillPrice": 0.0,
            "marginTrade": false,
            "chargeQuote": false,
            "quantity": 0.1803,
            "unfilledQuantity": 0.1803,
            "makerFeeRate": 0.000100000000000000,
            "takerFeeRate": 0.000200000000000000,
            "type": "LIMIT",
            "status": "PENDING",
            "direction": "LONG",
            "triggerDirection": "LONG",
            "triggerOn": 0,
            "trailingBasePrice": 0,
            "trailingDistance": 0,
            "createdAt": 1595253140322,
            "updatedAt": 1595253140322,
            "symbol": "BTC_USDT",
            "trailing": false
        }
    ]
}
```

## 查询历史订单

API描述：查询当前用户历史订单。

API路径：GET /v1/trading/:accountId/spots/orders/closed

API请求参数(Path Param)：


| 参数          | 类型     | 说明                |
| :------------ | -------- | :------------------ |
| **accountId** | **path** | **必填**<br/>账户ID |

API请求参数(Request Param)：

| 参数         | 类型       | 说明                                                         |
| :----------- | ---------- | :----------------------------------------------------------- |
| **symbol**   | **string** | **选填**<br>交易对名称，例如`BTC_USDT`，默认空               |
| **limit**    | **long**   | **选填**<br/>返回结果集的最大记录数量，范围1～100，默认为100 |
| **range**    | **string** | **选填**<br/>查询月份，格式为YYYYMM，例如"201907"，默认为""，表示当前月份 |
| **offsetId** | **long**   | **选填**<br/>传入当前页的起始id，默认为0，表示第一页         |

```
API响应样例：订单详细信息，按时间排序
```

```json
{
    "range": "202007",
    "hasMore": true,
    "nextOffsetId": 4045822007,
    "results": [
        {
            "id": 4045852007,
            "features": 8,
            "price": 10091.500000000000000000,
            "fee": 0.000002300000000000,
            "fillPrice": 9181.6,
            "marginTrade": false,
            "chargeQuote": false,
            "quantity": 0.011500000000000000,
            "unfilledQuantity": 0E-18,
            "makerFeeRate": 0.000100000000000000,
            "takerFeeRate": 0.000200000000000000,
            "type": "MARKET",
            "status": "FULLY_FILLED",
            "direction": "LONG",
            "triggerDirection": "LONG",
            "triggerOn": 0E-18,
            "trailingBasePrice": 0E-18,
            "trailingDistance": 0E-18,
            "createdAt": 1595298848680,
            "updatedAt": 1595298848680,
            "symbol": "BTC_USDT",
            "trailing": false
        },
        {
            "id": 4045842007,
            "features": 8,
            "price": 8263.500000000000000000,
            "fee": 0.021100430000000000,
            "fillPrice": 9174.1,
            "marginTrade": false,
            "chargeQuote": false,
            "quantity": 0.011500000000000000,
            "unfilledQuantity": 0E-18,
            "makerFeeRate": 0.000100000000000000,
            "takerFeeRate": 0.000200000000000000,
            "type": "MARKET",
            "status": "FULLY_FILLED",
            "direction": "SHORT",
            "triggerDirection": "LONG",
            "triggerOn": 0E-18,
            "trailingBasePrice": 0E-18,
            "trailingDistance": 0E-18,
            "createdAt": 1595298848635,
            "updatedAt": 1595298848635,
            "symbol": "BTC_USDT",
            "trailing": false
        }
    ]
}
```
