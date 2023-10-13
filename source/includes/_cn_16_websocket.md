

# WebSocket API

WebSocket API是指连接到WebSocket后推送的数据。

## 连接信息

WebSocket只支持ws协议，地址是ws://18.181.46.207:18080/v1/market/notification

## 用户认证

要以认证用户连接wss，必须提供一个有效的wss token

获取wss token的方式是：

以API方式请求wss token，可以访问如下API地址（需要API签名）：

用户API请求：GET [/v1/users/wss/token](http://18.181.46.207:28080/v1/users/wss/token)


```
如果用户未登录，返回结果如下:

{
  "code": 1003,
  "msg": "AUTH_SIGNIN_REQUIRED",
  "data": null
}

如果用户已登录，返回包含Token的JSON：
{
  "code": 200,
  "msg": "success",
  "data": {
    "result": "ZVBkOHFYMDAwMDAyNzJlMTg5MjQxMjAxM2ZjZWQzYmRhYTUxNjM2OGQ4YjI4NjU5NDhjZjA3ZDQ3MjMyYWE5MDNlYTBkNTRlMWMxYTVhZGNlOGQ0YmI0YTA5"
  }
}

```
此Token默认有效期1分钟。

## 用户认证连接

将获取的token作为参数附加到wss连接

```
ws://13.230.140.54:18080/v1/market/notification?token=TlZsTWdwMDAwMDAyNzJlMTg4YzI3YjdmZDA5MmRkODY0MjE2YjNmYTU4YWMxY2EwZTdhMjRjY2IzMzNkMjMzMmViMzAxYzE1Njg2ZWFmNjAwYTNhNTdlOTgw

如果WSS服务器验证用户成功，推送消息

{
  "status": "connected",
  "message": "connected as signed user",
  "userId": 10030
}

如果WSS服务器验证用户失败，推送的消息不含userId

{
  "status": "connected",
  "message": "connected as anonymous user"
}

```
认证成功的WSS连接会推送用户的订单成交、取消信息等用户相关信息。

## 订阅

连接WSS后，需要订阅指定symbol，发送消息如下：

```json
{
  "action":"subscribe",
  "symbol":"BTC_USDT"   // 订阅指定的币对
}
```

返回格式：

```json
{
  "status": "subscribed"  // 该币对的所有信息都会被订阅
}
```

订阅后会推送的type如下

1. orderbook 
2. tick
3. bar : 支持kline {MIN,MIN5,MIN15,MIN30,HOUR,HOUR4,DAY,WEEK,MONTH}
4. bbo


##  取消订阅

已经订阅的topic可以通过`unsubscribe`的action取消，发送消息如下：

```json
{
    "action":"unsubscribe",
    "symbol":"BTC_USDT"   // 订阅指定的币对
}
```


## 心跳

客户端需要每`15秒`发送ping消息来维持连接心跳，格式如下

```json
{
    "action":"ping"
}
```

服务器响应：

```json
{
  "type":"pong",
  "timestamp":1688721638247
}
```

## Price消息

服务器以固定频率推送市场价格摘要消息如下：

```json
{
  "type": "prices",
  "data": {
    "XBTC": [
      1688458520000, // 时间戳
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

说明：

- 消息类型：price
- 只有24小时内有交易的symbol会出现，没有交易的symbol不会出现
- 数据格式：与Bar数据类型相同，可看作最近24小时的一条K线图（但不同于日K，因为日K的起始时间是0:00）


## Bar信息

服务器会根据实际成交以固定频率推送bar（candle）信息：

```json
{
  "type": "bar",
  "symbol": "BTC_USDT",
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
说明：

- 消息类型：bar
- 数据格式：[timestamp, open, high, low, close, volume,amount]

## OrderBook消息

服务器会根据用户挂单及成交信息增量推送,用户需要通过restApi请求orderBook并本地维护

OrderBook消息如下：

```json
{
  "type": "orderbook",
  "symbol": "BTC_USDT",
  "lastSequenceId": 629050,
  "sequenceId": 629112,
  "data": {
    "sellOrders": {},
    "direction": "LONG",
    "symbol": "BTC_USDT",
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

说明：

- 消息类型：orderbook
- 是否需要订阅：需要
- 深度数据格式：`[price, amount]`

## BBO消息

服务器会根据系统订单薄发生变化时固定频率推送bbo数据

BBO消息如下：

```json
{
  "type": "bbo",
  "symbol": "BTC_USDT",
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

说明：

- 消息类型：bbo
- 是否需要订阅：需要

## Tick消息

服务器不定期推送最新成交的Tick消息如下：

```json
{
  "type": "tick",
  "symbol": "BTC_USDT",
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

说明：

- 消息类型：tick
- 是否需要订阅：需要
- 一个tick消息包含一个或多个成交信息
- tick数据格式：`[timestamp, dir, price, volume ,amount, flag]`
  - timestamp: 时间戳
  - dir: 1=主动买入, 0=主动卖出
  - price: 成交价格
  - volume: 成交数量
  - amount: 成交价值
  - flag: 0=普通成交（后续增加爆仓标志）

  
## 订单成交消息

当用户订单成交后，获得订单成交消息：

```json
{
  "sequenceId": 629244,
  "data": {
    "symbolId": 100103,
    "symbol": "BTC_USDT",
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

说明：
- 是否需要认证：需要



## 订单状态变更消息

当用户订单状态发生变动后，获得订单状态变化消息：

```
// order_pending
{
  "sequenceId": 629212,
  "data": {
    "symbolId": 100103,
    "symbol": "BTC_USDT",
    "quantity": 0.1,
    "orderId": 134355368280128,
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
    "symbol": "BTC_USDT",
    "quantity": 0.1,
    "orderId": 134355368280128,
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
        "symbol": "BTC_USDT",
        "quantity": 0.1,
        "orderId": 134357624815680,
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
        "symbol": "BTC_USDT",
        "quantity": 10.0,
        "orderId": 134366348968000,
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

说明：
- 是否需要认证：需要

- type: order_pending,order_cancelled,order_filled,order_failed

- data 对象里面status会细化取消和filled


## 仓位变化消息

当用户仓位发生变动后，获得仓位变化消息

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

说明：
- 是否需要认证：需要


## 爆仓消息

当用户仓位发生爆仓后，获得爆仓消息

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

说明：
- 是否需要认证：需要

## 账户资产变化消息

当用户资产发生改变后，获取账户资产信息

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

说明：
- 是否需要认证：需要


