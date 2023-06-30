# 现货交易

现货交易API可用于现货交易，包括下单、撤单、查看当前活动订单以及查询历史订单。

## 查询账户余额

API描述：查询用户现货账户余额。

API路径：GET /v2/spots/accounts

API请求参数：

- 无

```
API响应样例:
```

```json
{
    "BTC": [
        493.81081575, // 现货可用
        5.9716 // 现货冻结
    ],
    "ETH": [
        10.9967000,
        0
    ],
    "USDT": [
        9937490.3587324070000000,
        0E-16
    ]
}
```

## 查询用户费率

API描述：查询用户现货交易费率。

API路径：GET /v2/spots/fee/rate

API请求参数(Query Param)：

| 参数       | 类型     | 说明                                  |
| :--------- | -------- | :------------------------------------ |
| **symbol* | **string** | **必填**<br>交易对名称,例如`BTC_USDT` |




```
API响应样例:
```

```json
{
    "taker": 0.000300000000000000, // taker费率
    "maker": 0.000100000000000000, // maker费率
    "timestamp": 1595297714823 // 时间戳
}
```

## 下单

API描述：创建一个新订单。

API路径：POST /v2/spots/orders

API请求参数(Request Json Body)：

| 参数          | 类型        | 说明                                                   |
| :------------ | ----------- | ------------------------------------------------------ |
| **symbol**    | **string**  | **必填**<br>交易对名称,例如`BTC_USDT`                  |
| **type**      | **enum**    | **必填**<br/>订单类型：限价单="LIMIT"，市价单="MARKET" |
| **direction** | **enum**    | **必填**<br/>订单方向：买入="LONG"，卖出="SHORT"       |
| **price**     | **decimal** | **仅限限价单**<br/>订单价格，例如`7123.5`              |
| **quantity**  | **decimal** | **必填**<br/>订单数量，例如`1.02`                      |
| **fillOrKill**  | **boolean** | **非必填**<br/>是否FOK订单，例如`true`                      |
| **immediateOrCancel**  | **boolean** | **非必填**<br/>是否IOC订单，例如`true`                      |
| **postOnly**  | **boolean** | **必填**<br/>是否被动委托订单，例如`true`                      |
| **hidden**  | **boolean** | **必填**<br/>是否冰山委托订单，例如`true`，冰山委托订单手续费率使用taker费率      |
| **clientOrderId** | **string** | **选填**<br/>用户自定义OrderId，可用于订单查询、撤单。自定义OrderId 24小时内可用。 |



```
API响应样例：订单信息
```

```json
{
    "id": 12345, // 订单ID
  	"clientOrderId": "clientOrderId", // 用户自定义OrderId
    "sequenceId": 23456, // 序列ID
    "type": "LIMIT", // 订单类型
    "direction": "LONG", // 订单方向
    "price": 7123.5, // 订单限价
    "quantity": 1.02, // 订单数量
    "unfilledQuantity": 1.0, // 订单尚未成交数量
    "fillPrice": 7102.2, // 订单成交均价，仅作为展示使用
    "status": "PARTICIAL_FILLED", // 订单状态
    "makerFeeRate": -0.001, // 作为Maker的费率
    "takerFeeRate": 0.002, // 作为Taker的费率
    "chargeQuote": true, // 是否总收取Quote作为手续费
    "marginTrade": false, // 是否是杠杆交易(目前未开启)
    "fee": 7.1022, // 累积收取的手续费总额
    "createdAt": 1564558783608, // 创建时间
    "updatedAt": 1564558783608, // 最后更新时间
    "baseCurrencyId": 100, // 交易币种ID
    "quoteCurrencyId": 103, // 计价币种ID
	  "chargeQuote": true, // 是否只用计价货币收取手续费
	  "features": 0, // 订单特性码
    "userAndAccount": { // 用户与子账户信息
        "userId": 10004,
        "accountId": 0
    }
}
```

API错误响应：

- `PARAMETER_INVALID`: 参数错误
- `ACCOUNT_NO_ENOUGH_AVAILABLE`: 没有足够的可用余额
- `ORDER_EXCEEDED`: 活动订单数量超过了限制
- `PRICE_EXCEEDED`: 价格超出限制：买入价过高或卖出价过低

## 撤单

API描述：撤销一个订单。

API路径：POST /v2/spots/orders/:order_id/cancel

API请求参数(Path Param)：

| 参数        | 类型     | 说明               |
| :---------- | -------- | :----------------- |
| **order_id** | **path** | **必填**<br>订单ID |

```
API响应样例：同上
```

API错误响应：

- `ORDER_NOT_FOUND`: 订单不存在（ID无效或已完全成交或已被取消）

说明：

如果撤单成功，返回的订单状态是`FULLY_CANCELLED`（全部取消）或`PARTICIAL_CANCELLED`（部分取消）。

如果撤单失败，返回错误响应。



## 撤单（通过自定义订单ID）

API描述：通过clientOrderId撤销一个订单。

API路径：POST /v2/spots/orders/client/:client_order_id/cancel

API请求参数(Path Param)：

| 参数              | 类型     | 说明                         |
| :---------------- | -------- | :--------------------------- |
| **clientOrderId** | **path** | **必填**<br>用户自定义订单ID |

```
API响应样例：同上
```

API错误响应：

- `ORDER_NOT_FOUND`: 订单不存在（ID无效或已完全成交或已被取消）

说明：

如果撤单成功，返回的订单状态是`FULLY_CANCELLED`（全部取消）或`PARTICIAL_CANCELLED`（部分取消）。

如果撤单失败，返回错误响应。



## 撤销所有订单

API描述：撤销所有订单。

API路径：POST /v2/spots/orders/cancel

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
    "results": {
        "cancelled" : 10 // 成功取消的订单数量
    }
}
```

说明：

该接口是服务端代为执行的批处理操作，并非所有订单同时尝试取消。所以不能保证所有订单取消的时间有效性。

## 查询订单详情

API描述：查询某个指定订单的详情。

API路径：GET /v2/spots/orders/:order_id

API请求参数(Path Param)：

| 参数        | 类型     | 说明               |
| :---------- | -------- | :----------------- |
| **order_id** | **path** | **必填**<br>订单ID |



```
API响应样例：订单详细信息
```

```json
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
```

OrderStatus：订单状态说明

- STOP_PENDING：正在等待触发的Stop单；
- PENDING：正在等待成交的活动单；
- FAILED：订单执行失败（无足够保证金等原因），最终状态；
- STOP_FAILED：Stop订单触发后执行失败（无足够保证金等原因），最终状态；
- FULLY_FILLED：全部成交，最终状态；
- PARTIAL_FILLED：已部分成交；
- PARTIAL_CANCELLED：部分成交后被用户取消，最终状态；
- STOP_CANCELLED：Stop订单尚未触发就被用户取消，最终状态；
- FULLY_CANCELLED：订单尚未成交就被用户取消，最终状态；

说明：目前未启用止盈止损（Stop Order）订单，所以STOP_*状态不会出现在系统中。



## 查询订单详情（通过自定义订单ID）

API描述：通过自定义订单ID查询订单的详情。

API路径：GET /v2/spots/orders/client/:client_order_id

API请求参数(Path Param)：

| 参数                | 类型     | 说明                         |
| :------------------ | -------- | :--------------------------- |
| **client_order_id** | **path** | **必填**<br>用户自定义订单ID |



```
API响应样例：订单详细信息
```

```json
{
    "id": 3527072007,
  	"clientOrderId": "clientOrderId", // 用户自定义OrderId
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
```

OrderStatus：订单状态说明

- STOP_PENDING：正在等待触发的Stop单；
- PENDING：正在等待成交的活动单；
- FAILED：订单执行失败（无足够保证金等原因），最终状态；
- STOP_FAILED：Stop订单触发后执行失败（无足够保证金等原因），最终状态；
- FULLY_FILLED：全部成交，最终状态；
- PARTIAL_FILLED：已部分成交；
- PARTIAL_CANCELLED：部分成交后被用户取消，最终状态；
- STOP_CANCELLED：Stop订单尚未触发就被用户取消，最终状态；
- FULLY_CANCELLED：订单尚未成交就被用户取消，最终状态；

说明：目前未启用止盈止损（Stop Order）订单，所以STOP_*状态不会出现在系统中。



## 查询订单成交详情

API描述：查询当前用户的某个订单成交详情。

API路径：GET /v2/spots/orders/:order_id/matches

API请求参数(Path Param)：


| 参数   | 类型     | 说明                            |
| :----- | -------- | :------------------------------ |
| **order_id** | **path** | **必填**<br>URL地址参数，订单ID |



```
API响应样例：订单成交详细信息，按时间排序
```

```json
{
    "results": [
        {
            "taker": true, // 是否是taker
            "price": 9801.5, // 成交价
            "quantity": 120, // 成交数量
            "fee": 0.0012845, // 手续费
            "createdAt": 1564558783608 // 创建时间
        },
        {
            "taker": false, // 是否是taker
            "price": 9801.0, // 成交价
            "quantity": 26, // 成交数量
            "fee": -0.0006175, // 手续费，负数表示反佣
            "createdAt": 1564558790832 // 创建时间
        },
        ...
    ]
}
```

如果一个订单的成交记录超过1000条，则最多返回前1000条记录。

## 查询清算结果

API描述：查询当前用户的某个订单成交详情。

API路径：GET /v2/spots/match/clearings

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
    "range": "202007",
    "hasMore": true,
    "nextOffsetId": 1039651,
    "results": [
        {
            "id": 1039652,
            "orderId": 4059332007,
            "symbol": "BTC_USDT",
            "sequenceId": 405933,
            "direction": "LONG",
            "type": "TAKER",
            "matchPrice": 9182.900000000000000000,
            "matchQuantity": 0.010200000000000000,
            "orderStatusAfterClearing": "FULLY_FILLED",
            "orderUnfilledQuantityAfterClearing": 0E-18,
            "feeCurrency": "BTC",
            "fee": 0.000002040000000000,
            "rate": 0.000200000000000000,
            "createdAt": 1595300043398
        }
    ]
}
```

## 查询所有活跃订单

API描述：查询当前用户活跃订单。

API路径：GET /v2/spots/orders/open

API请求参数：

- 无

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

API路径：GET /v2/spots/orders/open/:symbol

API请求参数(Path Param)：


| 参数   | 类型     | 说明                            |
| :----- | -------- | :------------------------------ |
| **symbol**   | **string** | **必填**<br>交易对名称，例如`BTC_USDT`，默认空               |


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

API路径：GET /v2/spots/orders/closed

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



## 币币账户划转到资产中心

API描述：从币币账户划转资产到资产中心

API路径：POST /v2/spots/transfer/out/request

API请求参数：JSON Object

| 参数             | 类型        | 说明                               |
| :--------------- | ----------- | :--------------------------------- |
| **currency**     | **string**  | **必填**<br/>currency名称，如“BTC” |
| **amount**       | **decimal** | **必填**<br/>划转数量              |
| **transferFrom** | **string**  | **必填**<br/>"SPOTS"               |
| **transferTo**   | **string**  | **必填**<br/>"WALLET"              |

返回：

```json
{
    "transferFrom": "SPOTS",
    "transferTo": "WALLET",
    "userId": 122017,
    "amount": 0.001,
    "transferId": "CT0001b881ea800042",
    "currencyName": "BTC",
    "currencyId": 100
}
```

## 币币账户之间划转

API描述：币币账户之间的划转, 默认账户为0，普通用户开设1，2两个子账户，做市商用户开设1～9 共9个子账户， 子账户使用方式为在原有的接口上增加 Account Header，填写使用的账户，默认为0

API路径：POST /v2/spots/transfer/account/request

API请求参数：JSON Object

| 参数             | 类型        | 说明                               |
| :--------------- | ----------- | :--------------------------------- |
| **currency**     | **string**  | **必填**<br/>currency名称，如“BTC” |
| **amount**       | **decimal** | **必填**<br/>划转数量              |
| **transferFrom** | **string**  | **必填**<br/>"ACCOUNT"               |
| **transferTo**   | **string**  | **必填**<br/>"ACCOUNT"              |
| **fromAccount** | **string**  | **必填**<br/>0～2               |
| **toAccount**   | **string**  | **必填**<br/>0～2              |

请求样例：

```json
{
    "currency":"BTC",
    "transferFrom":"ACCOUNT",
    "transferTo":"ACCOUNT",
    "fromAccount":0,
    "toAccount":1,
    "amount":0.001
}

```


返回：

```json
{
    "result": true
}
```
