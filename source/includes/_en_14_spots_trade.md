# Spot Trading

Spot trading API can be used for spot trading, including placing orders, cancelling orders, viewing current active orders, and querying historical orders.

## Query Account Balance

API Description: Query the balance of a user's spot account.

API Path: GET /v1/trading/:accountId/balances

API Path Parameters:

| Parameter        | Type       | Description               |
| :---------- |----------| :----------------- |
| *accountId* | **path** | **Required**<br>account id |

```
API Response Example:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "BTC": [
      "0", // available
      "0" // spot frozen
    ],
    "USDT": [
      "0",
      "0"
    ]
  }
}
```

## Query User Fees

API Description: Query the trading fees for a user's spot trades.

API Path: GET /v1/trading/spots/fee/rate

API Query Parameters:

| Parameter       | Type     | Description                            |
| :--------- | -------- |:------------------------------|
| **symbol** | **string** | **Required**<br>Symbol name, for example `WBTC_USDC` |




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
    "timestamp": 1688386621807
  }
}
```

## Get Order Nonce Information

API Description: This API provides real-time access to the order nonce information required when creating an order.

API Path: GET /v1/orderNonce/gen/:slotType/:size

API Path Parameters:

| Parameter         | Type      | Description                     |
|:-----------|---------|:-----------------------|
| *slotType* | **int** | **Required**<br> Spot 0 Contract 1  |
| *size*     | **int** | **Optional**<br> Batch retrieval. If not specified, returns one entry |

```
API Response Example:
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "userId": 10030,
    "nonce": 0,    // Required when placing an order
    "slotId": 28773,  // Raequired when placing an order
    "available": true // Whether available to use
  }
}
```


## Place Order

API Description: Create a new order.

API Path: POST /v1/trading/:accountId/spots/orders

API Path Parameters:

| Parameter        | Type    | Description               |
| :---------- | ------- | :----------------- |
| *accountId* | **int** | **Required**<br>account id |

API Request Json Body:

| Parameter                    | Type          | Description                                                  |
|:----------------------|-------------|-----------------------------------------------------|
| **symbol**            | **string**  | **Required**<br>Symbol name, for example `WBTC_USDC`                       |
| **type**              | **enum**    | **Required**<br/>Order types: Limit Order="LIMIT", Market Order="MARKET"            |
| **direction**         | **enum**    | **Required**<br/>Order directions: Buy="LONG", Sell="SHORT"                |
| **price**             | **decimal** | **Required**<br/>Order price, for example `7123.5`                          |
| **quantity**          | **decimal** | **Required**<br/>Order quantity, for example `1.02`                            |
| **fillOrKill**        | **boolean** | **Optional**<br/>Whether it's a Fill or Kill (FOK) order, for example `true`                       |
| **immediateOrCancel** | **boolean** | **Optional**<br/>Whether it's an Immediate or Cancel (IOC) order, for example `true`                        |
| **postOnly**          | **boolean** | **Required**<br/>Whether it's a Post Only order, for example `true`                        |
| **hidden**            | **boolean** | **Required**<br/>Whether it's an Iceberg order, for example `true`. The fee rate for Iceberg orders is based on the taker fee rate    |
| **clientOrderId**     | **string**  | **Optional**<br/>User-defined OrderId, which can be used to query activity Order        |
| **slotId**            | **long**    | **Required**<br/>Order slotId, obtained through the /v1/orderNonce/gen interface, for example `1`  |
| **nonce**             | **long**    | **Required**<br/>Order NonceId, obtained through the /v1/orderNonce/gen interface, for example `0` |
| **signature**         | **String**  | **Required**<br/>The request parameter signature string is signed using a two-layer private key, referring to the ZkLink signature document           |



```
API Response Example: Order Information
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

Note: The order ID is returned asynchronously. The result of the order transaction will be notified through the WebSocket (WS), or you can actively query the transaction result<br/>
For market orders, a price is still required. Prices can be obtained from the endpoint /v1/trading/market/price/:type

## Cancel Order

API Description: Cancel an order.

API Path: POST /v1/trading/:accountId/spots/orders/:orderId/cancel

API Path Parameters:

| Parameter        | Type     | Description               |
| :---------- | -------- | :----------------- |
| **order_id** | **path** | **Required**<br>Order ID |
| **accountId** | **path** | **Required**<br/>Account ID |

```
API Response Example: 
{
    "code": 200,
    "msg": "success",
    "data": {
        "orderId": 133016311562368
    }
}
```
```
API Error Response:
{
"code": 1003,
"msg": "Active spots order not found.",
"data": null
}
```

## Cancel All Orders

API Description: Cancel all orders.

API Path: POST /v1/trading/:accountId/spots/orders/cancel

API Path Parameters:

| Parameter        | Type    | Description               |
| :---------- | ------- | :----------------- |
| *accountId* | **int** | **Required**<br>Account id |

API Request Json Body:

```json
{
    "symbol":"WBTC_USDC"
}
```

| Parameter       | Type       | Description                                           |
| :--------- | ---------- |:---------------------------------------------|
| **symbol** | **string** | **Optional**<br>Symbol name, for example `WBTC_USDC`, cancel all trading pair orders when it's empty |

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
Note:

This interface is a batch operation executed by the server on behalf of the client, and not all orders are attempted to be canceled simultaneously. Therefore, the timeliness of canceling all orders cannot be guaranteed.

## Query Order Details by Order ID

API Description: Retrieve details of a specific order based on the order ID.

API Path: GET /v1/trading/:accountId/spots/orders/:orderId

API Path Parameters:

| Parameter        | Type     | Description               |
| :---------- | -------- | :----------------- |
| **accountId** | **path** | **Required**<br/>Account ID |
| **order_id** | **path** | **Required**<br>Order ID |



```
API Response Example: Order Details
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

OrderStatus:

- PENDING: Active Orders Waiting for Execution.
- FAILED: Order Execution Failed (Insufficient Margin or Other Reasons), Final Status.
- FULLY_FILLED: Fully Executed, Final Status.
- PARTIAL_FILLED: Partially Filled.
- PARTIAL_CANCELLED: Canceled After Partially Filled, Final Status.
- FULLY_CANCELLED: Canceled Before Fill, Final Status.



## Query Order Details by clientOrderId (Only supports active orders)

API Description: Query the details of an order based on the user-defined Order ID.

API Path: GET /v1/trading/:accountId/spots/clientOrders/open/:client_order_id

API Path Parameters:

| Parameter                | Type     | Description                         |
| :------------------ | -------- | :--------------------------- |
| **accountId**       | **path** | **Required**<br/>Account ID          |
| **client_order_id** | **path** | **Required**<br>User-defined Order ID |



```
API Response Example: Order Details
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



## Query Order Execution Details

API Description: Query the execution details of a specific order for the current user.

API Path: GET /v1/trading/:accountId/spots/orders/:order_id/matches

API Path Parameters:


| Parameter   | Type     | Description                            |
| :----- | -------- | :------------------------------ |
| **accountId** | **path** | **Required**<br/>Account ID |
| **order_id** | **path** | **Required**<br>URL address parameters, order ID |



```
API Response Example: Order execution details sorted by time
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

If an order's transaction records exceed 1000 entries, only the first 1000 records will be returned at most.

## Query Settlement Result

API Description: Query the settlement result of a specific order for the current user.

API Path: GET /v1/trading/:accountId/spots/match/clearings

API Path Parameters:


| Parameter          | Type     | Description                |
| :------------ | -------- | :------------------ |
| **accountId** | **path** | **Required**<br/>Account ID |

API Request Parameters:

| Parameter         | Type       | Description                                                |
| :----------- | ---------- |:--------------------------------------------------|
| **symbol**   | **string** | **Optional**<br>Trading pair name, for example `WBTC_USDC`, default is empty.                 |
| **limit**    | **long**   | **Optional**<br/>The maximum number of records in the result set, ranging from 1 to 100, default is 100            |
| **range**    | **string** | **Optional**<br/>The query month, formatted as YYYYMM, for example "201907", default is "" which represents the current month |
| **offsetId** | **long**   | **Optional**<br/>The starting ID of the current page, default is 0, indicating the first page                  |



```
API Response Example: Order execution details sorted by time
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
        "symbol": "WBTC_USDC"
      }
    ]
  }
}
```

## Query all active orders

API Description: Query active orders for the current user.

API Path: GET /v1/trading/:accountId/spots/orders/open

API Path Parameters:


| Parameter          | Type     | Description                |
| :------------ | -------- | :------------------ |
| **accountId** | **path** | **Required**<br/>Account ID |

```
API Response Example: Order details sorted by time
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      {
        "id": 129987889856579,
        "userId": 10030,
        "accountId": 0,
        "clientOrderId": "@129987889856579",
        "symbolId": 100103,
        "sequenceId": 558326,
        "type": "LIMIT",
        "status": "PARTIAL_FILLED",
        "direction": "LONG",
        "features": 0,
        "price": 40000.0,
        "fee": 39.600000,
        "fillPrice": 40000.0,
        "makerFeeRate": 0.001,
        "takerFeeRate": 0.002,
        "createdAt": 1688026964900,
        "updatedAt": 1688026993802,
        "chargeQuote": true,
        "quantity": 1.00,
        "unfilledQuantity": 0.01,
        "symbol": "WBTC_USDC"
      }
    ]
  }
}
```

## Query active orders by order ID

API Description: Query active orders for the current user based on order ID.

API Path: GET /v1/trading/:accountId/spots/orders/open/:orderId

API Path Parameters:


| Parameter          | Type     | Description             |
| :------------ | -------- |:----------------|
| **accountId** | **path** | **Required**<br/>Account ID |
| **orderId** | **path** | **Required**<br/>Order Id |


```
API Response Example: Order details sorted by time
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "id": 133701593727043,
    "userId": 10030,
    "accountId": 0,
    "clientOrderId": "@133701593727043",
    "symbolId": 100103,
    "sequenceId": 620475,
    "type": "LIMIT",
    "status": "PENDING",
    "direction": "LONG",
    "features": 0,
    "price": 40000.0,
    "fee": 0,
    "fillPrice": 0.0,
    "makerFeeRate": 0.001,
    "takerFeeRate": 0.002,
    "createdAt": 1688469672168,
    "updatedAt": 1688469672168,
    "chargeQuote": false,
    "quantity": 0.10,
    "unfilledQuantity": 0.10,
    "symbol": "WBTC_USDC"
  }
}
```

## Query historical orders

API Description: Query historical orders for the current user.

API Path: GET /v1/trading/:accountId/spots/orders/closed

API Path Parameters:


| Parameter          | Type     | Description                |
| :------------ | -------- | :------------------ |
| **accountId** | **path** | **Required**<br/>Account ID |

API Request Parameters:

| Parameter         | Type       | Description                                                |
| :----------- | ---------- |:--------------------------------------------------|
| **symbol**   | **string** | **Optional**<br>Trading pair name, for example `WBTC_USDC`, default is empty                |
| **status**   | **string** | **Optional**<br/> Order status, for example FULLY_FILLED                  |
| **limit**    | **long**   | **Optional**<br/>返The maximum number of records in the result set, ranging from 1 to 100, default is 100            |
| **range**    | **string** | **Optional**<br/>The query month, formatted as YYYYMM, for example "201907", default is "" which represents the current month |
| **offsetId** | **long**   | **Optional**<br/>The starting ID of the current page, default is 0, indicating the first page                  |

```
API Response Example: Order details sorted by time
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "range": "202306",
    "hasMore": false,
    "nextOffsetId": 0,
    "results": [
      {
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
      },
      {
        "id": 128593124393026,
        "userId": 10030,
        "accountId": 0,
        "clientOrderId": null,
        "symbolId": 101103,
        "sequenceId": 539618,
        "type": "LIMIT",
        "status": "FULLY_CANCELLED",
        "direction": "SHORT",
        "features": 0,
        "price": 50000.000000000000000000,
        "fee": 0E-18,
        "fillPrice": 0.0,
        "makerFeeRate": 0.001000000000000000,
        "takerFeeRate": 0.002000000000000000,
        "createdAt": 1687860695563,
        "updatedAt": 1687860944692,
        "chargeQuote": false,
        "quantity": 0.010000000000000000,
        "unfilledQuantity": 0.010000000000000000,
        "symbol": "ETH_USDT"
      }
    ]
  }
}
```

Note:

Historical orders are always queried by page. For the first page, the offsetId should be set to 0 or not included.

If the API response example returns hasMore=true, you can construct the URL for the next page query based on nextOffsetId.

This interface does not support cross-month queries.