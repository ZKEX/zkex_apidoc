# 公开市场

公开市场API是指无需登录，即可直接访问的API。

## 获取交易信息

API描述：获取交易所当前所有交易。

API路径：GET [/v1/market//trades](http://54.199.66.35:8080/v1/market/trades)

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
      },
      {
        "id": 101103,
        "name": "ETH_USDT",
        "startTime": 1680278400000,
        "endTime": 5995814400000,
        "trade": "SPOTS",
        "baseName": "ETH",
        "baseMinimumIncrement": 0.01,
        "baseScale": 2,
        "baseMinimumQuantity": 0.01,
        "baseMaximumQuantity": 10000.0,
        "quoteName": "USDT",
        "quoteMinimumIncrement": 0.2,
        "quoteScale": 1,
        "supportMarginTrade": true,
        "alwaysChargeQuote": false
      },
      {
        "id": 102103,
        "name": "BCH_USDT",
        "startTime": 1680278400000,
        "endTime": 5995814400000,
        "trade": "SPOTS",
        "baseName": "BCH",
        "baseMinimumIncrement": 0.01,
        "baseScale": 2,
        "baseMinimumQuantity": 0.01,
        "baseMaximumQuantity": 10000.0,
        "quoteName": "USDT",
        "quoteMinimumIncrement": 0.1,
        "quoteScale": 1,
        "supportMarginTrade": true,
        "alwaysChargeQuote": false
      },
      {
        "id": 104101,
        "name": "ARM_ETH",
        "startTime": 1680278400000,
        "endTime": 5995814400000,
        "trade": "SPOTS",
        "baseName": "ARM",
        "baseMinimumIncrement": 0.01,
        "baseScale": 2,
        "baseMinimumQuantity": 0.01,
        "baseMaximumQuantity": 10000.0,
        "quoteName": "ETH",
        "quoteMinimumIncrement": 0.01,
        "quoteScale": 2,
        "supportMarginTrade": false,
        "alwaysChargeQuote": false
      },
      {
        "id": 101100,
        "name": "ETH_BTC",
        "startTime": 1680278400000,
        "endTime": 5995814400000,
        "trade": "SPOTS",
        "baseName": "ETH",
        "baseMinimumIncrement": 0.01,
        "baseScale": 2,
        "baseMinimumQuantity": 0.01,
        "baseMaximumQuantity": 10000.0,
        "quoteName": "BTC",
        "quoteMinimumIncrement": 0.01,
        "quoteScale": 2,
        "supportMarginTrade": false,
        "alwaysChargeQuote": false
      },
      {
        "id": 104103,
        "name": "ARM_USDT",
        "startTime": 1680278400000,
        "endTime": 5995814400000,
        "trade": "SPOTS",
        "baseName": "ARM",
        "baseMinimumIncrement": 0.01,
        "baseScale": 2,
        "baseMinimumQuantity": 0.01,
        "baseMaximumQuantity": 10000.0,
        "quoteName": "USDT",
        "quoteMinimumIncrement": 0.01,
        "quoteScale": 2,
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
      },
      {
        "id": 132,
        "name": "XBTC",
        "startTime": 1680278400000,
        "endTime": 5995814400000,
        "trade": "CONTRACTS",
        "type": "PERPETUAL",
        "baseCurrency": "BTC",
        "marginCurrency": "USDT",
        "quoteCurrency": "USDT",
        "multiplier": 0.1,
        "quoteMinimumIncrement": 0.5,
        "quoteScale": 1,
        "baseScale": 0,
        "baseMinimumIncrement": 1,
        "maximumQuantityPerOrder": 100,
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
        "settlementFeeRate": 0.0,
        "referencedIndexes": {
          "SPOT": 10122,
          "FAIR": 10125,
          "PREMIUM_1M": 10126,
          "PREMIUM_AGGREGATE": 10127,
          "MAX_PREMIUM_RATE": 10124,
          "FUNDING_RATE_1M": 10128,
          "FUNDING_RATE_AGGREGATE": 10129,
          "LENDING_RATE_1D": 10123
        }
      },
      {
        "id": 119,
        "name": "XBCH",
        "startTime": 1680278400000,
        "endTime": 5995814400000,
        "trade": "CONTRACTS",
        "type": "PERPETUAL",
        "baseCurrency": "BCH",
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
          "SPOT": 10109,
          "FAIR": 10112,
          "PREMIUM_1M": 10113,
          "PREMIUM_AGGREGATE": 10114,
          "MAX_PREMIUM_RATE": 10111,
          "FUNDING_RATE_1M": 10115,
          "FUNDING_RATE_AGGREGATE": 10116,
          "LENDING_RATE_1D": 10110
        }
      }
    ],
    "indexes": [
      {
        "id": 10109,
        "name": "BCH",
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
        "name": "BCH_USD_FAIR",
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
      },
      {
        "id": 10122,
        "name": "BTC",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 2
      },
      {
        "id": 10123,
        "name": "BTC_DAILYRATE",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10124,
        "name": "BTC_MAXBTCUSDPI",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10125,
        "name": "BTC_USD_FAIR",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10126,
        "name": "BTC_USD_PI",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10127,
        "name": "BTC_USD_PI8H",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10128,
        "name": "BTC_FR",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10129,
        "name": "BTC_FR8H",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10130,
        "name": "BTC_INS1H",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10131,
        "name": "BTC_INS1D",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10135,
        "name": "ETH",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 2
      },
      {
        "id": 10136,
        "name": "ETH_DAILYRATE",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10137,
        "name": "ETH_MAXETHUSDPI",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10138,
        "name": "ETH_USD_FAIR",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10139,
        "name": "ETH_USD_PI",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10140,
        "name": "ETH_USD_PI8H",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10141,
        "name": "ETH_FR",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10142,
        "name": "ETH_FR8H",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10143,
        "name": "ETH_INS1H",
        "startTime": 0,
        "endTime": 5995814400000,
        "scale": 6
      },
      {
        "id": 10144,
        "name": "ETH_INS1D",
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
      },
      {
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
      {
        "id": 107,
        "initialMarginRate": 0.015,
        "maintenanceMarginRate": 0.01,
        "marginRateStep": 0.005,
        "maxLeverage": 10,
        "riskLimitBase": 100,
        "riskLimitStep": 50,
        "maxRiskLimitSteps": 9,
        "createdAt": 1686047861717
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
      ],
      "146": [
        {
          "id": 31,
          "chainTokenId": 146,
          "chainId": 2,
          "address": "0x145407c16831512459f6d4a672e16b4dcfb3fc40",
          "decimals": 18,
          "fastWithdraw": true,
          "isSupport": true,
          "updateTime": 1686968221632
        }
      ],
      "36": [
        {
          "id": 23,
          "chainTokenId": 36,
          "chainId": 7,
          "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
          "decimals": 18,
          "fastWithdraw": false,
          "isSupport": true,
          "updateTime": 1684493402887
        },
        {
          "id": 24,
          "chainTokenId": 36,
          "chainId": 4,
          "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
          "decimals": 18,
          "fastWithdraw": false,
          "isSupport": true,
          "updateTime": 1684493403079
        },
        {
          "id": 25,
          "chainTokenId": 36,
          "chainId": 6,
          "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
          "decimals": 18,
          "fastWithdraw": false,
          "isSupport": true,
          "updateTime": 1684493403284
        },
        {
          "id": 26,
          "chainTokenId": 36,
          "chainId": 5,
          "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
          "decimals": 18,
          "fastWithdraw": false,
          "isSupport": true,
          "updateTime": 1684493403466
        },
        {
          "id": 30,
          "chainTokenId": 36,
          "chainId": 9,
          "address": "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
          "decimals": 18,
          "fastWithdraw": false,
          "isSupport": true,
          "updateTime": 1686910890166
        }
      ],
      "150": [
        {
          "id": 17,
          "chainTokenId": 150,
          "chainId": 3,
          "address": "0x5f9969e07339553969f01d9d23f7abb877c25baf",
          "decimals": 18,
          "fastWithdraw": true,
          "isSupport": true,
          "updateTime": 1684493401578
        },
        {
          "id": 18,
          "chainTokenId": 150,
          "chainId": 4,
          "address": "0x4647a67c08ffa70ac1051fee36a1a16a1690b90a",
          "decimals": 18,
          "fastWithdraw": true,
          "isSupport": true,
          "updateTime": 1684493401785
        },
        {
          "id": 19,
          "chainTokenId": 150,
          "chainId": 1,
          "address": "0x91e5d0c39e3f2de1d8cbbecca3604f6704fb3494",
          "decimals": 6,
          "fastWithdraw": true,
          "isSupport": true,
          "updateTime": 1684493402004
        },
        {
          "id": 20,
          "chainTokenId": 150,
          "chainId": 7,
          "address": "0x18cd805e441c906d13d339842bb95c321b352a7d",
          "decimals": 18,
          "fastWithdraw": true,
          "isSupport": true,
          "updateTime": 1684493402188
        },
        {
          "id": 21,
          "chainTokenId": 150,
          "chainId": 2,
          "address": "0x2b1d07f867b220fcc818e9d7ff4fcb08e63b2ae5",
          "decimals": 18,
          "fastWithdraw": true,
          "isSupport": true,
          "updateTime": 1684493402382
        },
        {
          "id": 22,
          "chainTokenId": 150,
          "chainId": 6,
          "address": "0xd4e2b83d2696ed0e05818db08f95416aac8d7e2f",
          "decimals": 18,
          "fastWithdraw": true,
          "isSupport": true,
          "updateTime": 1684493402593
        }
      ],
      "140": [
        {
          "id": 15,
          "chainTokenId": 140,
          "chainId": 1,
          "address": "0x0fc3283a6d40550185a4d8cbd00030194475bbc4",
          "decimals": 18,
          "fastWithdraw": true,
          "isSupport": true,
          "updateTime": 1684493401196
        },
        {
          "id": 16,
          "chainTokenId": 140,
          "chainId": 2,
          "address": "0xc6df93a49198bb902abc0231955ec77ae0cc34aa",
          "decimals": 18,
          "fastWithdraw": true,
          "isSupport": true,
          "updateTime": 1684493401388
        }
      ],
      "143": [
        {
          "id": 29,
          "chainTokenId": 143,
          "chainId": 2,
          "address": "0x2796baed33862664c08b8ee5fa2d1283c79593b1",
          "decimals": 18,
          "fastWithdraw": true,
          "isSupport": true,
          "updateTime": 1686211574668
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
        "id": 2,
        "tokenId": 103,
        "tokenName": "USDT",
        "mappingTokenId": 0,
        "tokenIconUrl": "https://static.zk.link/token/icons/default/usdt.svg",
        "tokenType": 0
      },
      {
        "id": 3,
        "tokenId": 132,
        "tokenName": "XBTC",
        "mappingTokenId": 0,
        "tokenIconUrl": "https://static.zk.link/token/icons/default/btc.svg",
        "tokenType": 1
      },
      {
        "id": 4,
        "tokenId": 101,
        "tokenName": "ETH",
        "mappingTokenId": 2,
        "tokenIconUrl": "https://static.zk.link/token/icons/default/eth.svg",
        "tokenType": 0
      },
      {
        "id": 8,
        "tokenId": 145,
        "tokenName": "XETH",
        "mappingTokenId": 1,
        "tokenIconUrl": "https://static.zk.link/token/icons/default/eth.svg",
        "tokenType": 1
      },
      {
        "id": 14,
        "tokenId": 102,
        "tokenName": "BCH",
        "mappingTokenId": 3,
        "tokenIconUrl": "",
        "tokenType": 0
      },
      {
        "id": 15,
        "tokenId": 119,
        "tokenName": "XBCH",
        "mappingTokenId": 3,
        "tokenIconUrl": "",
        "tokenType": 1
      },
      {
        "id": 16,
        "tokenId": 104,
        "tokenName": "ARM",
        "mappingTokenId": 4,
        "tokenIconUrl": "",
        "tokenType": 0
      },
      {
        "id": 17,
        "tokenId": 146,
        "tokenName": "XARM",
        "mappingTokenId": 4,
        "tokenIconUrl": "",
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
      },
      {
        "id": 101,
        "name": "ETH",
        "main": false,
        "conversionRatio": 0.9,
        "createdAt": 0,
        "displayName": "Ethereum"
      },
      {
        "id": 102,
        "name": "BCH",
        "main": false,
        "conversionRatio": 0.8,
        "createdAt": 0,
        "displayName": "Bitcoin Cash"
      },
      {
        "id": 103,
        "name": "USDT",
        "main": true,
        "conversionRatio": 1,
        "createdAt": 0,
        "displayName": "USD Tether"
      },
      {
        "id": 104,
        "name": "ARM",
        "main": false,
        "conversionRatio": 0,
        "createdAt": 0,
        "displayName": "Arweave"
      }
    ]
  }
}
```

## 获取时间

API描述：获取服务器当前时间，以毫秒为单位。

API路径：GET [/v1/market/timestamp](https://uniapi.876ex.com/v1/market/timestamp)

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

API路径：GET [/v1/market/fex](https://uniapi.876ex.com/v1/market/fex)

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

API路径：GET [/v1/market/errorCodes](https://uniapi.876ex.com/v1/market/errorCodes)

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

API路径：GET [/v1/market/error](https://uniapi.876ex.com/v1/market/error)

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

API路径：GET [/v1/market/indexes](https://uniapi.876ex.com/v1/market/indexes)

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

API路径：GET /v1/market/indexes/:index_name

API示例：GET [/v1/market/indexes/BBTCUSD](https://uniapi.876ex.com/v1/market/indexes/BBTCUSD)

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

API路径：GET /v1/market/ticker/:symbol_name

API示例：GET [/v1/market/ticker/BTC_USDT](https://uniapi.876ex.com/v1/market/ticker/BTC_USDT)

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
      	24
        2142.3818
    ],
    "sequenceId": "1194721",
    "ts": 1595988438053
}
```
说明：

数据格式：

`[timestamp, open, high, low, close, amount, volume]`

`[时间戳，开盘价，最高价，最低价，收盘价，成交量, 成交额]`


## 获取所有统计价格

API描述：获取所有交易对的最近24小时统计价格。接口不区分现货、合约。

API路径：GET /v1/market/all-ticker

API示例：GET [/v1/market/all-ticker](https://uniapi.876ex.com/v1/market/all-ticker)

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

API路径：GET /v1/market/spots/all-ticker

API示例：GET [/v1/market/spots/all-ticker](https://uniapi.876ex.com/v1/market/spots/all-ticker)

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

API路径：GET /v1/market/contracts/all-ticker

API示例：GET [/v1/market/contracts/all-ticker](https://uniapi.876ex.com/v1/market/contracts/all-ticker)

API请求参数：

- 无

```
API响应样例：
```

```json
{
    "results": [
        {
            "type": "TICKER",
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



## 获取OrderBook

API描述：获取某个交易对的最近OrderBook。

API路径：GET /v1/market/orderBook/:symbol_name

API示例：GET [/v1/market/orderBook/BTC_USDT](https://uniapi.876ex.com/v1/market/orderbook/BTC_USDT)

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

## 获取REST Tick数据

API描述：获取某个交易对的最近tick信息。接口不区分现货、合约。

API路径：GET /v1/market/ticks/:symbol_name

API示例：GET [/v1/market/ticks/BTC_USDT](https://uniapi.876ex.com/v1/market/ticks/BTC_USDT?limit=10)

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

API示例：GET [/v1/market/bars/BTC_USDT/min](https://uniapi.876ex.com/v1/market/bars/BTC_USDT/min)

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
      1.11,
      2.3
    ],
    [
      1595584380000,
      9526.76,
      9529.63,
      9526.76,
      9529.63,
      0.3,
      1.2
    ],
    [
      1595584320000,
      9526.76,
      9562.95,
      9526.76,
      9528.05,
      1.64,
      2.1
    ],
    [
      1595584260000,
      9530.33,
      9562.95,
      9526.76,
      9532.56,
      1.23,
      2.1
    ],
    [
      1595584200000,
      9526.76,
      9562.95,
      9526.76,
      9530.33,
      0.93,
      3.1
    ]
	]
}
```

说明：

bar数据格式：

`[timestamp, open, high, low, close, amount, volume]`

`[时间戳，开盘价，最高价，最低价，收盘价，成交量，成交额]`

结果总是按时间戳升序排序

