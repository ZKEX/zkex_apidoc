

# WebSocket API

WebSocket API是指连接到WebSocket后推送的数据。

## 连接信息

WebSocket只支持wss协议，地址是wss://uniwss.876ex.com/v2/market/notification

## 用户认证

要以认证用户连接wss，必须提供一个有效的wss token。获取wss token的方式是：

以API方式请求wss token，可以访问如下API地址（需要API签名）：

用户API请求：GET https://uniapi.876ex.com/v2/users/notification/token

如果用户未登录，返回400错误，如果用户已登录，返回包含Token的JSON如下：

```json
{
    "result": "d3p0ZGZFMDAwMuD"
}
```

此Token默认有效期1分钟。

2. 立刻连接WSS：

将获取的token作为参数附加到wss连接：

```
wss://uniwss.876ex.com/v2/market/notification?token=d3p0ZGZFMDAwMuD
```

WSS连接成功后，服务器会立刻推送一条status信息。如果WSS服务器验证用户成功，推送消息如下：

```json
{
    "type":"CONNECTED",
    "message":"connected as signed user"
}
```



如果WSS服务器验证用户失败，推送的消息不含userId：

```json
{
    "type":"CONNECTED",
    "message":"connected as anonymous user"
}
```



3. 认证成功的WSS连接会推送用户的订单成交、取消信息等用户相关信息。

## 订阅

连接WSS后，需要订阅指定topic，发送消息如下：

```javascript
socket.send(JSON.stringify({
    action: "subscribe",
    topic: "tick.BTC_USDT"
});
```

返回格式：

```json
{
    "type":"TOPICS",
    "data":[ "tick.btc_usdt"] // 所有已经订阅的topic
}
```

目前支持的topic

1. ORDERBOOK.{symbol_name}
2. TICK.{symbol_name}
3. TICKER.{symbol_name}
4. ALL-TICKER
5. BAR.{MIN,MIN5,MIN15,MIN30,HOUR,HOUR4,DAY,WEEK,MONTH}.{symbol_name}
6. ORDER_STATUS_CHANGED
7. ORDER_MATCHED
8. ACCOUNT_CHANGED
9. POSITION_CHANGED
10. INDEX.{index_name}

目前服务端返回消息的type list

1. CONNECTED
2. ORDERBOOK
3. TICK
4. TICKER
5. ALL-TICKER
6. BAR
7. ORDER_STATUS_CHANGED
8. ORDER_MATCHED
9. ACCOUNT_CHANGED
10. POSITION_CHANGED
11. INDEX

##  取消订阅

已经订阅的topic可以通过`unsubscribe`的action取消，发送消息如下：

```javascript
socket.send(JSON.stringify({
    action: "unsubscribe",
    topic: "tick.BTC_USDT"
});
```

支持的topic与订阅相同

## 接收消息

服务器向客户端推送的消息均为JSON文本，格式如下：

```json
{
    "type": "message-type", // 消息类型
    "sequenceId": 12345, // 序列ID
    "symbol": "BTC_USDT", // 可选，仅限已订阅的symbol
    "userId": 34567, // 可选，仅限针对单用户的消息，例如order_filled
    "resolution": "MIN", // 可选，仅限针对bar类型的消息，表示bar的类型
    "data": ... // 消息其他字段
}
```

JavaScript代码如下：

```javascript
socket.onmessage = function (event) {
    var message = JSON.parse(event.data);
    if (message.type === "orderbook") {
        processOrderBook(message);
    } else if (message.type === "bar") {
        processBar(message);
    } else ...
};
```



## 心跳

客户端需要每`15秒`发送ping消息来维持连接心跳，格式如下

```json
{
    "action":"ping",
    "ts":1595844075067 // client端时间戳, 单位ms
}
```

服务器响应：

```json
{
    "type":"PONG",
    "ts":1595844075403, // 服务器端时间戳
    "gap":336 // 服务器时间与client时间的差值，可以用来判断连接网络状态
}
```



## Price消息

服务器以固定频率推送市场价格摘要消息如下：

```json
{
    "type" : "PRICE",
    "symbol" : "BTC_USDT",
    "data" : [1576656730000, 7243.67, 7966.01, 7201.9, 7226.5, 450.3]
}
```

说明：

- 消息类型：price
- 只有24小时内有交易的symbol会出现，没有交易的symbol不会出现
- 数据格式：与Bar数据类型相同，可看作最近24小时的一条K线图（但不同于日K，因为日K的起始时间是0:00）



## ALL-TICKER消息

服务器以固定频率推送全交易对市场价格摘要。首次订阅将返回所有交易对的价格摘要，后续会只推送有数据更新的交易对价格摘要。

订阅：

```json
{
    "action":"subscribe",
    "topic":"ALL-TICKER"
}
```

消息格式：

```json
{
    "type":"ALL-TICKER",
    "data":[
        {
            "type":"PRICE",
            "symbol":"BTC_USDT",
            "data":[
                1596445560000,
                11299.5,
                12111.9,
                10940,
                11223.2,
                2052.8359
            ],
            "sequenceId":"2268111",
            "ts":1596445607162
        },
        {
            "type":"PRICE",
            "symbol":"ETH_USDT",
            "data":[
                1596445560000,
                376.9,
                388.3,
                354.5,
                382.9,
                2196.89
            ],
            "sequenceId":"2268112",
            "ts":1596445607746
        }
    ]
}
```



## Websocket Bar信息

服务器会根据实际成交以最低0.5s的速度推送bar（candle）信息：

```json
{
    "type" : "BAR",
    "symbol" : "BTC_USDT",
    "resolution" : "MIN",
    "sequenceId" : 24918,
    "data" : [1594973040000,9100.8,9109.4,9099.7,9109.4,0.2004]
}
```
说明：

- 消息类型：bar
- 数据格式：[1594973040000,9100.8,9109.4,9099.7,9109.4,0.2004] --- [timestamp, open, high, low, close, volume]

## Websocket OrderBook消息

服务器以固定频率推送OrderBook消息如下：

```json
{
    "type": "ORDERBOOK",
    "symbol": "BTC_USDT",
    "sequenceId": 1130,
    "data": {
        "price": 6705.5,
        "buyOrders": [
            [6704.5, 0.9],
            [6704, 1.1],
            [6703.5, 1.0],
            [6702.5, 0.3]
        ],
        "sellOrders": [
            [6705.5, 0.9],
            [6706, 1.3],
            [6706.5, 0.1]
        }
    }
}
```

说明：

- 消息类型：orderbook
- 是否需要订阅：需要
- 深度数据格式：`[price, amount]`

## Websocket Tick消息

服务器不定期推送最新成交的Tick消息如下：

```json
{
    "type": "TICK",
    "symbol": "BTC_USDT",
    "sequenceId": 1131,
    "data": [
        [1576743711096, 1, 7200.4, 0.11, 0],
        [1576743711096, 1, 7200.5, 0.24, 0],
    ]
}
```

说明：

- 消息类型：tick
- 是否需要订阅：需要
- 一个tick消息包含一个或多个成交信息
- tick数据格式：`[timestamp, dir, price, amount, flag]`
  - timestamp: 时间戳
  - dir: 1=主动买入, 0=主动卖出
  - price: 成交价格
  - amount: 成交数量
  - flag: 0=普通成交（后续增加爆仓标志）



## Websocket Index消息

服务器会定期推送Index消息

订阅：

```json
{
    "action":"subscribe",
    "topic":"INDEX.BBTCUSD"
}
```

消息格式:

```json
{
    "type":"INDEX",
    "indexName":"BBTCUSD",
    "data":[
        1596620650000, // 时间戳
        11373.57 // 指数值
    ]
}
```



## 订单成交消息

当用户订单成交后，获得订单成交消息：

```json
{
    "type":"ORDER_MATCHED",
    "sequenceId":627926,
    "data":{
        "symbol":"BTC_USDT",
        "orderId":6279262007,
        "price":9514.7,
        "matchType":"TAKER",
        "fee":0.055090113,
        "matchedQuantity":0.0193,
        "direction":"SHORT"
    },
    "userId":123
}
```

说明：

- 消息类型：`ORDER_MATCHED`
- 是否需要认证：需要



## 订单状态变更消息

当用户订单状态发生变动后，获得订单状态变化消息：

```json
{
    "userId":123,
    "type":"ORDER_STATUS_CHANGED",
    "data":{
        "symbol":"BTC_USDT",
        "quantity":0.8443,
        "unfilledQuantity":0.3315,
        "orderId":6290602007,
        "price":9509.8,
        "fee":-0.00005128,
        "type":"LIMIT",
        "fillPrice":9509.8,
        "direction":"LONG",
        "status":"PARTIAL_FILLED"
    },
    "sequenceId":629078
}
```

说明：

- 消息类型：ORDER_STATUS_CHANGED`
- 是否需要认证：需要

