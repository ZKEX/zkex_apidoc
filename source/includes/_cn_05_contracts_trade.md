

# 交易

交易API可用于现货与合约交易，包括下单、撤单、查看当前活动订单以及查询历史订单。

交易API必须附带用户的API签名，以便验证用户身份。

## 查询账户余额

API描述：查询用户现货账户余额。

API路径：GET /v2/contracts/accounts

API请求参数：
- 无

```
API响应样例:
```

```json
{
    "BTC": [
        9697, // 现货可用
        0 // 现货冻结
    ],
    "ETH": [
        1E+4,
        0
    ],
    "BCH": [
        9.7E+3,
        0
    ],
    "USDT": [
        111904.291800928840035951,
        638.6656303311284
    ],
    "ARM": [
        9.6E+3,
        0
    ]
}
```



## 查询用户费率

API描述：查询用户合约交易费率(期货、合约)。

API路径：GET /v1/[contracts,spots]/fee/rate

API请求参数(Query Param)：

| 参数       | 类型       | 说明                                                         |
| :--------- | ---------- | :----------------------------------------------------------- |
| **symbol** | **string** | **选填**<br>交易对名称,例如`XBTCUSD_PERP`|


```
API响应样例:
```

```json
{
    "taker": 0.000200000000000000, // taker费率
    "maker": 0.000100000000000000, // maker费率
    "timestamp": 1595297714823 // 时间戳
}
```



## 创建新订单

API描述：创建一个新的订单。

API路径：POST /v2/contracts/orders

API请求参数：JSON Object

| 字段名称           | 字段类型  | 是否必须 | 默认值 | 描述 |
|-------------------|---------|---|--|------|
| symbol            | string  | Y |  | 合约代码，例如"XBTCUSD_PERP" |
| type              | enum    | Y |  | 订单类型，限价单LIMIT与市价单MARKET |
| direction         | enum    | Y |  | 订单方向，LONG与SHORT |
| source            | string  |   | "" | 订单来源标识，例如"WEB", "APP"，字母和数字组合，不超过20个字符 |
| price             | decimal | 仅限价单 |  | 限价单报价 |
| quantity          | long    | Y |  | 订单数量，至少为1 |
| triggerOn         | decimal |   |  | 订单触发价格，如果不填，则立刻执行 |
| triggerDirection  | enum    |   |  | 价格触发方向，LONG与SHORT |
| trailingDistance  | decimal |   |  | TrailingStop订单触发距离，如果不填，则不是TrailingStop |
| fillOrKill        | boolean |   | false | 是否设置FOK订单 |
| immediateOrCancel | boolean |   | false | 是否设置IOC订单 |
| postOnly          | boolean |   | false | 是否设置PostOnly订单 |
| hidden            | boolean |   | false | 是否设置Hidden订单 |
| reduceOnly        | boolean |   | false | 是否设置ReduceOnly订单 |
| clientOrderId | string | N |  | 用户自定义订单ID，可用于查询、撤销订单，24小时内可用 |

请注意：

订单类型如果为LIMIT，则必须填写price；

triggerOn与trailingDistance不能同时填写；

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
    "id":3854532008,
  	"clientOrderId":"clientOrderId",
    "sequenceId":0,
    "type":"LIMIT",
    "status":"PENDING",
    "direction":"LONG",
    "features":0,
    "fillPrice":0,
    "price":6000.0,
    "makerFeeRate":-0.0002,
    "takerFeeRate":0.0007,
    "fee":0,
    "createdAt":1597112074952,
    "updatedAt":1597112074952,
    "triggerDirection":"LONG",
    "triggerOn":0,
    "trailingBasePrice":0,
    "trailingDistance":0,
    "marginCurrencyId":null,
    "quantity":1,
    "unfilledQuantity":1,
    "symbol":"XBTCUSD_PERP",
    "trailing":false
}
```

API错误响应：

- `PARAMETER_INVALID`：参数错误。
- `ORDER_EXCEEDED`：当前活动订单数量超出限制，无法创建新的订单。
- `ORDER_INVALID`：无法创建Market订单或Stop订单，因为当前没有市场价。
- `PRICE_EXCEEDED`：无法创建订单，因为报价超出允许范围。
- `ACCOUNT_NO_ENOUGH_AVAILABLE`：无法创建订单，因为没有足够的可用保证金。

## 查询活动订单

API描述：查询当前用户的所有活跃订单。

API路径：GET /v2/contracts/orders/open

API请求参数：无

```
API响应样例:
```

```json
{
    "results":[
        {
            "id":"893f9e20df579fb248ed349406c82c2d",
            "features":0,
            "price":11898,
            "fee":0.000003361916629248,
            "fillPrice":11897.975,
            "quantity":102,
            "unfilledQuantity":22,
            "makerFeeRate":0.0005,
            "takerFeeRate":0.0005,
            "type":"LIMIT",
            "status":"PARTIAL_FILLED",
            "direction":"LONG",
            "triggerDirection":"LONG",
            "triggerOn":0,
            "trailingBasePrice":0,
            "trailingDistance":0,
            "createdAt":1597112143138,
            "updatedAt":1597112149959,
            "symbol":"XBTCUSD_PERP",
            "trailing":false
        },
        {
            "id":"25f2fd62fdf2a6bf33e9431ba184dd2e",
            "features":0,
            "price":11903,
            "fee":0.000001722254893724,
            "fillPrice":11903,
            "quantity":86,
            "unfilledQuantity":45,
            "makerFeeRate":0.0005,
            "takerFeeRate":0.0005,
            "type":"LIMIT",
            "status":"PARTIAL_FILLED",
            "direction":"SHORT",
            "triggerDirection":"LONG",
            "triggerOn":0,
            "trailingBasePrice":0,
            "trailingDistance":0,
            "createdAt":1597112147859,
            "updatedAt":1597112152051,
            "symbol":"XBTCUSD_PERP",
            "trailing":false
        }
      	... ...
    ]
}
```



## 查询活动订单（通过订单ID）

API描述：通过订单ID查询当前用户活跃订单。

API路径：GET /v2/contracts/orders/open/:order_id

API请求参数(Path Param)：

| 参数         | 类型       | 说明                                                      |
| :----------- | ---------- | :-------------------------------------------------------- |
| **order_id** | **string** | **选填**<br>订单Id,例如"25f2fd62fdf2a6bf33e9431ba184dd2e" |

```
API响应样例:
```

```json

{
  "id":"25f2fd62fdf2a6bf33e9431ba184dd2e",
  "features":0,
  "price":11898,
  "fee":0.000003361916629248,
  "fillPrice":11897.975,
  "quantity":102,
  "unfilledQuantity":22,
  "makerFeeRate":0.0005,
  "takerFeeRate":0.0005,
  "type":"LIMIT",
  "status":"PARTIAL_FILLED",
  "direction":"LONG",
  "triggerDirection":"LONG",
  "triggerOn":0,
  "trailingBasePrice":0,
  "trailingDistance":0,
  "createdAt":1597112143138,
  "updatedAt":1597112149959,
  "symbol":"XBTCUSD_PERP",
  "trailing":false
}
```



## 查询活动订单（通过自定义订单ID）

API描述：通过订单ID查询当前用户活跃订单。

API路径：GET /v2/contracts/orders/open/client/:client_order_id

API请求参数(Path Param)：

| 参数                | 类型       | 说明                                         |
| :------------------ | ---------- | :------------------------------------------- |
| **client_order_id** | **string** | **选填**<br>自定义订单Id,例如"clientOrderId" |

```
API响应样例:
```

```json
{
  "id":"25f2fd62fdf2a6bf33e9431ba184dd2e",
  "clientOrderId": "clientOrderId",
  "features":0,
  "price":11898,
  "fee":0.000003361916629248,
  "fillPrice":11897.975,
  "quantity":102,
  "unfilledQuantity":22,
  "makerFeeRate":0.0005,
  "takerFeeRate":0.0005,
  "type":"LIMIT",
  "status":"PARTIAL_FILLED",
  "direction":"LONG",
  "triggerDirection":"LONG",
  "triggerOn":0,
  "trailingBasePrice":0,
  "trailingDistance":0,
  "createdAt":1597112143138,
  "updatedAt":1597112149959,
  "symbol":"XBTCUSD_PERP",
  "trailing":false
}
```





## 查询订单（通过订单ID）

API描述：通过订单ID查询当前用户订单。

API路径：GET /v2/contracts/orders/:order_id

API请求参数(Path Param)：

| 参数         | 类型       | 说明                                                         |
| :----------- | ---------- | :----------------------------------------------------------- |
| **order_id** | **string** | **选填**<br>自定义订单Id,例如"25f2fd62fdf2a6bf33e9431ba184dd2e" |

```
API响应样例:
```

```json
{
  "id":"25f2fd62fdf2a6bf33e9431ba184dd2e",
  "features":0,
  "price":11898,
  "fee":0.000003361916629248,
  "fillPrice":11897.975,
  "quantity":102,
  "unfilledQuantity":22,
  "makerFeeRate":0.0005,
  "takerFeeRate":0.0005,
  "type":"LIMIT",
  "status":"PARTIAL_FILLED",
  "direction":"LONG",
  "triggerDirection":"LONG",
  "triggerOn":0,
  "trailingBasePrice":0,
  "trailingDistance":0,
  "createdAt":1597112143138,
  "updatedAt":1597112149959,
  "symbol":"XBTCUSD_PERP",
  "trailing":false
}
```



## 查询订单（通过自定义订单ID）

API描述：通过订单ID查询当前用户订单。

API路径：GET /v2/contracts/orders/client/:client_order_id

API请求参数(Path Param)：

| 参数                | 类型       | 说明                                         |
| :------------------ | ---------- | :------------------------------------------- |
| **client_order_id** | **string** | **选填**<br>自定义订单Id,例如"clientOrderId" |

```
API响应样例:
```

```json
{
  "id":"25f2fd62fdf2a6bf33e9431ba184dd2e",
  "clientOrderId": "clientOrderId",
  "features":0,
  "price":11898,
  "fee":0.000003361916629248,
  "fillPrice":11897.975,
  "quantity":102,
  "unfilledQuantity":22,
  "makerFeeRate":0.0005,
  "takerFeeRate":0.0005,
  "type":"LIMIT",
  "status":"PARTIAL_FILLED",
  "direction":"LONG",
  "triggerDirection":"LONG",
  "triggerOn":0,
  "trailingBasePrice":0,
  "trailingDistance":0,
  "createdAt":1597112143138,
  "updatedAt":1597112149959,
  "symbol":"XBTCUSD_PERP",
  "trailing":false
}
```





## 查询历史订单

API描述：查询当前用户的历史订单记录。

API路径：GET /v2/contracts/orders/closed

API请求参数(Request Param)：

| 参数         | 类型       | 说明                                                         |
| :----------- | ---------- | :----------------------------------------------------------- |
| **symbol**   | **string** | **选填**<br>交易对名称，例如`XBTCUSD_PERP`，默认空           |
| **limit**    | **long**   | **选填**<br/>返回结果集的最大记录数量，范围1～100，默认为100 |
| **range**    | **string** | **选填**<br/>查询月份，格式为YYYYMM，例如"201907"，默认为""，表示当前月份 |
| **offsetId** | **long**   | **选填**<br/>传入当前页的起始id，默认为0，表示第一页         |

```
API响应样例:
```

```json
{
    // 订单集所在范围:
    "range":"202008",
    // 是否有下一页:            
    "hasMore":true,
    // 下一页查询的起始ID:            
    "nextOffsetId":3857772008,
    "results":[
        {
            "id":"25f2fd62fdf2a6bf33e9431ba184dd2e",
            "features":8,
            "price":13097.5,
            "fee":0.000001511461919556,
            "fillPrice":11909,
            "quantity":36,
            "unfilledQuantity":0,
            "makerFeeRate":0.0005,
            "takerFeeRate":0.0005,
            "type":"MARKET",
            "status":"FULLY_FILLED",
            "direction":"LONG",
            "triggerDirection":"LONG",
            "triggerOn":0,
            "trailingBasePrice":0,
            "trailingDistance":0,
            "createdAt":1597112483928,
            "updatedAt":1597112483928,
            "symbol":"XBTCUSD_PERP",
            "trailing":false
        },
        {
            "id":"b14de0b92ea48fa41c57109059df2540",
            "features":8,
            "price":10718.5,
            "fee":0.00000151171579743,
            "fillPrice":11907,
            "quantity":36,
            "unfilledQuantity":0,
            "makerFeeRate":0.0005,
            "takerFeeRate":0.0005,
            "type":"MARKET",
            "status":"FULLY_FILLED",
            "direction":"SHORT",
            "triggerDirection":"LONG",
            "triggerOn":0,
            "trailingBasePrice":0,
            "trailingDistance":0,
            "createdAt":1597112483895,
            "updatedAt":1597112483895,
            "symbol":"XBTCUSD_PERP",
            "trailing":false
        }
      	... ...
    ]
}
```

说明：

历史订单总是按页查询，第一页的offsetId传入0或者不传；

如果API响应样例返回的hasMore=true，则可以根据nextOffsetId构造下一页查询的URL。

## 取消订单

API描述：取消一个活动订单。

API路径：GET /v2/contracts/orders/:order_id/cancel

API请求参数(Path Param)：

| 参数         | 类型     | 说明               |
| :----------- | -------- | :----------------- |
| **order_id** | **path** | **必填**<br>订单ID |

```
API响应样例:
```

```json
{
    "id": "b14de0b92ea48fa41c57109059df2540",
    "sequenceId": 0,
    "type": "LIMIT",
    "status": "FULLY_CANCELLED",
    "direction": "SHORT",
    "features": 0,
    "fillPrice": 0.0,
    "price": null,
    "makerFeeRate": 0.0005,
    "takerFeeRate": 0.0005,
    "fee": 0,
    "createdAt": 1597112497928,
    "updatedAt": 1597112615906,
    "triggerDirection": "LONG",
    "triggerOn": 0,
    "trailingBasePrice": 0,
    "trailingDistance": 0,
    "marginCurrencyId": null,
    "quantity": 52,
    "unfilledQuantity": 52,
    "symbol": "XBTCUSD_PERP",
    "trailing": false
}
```



## 取消订单（通过自定义订单ID）

API描述：通过自定义订单ID取消一个活动订单。

API路径：GET /v2/contracts/orders/client/:client_order_id/cancel

API请求参数(Path Param)：

| 参数                | 类型     | 说明                     |
| :------------------ | -------- | :----------------------- |
| **client_order_id** | **path** | **必填**<br>自定义订单ID |

```
API响应样例:
```

```json
{
    "id": "b14de0b92ea48fa41c57109059df2540",
  	"clientOrderId": "clientOrderId",
    "sequenceId": 0,
    "type": "LIMIT",
    "status": "FULLY_CANCELLED",
    "direction": "SHORT",
    "features": 0,
    "fillPrice": 0.0,
    "price": null,
    "makerFeeRate": 0.0005,
    "takerFeeRate": 0.0005,
    "fee": 0,
    "createdAt": 1597112497928,
    "updatedAt": 1597112615906,
    "triggerDirection": "LONG",
    "triggerOn": 0,
    "trailingBasePrice": 0,
    "trailingDistance": 0,
    "marginCurrencyId": null,
    "quantity": 52,
    "unfilledQuantity": 52,
    "symbol": "XBTCUSD_PERP",
    "trailing": false
}
```



## 取消所有订单

API描述：取消所有活动订单。

API路径: POST /v2/contracts/orders/cancel

API请求参数(Request Json Body)：

```json
{
    "symbol":"XBTCUSD_PERP"
}
```

| 参数       | 类型       | 说明                                                         |
| :--------- | ---------- | :----------------------------------------------------------- |
| **symbol** | **string** | **选填**<br>交易对名称,例如`XBTCUSD_PERP`，为空则撤销所有交易对活动订单 |

```
API响应样例:
```

```json
{ 
  "cancelled" : 10 // 成功取消的订单数量  
}
```

## 查询订单成交详情

API描述：查询当前用户的某个订单成交详情。

API路径：GET /v2/contracts/orders/:order_id/matches

API请求参数(Path Param)：

| 参数         | 类型       | 说明               |
| :----------- | ---------- | :----------------- |
| **order_id** | **string** | **必填**<br>订单ID |

API响应样例：订单成交详细信息，按时间排序

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

-如果一个订单的成交记录超过1000条，则最多返回前1000条记录。

## 查询当前活动仓位列表

API描述：获取当前活动仓位列表。

API路径：GET /v2/contracts/positions

API请求参数：无

```
API响应样例:
```

```json
{
    "results":[
        {
            "id":"122019_14",
            "leverage":0,
            "riskLevel":0,
            "maxQuantity":100000,
            "margin":0.000279798244065283,
            "realizedPNL":-0.000869129308091017,
            "takerFeeRate":0.0005,
            "symbol":"XBTCUSD_PERP",
            "bankruptcyPrice":0,
            "liquidationPrice":0,
            "updatedAt":1597116449876,
            "direction":"SHORT",
            "quantity":555,
            "entryPrice":11866.877971329317,
            "minimumMaintenanceMarginRate":0.005,
            "closed":false
        }
      ...
    ]
}
```

## 查询仓位清算列表

API描述：获取仓位清算列表。

API路径：GET /v2/contracts/clearings/positions

API请求参数(Request Param)：

| 参数         | 类型       | 说明                                                         |
| :----------- | ---------- | :----------------------------------------------------------- |
| **symbol**   | **string** | **选填**<br>交易对名称，例如`XBTCUSD_PERP`，默认空           |
| **limit**    | **long**   | **选填**<br/>返回结果集的最大记录数量，范围1～100，默认为100 |
| **range**    | **string** | **选填**<br/>查询月份，格式为YYYYMM，例如"201907"，默认为""，表示当前月份 |
| **offsetId** | **long**   | **选填**<br/>传入当前页的起始id，默认为0，表示第一页         |

```
API响应样例:
```

```json
{
    "range": "202007",
    "hasMore": true,
    "nextOffsetId": 35905,
    "results": [
        {
            "id": 35925,
            "orderId": "b14de0b92ea48fa41c57109059df2540",
            "symbol": "XBTCUSD_PERP",
            "sequenceId": 121577,
            "direction": "SHORT",
            "type": "OPEN",
            "clearingPrice": 9178.000000000000000000,
            "rate": 0.001000000000000000,
            "fee": 0.000005229897581172,
            "quantityChanged": 48,
            "quantityAfterClearing": 48,
            "realizedPNLChanged": -0.000005229897581172,
            "positionMargin": 0.000047069078230551,
            "createdAt": 1594711903062
        },
        {
            "id": 35920,
            "orderId": "25f2fd62fdf2a6bf33e9431ba184dd2e",
            "symbol": "XBTCUSD_PERP",
            "sequenceId": 121576,
            "direction": "LONG",
            "type": "CLOSE",
            "clearingPrice": 9170.000000000000000000,
            "rate": 0.002000000000000000,
            "fee": 0.000007415485278080,
            "quantityChanged": -34,
            "quantityAfterClearing": 0,
            "realizedPNLChanged": -0.000007415485278080,
            "positionMargin": 0E-18,
            "createdAt": 1594711902592
        }
      	... ...
    ]
}
```
## 设置仓位杠杠

API描述：为指定仓位设置杠杠倍数。如果设置成功，仓位保证金会随之调整。

API路径：POST /v2/contracts/positions/:symbol/leverage

API请求参数(Path Param)：

| 参数       | 类型       | 说明                                      |
| :--------- | ---------- | :---------------------------------------- |
| **symbol** | **string** | **必填**<br>交易对名称,例如`XBTCUSD_PERP` |

API请求参数(Request Json Body)：

```json
{
    "leverage": 10 // leverage: integer, 必填，0～最大允许值，0=全仓，1～最大允许值=逐仓杠杠倍数。
}
```

```
API响应样例:
```

```json
{
    "id":"122019_14",
    "direction":"SHORT",
    "updatedAt":1597137167156,
    "quantity":36,
    "leverage":10,
    "riskLevel":0,
    "maxQuantity":100000,
    "realizedPNL":-0.000026873193329901,
    "takerFeeRate":0.0005,
    "margin":0.000154997374707182,
    "bankruptcyPrice":12386.641916798311,
    "liquidationPrice":12324.677725118481,
    "entryPrice":11765.086830804228,
    "symbol":"XBTCUSD_PERP",
    "closed":false,
    "minimumMaintenanceMarginRate":0.005
}
```

API错误响应：

- `PARAMETER_INVALID`：无效的leverage参数：null，负数或高于该Symbol允许的最大杠杆倍数。
- `ACCOUNT_NO_ENOUGH_AVAILABLE`: 没有足够的可用保证金。

## 设置仓位保证金

API描述：为指定仓位设置新的保证金额度，仅限逐仓仓位。为全仓仓位设置保证金将返回错误。

API路径：POST /v2/contracts/positions/:symbol/margin

API请求参数(Path Param)：

| 参数       | 类型       | 说明                                      |
| :--------- | ---------- | :---------------------------------------- |
| **symbol** | **string** | **必填**<br>交易对名称,例如`XBTCUSD_PERP` |

- API请求参数(Request Json Body)：

```json
{
    "margin": 1.05
}
```

| 参数       | 类型        | 说明                             |
| :--------- | ----------- | :------------------------------- |
| **margin** | **decimal** | **必填**<br>设置的新的仓位保证金 |

```
API响应样例:
```

```json
{
    "id":"122019_14",
    "direction":"SHORT",
    "updatedAt":1597137642109,
    "quantity":36,
    "leverage":20,
    "riskLevel":0,
    "maxQuantity":100000,
    "realizedPNL":-0.000010729975524031,
    "takerFeeRate":0.0005,
    "margin":1.05,
    "bankruptcyPrice":999999,
    "liquidationPrice":999999,
    "entryPrice":11756.02490016114,
    "symbol":"XBTCUSD_PERP",
    "closed":false,
    "minimumMaintenanceMarginRate":0.005
}
```

API错误响应：

- `PARAMETER_INVALID`：无效的保证金额度（null、0或负数）。
- `POSITION_NOT_FOUND`：未找到活动仓位。
- `POSITION_CHANGE_FAILED`: 不能调整全仓仓位的保证金。
- `ACCOUNT_NO_ENOUGH_AVAILABLE`: 没有足够的可用保证金。

## 设置仓位风险等级

API描述：为指定仓位设置风险等级。如果设置成功，继续增加仓位时最小维持保证金率可能会增加。

API路径：POST /v2/contracts/positions/:symbol/riskLevel

API请求参数：

| 参数       | 类型       | 说明                                      |
| :--------- | ---------- | :---------------------------------------- |
| **symbol** | **string** | **必填**<br>交易对名称,例如`XBTCUSD_PERP` |

- POST参数：JSON Object：

```json
{
    "riskLevel": 1
}
```

| 参数          | 类型        | 说明                      |
| :------------ | ----------- | :------------------------ |
| **riskLevel** | **integer** | **必填**<br>0～最大允许值 |

```
API响应样例:
```

```json
{
    "id":"122019_14",
    "direction":"SHORT",
    "updatedAt":1597137704240,
    "quantity":36,
    "leverage":20,
    "riskLevel":1,
    "maxQuantity":150000,
    "realizedPNL":-0.000001531654186521,
    "takerFeeRate":0.0005,
    "margin":0.000151633764465623,
    "bankruptcyPrice":12357.836927932667,
    "liquidationPrice":12296.016833245661,
    "entryPrice":11752,
    "symbol":"XBTCUSD_PERP",
    "closed":false,
    "minimumMaintenanceMarginRate":0.005
}
```

API错误响应：

- `PARAMETER_INVALID`：无效的riskLevel参数：null，负数或高于该Symbol允许的最大风险等级。
- `ACCOUNT_NO_ENOUGH_AVAILABLE`: 没有足够的可用保证金。



## 合约账户划转到资产中心

API描述：从合约账户划转资产到资产中心

API路径：POST /v2/contracts/transfer/out/request

API请求参数：JSON Object

| 参数             | 类型        | 说明                               |
| :--------------- | ----------- | :--------------------------------- |
| **currency**     | **string**  | **必填**<br/>currency名称，如“BTC” |
| **amount**       | **decimal** | **必填**<br/>划转数量              |
| **transferFrom** | **string**  | **必填**<br/>"CONTRACTS"           |
| **transferTo**   | **string**  | **必填**<br/>"WALLET"              |

返回：

```json
{
    "transferFrom": "CONTRACTS",
    "transferTo": "WALLET",
    "amount": 0.001,
    "transferId": "CT0001b881ea800042",
    "currencyName": "BTC",
    "currencyId": 100
}
```

