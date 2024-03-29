

# WebSocket API

The WebSocket API refers to the data that is pushed after connecting to WebSocket.

## Connection Information

WebSocket only supports the wss protocol:

- test environment: wss://testnet.app.zkex.com/v1/market/notification
- prd environment: wss://app.zkex.com/v1/market/notification


## User Authentication

To connect to the WebSocket as an authenticated user, you must provide a valid WebSocket token.

You can obtain a WebSocket token by making an API request to the following API endpoint (API signature required):

User API Request: GET [/v1/users/wss/token](https://testnet.app.zkex.com/v1/users/wss/token)


```
If the user is not logged in, the result will be as follows:

{
  "code": 1003,
  "msg": "AUTH_SIGNIN_REQUIRED",
  "data": null
}

If the user is already logged in, the JSON response will include the Token:
{
  "code": 200,
  "msg": "success",
  "data": {
    "result": "ZVBkOHFYMDAwMDAyNzJlMTg5MjQxMjAxM2ZjZWQzYmRhYTUxNjM2OGQ4YjI4NjU5NDhjZjA3ZDQ3MjMyYWE5MDNlYTBkNTRlMWMxYTVhZGNlOGQ0YmI0YTA5"
  }
}

```
This token has a default validity period of 1 minute.

## Authentication Connection

Append the obtained token as a parameter to the wss connection.

```
wss://testnet.app.zkex.com/v1/market/notification?token=TlZsTWdwMDAwMDAyNzJlMTg4YzI3YjdmZDA5MmRkODY0MjE2YjNmYTU4YWMxY2EwZTdhMjRjY2IzMzNkMjMzMmViMzAxYzE1Njg2ZWFmNjAwYTNhNTdlOTgw

If the WSS server successfully verifies the user, it will push messages.

{
  "status": "connected",
  "message": "connected as signed user",
  "userId": 10030
}

If the WSS server fails to verify the user, the pushed message will not contain the userId.

{
  "status": "connected",
  "message": "connected as anonymous user"
}

```
After successfully authenticating the WSS connection, the server will push user-related information such as order executions and cancellations.

## Subscribing

After connecting to WSS, you need to subscribe to a specific symbol by sending the following message:

```json
{
  "action":"subscribe",
  "symbol":"WBTC_USDC"   // Subscribe to the specified symbol
}
```

Response Format:

```json
{
  "status": "subscribe"  // All information related to the specified symbol will be subscribed to
}
```

The types of messages pushed via WebSocket (WS) are as follows:

1. orderbook 
2. prices  
3. bar: support kline {MIN,MIN5,MIN15,MIN30,HOUR,HOUR4,DAY,WEEK,MONTH}
4. bbo


##  Unsubscribing

Already subscribed topics can be unsubscribed using the 'unsubscribe' action. The message format is as follows:

```json
{
    "action":"unsubscribe",
    "symbol":"WBTC_USDC"   // Subscribe to the specified symbol
}
```


## Heartbeat

Clients need to send a ping message every '15 seconds' to maintain connection heartbeat. The format is as follows:

```json
{
    "action":"ping"
}
```

Server Response:

```json
{
  "type":"pong",
  "timestamp":1688721638247
}
```

## Prices Message
This message type does not require subscription. After connecting to the WebSocket (WS), the server will periodically push market price summary messages with the following format:

```json
{
  "type": "prices",
  "data": {
    "XBTC": [
      1688458520000, // timestamp
      40000.000000,  // open
      40000.000000,  // high
      40000.000000,  // low
      40000.000000,  // close
      0.000000,      // volume
      0.000000       // amount
    ],
    "XBCH": [
      1688458520000,
      117.500000,
      117.500000,
      117.500000,
      117.500000,
      0.000000,
      0.000000
    ],
    "ETH_USDT": [
      1688458520000,
      1148.000000,
      1148.000000,
      654.000000,
      654.000000,
      1.000000,
      654.000000
    ]
  }
}
```

Note:

- Message Type: prices
- Data Format: Same as the Bar data type, can be interpreted as a single candlestick chart for the past 24 hours (but different from daily candles, as the start time for daily candles is 0:00).
- Subscription Required: No

## Bar Information
This message type requires subscription based on the symbol, pushing all supported bars.
The server will periodically push bar (candle) information based on actual trades:

```json
{
  "type": "bar",
  "symbol": "WBTC_USDC",
  "resolution": "MONTH",
  "sequenceId": 629050,
  "data": [
    1688198400000,
    40000,
    40000,
    40000,
    40000,
    0.1,
    4000
  ]
}  
```
Note:

- Message Type: bar
- Data Format: [timestamp, open, high, low, close, amount, volume]
- Subscription Required: Yes

## OrderBook Information

The server incrementally pushes OrderBook messages at a fixed frequency, as follows:

OrderBook message format:

```json
{
  "type": "orderbook",
  "symbol": "WBTC_USDC",
  "lastSequenceId": 629050,
  "sequenceId": 629112,
  "data": {
    "sellOrders": {},
    "direction": "LONG",
    "symbol": "WBTC_USDC",
    "price": 40000,
    "buyOrders": [
      [
        40000,
        0.1
      ]
    ],
    "symbolId": 100103,
    "sequenceId": 629112,
    "timestamp":1688721638247

  }
}
```

Note:

- Message Type: orderbook
- Subscription Required: Yes
- The format of depth data is `[price, amount]`
- You need to maintain the order book yourself and update it incrementally based on the sequence ID.
- Rules for maintaining the order book: When pushing, match based on price. If the current price exists, update the amount to the latest value (replace directly).
  If the amount equals 0, remove that price from the order book. If the current price does not exist, insert it into the order book in sorted order based on price.

## BBO Message

The server pushes BBO (Best Bid Offer) data at a fixed frequency when there are changes in the system's order book.

BBO Message Format:

```json
{
  "type": "bbo",
  "symbol": "WBTC_USDC",
  "data": {
    "sequenceId": 656695,
    "timestamp": 1689129928255,
    "bidPrice": 39000.0,
    "bidVolume": 0.10,
    "askPrice": 39200.0,
    "askVolume": 0.10
  }
}
```

Note:

- Message Type: bbo
- Subscription Required: Yes

## Tick Message

The server irregularly pushes the latest tick message of the last trade as follows:

```json
{
  "type": "tick",
  "symbol": "WBTC_USDC",
  "sequenceId": 629128,
  "data": [
    [
      1688546965845,
      0,   
      40000.000000,
      0.100000,
      4000.000000,
      0
    ]
  ]
}
```

Note:

- Message Type: tick
- Subscription Required: Yes
- A tick message contains one or more trade information.
- tick data format `[timestamp, dir, price, volume ,amount, flag]`
  - timestamp
  - dir: 1=Buy, 0=Sell
  - price: Trade price
  - amount: Trade volume
  - volume: Trade value
  - flag: 0=Normal trade (additional liquidation flag will be added later)

  
## Order Execution Message

When a user's order is executed, an order execution message is generated:

```json
{
  "sequenceId": 629244,
  "data": {
    "symbolId": 100103,
    "symbol": "WBTC_USDC",
    "matchQuantity": 0.1,
    "orderId": 134357624815680,
    "matchType": "MAKER",
    "fee": 4.0,
    "feeCurrency": "USDT",
    "createdAt": 1688547890569,
    "matchPrice": 40000.0,
    "txHash": "0x0d2378a304e5b9681362ed6da7b1cdf42e6e4a9888aef5f494b7b0d6a955b3ce",
    "tradeType": "SPOTS",
    "direction": "SHORT"
  },
  "type": "order_matched",
  "userId": 10030,
  "timestamp":1688721638247
}
```

Note:
- Authentication Required: Yes



## Order Status Change Message

When the status of a user's order changes, an order status change message is received:

```
// order_pending
{
  "sequenceId": 629212,
  "data": {
    "symbolId": 100103,
    "symbol": "WBTC_USDC",
    "quantity": 0.1,
    "orderId": 134355368280128,
    "clientOrderId": "@207390481842241",
    "fee": 0,
    "type": "LIMIT",
    "fillPrice": 0.0,
    "createdAt": 1688547608090,
    "unfilledQuantity": 0.1,
    "price": 40000.0,
    "tradeType": "SPOTS",
    "status": "PENDING",
    "direction": "SHORT"
  },
  "type": "order_pending",
  "userId": 10030,
  "timestamp":1688721638247
}
// order_cancelled
{
  "sequenceId": 629219,
  "data": {
    "symbolId": 100103,
    "symbol": "WBTC_USDC",
    "quantity": 0.1,
    "orderId": 134355368280128,
    "clientOrderId": "@207390481842241",
    "fee": 0,
    "type": "LIMIT",
    "fillPrice": 0.0,
    "createdAt": 1688547608090,
    "unfilledQuantity": 0.1,
    "price": 40000.0,
    "tradeType": "SPOTS",
    "status": "FULLY_CANCELLED",
    "direction": "SHORT"
  },
  "type": "order_cancelled",
  "userId": 10030,
  "timestamp":1688721638247
}
// order_filled
{
    "sequenceId": 629244,
    "data": {
        "symbolId": 100103,
        "symbol": "WBTC_USDC",
        "quantity": 0.1,
        "orderId": 134357624815680,
        "clientOrderId": "@207390481842241",
        "fee": 4.0,
        "type": "LIMIT",
        "fillPrice": 40000.0,
        "createdAt": 1688547877777,
        "unfilledQuantity": 0.0,
        "price": 40000.0,
        "tradeType": "SPOTS",
        "status": "FULLY_FILLED",
        "direction": "SHORT"
    },
    "type": "order_filled",
    "userId": 10030,
    "timestamp":1688721638247
}
// order_failed
{
    "sequenceId": 629378,
    "data": {
        "symbolId": 100103,
        "symbol": "WBTC_USDC",
        "quantity": 10.0,
        "orderId": 134366348968000,
        "clientOrderId": "@207390481842241",
        "fee": 0,
        "type": "LIMIT",
        "fillPrice": 0.0,
        "createdAt": 1688548917424,
        "unfilledQuantity": 10.0,
        "price": 40000.0,
        "failReason": "ACCOUNT_NO_ENOUGH_AVAILABLE",
        "tradeType": "SPOTS",
        "status": "FAILED",
        "direction": "LONG"
    },
    "type": "order_failed",
    "userId": 10030,
    "timestamp":1688721638247
}

```

Note:
- Authentication Required: Yes

- type: order_pending,order_cancelled,order_filled,order_failed
- The data object will further specify the status for cancellation and filled orders.
- If provided by the user, the clientOrderId will be taken as is. If not provided, the system will generate one.


## Position Change Message

When there is a change in user positions, you will receive a position change message.

```json

{
    "sequenceId":630902,
    "userId":10030,
    "type":"position_changed",
    "data":{
        "symbolId":132,
        "createdAt":1688555522202,
        "symbol":"XBTC",
        "orderId":134421336293440,
        "price":40000,
        "fee":16,
        "feeCurrency":"USDT",
        "type":"OPEN",
        "txHash":"0xb88f84d1b73a04593ea63c08251a190c544ab558302a548ec471b4fa069d544a",
        "quantityChanged":4,
        "direction":"SHORT"
    },
    "timestamp":1688721638247
}

```

Note:
- Authentication Required: Yes


## Liquidation Message

When a user's position is liquidated, you will receive a liquidation message.

```json

{
    "sequenceId":630941,
    "userId":10030,
    "type":"position_liquidated",
    "data":{
        "accountId":0,
        "balances":{
            "USDT":4000
        },
        "contractsPrices":{
            "145":546.004777,
            "132":50000.4375,
            "119":114.000997
        },
        "positions":[
            {
                "symbolId":132,
                "quantity":4,
                "requiredMargin":1600,
                "entryPrice":40000,
                "entryTotalValue":16000,
                "minimumMaintenanceMarginRate":0.005,
                "minimumMaintenanceMargin":80,
                "unrealizedPNL":-4000.175
            }
        ],
        "priceInfos":{
            "100":{
                "price":45714.29,
                "conversionRatio":1
            },
            "101":{
                "price":546,
                "conversionRatio":0.9
            },
            "102":{
                "price":114,
                "conversionRatio":0.8
            },
            "103":{
                "price":1,
                "conversionRatio":1
            }
        },
        "userId":10030,
        "timestamp":1688721638247
    }
}

```

Note:
- Authentication Required: Yes

## Account Asset Change Message

When there is a change in user's assets, you will receive an account asset change message.

```json

{
    "sequenceId":630878,
    "userId":10030,
    "type":"account_changed",
    "data":{
        "accountId":0,
        "createdAt":1688555305545,
        "total":49908,
        "available":45908,
        "currencyId":103,
        "userId":10030,
        "spotsFrozen":4000,
        "sequenceId":630878
    },
    "timestamp":1688721638247
}

```

Note:
- Authentication Required: Yes
