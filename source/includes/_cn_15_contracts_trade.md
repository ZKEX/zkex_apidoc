

# 合约交易

合约交易API可用于合约交易，包括仓位、下单、撤单、查看当前活动订单以及查询历史订单。

交易API必须附带用户的API签名，以便验证用户身份。



## 查询账户权益

API描述：查询用户账户权益，包含仓位、未实现盈亏等。

API路径：GET /v1/trading/:accountId/assets

API请求参数(PATH)：

| 参数        | 类型    | 说明               |
| :---------- | ------- | :----------------- |
| *accountId* | **int** | **必填**<br>账户id |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "totalTransferableMargin": 46000.000,
    "totalSpotsAssets": 50000.000,
    "totalAvailableMargin": 46000.000,
    "minimumMaintenanceMargin": 0,
    "contractsUnrealized": 0,
    "contractsPrices": {
      "XBTC": 40000.133333000000000000,
      "XBCH": 123.000410000000000000,
      "XETH": 546.001820000000000000
    },
    "priceInfos": {
      "BTC": {
        "price": 40000.00,
        "conversionRatio": 1
      },
      "BCH": {
        "price": 123.00,
        "conversionRatio": 0.8
      },
      "ETH": {
        "price": 546.00,
        "conversionRatio": 0.9
      },
      "USDT": {
        "price": 1,
        "conversionRatio": 1
      }
    },
    "totalMargin": 46000.000,
    "contractsOrders": [],
    "contractsPositions": {},
    "balances": {
      "USDT": [
        46000.000,
        4000.000
      ]
    },
    "totalUsedMargin": 0,
    "totalFrozenMargin": 0,
    "totalMarginRate": 0,
    "totalNetValue": 50000.000
  }
}
```

## 查询用户费率 （合约）

API描述：查询用户合约交易费率(期货、合约)。

API路径：GET /v1/trading/contracts/fee/rate

API请求参数(Query Param)：

| 参数       | 类型       | 说明                                                 |
| :--------- | ---------- | :--------------------------------------------------- |
| **symbol** | **string** | **选填**<br>交易对名称,例如`XBTC`|


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
    "timestamp": 1688471068462
  }
}
```



## 创建新订单

API描述：创建一个新的订单。

API路径：POST /v1/trading/:accountId/contracts/orders

API请求参数(PATH)：

| 参数        | 类型    | 说明               |
| :---------- | ------- | :----------------- |
| *accountId* | **int** | **必填**<br>账户id |

API请求参数(Request Json Body):

| 字段名称                  | 字段类型       | 是否必须 | 默认值   | 描述                                       |
|:----------------------|------------|------|-------|:-----------------------------------------|
| **symbol**            | string     | Y    |       | 合约代码，例如"XBTC"                            |
| **type**              | enum       | Y    |       | 订单类型，限价单LIMIT与市价单MARKET                  |
| **direction**         | enum       | Y    |       | 订单方向，LONG与SHORT                          |
| **price**             | decimal    | Y    |       | 限价单报价                                    |
| **quantity**          | long       | Y    |       | 订单数量，至少为1                                |
| **fillOrKill**        | boolean    |      | false | 是否设置FOK订单                                |
| **immediateOrCancel** | boolean    |      | false | 是否设置IOC订单                                |
| **postOnly**          | boolean    |      | false | 是否设置PostOnly订单                           |
| **hidden**            | boolean    |      | false | 是否设置Hidden订单                             |
| **reduceOnly**        | boolean    |      | false | 是否设置ReduceOnly订单                         |
| **clientOrderId**     | string     | N    |       | 用户自定义订单ID，可用于查询activityOrder             |            |
| **slotId**            | **long**   | Y    |       | 订单slotId，通过接口/v1/orderNonce/gen获取 例如`1`  |
| **nonce**             | **long**   | Y    |       | 订单NonceId，通过接口/v1/orderNonce/gen获取 例如`0` |
| **signature**         | **String** | Y    |       | 请求参数签名串，通过2层私钥签名                         |

请注意：

若fillOrKill=true，则无法设置immediateOrCancel、postOnly、hidden和reduceOnly；

若immediateOrCancel=true，则无法设置fillOrKill、postOnly、hidden和reduceOnly；

若postOnly=true，则无法设置fillOrKill、immediateOrCancel和reduceOnly；

若hidden=true，则无法设置fillOrKill和immediateOrCancel；

只有MARKET订单与IOC订单可以设置reduceOnly=true。

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "orderId": 133718622601283
  }
}
```

## 查询活跃订单

API描述：查询当前用户的所有活跃订单。

API路径：GET /v1/trading/:accountId/contracts/orders/open

API请求参数(PATH)：

| 参数        | 类型    | 说明               |
| :---------- | ------- | :----------------- |
| *accountId* | **int** | **必填**<br>账户id |

```
API响应样例：订单信息
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      {
        "id": 133718622601283,
        "userId": 10030,
        "accountId": 0,
        "clientOrderId": "@133718622601283",
        "symbolId": 132,
        "sequenceId": 620679,
        "type": "LIMIT",
        "status": "PENDING",
        "direction": "LONG",
        "features": 0,
        "price": 40000.0,
        "fee": 0,
        "fillPrice": 0.0,
        "makerFeeRate": 0.001,
        "takerFeeRate": 0.002,
        "createdAt": 1688471702940,
        "updatedAt": 1688471702940,
        "marginCurrencyId": 103,
        "quantity": 4,
        "unfilledQuantity": 4,
        "frozenMargin": 1600.000000000000000000,
        "symbol": "XBTC"
      }
    ]
  }
}
```



## 查询活跃订单（通过订单ID）（合约）

API描述：通过订单ID查询当前用户活跃订单（合约）。

API路径：GET /v1/trading/:accountId/contracts/orders/open/:orderId

API请求参数(PATH)：

| 参数        | 类型    | 说明               |
| :---------- | ------- | :----------------- |
| *accountId* | **int** | **必填**<br>账户id |

API请求参数(Path Param)：

| 参数         | 类型       | 说明                                                      |
| :----------- | ---------- | :-------------------------------------------------------- |
| **order_id** | **string** | **选填**<br>订单Id,例如"25f2fd62fdf2a6bf33e9431ba184dd2e" |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": 133718622601283,
    "userId": 10030,
    "accountId": 0,
    "clientOrderId": "@133718622601283",
    "symbolId": 132,
    "sequenceId": 620679,
    "type": "LIMIT",
    "status": "PENDING",
    "direction": "LONG",
    "features": 0,
    "price": 40000.0,
    "fee": 0,
    "fillPrice": 0.0,
    "makerFeeRate": 0.001,
    "takerFeeRate": 0.002,
    "createdAt": 1688471702940,
    "updatedAt": 1688471702940,
    "marginCurrencyId": 103,
    "quantity": 4,
    "unfilledQuantity": 4,
    "frozenMargin": 1600.000000000000000000,
    "symbol": "XBTC"
  }
}
```

## 查询订单（通过订单ID）

API描述：通过订单ID查询当前用户订单。

API路径：GET /v1/trading/:accountId/contracts/orders/:order_id

API请求参数(Path Param)：

| 参数          | 类型       | 说明                                                      |
| :------------ | ---------- |:--------------------------------------------------------|
| **accountId** | **int**    | **必填**<br>账户ID                                          |
| **order_id**  | **string** | **选填**<br>订单Id,例如"133718622601283"                      |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": 133718622601283,
    "userId": 10030,
    "accountId": 0,
    "clientOrderId": "@133718622601283",
    "symbolId": 132,
    "sequenceId": 620679,
    "type": "LIMIT",
    "status": "PENDING",
    "direction": "LONG",
    "features": 0,
    "price": 40000.0,
    "fee": 0,
    "fillPrice": 0.0,
    "makerFeeRate": 0.001,
    "takerFeeRate": 0.002,
    "createdAt": 1688471702940,
    "updatedAt": 1688471702940,
    "marginCurrencyId": 103,
    "quantity": 4,
    "unfilledQuantity": 4,
    "frozenMargin": 1600.000000000000000000,
    "symbol": "XBTC"
  }
}
```

## 查询历史订单（合约）

API描述：查询当前用户的历史订单记录。

API路径：GET /v1/trading/:accountId/contracts/orders/closed

API请求参数(Path Param)：

| 参数          | 类型    | 说明               |
| :------------ | ------- | :----------------- |
| **accountId** | **int** | **必填**<br>账户ID |

API请求参数(Request Param)：

| 参数         | 类型       | 说明                                                 |
| :----------- | ---------- |:---------------------------------------------------|
| **symbol**   | **string** | **选填**<br>交易对名称，例如`XBTC`，默认空                       |
| **limit**    | **long**   | **选填**<br/>返回结果集的最大记录数量，范围1～100，默认为100             |
| **range**    | **string** | **选填**<br/>查询月份，格式为YYYYMM，例如"201907"，默认为""，表示当前月份  |
| **offsetId** | **long**   | **选填**<br/>传入当前页的起始id，默认为0，表示第一页                   |

```
API响应样例:
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
        "id": 132847776038979,
        "userId": 10030,
        "accountId": 0,
        "clientOrderId": null,
        "symbolId": 132,
        "sequenceId": 592222,
        "type": "LIMIT",
        "status": "FULLY_FILLED",
        "direction": "LONG",
        "features": 0,
        "price": 40000.000000000000000000,
        "fee": 32.000000000000000000,
        "fillPrice": 40000.0,
        "makerFeeRate": 0.001000000000000000,
        "takerFeeRate": 0.002000000000000000,
        "createdAt": 1688367889917,
        "updatedAt": 1688367889917,
        "marginCurrencyId": 103,
        "quantity": 4,
        "unfilledQuantity": 0,
        "frozenMargin": null,
        "symbol": "XBTC"
      }
    ]
  }
}
```

说明：

历史订单总是按页查询，第一页的offsetId传入0或者不传；

如果API响应样例返回的hasMore=true，则可以根据nextOffsetId构造下一页查询的URL。


## 取消订单(根据orderId)

API描述：取消一个活动订单。

API路径：GET /v1/trading/:accountId/contracts/orders/:order_id/cancel

API请求参数(Path Param)：

| 参数          | 类型     | 说明               |
| :------------ | -------- | :----------------- |
| **accountId** | **int**  | **必填**<br>账户ID |
| **order_id**  | **path** | **必填**<br>订单ID |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "orderId": 133718622601283
  }
}
```



## 取消所有订单

API描述：取消所有活动订单。

API路径: POST /v1/trading/:accountId/contracts/orders/cancel

API请求参数(Path Param)：

| 参数          | 类型    | 说明               |
| :------------ | ------- | :----------------- |
| **accountId** | **int** | **必填**<br>账户ID |

API请求参数(Request Json Body)：

```json
{
    "symbol":"XBTC"
}
```

| 参数       | 类型       | 说明                                                         |
| :--------- | ---------- | :----------------------------------------------------------- |
| **symbol** | **string** | **选填**<br>交易对名称,例如`XBTC`，为空则撤销所有交易对活动订单 |

```
API响应样例:
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

## 查询订单成交详情（合约）

API描述：查询当前用户的某个订单成交详情。

API路径：GET /v1/trading/:accountId/contracts/orders/:order_id/matches

API请求参数(Path Param)：

| 参数          | 类型       | 说明               |
| :------------ | ---------- | :----------------- |
| **accountId** | **int**    | **必填**<br>账户ID |
| **order_id**  | **string** | **必填**<br>订单ID |

API响应样例：订单成交详细信息，按时间排序

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      {
        "taker": true,
        "price": 40000.000000000000000000,
        "quantity": 4.000000000000000000,
        "fee": 32.000000000000000000,
        "createdAt": 1688367889917
      }
    ]
  }
}
```

-如果一个订单的成交记录超过1000条，则最多返回前1000条记录。

## 查询当前活动仓位列表

API描述：获取当前活动仓位列表。

API路径：GET /v1/trading/:accountId/contracts/positions

API请求参数(Path Param)：

| 参数          | 类型    | 说明               |
| :------------ | ------- | :----------------- |
| **accountId** | **int** | **必填**<br>账户ID |

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      {
        "direction": "SHORT",
        "userId": 10031,
        "accountId": 0,
        "symbolId": 132,
        "id": "10031_0_132",
        "riskLevel": 0,
        "riskLimitId": 105,
        "riskLimit": {
          "id": 105,
          "initialMarginRate": 0.01,
          "maintenanceMarginRate": 0.005,
          "marginRateStep": 0.005,
          "maxLeverage": 10,
          "riskLimitBase": 200,
          "riskLimitStep": 100,
          "maxRiskLimitSteps": 9,
          "createdAt": 1546956010600
        },
        "quantity": 4,
        "maxQuantity": 200,
        "updatedAt": 1688367889917,
        "realizedPNL": -16.000000000000000000,
        "entryPrice": 40000.0,
        "entryValue": 16000.0000000000000000000,
        "minimumMaintenanceMarginRate": 0.005,
        "requiredMargin": 1600.000000000000000000,
        "minimumMaintenanceMargin": 80.0000000000000000000000,
        "unrealizedPNL": 2999.645208400000000000000000000000000000,
        "symbol": "XBTC",
        "closed": false
      }
    ]
  }
}
```

## 查询仓位清算列表

API描述：获取仓位清算列表。

API路径：GET /v1/trading/:accountId/contracts/clearings/positions

API请求参数(Path Param)：

| 参数          | 类型    | 说明               |
| :------------ | ------- | :----------------- |
| **accountId** | **int** | **必填**<br>账户ID |

API请求参数(Request Param)：

| 参数         | 类型       | 说明                                                |
| :----------- | ---------- |:--------------------------------------------------|
| **symbol**   | **string** | **选填**<br>交易对名称，例如`XBTC`，默认空                      |
| **limit**    | **long**   | **选填**<br/>返回结果集的最大记录数量，范围1～100，默认为100            |
| **range**    | **string** | **选填**<br/>查询月份，格式为YYYYMM，例如"201907"，默认为""，表示当前月份 |
| **offsetId** | **long**   | **选填**<br/>传入当前页的起始id，默认为0，表示第一页                  |

```
API响应样例:
```

```json
{
    "code": 200,
    "msg": "success",
    "data": {
        "range": "202306",
        "hasMore": true,
        "nextOffsetId": 22979,
        "results": [
            {
                "id": 22991,
                "orderId": 0,
                "sequenceId": 550183,
                "direction": "LONG",
                "type": "LIQUIDATE",
                "clearingPrice": 30001.700000000000000000,
                "rate": 0E-18,
                "fee": 0E-18,
                "quantityChanged": -5,
                "quantityAfterClearing": 0,
                "realizedPNLChanged": -4999.150000000000000000,
                "createdAt": 1687937250385,
                "txHash": "",
                "symbol": "XBTC"
            },
            {
                "id": 22987,
                "orderId": 129221917671488,
                "sequenceId": 549851,
                "direction": "LONG",
                "type": "OPEN",
                "clearingPrice": 40000.000000000000000000,
                "rate": 0.001000000000000000,
                "fee": 20.000000000000000000,
                "quantityChanged": 5,
                "quantityAfterClearing": 5,
                "realizedPNLChanged": -20.000000000000000000,
                "createdAt": 1687935674303,
                "txHash": "0x8f9bfeacbc18398d2303935672eeea0515a867531ed7920846f822b47254dce1",
                "symbol": "XBTC"
            },
            {
                "id": 22983,
                "orderId": 129219359146048,
                "sequenceId": 549817,
                "direction": "LONG",
                "type": "CLOSE",
                "clearingPrice": 40000.000000000000000000,
                "rate": 0.001000000000000000,
                "fee": 4.800000000000000000,
                "quantityChanged": -1,
                "quantityAfterClearing": 0,
                "realizedPNLChanged": -4.800000000000000000,
                "createdAt": 1687935391136,
                "txHash": "0x5fe2e858dbfa721198d71a6514dccca5669f3defad5ac132733a83e9a8288514",
                "symbol": "XBTC"
            },
            {
                "id": 22981,
                "orderId": 129217966637120,
                "sequenceId": 549799,
                "direction": "LONG",
                "type": "OPEN",
                "clearingPrice": 40000.000000000000000000,
                "rate": 0.001000000000000000,
                "fee": 4.000000000000000000,
                "quantityChanged": 1,
                "quantityAfterClearing": 1,
                "realizedPNLChanged": -4.000000000000000000,
                "createdAt": 1687935232397,
                "txHash": "0xd71871b40cb775d96af462e086fbf22bdb725fa8a01c6cae313cee9fa69efcba",
                "symbol": "XBTC"
            },
            {
                "id": 22980,
                "orderId": 129217966637120,
                "sequenceId": 549799,
                "direction": "SHORT",
                "type": "CLOSE",
                "clearingPrice": 40000.000000000000000000,
                "rate": 0.001000000000000000,
                "fee": 8.000000000000000000,
                "quantityChanged": -2,
                "quantityAfterClearing": 0,
                "realizedPNLChanged": -8.000000000000000000,
                "createdAt": 1687935232397,
                "txHash": "0xd71871b40cb775d96af462e086fbf22bdb725fa8a01c6cae313cee9fa69efcba",
                "symbol": "XBTC"
            }
        ]
    }
}

```


## 平仓根据symbol

API描述：以市价单平掉该symbol仓位

API路径: POST /v1/trading/:accountId/contracts/positions/close

API请求参数(Path Param)：

| 参数          | 类型    | 说明               |
| :------------ | ------- | :----------------- |
| **accountId** | **int** | **必填**<br>账户ID |

API请求参数(Request Json Body)：

```json
{
  "symbol": "XBTC",
  "slotId": 2,  // 参照下单
  "signature": "test", // 参照下单
  "nonce": 11,  // 参照下单
  "price": 40000 // 调用接口获取市价单价格
}
```

```
API响应样例:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "orderId": 129218386067520
  }
}
```


