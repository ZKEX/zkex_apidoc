# 公开市场

公开市场API是指无需登录，即可直接访问的API。

## 获取交易信息

API描述：获取交易所当前所有交易。

API路径：GET [/v2/market/meta](https://defiapi.876ex.com/v2/market/meta)

API请求参数：

- 无

```
API响应样例：
```

```json
{
    // 系统支持的所有币种:
    "currencies":[
        {
            "id":100,
            "name":"BTC",
            "displayScale":7,
            "createdAt":0
        },
        {
            "id":101,
            "name":"ETH",
            "displayScale":3,
            "createdAt":0
        },
        {
            "id":102,
            "name":"LTC",
            "displayScale":7,
            "createdAt":1594957517343
        },
        {
            "id":103,
            "name":"BCH",
            "displayScale":3,
            "createdAt":0
        },
        {
            "id":105,
            "name":"USDT",
            "displayScale":7,
            "createdAt":0
        },
        {
            "id":106,
            "name":"USDS",
            "displayScale":7,
            "createdAt":1594957305036
        },
        {
            "id":107,
            "name":"EOS",
            "displayScale":4,
            "createdAt":1594957495878
        },
        {
            "id":701,
            "name":"ARM",
            "displayScale":2,
            "createdAt":0
        }
    ],
    // 可交易的所有现货币种:
    "spotsCurrencies":[
        "LTC",
        "USDS",
        "USDT",
        "BTC",
        "EOS",
        "ETH"
    ],
    // 可交易的所有期货合约币种:
    "contractsCurrencies":[
        "BTC"
    ],
    // 可交易的现货信息:
    "spotsSymbols":[
        {
            "id":100105,
            "name":"BTC_USDT",
            "startTime":0,
            "endTime":5000000000000,
            "baseName":"BTC",
            "baseMinimumIncrement":0.0001,
            "baseScale":4,
            "baseMinimumQuantity":0.0001,
            "baseMaximumQuantity":100,
            "quoteName":"USDT",
            "quoteMinimumIncrement":0.1,
            "quoteScale":1,
            "supportMarginTrade":false,
            "alwaysChargeQuote":false
        },
        {
            "id":101105,
            "name":"ETH_USDT",
            "startTime":0,
            "endTime":5000000000000,
            "baseName":"ETH",
            "baseMinimumIncrement":0.001,
            "baseScale":3,
            "baseMinimumQuantity":0.001,
            "baseMaximumQuantity":10000,
            "quoteName":"USDT",
            "quoteMinimumIncrement":0.1,
            "quoteScale":1,
            "supportMarginTrade":false,
            "alwaysChargeQuote":false
        },
        {
            "id":102105,
            "name":"LTC_USDT",
            "startTime":1594972800000,
            "endTime":5000000000000,
            "baseName":"LTC",
            "baseMinimumIncrement":0.01,
            "baseScale":2,
            "baseMinimumQuantity":0.01,
            "baseMaximumQuantity":100,
            "quoteName":"USDT",
            "quoteMinimumIncrement":0.1,
            "quoteScale":1,
            "supportMarginTrade":false,
            "alwaysChargeQuote":false
        },
        {
            "id":107105,
            "name":"EOS_USDT",
            "startTime":1594972800000,
            "endTime":5000000000000,
            "baseName":"EOS",
            "baseMinimumIncrement":0.1,
            "baseScale":1,
            "baseMinimumQuantity":0.1,
            "baseMaximumQuantity":100,
            "quoteName":"USDT",
            "quoteMinimumIncrement":0.1,
            "quoteScale":1,
            "supportMarginTrade":false,
            "alwaysChargeQuote":false
        },
        {
            "id":101100,
            "name":"ETH_BTC",
            "startTime":1594972800000,
            "endTime":5000000000000,
            "baseName":"ETH",
            "baseMinimumIncrement":0.01,
            "baseScale":2,
            "baseMinimumQuantity":0.01,
            "baseMaximumQuantity":100,
            "quoteName":"BTC",
            "quoteMinimumIncrement":0.00001,
            "quoteScale":5,
            "supportMarginTrade":false,
            "alwaysChargeQuote":false
        },
        {
            "id":102100,
            "name":"LTC_BTC",
            "startTime":1594972800000,
            "endTime":5000000000000,
            "baseName":"LTC",
            "baseMinimumIncrement":0.001,
            "baseScale":3,
            "baseMinimumQuantity":0.001,
            "baseMaximumQuantity":100,
            "quoteName":"BTC",
            "quoteMinimumIncrement":0.00001,
            "quoteScale":5,
            "supportMarginTrade":false,
            "alwaysChargeQuote":false
        },
        {
            "id":100106,
            "name":"BTC_USDS",
            "startTime":1594972800000,
            "endTime":5000000000000,
            "baseName":"BTC",
            "baseMinimumIncrement":0.0001,
            "baseScale":4,
            "baseMinimumQuantity":0.0001,
            "baseMaximumQuantity":100,
            "quoteName":"USDS",
            "quoteMinimumIncrement":0.1,
            "quoteScale":1,
            "supportMarginTrade":false,
            "alwaysChargeQuote":false
        },
        {
            "id":101106,
            "name":"ETH_USDS",
            "startTime":1594972800000,
            "endTime":5000000000000,
            "baseName":"ETH",
            "baseMinimumIncrement":0.01,
            "baseScale":2,
            "baseMinimumQuantity":0.01,
            "baseMaximumQuantity":100,
            "quoteName":"USDS",
            "quoteMinimumIncrement":0.1,
            "quoteScale":1,
            "supportMarginTrade":false,
            "alwaysChargeQuote":false
        }
    ],
    // 可交易的合约信息:
    "contractsSymbols":[
        {
            "id":14,
            "name":"XBTCUSD_PERP",
            "startTime":1546300800000,
            "endTime":5000000000000,
            "type":"PERPETUAL",
            "marginCurrency":"BTC",
            "inverse":true,
            "liquidateBy":"INDEX_PRICE",
            "multiplier":1,
            "minimumPriceIncrement":0.5,
            "priceStep":5,
            "priceScale":1,
            "maximumQuantityPerOrder":10000,
            "riskLimit":{
                "id":16,
                "initialMarginRate":0.01,
                "maintenanceMarginRateStep":0.005,
                "maxLeverage":100,
                "riskLimitBase":100000,
                "riskLimitStep":50000,
                "maxRiskLimitSteps":9,
                "createdAt":1546956010600
            },
            "settlementFeeRate":0,
            "referencedIndexes":{
                "SPOT":10006,
                "FAIR":10009,
                "PREMIUM_1M":10010,
                "PREMIUM_AGGREGATE":10011,
                "MAX_PREMIUM_RATE":10008,
                "FUNDING_RATE_1M":10012,
                "FUNDING_RATE_AGGREGATE":10013,
                "LENDING_RATE_1D":10007
            }
        }
    ],
    // 系统使用的指数信息:
    "indexes":[
        {
            "id":10006,
            "name":"BBTCUSD",
            "startTime":0,
            "endTime":5000000000000,
            "scale":2
        },
        {
            "id":10007,
            "name":"XBTCUSDDAILYRATE",
            "startTime":0,
            "endTime":5000000000000,
            "scale":6
        },
        {
            "id":10008,
            "name":"XBTCUSDMAXPIRATE",
            "startTime":0,
            "endTime":5000000000000,
            "scale":6
        },
        {
            "id":10009,
            "name":"XBTCUSDFAIR",
            "startTime":0,
            "endTime":5000000000000,
            "scale":6
        },
        {
            "id":10010,
            "name":"XBTCUSDPI",
            "startTime":0,
            "endTime":5000000000000,
            "scale":6
        },
        {
            "id":10011,
            "name":"XBTCUSDPI8H",
            "startTime":0,
            "endTime":5000000000000,
            "scale":6
        },
        {
            "id":10012,
            "name":"XBTCUSDFR",
            "startTime":0,
            "endTime":5000000000000,
            "scale":6
        },
        {
            "id":10013,
            "name":"XBTCUSDFR8H",
            "startTime":0,
            "endTime":5000000000000,
            "scale":6
        },
        {
            "id":10014,
            "name":"BTCINS1H",
            "startTime":0,
            "endTime":5000000000000,
            "scale":6
        },
        {
            "id":10015,
            "name":"BTCINS1D",
            "startTime":0,
            "endTime":5000000000000,
            "scale":6
        }
    ],
    // 系统使用的风险等级信息:
    "riskLimits":[
        {
            "id":16,
            "initialMarginRate":0.01,
            "maintenanceMarginRateStep":0.005,
            "maxLeverage":100,
            "riskLimitBase":100000,
            "riskLimitStep":50000,
            "maxRiskLimitSteps":9,
            "createdAt":1546956010600
        }
    ]
}
```

## 获取时间

API描述：获取服务器当前时间，以毫秒为单位。

API路径：GET [/v2/market/timestamp](https://defiapi.876ex.com/v2/market/timestamp)

API请求参数：

- 无

```
API响应样例：
```

```json
{
    // 系统当前时间，以毫秒为单位:
    "timestamp": 1563326294936
}
```

## 获取汇率

API描述：获取当前市场汇率。

API路径：GET [/v2/market/fex](https://defiapi.876ex.com/v2/market/fex)

API请求参数：

- 无

```
API响应样例：
```

```json
{
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
        "USD_GBP":0.78886,
        "USD_CAD":1.35191,
        "USD_JPY":107.206,
        "USD_HKD":7.75152,
        "USD_TWD":29.423,
        "USD_EUR":0.8726,
        "USD_RUB":71.3516,
        "USD_KRW":1197.88,
        "USD_SGD":1.38853,
        "USD_AUD":1.42376,
        "USD_CNY":6.9836
    }
}
```

## 获取系统错误代码

API描述：获取系统定义的全部错误代码。

API路径：GET [/v2/market/errorCodes](https://defiapi.876ex.com/v2/market/errorCodes)

API请求参数：

- 无

```
API响应样例：
```

```json
{
    // "错误代码常量": {"语言"："错误描述模版..."}
    "ACCOUNT_NO_ENOUGH_AVAILABLE": {
        "cn": "{0} 账户缺少足够的余额",
        "en": "Account error: {{0}} has no enough available"
    },
    "ACCOUNT_NO_ENOUGH_BALANCE": {
        "cn": "余额不足",
        "en": "Account error: no enough balance"
    },
    "ACCOUNT_TRANSFER_FAILED": {
        "cn": "转账失败",
        "en": "Account error: transfer failed"
    },
    ...
}
```

## 返回错误响应

API描述：返回一个错误响应，以便调试客户端错误处理代码。

API路径：GET [/v2/market/error](https://defiapi.876ex.com/v2/market/error)

API请求参数：

- 无

```
API响应样例：
```

```json
{
    // 标准错误响应:
    "error": "TEST_API_ERROR",
    "data": {
        "0": "Hello"
    },
    "message": "Hello, this is a test api message"
}
```

## 获取当前系统指数(合约)

API描述：获取当前所有指数的最新值。

API路径：GET [/v2/market/indexes](https://defiapi.876ex.com/v2/market/indexes)

API请求参数：

- 无

```
API响应样例：
```

```json
{
    "results":[
        {
            "type":"INDEX",
            "indexName":"XBTCUSDDAILYRATE",
            "data":[
                1546272000000, // 时间戳
                0.0003 // 价格
            ]
        },
        {
            "type":"INDEX",
            "indexName":"XBTCUSDPI",
            "data":[
                1546272000000,
                0.0005
            ]
        },
				... ...
    ]
}
```

某些指数是价格，例如`BBTCUSD`是比特币现货加权指数，某些指数是计算的数值，例如`XBTCUSDPI`是当前比特币对美元溢价率。

## 获取某个指数的最近历史值(合约)

API描述：获取某个指数的最近历史值。

API路径：GET /v2/market/indexes/:index_name

API示例：GET [/v2/market/indexes/BBTCUSD](https://defiapi.876ex.com/v2/market/indexes/BBTCUSD)

API请求参数(Path Param)：

| 参数     | 类型     | 说明                                             |
| :------- | -------- | :----------------------------------------------- |
| **index_name** | **path** | **必填**<br>指数名称，注意以`.`开头，例如`BBTCUSD` |

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
    "results": [
        // 按时间从近到远排序: [时间戳, 值]
        [1564815660000, 10801.32],
        [1564815600000, 10799.67],
        [1564815540000, 10794.05],
        [1564815480000, 10797.47],
        ...
    ]
}
```

## 获取24小时统计价格

API描述：获取某个交易对的最近24小时统计价格。接口不区分现货、合约。

API路径：GET /v2/market/ticker/:symbol_name

API示例：GET [/v2/market/ticker/BTC_USDT](https://defiapi.876ex.com/v2/market/ticker/BTC_USDT)

API请求参数(Path Param)：

| 参数       | 类型     | 说明                                  |
| :--------- | -------- | :------------------------------------ |
| **symbol_name** | **path** | **必填**<br>交易对名称,例如`BTC_USDT` |

```
API响应样例：
```

```json
{
    "type": "TICKER",
    "symbol": "BTC_USDT",
    "data": [
        1595988420000,
        11171,
        11212.2,
        9953.5,
        10905.1,
        2142.3818
    ],
    "sequenceId": "1194721",
    "ts": 1595988438053
}
```
说明：

数据格式：

`[timestamp, open, high, low, close, amount]`

`[时间戳，开盘价，最高价，最低价，收盘价，成交量]`


## 获取所有统计价格

API描述：获取所有交易对的最近24小时统计价格。接口不区分现货、合约。

API路径：GET /v2/market/all-ticker

API示例：GET [/v2/market/all-ticker](https://defiapi.876ex.com/v2/market/all-ticker)

API请求参数：

- 无

```
API响应样例：
```

```json
{
    "results": [
        {
            "type": "PRICE",
            "symbol": "BTC_USDT",
            "data": [
                1595988420000,
                11171,
                11212.2,
                9953.5,
                10895.2,
                2142.2028
            ],
            "sequenceId": "1194709",
            "ts": 1595988427765
        }
    ]
}
```



## 获取所有币币交易对统计价格

API描述：获取所有币币交易对的最近24小时统计价格。

API路径：GET /v2/market/spots/all-ticker

API示例：GET [/v2/market/spots/all-ticker](https://defiapi.876ex.com/v2/market/spots/all-ticker)

API请求参数：

- 无

```
API响应样例：
```

```json
{
    "results": [
        {
            "type": "PRICE",
            "symbol": "BTC_USDT",
            "data": [
                1595988420000,
                11171,
                11212.2,
                9953.5,
                10895.2,
                2142.2028
            ],
            "sequenceId": "1194709",
            "ts": 1595988427765
        }
    ]
}
```



## 获取所有合约交易对统计价格

API描述：获取所有合约交易对的最近24小时统计价格。

API路径：GET /v2/market/contracts/all-ticker

API示例：GET [/v2/market/contracts/all-ticker](https://defiapi.876ex.com/v2/market/contracts/all-ticker)

API请求参数：

- 无

```
API响应样例：
```

```json
{
    "results": [
        {
            "type": "PRICE",
            "symbol": "BTCUSDT_PERP",
            "data": [
                1595988420000,
                11171,
                11212.2,
                9953.5,
                10895.2,
                2142.2028
            ],
            "sequenceId": "1194709",
            "ts": 1595988427765
        }
    ]
}
```



## 获取OrderBook（现货）

API描述：获取某个交易对的最近OrderBook。

API路径：GET /v2/market/spots/orderbook/:symbol_name

API示例：GET [/v2/market/spots/orderbook/BTC_USDT](https://defiapi.876ex.com/v2/market/spots/orderbook/BTC_USDT)

API请求参数(Path Param)：

| 参数       | 类型     | 说明                                  |
| :--------- | -------- | :------------------------------------ |
| **symbol_name** | **path** | **必填**<br>交易对名称,例如`BTC_USDT` |

```
API响应样例：
```

```json
{
    // 最近成交价
    "price": 9179.33,
    // 序列ID
    "sequenceId": 40519,
    // 买盘
    "buyOrders": [
        [9175.08,0.35],
        [9168.73,0.54]
    ],
    // 卖盘
    "sellOrders": [
        [9179.33,0.76],
        [9181.37,0.97]
    ]
}
```


## 获取OrderBook（合约）

API描述：获取某个交易对的最近OrderBook。

API路径：GET /v2/market/contracts/orderbook/:symbol_name

API示例：GET [/v2/market/contracts/orderbook/XBTCUSD_PERP](https://defiapi.876ex.com/v2/market/contracts/orderbook/XBTCUSD_PERP)

API请求参数(Path Param)：

| 参数       | 类型     | 说明                              |
| :--------- | -------- | :-------------------------------- |
| **symbol_name** | **path** | **必填**<br>交易对名称,例如`XBTCUSD_PERP` |

```
API响应样例:
```

```json
{
    // 最近成交价
    "price": 9179.33,
    // 序列ID
    "sequenceId": 40519,
    // 买盘
    "buyOrders": [
        [9175.08,0.35],
        [9168.73,0.54]
    ],
    // 卖盘
    "sellOrders": [
        [9179.33,0.76],
        [9181.37,0.97]
    ]
}
```

## 获取REST Tick数据

API描述：获取某个交易对的最近tick信息。接口不区分现货、合约。

API路径：GET /v2/market/ticks/:symbol_name

API示例：GET [/v2/market/ticks/BTC_USDT](https://defiapi.876ex.com/v2/market/ticks/BTC_USDT?limit=10)

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

API路径：GET /v2/market/bars/:symbol_name/:type

API示例：GET [/v2/market/bars/BTC_USDT/min](https://defiapi.876ex.com/v2/market/bars/BTC_USDT/min)

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
"results": [
    [
      1595584440000,
      9526.76,
      9568.38,
      9526.76,
      9528.71,
      1.11
    ],
    [
      1595584380000,
      9526.76,
      9529.63,
      9526.76,
      9529.63,
      0.3
    ],
    [
      1595584320000,
      9526.76,
      9562.95,
      9526.76,
      9528.05,
      1.64
    ],
    [
      1595584260000,
      9530.33,
      9562.95,
      9526.76,
      9532.56,
      1.23
    ],
    [
      1595584200000,
      9526.76,
      9562.95,
      9526.76,
      9530.33,
      0.93
    ]
	]
}
```

说明：

bar数据格式：

`[timestamp, open, high, low, close, amount]`

`[时间戳，开盘价，最高价，最低价，收盘价，成交量]`

结果总是按时间戳升序排序

