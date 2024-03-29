

# Futures Trading

The Futures Trading API can be used for futures trading, including position management, order placement, order cancellation, viewing current active orders, and querying historical orders.

The trading API must be accompanied by the user's API signature for identity verification.



## Query Account Equity

API Description: Query user account equity, including positions, unrealized profit and loss, etc.

API Path: GET /v1/trading/:accountId/assets

API Path Parameters:

| Parameter        | Type    | Description               |
| :---------- | ------- | :----------------- |
| *accountId* | **int** | **Required**<br>Account id |

```
API Response Example:
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

## Query User Fee Rates (Contract)

API Description: Query user fee rates for contract trading (futures, derivatives).

API Path: GET /v1/trading/contracts/fee/rate

API Query Parameters:

| Parameter       | Type       | Description                                                 |
| :--------- | ---------- | :--------------------------------------------------- |
| **symbol** | **string** | **Optional**<br>Trading Pair Name, for example `XBTC`|


```
API Response Example:
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

## Create New Order

API Description: Create a new order.

API Path: POST /v1/trading/:accountId/contracts/orders

API Path Parameters:

| Parameter        | Type    | Description               |
| :---------- | ------- | :----------------- |
| *accountId* | **int** | **Required**<br>Account id |

API Request Json Body:

| Field                  | Field Type       | Required | Default Value   | Description                                       |
|:----------------------|------------|------|-------|:-----------------------------------------|
| **symbol**            | string     | Y    |       | Contract code, for example "XBTC"                           |
| **type**              | enum       | Y    |       | Order type, limit order (LIMIT) and market order (MARKET)                  |
| **direction**         | enum       | Y    |       | Order direction, LONG and SHORT                          |
| **price**             | decimal    | Y    |       | Limit order price                                    |
| **quantity**          | long       | Y    |       | Order quantity, must be at least 1                                |
| **fillOrKill**        | boolean    |      | false | Whether to set FOK order                                |
| **immediateOrCancel** | boolean    |      | false | Whether to set IOC order                                |
| **postOnly**          | boolean    |      | false | Whether to set PostOnly order                           |
| **hidden**            | boolean    |      | false | Whether to set Hidden order                             |
| **reduceOnly**        | boolean    |      | false | Whether to set ReduceOnly order                         |
| **clientOrderId**     | string     | N    |       | User-defined order ID, which can be used to query activityOrder                 |
| **slotId**            | **long**   | Y    |       | Order slot ID, obtained through the /v1/orderNonce/gen interface, for example `1` |
| **nonce**             | **long**   | Y    |       | Order Nonce ID, obtained through the /v1/orderNonce/gen interface, for example `0` |
| **signature**         | **String** | Y    |       | The request parameter signature string is signed using a two-tier private key                         |

Please note:

If fillOrKill=true, then immediateOrCancel, postOnly, hidden, and reduceOnly cannot be set.

If immediateOrCancel=true, then fillOrKill, postOnly, hidden, and reduceOnly cannot be set.

If postOnly=true, then fillOrKill, immediateOrCancel, and reduceOnly cannot be set.

If hidden=true, then fillOrKill and immediateOrCancel cannot be set.

Only MARKET orders and IOC orders can set reduceOnly=true.

```
API Response Example:
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


## Query Order Details by clientOrderId (Only supports active orders)

API Description: Retrieve order details by the custom clientOrderId.

API Path: GET /v1/trading/:accountId/contracts/clientOrders/open/:client_order_id

API Path Parameters:

| Parameter                | Type   | Description                        |
| :------------------ | -------- | :--------------------------- |
| **accountId**       | **path** | **Required**<br/>AccountID          |
| **client_order_id** | **path** | **Required**<br>User-defined order ID |



```
API Response Example: Order details
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

## Query Active Orders

API Description: Query all active orders for the current user.

API Path: GET /v1/trading/:accountId/contracts/orders/open

API Path Parameters:

| Parameter        | Type    | Description              |
| :---------- | ------- | :----------------- |
| *accountId* | **int** | **Required**<br>AccountID |

```
API Response Example: Order information
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



## Query Active Order (By Order ID) (Contract)

API Description: Query the current user's active order by order ID (contract).

API Path: GET /v1/trading/:accountId/contracts/orders/open/:orderId

API Path Parameters:

| Parameter        | Type    | Description               |
| :---------- | ------- | :----------------- |
| *accountId* | **int** | **Required**<br>AccountID |

API Path Parameters:

| Parameter         | Type       | Description                                |
| :----------- | ---------- |:----------------------------------|
| **order_id** | **string** | **Optional**<br>OrderID, for example "12345678654321" |

```
API Response Example:
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

## Query Order (By Order ID)

API Description: Query the current user's order by order ID.

API Path: GET /v1/trading/:accountId/contracts/orders/:order_id

API Path Parameters:

| Parameters          | Type       | Description                                                      |
| :------------ | ---------- |:--------------------------------------------------------|
| **accountId** | **int**    | **Required**<br>AccountID                                          |
| **order_id**  | **string** | **Optional**<br>OrderID, for example "133718622601283"                      |

```
API Response Example:
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

## Query Historical Orders (Contract)

API Description: Retrieve the historical order records of the current user.

API Path: GET /v1/trading/:accountId/contracts/orders/closed

API Path Parameters:

| Parameter          | Type    | Description               |
| :------------ | ------- | :----------------- |
| **accountId** | **int** | **Required**<br>AccountID |

API Request Parameters:

| Parameter           | Type        | Description                                                           |
|:-------------|------------|:-------------------------------------------------------------|
| **symbol**   | **string** | **Optional**<br>Trading pair name, for example `WBTC_USDC`, default is empty                                 |
| **status**   | **string** | **Optional**<br/> Order status, for example FULLY_FILLED                             |
| **limit**    | **long**   | **Optional**<br/>The maximum number of records in the result set, ranging from 1 to 100, default is 100                |
| **range**    | **string** | **Optional**<br/>The query month, formatted as YYYYMM, for example "201907", default is "" which represents the current month            |
| **offsetId** | **long**   | **Optional**<br/>The starting ID of the current page, default is 0, indicating the first page                             |


```
API Response Example:
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


Note:

Historical orders are always queried by page, with the offsetId of the first page set to 0 or left unspecified.

If the API response example returns hasMore=true, you can construct the URL for the next page query based on nextOffsetId.

This interface does not support cross-month queries.



## Cancel Order (by Order ID)

API Description: Cancel an active order.

API Path: GET /v1/trading/:accountId/contracts/orders/:order_id/cancel

API Path Parameters:

| Parameter          | Type     | Description               |
| :------------ | -------- | :----------------- |
| **accountId** | **int**  | **Required**<br>AccountID |
| **order_id**  | **path** | **Required**<br>OrderID |

```
API Response Example:
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



## Cancel All Orders

API Description: Cancel all active orders.

API Path: POST /v1/trading/:accountId/contracts/orders/cancel

API Path Parameters:

| Parameter          | Type    | Description               |
| :------------ | ------- | :----------------- |
| **accountId** | **int** | **Required**<br>Account ID |

API Request Json Body:

```json
{
    "symbol":"XBTC"
}
```

| Parameter       | Type       | Description                                                         |
| :--------- | ---------- | :----------------------------------------------------------- |
| **symbol** | **string** | **Optional**<br>Trading Pair Name, e.g., `XBTC`. Leave empty to cancel all active orders for all trading pairs |

```
API Response Example:
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

## Query Order Execution Details (Futures)

API Description: Retrieve the execution details of a specific order for the current user.

API Path: GET /v1/trading/:accountId/contracts/orders/:order_id/matches

API Path Parameters:

| Parameter          | Type       | Description               |
| :------------ | ---------- | :----------------- |
| **accountId** | **int**    | **Required**<br>Account ID |
| **order_id**  | **string** | **Required**<br>Order ID |

API Response Example: Order execution details sorted by time

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

-If an order's transaction records exceed 1000 entries, only the first 1000 records will be returned at most.

## Query Active Position List

API Description: Retrieve the list of current active positions.

API Path: GET /v1/trading/:accountId/contracts/positions

API Path Parameters:

| Parameter          | Type    | Description               |
| :------------ | ------- | :----------------- |
| **accountId** | **int** | **Required**<br>Account ID |

```
API Response Example:
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

## Query Position Liquidation List

API Description: Retrieve the list of position liquidations.

API Path: GET /v1/trading/:accountId/contracts/clearings/positions

API Path Parameters:

| Parameter          | Type    | Description               |
| :------------ | ------- | :----------------- |
| **accountId** | **int** | **Required**<br>Account ID |

API Request Parameters:

| Parameter         | Type       | Description                                                |
| :----------- | ---------- |:--------------------------------------------------|
| **symbol**   | **string** | **Optional**<br>Trading pair name, for example `WBTC_USDC`, default is empty                      |
| **limit**    | **long**   | **Optional**<br/>The maximum number of records in the result set, ranging from 1 to 100, default is 100           |
| **range**    | **string** | **Optional**<br/>The query month, formatted as YYYYMM, for example "201907", default is "" which represents the current month |
| **offsetId** | **long**   | **Optional**<br/>The starting ID of the current page, default is 0, indicating the first page                 |

```
API Response Example:
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


## Close Position by Symbol

API Description: Close the position for the specified symbol with a market order.

API Path: POST /v1/trading/:accountId/contracts/positions/close

API Path Parameters:

| Parameter          | Type   | Description               |
| :------------ | ------- | :----------------- |
| **accountId** | **int** | **Required**<br>Account ID |

API Request Json Body:

```json
{
  "symbol": "XBTC",
  "slotId": 2,  // Refer to the order placement.
  "signature": "test", // Refer to the order placement.
  "nonce": 11,  // Refer to the order placement.
  "price": 40000 // Call API to Retrieve Market Order Price
}
```

```
API Response Example:
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

## Obtaining Market Order Price

API Description: Users need to sign the market order price. The system will execute trades around this price.

API Path: POST /v1/trading/market/price/:type

API Path Parameters:

| Parameter          | Type    | Description                   |
| :------------ | ------- |:---------------------|
| **type** | **int** | **Required**<br> Spot 0 Contract 1 |

API Request Json Body:

```json
{
  "marketPriceList":[
    {
    "symbolId": 132,   // Order symbolId
    "direction": "SHORT"  // Order direction
    }
    // Support multiple queries
  ]
}
```

```
API Response Example:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "SHORT": {
      "XBTC": "0"
    }
  }
}
```
