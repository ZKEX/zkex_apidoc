# 公开市场

公开市场API是指无需登录，即可直接访问的API。

## 获取交易信息

API描述：获取交易所当前所有交易。

API路径：GET [/v1/market/trades](http://54.199.66.35:8080/v1/market/trades)

API请求参数：

- 无

```
API响应样例：
```
| 字段                      | 说明      |
|:------------------------|---------|
| **spotsSymbols**        | 现货交易对信息 |
| **contractsCurrencies** | 合约结算币种  |
| **contractsSymbols**    | 合约交易对信息 | 
| **indexes**             | 价格信息    |
| **spotsCurrencies**     | 现货Currency |
| **riskLimits**          | 合约风险系数信息 |
| **chains**              | 支持的链信息  |
| **currencies**          |  currency基础信息|
| **tokenMappingList**    |  现货和合约与链上Id映射关系|

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "spotsSymbols": [
      {
        "id": 100103,
        "name": "BTC_USDT",
        "startTime": 1680278400000,
        "endTime": 5995814400000,
        "trade": "SPOTS",
        "baseName": "BTC",
        "baseMinimumIncrement": 0.01,
        "baseScale": 2,
        "baseMinimumQuantity": 0.01,
        "baseMaximumQuantity": 10000.0,
        "quoteName": "USDT",
        "quoteMinimumIncrement": 0.5,
        "quoteScale": 1,
        "supportMarginTrade": true,
        "alwaysChargeQuote": false
      }
    ],
    "contractsCurrencies": [
      "USDT"
    ],
    "contractsSymbols": [
      {
        "id": 145,
        "name": "XETH",
        "startTime": 1680278400000,
        "endTime": 5995814400000,
        "trade": "CONTRACTS",
        "type": "PERPETUAL",
        "baseCurrency": "ETH",
        "marginCurrency": "USDT",
        "quoteCurrency": "USDT",
        "multiplier": 0.1,
        "quoteMinimumIncrement": 0.5,
        "quoteScale": 1,
        "baseScale": 0,
        "baseMinimumIncrement": 1,
        "maximumQuantityPerOrder": 100,
        "riskLimit": {
          "id": 106,
          "initialMarginRate": 0.02,
          "maintenanceMarginRate": 0.01,
          "marginRateStep": 0.01,
          "maxLeverage": 5,
          "riskLimitBase": 200,
          "riskLimitStep": 100,
          "maxRiskLimitSteps": 9,
          "createdAt": 1546956031080
        },
        "settlementFeeRate": 0.0,
        "referencedIndexes": {
          "SPOT": 10135,
          "FAIR": 10138,
          "PREMIUM_1M": 10139,
          "PREMIUM_AGGREGATE": 10140,
          "MAX_PREMIUM_RATE": 10137,
          "FUNDING_RATE_1M": 10141,
          "FUNDING_RATE_AGGREGATE": 10142,
          "LENDING_RATE_1D": 10136
        }
      }],                                                                                                                                                                                                                           ],
    "indexes": [
      {
        "id": 10109,
        "name": "BCH",  // 指数价格
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 2
      },
      {
        "id": 10110,
        "name": "BCH_DAILYRATE",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10111,
        "name": "BCH_MAXBCHUSDPI",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10112,
        "name": "BCH_USD_FAIR", // 标记价格
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10113,
        "name": "BCH_USD_PI",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10114,
        "name": "BCH_USD_PI8H",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10115,
        "name": "BCH_FR",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10116,
        "name": "BCH_FR8H",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10117,
        "name": "BCH_INS1H",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10118,
        "name": "BCH_INS1D",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      }
    ],
    "spotsCurrencies": [
      "BTC",
      "ETH",
      "BCH",
      "ARM",
      "USDT"
    ],
    "riskLimits": [
      {
        "id": 105,
        "initialMarginRate": 0.01,
        "maintenanceMarginRate": 0.005,
        "marginRateStep": 0.005,
        "maxLeverage": 10,
        "riskLimitBase": 200,
        "riskLimitStep": 100,
        "maxRiskLimitSteps": 9,
        "createdAt": 1546956010600
      }
    ],
    "chains": {
      "145": [
        {
          "id": 27,
          "chainTokenId": 145,
          "chainId": 1,
          "address": "0x1f34934e3165b3e5428f6a4d873d2620302c7223",
          "decimals": 18,
          "fastWithdraw": true,
          "isSupport": true,
          "updateTime": 1684493403660
        },
        {
          "id": 28,
          "chainTokenId": 145,
          "chainId": 2,
          "address": "0x8f8b0bfb8458f73249024f22b4cf7b6c0eb60996",
          "decimals": 18,
          "fastWithdraw": true,
          "isSupport": true,
          "updateTime": 1684493403857
        }
      ]
    },
    "tokenMappingList": [
      {
        "id": 1,
        "tokenId": 100,
        "tokenName": "BTC",
        "mappingTokenId": 1,
        "tokenIconUrl": "https://static.zk.link/token/icons/default/btc.svg",
        "tokenType": 0
      },
      {
        "id": 3,
        "tokenId": 132,
        "tokenName": "XBTC",
        "mappingTokenId": 0,
        "tokenIconUrl": "https://static.zk.link/token/icons/default/btc.svg",
        "tokenType": 1
      }
    ],
    "currencies": [
      {
        "id": 100,
        "name": "BTC",
        "main": false,
        "conversionRatio": 1,
        "createdAt": 0,
        "displayName": "Bitcoin"
      }
    ]
  }
}
```

## 获取时间

API描述：获取服务器当前时间，以毫秒为单位。

API路径：GET [/v1/market/timestamp](http://54.199.66.35:8080/v1/market/timestamp)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "timestamp": 1688098283693
  }
}
```

## 获取汇率

API描述：获取当前市场汇率。

API路径：GET [/v1/market/fex](http://54.199.66.35:8080/v1/market/fex)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    // 基准汇率:
    "from": "USD",
    // 转换汇率:
    "tos": [
      "AUD",
      "CAD",
      "CNY",
      "EUR",
      "GBP",
      "HKD",
      "JPY",
      "KRW",
      "RUB",
      "SGD",
      "TWD"
    ],
    // 当前汇率:
    "exchanges": {
      "USD_GBP": 0.79251,
      "USD_CAD": 1.32423,
      "USD_JPY": 144.76,
      "USD_HKD": 7.83533,
      "USD_TWD": 31.104,
      "USD_EUR": 0.9193,
      "USD_RUB": 87.2942,
      "USD_KRW": 1319.57,
      "USD_SGD": 1.35535,
      "USD_AUD": 1.5077,
      "USD_CNY": 7.2543
    }
  }
}
```

## 获取系统错误代码

API描述：获取系统定义的全部错误代码。

API路径：GET [/v1/market/errorCodes](http://54.199.66.35:8080/v1/market/errorCodes)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "ACCOUNT_FREEZE_FAILED": "Account error: freeze failed.",
    "ACCOUNT_NO_ENOUGH_AVAILABLE": "Account error: no enough available.",
    "ACCOUNT_NO_ENOUGH_BALANCE": "Account error: no enough balance."
  }
}
```

## 获取当前系统指数(合约)

API描述：获取当前所有指数的最新值。

API路径：GET [/v1/market/indexes](http://54.199.66.35:8080/v1/market/indexes)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "BTC_USD_PI": [
      "1688098860000", // 时间戳
      "0.000096"  // 价格
    ],
    "BTC_MAXBTCUSDPI": [
      "1546272000000",
      "0.0005"
    ]
  }
}
```

某些指数是价格，
例如`BCH`是币现货加权指数，某些指数是计算的数值。
例如`BCH_USD_PI`是当前币对美元溢价率。
例如`BCH_USD_FAIR`是当前币对合约的标记价格。

## 获取某个指数的最近历史值(合约)

API描述：获取某个指数的最近历史值。

API路径：GET /v1/market/indexes/:index_name

API示例：GET [/v1/market/indexes/ETH_USD_FAIR](http://54.199.66.35:8080/v1/market/indexes/ETH_USD_FAIR)

API请求参数(Path Param)：

| 参数     | 类型     | 说明                                            |
| :------- | -------- |:----------------------------------------------|
| **index_name** | **path** | **必填**<br>指数名称，例如`ETH_USD_FAIR`  |

API请求参数(Request Param)：

| 参数      | 类型     | 说明                                           |
| :-------- | :------- | ---------------------------------------------- |
| **start** | **long** | **选填**<br/>起始时间戳，单位ms                |
| **end**   | **long** | **选填**<br/>结束时间戳，单位ms                |
| **limit** | **long** | **选填**<br/>获取BAR的数量，默认1000，最大1000 |

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      [
        1688099240000,
        "564.05311"
      ]
    ]
  }
}
```

## 获取24小时统计价格

API描述：获取某个交易对的最近24小时统计价格。接口不区分现货、合约。

API路径：GET /v1/market/ticker/:symbolName

API示例：GET [/v1/market/ticker/BTC_USDT](http://54.199.66.35:8080/v1/market/ticker/BTC_USDT)

API请求参数(Path Param)：

| 参数             | 类型     | 说明                                  |
|:---------------| -------- | :------------------------------------ |
| **symbolName** | **path** | **必填**<br>交易对名称,例如`BTC_USDT` |

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "XBTC": [
      1688281000000,
      40000.0,
      40000.0,
      40000.0,
      40000.0,
      0.0,
      0.0
    ]
  }
}
```
说明：

数据格式：

`[timestamp, open, high, low, close, amount, volume]`

`[时间戳，开盘价，最高价，最低价，收盘价，成交量, 成交额]`


## 获取所有统计价格

API描述：获取所有交易对的最近24小时统计价格。接口不区分现货、合约。

API路径：GET /v1/market/allTicker

API示例：GET [/v1/market/allTicker](http://54.199.66.35:8080/v1/market/allTicker)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "XBTC": [
      1688278080000,
      40000.0,
      40000.0,
      40000.0,
      40000.0,
      0.0,
      0.0
    ],
    "XBCH": [
      1688278080000,
      117.5,
      117.5,
      117.5,
      117.5,
      0.0,
      0.0
    ],
    "ETH_USDT": [
      1688278080000,
      1148.0,
      1148.0,
      1148.0,
      1148.0,
      0.0,
      0.0
    ]
  }
}
```



## 获取所有币币交易对统计价格

API描述：获取所有币币交易对的最近24小时统计价格。

API路径：GET /v1/market/spots/allTicker

API示例：GET [/v1/market/spots/allTicker](http://54.199.66.35:8080/v1/market/spots/allTicker)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "ARM_USDT": null,
    "ETH_USDT": [
      1688284240000,
      1148.0,
      1148.0,
      1148.0,
      1148.0,
      0.0,
      0.0
    ],
    "BCH_USDT": [
      1688284240000,
      115.5,
      115.5,
      115.5,
      115.5,
      0.0,
      0.0
    ]
  }
}
```



## 获取所有合约交易对统计价格

API描述：获取所有合约交易对的最近24小时统计价格。

API路径：GET /v1/market/contracts/allTicker

API示例：GET [/v1/market/contracts/allTicker](http://54.199.66.35:8080/v1/market/contracts/allTicker)

API请求参数：

- 无

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "XBTC": [
      1688278390000,
      40000.0,
      40000.0,
      40000.0,
      40000.0,
      0.0,
      0.0
    ]
  }
}
```



## 获取OrderBook

API描述：获取某个交易对的最近OrderBook。

API路径：GET /v1/market/orderBook/:symbol_name

API示例：GET [/v1/market/orderBook/BTC_USDT](http://54.199.66.35:8080/v1/market/orderBook/BTC_USDT)

API请求参数(Path Param)：

| 参数       | 类型     | 说明                                  |
| :--------- | -------- | :------------------------------------ |
| **symbol_name** | **path** | **必填**<br>交易对名称,例如`BTC_USDT` |

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "symbolId": 100103,
    "symbol": "BTC_USDT",
    "price": 40000.0, // 成交价
    "sellOrders": [
      [
        40000.0, // 价格
        2.0 // 数量
      ]
    ],
    "buyOrders": [
      [
        39000.0,
        2.0
      ]
    ],
    "sequenceId": 566295,
    "direction": "SHORT"
  }
}
```

## 获取REST Tick数据（暂不支持）

API描述：获取某个交易对的最近tick信息。接口不区分现货、合约。

API路径：GET /v1/market/ticks/:symbol_name

API示例：GET [/v1/market/ticks/BTC_USDT](http://54.199.66.35:8080/v1/market/ticks/BTC_USDT?limit=10)

API请求参数(Path Param)：

| 参数       | 类型     | 说明                                     |
| :--------- | -------- | :--------------------------------------- |
| **symbol_name** | **path** | **必填**<br>交易对名称,例如`BTC_USDT`    |
| **limit**  | **int**  | **选填**<br>获取tick数量, 1-500，默认200 |

```
API响应样例：
```

```json
{
    "results":[
        {
            "sequenceId":1666486,
            "data":[
                [
                    1596194089219,
                    0,
                    11171.6,
                    0.0119,
                    0
                ],
              	[
                    1596194089219,
                    0,
                    11171.6,
                    0.028,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666477,
            "data":[
                [
                    1596194086281,
                    0,
                    11171.6,
                    0.0239,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666474,
            "data":[
                [
                    1596194085781,
                    0,
                    11171.6,
                    0.0266,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666462,
            "data":[
                [
                    1596194082834,
                    0,
                    11171.6,
                    0.0162,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666460,
            "data":[
                [
                    1596194081031,
                    1,
                    11183,
                    0.0266,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666459,
            "data":[
                [
                    1596194081006,
                    0,
                    11171.6,
                    0.0266,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666432,
            "data":[
                [
                    1596194067487,
                    0,
                    11171.6,
                    0.0137,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666423,
            "data":[
                [
                    1596194065608,
                    0,
                    11172.3,
                    0.0177,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666417,
            "data":[
                [
                    1596194062609,
                    0,
                    11175.2,
                    0.0146,
                    0
                ]
            ]
        },
        {
            "sequenceId":1666411,
            "data":[
                [
                    1596194060078,
                    0,
                    11175.2,
                    0.0124,
                    0
                ]
            ]
        }
    ]
}
```

说明：

tick数据格式：`[timestamp, dir, price, amount, flag]`

| 参数          | 说明                           |
| :------------ | :----------------------------- |
| **timestamp** | 时间戳，单位毫秒               |
| **dir**       | 1=主动买入, 0=主动卖出         |
| **price**     | 成交价格                       |
| **amount**    | 成交量                         |
| **flag**      | 0=普通成交（后续增加爆仓标志） |



## 获取Bar数据

API描述：获取某个交易对的最近Bar数据。接口不区分现货、合约。

API路径：GET /v1/market/bars/:symbol_name/:type

API示例：GET [/v1/market/bars/BTC_USDT/min](http://54.199.66.35:8080/v1/market/bars/BTC_USDT/min)

API请求参数(Path Param)：

|参数| 类型 | 说明 |
|:---|:---|----|
|**symbol**|**string**|**必填**<br>交易对名称,例如`BTC_USDT`|
|**type**|**enum**|**必填**<br>Bar类型，目前支持`MIN`、`MIN5`、`MIN15`、`MIN30`、`HOUR`、`HOUR4`、`DAY`、`WEEK`、`MONTH`几种类型|

API请求参数(Request Param)：

|参数| 类型 | 说明 |
|:---|:---|----|
|**start**|**long**|**选填**<br/>起始时间戳，单位ms|
|**end**|**long**|**选填**<br/>结束时间戳，单位ms|
|**limit**|**long**|**选填**<br/>获取BAR的数量，默认200，最大200|

```
API响应样例：
```

```json
{
  "code": 200,
  "msg": "success",
  "data": {
    "results": [
      [
        1685404800000,
        40000,
        40000,
        40000,
        40000,
        0.3,
        12000
      ],
      [
        1687132800000,
        40000,
        40000,
        40000,
        40000,
        1,
        40000
      ]
    ]
  }
}
```

说明：

bar数据格式：

`[timestamp, open, high, low, close, amount, volume]`

`[时间戳，开盘价，最高价，最低价，收盘价，成交量，成交额]`

结果总是按时间戳升序排序

